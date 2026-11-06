---
title: "DoVer: Intervention-Driven Multi-Agent Debugging"
date: 2026-11-06T18:45:50+08:00
lastmod: 2026-11-06T18:45:50+08:00
description: "Reading notes on DoVer, multi-agent failure attribution, intervention, and validation."
draft: false
---

## The Problem

The evaluation approach used by the Who&When (WW) dataset has limitations. Its setup is:

1. **Input:** A complete failed session log, from the user's question through rounds of interaction among agents to an incorrect final answer or a stopping rule such as the maximum number of replanning rounds.
2. **Task:** Ask an LLM to read the log and identify the earliest, uniquely responsible agent and step that caused the task to fail.
3. **Decisive error step:** A step whose incorrect action could be replaced by a correct one so that the subsequent trajectory succeeds.

The authors found that WW trajectories did not explicitly show step numbers, so the LLM had to track or infer each step. The evaluation criteria given to the LLM also differed from those used in expert annotations. The authors embedded a concise version of the human annotator guidance in the LLM prompt: `Find the earliest decisive step that diverted the task and whose correction could lead to success`.

After these two changes, GPT-4o's step attribution accuracy on the WW hand-crafted set with ground truth rose from 6.04% to 23.56%.

WW's attribution perspective is also narrow. Choosing a single absolute error from multiple errors is inherently uncertain: an LLM's answer may differ from an expert's while both identify plausible critical errors. Responsibility between agents can also be ambiguous. A subagent's error may arise from the subagent itself or from an inadequate task description by the assigning agent; attributing it to only one agent is therefore incomplete.

---

## DoVer

The authors propose DoVer in response. Its central framework can be summarized in three parts:

1. Set aside human-annotated ground truth, since failures can be viewed from multiple perspectives, as in MP-Bench. DoVer returns to the definition of a decisive error: directly modify the decision at step $t$ and see whether the subsequent task succeeds. If it does, that step is treated as decisive.
2. A long session can contain several ReAct loops, and multiple trials may fail because an LLM may initially explore by trial and error. DoVer separates each trial and looks for failure points within it, shortening context and enabling repair along different failed paths.

<figure class="fig">
  <img src="/blog/dover-notes/figures/failure-trials.png" width="1584" height="800" alt="Figure 1. Failure trace of Case 3 in WW-HC, illustrating ambiguity in failure attribution. The session consists of four distinct trials, each initiated by a plan update and executed via a ReAct-style Yao et al. (2023) loop. Different strategies (e.g., direct scrolling in Trial 1 vs. calendar navigation in Trial 2) yield separate error points, making single-step attribution across the session inherently ambiguous. Trial 2 (Steps 53–55) further shows inter-agent misalignment: the Orchestrator issued an invalid instruction, while the WebSurfer compounded the error by executing an unrelated action." loading="lazy" decoding="async">
  <figcaption>Figure 1. Failure trace of Case 3 in WW-HC, illustrating ambiguity in failure attribution. The session consists of four distinct trials, each initiated by a plan update and executed via a ReAct-style Yao et al. (2023) loop. Different strategies (e.g., direct scrolling in Trial 1 vs. calendar navigation in Trial 2) yield separate error points, making single-step attribution across the session inherently ambiguous. Trial 2 (Steps 53–55) further shows inter-agent misalignment: the Orchestrator issued an invalid instruction, while the WebSurfer compounded the error by executing an unrelated action.</figcaption>
</figure>

3. Responsibility between an orchestrator and subagents is often entangled. DoVer distinguishes two levels of intervention:
   1. Directly edit the orchestrator's plan or the prompt sent to a subagent.
   2. If a subagent cannot complete a task because it lacks a tool, create a new tool to help. This directly modifies the harness and can connect to RSI.

<figure class="fig">
  <img src="/blog/dover-notes/figures/dover-pipeline.png" width="1832" height="576" alt="Figure 2. DoVer (Do–then–Verify) Debugging Pipeline. (1) Trial segmentation: split the failed session log into trials using re-plan steps as cut points. (2) Failure attribution: for each trial, propose a hypothesis $h_{i}$ that marks a faulty step or agent. (3) Intervention generation: turn $h_{i}$ into an actionable intervention that edits either the plan or the attributed message or step in the original log. (4) Intervention execution: replay the trajectory in place, i.e., preserve all steps before the intervened step, then execute the intervention and measure progress of the new log. Colors indicate plan/re-plan (blue), execution (green), attributed failure (red), terminal failure (dark red), terminal success (dark green), intervention (yellow), new plan/re-plan (blue hatch), and new execution (green hatch)." loading="lazy" decoding="async">
  <figcaption>Figure 2. DoVer (Do–then–Verify) Debugging Pipeline. (1) Trial segmentation: split the failed session log into trials using re-plan steps as cut points. (2) Failure attribution: for each trial, propose a hypothesis $h_{i}$ that marks a faulty step or agent. (3) Intervention generation: turn $h_{i}$ into an actionable intervention that edits either the plan or the attributed message or step in the original log. (4) Intervention execution: replay the trajectory in place, i.e., preserve all steps before the intervened step, then execute the intervention and measure progress of the new log. Colors indicate plan/re-plan (blue), execution (green), attributed failure (red), terminal failure (dark red), terminal success (dark green), intervention (yellow), new plan/re-plan (blue hatch), and new execution (green hatch).</figcaption>
