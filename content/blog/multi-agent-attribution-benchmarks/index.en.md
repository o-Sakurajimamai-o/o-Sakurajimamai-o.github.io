---
title: "Multi-Agent Failure Attribution Benchmark Notes"
date: 2026-11-04T21:57:38+08:00
lastmod: 2026-11-04T23:08:16+08:00
description: "Notes on the Who&When and MP-Bench multi-agent failure attribution benchmarks."
draft: false
---

## Who&When

Published in mid-2025, the paper uses relatively older models. The table shows the performance of OpenAI o1 and DeepSeek R1 on the dataset:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-reasoning-models.png" width="1464" height="244" alt="OpenAI o1 and DeepSeek R1" loading="lazy" decoding="async">
  <figcaption>OpenAI o1 and DeepSeek R1</figcaption>
</figure>


The dataset contains algorithm-generated and hand-crafted agent systems: 127 systems in total, with human annotations. It distinguishes two levels of error: Agent-Level (Who, identifying the responsible agent) and Step-Level (When, identifying the specific step). Step-level performance is lower.

Three evaluation methods are used: All-at-Once, Step-by-Step, and Binary Search. Their performance with GPT-4o is shown below:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/who-when-gpt4o-methods.png" width="1780" height="720" alt="GPT-4o" loading="lazy" decoding="async">
  <figcaption>GPT-4o</figcaption>
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
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-attribution-results.png" width="1632" height="500" alt="Failure attribution and attribution reasoning performance on MP-Bench" loading="lazy" decoding="async">
  <figcaption>Failure attribution and attribution reasoning performance on MP-Bench</figcaption>
</figure>


The paper also evaluates a multi-agent setting:

<figure class="fig">
  <img src="/blog/multi-agent-attribution-benchmarks/figures/mp-bench-multi-llm-results.png" width="768" height="412" alt="Single-LLM and multi-LLM failure attribution systems" loading="lazy" decoding="async">
  <figcaption>Single-LLM and multi-LLM failure attribution systems</figcaption>
</figure>
