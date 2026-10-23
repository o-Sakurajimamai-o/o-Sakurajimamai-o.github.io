---
title: "Multi-Agent Systems Survey Notes"
date: 2026-10-23T00:00:00+08:00
lastmod: 2026-10-23T16:37:38+08:00
description: "Reading notes on Beyond Individual Intelligence, covering multi-agent system challenges, individual reasoning enhancement, and memory methods."
draft: false
---

Recent LLM capabilities have grown linearly or exponentially, sometimes exceeding even AI developers' expectations. OpenAI reported that an internal model coordinated around 10,000 concurrent agents to produce [a solution to the Navier–Stokes Millennium Prize problem](https://openai.com/index/navier-stokes-solution/). Anthropic researchers also described [genetics research conducted with Claude and domain experts](https://www.anthropic.com/research/claude-shaped-science). Google's Gemini 4 Argon helped researchers [extend mathematical solutions related to magnetic confinement in fusion](https://www.techradar.com/pro/its-over-ai-won-google-gemini-4-argon-shatters-59-year-old-nuclear-fusion-conjecture-in-just-a-few-hours) through multi-agent collaboration. This work relates to Grad's 59-year-old conjecture.

As task scale and execution time increase, it becomes harder to concentrate every required capability in a single agent. Multi-agent collaboration offers a principled way to address this challenge.

## Problems in Multi-Agent Systems

Individual models still make local errors, including factual hallucinations, miscommunicated messages, and incorrect tool calls. These errors propagate through later interaction rounds and become difficult to detect and locate as the number of steps increases. Failure attribution and self-correction weaknesses become deeply intertwined, preventing agents from improving.

This reveals a fundamental asymmetry. A complex multi-agent system's capabilities far exceed the mechanisms available to understand its failures and enable autonomous improvement (RSI).

Bridging this gap requires a closed-loop framework that identifies failures, reduces budget requirements, and guides structural self-improvement.

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/life-overview.png" width="1717" height="1228" alt="Figure 1. An operational lifecycle overview of multi-agent systems, covering individual capabilities, collaboration, failure attribution, system self-evolution, and representative work." loading="lazy" decoding="async">
  <figcaption>Figure 1. An operational lifecycle overview of multi-agent systems, covering individual capabilities, collaboration, failure attribution, system self-evolution, and representative work.</figcaption>
</figure>

## Individual Intelligence

LLMs remain limited in complex settings such as automated software engineering, scientific experiment design, and multistep web navigation. They lack persistent memory and active perception of external environments, remaining constrained by the static knowledge acquired during training.

A harness gives an LLM capabilities such as environmental perception, goal formulation, and tool execution: Agent = Harness + LLM.

<figure class="fig">
  <img src="/blog/multi-agent-survey-notes/figures/individual-capabilities.png" width="1822" height="1131" alt="Figure 2. Individual agent capabilities organized along the reasoning pipeline. Memory supplies retrieved context and incorporates execution feedback, alongside planning and tool use." loading="lazy" decoding="async">
  <figcaption>Figure 2. Individual agent capabilities organized along the reasoning pipeline. Memory supplies retrieved context and incorporates execution feedback, alongside planning and tool use.</figcaption>
</figure>

### Reasoning

Chain-of-thought (CoT) prompting elicits intermediate reasoning steps, substantially improving performance on complex tasks without changing model parameters. The central challenge then becomes how external mechanisms bridge static parametric knowledge and the dynamic demands of real-world tasks.

From this perspective, existing methods can be organized by information flow through the reasoning pipeline into three stages:

1. *Input-stage enhancement*: expanding the available information through external knowledge retrieval and multimodal perception.
2. *Reasoning-process enhancement*: improving reasoning trajectories through search-space expansion and path verification.
3. *Output-stage regulation*: improving reliability and factual accuracy through post-hoc evaluation and adaptive correction.

#### Query Enhancement

Retrieval-augmented generation (RAG) combines a parametric generator with a nonparametric retriever, allowing the model to obtain relevant external documents during inference. Subsequent approaches include:

1. Training models to decide adaptively how to retrieve, and using reflection tokens to assess retrieved and generated content.
2. Retrieving relevant subgraphs from knowledge graphs and performing multi-hop reasoning over them.
3. Knowledge Card designs small models as pluggable knowledge modules for different domains. They generate background material, which a large model retrieves to answer the query.
4. RuAG uses an LLM to help define candidate conditions and prediction targets. Monte Carlo tree search then mines reliable logical rules from offline data. These rules become natural-language instructions, injected directly or retrieved on demand to guide reasoning and generation.

#### Reasoning Enhancement

Methods operating on reasoning trajectories can be organized along two dimensions. Search-space expansion increases candidate coverage, while path verification provides the selection pressure needed to identify reliable outcomes.

**Search-space expansion**

CoT, Tree of Thoughts (ToT), and Graph of Thoughts (GoT) depend on manually designed prompting protocols. DeepSeek-R1 uses large-scale reinforcement learning (RL) to learn long reasoning chains autonomously. Reward optimization alone gives rise to self-verification, spontaneous backtracking, and self-critique.

**Reasoning-path verification**

Expanding the search space produces more candidates, but also introduces erroneous paths. Current approaches include internal voting over multiple sampled outputs and iterative refinement that retains failures, without changing model parameters.

Process reward models (PRMs) independently score reasoning steps, substantially improving reasoning capability. Subsequent work integrates PRMs into RL training. This shows that process supervision is most effective when it shapes both the learned policy and the selection mechanism.

#### Output Verification

Reliability assessment of generated outputs falls into two categories. The first decomposes outputs into individual claims and checks them against a knowledge base. This can extend to finer-grained error localization in code, mathematical reasoning, and scientific literature.

The second detects hallucinations without external knowledge by measuring consistency across samples. Models may also verify their own outputs, answer independently, or select a winner by voting over multiple samples before further iteration.

**Table 1.** Comparison of reasoning enhancement methods, organized by intervention stage and subtype.

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Editable table transcription</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

Subtypes: KE = knowledge enhancement; PE = perceptual enhancement; ESS = expanding search space; VRP = verification of reasoning paths; OV = output verification; RC = response correction. Task scope: Ma = mathematics; QA = question answering; Co = code; LF = long-form generation; Ag = agent; Mu = multitask. Primary source: Evid = external evidence; Inter = internal states; Consis = cross-sample consistency; Log = logic/verification; Tool = tool-based. Correction: SF = self-feedback; Refl = reflection; TV = tool verification; SC = self-consistency. Ext. Ret. indicates external retrieval; TF means training-free; TTC indicates test-time compute scaling.

<div class="survey-table" tabindex="0" role="region" aria-label="Table 1">

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

### Memory

Memory concepts were covered in [an earlier post](/blog/graph-based-agent-memory/). Here, only a comparison of memory enhancement methods organized along the memory lifecycle is provided.

**Table 2.** Comparison of memory enhancement methods along the memory lifecycle.

<details class="survey-details">
<summary>
  <svg width="24" height="24" class="survey-details__icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" aria-hidden="true"><path d="M8 6h13M8 12h13M8 18h13M3 6h.01M3 12h.01M3 18h.01"/></svg>
  <span>Editable table transcription</span>
  <svg width="24" height="24" class="survey-details__chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="m9 5 7 7-7 7"/></svg>
</summary>
<div class="survey-details__body">

Formation describes how raw experience becomes memory: Record = raw logging; Distill = abstractive summarization; Routing = typed classification. Storage describes the underlying structure: Flat = flat storage; Tiered = main context plus archival memory; Graph = graph structure; Tree = tree structure; Typed = separate type-specific stores; Unified = unified storage across memory types.

Maintenance describes updates and removal: Decay = time-based forgetting; Eviction = budget-constrained removal; CRUD = create/read/update/delete; Evolution = retrospective refinement; Versioning = bi-temporal tracking; Lifecycle = unified management; Learned = RL-based selection.

Retrieval describes how relevant entries are recalled: Multi-factor = recency + importance + relevance; Injection = full-buffer injection; Autonomous = self-directed function calls; Graph/Tree trav. = traversal of the corresponding structure. Type-aware = per-type top-k retrieval followed by merging; Hybrid = semantic + keyword + graph; Routed = multiple retrieval routes; Utility-driven = similarity + Q-value reranking.

Memory type symbols are ordered E (episodic), S (semantic), P (procedural): filled circles indicate support, and hollow circles indicate absence. Task domains: Conv = conversation; Ent = enterprise.

<div class="survey-table" tabindex="0" role="region" aria-label="Table 2">

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
