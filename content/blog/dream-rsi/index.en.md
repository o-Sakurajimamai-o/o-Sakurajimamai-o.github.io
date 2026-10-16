---
title: "Dream-RSI: Reading Notes"
date: 2026-10-16T00:00:00+08:00
description: "Notes on Dream-RSI: its motivation, online exploration and offline replay, and a few thoughts on its limitations."
draft: false
---

<style>
.prose mjx-container[display="true"] { max-width: 100%; overflow-x: auto; overflow-y: hidden; padding: .4em 0; }
</style>

## Pain Points

1. Evaluating a candidate program usually only requires running it and checking the result. Evaluating an exploration policy requires letting it schedule many successive attempts, then observing the best solution found and the resources consumed. A direction with a low initial score may produce a breakthrough after several repairs; an early leader may soon stagnate.
2. Feedback on policy quality is more delayed than feedback on individual programs. Rerunning the entire online discovery process after every controller change repeatedly incurs costs for model calls, code execution, and evaluation. Meanwhile, the space of exploration policies is large. Branch selection, stopping rules, parallel batches, and failure handling can all be combined in different ways. These are the two bottlenecks identified in the introduction: **expensive, delayed feedback and many candidate meta-policies**.
3. A fixed exploration policy is simple and stable, but cannot change its search habits across rounds based on accumulated discovery experience. Dream-RSI aims to reduce the cost of comparing exploration policies, rather than directly designing a stronger fixed heuristic for a particular domain.

## How Dream-RSI Works

0. Dream-RSI consists of two processes: online exploration and offline planning.
1. Online exploration: first, an initial workspace is set up as the root node $r$. Each node records its parent, preceding context, and score. The scheduler selects either a leaf node to continue its work or the root to start a new approach. A coding agent, or an agent dedicated to the task, carries out the continuation. The resulting node saves its context and score and becomes a new leaf. Exploration stops when the maximum number of iterations is reached or the scheduling policy decides to stop.
2. Offline planning: step 1 produces the discovery tree $\mathcal T$ for the current round. This tree consists of several chains because the online phase only extends leaf nodes. New branches cannot start from internal nodes, except for the root. We continue with the current policy $\pi_t$, but the tree observed during offline replay is only partially revealed. Starting again from the root, the policy selects the workspace for the next step. Suppose A1 is the earliest child created from the root; replay can proceed as follows:

| Round | Starting points selected by the policy | Simulator returns |
| --- | --- | --- |
| Round 1 | Root $r$ | Saved node A1 |
| Round 2 | Root $r$ | Saved node B1 |
| Round 3 | A1, B1 | Saved nodes A2, B2 |
| Next | Return an empty batch | Stop without revealing A3 |

This policy creates no new experiments; it only produces a new scheduling trajectory:

- **Select a non-root node**: return its existing next node.
- **Select the root**: open the next unrevealed branch in creation order.
- **No recorded successor exists**: return no new node for that selection.

At the end of a rollout, the trajectory is evaluated using:

$$
V=\underbrace{\max_{v\in\mathcal T_{\mathrm{revealed}}}s_v}_{\text{best discovery quality}}
-\underbrace{\beta_1N}_{\text{total attempt penalty}}
+\underbrace{\beta_2\frac{N}{\max\{1,k^\star\}}}_{\text{average batch size reward}}.
$$

Here, $N$ is the number of revealed non-root nodes, representing the generation–evaluation requests associated with this trajectory. Replay itself does not execute these requests again. The first term uses the best score found, rather than averaging all scores. The second term encourages fewer attempts. The third favors scheduling work within fewer decision rounds, serving as a proxy reward for parallelism.

3. To avoid overfitting to history and increase diversity, each RSI round evaluates the policy on all $t$ historical trees and takes the average:

$$
V^m=\frac{1}{t}\sum_{i=1}^{t}V_i^m.
$$

4. After each complete offline evaluation, the decision-making agent uses $V$ to adjust the policy for the next round.

## Limitations of RSI

1. Different tasks require different evaluation methods, which must be designed for the dataset or task setting.
2. Only leaf nodes or the root can be selected, so the tree consists of several chains. **Opening new child branches from an earlier parent node could be considered, but the details remain open for discussion.**
3. The paper does not mention an explicit rule to stop a branch once its current task is complete. If the scheduler keeps extending an already completed leaf, this could produce redundant attempts and consequently reduce the weight assigned to that branch.

Paper: [Dream-RSI: Recursive Self-Improvement through Evolving Worlds](https://arxiv.org/abs/2609.14858v1).
