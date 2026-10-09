---
title: "KL 散度"
date: 2026-10-09T00:00:00+08:00
description: "KL 散度学习笔记：精确 KL、k₁、k₃，以及 GRPO 中的 KL 约束与聚合方式。"
draft: false
isCJKLanguage: true
---

<style>
.prose mjx-container[display="true"] { max-width: 100%; overflow-x: auto; overflow-y: hidden; padding: .4em 0; }
.prose .kl-table { overflow-x: auto; }
.prose .kl-table table { width: 100%; min-width: 24rem; }
</style>

经过 SFT 之后的模型是针对 Token level 上做了训练，让模型在 token 概率分布上更偏向遵守格式的回答。但具体如何回答，回答的准确性可以用 RL 来进一步增强，在 RL 的训练过程中，我们不希望模型又大幅度偏离 SFT 制定的回答格式，于是需要用到 KL 散度来进行约束，限制 RL 训练后的模型过度偏离 SFT 后的模型，同时争取进一步提升准确率。

设模型生成的完整回答是 $o=(o_1,o_2,\ldots,o_{|o|})$。 第 $t$ 步之前已经生成的内容记为：$o_{\lt t}=(o_1,o_2,\ldots,o_{t-1})$。 那么第 $t$ 步的上下文前缀设为：$h_t=(q,o_{\lt t})$。

在相同的上下文前缀下，参考策略模型（即SFT之后的模型，希望模型遵守的语气和格式）选择同一个候选 token 的概率为：$\pi_{\mathrm{ref}}(a\mid h_t)=\pi_{\mathrm{ref}}(a\mid q,o_{\lt t})$。 当候选位置写成一个点时：$\pi_\theta(\cdot\mid h_t),\pi_{\mathrm{ref}}(\cdot\mid h_t)$， 给定一个具体 token，得到一个概率数值；遍历所有候选 token，得到一份概率列表。

对于固定的上下文前缀 $h$，假设两个策略具有相同的支持集，共同候选集合为：

$$
\begin{aligned}
\mathcal A_h &=\{a:\pi_\theta(a\mid h)>0\}\\
&=\{a:\pi_{\mathrm{ref}}(a\mid h)>0\}.
\end{aligned}
$$

记集合大小为 $m_h$，定义两个概率，分别是从当前训练模型和参考模型取一个 token 的概率：

$$
p_a=\pi_\theta(a\mid h), \qquad b_a=\pi_{\mathrm{ref}}(a\mid h).
$$

其中 $p,b\in\mathbb R^{m_h}$。 并且两份列表各自归一化：$\sum_{a\in\mathcal A_h}p_a=1,\sum_{a\in\mathcal A_h}b_a=1$。

## 精确 KL

对固定的前缀 $h_t$，KL 的定义是：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h_t) \Vert \pi_{\mathrm{ref}}(\cdot\mid h_t) \right)\\
&\quad= \mathbb E_{a\sim\pi_\theta(\cdot\mid h_t)} \left[ \log\frac{\pi_\theta(a\mid h_t)} {\pi_{\mathrm{ref}}(a\mid h_t)} \right].
\end{aligned}
$$

左边比较的是两份完整的下一 token 分布。右边的期望表示：按照当前策略选择各个 token 的概率，对括号里的数值做加权平均。对有限候选集合，期望可以直接展开成求和：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h_t) \Vert \pi_{\mathrm{ref}}(\cdot\mid h_t) \right)\\
&\quad= \sum_{a\in\mathcal A_{h_t}} \pi_\theta(a\mid h_t) \log\frac{\pi_\theta(a\mid h_t)} {\pi_{\mathrm{ref}}(a\mid h_t)}.
\end{aligned}
$$

这一步使用的只是离散随机变量的期望定义。$\log\frac{\pi_\theta(a\mid h_t)}{\pi_{\mathrm{ref}}(a\mid h_t)}$ 是该 token 的对数概率比，单项可以为正，也可以为负；按当前概率对所有候选加权求和后，才得到非负的 KL 惩罚。若两个分布完全一致，所有比值都为 1，取 $\log$ 为 0，则无惩罚。

<strong>精确 KL 的总和不会是负数，因此可以起到惩罚的作用</strong>

使用对所有正数成立的不等式：

$$
\log u\leq u-1,\qquad u>0.
$$

令：

$$
u=\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}.
$$

移项可得：

