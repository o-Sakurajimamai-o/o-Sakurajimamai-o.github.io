---
title: "基于 Hermes 的 Harness 工程"
date: 2026-08-08
description: "围绕一个自动化科研助手场景，从 Plugin、权限、审计到三条工作流与自动化测试的 Harness 全流程搭建"
---

## 确立模拟业务场景，建立数据

我选取的 `是自动化科研助手`，其中数据是让 AI 随机化生成的，主要结构如下：

```text
D:\Shixun\harness-research\
├── data\           # 模拟文献、实验数据（只读，Worker 不可改）
├── scripts\        # 工具脚本（lit_read / exp_stats / report_export）...
├── evidence\       # 证据目录：调研纪要、分析日志、评审意见（按 task_id 组织）
├── .hermes.md      # 项目规则（Hermes 自动发现并注入）
```

## 确立工作流

| 工作流 | 编排方式 | 为什么用这种方式 | 验证架构的哪一层 |
| --- | --- | --- | --- |
| **1. 文献调研纪要** | 单 Agent 直执行 | 6 篇文献顺序读取，彼此无依赖也无并行收益，单会话可完成 | **执行层**：工具链能否跑通、Skill 能否加载、审计能否留痕 |
| **2. 实验方案设计** | `delegate_task` 并行委派 | 文献依据 / 数据审计 / 方案设计三个子任务互不依赖，且各自需要独立上下文，避免互相污染 | **编排层**：多 Agent 分工、结构化交接、主 Agent 合并 |
| **3. 报告评审全流程** | Kanban | 跨会话、有依赖顺序（Planner → Worker → Reviewer）、失败后要能阻塞并恢复 | **状态层 + 控制层**：状态持久化、返工闭环、异常恢复 |

1. 工作流 1 回答「**单个 Agent 到底能不能干活**」——这是一切的前提；
2. 工作流 2 回答「**多个 Agent 能不能分工而不互相干扰**」；
3. 工作流 3 回答「**出错了能不能恢复**」。

## 建立可提交的 Plugin

### 新建版本化插件

在工作区新增：

```text
D:\Shixun\harness-research\plugins\research_v2\
├─ __init__.py
├─ plugin.yaml
├─ schemas.py
└─ tools.py
```

`plugin.yaml` 使用新名称：

```yaml
name: research_v2
version: 2.0.0
description: 科研 Harness 受限业务工具——规划读取、文献读取、实验统计、证据输出与独立评审
provides_tools:
  - project_read
  - lit_read
  - exp_stats
  - exp_head
  - evidence_read
  - report_export
  - review_export
```

### 将工具拆成角色可配置的 toolset

在 `__init__.py` 中按角色能力注册：

| toolset | 工具 | 角色 |
| --- | --- | --- |
| `research_planner` | `project_read` | Planner，只读研究问题和项目规则 |
| `research_read` | `lit_read`、`exp_stats`、`evidence_read` | Worker/Reviewer 读取与复核 |
| `research_write` | `report_export` | Worker 输出业务报告 |
| `research_review` | `review_export` | Reviewer 输出评审意见 |

其余工具按上表注册。这样才能用 `hermes tools enable/disable` 对 Profile 做真正的工具白名单，而不是让三个角色共享全部写入能力。

{{< fig src="figures/01-tools-whitelist.png" caption="用 hermes tools enable/disable 做工具白名单" >}}

### 创建 schemas.py

指明 Agent 中各个角色的模式

{{< fig src="figures/02-schemas.png" caption="schemas.py：各个角色的模式" >}}

### 在 tools.py 加路径白名单，禁止覆盖证据

使用 `pathlib.Path.resolve()`，把每类工具限制在固定目录：

```python
WORKSPACE_ROOT = Path(r"D:\Shixun\harness-research").resolve()
LITERATURE_ROOT = (WORKSPACE_ROOT / "data" / "literature").resolve()
EXPERIMENT_ROOT = (WORKSPACE_ROOT / "data" / "experiments").resolve()
EVIDENCE_ROOT = (WORKSPACE_ROOT / "evidence").resolve()
```

