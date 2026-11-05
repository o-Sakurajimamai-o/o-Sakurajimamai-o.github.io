---
title: "TrajDebug: Tracing Error Lifecycles"
date: 2026-11-05T23:32:07+08:00
lastmod: 2026-11-05T23:32:07+08:00
description: "Notes on TrajDebug, critical error localization, error lifecycle tracing, and feedback."
draft: false
---

## Core Idea

The paper's central idea is to find the **critical error step** $t^\star$ in a failed trajectory $\tau = (x_1, x_2, \dots, x_N)$. The authors identify $t^\star$ as the **earliest error step directly and causally responsible for the final task failure**: if that step had been corrected, the later cascade would not have occurred and the task would have succeeded.

For an evaluation such as Who&When that directly gives an LLM a failed trajectory, long context, numerous local errors, and cascading mistakes often lead to poor performance.

<figure class="fig">
  <img src="/blog/trajdebug-notes/figures/critical-error-cascade.png" width="842" height="588" alt="Figure 1. Example of a critical error and its cascading effects." loading="lazy" decoding="async">
  <figcaption>Figure 1. Example of a critical error and its cascading effects.</figcaption>
</figure>

## Method

<figure class="fig">
  <img src="/blog/trajdebug-notes/figures/trajdebug-framework.png" width="1653" height="822" alt="Figure 2. Overview of TrajDebug’s error-lifecycle tracing framework." loading="lazy" decoding="async">
  <figcaption>Figure 2. Overview of TrajDebug’s error-lifecycle tracing framework.</figcaption>
</figure>

### Multi-Granularity History Compression

For each step in a trajectory, TrajDebug has an LLM create views at three levels of detail:

**Table 1. Content and character limits of multi-granularity history views.**

<div class="survey-table" tabindex="0" role="region" aria-label="Table 1. Content and character limits of multi-granularity history views.">

| View | Code and character limit | Retained content and purpose |
| --- | --- | --- |
| **High-detail view** | `th1` $\le 3000$ characters per field | **Nearly verbatim:** Retains original instructions, actions, environment observations, and local reasoning. Removes obvious filler, repeated phrasing, and decorative formatting; used for detailed evidence checks at the current and immediately adjacent steps. |
| **Medium-detail view** | `th2` $\le 1200$ characters per field | **Backbone summary:** Merges related information and omits secondary elaboration while retaining the step's main intent, specific action, state updates, errors, and key observations; used for mid-range history. |
| **Low-detail view** | `th3` $\le 600$ characters per field | **Minimal summary:** Records rough progress, key entities, and unresolved commitments or hard constraints in as few words as possible; used for distant history. |

</div>

The compression process has two constraints:

1. Preserve the user's constraints and plans at all three granularities, such as prohibitions and subgoal plans.
2. Preserve critical tokens verbatim rather than compressing or simplifying them, including specific item or order IDs and tool function names.

### Error Trigger Detection

When the LLM checks step $t$, it receives context according to the following rules, which can reduce context length:

1. Use `th1`, the original content, for $Step_t$.
2. Use `th2` for $Step_{t-x} \to Step_{t-1}$.
3. Use `th3` for earlier history.

The LLM outputs each detected error as:

<div style="max-width:100%;overflow-x:auto">

$$
e = (t, c, p, q_w, q_r)
$$

</div>

Here, $t$ is the step; $q_w$ is the original erroneous commitment, which may be text or an action; $q_r$ is the original reference text containing the rule or fact violated by $q_w$; $c$ is the reference category; and $p$ is the execution phase in which the agent made the error.

**There are four reference categories $c$:**

1. Task conflict: $q_r$ comes from an earlier task instruction or system constraint.
2. History conflict: $q_r$ comes from earlier environment feedback, tool output, or established facts.
3. Intra-step conflict: $q_r$ comes from within the current step $t$.
4. Environment anomaly: usually caused by external factors such as a lost connection or a 404 page.

**There are five execution phases $p$:**

1. **Planning:** Omitting subtasks, ordering them incorrectly, or drifting from the objective.
2. **Reasoning:** Distorting evidence and choosing incorrectly, overgeneralizing, or inventing unchecked premises.
3. **Action:** Choosing the wrong tool or acting inconsistently with stated intentions, using malformed arguments, or taking prohibited actions.
4. **Observation:** Misreading tool output or ignoring a key result.
5. **Verification:** Finishing without a required check, stopping before the task is complete, or retrying blindly in a loop.