$$
\log\frac{\pi_\theta(a\mid h)} {\pi_{\mathrm{ref}}(a\mid h)} \geq 1-\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}.
$$

两边乘以当前概率，再对所有候选求和：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h) \Vert \pi_{\mathrm{ref}}(\cdot\mid h) \right)\\
&\quad\geq \sum_{a\in\mathcal A_h} \left[ \pi_\theta(a\mid h) -\pi_{\mathrm{ref}}(a\mid h) \right]\\
&\quad=1-1=0.
\end{aligned}
$$

这个证明同时用到了两份概率分布各自加起来等于 1。前面的基本不等式只在 $u$ 等于 1 时取等号。因此，在共同支持的条件下，KL 为 0 当且仅当两个分布在所有候选上完全一致。

## $k_1$ {#k1}

固定前缀 $h$，对抽到的 token $a$，定义：

$$
k_1(a,h;\theta) = \log\frac{\pi_\theta(a\mid h)} {\pi_{\mathrm{ref}}(a\mid h)}.
$$

如果采用“第 $i$ 条回答、第 $t$ 个 token”的下标，先定义：

$$
h_{i,t}=(q,o_{i,\lt t}),
$$

然后写成：

$$
k_{1,i,t}(\theta) = \log \frac{\pi_\theta(o_{i,t}\mid h_{i,t})} {\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})}.
$$

它只是被抽中 token 的对数概率比。对固定前缀、从当前策略抽取的 token，有：

$$
\begin{aligned}
&\mathbb E_{a\sim\pi_\theta(\cdot\mid h)} \left[k_1(a,h;\theta)\right]\\
&\quad= D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h) \Vert \pi_{\mathrm{ref}}(\cdot\mid h) \right).
\end{aligned}
$$

这是 KL 定义的直接应用。$k_1$ 的单个样本可以为负，整个分布的 KL 非负。同时，如果选择多次抽样 $k_1$ 的话，总体期望与精确 KL 一样，属于无偏估计：

保持前缀不变，独立地重复抽取 $N$ 个 token：

$$
a^{(1)},\ldots,a^{(N)} \overset{\mathrm{iid}}{\sim} \pi_\theta(\cdot\mid h).
$$

$$
\begin{aligned}
&\widehat D_{\mathrm{KL}}^{(k_1,N)}(h;\theta)\\
&\quad= \frac{1}{N}\sum_{n=1}^{N} \log \frac{\pi_\theta(a^{(n)}\mid h)} {\pi_{\mathrm{ref}}(a^{(n)}\mid h)}.
\end{aligned}
$$

对这个随机估计量取期望，利用期望的线性性：

$$
\begin{aligned}
&\mathbb E\!\left[ \widehat D_{\mathrm{KL}}^{(k_1,N)}(h;\theta) \right]\\
&\quad= \frac{1}{N}\sum_{n=1}^{N} \mathbb E\!\left[ \log \frac{\pi_\theta(a^{(n)}\mid h)} {\pi_{\mathrm{ref}}(a^{(n)}\mid h)} \right]\\
&\quad= D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h) \Vert \pi_{\mathrm{ref}}(\cdot\mid h) \right).
\end{aligned}
$$

## 整条回答的 KL 约束

规定 EOS 结束符也为一个 token，采用以下规定：

1. 回答生成 EOS 时结束。
2. 已经结束的回答，在后续位置确定性地补 EOS。
3. 到第 $T$ 个位置时，两个策略都强制选择 EOS。

后文 $|o|$ 指含首次 EOS、不含补齐位置的有效长度。

一条较早结束的回答，可以是 ${o=(\text {hi,how,are,you,today,?,EOS,EOS,EOS})}$，第一次 EOS 是结束选择，后面的 EOS 是为统一长度补上的。补齐位置的两个条件概率都等于 1：

$$
\pi_\theta(\mathrm{EOS}\mid h_t) = \pi_{\mathrm{ref}}(\mathrm{EOS}\mid h_t) =1.
$$

为了明确区分层级，用大写符号表示完整回答的概率：

$$
P_\theta(o\mid q), \qquad P_{\mathrm{ref}}(o\mid q).
$$

对于一条已经指定的完整回答，每一步的 token 与前文都是确定的。当前策略产生这条回答的概率是：

$$
P_\theta(o\mid q) = \prod_{t=1}^{T} \pi_\theta(o_t\mid h_t).
$$

参考策略产生同一条回答的概率是：