把报告写入模式从 `w` 改为独占创建 `x`：

```python
with open(output_path, "x", encoding="utf-8") as f:
    ...
```

{{< fig src="figures/03-path-whitelist.png" caption="tools.py 的路径白名单与独占创建" >}}

### 增加逐次业务审计

不依赖 `agent.log`。每次工具调用另写一个唯一 JSON 审计文件：

```text
evidence/<task_id>/audit/<timestamp>_<uuid>.json
```

日志字段具体为：

```json
{
  "timestamp": "2026-08-08T10:00:00+08:00",
  "task_id": "task-003",
  "profile": "worker",
  "tool": "exp_stats",
  "input_paths": ["data/experiments/exp_adversarial.csv"],
  "parameters": {"columns": ["standard_acc", "robust_acc"], "group_by": "setting"},
  "status": "ok",
  "duration_ms": 12,
  "output_path": null,
  "error": null
}
```

审计中不记录报告全文、API Key、密码或完整环境变量。审计文件同样使用独占创建，保证并发调用不互相覆盖。

### 测试 Plugin 源码

Powershell 的命令分别为：检查语法错误、测试只读工具、生成的审计日志。

左边界面发现审计日志中有 task_id、profile、tool、parameters、status、duration_ms；没有报告全文、API Key 或密码；路径越界失败也有审计

{{< fig src="figures/04-plugin-test.png" caption="Plugin 源码测试：语法检查、只读工具与审计日志" >}}

语法检查通过、3 个合法调用成功、越界调用失败、审计存在。

### 安装 Plugin 到 Hermes

由于我在此之前做过一个类似的插件，所以这个插件标记为 v2，因此我需要先禁用掉原来的 Plugin 在启动，从运行截图可以看到 Hermes 安装成功

{{< fig src="figures/05-plugin-install.png" caption="Hermes 安装 Plugin 成功" >}}

执行

```text
hermes tools list
```

{{< fig src="figures/06-tools-list.png" caption="hermes tools list" >}}

## 设置角色 Profile、规划、记忆

### 创建 Profile

创建 Profile，分别是 Planner，Worker，Reviewer，按照以下命令进行，可以看到 list 正常显示了三个角色

```text
hermes profile create planner --clone-all --description "拆解科研任务、设置依赖和验收标准，只负责规划与编排"
hermes profile create worker --clone-all --description "调用受限科研工具读取文献、统计数据并生成新证据"
hermes profile create reviewer --clone-all --description "独立读取原始数据和证据，决定通过、阻塞或返工"

hermes profile list
```

{{< fig src="figures/07-profile-list.png" caption="三个角色的 Profile 列表" >}}

### 建可提交的角色模板目录

下面创建 9 个模板文件：每个角色一个 SOUL.md、一个 memories/USER.md、一个 memories/MEMORY.md，下图展示部分模板：

{{< fig src="figures/08-profile-templates.png" caption="角色模板目录（部分）" >}}

### 设置工作目录和工具白名单

每个角色都应该针对于不同的 tools 拥有不同的使用权，因此需要单独设置，这里给出 Planner 可以使用以及不可以使用的工具

```text
hermes profile use planner
hermes config set terminal.cwd D:\Shixun\harness-research
hermes tools disable web browser terminal file code_execution vision image_gen bfl tts cronjob computer_use research_read research_write research_review
hermes tools enable research_planner skills todo memory session_search clarify delegation
hermes tools list
```

{{< fig src="figures/09-planner-tools.png" caption="Planner 的工具白名单" >}}

### 验证 Profile 权限与 Memory 加载

Planner 权限测试：

{{< fig src="figures/10-planner-perm-test.png" caption="Planner 权限测试" >}}