### Error State Classification

After obtaining the error set, the triggers still need clustering: the same error can be recorded multiple times, and already repaired errors can also be recorded. Further clustering removes duplicates.

TrajDebug groups a set of triggers $\mathcal{E}$ around the same violated reference object $O$ into an error instance $E = (\mathcal{E}, O)$. The reference category $c$ and the specific violated reference $q_r$ jointly determine $O$.

For each error instance $E$, the state classifier makes two classifications grounded in the original text:

1. An issue is resolved only when a later step provides citable original evidence that the erroneous commitment involving the same reference object $O$ has been corrected. Unresolved errors remain Active.
2. A **terminal footprint** means that an erroneous action has caused an irreversible change to the external environment; an incorrect belief or fact has influenced subsequent actions without correction; or correcting an error consumed more than half the cost or more than half the trajectory length.

These two dimensions yield four states:

**Table 2. Error-instance lifecycle states and candidate-set selection.**

<div class="survey-table" tabindex="0" role="region" aria-label="Table 2. Error-instance lifecycle states and candidate-set selection.">

| Error lifecycle state | Dimension 1: resolved? | Dimension 2: terminal footprint? | Retained in candidate set $\mathcal{F}(\tau)$? |
| --- | --- | --- | --- |
| **1. Clean resolution** | **Resolved** | **No footprint** | × |
| **2. Latent active** | **Unresolved** | **No footprint** | × |
| **3. Costly resolution** | **Resolved** | **Footprint present** | √ |
| **4. Manifest active** | **Unresolved** | **Footprint present** | √ |

</div>

We have:

<div style="max-width:100%;overflow-x:auto">

$$
\mathcal{F}(\tau) = \{ E : \operatorname{state}(E) \in \{\operatorname{CostlyResolution}, \operatorname{ManifestActive}\} \}
$$

</div>

### Candidate-Set-Guided Causal Attribution

For $\mathcal{F}(\tau)$, the earliest candidate should not simply be selected: the earliest error is not necessarily the most critical one. TrajDebug uses an LLM as the final causal attribution judge:

1. Provide the candidate set $\mathcal{F}(\tau)$, including $e = (t, c, p, q_w, q_r)$.
2. Let the LLM reason about which candidate step, if corrected, would fundamentally eliminate the subsequent cascade.
3. To guard against reasoning errors or missed steps, allow the LLM to override $\mathcal{F}(\tau)$ and choose another trajectory step, but impose a **very strict evidence threshold**. It must first explain why none of the candidates accounts for the final failure and must provide $e = (t, c, p, q_w, q_r)$, as in the first two stages. This strictly evidence-constrained fallback still scores four percentage points higher than direct prompting.

## Feedback

A critical-error detector is useful only when its output translates into measurable agent improvement. The paper uses TrajDebug as a feedback source and injects its feedback into the executing agent's system prompt. It compares two baselines, Vanilla and Self-Reflection, with the following results:

<figure class="fig">
  <img src="/blog/trajdebug-notes/figures/feedback-results.png" width="906" height="478" alt="Table 3. Task success rates when detector outputs are used as feedback for GLM-5.1." loading="lazy" decoding="async">
  <figcaption>Table 3. Task success rates when detector outputs are used as feedback for GLM-5.1.</figcaption>
</figure>

The results improve. The reflection used here is: `For each failed trajectory, the detector generates feedback and injects it into the system prompt`. These results exceed those of global prompting or general reflection.

## Summary

1. The cost is high because an LLM compresses every step into three levels. Perhaps a smaller model or low-detail views of chunks could locate the relevant area more quickly?
2. Without replay in the third step, only 42.0% of truly critical error steps survive into the final candidate set $\mathcal{F}(\tau)$. The explicit $q_w$ and $q_r$ requirements are demanding, making implicit errors harder to attribute effectively.
3. There is no real-time interception (later methods address this...
4. One could freeze earlier branches and replay from $\mathcal{F}(\tau)$, instead of rerunning everything through the system prompt.
