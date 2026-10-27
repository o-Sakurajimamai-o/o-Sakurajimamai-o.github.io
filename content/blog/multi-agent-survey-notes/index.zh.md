---
title: "Multi-Agent Systems Survey Notes"
date: 2026-10-23T00:00:00+08:00
lastmod: 2026-10-27T22:24:12+08:00
description: "《Beyond Individual Intelligence》阅读笔记：个体能力、推理、记忆、规划、角色分配、多智能体通信、编排与故障归因。"
draft: false
isCJKLanguage: true
---

近期 LLM 的能力呈线性或指数级上升，甚至大模型能力已然超出 AI 开发者的意料。OpenAI 团队称，内部模型通过约 10,000 个并行智能体协作，给出了 [Navier–Stokes 千禧年难题的解答](https://openai.com/index/navier-stokes-solution/)；Anthropic 研究员也介绍了 [Claude 与领域专家合作开展的遗传学研究](https://www.anthropic.com/research/claude-shaped-science)；Google 的 Gemini 4 Argon 则通过多智能体协作，帮助研究人员[扩展了核聚变磁约束相关问题的数学解](https://www.techradar.com/pro/its-over-ai-won-google-gemini-4-argon-shatters-59-year-old-nuclear-fusion-conjecture-in-just-a-few-hours)，这项研究与已有 59 年历史的 Grad 猜想有关。由此可见，随着任务规模和执行时长的增长，任务所要求的多元能力越来越难以全部集中在单个智能体中，多智能体协同提供了一种原则性的解决方案。

## 多智能体系统存在的问题

在多智能体协作中，单模型依旧有幻觉事实、消息误传或错误的工具调用等局部错误，这些错误在后续的交互轮次中扩散传播，随着步数的迭代，这些错误难以被发现定位，归因和自我纠错两种缺陷深度交织在一起，从而智能体们无法进行改进。

这种错误暴露出一种本质的不对称性：对于一个复杂精密的多智能体系统，它的能力远远超过了用于理解系统为何发生故障并促使自主改进的机制体系（RSI）。

弥合这一鸿沟需要一个闭环的框架，使得能够识别故障、减少预算、指导结构性自我改进。

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/life-overview.png" width="1717" height="1228" alt="图 1：沿运行生命周期组织的多智能体系统概览，展示个体能力、协同、故障归因与系统自进化及其代表性工作。" loading="lazy" decoding="async">
  <figcaption>图 1：沿运行生命周期组织的多智能体系统概览，展示个体能力、协同、故障归因与系统自进化及其代表性工作。</figcaption>
</figure>

## 个体智能

LLM 在自动化软件工程、科学实验设计以及多步网页导航等复杂场景中依然具有局限性，本质的缺陷还是缺乏持久性记忆、无法主动感知外部环境，受限于训练期间的静态知识，Harness 赋予了 LLM 感知环境、制定目标、执行工具等能力，Agent = Harness + LLM：

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/individual-capabilities.png" width="1822" height="1131" alt="图 2：沿推理管线组织的个体智能体能力架构。记忆贯穿其中，提供检索上下文并吸收执行反馈；规划与工具使用构成其他核心能力。" loading="lazy" decoding="async">
  <figcaption>图 2：沿推理管线组织的个体智能体能力架构。记忆贯穿其中，提供检索上下文并吸收执行反馈；规划与工具使用构成其他核心能力。</figcaption>
</figure>

模块化优化占据主导地位，但模块间的耦合仍未经深入研究。推理、记忆、规划和工具使用在每一个时间步上都相互依存，**然而现有方法几乎完全孤立地优化单个模块。各模块之间的能力不匹配往往成为端到端部署中的真正瓶颈**，但这种故障模式对于目前的基准测试而言却是不可见的，因为现有基准大多只针对一到两种能力。

### 推理

`CoT` 通过精心设计的提示诱导出中间推理步骤，在不改变模型参数的情况下大幅度提升了复杂任务的性能。核心挑战便转变为外部机制应如何弥合模型的静态参数化知识与现实世界任务的动态需求之间的鸿沟。遵循这一视角，我们沿着推理管线的信息流来组织现有方法，将其划分为三个阶段：

1. _输入阶段增强_，通过外部知识检索与多模态感知来拓展模型可获取的信息边界；
2. _推理过程增强_，通过搜索空间扩展与路径验证来优化推理轨迹；
3. _输出阶段调控_，通过事后评估与自适应修正来提升生成结果的可靠性与事实准确性。

#### 查询增强

`RAG` 将参数化生成器与非参数化检索器相结合，使得模型能在推理阶段从外部知识库中获取相关文档。在此基础上，后续的工作有

1. 通过训练模型来自适应决定如何检索、通过反思标记对检索内容和生成内容进行评判
2. 利用知识图谱进行检索，查询相关子图、在知识图谱上进行多跳推理
3. `Knowledge Card` 设计多个小模型作为各个领域可插拔的知识模块，小模型负责生成背景材料，依然通过检索由大模型来回答
4. `RuAG` 利用大模型辅助确定候选条件和预测目标，通过蒙特卡洛树搜索从离线数据中挖掘可靠的逻辑规则，再将规则转化为自然语言，直接或按需检索后注入提示词，指导大模型进行推理和生成。

#### 推理增强

对于推理轨迹本身，沿两个维度对其进行组织：搜索空间扩展，用于增加候选解的覆盖度；以及路径验证，用于提供识别最可靠结果所需的选择压力。

**搜索空间扩展**

`CoT`、`ToT`、`GoT` 依赖人工设计的提示协议，`DeepSeek-R1` 通过大规模的 RL 训练模型自主生成长思维链，展现出纯粹源自奖励优化的自我验证、自发回溯、自我批评的能力。

**推理路径验证**

拓展了搜索空间能够产生更多的候选解，更多的候选解意味着会引入等量的错误路径。目前的方法通过采样多条输出后内部投票、维护失败自迭代等方式，这种方法不改变模型的参数。 PRM 通过对推理步骤进行独立评分，大幅度提升了模型的推理能力，后续的进展基于 PRM 整合到 RL 训练过程中，证实了过程监督在同时塑造习得策略与选择机制时最为有效。

#### 输出检验

目前针对输出内容的可靠性评估分为两类。一类是通过将输出分解为多个待检验项，然后逐步核对知识库，还可以扩展到代码、数学推理、科学文献等更细粒度的错误定位。另一类则是不借助外部知识，通过衡量多个样本的一致性来检测幻觉，或是模型自主去检验、独立回答，亦或者多条样本根据投票选出胜者，然后以此进行自迭代。

**表 1.** 推理增强方法对比，按干预阶段和子类型编排。

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Table 1</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

子类型：KE 知识增强；PE 感知增强；ESS 扩展搜索空间；VRP 推理路径验证；OV 输出验证；RC 响应修正。任务范围：评估领域——Ma 数学；QA 问答；Co 代码；LF 长文本生成；Ag 智能体；Mu 多任务。主要来源：主要信息源——Evid = 外部证据；Inter = 内部状态；Consis = 跨样本一致性；Log = 逻辑/验证；Tool = 基于工具。修正：错误修正机制——SF = 自我反馈；Refl = 反思；TV = 工具验证；SC = 自洽性。Ext. Ret.：使用外部检索。TF：免训练。TTC：支持测试时计算扩展。

<div class="survey-table" tabindex="0" role="region" aria-label="表 1">

| **Method** | **Sub-type** | **Task Scope** | **Prim.** **Source** | **Correction** | **Ext.** **Ret.** | **Tool** | **TF** | **TTC** | **Year** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *A. Input-Stage Enhancement* |  |  |  |  |  |  |  |  |  |
| ViperGPT [89](https://arxiv.org/html/2605.14892v2#bib.bib89) | PE | Mu | Inter | — | — | ✓ | ✓ | — | 2023 |
| RAPTOR [90](https://arxiv.org/html/2605.14892v2#bib.bib90) | KE | QA LF | Evid | — | ✓ | — | ✓ | — | 2024 |
| Self-RAG [50](https://arxiv.org/html/2605.14892v2#bib.bib50) | KE | QA LF | Evid | SF | ✓ | — | — | — | 2024 |
| LLaVA [91](https://arxiv.org/html/2605.14892v2#bib.bib91) | PE | Mu | Inter | — | — | ✓ | — | — | 2024 |
| TopViewRS [92](https://arxiv.org/html/2605.14892v2#bib.bib92) | PE | Mu | Inter | — | — | — | ✓ | — | 2024 |
| VisCoT [93](https://arxiv.org/html/2605.14892v2#bib.bib93) | PE | Mu | Inter | — | — | — | ✓ | — | 2024 |
| HippoRAG2 [94](https://arxiv.org/html/2605.14892v2#bib.bib94) | KE | QA LF | Evid | — | ✓ | — | ✓ | — | 2025 |
| Theanine [95](https://arxiv.org/html/2605.14892v2#bib.bib95) | KE | QA LF | Evid | — | ✓ | — | ✓ | — | 2025 |
| SubgraphRAG [51](https://arxiv.org/html/2605.14892v2#bib.bib51) | KE | QA | Evid | — | ✓ | — | ✓ | — | 2025 |
| *B. Reasoning-Process Enhancement* |  |  |  |  |  |  |  |  |  |
| Self-Consistency [64](https://arxiv.org/html/2605.14892v2#bib.bib64) | VRP | Ma QA | Consis | SC | — | — | ✓ | ✓ | 2023 |
| LATS [96](https://arxiv.org/html/2605.14892v2#bib.bib96) | ESS | Mu | Inter | Refl | — | ✓ | ✓ | ✓ | 2024 |
| CoC [97](https://arxiv.org/html/2605.14892v2#bib.bib97) | ESS | Ma QA Co | Tool | — | — | ✓ | ✓ | — | 2024 |
| AnyTool [98](https://arxiv.org/html/2605.14892v2#bib.bib98) | ESS | Ag | Tool | SF | — | ✓ | ✓ | — | 2024 |
| RAIN [99](https://arxiv.org/html/2605.14892v2#bib.bib99) | ESS | QA | Inter | SF | — | — | ✓ | ✓ | 2024 |
| Math-Shepherd [67](https://arxiv.org/html/2605.14892v2#bib.bib67) | VRP | Ma | Log | — | — | — | — | ✓ | 2024 |
| CPO [62](https://arxiv.org/html/2605.14892v2#bib.bib62) | VRP | Ma | Log | — | — | — | — | — | 2024 |
| rStar [100](https://arxiv.org/html/2605.14892v2#bib.bib100) | ESS | Ma | Consis | SC | — | — | ✓ | ✓ | 2025 |
| rStar-Math [101](https://arxiv.org/html/2605.14892v2#bib.bib101) | ESS | Ma | Consis | SC | — | — | — | ✓ | 2025 |
| TTS [102](https://arxiv.org/html/2605.14892v2#bib.bib102) | ESS | Ma | Log | — | — | — | ✓ | ✓ | 2025 |
| TOPS [103](https://arxiv.org/html/2605.14892v2#bib.bib103) | ESS | Ma | Log | — | — | — | ✓ | ✓ | 2025 |
| Rewarding Progress [104](https://arxiv.org/html/2605.14892v2#bib.bib104) | VRP | Ma | Log | — | — | — | — | ✓ | 2025 |
| WizardMath [68](https://arxiv.org/html/2605.14892v2#bib.bib68) | VRP | Ma | Log | — | — | — | — | ✓ | 2025 |
| *C. Output-Stage Regulation* |  |  |  |  |  |  |  |  |  |
| FActScore [70](https://arxiv.org/html/2605.14892v2#bib.bib70) | OV | LF | Evid | — | ✓ | — | ✓ | — | 2023 |
| FacTool [71](https://arxiv.org/html/2605.14892v2#bib.bib71) | OV | Mu | Evid | — | ✓ | ✓ | ✓ | — | 2023 |
| SelfCheckGPT [73](https://arxiv.org/html/2605.14892v2#bib.bib73) | OV | LF | Consis | — | — | — | ✓ | — | 2023 |
| Semantic Entropy [105](https://arxiv.org/html/2605.14892v2#bib.bib105) | OV | QA LF | Inter | — | — | — | ✓ | — | 2023 |
| RARR [78](https://arxiv.org/html/2605.14892v2#bib.bib78) | OV | QA LF | Evid | TV | ✓ | — | ✓ | — | 2023 |
| ITI [81](https://arxiv.org/html/2605.14892v2#bib.bib81) | RC | QA | Inter | — | — | — | ✓ | — | 2023 |
| Reflexion [19](https://arxiv.org/html/2605.14892v2#bib.bib19) | RC | Ag Co | Inter | Refl | — | — | ✓ | ✓ | 2023 |
| L2R [106](https://arxiv.org/html/2605.14892v2#bib.bib106) | RC | QA | Inter | — | — | — | — | — | 2023 |
| Factcheck-GPT [72](https://arxiv.org/html/2605.14892v2#bib.bib72) | OV | QA LF | Evid | — | ✓ | — | ✓ | — | 2024 |
| HaloScope [75](https://arxiv.org/html/2605.14892v2#bib.bib75) | OV | QA LF | Inter | — | — | — | ✓ | — | 2024 |
| DoLa [79](https://arxiv.org/html/2605.14892v2#bib.bib79) | RC | QA LF | Inter | — | — | — | ✓ | — | 2024 |
| CAD [80](https://arxiv.org/html/2605.14892v2#bib.bib80) | RC | QA LF | Evid | — | — | — | ✓ | — | 2024 |
| FactTune [84](https://arxiv.org/html/2605.14892v2#bib.bib84) | RC | QA LF | Evid | — | — | — | — | — | 2024 |
| CoVe [77](https://arxiv.org/html/2605.14892v2#bib.bib77) | RC | QA LF | Log | SF | — | — | ✓ | ✓ | 2024 |
| CRITIC [87](https://arxiv.org/html/2605.14892v2#bib.bib87) | RC | QA Ma Co | Tool | TV | — | ✓ | ✓ | — | 2024 |
| R-Tuning [85](https://arxiv.org/html/2605.14892v2#bib.bib85) | RC | QA | Inter | — | — | — | — | — | 2024 |
| HalluLens [76](https://arxiv.org/html/2605.14892v2#bib.bib76) | OV | Mu | Inter | — | — | — | ✓ | — | 2025 |
| SADI [82](https://arxiv.org/html/2605.14892v2#bib.bib82) | RC | QA | Inter | — | — | — | ✓ | — | 2025 |
| MCD [83](https://arxiv.org/html/2605.14892v2#bib.bib83) | RC | QA LF | Inter | — | — | — | ✓ | — | 2025 |
| SEAL [88](https://arxiv.org/html/2605.14892v2#bib.bib88) | RC | QA LF | Inter | — | — | — | — | — | 2025 |
| SMC steering [107](https://arxiv.org/html/2605.14892v2#bib.bib107) | RC | QA Co | Log | — | — | — | ✓ | — | 2025 |

</div>

</div>
</details>


### 记忆

记忆的相关知识在[之前文章](/zh/blog/graph-based-agent-memory/)已经给出，这里只给出沿记忆生命周期组织的记忆增强方法比较。

**表 2.** 沿记忆生命周期组织的记忆增强方法比较。

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Table 2</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

形成（Formation）：原始经验如何转化为记忆——Record = 原始记录；Distill = 生成式摘要；Routing = 类型化分类。存储（Storage）：保存记忆的数据结构——Flat 扁平存储；Tiered 主上下文 + 归档；Graph 图结构；Tree 树结构；Typed 类型分离存储；Unified 多类型统一存储。维护（Maintenance）：记忆如何更新或淘汰——Decay = 基于时间的遗忘；Eviction = 受预算约束的驱逐；CRUD = 创建/读取/更新/删除；Evolution = 追溯性细化；Versioning = 双时态追踪；Lifecycle = 统一管理；Learned = 基于强化学习的选择。检索（Retrieval）：相关条目如何被召回——Multi-factor = 新近度 + 重要性 + 相关性；Injection = 全缓冲区注入；Autonomous = 通过函数调用自主驱动；Graph/Tree trav. = 在对应结构上遍历；Type-aware = 每种类型 top-k 检索后合并；Hybrid = 语义 + 关键词 + 图；Routed = 多路由检索；Utility-driven = 相似度 + Q 值重排序。记忆类型（Mem. Type）：E（情景记忆），S（语义记忆），P（程序记忆）；实心 = 支持，空心 = 缺失。任务领域（Task Domain）：Conv = 对话；Ent = 企业。

<div class="survey-table" tabindex="0" role="region" aria-label="表 2">

| **Method** | **Formation** | **Storage** | **Maintenance** | **Retrieval** | **Mem.** **Type** | **Task** **Domain** | **Year** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Generative Agents [110](https://arxiv.org/html/2605.14892v2#bib.bib110) | Record, Distill | Flat | Append | Multi-factor | ●●○ | Simulation | 2023 |
| MemoChat [112](https://arxiv.org/html/2605.14892v2#bib.bib112) | Distill | Flat | Append | Similarity | ●●○ | Conversation | 2023 |
| Reflexion [19](https://arxiv.org/html/2605.14892v2#bib.bib19) | Distill | Flat | Append | Injection | ●○○ | General | 2023 |
| MemoryBank [123](https://arxiv.org/html/2605.14892v2#bib.bib123) | Record | Flat | Decay | Similarity | ●●○ | Conversation | 2024 |
| MemGPT [111](https://arxiv.org/html/2605.14892v2#bib.bib111) | Record | Tiered | Eviction | Autonomous | ●○○ | Conv, QA | 2024 |
| HippoRAG [119](https://arxiv.org/html/2605.14892v2#bib.bib119) | Distill | Graph | Incremental | Graph trav. | ○●○ | QA | 2024 |
| EMG [120](https://arxiv.org/html/2605.14892v2#bib.bib120) | Distill | Graph | Rewrite | Similarity | ●●○ | Conversation | 2024 |
| SeCom [115](https://arxiv.org/html/2605.14892v2#bib.bib115) | Distill | Flat | Append | Similarity | ●●○ | Conversation | 2025 |
| ENGRAM [116](https://arxiv.org/html/2605.14892v2#bib.bib116) | Routing | Typed | Append | Type-aware | ●●● | Conversation | 2025 |
| Mem0 [114](https://arxiv.org/html/2605.14892v2#bib.bib114) | Distill | Graph | CRUD | Similarity | ○●○ | Conversation | 2025 |
| MemTree [121](https://arxiv.org/html/2605.14892v2#bib.bib121) | Distill | Tree | Compression | Tree trav. | ○●○ | QA | 2025 |
| A-MEM [122](https://arxiv.org/html/2605.14892v2#bib.bib122) | Distill | Graph | Evolution | Similarity | ○●○ | Conversation | 2025 |
| Zep [118](https://arxiv.org/html/2605.14892v2#bib.bib118) | Distill | Graph | Versioning | Hybrid | ●●○ | Conv, Ent | 2025 |
| RecurSum [113](https://arxiv.org/html/2605.14892v2#bib.bib113) | Distill | Flat | Compression | Similarity | ○●○ | Conversation | 2025 |
| COMEDY [124](https://arxiv.org/html/2605.14892v2#bib.bib124) | Distill | Flat | Compression | Similarity | ●●○ | Conversation | 2025 |
| MemOS [125](https://arxiv.org/html/2605.14892v2#bib.bib125) | Routing | Unified | Lifecycle | Routed | ●●● | General | 2025 |
| MemRL [126](https://arxiv.org/html/2605.14892v2#bib.bib126) | Record | Flat | Learned | Utility-driven | ●○○ | General | 2026 |

</div>

</div>
</details>

### 规划

与单步推理不同，规划需要在长视野跨度下考虑子任务依赖性、环境反馈以及潜在的执行失败。现在将方法划分为两种范式：基于分解的规划以及基于搜索的规划

#### 基于分解的规划

基于分解的方法可以简单理解为：将长程任务或复杂任务分解成多个易于管理、易于并行、易于执行的子任务，主要区别在于分解相对于执行过程发生的时间节点。

##### 前瞻性分解

前瞻性分解在采取任何动作之前生成一份完整的规划。`Least-to-Most` 引入了将问题逐步分解为更简单的子问题并依序求解的思想。`Plan-and-Solve` 则用显式的规划阶段及随后的依序执行取代了通用的“think step by step”。`HuggingGPT` 将用户的**多模态**请求分解为任务依赖图（即拆解为任务1、2、3...，其中任务3依赖任务2，任务2依赖任务1），随后根据规划出的每个子任务去HuggingFace上去寻找对口模型进行工作。

LLM 做长链规划的时候有**逻辑不严密**、**自回归生成太慢**、**容易在细节里迷失方向**。`Planning Anything` (`LLMFP`)通过运筹学的规划器把任何自然语言规划问题写成一段调用形式化求解器，但使用场景较为局限。`Plan-and-Act` 专门针对长视野 Web 任务，将高层规划与底层动作执行相分离，模型通过训练集来理解应该先去做什么以此来进行规划，证明了显式前瞻性规划在复杂 Web 基准测试中显著优于反应式方法。

#### 基于搜索的规划

基于搜索的方法显式探索多条候选路径，并通过评估、剪枝或回溯在其中做出选择

##### 步骤级搜索

步骤级搜索在每个决策点展开并评估候选方案。蒙特卡洛树搜索（MCTS）已被证明对基于大语言模型的规划尤为有效。MCTS 的决策公式可表现为：

<div style="max-width:100%;overflow-x:auto">

$$
a^* = \arg\max_{a} \left[ \underset{\text{利用 : 历史平均回报}}{Q(s, a)} + c \cdot \underset{\text{先验概率 }}{P(s, a)} \cdot \underset{\text{探索: 越少走过的路分越高}}{\frac{\sqrt{N(s)}}{1 + N(s, a)}} \right]
$$

</div>

在决定下一步的时候，它会选择得分最高的节点，其中 $Q$ 是以前走过的路（效果有多好），$N$ 为在该状态下执行动作的次数有多少，$N$ 越小决策就越鼓励去尝试新方法。

在此基础上，`LLM-MCTS`、`RAP`、`REST-MCTS` 都相继进行了优化和改进，其中 `REST` 利用了 PRM 模型来针对于每个决策节点进行评分从而进行微调达到性能提升。

除此之外，`AFlow` 使用了类似 `Dream-RSI` 的方法（可以说 `Dream-RSI` 参考了 `AFlow`），在任务动作节点上进行扩展，缺点是成本太高。`ToolChain` 利用了 A* 算法来进行启发式扩展，也有利用扩展模型进行隐式搜索的方法。

**表 3.** 按规划范式组织的规划增强方法比较

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Table 3</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

策略（Strategy）：规划与执行之间的时间关系（表 A 部分）或搜索算法（表 B 部分）——Proactive 执行前完整规划；Progressive 规划与执行交织循环；MCTS 蒙特卡洛树搜索；A _启发式 A_ 搜索；Retrieve 检索引导；Diffusion 基于扩散；Greedy 贪心。结构（Structure）：规划表征（表 A 部分）或搜索空间（表 B 部分）——Seq = 顺序序列；DAG = 有向无环图；Tree = 树状结构；Program = 编程式；State/Action/Plan = 相应的搜索空间。执行模式（Exec Mode）：Seq = 顺序执行；Hier = 分层执行；Intlv = 与规划交织。

<div class="survey-table" tabindex="0" role="region" aria-label="表 3">

| **Method** | **Strategy** | **Structure** | **Exec** **Mode** | **Replan** | **Task** **Domain** | **Year** |
| --- | --- | --- | --- | --- | --- | --- |
| *A. Decomposition-Based Planning* |  |  |  |  |  |  |
| Least-to-Most [130](https://arxiv.org/html/2605.14892v2#bib.bib130) | Proactive | Seq | Seq | — | General | 2023 |
| Plan-and-Solve [131](https://arxiv.org/html/2605.14892v2#bib.bib131) | Proactive | Seq | Seq | — | General | 2023 |
| HuggingGPT [132](https://arxiv.org/html/2605.14892v2#bib.bib132) | Proactive | DAG | Hier | — | General | 2023 |
| LLM-Planner [134](https://arxiv.org/html/2605.14892v2#bib.bib134) | Proactive | Seq | Seq | ✓ | Robotics | 2023 |
| LLM+P [135](https://arxiv.org/html/2605.14892v2#bib.bib135) | Proactive | Seq | Seq | — | Robotics | 2023 |
| ReAct [18](https://arxiv.org/html/2605.14892v2#bib.bib18) | Progressive | Seq | Intlv | — | General | 2023 |
| DEPS [142](https://arxiv.org/html/2605.14892v2#bib.bib142) | Progressive | Seq | Intlv | ✓ | General | 2023 |
| LLMCompiler [133](https://arxiv.org/html/2605.14892v2#bib.bib133) | Proactive | DAG | Hier | ✓ | Tool | 2024 |
| Skeleton-of-Thought [137](https://arxiv.org/html/2605.14892v2#bib.bib137) | Proactive | Tree | Hier | — | General | 2024 |
| Voyager [128](https://arxiv.org/html/2605.14892v2#bib.bib128) | Progressive | Seq | Intlv | ✓ | General | 2024 |
| ADaPT [139](https://arxiv.org/html/2605.14892v2#bib.bib139) | Progressive | Tree | Intlv | ✓ | General | 2024 |
| CodeAct [141](https://arxiv.org/html/2605.14892v2#bib.bib141) | Progressive | Program | Intlv | ✓ | General | 2024 |
| KnowAgent [143](https://arxiv.org/html/2605.14892v2#bib.bib143) | Progressive | Seq | Intlv | ✓ | General | 2024 |
| Planning Anything [136](https://arxiv.org/html/2605.14892v2#bib.bib136) | Proactive | Program | Seq | — | General | 2025 |
| Plan-and-Act [138](https://arxiv.org/html/2605.14892v2#bib.bib138) | Proactive | Seq | Hier | — | Web | 2025 |
| SelfGoal [140](https://arxiv.org/html/2605.14892v2#bib.bib140) | Progressive | Tree | Intlv | ✓ | General | 2025 |
| *B. Search-Based Planning* |  |  |  |  |  |  |
| LLM-MCTS [145](https://arxiv.org/html/2605.14892v2#bib.bib145) | MCTS | State | Seq | — | Robotics | 2023 |
| RAP [147](https://arxiv.org/html/2605.14892v2#bib.bib147) | MCTS | State | Seq | — | General | 2023 |
| PromptAgent [146](https://arxiv.org/html/2605.14892v2#bib.bib146) | MCTS | State | Seq | — | General | 2024 |
| ToolChain* [150](https://arxiv.org/html/2605.14892v2#bib.bib150) | A* | Action | Seq | — | Tool | 2024 |
| REST-MCTS* [148](https://arxiv.org/html/2605.14892v2#bib.bib148) | MCTS | State | Seq | — | General | 2024 |
| PlanRAG [153](https://arxiv.org/html/2605.14892v2#bib.bib153) | Retrieve | Plan | Seq | ✓ | QA | 2024 |
| AFlow [149](https://arxiv.org/html/2605.14892v2#bib.bib149) | MCTS | Plan | Seq | — | General | 2025 |
| DiffuSearch [151](https://arxiv.org/html/2605.14892v2#bib.bib151) | Diffusion | State | Seq | — | General | 2025 |
| Agent S [127](https://arxiv.org/html/2605.14892v2#bib.bib127) | Greedy | Plan | Hier | ✓ | Web | 2025 |

</div>

</div>
</details>

## 多智能体协同

多智能体系统（MAS）类似于人类社会中的分工与合作，其中智能体通过协同解决复杂问题。通过整合具备不同能力的智能体，多智能体系统能够应对超出任何单个智能体能力范围的任务。在本文中，我们将多智能体系统 $S$ 形式化定义为

<div style="max-width:100%;overflow-x:auto">

$$
S=(\mathcal{A},\;\mathcal{E},\;\mathcal{C},\;\mathcal{G},\;\Pi),
$$

</div>

其中 $\mathcal{A}=\{a_{1},\ldots,a_{N}\}$ 为智能体集合； $\mathcal{E}$ 表示可能影响多智能体系统 $S$ 运行的共享环境； $\mathcal{C}$ 为规范消息交换的通信协议；$\mathcal{G}$ 表示描述多智能体系统 $S$ 架构的编排拓扑； $\Pi$ 表示协同策略集合。

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/collaboration-overview.png" width="1822" height="1733" alt="图 3：多智能体协同系统概览。右侧以基础层、结构层和执行层组织角色、通信、编排拓扑与交互；左侧展示从概念与领域建模到性能评估与反馈的系统生命周期。" loading="lazy" decoding="async">
  <figcaption>图 3：多智能体协同系统概览。右侧以基础层、结构层和执行层组织角色、通信、编排拓扑与交互；左侧展示从概念与领域建模到性能评估与反馈的系统生命周期。</figcaption>
</figure>

### 角色

理解角色需要回答两个基本问题：智能体能够做什么，以及责任如何在它们之间分配。因此，我们将角色分解为两个维度：角色能力回答第一个问题，而角色分配则通过映射函数 $\rho(\cdot)$ 分配这些能力来回答第二个问题。

#### 角色能力

顾名思义，角色能力就是 LLM 自身所具备的能力，角色能力大致分为同质角色能力和异质角色能力两种。

##### 同质角色能力

在 MAS 中，同质角色能力指的是由相同的 LLM 驱动，即多智能体均由同一智能体驱动，拥有**完全对称的推理能力、相同的工具集和相同的视野范围**。它们之间唯一的区别，仅仅是 Prompt 里写的人设句子不同（比如一个被告知“你是甲方提问者”，另一个被告知“你是乙方回答者”）。

- **优点**：工程实现极简（一套模型接口和工具集到处复用），且在学术研究上提供了**控制变量的纯净环境**（排除不同模型能力差异的干扰，纯粹研究协作协议本身）。
- **缺点**：遇到高难任务时，同 LLM 的知识盲区一样，难以突破。

##### 异质角色能力

与上一部分相反，由不同 LLM 进行驱动，可以合理利用各 LLM 擅长的能力，组合将能力、成本、性价比最优化。让不同 Agent 在**底座模型、动作权限、或信息视野**上真正产生本质差异。

可以通过固定不同的 LLM、按照职能切分、通过后训练和 RL 来自行决定等方法来进行分配。

#### 角色分配

**静态角色分配**：指定智能体或子智能体担任的角色和职责，在任务分配之前安排好，过程中不会改变，这使得工作流程边界清晰、零临场组队开销、极其稳定。同时这种分配只适用于较为局限的场景下，对于动态或不确定的场景下性能表现较差。

**动态角色分配**：动态角色分配允许系统在运行时根据当前任务有多难、进行到哪一步了，实时改变子智能体的分配策略。及其灵活，具备较高的通用性，但相对的，动态分配的策略依赖于 Agent 架构和主 LLM 的能力，因此实现较为复杂，并且管理复杂度也会上升。

### 多智能体通信

多智能体通信可以分为通信模式和通信协议

#### 通信模式

**显式通信**：目前最主流的通信方式，智能体与智能体之间通过标准化的文字、或以聊天形式呈现。该方式可以通过约束聊天内容来减少 Token 消耗，不过整体依然很冗余，效率并不高。

**隐式通信**：智能体与智能体之间可以不直接发消息，可以通过观察环境变化、动作轨迹来判断其它子智能体做了什么。

**更少见的**，可以通过潜空间亦或是隐变量交流（`LatentMAS`），**我认为这是未来一两年的研究主流，也是 RSI 的必经之路**，但这种缺点在于不好控制、容易触发安全红线，因为我们看不到智能体之间在交流什么。一个近期较为具体的例子是最近 OpenAI 的 Hugging Face 事件是若干独立智能体在爬取 HF 网站的时候，通过轨迹、日志判断出还有其它智能体进行着相似任务，从而它们绕过监督进行了交流，造成了大规模入侵事件。

#### 通信协议

主流通信协议有以下三类：

- **A2T (Agent-to-Tool)**：智能体 $\leftrightarrow$ 外部工具/数据库
- **A2A (Agent-to-Agent)**：智能体 $\leftrightarrow$ 智能体
- **A2S (Agent-to-System)**：智能体 $\leftrightarrow$ 宿主平台/前端网关

A2T 目前 MCP 成为主流，相关的内容我之前的博客里面也有写。

A2A 是 Google 联合 50+ 科技巨头共同提出的，它负责解决不同责任的 Agent 之间的通信

**核心机制**：每个 Agent 对外公布一张名片叫 **`Agent Card`**（一个 JSON 文件，写明我擅长干什么、收什么格式的参数、怎么鉴权）。然后两个黑盒 Agent 就能像发 HTTP 请求一样直接点对点派活、通过 SSE 实时同步长任务的进度，以及互传产出物。

其中， **`Agora`** 是由牛津大学提出的“自适应语言切换”协议，它认为智能体之间应该由低频的二进制语言或其它被压缩过的语言进行交流，这一点很接近上述潜空间的定义。

**表 4.** 智能体通信协议对比。

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Table 4</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

通信类型（Comm. Type）：A2A = 智能体对智能体；A2T = 智能体对工具；A2S = 智能体对系统。消息格式（Msg. Format）描述数据序列化方式，其中 PD = Protobuf/数据驱动；NL = 自然语言；TD = 物描述；SSE = 服务器发送事件。核心功能（Core Functions）：该协议提供的主要能力或服务。

<div class="survey-table" tabindex="0" role="region" aria-label="表 4">

| **Protocol** | **Proposer** | **Comm.** **Type** | **Msg. Format** | **Core Functions** | **Year** |
| --- | --- | --- | --- | --- | --- |
| Agora [275](https://arxiv.org/html/2605.14892v2#bib.bib275) | University of Oxford | A2A | PD | Consensus formation | 2024 |
| MCP [276](https://arxiv.org/html/2605.14892v2#bib.bib276) | Anthropic | A2T | JSON-RPC | Standardized context access | 2024 |
| ANP [277](https://arxiv.org/html/2605.14892v2#bib.bib277) | ANP Community | A2A | JSON-LD+NL | Meta-protocol negotiation | 2024 |
| LMOS [278](https://arxiv.org/html/2605.14892v2#bib.bib278) | Eclipse Foundation | A2S | JSON-LD+TD | Group management | 2024 |
| ACP [279](https://arxiv.org/html/2605.14892v2#bib.bib279) | IBM | A2A | JSON-LD | Async & Sync interaction | 2025 |
| A2A [26](https://arxiv.org/html/2605.14892v2#bib.bib26) | Google | A2A | JSON-RPC/SSE | Agent discovery | 2025 |
| Agent Protocol [280](https://arxiv.org/html/2605.14892v2#bib.bib280) | AI Engineer Foundation | A2S | REST/HTTP | Standardized task API | 2025 |

</div>

</div>
</details>

### 编排

编排描述了在多智能体系统 $S$ 中规范多个智能体如何组织与交互的全局协调结构。从系统级视角来看，编排可以被视为协同拓扑 $\mathcal{G}=(\mathcal{A},\mathcal{E}_{c})$：它定义了智能体 $a_{i}\in\mathcal{A}$ 如何相连、信息如何在它们之间的通信链路 $\mathcal{E}_{c}$ 中流动，以及在任务执行过程中协调决策是如何构建的。编排聚焦于更高层次的结构组织，决定了这些角色与通信如何被整合为一个连贯的协同过程。编排主要可以分为三大类：集中式编排、分布式编排、混合式编排。

#### 集中式编排

集中式编排为中心化一个智能体来调度下面的子智能体做任务。`AOP` 尝试不再让中心智能体单纯凭借知识来拆解任务分给子智能体，而是通过一个轻量级奖励模型给“子任务给子智能体的匹配度”打分，将调度分为可解性（分配的任务子智能体必须有能力完成）、完备性、非冗余性。`CoRL`针对于多基座模型协作通过强化学习训练了一个中心控制器，让他在每一步决定调用哪个专家模型，其目标奖励函数为：

<div style="max-width:100%;overflow-x:auto">

$$
R(\tau, \lambda) = \underbrace{\text{Acc}(\tau)}_{\text{任务最终准确率}} - \lambda \cdot \underbrace{\sum_{t=1}^T \text{Cost}(a_t)}_{\text{沿途调用各专家模型的累计API花销}}
$$

</div>

其中 $\lambda \ge 0$，当 $\lambda$ 较小的时候，意味着预算充足，可以多调用专家模型，相同的，$\lambda$ 较大则鼓励只调用少量专家来解决任务。

在集群智能体（大规模的智能体协作）场景下，中心调度会出现注意力稀疏、单点崩盘的后果。

#### 分布式编排

顾名思义，分布式编排没有中心调度器，每个智能体都是独立的个体不受人控制，但没有了中心管理，如何让多智能体一起工作成为了主要难题。

`AgentNet` 给每个智能体分配了一个 Router 和 Executor，网络开始是全连接的，当一个智能体收到任务时，它会判断该任务自己是否可以完成，否则拆一部分分派给其它智能体，通过 EMA 动态权重来进行动态分配，以确保分配给的智能体有能力去做这部分任务。

`MAAC` 则通过多 LLM 训练来解决，它把每个智能体设为独立的 Actor，引入 Critic 模型评估每一步的协同价值。`CoLLM-CC`（ **CTDE：中心化训练，去中心化执行**）：在**训练阶段**用一个能看到所有人状态的全局 Critic 来指导各个 LLM 更新梯度；到了**实际部署运行阶段**，撤掉 Critic，各个 LLM 完全独立分布式运行。实验证明，在长视野稀疏奖励任务中，`CoLLM-CC` 收敛速度和协作效果远超纯启发式规则。

`AHKG` 试图解决多智能体在工作时的视野局限性问题，没有了中心智能体就没有人能从上帝视角来看整个任务的进度，可以通过通信机制，或者通过分层知识图谱。两个 Agent 相遇或通信时对知识图谱的“高层语义节点”和“任务相关局部子图”做差异合并。

#### 混合编排

在宏观上保留“中心化的顶层设计与目标约束”，在微观执行上放权给底层做“去中心化的局部自治与并行沟通”。

`GoA` 通过在局部 Agent 之间动态建立非线性的有向边，使得相邻节点自由互相补充和纠错。同时，整张图的构建和消息池化受到一个**全局预定义拓扑目标**（如信息论中的信息瓶颈压缩目标或全局任务对齐约束）的严格统领，确保局部的自由讨论最终收敛到统一的中心目标上。

`AdaptOrch` 针对不同的任务选择了不同的拓扑编排。`AdaptOrch` 设了一个中心化的**自适应元协调器**。它先画出任务的依赖有向无环图 $G = (V, E)$，然后只花线性时间 $O(|V|+|E|)$ 计算图的**结构特征**（如最大宽度/并行度、最大深度/串行链长、子图耦合度），从 **4 种标准拓扑** 中动态选出最匹配的一种：

1. **Parallel**：当子任务完全独立（$|E| \approx 0$）时启用；
2. **Sequential**：当任务是单链条强因果依赖时启用；
3. **Hierarchical**：当任务跨模块且需要中心汇总对账时启用；
4. **Hybrid**：当任务图里既有并行簇又有长依赖链时动态组合启用。

## 多智能体系统故障归因

在单智能体中，一般是可以很好定位到错误，它的产出结果由它一人负责。但是在多智能体系统中，一个智能体的产出并不会立刻报错，而且会顺着交互链一直延续下去，而其它智能体可能会受该产物影响又产生新的错误。因此错误归因变得极其重要。

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/failure-attribution.png" width="1731" height="946" alt="图 4：多智能体系统中的故障归因机制，展示执行轨迹、故障传播和归因分析。" loading="lazy" decoding="async">
  <figcaption>图 4：多智能体系统中的故障归因机制，展示执行轨迹、故障传播和归因分析。</figcaption>
</figure>

### 诊断

多智能体执行轨迹 $\tau$ 是指在给定的用户查询、初始环境状态和系统配置下形成的时间有序动态序列，可以形式化为：

<div style="max-width:100%;overflow-x:auto">

$$
\tau=\big((i_{0},u_{0}),\dots,(i_{t},u_{t}),\dots,(i_{H},u_{H})\big),
$$

</div>

其中 $H$ 表示轨迹时域长度，$(i_{t},u_{t})$ 则表示在时间步 $t$ 处于活跃状态的智能体及其相应的行动。

为了定义故障发生的条件，引入了轨迹评估函数 $Z(\tau)$。当 $Z(\tau)=1$ 时，表示轨迹 $\tau$ 的最终结果状态被评估为异常。故障归因过程可形式化为：

<div style="max-width:100%;overflow-x:auto">

$$
(I,t)=\begin{cases}f(\Omega,\tau,q),&\text{if }Z(\tau)=1,\\
\emptyset,&\text{otherwise}.\end{cases}
$$

</div>

只有当系统轨迹被判定为异常时（即 $Z(\tau)=1$），归因模型 $f$ 才会以系统配置 $\Omega$、轨迹 $\tau$ 和用户查询 $q$ 作为输入，并返回对故障负责的智能体及对应的决定性时间步（表示为二元组 $(I,t)$）；若系统运行正常，则输出空集 $\emptyset$。

### 故障分类

可以将现有的故障分类体系归纳为三大正交视角：**系统结构视角**、**执行阶段视角** 和 **因果生命周期视角**。

#### 系统结构视角

这一视角聚焦于智能体之间的组织架构与交互边。基本方法是定义若干类错误（`MAST` 定义了 3 大类 14 子类、`AgentAsk` 定义了 4 种交接错误类），然后通过根据执行轨迹来判断某个智能体的轨迹是否出错，归属于哪一类（一般通过 LLM 来决策），其中 `AgentAsk` 增加了追问打回的流程，即判断出错误后，进一步的向产生错误的 Agent 澄清问题重新做。

#### 执行视角

这一视角不再按智能体之间的边来切分，而是按Agent 内部的功能模块或任务执行的时序阶段来切分。

`AgentErrorTaxonomy` / `AgentErrorBench` / `AgentDebug` 采用了模块化的 Agent 架构，将错误归因于五个分类：记忆、反思、规划、执行、系统级操作；将每一步 $t$ 的输出按功能拆解为五个显式字段：$u_t = \{m_t^{\text{mem}}, m_t^{\text{ref}}, m_t^{\text{plan}}, m_t^{\text{act}}, m_t^{\text{sys}}\}$，利用 Critic LLM 选择出错误模块节点集合 $\mathcal{C} = \{(t_k, \text{module}_k, \mathrm{error\_type}_k)\}$ 然后在该集合中找到最早触发错误的节点，之后可以进行回溯修复重做。

#### 因果生命周期视角

前两个视角（系统结构、执行阶段）本质上还是在给“单个故障点”贴静态标签；而因果生命周期视角则把故障看作一个活的、动态演化的生物体，重点刻画它发生错误的全过程。

故障引入的时间步（Originating Point $t_{\text{origin}}$）与故障暴露为可见异常的时间步（Manifestation Point $t_{\text{manifest}}$）往往严重分离，`FailCycle` 将 MAS 中的错误划分为四个阶段：Fault Introduction (潜伏引入)、Propagation (隐蔽传播)、Evolution / Mutation (跨节点变异)、Manifestation (显性爆发)，随后依然是构建图然后定位错误进行回溯处理。

`AGENTSCOPE` 将异常归结为五种高层行为模式，提出了一套 **神经符号诊断框架**。它利用 LLM 将轨迹 $\tau$ 整合成规则化节点：`Node_t = {Intent(意图), Pre-condition(前置观测状态), Action(调用的工具/发出的指令), Post-condition(执行后状态变化)}`，然后对 Agent 进行规范化定义，自动生成一组**神经不变量断言**，后再基于这组轨迹，利用 Critic LLM 进行判定，直接映射对应节点的故障模式。