Worker Memory 测试：

{{< fig src="figures/11-worker-memory-test.png" caption="Worker Memory 测试" >}}

其余角色测试均没问题，不再赘述。

值得注意的是，当我完成 3 个工作流后，Reviewer 的记忆发生了改变，Reviewer 往自己的记忆里面写入了一些新东西

{{< fig src="figures/12-reviewer-memory.png" caption="流程结束后 Reviewer 的记忆发生了变化" >}}

本阶段通过：三 Profile 存在、工具权限不同、记忆在新会话生效、Planner/Reviewer 无业务报告写权限。

## 工作流 1

### 启动 Worker

其中 -s + skills 代表手动激活 skills，因为之前部署过 SKILL，不手动激活也会自动激活

```text
hermes profile use worker
Set-Location D:\Shixun\harness-research
hermes --cli -s lit-review
```

经过 AI 优化过 Prompt，提交给 Hermes，得到如下结果

{{< fig src="figures/13-worker-result.png" caption="Worker 生成文献调研纪要" >}}

### 验证 Worker 结果

可以看到成功生成了研究报告，通过审计日志可以看到，使用的工具就是文献读取，且角色是 Worker

{{< fig src="figures/14-audit-log.png" caption="审计日志：工具为文献读取，角色为 Worker" >}}

{{< fig src="figures/15-audit-detail.png" caption="审计详情" >}}

### Reviewer 独立复核

```text
$env:HERMES_PROFILE = 'reviewer'
hermes -p reviewer --cli

评审 task-001-v2。
使用 evidence_read 读取 lit_review.md，并检查 audit/ 下的工具审计。
核对：是否覆盖 6 篇、每篇是否有引用、是否有主题对比/研究缺口/局限性、是否声明模拟文献。
通过则使用 review_export 新建 review_v1.md；不通过也使用 review_v1.md 写清返工项。
不要修改业务报告。
```

可以看到得到了正确的结果，左边日志也正常显示

{{< fig src="figures/16-reviewer-result.png" caption="Reviewer 独立复核结果" >}}

工作流 1 完成。

## 工作流 2

### 多 Agent 协作

工作流 2 要进行多 Agent 协作，通过阅读官方文档，若不需要修改 API Key，大部分保持 Delegation 权限的默认格式即可，因此直接运行 Hermes。

经过 AI 润色后的 Prompt 为

```text
执行工作流 2 修正版，task_id=task-002-v2。

用 delegate_task 并行委派 3 个独立子任务：

1. 文献依据：读取与对抗训练、鲁棒性和小样本相关的模拟文献，返回引用、可用方法和局限性。
2. 数据审计：检查 data/experiments/ 三个 CSV 以及 scripts/gen_mock_data.py，说明字段、行数、生成方式和数据性质；必须核对 exp_adversarial.csv 第一行 standard_acc=78.64、robust_acc=59.55，二者不相等。
3. 实验方案：设计研究假设、变量、分组、评估指标、成功判据和风险，但不得声称已真实训练模型。

每个子任务必须返回：task_id、objective、inputs、constraints、expected_output、evidence、risks、status、next_role。

主 Agent 合并时必须明确：所有 CSV 由 gen_mock_data.py 用固定随机种子生成，只用于 Harness 流程演示，不是实际 ResNet 训练或真实 PGD 攻击结果。

不得修改 data/，不得覆盖 evidence/task-002/plan.md。
最后用 report_export 新建：
task_id=task-002-v2
file_name=plan.md
```

可以看到 Hermes 已经委派了 3 个子 Agent 去工作, 得到的报告也符合要求

{{< fig src="figures/17-delegation.png" caption="Hermes 委派 3 个子 Agent" >}}

{{< fig src="figures/18-delegation-report.png" caption="生成的报告符合要求" >}}

### Reviewer 复核