$$
P_{\mathrm{ref}}(o\mid q) = \prod_{t=1}^{T} \pi_{\mathrm{ref}}(o_t\mid h_t).
$$

把当前策略的乘积展开，就是：

$$
\begin{aligned}
P_\theta(o\mid q) &=\pi_\theta(o_1\mid q)\\
&\quad\times\pi_\theta(o_2\mid q,o_1)\\
&\quad\times\cdots\\
&\quad\times \pi_\theta(o_T\mid q,o_1,\ldots,o_{T-1}).
\end{aligned}
$$

它没有假设各个 token 独立：每一项都明确依赖前面已经出现的 token。先取两条完整回答概率的比值，再取对数：

$$
\begin{aligned}
&\log\frac{P_\theta(o\mid q)} {P_{\mathrm{ref}}(o\mid q)}\\
&\quad= \log \frac{\prod_{t=1}^{T}\pi_\theta(o_t\mid h_t)} {\prod_{t=1}^{T}\pi_{\mathrm{ref}}(o_t\mid h_t)}\\
&\quad= \log\prod_{t=1}^{T} \frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)}\\
&\quad= \sum_{t=1}^{T} \log\frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)}.
\end{aligned}
$$

对于一个固定问题 $q$，把遵守上述停止规则的可能回答集合记为：$\Omega_T(q)$，有：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( P_\theta(\cdot\mid q) \Vert P_{\mathrm{ref}}(\cdot\mid q) \right)\\
&\quad= \sum_{o\in\Omega_T(q)} P_\theta(o\mid q) \log\frac{P_\theta(o\mid q)} {P_{\mathrm{ref}}(o\mid q)}\\
&\quad= \mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \log\frac{P_\theta(o\mid q)} {P_{\mathrm{ref}}(o\mid q)} \right].
\end{aligned}
$$

结构与单个位置的 KL 一样，只是候选对象从“一个 token”变成了“一条完整回答”。权重也从下一 token 的概率，变成了当前策略生成整条回答的概率。同样的，对于整条回答的 KL 形式还有多种变体，如：

1. 把连乘的对数改写为求和：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( P_\theta(\cdot\mid q) \Vert P_{\mathrm{ref}}(\cdot\mid q) \right)\\
&\quad= \mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \sum_{t=1}^{T} \log\frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)} \right].
\end{aligned}
$$

2. 把有限求和移到期望外面：

$$
\begin{aligned}
&\mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \sum_{t=1}^{T} \log\frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)} \right]\\
&\quad= \sum_{t=1}^{T} \mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \log\frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)} \right].
\end{aligned}
$$

## $k_3$ {#k3}

依然用 $h_{i,t}$ 表示第 $i$ 条回答第 $t$ 个 token 之前的上下文，设 $u_{i,t}(\theta)=\frac{\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})}{\pi_\theta(o_{i,t}\mid h_{i,t})}$。那么我们的逐 token 惩罚估计为：

$$
\hat d_{i,t}(\theta) = u_{i,t}(\theta) -\log u_{i,t}(\theta)-1.
$$

$$
\begin{aligned}
\hat d_{i,t}(\theta) &= \frac{\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})} {\pi_\theta(o_{i,t}\mid h_{i,t})}\\
&\quad- \log \frac{\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})} {\pi_\theta(o_{i,t}\mid h_{i,t})} -1.
\end{aligned}
$$

我们把上述定义为 $k_3$，其中如果讨论固定前缀 $h$ 下的候选 token $a$，有定义：

$$
\begin{aligned}
k_3(a,h;\theta) &= \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}\\
&\quad- \log\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)} -1.
\end{aligned}
$$

<strong>与 $k_1$ 不同，$k_3$ 的每个样本都是非负的。</strong>

任意正数 $u$，考虑函数：

$$
g(u)=u-\log u-1.
$$

它的导数为：

$$
g'(u)=1-\frac{1}{u}.
$$

当 $u$ 小于 1 时，导数为负；当 $u$ 大于 1 时，导数为正。因此函数在 $u$ 等于 1 时取得最小值：

$$
g(1)=1-\log 1-1=0.
$$

于是：

$$
u-\log u-1\geq0,\qquad u>0.
$$

同样的，<strong>按当前策略采样时，$k_3$ 也是精确 KL 的无偏估计：</strong>

