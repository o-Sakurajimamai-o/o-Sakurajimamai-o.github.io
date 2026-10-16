---
title: "Dream-RSI: Reading Notes"
date: 2026-10-16T00:00:00+08:00
lastmod: 2026-10-16T21:26:20+08:00
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

### 0. Dream-RSI

Dream-RSI consists of online exploration and offline planning, alternating through the following cycle:

> Online exploration → save the discovery tree → evaluate and modify policies offline → select a policy for deployment → another round of online exploration.

### 1. Online Exploration

First, an initial workspace is set up as the root node $r$. Each non-root node records its parent, preceding context, workspace snapshot, evaluation diagnostics, and score. Let $t$ index the outer online–offline loop, and let $\pi_t$ denote the executable scheduling policy code used online in round $t$.

The scheduler $\pi_t$ selects a leaf of the current discovery tree or the root. This means continuing work from the selected node or starting a new approach. A coding agent, or an agent dedicated to the task, carries out the continuation. The resulting node saves its context and score and becomes a new leaf. Exploration stops when:

1. The maximum number of online scheduling rounds is reached. One parallel batch counts as one round, which is not the same as one attempt.
2. The scheduling policy decides not to continue.

### 2. Offline Planning

Step 1 produces the discovery tree $\mathcal T_t$ for the current round, abbreviated below as $\mathcal T$. This tree consists of several chains because the online phase only extends leaf nodes. New branches cannot start from internal nodes, except for the root.

Offline evaluation first evaluates the current policy $\pi_t$, then its modified versions. The simulator holds the complete tree, but each replay starts from the root and reveals visited nodes to the policy incrementally. The policy chooses the workspace for the next step using only the information revealed so far. Let $\mathcal T_i^{m,k}$ denote the tree revealed after policy version $m$ completes $k$ replay rounds on historical tree $i$. Then $\mathcal T_i^{m,k}\subseteq\mathcal T_i$, and the policy code remains unchanged throughout a single replay. Suppose A1 is the earliest child created from the root; replay can proceed as follows:

| Round | Starting points selected by the policy | Simulator returns |
| --- | --- | --- |
| Round 1 | Root $r$ | Saved node A1 |
| Round 2 | Root $r$ | Saved node B1 |
| Round 3 | A1, B1 | Saved nodes A2, B2 |
| Next | Return an empty batch | Stop without revealing A3 |

This policy creates no new experiments; it only produces a new scheduling trajectory:

- **Select a non-root leaf of the currently revealed tree**: return its existing next node.
- **Select the root**: open the next unrevealed branch in creation order.
- **No recorded successor exists**: return no new node for that selection.

Replay ends when the policy returns an empty batch, reaches the maximum number of replay scheduling rounds, or reveals the entire historical tree. The score at the end of a replay is calculated as:

$$
V=\underbrace{\max_{v\in\mathcal T_{\mathrm{revealed}}}s_v}_{\text{best discovery quality}}
-\underbrace{\beta_1N}_{\text{total attempt penalty}}
+\underbrace{\beta_2\frac{N}{\max\{1,k^\star\}}}_{\text{average batch size reward}}.
$$

Here, $\mathcal T_{\mathrm{revealed}}$ is the subtree revealed by the end of this replay. The task score of node $v$ is $s_v$, with higher values being better, and $V$ is the score for this replay. The number of revealed non-root nodes is $N=|\mathcal T_{\mathrm{revealed}}|-1$, and $k^\star$ is the number of scheduling rounds completed during this replay. The fixed coefficients $\beta_1,\beta_2\geq0$ control the cost penalty and parallelism reward, respectively.

$N$ represents the generation–evaluation requests associated with this trajectory; replay itself does not execute these requests again. The first term uses the highest score among nodes revealed in this replay. It neither averages all scores nor includes unrevealed nodes. The second term encourages fewer attempts. The third favors scheduling work within fewer decision rounds, serving as a proxy reward for parallelism. Its denominator, $\max\{1,k^\star\}$, prevents division by zero.

To reduce reliance on a single exploration history and increase diversity, round $t$ of RSI replays each policy version on all $t$ accumulated historical trees. The average score is:

$$
V^m=\frac{1}{t}\sum_{i=1}^{t}V_i^m.
$$

Here, $m$ indexes the policy version, $i$ indexes the historical tree, and $V_i^m$ is the replay score of that same version $m$ on tree $i$. Its average score is $V^m$, rather than an average of the scores obtained by the old policies in their original rounds. Evaluation across multiple trees does not guarantee that overfitting to history is eliminated.

### 3. Selecting a Policy for Deployment

After each version has been evaluated offline on all historical trees, a policy development agent uses the replay trajectories, scores, and previous feedback to modify the scheduling policy code. This produces the next policy version, rather than directly modifying the trajectories.

Once the prescribed number of versions has been reached, the version with the highest average score is selected from all evaluated versions, including the original policy. This becomes the online policy for the next round. The only guarantee is that its average replay score on the fixed historical trees is no lower than the original policy's score. Improved online performance is not guaranteed.

### 4. The Role of Offline Evaluation

The value of the offline phase is not creating new discoveries from old records, but **comparing different scheduling programs at low cost**.

On the same historical tree, different policies may:

- Find results of the same quality using fewer attempts.
- Avoid stopping too early, thereby reaching better results.
- Organize valuable attempts into parallel batches.

Without replay, comparing each policy version usually requires another round of real exploration. Replay allows existing results to be used to screen policies before promising versions are deployed online.

## Limitations of RSI

1. Different tasks require different evaluation methods, which must be designed for the dataset or task setting.
2. Only leaf nodes or the root can be selected, so the tree consists of several chains. **Opening new child branches from an earlier parent node could be considered, but the details remain open for discussion.** This means branching again from an internal node that already has a successor. It is a proposed extension, rather than an operation supported in the paper.
3. The paper does not separately define a branch termination rule that stops a branch once its current task is complete. If the scheduler keeps extending an already completed leaf without improving the best score, this increases the attempt cost and reduces $V$.
4. Averaging over all historical trees can reduce reliance on any single tree. However, these histories are also used for policy development, so overfitting remains possible.

Paper: [Dream-RSI: Recursive Self-Improvement through Evolving Worlds](https://arxiv.org/abs/2609.14858v1).
