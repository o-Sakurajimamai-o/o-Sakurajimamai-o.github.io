---
title: "Mini-SWE-Agent Source Code Analysis"
date: 2026-09-20
description: "A deep dive into the source code of Mini-SWE-Agent based on its execution trajectories, analyzing prompt injection, tool actions, exit mechanisms, and process management."
---

After setting up the environment in `.config/mini-swe-agent`, we run:
```bash
python -m minisweagent.run.mini \
      -c mini.yaml \
      -t "Create a test_agent.txt file and write Hello Agent inside it" \
      --model "deepseek/deepseek-chat" \
      --agent-class minisweagent.agents.default.DefaultAgent
```
This produces the latest execution trajectory in `.config/mini-swe-agent`. We can interpret the source code based on this trajectory.
The framework of the entire Repo:
`run/mini.py:99-101`:
```python
model = get_model(config=config.get("model", {}))
env   = get_environment(config.get("environment", {}), default_type="local")
agent = get_agent(model, env, config.get("agent", {}), default_type="interactive")
```
## Prompt Injection
In `default.py`, we see:
```python
self.add_messages(
	self.model.format_message(role="system", content=self._render_template(self.config.system_template)),
	self.model.format_message(role="user", content=self._render_template(self.config.instance_template)),
)
```
This performs prompt injection and user querying. The prompt is in `mini.yaml`, which instructs that if the Agent completes the task, the first sentence output should be `echo COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT`. This is very useful for our later task verification.

## Tool Action
Next, observing the trajectory's content and tool calls:
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
The `extra` field of an assistant message has exactly four keys:
```json
extra = {
    "actions":   [{"command": "pwd && ls -la", "tool_call_id": "call_00_..."}],
    "response":  {...},     # The raw model response, inserted entirely
    "cost":      0.000327,
    "timestamp": 1789796928.007539,
}
```
`extra` is not sent to the model. On the model side, there is:
```python
def _prepare_messages_for_api(self, messages: list[dict]) -> list[dict]:
    prepared = [{k: v for k, v in msg.items() if k != "extra"} for msg in messages]
```
The `extra` field also contains usage costs and more specific messages for statistical purposes. Checking the agent's tools, we find this agent only has one tool, which is `bash`. The advantage of this is that no additional sandbox installation is needed. Furthermore, `read/edit/write/grep` are essentially subsets of it—adding a `read` tool is equivalent to wrapping a shell around a small piece of bash's capability.
But in reality, specialized tools offer better precision and completeness, making them more suitable for model invocation. Conversely, using only bash heavily tests the model's fundamental capabilities, though current models are indeed capable of handling the vast majority of scenarios.

## Flag / Return
The exit mechanisms of Mini-Swe-Agent are divided into three types, defining `Submitted`, `LimitsExceeded`, and `FormatError` inherited from a base exception class. They are directly `raise`d in the environment or internal validation points, unified and caught by the top-level main loop's `try...except`, and appended with `role: "exit"` in `messages`. The advantage is the ability to jump out across the call stack with one click, distinguishing between normal completion and abnormal termination. However, this method relies on Python's exception flow for normal control flow.
The three exit mechanisms consist of five exception classes and one synthetic state:
```
InterruptAgentFlow
├── Submitted
├── LimitsExceeded
│   └── TimeExceeded        ← Note: Inherits from LimitsExceeded, not directly from the base class
├── UserInterruption
└── FormatError
```
If there are too many format errors, too many iterations, or a timeout occurs, `FormatError` and `LimitsExceeded` are returned:
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
If completed normally, the model must output the `echo COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT` command. Task completion is judged at the environment layer, rather than letting the agent call a completion tool itself or terminate on its own.
```python
if lines and lines[0].strip() == "COMPLETE_TASK_AND_SUBMIT_FINAL_OUTPUT" and output["returncode"] == 0:
	submission = "".join(lines[1:])
	raise Submitted({
			"role": "exit",
			"content": submission,
			"extra": {"exit_status": "Submitted", "submission": submission},
		})
```
Before returning, a save operation is required:
```python
finally:
	self.save(self.config.output_path)
```
During the execution of bash, mini-swe-agent uses `subprocess.Popen()` + process groups for thorough task execution and revocation. By using `start_new_session` to create a new process to execute bash, when execution times out, all descendant processes of this process are killed together with the parent process. Then the results are returned:
```python
try:
	stdout, _ = process.communicate(timeout=timeout)
except subprocess.TimeoutExpired:
	os.killpg(process.pid, signal.SIGKILL) if os.name == "posix" else process.kill()
	stdout, _ = process.communicate()
	raise subprocess.TimeoutExpired(command, timeout, output=stdout)
return subprocess.CompletedProcess(command, process.returncode, stdout=stdout)
```

## `finish_reason` and `exit_status`

`finish_reason` is generated by the model API, including `tool_calls`/`stop`/`length`/`content_filter`. It indicates the reason for the model's stop and is placed in `extra.response.choices[0].finish_reason`.
`exit_status` is determined by the harness, including the aforementioned `Submitted` / `LimitsExceeded` / `RepeatedFormatError` / ... It indicates the reason for the entire task's stop and is placed in `extra.exit_status` of the last `message`.