$$
\begin{aligned}
&\mathbb E_{a\sim\pi_\theta(\cdot\mid h)} \left[ \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)} \right]\\
&\quad= \sum_{a\in\mathcal A_h} \pi_\theta(a\mid h) \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}\\
&\quad= \sum_{a\in\mathcal A_h} \pi_{\mathrm{ref}}(a\mid h)\\
&\quad=1.
\end{aligned}
\tag{1}
$$

$$
-\log \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)} = \log \frac{\pi_\theta(a\mid h)} {\pi_{\mathrm{ref}}(a\mid h)}.
\tag{2}
$$

把公式 (1)、(2) 代入到 $k_3$ 的期望计算：

$$
\begin{aligned}
&\mathbb E_{a\sim\pi_\theta(\cdot\mid h)} \Biggl[ \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}\\
&\qquad\qquad- \log\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)} -1 \Biggr]\\
&\quad= 1+ \mathbb E_{a\sim\pi_\theta(\cdot\mid h)} \left[ \log\frac{\pi_\theta(a\mid h)} {\pi_{\mathrm{ref}}(a\mid h)} \right]-1\\
&\quad= D_{\mathrm{KL}}\!\left( \pi_\theta(\cdot\mid h) \Vert \pi_{\mathrm{ref}}(\cdot\mid h) \right).
\end{aligned}
$$

可以看出 $k_3$ 可以看作给 $k_1$ 增加了一个平均值为 0 的修正项，修正项改变单个样本的值，使其非负，但不改变期望值：

$$
\begin{aligned}
&\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)} -\log\frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}-1\\
&\quad= \log\frac{\pi_\theta(a\mid h)} {\pi_{\mathrm{ref}}(a\mid h)} +\left( \frac{\pi_{\mathrm{ref}}(a\mid h)} {\pi_\theta(a\mid h)}-1 \right).
\end{aligned}
$$

## KL 进入 GRPO

裁剪目标使用当前策略与旧策略的概率比：

$$
\rho_{i,t}(\theta) = \frac{\pi_\theta(o_{i,t}\mid q,o_{i,\lt t})} {\pi_{\mathrm{old}}(o_{i,t}\mid q,o_{i,\lt t})}.
$$

$k_3$ 使用参考策略与当前策略的概率比：

$$
u_{i,t}(\theta) = \frac{\pi_{\mathrm{ref}}(o_{i,t}\mid q,o_{i,\lt t})} {\pi_\theta(o_{i,t}\mid q,o_{i,\lt t})}.
$$

前者衡量当前模型相对于采集这批样本时的变化，用在奖励优势的裁剪部分。后者用来构造偏离参考模型的惩罚。分母、分子和比较目的都有区别。

在这里的结果监督 GRPO 中，每个 token 的贡献定义为：

$$
\begin{aligned}
\ell_{i,t}(\theta) &= \min\Bigl( \rho_{i,t}(\theta)\hat A_i,\\
&\qquad \operatorname{clip}\bigl( \rho_{i,t}(\theta),1-\epsilon,1+\epsilon \bigr)\hat A_i \Bigr)\\
&\quad-\beta\,\hat d_{i,t}(\theta).
\end{aligned}
$$

GRPO 的优化目标是：

$$
\begin{aligned}
&J_{\mathrm{GRPO}}(\theta)\\
&\quad= \mathbb E_{ \substack{ q\sim\mathcal D\\
\{o_i\}_{i=1}^{G} \sim P_{\mathrm{old}}(\cdot\mid q) } } \left[ \frac{1}{G}\sum_{i=1}^{G} \frac1{|o_i|} \sum_{t=1}^{|o_i|} \ell_{i,t}(\theta) \right].
\end{aligned}
$$

前面 $k_3$ 的无偏性要求按当前策略采样；这里回答来自旧策略，因此使用的是 KL 代理项。

训练最大化这个目标。若实现将目标取负后作为损失最小化，那么 KL 惩罚会以正号出现在损失中。这只是最大化目标与最小化负目标的符号关系。对已经采样到的一组回答，目标内部使用的平均惩罚估计是：

$$
\frac{1}{G}\sum_{i=1}^{G} \frac1{|o_i|} \sum_{t=1}^{|o_i|} \hat d_{i,t}(\theta).
$$

代入完整的 $k_3$ 概率表达式：

