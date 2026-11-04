---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-04T23:08:16+08:00
description: "多智能体故障归因基准 Who&When 与 MP-Bench 的阅读笔记。"
draft: false
isCJKLanguage: true
---

## Who&When
由于是 25 年中旬的论文，用的模型较老，以 o1 和 R1 为测评模型，在该数据集上的表现为：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-reasoning-models.png" width="1464" height="244" alt="OpenAI o1 和 DeepSeek R1" loading="lazy" decoding="async">
  <figcaption>OpenAI o1 和 DeepSeek R1</figcaption>
</figure>

数据集的主要构成：由算法生成的 Agent System 和人工构建的 Agent System ；共 127 个系统，由人工标注，具体来说分为两类错误：Agent-Level（Who，即定位到具体错误的智能体），Step-Level（When，即定位到具体的步骤）；其中 Step-Level 表现较差。
分为了三种测评方法，分别是 All-at-Once、Step-by-Step、Binary-Search 三种方法，表现性能如下，以 4o 为例：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-gpt4o-methods.png" width="1780" height="720" alt="GPT-4o" loading="lazy" decoding="async">
  <figcaption>GPT-4o</figcaption>
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
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-attribution-results.png" width="1632" height="500" alt="在 MP-Bench 上各系统的失败归因与归因推理性能对比。" loading="lazy" decoding="async">
  <figcaption>在 MP-Bench 上各系统的失败归因与归因推理性能对比。</figcaption>
</figure>

论文还测了在多智能体测试下的结果：

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-multi-llm-results.png" width="768" height="412" alt="单大模型系统与多大模型融合失败归因系统之间的性能对比" loading="lazy" decoding="async">
  <figcaption>单大模型系统与多大模型融合失败归因系统之间的性能对比</figcaption>
</figure>
