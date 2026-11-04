---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-04T21:57:38+08:00
description: "Notes on the Who&When and MP-Bench multi-agent failure attribution benchmarks."
draft: false
---

## Who&When

Published in mid-2025, the paper uses relatively older models. The table shows the performance of OpenAI o1 and DeepSeek R1 on the dataset:

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>OpenAI o1 and DeepSeek R1</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="OpenAI o1 and DeepSeek R1">

|  | **GPT-4o** |  | **OpenAI o1** |  | **DeepSeek R1** |  |
| --- | --- | --- | --- | --- | --- | --- |
| **Accuracy** | Agent-Level | Step-Level | Agent-Level | Step-Level | Agent-Level | Step-Level |
| **All-at-Once** | 54.31 | 4.39 | 41.38 | **10.34** | **56.90** | 3.45 |
| **Step-by-Step** | 33.62 | 7.90 | **36.21** | **13.79** | 32.76 | 6.90 |

</div>
</div>
</details>


The dataset contains algorithm-generated and hand-crafted agent systems: 127 systems in total, with human annotations. It distinguishes two levels of error: Agent-Level (Who, identifying the responsible agent) and Step-Level (When, identifying the specific step). Step-level performance is lower.

Three evaluation methods are used: All-at-Once, Step-by-Step, and Binary Search. Their performance with GPT-4o is shown below:

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


Dataset example:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-example.png" width="1424" height="856" alt="Figure 1. Examples of failure logs from algorithm-generated and hand-crafted systems in Who&When." loading="lazy" decoding="async">
  <figcaption>Figure 1. Examples of failure logs from algorithm-generated and hand-crafted systems in Who&When.</figcaption>
</figure>

## MP-Bench

Different analysts may reasonably attribute a failure to different steps. This non-deterministic, multi-perspective nature arises from the interdependencies inherent in multi-agent systems. Multiple specialized agents can be combined interdependently into diverse, valid execution trajectories aimed at the same goal. Failure attribution therefore depends heavily on an observer's internal reasoning about what constitutes a “correct” trajectory.

The limitations observed in previous work stem from biased evaluation assumptions that overlook the multi-perspective nature of MAS failures, rather than from insufficient model capabilities.

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-perspectives.png" width="1640" height="724" alt="Figure 2. An example illustrating the multi-perspective nature of failure attribution in multi-agent systems." loading="lazy" decoding="async">
  <figcaption>Figure 2. An example illustrating the multi-perspective nature of failure attribution in multi-agent systems.</figcaption>
</figure>

MP-Bench uses normalized discounted cumulative gain (nDCG@K), which evaluates agreement between the predicted and ground-truth rankings and gives more weight to correctly prioritizing highly salient failure steps.

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Failure attribution and attribution reasoning performance on MP-Bench</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="Failure attribution and attribution reasoning performance on MP-Bench">

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


The paper also evaluates a multi-agent setting:

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Single-LLM and multi-LLM failure attribution systems</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

<div class="survey-table" tabindex="0" role="region" aria-label="Single-LLM and multi-LLM failure attribution systems">

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