</figure>

DoVer has four steps:

1. **Trial segmentation:** Split the long failed trajectory $\tau$ into separate trials at self-reflection and replanning steps, shown as blue blocks in Figure 2. This shortens context, supports parallel interventions, and allows multiple repair paths.
2. **Failure attribution:** For a trial $\tau^i$, use an LLM and prompt to generate a candidate hypothesis $h_i = (\hat{a}_{\hat{t}}^i, r_{\hat{t}}^i)$, where $\hat{a}^i$ is the suspected agent and $r_{\hat{t}}^i$ is a natural-language error analysis.
3. **Intervention generation:** Turn $h_i$ into a concrete, minimal text edit $I_i$ for the context at step $\hat{t}$, using one of the two intervention types above.
4. **Intervention execution:** Restore the system snapshot $\sigma_{\hat{t}}$ from before step $\hat{t}$, replace the original erroneous message $m_{\hat{t}}$ with the intervention $I_i$, and continue execution.

---

## Metrics

1. Measure the proportion of intervened trial runs that successfully solve the task. For runs that do not succeed, also assess whether the intervention made progress. On datasets with human-annotated solution steps, such as GAIA, an LLM first extracts at most $K \le 5$ tool-agnostic milestones $\{\mathbf{m}^1, \mathbf{m}^2, \dots, \mathbf{m}^K\}$ from the human solution. An LLM judge then counts how many milestones a trajectory $\gamma$ achieves:

<div style="max-width:100%;overflow-x:auto">

$$
A(\gamma) = \sum_{k=1}^K \mathbb{I}\left[\text{trajectory }\gamma\text{ reaches milestone }\mathbf{m}^k\right]
$$

</div>

Compare the new trajectory after intervention, $\tilde{\tau}_I$, with the original failed trajectory $\tau$:

<div style="max-width:100%;overflow-x:auto">

$$
\text{Prog}(\tau \to \tilde{\tau}_I) = \frac{A(\tilde{\tau}_I) - A(\tau)}{K} \in [-1, 1]
$$

</div>

2. To check whether the LLM actually follows $I_i$, the paper also uses an LLM-as-Judge and distinguishes four validation outcomes.

**Table 1. Validation outcomes for failure hypotheses after intervention.**

<div class="survey-table" tabindex="0" role="region" aria-label="Table 1. Validation outcomes for failure hypotheses after intervention.">

| Validation outcome | Criteria, based on three independent reruns |
| --- | --- |
| **1. Validated** | At least **two of three reruns fully succeed** at the task. |
| **2. Partially validated** | Fewer than 2 successful runs, but at least **two runs** both faithfully follow the intervention and achieve milestone progress of $\ge +20\%$. |
| **3. Refuted** | Fewer than 2 successful runs, and at least **two runs** both faithfully follow the intervention and achieve milestone progress of $\le +20\%$. |
| **4. Inconclusive** | Any other case that does not satisfy the first three categories. |

</div>

## Experiments

Distribution of cases in the paper's datasets:

<figure class="fig">
  <img src="/blog/dover-notes/figures/dataset-cases.png" width="1552" height="228" alt="Table 2. Summary of failed and intervened cases across datasets, showing the total number of cases, failed cases, intervened cases, total intervened trials, and the average number of trials per case." loading="lazy" decoding="async">
  <figcaption>Table 2. Summary of failed and intervened cases across datasets, showing the total number of cases, failed cases, intervened cases, total intervened trials, and the average number of trials per case.</figcaption>
</figure>

Failure-flipping performance:

<figure class="fig">
  <img src="/blog/dover-notes/figures/failure-flipping.png" width="1204" height="224" alt="Table 3. Experimental results on failure-flipping metrics across settings. The table reports the number of Intervened Trials, the Trial Success Rate, and the average Progress Made." loading="lazy" decoding="async">
  <figcaption>Table 3. Experimental results on failure-flipping metrics across settings. The table reports the number of Intervened Trials, the Trial Success Rate, and the average Progress Made.</figcaption>
</figure>

On WW-AB, which contains many difficult tasks, progress is +0%. On extremely difficult tasks, inappropriate interventions may even interfere with existing progress, so negative progress can offset positive progress.

Validation outcome distribution for failure hypotheses:

<figure class="fig">
  <img src="/blog/dover-notes/figures/hypothesis-validation.png" width="1344" height="224" alt="Table 4. Validation outcomes of failure hypotheses across datasets. The table reports the number and percentage of trials classified as Validated, Inconclusive, Partially Validated, or Refuted." loading="lazy" decoding="async">
  <figcaption>Table 4. Validation outcomes of failure hypotheses across datasets. The table reports the number and percentage of trials classified as Validated, Inconclusive, Partially Validated, or Refuted.</figcaption>
</figure>

On difficult datasets, the framework alone cannot effectively improve performance.

---

## Self-Improvement

These are two future directions the authors anticipate. In the paper, the authors use human collaboration to optimize and add tools, improving agent performance on difficult tasks. I would expect: (i) a fully automated debugging loop, or RSI, where DoVer's insights feed into automated subagent improvement, for example by using a coding agent to implement repairs; and (ii) capability-aware intervention generation that tailors interventions to known subagent capability boundaries when code changes are not allowed. Both directions can help build robust, self-improving multi-agent systems.