通过 Reviewer 发现有错误，申请 Worker 根据 reviewer 的报告进行修改，证明了 "Reviewer 不信任 Worker 自述、用工具独立取证、查出真实方法学错误"

{{< fig src="figures/19-reviewer-found-error.png" caption="Reviewer 独立取证查出错误" >}}

### 修改错误

通过 sessions 回到原来的对话，然后提问

```text
根据 Reviewer 得到的报告，进行修改，创建 plan_v2.md
```

耗时 10 min，重新生成了 plan v2

{{< fig src="figures/20-plan-v2.png" caption="重新生成的 plan v2" >}}

### Reviewer 再次复核

启动 Reviewer，重新复核结果

```text
重新生成了 planv2，创建 review-v2，复核结果
```

可以看到，第二次 Reviewer 通过了！

{{< fig src="figures/21-review-passed.png" caption="第二次复核通过" >}}

## 工作流 3

由于工作流 2 可以看到有时候 Agent 并不是一次就能成功的，但手工去找 Reviewer 又会很麻烦，Kanban 就是自动干这个事情的，首先我们建立两个 PowerShell，一个用来观察状态一个进行创建和任务调度

### 初始化 kanban

窗口：

```text
hermes profile use default
Set-Location D:\Shixun\harness-research

hermes kanban init
hermes kanban boards list
hermes kanban boards set-default-workdir default D:\Shixun\harness-research
hermes kanban assignees
```

另一个窗口设置 `hermes kanban watch` 监视。

### 自动保存三个任务 ID

建立三个任务各自的 ID：

{{< fig src="figures/22-task-ids.png" caption="三个任务各自的 ID" >}}

可以看到建立成功

{{< fig src="figures/23-kanban-created.png" caption="Kanban 建立成功" >}}

### 调度 Planner 和 Worker

先运行 Planner

```text
hermes kanban dispatch --max 1
hermes kanban list
```

可以看到 Planner 已经在运行中了

{{< fig src="figures/24-planner-running.png" caption="Planner 在运行中" >}}

由于我在第一次中，建立了两个 Planner 卡，当我运行的时候我才发现，等待第一个 Planner 运行完之后，碰巧出现了一个问题，**可以看到 Planner 居然越权创建了一个新的 Worker 卡，可以充当真实的异常案例**

{{< fig src="figures/25-planner-overreach.png" caption="Planner 越权创建了新的 Worker 卡" >}}

删除两个卡之后，重新运行了一下，可以看到 Planner 和 Worker 都已经完成了，来检查一下内容

{{< fig src="figures/26-planner-worker-done.png" caption="Planner 和 Worker 都已完成" >}}

### 第一次 Reviewer 评审

可以看到第一次评审不合格，原因是缺少数据等， Reviewer 已经 Blocked 了，符合要求，并且从 Worker 中得知，Worker 是故意缺少这些数值的，所以 Reviewer 评审结果正确

{{< fig src="figures/27-review-blocked.png" caption="第一次评审：Reviewer 已 Blocked" >}}

### 创建返工卡、调度返工、第二次评审

创建返工卡：

```powershell
$reworkTask = hermes kanban create 'task-003 根据 review_v1 完成返工' `
    --assignee worker `
    --parent $workerV1Id `
    --workspace 'dir:D:\Shixun\harness-research' `
    --max-runtime 20m `
    --max-retries 2 `
    --body '读取 evidence/task-003/review_v1.md 和原始 CSV。用 exp_stats 补齐各 setting 的 count/mean/std/min/max，明确数据由 gen_mock_data.py 生成，不能作为真实攻击实验；新建 analysis_v2.md，不覆盖 analysis_v1.md。完成后返回证据路径和结构化交接。' `
    --json | ConvertFrom-Json
$reworkId = $reworkTask.id
$reworkId

