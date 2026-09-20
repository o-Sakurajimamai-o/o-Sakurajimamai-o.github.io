---
title: "Mini-SWE-Agent 源码解读"
date: 2026-09-20
description: "通过运行轨迹深入解读 Mini-SWE-Agent 的源码，分析 prompt 注入、工具调用、退出机制和进程管理。"
---

设置好 `.config/mini-swe-agent` 中的环境之后，我们运行
```bash
python -m minisweagent.run.mini \
      -c mini.yaml \
      -t "创建一个 test_agent.txt 文件并在里面写入 Hello Agent" \
      --model "deepseek/deepseek-chat" \
      --agent-class minisweagent.agents.default.DefaultAgent
```
会在 `.config/mini-swe-agent` 下得到最后一次的运行轨迹，根据该运行轨迹来进行源码的解读
整个 Repo 的框架：
`run/mini.py:99-101`：
```python
model = get_model(config=config.get("model", {}))
env   = get_environment(config.get("environment", {}), default_type="local")
agent = get_agent(model, env, config.get("agent", {}), default_type="interactive")
```
## Prompt 注入
在 `default.py`  中我们看到了
```python
self.add_messages(
	self.model.format_message(role="system", content=self._render_template(self.config.system_template)),
	self.model.format_message(role="user", content=self._render_template(self.config.instance_template)),
)
```
进行 prompt 的注入和用户的询问，prompt 在 `mini.yaml` 中，其中指示了如果 Agent 完成了任务，第一句应该输出  `echo COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT`。这对我们后面进行任务检查很有用。
## Tool Action
接下来观察到轨迹的 content 和 tool call：
```json
"content": "I'll start by analyzing the current directory structure to understand the environment.",
"role": "assistant",
"tool_calls": [{
	  "index": 0,
	  "function": {
		"arguments": "{\"command\": \"pwd && ls -la\"}",
		"name": "bash"
	  },
	  "id": "call_00_61SsIsH70VtP63w7RQ6G5957",
	  "type": "function"
	}],
```
一条 assistant 消息的 `extra` 恰好四个键：
```json
extra = {
    "actions":   [{"command": "pwd && ls -la", "tool_call_id": "call_00_..."}],
    "response":  {...},     # 模型原始响应，整份塞进来
    "cost":      0.000327,
    "timestamp": 1789796928.007539,
}
```
`extra` 不会发给模型，在 model 端有
```python
def _prepare_messages_for_api(self, messages: list[dict]) -> list[dict]:
    prepared = [{k: v for k, v in msg.items() if k != "extra"} for msg in messages]
```
在 `extra` 中还有用量花费和更具体的 message 可以作为统计。查看 agent 的 tools，发现该 agent 只有一个 tool 即 bash，这样做的好处是不需要进行额外的沙箱安装。况且，`read/edit/write/grep` 本质都是它的一行子集——加一个 `read` 工具，等于给 bash 的一小块能力包了层壳。
但实际上，专用工具的精确度和完成度更好，可以更适合 model 调用。相对的，只用 bash 很考验 model 的基础能力，现在的 model 能力确实足以完成绝大部分的场景。
## Flag / Return
Mini-Swe-Agent 的退出机制分为三种，定义继承自基类异常的 `Submitted`、`LimitsExceeded`、`FormatError`。环境或内部校验处直接 `raise`，顶层主循环 `try...except` 统一捕获，并在 `messages` 中追加 `role: "exit"`。优点就是可以跨调用栈一键跳出，可以区分正常完成和异常终止。但是这种方法是依赖 python 的异常流做正常控制流。
三种退出机制有五个异常类以及一个合成状态
```
InterruptAgentFlow
├── Submitted
├── LimitsExceeded
│   └── TimeExceeded        ← 注意：继承的是 LimitsExceeded，不是直接继承基类
├── UserInterruption
└── FormatError
```
如果格式错误次数过多、迭代次数过多或者运行超时，则返回 `FormatError` 和 `LimitsExceeded`
```python
if 0 < self.config.max_consecutive_format_errors <= self.n_consecutive_format_errors:
	self.add_messages(
		*e.messages,{
			"role": "exit",
			"content": "RepeatedFormatError",
			"extra": {"exit_status": "RepeatedFormatError", "submission": ""},
		},)
if 0 < self.config.step_limit <= self.n_calls or 0 < self.config.cost_limit <= self.cost:
	raise LimitsExceeded({
			"role": "exit",
			"content": "LimitsExceeded",
			"extra": {"exit_status": "LimitsExceeded", "submission": ""},
		})
if 0 < self.config.wall_time_limit_seconds <= int(time.time() - self._start_time):
	raise TimeExceeded({
			"role": "exit",
			"content": "TimeExceeded",
			"extra": {"exit_status": "TimeExceeded", "submission": ""},
		})
```
如果正常完成，model 必须输出 `echo COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT` 命令，判断任务完成在 environment 层，而非让 agent 自行调用完成工具或者自行结束
```python
if lines and lines[0].strip() == "COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT" and output["returncode"] == 0:
	submission = "".join(lines[1:])
	raise Submitted({
			"role": "exit",
			"content": submission,
			"extra": {"exit_status": "Submitted", "submission": submission},
		})
```
在返回前，需要做一次保存
```python
finally:
	self.save(self.config.output_path)
```
在执行 bash 的过程中，mini-swe-agent 使用 `subprocess.popen()` + 进程组来进行彻底的任务执行与撤回。通过 `start_new_session` 来创建一个新进程执行 bash，当执行超时的时候，该进程执行的所有子孙进程跟随着父进程统一杀死。随后将结果返回
```python
try:
	stdout, _ = process.communicate(timeout=timeout)
except subprocess.TimeoutExpired:
	os.killpg(process.pid, signal.SIGKILL) if os.name == "posix" else process.kill()
	stdout, _ = process.communicate()
	raise subprocess.TimeoutExpired(command, timeout, output=stdout)
return subprocess.CompletedProcess(command, process.returncode, stdout=stdout)
```

## `finish_reason` 与 `exit_status` 

`finish_reason` 由 model api 生成，包含 `tool_calls`/`stop`/`length`/`content_filter`，它表示 model 停止的原因，放在 `extra.response.choices[0].finish_reason` 里面。
`exit_status` 则是 harness 来判断，包含上述的 `Submitted` / `LimitsExceeded` / `RepeatedFormatError` / ...，它表示整个任务停止的原因，放在末条 `message` 的 `extra.exit_status` 里
