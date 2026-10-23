---
title: "Multi-Agent Systems Survey Notes"
date: 2026-10-23T00:00:00+08:00
lastmod: 2026-10-23T16:37:38+08:00
description: "《Beyond Individual Intelligence》阶段性阅读笔记，整理多智能体系统的问题、个体推理增强与记忆方法。"
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

### 推理

CoT 通过精心设计的提示诱导出中间推理步骤，在不改变模型参数的情况下大幅度提升了复杂任务的性能。核心挑战便转变为外部机制应如何弥合模型的静态参数化知识与现实世界任务的动态需求之间的鸿沟。遵循这一视角，我们沿着推理管线的信息流来组织现有方法，将其划分为三个阶段：

1. _输入阶段增强_，通过外部知识检索与多模态感知来拓展模型可获取的信息边界；
2. _推理过程增强_，通过搜索空间扩展与路径验证来优化推理轨迹；
3. _输出阶段调控_，通过事后评估与自适应修正来提升生成结果的可靠性与事实准确性。

#### 查询增强

RAG 将参数化生成器与非参数化检索器相结合，使得模型能在推理阶段从外部知识库中获取相关文档。在此基础上，后续的工作有

1. 通过训练模型来自适应决定如何检索、通过反思标记对检索内容和生成内容进行评判
2. 利用知识图谱进行检索，查询相关子图、在知识图谱上进行多跳推理
3. Knowledge Card 设计多个小模型作为各个领域可插拔的知识模块，小模型负责生成背景材料，依然通过检索由大模型来回答
4. RuAG 利用大模型辅助确定候选条件和预测目标，通过蒙特卡洛树搜索从离线数据中挖掘可靠的逻辑规则，再将规则转化为自然语言，直接或按需检索后注入提示词，指导大模型进行推理和生成。

#### 推理增强

对于推理轨迹本身，沿两个维度对其进行组织：搜索空间扩展，用于增加候选解的覆盖度；以及路径验证，用于提供识别最可靠结果所需的选择压力。

**搜索空间扩展**

CoT、ToT、GoT 依赖人工设计的提示协议，DeepSeek-R1 通过大规模的 RL 训练模型自主生成长思维链，展现出纯粹源自奖励优化的自我验证、自发回溯、自我批评的能力。

**推理路径验证**

拓展了搜索空间能够产生更多的候选解，更多的候选解意味着会引入等量的错误路径。目前的方法通过采样多条输出后内部投票、维护失败自迭代等方式，这种方法不改变模型的参数。 PRM 通过对推理步骤进行独立评分，大幅度提升了模型的推理能力，后续的进展基于 PRM 整合到 RL 训练过程中，证实了过程监督在同时塑造习得策略与选择机制时最为有效。

#### 输出检验

目前针对输出内容的可靠性评估分为两类。一类是通过将输出分解为多个待检验项，然后逐步核对知识库，还可以扩展到代码、数学推理、科学文献等更细粒度的错误定位。另一类则是不借助外部知识，通过衡量多个样本的一致性来检测幻觉，或是模型自主去检验、独立回答，亦或者多条样本根据投票选出胜者，然后以此进行自迭代。

**表 1.** 推理增强方法对比，按干预阶段和子类型编排。

<details class="survey-details">
<summary>
  <svg class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Editable table transcription</span>
  <svg class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
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
  <svg class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Editable table transcription</span>
  <svg class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
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
