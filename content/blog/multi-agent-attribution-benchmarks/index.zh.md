---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-05T22:20:38+08:00
description: "多智能体故障归因基准 Who&When 与 MP-Bench 的阅读笔记。"
draft: false
isCJKLanguage: true
---

## Who&When
由于是 25 年中旬的论文，用的模型较老，以 o1 和 R1 为测评模型，在该数据集上的表现为：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-reasoning-models.png" width="1464" height="244" alt="表 1：OpenAI o1、DeepSeek R1 与 GPT-4o 的失败归因性能对比" loading="lazy" decoding="async">
  <figcaption>表 1：OpenAI o1、DeepSeek R1 与 GPT-4o 的失败归因性能对比</figcaption>
</figure>

数据集的主要构成：由算法生成的 Agent System 和人工构建的 Agent System ；共 127 个系统，由人工标注，具体来说分为两类错误：Agent-Level（Who，即定位到具体错误的智能体），Step-Level（When，即定位到具体的步骤）；其中 Step-Level 表现较差。
分为了三种测评方法，分别是 All-at-Once、Step-by-Step、Binary-Search 三种方法，表现性能如下，以 4o 为例：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-gpt4o-methods.png" width="1780" height="720" alt="表 2：GPT-4o 在有无真实标签下的失败归因方法性能对比" loading="lazy" decoding="async">
  <figcaption>表 2：GPT-4o 在有无真实标签下的失败归因方法性能对比</figcaption>
</figure>

数据集实例：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-example.png" width="1424" height="856" alt="图 1：Who&When 数据集中的算法生成系统与人工构建系统失败日志示例。" loading="lazy" decoding="async">
  <figcaption>图 1：Who&When 数据集中的算法生成系统与人工构建系统失败日志示例。</figcaption>
</figure>

## MP-Bench

不同的分析人员可以合情合理地将失败归因于不同的步骤。这种非确定性和多视角的本质源于多智能体系统内在的相互依赖性。多个专业智能体可以相互依赖地组合成旨在达成同一目标的多样化有效执行轨迹。因此，失败归因在很大程度上取决于观察者对于何为“正确”轨迹的内在心智推理。

以往工作中观察到的局限源于忽视 MAS 失败多视角特性的评测假设偏差，而非模型能力不足。
<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-perspectives.png" width="1640" height="724" alt="图 2：说明多智能体系统失败归因多视角特性的动机示例。" loading="lazy" decoding="async">
  <figcaption>图 2：说明多智能体系统失败归因多视角特性的动机示例。</figcaption>
</figure>

MP-Bench 采用归一化折损累计增益（nDCG@K），该指标评估预测排序与真实排序之间的一致性，更加重视对高度显著的失败步骤进行正确优先排序。

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-attribution-results.png" width="1632" height="500" alt="表 3：在 MP-Bench 上各系统的失败归因与归因推理性能对比。" loading="lazy" decoding="async">
  <figcaption>表 3：在 MP-Bench 上各系统的失败归因与归因推理性能对比。</figcaption>
</figure>

论文还测了在多智能体测试下的结果：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-multi-llm-results.png" width="768" height="412" alt="表 4：单大模型系统与多大模型融合失败归因系统之间的性能对比" loading="lazy" decoding="async">
  <figcaption>表 4：单大模型系统与多大模型融合失败归因系统之间的性能对比</figcaption>
</figure>

## TrajErrBench

目前的一些数据集存在以下问题：

- **轨迹普遍偏短**：Who&When 的算法生成子集平均只有 **8.72 步**，很难测试长上下文下的证据丢失问题。
- **场景相对单一或偏玩具化**：多集中在简单的多智能体对话、网页购物（WebShop）或文字房间游戏（ALFWorld）。
- **缺少真实复杂场景**：缺乏在工业界最受关注的**复杂多轮工具调用**（如客服业务规则约束）和**超长程仓库级代码开发**场景。

TrajErrBench 由两个互补的真实高难度领域子集构成：

**表 5. TrajErrBench 数据集构成**

<div class="survey-table" tabindex="0" role="region" aria-label="表 5. TrajErrBench 数据集构成">

| 子集名称                    | 轨迹数量       | 平均步数     | 任务场景与特点                                                                                             | 产生失败轨迹的LLM                                                                                     |
| ----------------------- | ---------- | -------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| <br>$\tau^{2}$-Bench 子集 | 400 条      | 29.27 步  | 多轮客服与工具调用（航空 Airline & 零售 Retail）：智能体需要一边和模拟用户多轮对话收集需求，一边调用后台数据库 API，并严格遵守复杂业务政策（如退改签规则、预算限制、舱位规定）。 | DeepSeek-Reasoner、  <br>GPT-5、  <br>Qwen3-Max                                                  |
| SWE-Bench Pro 子集        | 86 条  <br> | 119.70 步 | 超长程仓库级软件工程：智能体在真实的 GitHub 代码仓库中定位 Bug、阅读多个源码文件、编写补丁并运行测试。轨迹极长，上下文高度复杂。                              | 基于 OpenHands 框架运行：  <br>Claude-Opus-4.6、Kimi-K2.6-Coding、Gemini-3.1-Pro、Grok-4.20-Beta、GPT-5.4 |
| 合计                      | 486 条      | —        | 覆盖真实工具交互与长程代码修复                                                                                     | 8 个前沿大模型                                                                                       |

</div>

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/trajerrbench-critical-error-results.png" width="1847" height="808" alt="表 6：多基准关键错误检测的主要实验结果" loading="lazy" decoding="async">
  <figcaption>表 6：多基准关键错误检测的主要实验结果</figcaption>
</figure>

作者在该数据集中发现了以下问题：

1. 关键错误往往在轨迹的后半段；在以前的数据集里，智能体经常一上来前几步就犯致命错误。但在该数据集中，作者发现 Agent 之前大部分都是在进行分析、解读而不是执行，开始做核心决策或修改代码时，才会犯下致命错误。
2. Agent 往往在推理阶段出问题，在 $\tau^{2}$-Bench 中占 **60.7%**，在 SWE-Bench Pro 中占 **57.1%**。