$$
\begin{aligned}
&\frac{1}{G}\sum_{i=1}^{G} \frac1{|o_i|} \sum_{t=1}^{|o_i|} \Biggl[ \frac{\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})} {\pi_\theta(o_{i,t}\mid h_{i,t})}\\
&\qquad\qquad- \log \frac{\pi_{\mathrm{ref}}(o_{i,t}\mid h_{i,t})} {\pi_\theta(o_{i,t}\mid h_{i,t})} -1 \Biggr].
\end{aligned}
$$

在最大化的目标里，再乘以负的 $\beta$。这个式子保留了完整的回答下标、token 下标、上下文条件、概率比和两层平均。$\beta$ 越大，偏离参考分布的代价越高；$\beta$ 越小，奖励驱动的变化相对更容易占主导。

作为对比，按当前策略采样时，完整回答 KL 对应：

$$
\begin{aligned}
&D_{\mathrm{KL}}\!\left( P_\theta(\cdot\mid q) \Vert P_{\mathrm{ref}}(\cdot\mid q) \right)\\
&\quad= \mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \sum_{t=1}^{|o|} \log \frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)} \right].
\end{aligned}
$$

如果改为：

$$
\mathbb E_{o\sim P_\theta(\cdot\mid q)} \left[ \frac1{|o|} \sum_{t=1}^{|o|} \log \frac{\pi_\theta(o_t\mid h_t)} {\pi_{\mathrm{ref}}(o_t\mid h_t)} \right],
$$

每条回答的总和都额外乘上了自己长度的倒数。这一般不再等于原来的完整回答 KL。

如果所有回答长度固定为同一个数，长度平均只带来一个固定缩放因子。如果长度不同，这个因子随回答变化，不能用一个统一常数从期望里提出。因此，长回答可能在每步偏移相似时累积更多序列惩罚；采用回答内平均，会改变这种长度效应。长度归一化可以是一种训练设计，但需要按它实际计算的量解释结果。

## 流程

<strong>估计方式</strong>：是精确 KL 还是 $k_1$ 或者 $k_3$

三者的区别可以概括为：

<div class="kl-table">

| 计算方式    | 使用的信息                      | 固定模型与前缀后的特点         |
| ----------- | ------------------------------- | ------------------------------ |
| 精确条件 KL | 全部候选 token 的两份概率分布   | 数值确定，理论上非负           |
| $k_1$       | 实际选中 token 的当前与参考概率 | 随采样变化，可以为负           |
| $k_3$       | 实际选中 token 的当前与参考概率 | 随采样变化，每个样本理论上非负 |

</div>

<strong>聚合方式</strong>：

设同一个问题共有 $G$ 条参与统计的回答，各回答补齐到统一的最大长度。定义有效 token 掩码：$m_{i,t}\in\{0,1\}$。其中，取值为 1 表示该位置计入统计，取值为 0 表示排除。以下假设每条参与平均的回答至少包含一个有效 token。

沿用前文的逐 token KL 惩罚估计：$\hat d_{i,t}(\theta)$，同一组逐 token 数值，可以采用不同的聚合方式。区别在于：<strong>是否先按回答长度归一化，以及回答或 token 分别获得多大的权重。</strong>

1. 回答内 token 平均，再对回答等权平均：

$$
\frac{1}{G} \sum_{i=1}^{G} \frac{ \sum_{t=1}^{T_{\max}} m_{i,t}\hat d_{i,t}(\theta) }{ \sum_{t=1}^{T_{\max}} m_{i,t} }.
$$

这种方式下，<strong>每条回答的权重相同</strong>。较长回答不会仅因为包含更多 token，就在组内获得更大的总权重；相应地，短回答中的单个 token 会分到更大的权重。

2. 对全部有效 token 混合平均：

$$
\frac{ \sum_{i=1}^{G} \sum_{t=1}^{T_{\max}} m_{i,t}\hat d_{i,t}(\theta) }{ \sum_{i=1}^{G} \sum_{t=1}^{T_{\max}} m_{i,t} }.
$$

这种方式下，<strong>每个有效 token 的权重相同</strong>。较长回答包含更多有效 token，因此在最终平均值中占据更大的权重。

3. 先计算每条回答的序列总和，再对回答等权平均：

$$
\frac{1}{G} \sum_{i=1}^{G} \sum_{t=1}^{T_{\max}} m_{i,t}\hat d_{i,t}(\theta).
$$

这种方式保留了<strong>序列总量的长度效应</strong>。在逐 token 惩罚相近时，较长回答通常会累积更大的惩罚总和。

本文讨论的原始 GRPO 形式，对 KL 代理项采用<strong>第 1 种方式</strong>。