if ($reviewWasAlreadyDone) {
    $reviewGateTask = hermes kanban create 'task-003 返工后的 Reviewer Gate' `
        --assignee reviewer `
        --parent $reworkId `
        --workspace 'dir:D:\Shixun\harness-research' `
        --initial-status blocked `
        --max-runtime 20m `
        --max-retries 2 `
        --body '等待 analysis_v2.md 完成。解除阻塞后，直接复核原始 CSV、analysis_v2.md、review_v1.md 和审计；使用 review_export 新建 review_v2.md，通过后 complete。' `
        --json | ConvertFrom-Json
    $reviewId = $reviewGateTask.id
    Write-Host "已创建恢复用 Reviewer Gate：$reviewId"
} else {
    hermes kanban link $reworkId $reviewId
}
```

{{< fig src="figures/28-rework-passed.png" caption="返工后评审通过" >}}

最终结果得到评审通过，工作流 3 通过，产生了以下结果：

- review_v2.md 存在；
- Reviewer 任务为 done；
- 正常分支中，runs $reviewId 至少有两次运行记录；
- 恢复分支中，原 $reviewV1Id 有第一次驳回运行，新 $reviewId Gate 有 blocked → unblock → 最终通过记录；
- v1、review_v1、v2、review_v2 全部保留。

## 自动化测试

### 创建测试文件

#### 测试架构

测试文件的架构为：

```text
D:\Shixun\harness-research\tests\
├─ test_research_tools.py
├─ test_permissions.md
├─ test_workflows.md
```

#### 模块测试

至少覆盖：

- `lit_read`：合法文献、缺失文件、非 Markdown、超长 `max_chars`；
- `exp_stats`：全局统计、分组统计、指定列、空 CSV、非法列；
- `exp_head`：返回 csv 中原始数据；
- `evidence_read`：合法证据和越界路径；
- `report_export`：新建成功、重复路径拒绝、非法 task_id、越界路径；
- `review_export`：只允许评审文件和 evidence 目录；
- 审计：成功和失败均产生唯一 JSON，字段完整且不含正文/密钥。

#### 集成测试

- Planner 交接能被 Worker 使用；
- Worker 报告能被 Reviewer 读取；
- Reviewer 无 `research_write`；
- Worker 无 `research_review`；
- **Worker 与 Reviewer 都能调用 `exp_head`**（同属 `research_read`），但写权限不同——这是「读权限共享、写权限分离」的直接验证；
- **数值可追溯**：Reviewer 用 `exp_head` 取回原始行后，能定位到 Worker 报告里引用的每一个数字的行号；对不上即判驳回；
- **`exp_head` 与 `exp_stats` 口径一致**：同一 CSV 的原始行与分组统计不矛盾（例如原始行最大值不超过统计返回的 `max`）；
- Profile 的 USER/MEMORY 能在新会话继续生效；
- Kanban 父任务完成后子任务从 todo 进入 ready；
- Reviewer 返工意见能传回 Worker，v2 能再次评审。

#### 异常测试

- 请求不存在的 `paper_999.md`，返回结构化错误；
- 请求允许根目录外的普通测试路径，返回 path denied；
- 对同一个报告路径写两次，第二次返回 already_exists；
- CSV 格式错误时工具返回 error，Agent 不编造结果；
- Kanban Worker 失败达到 `--max-retries` 后进入 blocked；
- 重启 Hermes 后，Kanban、证据和 Profile 记忆仍可恢复。

#### 场景测试

对三条工作流逐一检查完整证据链：

```text
输入 → 任务/委派 → 工具调用 → 审计 → 产物 → Reviewer 检查 → 状态变化 → 最终结论
```

把所有结果汇总到：

```text
D:\Shixun\harness-research\evidence\tests\Harness_test_matrix.md
```

每项记录：测试 ID、类型、输入、预期、实际、证据路径、是否通过、问题与修复。

### 运行测试

`python -m unittest discover -s tests -v` 显示通过全部的模块测试

{{< fig src="figures/29-unittest-passed.png" caption="unittest 全部通过" >}}
