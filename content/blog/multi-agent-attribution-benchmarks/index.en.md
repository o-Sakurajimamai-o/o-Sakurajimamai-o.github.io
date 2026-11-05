---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-05T22:20:38+08:00
description: "Notes on the Who&When and MP-Bench multi-agent failure attribution benchmarks."
draft: false
---

## Who&When

Published in mid-2025, the paper uses relatively older models. The table shows the performance of OpenAI o1 and DeepSeek R1 on the dataset:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-reasoning-models.png" width="1464" height="244" alt="Table 1. Failure attribution performance of OpenAI o1, DeepSeek R1, and GPT-4o" loading="lazy" decoding="async">
  <figcaption>Table 1. Failure attribution performance of OpenAI o1, DeepSeek R1, and GPT-4o</figcaption>
</figure>


The dataset contains algorithm-generated and hand-crafted agent systems: 127 systems in total, with human annotations. It distinguishes two levels of error: Agent-Level (Who, identifying the responsible agent) and Step-Level (When, identifying the specific step). Step-level performance is lower.

Three evaluation methods are used: All-at-Once, Step-by-Step, and Binary Search. Their performance with GPT-4o is shown below:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-gpt4o-methods.png" width="1780" height="720" alt="Table 2. GPT-4o failure attribution performance with and without ground-truth labels" loading="lazy" decoding="async">
  <figcaption>Table 2. GPT-4o failure attribution performance with and without ground-truth labels</figcaption>
</figure>


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

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-attribution-results.png" width="1632" height="500" alt="Table 3. Failure attribution and attribution reasoning performance on MP-Bench" loading="lazy" decoding="async">
  <figcaption>Table 3. Failure attribution and attribution reasoning performance on MP-Bench</figcaption>
</figure>


The paper also evaluates a multi-agent setting:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-multi-llm-results.png" width="768" height="412" alt="Table 4. Single-LLM and multi-LLM failure attribution systems" loading="lazy" decoding="async">
  <figcaption>Table 4. Single-LLM and multi-LLM failure attribution systems</figcaption>
</figure>

## TrajErrBench

Some existing datasets have the following limitations:

- **Trajectories tend to be short:** The algorithm-generated subset of Who&When averages only **8.72 steps**, making it difficult to test the loss of evidence over long contexts.
- **Scenarios can be narrow or toy-like:** Many focus on simple multi-agent conversations, web shopping (WebShop), or text-based room games (ALFWorld).
- **A lack of realistic complex settings:** They offer limited coverage of **complex multi-turn tool use** with customer-service business rules and **very long-horizon repository-level software development**.

TrajErrBench consists of two complementary, challenging real-world subsets:

**Table 5. Composition of TrajErrBench**

<div class="survey-table" tabindex="0" role="region" aria-label="Table 5. Composition of TrajErrBench">

| Subset | Trajectories | Average steps | Task setting and characteristics | LLMs generating failed trajectories |
| --- | --- | --- | --- | --- |
| $\tau^{2}$-Bench subset | 400 | 29.27 | Multi-turn customer service and tool use in airline and retail settings: the agent gathers requirements through dialogue, calls backend APIs, and follows business policies such as change and refund rules, budgets, and seat-class restrictions. | DeepSeek-Reasoner, GPT-5, Qwen3-Max |
| SWE-Bench Pro subset | 86 | 119.70 | Long-horizon repository-level software engineering: the agent locates bugs, reads source files, writes patches, and runs tests in real GitHub repositories. | OpenHands with Claude-Opus-4.6, Kimi-K2.6-Coding, Gemini-3.1-Pro, Grok-4.20-Beta, GPT-5.4 |
| Total | 486 | — | Real tool interaction and long-horizon code repair | 8 frontier models |

</div>

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/trajerrbench-critical-error-results.png" width="1847" height="808" alt="Table 6. Main critical-error detection results across multiple benchmarks" loading="lazy" decoding="async">
  <figcaption>Table 6. Main critical-error detection results across multiple benchmarks</figcaption>
</figure>

The authors observed the following in this dataset:

1. Critical errors often occur in the latter half of a trajectory. In previous datasets, agents often make decisive errors within the first few steps. In this dataset, agents spend much of the earlier trajectory analyzing and interpreting before they make a critical error while making key decisions or modifying code.
2. Agents often fail during reasoning: this accounts for **60.7%** of cases in $\tau^{2}$-Bench and **57.1%** in SWE-Bench Pro.
