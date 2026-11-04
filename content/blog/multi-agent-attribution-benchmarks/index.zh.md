---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-04T21:57:38+08:00
description: "多智能体故障归因基准 Who&When 与 MP-Bench 的阅读笔记。"
draft: false
isCJKLanguage: true
---

## Who&When
由于是 25 年中旬的论文，用的模型较老，以 o1 和 R1 为测评模型，在该数据集上的表现为：

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>OpenAI o1 和 DeepSeek R1</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="OpenAI o1 和 DeepSeek R1">

|  | **GPT-4o** |  | **OpenAI o1** |  | **DeepSeek R1** |  |
| --- | --- | --- | --- | --- | --- | --- |
| **Accuracy** | Agent-Level | Step-Level | Agent-Level | Step-Level | Agent-Level | Step-Level |
| **All-at-Once** | 54.31 | 4.39 | 41.38 | **10.34** | **56.90** | 3.45 |
| **Step-by-Step** | 33.62 | 7.90 | **36.21** | **13.79** | 32.76 | 6.90 |

</div>
</div>
</details>

数据集的主要构成：由算法生成的 Agent System 和人工构建的 Agent System ；共 127 个系统，由人工标注，具体来说分为两类错误：Agent-Level（Who，即定位到具体错误的智能体），Step-Level（When，即定位到具体的步骤）；其中 Step-Level 表现较差。
分为了三种测评方法，分别是 All-at-Once、Step-by-Step、Binary-Search 三种方法，表现性能如下，以 4o 为例：

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>GPT-4o</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="GPT-4o">

|  | **With Ground Truth** |  | **Without Ground Truth** |  |
| --- | --- | --- | --- | --- |
| **Agentic Systems Types** | Algorithm Generated | Hand Crafted | Algorithm Generated | Hand Crafted |
| Random |  |  |  |  |
| Agent-Level Accuracy | 29.10 | 12.00 | 29.10 | 12.00 |
| Step-Level Accuracy | 19.06 | 4.16 | 19.06 | 4.16 |
| All-at-Once |  |  |  |  |
| Agent-Level Accuracy | **54.33** | **55.17** | **51.12** | **53.44** |
| Step-Level Accuracy | 12.50 | 5.26 | 13.53 | 3.51 |
| Step-by-Step |  |  |  |  |
| Agent-Level Accuracy | 35.20 | 34.48 | 26.02 | 32.75 |
| Step-Level Accuracy | **25.51** | **7.02** | 15.31 | **8.77** |
| Binary Search |  |  |  |  |
| Agent-Level Accuracy | 44.13 | 51.72 | 30.11 | 36.21 |
| Step-Level Accuracy | 23.98 | 6.90 | **16.59** | 6.90 |

</div>
</div>
</details>

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

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>在 MP-Bench 上各系统的失败归因与归因推理性能对比。</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="在 MP-Bench 上各系统的失败归因与归因推理性能对比。">

|  | Hand-Crafted MAS |  |  |  |  | Automatic MAS |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | Failure Attribution Eval |  |  |  | Attr. Reason. Eval | Failure Attribution Eval |  |  |  | Attr. Reason. Eval |
|  | nDCG@5 (↑) |  | nDCG@full (↑) |  | LLM-Judge | nDCG@5 (↑) |  | nDCG@full (↑) |  | LLM-Judge |
| Model | Exp | Linear | Exp | Linear | Avg. Score (↑) | Exp | Linear | Exp | Linear | Avg. Score (↑) |
| Random | 0.1275 | 0.2368 | 0.1476 | 0.2228 | - | 0.3147 | 0.4153 | 0.3207 | 0.415 | - |
| GPT-4.1 | 0.4313 | 0.5386 | 0.4717 | 0.4961 | 7.63 | 0.6755 | 0.695 | 0.6761 | 0.6851 | 7.78 |
| GPT-5.1 | 0.3747 | 0.5164 | 0.4526 | 0.553 | 7.62 | 0.7844 | **0.8134** | 0.7856 | **0.8051** | **8.25** |
| O3-mini | 0.4367 | 0.5299 | 0.4487 | 0.4436 | 7.48 | 0.3944 | 0.4085 | 0.3936 | 0.3979 | 7.37 |
| Claude-Sonnet-4.5 | **0.4397** | **0.5814** | **0.506** | **0.571** | **7.95** | **0.7894** | 0.7998 | **0.792** | 0.7936 | 8.23 |
| Qwen3-8B | 0.2944 | 0.3945 | 0.338 | 0.3885 | 6.11 | 0.6681 | 0.6739 | 0.6809 | 0.6784 | 6.54 |
| GPT-oss-120B | 0.4245 | 0.5456 | 0.4852 | 0.5405 | 7.51 | 0.703 | 0.7173 | 0.7134 | 0.7166 | 7.92 |

</div>
</div>
</details>

论文还测了在多智能体测试下的结果：

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>单大模型系统与多大模型融合失败归因系统之间的性能对比</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="单大模型系统与多大模型融合失败归因系统之间的性能对比">

| **Model Combination** |  |  |  | **nDCG@5 (Exp)** |  |
| --- | --- | --- | --- | --- | --- |
| **G-4.1** | **G-5.1** | **OSS** | **Sonnet** | **Hand-Crafted** | **Automatic** |
| ✓ | ✗ | ✗ | ✗ | 0.4313 | 0.6755 |
| ✗ | ✓ | ✗ | ✗ | 0.3747 | 0.7844 |
| ✗ | ✗ | ✓ | ✗ | 0.4245 | 0.703 |
| ✗ | ✗ | ✗ | ✓ | 0.4397 | 0.7894 |
| ✓ | ✓ | ✓ | ✗ | 0.479 | 0.8 |
| ✓ | ✓ | ✗ | ✓ | 0.4909 | 0.8142 |
| ✓ | ✗ | ✓ | ✓ | **0.496** | 0.7937 |
| ✗ | ✓ | ✓ | ✓ | 0.4906 | **0.8228** |

</div>
</div>
</details>
