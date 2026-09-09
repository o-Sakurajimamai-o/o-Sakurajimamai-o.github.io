---
title: "考研数学笔记：高等数学与线性代数"
date: 2026-09-09
lastmod: 2026-09-09
description: "高等数学与线性代数的完整备考笔记。"
translationKey: kaoyan-math
draft: false
---

按最新备份逐条保留笔记原文、例题、推导、复习提醒和配图，并整理标题与排版。原文中的错误或依赖条件的表述，以对应脚注补充勘误，便于对照回看。

<!-- source-content:calculus:start -->
>   做到一个新的问题，想起与过去某个问题类似。发现在解答中，对此类问题，以及工具和方法的理解是存在缺陷的，或者发现理解不够深刻。于是通过解决新的问题，一并更新迭代过去的理解。

## 极限、导数以及微分 {#calculus-h-01}

---

<!-- added-heading -->

### 代数变换、递推与乘积求导 {#algebra-and-product-derivatives}

* 双曲正弦函数 $y=\dfrac{e^x-e^{-x}}{2}$, 反解方法如下:
  先化成 $2y+e^{-x}=e^x$, 同乘 $e^y$, 解关于 $e^y$ 的二元一次方程组, 且 $e^y$ 不为负[^cal1-6]

---

* 对于求解数列第 $n$ 项时, 要灵活运用 $x_n=f(x_{n-1})$ 且 $x_n=x_{n-1}=a$, 然后化成方程组求解$(660_{18})$[^cal1-8]

---

* 若 $f(x)=u(x)v(x)$, 则 $f(x)$ 的泰勒展开式是这两个函数的泰勒展开式相乘, 同时也表明了, 无穷小也是会相乘的

---

* 关于 $f(x)=u(x)v(x)$ 的导数
  $f'(x)=u(x)v'(x)+v(x)u'(x)$ 证明在 $p_{167}$ 页, 如果一个多因使或者 $n$ 阶导无法直接求出, 尝试简化到上述模式, 然后通过**求导或者是莱布尼茨公式来求导**

  $$
  \frac{d^n}{dx^n} [u(x) v(x)] = \sum_{k=0}^{n} \binom{n}{k} u^{(k)}(x) v^{(n-k)}(x)
  $$

  或者是利用泰勒展开, 如第一点提到的多项式相乘

---

<!-- added-heading -->

### 无穷大、等价无穷小与幂指极限 {#infinity-and-equivalents}

* **无穷大指自某个数之后处处无穷大**, 而若出现时而无穷大时而常数如 $\dfrac{1}{x^2}sin\dfrac{1}{x}$ 这种振荡, 只能称为无界但非无穷大[^cal1-16]
  * 若 ${\lim_{n\to \infty}x_ny_n=\infty}$, 则有如下推论:
    * ${\lim_{n\to\infty}x_n=\infty,\,\lim_{n\to\infty}y_n=\infty}$ 至少有一个成立, **该说法不成立, 若换成无界变量, 则成立**, 原因在于可以构造一个奇数项为 $1$, 偶数项为 $n$ 的数列, 则不为无穷大, 因为不存在某个数之后处处无穷
    * 若 ${\lim_{n\to\infty}x_n=a(a\neq0)}$, 则可以推出 ${\lim_{n\to\infty}y_n=\infty}$, 因为此时 $x_n$ 恒定, 则 $y_n$ 也必须恒定无穷大, 符合无穷大定义[^cal1-19]
* 关于函数的无穷大, 若两函数均为无穷大, 则相乘未必是无穷大, 因为**可能出现振荡**, 故此时极限其实是不存在的[^cal1-20]
* 关于数列的无穷大, 若两数列均为无穷大, 则相乘一定是无穷大, 由于**数列不像函数那样存在路径依赖处处紧逼, 故由于离散性必定无穷大**[^cal1-21]

---

* 对于多项相乘的求导, 要学会**简化**
  * $f(x)=\prod_{n=1}^{100}\,(\tan(\frac{\pi\,x^{n}}{4})-n)$ , 求 $f'(1)$
    通过观察发现 $n=1$ 的时候值为 $0$, 由于多因式求导非常复杂, 我们不希望求解多次导, 那么可以提出 $n = 1$ 的时候, 这样就有

    $$
    (\tan(\frac{\pi\,x}{4})-1)\times\prod_{n=2}^{100}\,(\tan(\frac{\pi\,x^{n}}{4})-n)
    $$

    此时就变成了 $f(x)=u(x)v(x)$ 的形式, 再次求导就会非常方便
  * $y=\frac{1-x}{1+x}$ , 求 $y^{(n)}(0)$
    我们知道 $(\frac{1}{ax+b})^{n}$ 的导是好求的, 那么就进行转换 $y=-1+\frac{2}{1+x}$

---

<!-- added-heading -->

### 一点处的信息与邻域性质 {#pointwise-and-neighborhood}

* 一点处的信息不可推出区间的性质
  * 若 ${f'(x_0)}$ 存在, ${f(x)}$ 是否推出在 $x_0$ 邻域连续
    * 不可推出, 考虑狄利克雷函数$f(x)=\begin{cases} 1, x为无理数 \\ 0, x为有理数\end{cases}$, 在 $x_0$ 可导且 $f'(x_0)=0$, 但明显的, 不连续[^cal1-32]
  * 若 ${f'(0)\gt 0}$, 是否推出存在 ${\delta\gt 0}$ 有 ${f(x)}$ 在 ${(-\delta,\,\delta)}$ 单调增加
    * 不可推出, 考虑如下函数, 发现在 $x=0$ 邻域处导数振荡, 但$f'(0)=\dfrac{1}{2}$

      $$
      f(x)=\begin{cases}{x^2sin\frac{1}{x} +\frac{1}{2}x,\,x\neq0}\\ {0,\,x=0}\end{cases}
      $$

    * 但可以推出存在 ${\delta}$, 左邻域均比该点小, 右邻域均比该点大, 但**不意味着区间上严格单调**。
  * 搞清楚邻域连续和整体连续, 若 ${f(x)}$ 连续, ${g(f(x_0))}$ 不连续, 但 **${g(f(x))}$ 在 ${x_0}$ 处不一定不连续**[^cal1-36]
    * 这是因为 ${f(x_0)}$ 连续, 则 ${g(x)}$ 在 ${x=f(x_0)}$ 是不连续的, 但这个是在邻域部分得出, 即在 ${x_0}$ 处可能由于跳跃或可去亦或者无穷震荡造成的不连续, 但若 **${f(x)}$ 恒等于 ${C}$**, 那么此时复合后仍然连续, 因为邻域的不连续性消失了

---

* 关于等价无穷小
  * 对于形如: ${\int_{0}^{\phi(x)}f(t)dt}$, 若 ${\phi(x)}$ ~ ${x^n}$,${f(t)}$ ~ ${t^m}$, 则阶数为: ${n\times(m+1)}$
  * 无穷小相加, 最终阶数取低阶无穷小, 故类似的, 关于上面形式若出现${}$: ${\int_{g(x)}^{h(x)}f(t)dt}$, 则取 ${n\times (m + 1),\,k\times (m+1)}$ 的低阶[^cal1-41]
  * 对于任何泰勒展开形如 ${[1+Ax^p + o(x^p)]^\alpha}$, 其阶数仍不变, 只是系数变为 ${A\alpha}$如 ${1-cos^\alpha x}$ ~ ${\dfrac{\alpha}{2}x^2}$

---

* 对于导数的定义, 其实和**求极限**是一回事, 两者可以互逆
  * 若只给出了 $\lim_{x\to x_0}=A$ 这只代表在 $x_0$ 两边的极限是相同的, 并不意味着连续, 必须要在 $x_0$ 有定义且有 $f(x_0) = A$ 才算作在其点连续, 当没有给出定义点而是一个**抽象函数**(考虑分段函数, 如 $\begin{cases}f(x)=x^2,\,x\neq0\\ C(C\neq 0),\,(x=0)\end{cases}$时, 那就这个点就不连续, 所以就不会可导[^cal1-45]
    * ${\lim_{x\to x_0}\dfrac{f'(x)}{(x-x_0)^2}=A,\,A\neq 0}$, 是否意味着 $x_0$ 邻域内单调?
      * 从等式出发, 只能得到 $f'(x)$ 在 $x_0$ 邻域不包含 $x_0$ 点的极限是存在的, 即在这个范围内 $f(x)$ 是单调的, 但是不意味着 $x_0$ 也单调, 因为**无法推出 $f'(x_0)=A$, 这个点甚至可以不可导, 故无法推出 $x_0$ 邻域单调, 但可以推出去心邻域单调**[^cal1-47]
  * 相对的, 若是给出了 $f(x)$ 在区间 $[a,\, b]$ 可导则证明存在 $\lim_{x\to a^{+}}f(x)$ 存在, 右区间的左边也同理, 那么此时如果有隐含条件如 $\lim_{x\to a^{+}}\,\frac{f(x)}{x}=A$, 由于 $f(x)$ 在 $a$ 处右连续, 自然的就有 $f(a)=a\times A$
    * 进一步, **若 ${f(x)}$ 左导数, 右导数均存在, 则一定有函数左连续且右连续, 即 ${f(x)}$ 在该点连续**
  * ${f'(x_0)}$ 处有值并不代表导数在该点处连续, 不要和导数定义的左导右导混淆, 左导等于右导是导数在该点的极限存在, 并非连续, 该点的导数甚至可以不存在, 简单的例子就是:  $\begin{cases}f(x)=x^2,\,x\neq0\\ C(C\neq 0),\,(x=0)\end{cases}$[^cal1-50]
  * 更深层次的, 已知 $g(x)$, 若题目仅仅指出函数 $f(x)$ 可导, 且给出函数的一个等价极限, 如给出 $\lim_{x\to x_0}\dfrac{f(x)}{g(x)}=A,\,g(x)=x^2$, 此时分如下几种情况:
    * 若确定 $f(x)$ 是具体函数, 即给出具体的 $f(x)$ 表达式, 则通过极限存在可求得一些常数的值, 然后去判断之后的性质
    * 若 $f(x)$ 是抽象函数, 未明确给出表达式, 则需要细致讨论
      * 由于极限存在, 说明: $f(x)=Ax^2+o(x^2)$, 则可得到 $f(x)$ 是 $g(x)$ 的高阶或等价无穷小, 代入导数定义有: ${\lim_{x\to x_0}\dfrac{f(x)}{g(x)}\times \dfrac{g(x)}{x}}$ 可得出 **一阶导数的相关信息**, 这里例子假设 ${A\neq0 \Rightarrow f'(0)=0}$, 但由于题目**没明确是否二阶可导**, 故无法推出高阶导数的性质.[^cal1-54]
      * 同样的, 由于不知道是否二阶可导, 故不能直接使用洛必达, 因为洛必达要求要**有连续的导数(二阶可导, 若函数 ${n}$ 阶可导, 则可以用到 ${n-1}$ 阶, 若函数 ${n}$ 阶连续可导, 则可用到 $n$ 阶)**, 这样才能保证极限的可替换性, 所以无法推出: ${\lim_{x\to x}\dfrac{f'(x)}{g'(x)}}$ 存在[^cal1-55]
      * 然后考虑极限的保号性, 考虑 $g(x)$ 在 $x_0$ 处的正负性, 若得知 $f'(x_0)=0$, 则可由极值定义求出是否符合极值点性质(极值不要求导数或者连续), 由于例中的 $g(x)=x^2$, 故 $f(0)$ 在邻域均大于 $0$, 符合极小值定义[^cal1-56]
      * 综上, 针对例子 $g(x)=x^2$有:
        * **一定能推出的结论：**
          - $f(0)=0,\,f'(0)=0$
          - 若 $A\gt 0$, $f(0)$ 为极小值
        * **需要额外假设（如二次可微）才能推出的结论:**
          * $f''(0)=2A$
          * 洛必达或泰勒得: $\lim_{x \to 0} \dfrac{f'(x)}{x} = 2A$
      * 关于以上的所有无法推出的情况, 可构造函数证明:

        $$
        f(x)=\begin{cases}{x^3sin\frac{1}{x} +x^2,\,x\neq0}\\ {0,\,x=0}\end{cases}
        $$

  * 对于绝对值可导问题, 要明白函数的绝对值不可导情况下一定是加上绝对值之后使得函数变得相对来说 "没那么近了" 那么如果对于一个连续可导且值不为零的邻域内, 其值都是同号的, 加上绝对值也必然同号, 当然可导.
    * 对于一个**可导且值为零的邻域**, 加上绝对值之后有一边变号(这里一定是整体加上绝对值, 否则仍需按照定义), 距离变大, 不可导[^cal1-66]
    * **一定有 $f(x_0)$ 连续 $\rightarrow$ $|f(x_0)|$ 连续**, 反之不一定成立
    * 基于上述理论, 可以想象, 若函数 ${\phi(x)}$ 可导, 则 ${f(x)=\phi(x)|g(x)|}$ 在所有的 $g(x_0)=0$ 处有:
      * **若 ${\phi(x_0)=0}$ 且 ${\phi(x_0)}$ 可导, 则 $f(x_0)$ 可导, 该条件为充要条件**[^cal1-69]
        * 从理论证明, 在 $x_0$ 处的泰勒展开, 若 $\phi(x_0)\neq 0$, 则该处为 $A|x-x_0|$, 导数为 ${\dfrac{A|x-x_0|+o(x)}{x}}$, 显然不可导, 若 ${\phi(x_0)=0}$, 则导数 ${\dfrac{0+o(x)}{x}=0}$, 显然可导
        * 几何意义上出发, 由于 ${|g(x_0)|}$ 在 $x_0$ 处为**折角**, 若 ${\phi(x_0)}$ 不为零, 则乘上一个常数当然还是折角, 但是若乘上零则变为直线
  * $f(x)$ 在 $x_0$ 连续, 存在 $\lim_{x\to0}\dfrac{f(x+x_0)-f(x_0)}{x^n(n\geq2)}=A$ 或者$\lim_{x\to x_0}\dfrac{f(x)-f(x_0)}{(x-x_0)^n(n\geq2)}=A$, 此时 $f(x_0)$ 可导且 $f'(x_0)=0$, 进一步的, 还可以通过对上述式子进行洛必达可能会得到 $f''(x_0),\,\,f'''(x_0)$ 的性质[^cal1-72]
  * 若已知导数在 $x=x_0$ 处存在, 并有$f'(x_0)=A$, 求 $\lim_{x\to x_0^{+(-)}}\dfrac{f(x)-f(x_0)}{g(x)}$,这里的 $g(x_0)=0或\infty$ 由于导数并不一定在 $x_0$ 处连续, 则不能使用洛必达或者柯西中值, 考虑构造 $\lim_{x\to x_0^{+(-)}}\dfrac{\frac{f(x)-f(x_0)}{x-x_0}}{\frac{g(x)-g(x_0)}{x-x_0}}$, $g(x)$ 在 $x_0$ 处连续[^cal1-73]
  * 在 $f(0)=0$ 的条件下, 想要推出可导, 极限式子必须同时满足如下几点, 否则无法推出 ($660-164,\,880_{base}-2.1.11$)
    * **阶相同, 或者是分子的阶 $\leq$ 分母的阶极限存在**, 类似于 $\lim_{x\to 0}\dfrac{f(x)}{x},\,\dfrac{f(x)}{x^2},\,\dfrac{f[ln(1-x)]}{x}$[^cal1-75]
    * **双侧都有定义**, 即保证 $x\to x_0$ 包括 $x\to x^{+},\,x\to x_0^{-}$, 如 $\lim_{x\to 0}\dfrac{f(\sqrt{x^2 + 1}-1)}{x^2}$, 由于 ${(\sqrt{x^2 + 1}-1)\gt 0}$, 无法保证在 $x\to x_0^{-}$ 的极限存在
    * 不可跨过定点 $x_0$, 如 $\lim_{x\to 0}\dfrac{f(x)-f(-x)}{x},\,\dfrac{f(x)-f(x^2)}{x}$, 跨过了 $0$ 点, 则不可推出, 反例就是 $f(x)=\begin{cases} 1, x\neq 0 \\ 0, x=0\end{cases}$
    * 实路径存在, 与第二条相差不大, 如 $\lim_{n \to \infty}nf(\frac{1}{n})$, 由于 $n$ 不能取负数, 即只能代表趋向于 $0^{+}$ 时存在, 若是$\lim_{m\to\infty}mf(\frac{1}{m}),\, m是非零整数$, 也不可以, 因为不包含无理数, 反例就可以用迪利克雷函数 $f(x)=\begin{cases} 1, x为无理数 \\ 0, x为有理数\end{cases}$
  * 注意隐含的导数值, 透过题目信息得到导数值, 尝试利用放缩以及特殊值点观察, 如:
    * ${|f(x)|\leq x^2 \Rightarrow f(0)=0\Rightarrow f'(0)=\lim_{x\to0}\dfrac{f(x)}{x}\leq \lim_{x\to0}\dfrac{x^2}{x}=0}$[^cal1-80]

---

<!-- added-heading -->

### 高阶导数与图形信息 {#higher-derivatives-and-geometry}

* 关于高阶导数
  * 若出现了高阶导数(可能搭配抽象函数一起), 此时的高阶指阶数大于等于 $2$, 特别是在导数阶数在 $1\to3$ 这个范围内, 由于只给出抽象函数 $f(x)$ 无从下手, 只能**考虑驻点, 拐点, 图形凹凸性, 泰勒展开等**去寻找线索
    * 给出 $f(x)$ 二阶可导且 $f''(x)\gt (\lt )0$, 则根据在 $x=0$ 处泰勒展开可放缩为 $f(x)=f(0)+f'(0)x+\dfrac{f''(0)}{2!}x^2+o(x^2)$, 然后配合题目的约束条件 $\int_{-a}^{a}f(x)dx=b$, 由于 $x$ 项为奇函数, $x^2$ 项恒大于 $0$, 积分有 $b\geq 2a\times f(0)$[^cal1-84]
      * 还可以**利用凹凸性两点的连线一定在切线之上(下)**, 若为凹函数则大于等于切线, 凸函数小于等于切线, 此时我们有:

        <figure class="fig"><img src="/blog/kaoyan-math/figures/convexity-and-tangent.png" alt="原笔记配图 1" width="1321" height="587" loading="lazy" decoding="async"><figcaption>原笔记配图 1</figcaption></figure>

        [^cal1-85]
  * 若要求高阶导数, 且阶数适中, 但直接求导很难, 一般有以下几种方法:
    * 若是求 $f^{(n)}(0)$, 则观察函数的奇偶性, 检查是否为奇函数
    * 求导观察规律, 是否存在规律性
    * 直接或者求一到两次导, 若出现可以利用泰勒展开则直接利用, 如:

      $$
      f(x)=ln(\sqrt{1+x^2}-x), 求 f^{(5)}(0)
      $$

      直接求导或者利用泰勒困难, 考虑求一次导 $\to f'(x)=-\dfrac{1}{\sqrt{1+x^2}}$, 发现可以利用泰勒展开 $(1+x)^{α}$, 直接展开到 $-\dfrac{3}{8}x^4+o(x^4)$, 此时求导简单, 有 $f^{(5)}=f'^{(4)}(0)=-9$
    * 下面给出一些常见的求高阶导数技巧, 这些技巧通常是级数或者积分要用到的, 但有时往往想不到求导上面, 要多注意
      * ${F(x,\,y)=\dfrac{f(x)(或f(y))}{ax^2-by^2}}$, 利用平方差进行凑项, 如:

        $$
        {F(x,\,y)=\dfrac{2x}{x^2-y^2}=\dfrac{1}{x+y}+\dfrac{1}{x-y}}
        $$

        利用 ${(\dfrac{1}{x})^n=(-1)^n\dfrac{n!}{x^{n+1}}}$, 若求 ${\dfrac{\partial F}{\partial x}}$ 则整体代换 $x$, 同理 ${\dfrac{\partial F}{\partial y}}$[^cal1-93]

---

* 灵活观察是否存在 $1^{\infty}$, 是否满足 $\lim a(x)b(x)=A$,$lima(x)=0$,$limb(x)=\infty$, 此时直接有

  $$
  lim(1+a(x))^{b(x)}=e^{A}
  $$

   注: 通过简化, 这里的 $1$ 是**关键点**, 可以进行很多变换
* 若底项不为 $e$ 而是 $a$, 则需要乘上 $lna$, 例:

  $$
  {\lim_{x\to+\infty}x^P(a^{\frac{1}{x}}-a^{\frac{1}{x+1}})}
  $$

  提取公因子直接化成:

  $$
  {{\lim_{x\to+\infty}x^P\times a^{\frac{1}{x+1}}(a^{\frac{1}{x(x+1)}}-1)}\Rightarrow x^P\times a^{\frac{1}{x+1}}\times \dfrac{lna}{x(x+1)}}
  $$

---

* 关于 $\infty-\infty$ 的极限, 一般有两种做法, 第一种是通分, 二是对于通分难处理的, 可以进行上下同乘, 消除根号而从化成 $\dfrac{\infty}{\infty}$ 形式, 还有比较隐晦的一种, 即通过提取因子或者化简, 然后通过泰勒进行消项, 例:
  * ${\lim_{x\to \infty}(1-x^6)^{\frac{1}{3}}+x^2}$, 通过提取 $x^2 \Rightarrow \lim_{x\to\infty}-x^2(1-\dfrac{1}{x^3})^{\frac{1}{3}}+x^2$, 然后利用泰勒消去 $x^2$, 得到极限为 $0$[^cal1-101]

---

* 对于一个根号下面的式子(或者带着绝对值), 需要开根号(或者**加根号平方从而去掉绝对值**), 要时刻注意**值的正负**, 对于一些求极限根号下问题或者是间断点或者是渐近线等等,存在一类问题的结果是公式的最大值, 也就是说要时刻**注意着某些式子在对应条件下的值的大小**, 可以会造成不同的答案.
  * $lim_{n\to\infty}\dfrac{1-x^{2n}}{1+x^{2n}}$, 要注意 $x$ 在 $\lt 1,\,1,\,\gt 1$ 的情况下的不同取值[^cal1-104]
  * $lim_{n\to\infty}x^{2n+1},\,\,x^{2n},\,\,x^n$ 在 $(1,\, -1,\, \gt 1,\, \lt 1)$ 造成的不同结果, 极限值应该不会一样, 所以要写分段函数, **这个一定要注意!** 类$660_{19}$

---

<!-- added-heading -->

### 参数方程、极值与微分定义 {#parametric-curves-and-extrema}

* 对于参数方程求导, 一定要记住

  $$
  y=f(x),\,\,\, \begin{cases} x=\phi(t) \\y=\psi(t)  \end{cases}
  $$

  * 注意: **只有在均为 $t$ 的函数下**, $\frac{dx}{dy}$ 才等于 $\dfrac{\frac{dy}{dt}}{\frac{dx}{dt}}$, 如果出现类似 $te^{y}+y+1=0$ 的情况是不可以直接求导的, 要先求出 $\frac{dy}{dt}$ (即对 $t$ 求一次导, $y$ 也是关于 $t$ 的函数), 然后再求.[^cal1-108]
  * 对于讨论 $f(x)$ 的连续, 可导性质
    * 对于可导性, 可以变换成 $y=f(x)$ 来求解, 也可以讨论 $\frac{dy}{dx}$ 得出可导性
    * 对于连续性, 同上的, 一样可以换成 $f(x)$ 来讨论连续性, 若可导则一定连续了, 还有一种方法可以讨论连续性, 就是**讨论 $\phi(t),\,\,\,\psi(t)$ 是否连续**, 如果这两个函数均连续, 那么 $f(x)$ 自然连续[^cal1-111]
  * 若要求斜渐近线, 则需要找到 $x\to\infty$ 的 $t$, 利用 $\dfrac{y(t)}{x(t)}$ 求出 $a$, 然后利用 $y(t)-ax(t)=b$. ($880_{base}-2.2.11$)

---

* 对于两个根式的比较, 可以通过两边乘上 $x$ 次方
  * 比较 $2^{\frac{1}{2}},\,\,\,3^{\frac{1}{3}}$
    两边直接乘三次或者六次即可得出结果

---

* 对于 $f(x)$, 若存在 $f'(x) \geq 0$ 且等号只在有限个点成立, 就为严格单调增函数, 此时 $f'(x) = 0$ 并不代表**不递增而是表示增的没有 $x \to x_0$ 快**, 所以此时就引出了一个概念, 如果说一个 $f'(x_0)=0$ 且在 $x_0$ 处取最大值, 那么一定会有 $f''(x_0)\leq 0$ 因为此时我们只能得出一个广泛的概念, 就是说 $f'(x_0)$ 一定是不能是递增的, 那么根据之前的说法, 我们只需要 $f'(x_0)$ 递减即 $f''(x_0) \leq 0$ 在有限个点取等号, $f''(x_0) = 0$ **不能得出无变化**[^cal1-118]

---

* 对于 $f(x)$ 的极值与最值问题, 观察**是否存在不可导点是否是极值点**(极值点和拐点并不依赖于函数在此点可导, 但是必须连续)[^cal1-120]
  * 对于不可导的地方, 观察是否两边的导数值 $f'(x_{0}^{+})\times f'(x_{0}^{-}) \lt  0$, 这是满足驻点的条件, 同样的观察其二阶导, 这是满足拐点的条件. 若是给出二阶导数图像, 则需要观察是否存在 $x_0$ 使得两边异号[^cal1-121]
  * 对于可导的地方, 观察 $f'(x) = 0$ 得出其可能存在的驻点, 观察 $f''(x)$ 是否不等零, 若是则是驻点. 观察 $f''(x) = 0$ 得出可能存在的拐点, 然后通过 $f'''(x) \neq 0$ 验证[^cal1-122]
  * 若函数在该区间内部可导且只有一个极值, 则该极值为对应的最值, 否则函数最值在区间端点处或各个极大值处的最大值
  * 求出若干驻点极值点和不可导点的值之后, 还需要求出区间端点的值(包括 ${lim_{\infty}f(x)}$), 然后去比较各个值求出最值
  * 若已知函数只有一个极值, 不妨设此时为极大值, 则最小值一定取在端点处, 反之同理. 可用于证明题[^cal1-125]
  * 若是个分段函数, 若我们已知函数在区间可导或者分段点无定义点可导的时候, 求导数时一定要注意**分段点和无定义点**, 导数也应该进行分段, 在断点处用定义去求导数[^cal1-126]

---

* 若 $a$ 是 $f(x) = 0$ 的 $m(m\geq1)$ 重根, 则 $a$ 是 $f'(x) = 0$ 的 $m-1$ 重根[^cal1-128]

---

* 若题设条件出现类似于微分定义的形式即: $f(x+C)-f(x)=AC+o(C)$, $C$ 为任意数, 所以 $C$ 可作为 $\Delta x$, $A=f'(x)$.[^cal1-130]
  * 函数 $f(x)$ 对任意 $x,\, y$ 有: $f(x+y)-f(x)=(f(x)-1)y+o(y)$, 求 $f(x)$
    * 直接利用微分定义: $d(f(x))=(f(x)-1)dx+o(\Delta x)$, 两边积分求解[^cal1-132]

---

## 中值定理 {#calculus-h-02}

---

<!-- added-heading -->

### 逆用求导公式构造辅助函数 {#auxiliary-functions}

* **构造辅助函数进行证明, 常用的辅助函数如下**
  公式：$(uv)' = u'v + uv'$，逆用方法如下：
  * $[f(x)f'(x)]' = [f^2(x)]' = 2f(x) \cdot f'(x)$, 见到 $f(x)f'(x)$，令 $F(x) = f^2(x)$[^cal1-138]
  - $[f(x) \cdot f'(x)]' = [f'(x)]^2 + f(x)f''(x)$, 见到 $[f'(x)]^2 + f(x)f''(x)$，令 $F(x) = f(x)f'(x)$
  - $[f(x)e^{\varphi(x)}]' = [f'(x) + f(x)\varphi'(x)]e^{\varphi(x)}$, 见到 $f'(x) + f(x)\varphi'(x)$，令 $F(x) = f(x)e^{\varphi(x)}$
    - $\varphi(x) = x \Rightarrow$ 见到 $f'(x) + f(x)$，令 $F(x) = f(x)e^x$
    - $\varphi(x) = -x \Rightarrow$ 见到 $f'(x) - f(x)$，令 $F(x) = f(x)e^{-x}$
    - $\varphi(x) = kx \Rightarrow$ 见到 $f'(x) + kf(x)$，令 $F(x) = f(x)e^{kx}$
  注：$(uv)'' = u''v + 2u'v' + uv''$ 亦可能考

---

* 公式：$\left(\frac{u}{v}\right)' = \frac{u'v - uv'}{v^2}$，逆用方法如下：
  - $\left[\frac{f(x)}{x}\right]' = \frac{f'(x)x - f(x)}{x^2}$
    见到 $f'(x)x - f(x)$（$x \neq 0$），令 $F(x) = \frac{f(x)}{x}$。
  - $\left[\frac{f'(x)}{f(x)}\right]' = \frac{f''(x)f(x) - [f'(x)]^2}{f^2(x)}$, 见到 $f''(x)f(x) - [f'(x)]^2$（$f(x) \neq 0$），令 $F(x) = \frac{f'(x)}{f(x)}$
  - $(\ln f(x))' = \frac{f'(x)}{f(x)}, \quad \left(\frac{f'(x)}{f(x)}\right)' = \frac{f''(x)f(x) - [f'(x)]^2}{f^2(x)}$, 见到 $f''(x)f(x) - [f'(x)]^2$$(f(x) \gt  0)$，令 $F(x) = \ln f(x)$。

---

<!-- added-heading -->

### 从端点、泰勒余项和隐藏点入手 {#mean-value-proof-patterns}

* 若题目中要求证明的命题里含有 $a+b$ 或者 $a-b$, 不妨 $Lagrange$ 中值定理, $a+b$ 的出现需要构造一个抽象函数 $f(x)$ 然后若可以用 $Lagrange$ 且 $x\neq0$ 则可以使用柯西中值: $\dfrac{f'(\xi)}{2\xi}\times \dfrac{1}{f'(\eta)}\,=\,\dfrac{f(a)-f(b)}{a^2-b^2}\times\dfrac{a-b}{f(a)-f(b)}$

---

* 对于可用泰勒简化的难证明题, 如证明 $f(x) \leq \dfrac{1}{n!}g(x)$ 或者与 $f^{n(n\geq2)}$, 可以考虑 $Lagrange$ 余项公式证明
  * 证 $|\dfrac{sinx}{x}-1|\,\leq\,\dfrac{1}{2}|x|$
    把 $sinx$ 换成在 $x_0=0$ 处的 $lagrange$ 余项, 于是有

    $$
    sin0+(sinx)'|_{x=0}*x+\dfrac{(sinx)''|_{x=\xi}}{2!}*x^2=x-\dfrac{x^2sin\xi}{2},\,\,\xi\in(0,x)
    $$

    这样就有 $|-\dfrac{xsin\xi}{2}|$ 自然就证出来了[^cal1-156]

---

* 关于中值定理的证明题, 有时候找一些隐藏的点, 只需要其存在就可证明
  * 非常数函数 $f(x)$ 在 $[a,\,b]$ 可导, $f(a)=f(b)=0$, 证: $f'(\xi)\gt 0(\lt 0)$
    不妨设一点 $c$ 有 $a\lt c\lt b$, $f'(\xi_1)=\dfrac{f(a)-f(c)}{a-c}$,$f'(\xi_2)=\dfrac{f(b)-f(c)}{b-c}$, 若 $f'(\xi_1)\gt 0(\lt 0)$ 则 $f'{\xi_2}\lt 0(\gt 0)$, 得证[^cal1-160]

---

## 积分 {#calculus-h-03}

---

<!-- added-heading -->

### 原函数、可积性和积分中值定理 {#antiderivatives-and-integrability}

* 关于不定积分原函数 $F(x)$ 和 导函数 $f(x)$, 我们知道以下几点
  1. 对于变限积分 $F(x)=\int_{a}^{x}f(t)\,dt$, 存在即连续
  2. **积分中值定理的证明**
     * $m(b-a)\,dx\,\leq\,\int_{a}^{b}f(x)\,dx\,\leq\,M(b-a)$, 则由介值定理可得, 存在 $\xi\in[a, b]$ 使得 $\int_{a}^{b}f(x)\,dx=f(\xi)(b-a)$[^cal1-168]
     * 若证存在 $\eta\in(a,b)$ 使得 $\int_{a}^{b}f(x)\,dx=f(\eta)(b-a)$, 可考虑使用拉格朗日证明: 由于 $f(x)$ 连续, 则有 $F(x)=\int_{a}^{x}f(t)\,dt$, 有 $F(b)-F(a)=f(\eta)(b-a)\,\Rightarrow\,\int_{a}^{b}f(x)\,dx-0=f(\eta)(b-a),\,\,\eta\in(a,b)$
     * **积分中值定理的加权形式**:
       * $f(x),\,\,g(x)$ 在区间 $[a,b]$ 连续且 $g(x)$ 不变号, 则有:

         $$
         \int_{a}^{b}f(t)g(t)dt=f(\xi)\int_{a}^{b}g(t)dt,\,\,\xi\in[a,b]
         $$

     * 在证明过程中, 可将常量变量化, 然后通过积分中值定理以及单调性证明不等式, 如:
       * $f(x)$ 单调增, 证 ${\int_{a}^{b}xf(x)dx \geq \dfrac{a+b}{2}\int_{a}^{b}f(x)dx}$
         * 尝试常量变量化构造 ${F(x)={\int_{a}^{x}tf(t)dt - \dfrac{a+x}{2}\int_{a}^{t}f(t)dt}}$, 通过求导得到如下式子, 然后利用单调即可得证

           $$
           {F'(x)=\dfrac{x-a}{2}f(x)-\dfrac{1}{2}\int_{a}^{x}f(t)dt(\Rightarrow \dfrac{x-a}{2}f(\xi)),\,a\lt \xi\lt x}
           $$

           [^cal1-174]
  3. 连续函数 $f(x)$ 必有原函数 $F(x)$
     证: $f(x)$ 在 $[a, b]$ 连续, 有 $x\in(a,b),\,\, x+\Delta x\in(a, b)$, $\Delta F\,=\,F(x+\Delta x)-F(x)=\int_{a}^{x+\Delta x}f(t)\,dt-\int_{a}^{x}f(t)\,dt\,=\,\int_{x}^{x+\Delta x}f(t)\,dt\,=\,f(\xi)(\Delta x),\,\xi\in(x, x+\Delta x)$
     又 $F'(x)\,=\,\lim_{\Delta x \to0}\dfrac{F(x+\Delta x)-F(x)}{\Delta x}$ 则洛必达之后, 由于 $\Delta x \to 0 \Rightarrow \xi\to x \Rightarrow \lim_{\xi\to x}f(\xi)=f(x)$, 得证[^cal1-177]
  4. 若函数 $f(x)$ 有第一类间断点或无穷间断点, 则没有原函数 $F(x)$
  5. 对于导函数 $f(x)$, 若存在, 则原函数一定在定义域内处处可导处处连续, 则求原函数 $F(x)$ 的时候要注意可能间断的地方($f(x)$是分段函数)要用 $\int f(x)+C$ 在原函数的地方连接起来
  6. 求解不定积分的时候一定要 **$+C$**

---

* 关于定积分
  * **函数 $f(x)$ 可积意味着 $|f(x)|\leq M$**
  * 注意定积分的可加性, 例如求: ${\lim_{n\to\infty}\sum_{k=1}^{n}\int_{k}^{k + 1}f(x)}$, 注意求和符号里面的信息, 模拟几次发现: ${\int_{k}^{k + 1}+\int_{k + 1}^{k + 2}+\dots=\lim_{n\to+\infty}\int_{1}^{n + 1}f(x)=\int_{1}^{+\infty}f(x)dx}$
  * 对于定积分的几何意义, 我们有: 选择的区间一小块中的任意函数值都可以作为积分的数值, 只是为了方便来讲我们把它设置成第 $i$ 的函数值即 $f(\frac{i}{n})$ 但是可以取中间值即 $f(\frac{2i-1}{2n})$ 或者 其他值, 其通项公式是

    $$
    \int_{a}^{b} f(x) \,dx \,= \, \lim_{n \to \infty} \sum_{i=1}^{n}\,f(a+\frac{b-a}{n}i)\frac{b-a}{n}
    $$

    注意这里的公式, 我们不仅将其 $n$ 等分, 还可以 $2n, 3n\dots$, 但是定义还是不会变的, 那就是我们一定要有 $\frac{b-a}{n}i$ 对应 $\frac{b-a}{n}$

    <figure class="fig"><img src="/blog/kaoyan-math/figures/riemann-sum.png" alt="原笔记配图 2" width="636" height="205" loading="lazy" decoding="async"><figcaption>原笔记配图 2</figcaption></figure>

  * 若函数 $f(x)$ 具有有限个间断点(**除无穷间断点**), 则存在 $\int_{a}^{b}f(x)\,dx$ 存在[^cal1-186]
  * 定积分存在的充分条件
    1. $f(x)$ 在区间 $I$ 单调
    2. $f(x)$ 在区间 $I$ 连续
       1. $f(x)$ 在区间 $I$ 有界, 且只有有限个间断点
  * 定积分存在的必要条件
    1. $f(x)$ 在区间 $I$ 有界
    2. 积分区间长度有限
  * 对于类似比大小的题目, 要从以下方面探究(不全, 但是能想到的基本都会写上去):
    * 观察是否可以用奇函数简化, 或者拆分之后简化部分, 如:

      $$
      \int_{-\frac{1}{2}}^{\frac{1}{2}}\dfrac{x^2+x+1}{x^2+1}
      $$

      我们直接拆开然后会发现是个常数和奇函数, 此时就是 $1$
    * 使用放缩, 如比较 $M=\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}}sin^2xcosx\,dx$ 和 $N=\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}}sin^3x-cosx\,dx$, 可以把 $M$ 的 $cosx$ 换成 $sinx$, 然后进行相减, 由于在定义区间我们有 $cosx\gt sinx$[^cal1-196]
    * 讨论函数性质, 对于确定的分母部分不需要讨论, 可简化讨论步骤, 如 $\displaystyle\int_{0}^{1}\dfrac{(1+x)ln^2(1+x)}{x^2}\,dx$, 可直接讨论分子部分

---

<!-- added-heading -->

### 变限积分与反常积分 {#variable-limit-integrals}

* 关于变限积分
  * 变限积分 $\int_{a}^{x}f(t)\,dt$ 是关于 $x$ 的函数
  * 若 $f(x)$ 可积, 则 $F(x) = \int_{a}^{x}f(t)\,dt$ 连续, 这里注意 $F(x)$ 不一定是其原函数, 且 $f(x)$ 不一定连续, 有有限个间断点也可积, 即**变限积分若存在必然连续**, 证明如下:
    - $f(x)$ 可积, 则 $|f(x)|\leq M$, 有

      $$
      F(x+\Delta x)-F(x)=\int_{x}^{x+\Delta x}f(t)\,dt\leq M\Delta x
      $$

       取极限有 $\lim_{\Delta x \to 0}F(x+\Delta x)-F(x)=\lim_{\Delta x \to 0}M\Delta x = 0$, 即      $\lim_{\Delta x \to 0} F(x+\Delta x) =  F(x)$, 得证[^cal1-202]
  * 若 $f(x)$ 有跳跃间断点, 则 $F(x)$ 在间断点处不可导, 且对应的导数对应 $f(x)$ 的极限
  * 若 $f(x)$ 有可去间断点, 则 $F(x)$ 在间断点处可导且$F'(x_0)=\lim_{x\to x}f(x)$[^cal1-204]
  * 对于抽象函数变限积分求导, 类 $\int_{F_2(x)}^{F_1(x)}f(g(x)-h(t))dt$ , 对其求导, 需要先把里面的 $g(x)-h(t)$ 换元方便求导, 类$(888_{comp}(3.3.8))$
    * $\int_{0}^{x^2}tf(x^2-t^2)dt$ 的导数, 我们按照平常积分求出原函数, 然后再进行上下限相减, 这样是可行的, 但是更为简便的是 $\to$ 令 $u=x^2-t^2$, 然后就得到了 $\frac{1}{2}\int_{0}^{x^2}f(u)du$, 简洁了很多.[^cal1-206]
  * ${f(x)}$ 为奇函数, 则一定有 $\int_{a}^{x}f(t)dt$ 为偶函数, 若 $f(x)$ 为偶函数, 则若 **$a\neq0$ 或者 $F(a)\neq0$**, 则推不出 $\int_{a}^{x}f(t)dt$ 为奇函数
  * 对于反常积分的无穷区间敛散性判断, 类 $\int_{-\infty}^{\infty}e^{|x|}sinx$, 要注意这时奇函数的特性失效, 因为该函数在一端是发散的, 就有两个发散相加减仍然是发散的
  * 对于变限积分一类的求极限问题, 若被积函数 ${f(x)}$ 与 ${g(x)}$ 同敛散, 则可等价代换, 或在极限处对 ${f(x)}$ 进行泰勒展开, 然后逐项积分. 即: ${\lim_{x\to 0}\int_{0}^{x}f(x)dx=\lim_{x\to 0}\int_{0}^{x}g(x)dx}$[^cal1-209]

---

<!-- added-heading -->

### 根号符号、分部积分与区间对称 {#integration-signs-and-substitution}

* 关于积分的计算问题
  * **时刻关注开根号的正负**, 由于被积函数定义域的限制, 若需要将 $x$ 乘进根号或将其开出来, 若此时 $x$ 的值为负数则需要添负号
    * 曲线过 $(-2, 0)$ 且任意点的斜率为: ${\dfrac{1}{x\sqrt{x^2-1}}}$, 求曲线
      * 由于根号内部非负, 则得到 $x$ 的两个范围, 然后利用曲线过一点, 确定 $x$ 的具体范围, 然后求不定积分, 令 $x=sect$, 得:

        $$
        {\dfrac{1}{x\sqrt{x^2-1}}dx \Rightarrow \int\dfrac{sect\,tant}{sec\,(-tant)}dt=-arccos(\frac{1}{x})+C}
        $$

        **一定注意这里的 $-tant$, 由上述过程得到的 $x$ 的范围得出$tant\lt 0$ 也就是要: $\sqrt{tan^2t}=-tant$, 不仅仅是三角函数, 也适用于 $x$**
    * 求 ${\displaystyle\int_{-\tfrac{\pi}{2}}^{\tfrac{\pi}{2}}\int_{0}^{Rcos\theta}\dfrac{r}{\sqrt{R^2-r^2}}\,dr}$
      * 对被积函数求不定积分是简单的, 得到 ${R-Rsin\theta}$, 但是**观察外侧 ${\theta}$ 的值发现存在负数, 此时变为: ${R-R|sin\theta|}$**, 然后求积分即可
  * 拆开使用分部积分抵消, 这类方法需要先观察, 如果存在对拆开部分求导之后出现后面的部分则可以使用, 一般出现的形式为: ${e^{f(x)}sinx/cosx/tanx\dots}$, 大多数都带有三角函数, 例如出现 ${(1+tanx)^2=sec^2x+tanx}$, 发现前一项是后一项的导数, 于是尝试消去. $(张_{base}(9.5, \,\,T_{9.11}),\,880_{base}(3.3.4.6),\,880_{comp}(3.3.3))$[^cal1-218]
  * 若出现抽象积分如 $\int x^{n-1}\sqrt{1+x^n}dx$, 看到 $n-1$ 与 $n$, **想到凑微分**
  * 直接使用换元后的 $f(t)dg(t)$ 分部积分成 $g(t)df(t)$, 例($张_{base}9.7$)
  * 区间再现, 对于 $f(x)g(sinx),\,\,f(x)g(cosx)$, 有时出现 ${f(x)e^x}$ 时, 若难以积分, 可尝试性的考虑能否区间再现 类($张_{base}9.18,\,\,9.12$,${武_{comp}P_{115}-9}$)
  * 对于 $\dfrac{1}{1+sinx},\,\,\dfrac{1}{1+cosx}$ 等等, 要常用分子分母有理化, 上下同乘
    * 例: $\int\dfrac{x}{1+sinx}$, 直接有理化分母然后见到 $xf(sinx/tanx/cosx)$ 进行区间再现即可

<!-- added-heading -->

### 反三角函数分支与三角积分 {#inverse-trigonometric-branches}

* 对于积分换元的时候, 要注意反函数的区间是否要断开, 如果类似与 $arcsinsinx,\,\,arctantanx$, 需要**注意范围**, 只需要看函数是否单调即可
  * 求积分:

    $$
    {\int_{0}^{1}x\,arcsin(2\sqrt{x-x^2})\,dx}
    $$

    * 类似 ${\sqrt{x-x^2}}$, 令 ${x=sin^2\theta}$, 有:

      $$
      {\int_{0}^{\frac{\pi}{2}}sin^2\theta\cdot arcsin(sin2\theta)\cdot2sin\theta\cdot cos\theta\,d\theta}
      $$

      由于: $\arcsin(\sin 2\theta) =\begin{cases}2\theta, & 0 \leq \theta \leq \frac{\pi}{4} \\\pi - 2\theta, & \frac{\pi}{4} \leq \theta \leq \frac{\pi}{2}\end{cases}$, 有:

      $$
      {I = \int_0^{\frac{\pi}{4}} 2 \sin^3 \theta \cos \theta (2\theta) \, d\theta + \int_{\frac{\pi}{4}}^{\frac{\pi}{2}} 2 \sin^3 \theta \cos \theta (\pi - 2\theta) \, d\theta}
      $$

      此时: ${\displaystyle I_1=\int_{0}^{\frac{\pi}{4}}\theta\,dsin^4\theta,\,I_2=\int_{0}^{\frac{\pi}{4}}(\dfrac{\pi}{2}-\theta)\,dsin^4\theta}$, 分部积分即可[^cal1-228]
  * 这里罗列一些常用的反三角函数公式:
    * ${\begin{align} \arctan x+\arctan y&=\arctan\frac{x+y}{1-xy}\quad(xy\lt 1)\\ &=\pi+\arctan\frac{x+y}{1-xy}\quad(x\gt 0,xy\gt 1)\\ &=-\pi+\arctan\frac{x+y}{1-xy}\quad(x\lt 0,xy\gt 1)\\ \end{align}}$
    * ${\begin{align} \arctan x-\arctan y&=\arctan\frac{x-y}{1+xy}\quad(xy\gt -1)\\ &=\pi+\arctan\frac{x-y}{1+xy}\quad(x\gt 0,xy\lt -1)\\ &=-\pi+\arctan\frac{x-y}{1+xy}\quad(x\lt 0,xy\lt -1)\\ \end{align}}$
    * 当 ${xy = 1}$ 且 ${x \ne 0}$， ${\arctan x + \arctan \dfrac{1}{x} = \begin{cases} \dfrac{\pi}{2}, & x \gt  0 \\\\-\dfrac{\pi}{2}, & x \lt  0\end{cases}}$
    * 当 ${xy = -1}$ 且 ${x \ne 0}$，有：${{\arctan x - \arctan \left( -\dfrac{1}{x} \right) = \arctan x + \arctan \dfrac{1}{x} = \begin{cases} \dfrac{\pi}{2}, & x \gt  0 \\\\ -\dfrac{\pi}{2}, & x \lt  0 \end{cases}}}$
* 注意利用奇函数和偶函数积分性质, 必要时将式子拆开
* 对于 $\dfrac{1}{a^2+x^2}dx$, 一定要把它抽象化, 类 $\dfrac{1}{x^2-x+1}\rightarrow \dfrac{1}{(x-\frac{1}{2})^2+(\frac{\sqrt{3}}{2})^2}$, 注意这里一定要凑出 $d(x-\frac{1}{2})$, 类似的 $dAx$ 也要凑出常数项
* 对于 $R(-sinx,\,cosx)=-R(sinx,\,cosx)\dots$, 这类一定要学会换元和凑微分, 还要记住万能公式$(P_{289})$, 类($武_{base}P_{81}-13、14$)
  * 见到 $R(-sinx,\,-cosx)=-R(sinx,\,cosx)$, 令 $u=tanx$ 即可($1k_{base}14.20$)[^cal1-237]
* 对于 ${sinax \times cosbx}$ 要考虑积化和差, 同样的也可以逆用和差化积
  * ${\int sinx \times cosnx \,dx}={\int sin(1+n)x+sin(1-n)xdx}$[^cal1-239]
* 关于 $e^{ax}cos/sinbx$, 可直接套用公式$(张_{base}{285})$, 类($张_{base}(10.4)$)
* 若出现 ${\int\dfrac{asinx + bcosx}{csinx + dcosx}}$, 这类要进行配方凑微分, 解方程 ${\begin{cases}Ac-Bd=a \\ Ad+Bc =b \end{cases}}$ 求出 ${A,\,B}$ 配成 ${\dfrac{A(c\,sinx+d\,cosx)+B(c\,cosx-d\,sinx)}{c\,sinx+d\,cosx}}$, 然后利用 ${d(ln(csinx+dcosx))}$ 凑微分, 例(李范(3.40), $880_{base}(3.3.3,\,3.3.4)$)[^cal1-241]

<!-- added-heading -->

### 降阶、面积与特殊函数 {#area-beta-gamma}

* 对于部分出现变限积分 $f(x)\int_{a}^{x}g(t)dt$ 或者多阶导数 $f^{n}$, 可以尝试使用分部积分对其进行降阶, 从而得到解法($张_{base}11.6,\,1k_{base}{10.7}$)
* 要熟悉部分三角函数, 产生条件反射, 例如 ${(1+tanx)^2=sec^2x+tanx}$[^cal1-243]
* 若题目要求计算该函数的面积, 由于面积不为负, 则需要**注意函数在特定区间的正负性, 必要时通过定积分定义取极限计算**
  * 很好的例子就是 $y=e^{-x}sinx$, 我们要计算出一段区间的面积, 然后通过定积分定义进行, 即

    $$
    \lim_{n\to \infty}\sum_{k=0}^{n}\int_{k\pi}^{(k+1)\pi}e^{-x}|sinx|dx
    $$

     **不要觉得这是理所应当的, 要时刻注意着时刻关注着函数的正负**
* 注意积分${\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy}$, 此积分令 $F(x)=\int_{x}^{b}f(x)dx$, 然后去凑微分

  $$
  {\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy=\int_{a}^{b}[\int_{x}^{b}f(y)dy]\,d\,[-\int_{x}^{b}f(y)dy]}
  $$

* Beta函数和Gamma函数:
  * 形如: ${\displaystyle \int_{0}^{+\infty}x^{\alpha-1}e^{-x}dx,\,2\int_{0}^{+\infty}t^{2\alpha-1}e^{-t^2}dt}$, 考虑使用 ${\Gamma}$ 函数, 一些推导: ${\Gamma(\alpha+1)=\alpha\Gamma(\alpha),\,\Gamma(n+1)=n!,\,\Gamma(\dfrac{1}{2})=\sqrt{\pi}}$
  * 形如: ${\displaystyle B(p,q) = \int_0^{\infty} \dfrac{t^{p-1}}{(1+t)^{p+q}}dt,\int_0^1 u^{p-1}(1-u)^{q-1}(p\gt 0,\, q\gt 0)}$, 考虑使用 Beta 函数, 观察 Gamma 函数发现: ${B(p,q) = \dfrac{\Gamma(p)\Gamma(q)}{\Gamma(p+q)}}$, 一般此类出现在三角函数替换中[^cal1-249]
    * 推广: ${\displaystyle \int_0^{+\infty} \frac{x^{p-1}}{1 + x^q} \, \mathrm{d}x = \frac{\pi}{q\,\sin\left( \frac{p\pi}{q} \right)}, \quad 0 \lt  p \lt  q}$
    * 求:

      $$
      {\int_{0}^{\frac{\pi}{2}}\dfrac{sin^2x\,cos^2x}{(sinx+cosx)^6} dx}
      $$

      见到 $R(-sinx,\,-cosx)=-R(sinx,\,cosx)$, 令 $u=tanx$, 有:

      $$
      {\int_0^{\infty} \dfrac{t^2}{(1+t)^6}dt=B(3,3) = \dfrac{\Gamma(3)\Gamma(3)}{\Gamma(6)} = \dfrac{2! \cdot 2!}{5!} = \dfrac{4}{120} = \dfrac{1}{30}}
      $$

      [^cal1-252]

<!-- added-heading -->

### 倒数代换与含参数积分 {#reciprocal-and-parameter-integrals}

* 注意一类积分: ${\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}$, 从图形上理解, 由于其积分与 ${{\displaystyle \int_{0}^{+\infty} \dfrac{1}{1+x^4}}}$ 关于 ${x=1}$ 处镜像对称, 故两者只是把图形反转, 面积不变, 即: ${{\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}={\displaystyle \int_{0}^{+\infty} \dfrac{1}{1+x^4}}}$, 至此, 要求 ${{\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}}$ 即求: ${{\displaystyle \dfrac{1}{2} \int_{0}^{+\infty}\dfrac{1+x^2}{1+x^4}}}$, 对于 ${\displaystyle \int_{0}^{+\infty}\dfrac{1+x^2}{1+x^4+ax^2}}$ 这类积分, 可上下同除 ${x^2}$, 凑 ${d(x+\frac{1}{x}),\,d(x-\frac{1}{x})}$ 即可[^cal1-253]
  * 求积分: ${\displaystyle \int_{0}^{+\infty}\dfrac{1}{1+x^6}}$(${武_{comp}P_{102}-注}$)
    *

      $$
      \begin{aligned}\int \frac{1}{1+(x^2)^3}\,dx&= \int \frac{1}{(1+x^2)(1 - x^2 + x^4)}\,dx\\&= \int \frac{(1+x^2)-x^2}{(1+x^2)(1 - x^2 + x^4)}\,dx\\&= \int\Bigl(\frac{1}{1 - x^2 + x^4} - \frac{x^2}{1 + x^6}\Bigr)\,dx\\&= \int \frac{1}{1 - x^2 + x^4}\,dx \;-\; \int \frac{x^2}{1 + x^6}\,dx\\&= \tfrac12\int \frac{1 + x^2 - x^2 + 1}{1 - x^2 + x^4}\,dx \;-\; \tfrac13\int \frac{1}{1+(x^3)^2}\,d(x^3)\\&= \tfrac12\int \frac{1 + x^2}{1 - x^2 + x^4}\,dx \;+\; \tfrac12\int \frac{1 - x^2}{1 - x^2 + x^4}\,dx \;-\; \tfrac13\int \frac{1}{1+(x^3)^2}\,d(x^3)\\&= \tfrac12\int \frac{\tfrac1{x^2}+1}{\tfrac1{x^2}-1+x^2}\,dx \;+\; \tfrac12\int \frac{\tfrac1{x^2}-1}{\tfrac1{x^2}-1+x^2}\,dx \;-\; \tfrac13\arctan(x^3)\\&= -\tfrac12\int \frac{1}{\bigl(\tfrac1x-x\bigr)^2+1}\,d\Bigl(\tfrac1x - x\Bigr)\;-\;\tfrac12\int \frac{1}{\bigl(\tfrac1x+x\bigr)^2+3}\,d\Bigl(\tfrac1x + x\Bigr)\;-\;\tfrac13\arctan(x^3)\\&= -\tfrac12\arctan\!\Bigl(\tfrac1x - x\Bigr)\;-\;\tfrac1{2\sqrt3}\ln\!\Bigl|\frac{\tfrac1x + x - \sqrt3}{\tfrac1x + x + \sqrt3}\Bigr|\;-\;\tfrac13\arctan(x^3)+C\\\end{aligned}
      $$

      [^cal1-255]
  * 这类积分当然可以用 ${Beta}$ 函数的推广形式
* 求积分 ${\displaystyle \int_{0}^{1}\dfrac{x^t}{lnx}dx}$, 这类积分需要运用费曼思想, 在积分号内部对 ${t}$ 求导, 得到 ${\displaystyle \int_{0}^{1}\dfrac{lnx\,x^t}{lnx}dx}$, 此时对 ${x}$ 的积分容易, 得到 ${\left. \dfrac{x^{t+1}}{t+1} \right|_{0}^{1}=t+1}$, 由于我们这是对 ${t}$ 求导之后的, 所以要再对 ${t}$ 积回来 ${ln(1+t)}$, 即为答案[^cal1-257]
  * 求积分 ${\displaystyle \int_{0}^{1}\dfrac{x^7-x^3}{lnx}dx}$
    * 直接利用公式得 ${ln(1+7)-ln(1+3)=ln2}$

<!-- added-heading -->

### 逆用格林公式处理圆域 {#green-formula-on-disks}

* 有时候积分比较难做, 且此时涉及到圆域以及一二阶导数的性质, 可以考虑逆用格林公式或者高斯公式求解, 将面积分化作线积分, 然后利用题目条件化简, 从而计算出结果
  * ${f(x,\,y)}$ 在区域 ${D(x,\,y)|x^2+y^2\leq1}$ 上二阶偏导连续, 且满足 ${(f''_{xx}+f''_{yy})e^{x^2+y^2}=1}$, 求 ${\displaystyle \iint_D\,(xf'_x+yf'_y)\,dxdy}$
    * 充分利用题设条件, 明显的转到圆区域更好利用极坐标, 则用极坐标表示原积分 ${\displaystyle I=\int_{0}^{\tfrac{\pi}{2}}\,d\theta\int_{0}^{1}(rcos\theta f'_x+rsin\theta f'_y)\,r\,dr}$, 利用曲线面积分的反参数法: ${-rsin\theta\,d\theta=dx}$, 将原积分改写成 ${\displaystyle I=\int_{0}^{1}\,r\,dr\int_{0}^{\tfrac{\pi}{2}}(rcos\theta f'_x+rsin\theta f'_y)\,\,d\theta=\int_{0}^{1}\,r\,dr\oint_{L_r}\,(-f'_y+f'_x)d\theta}$, ${L_r}$ 是区域 ${D}$ 的边界. 故用格林公式得到 ${\displaystyle I=\int_{0}^{1}\,[\iint_D\,f''_{xx}+f''_{yy}\,dxdy]\,r\,dr}$, 这个积分利用原题的条件代入, 然后利用极坐标求出积分值 ${\dfrac{\pi}{2e}}$[^cal1-262]

## 多元微分 {#calculus-h-04}

---

<!-- added-heading -->

### 连续、偏导与可微 {#continuity-partials-differentiability}

* 多元函数相关概念
  * 由多元函数连续定义可知, 连续 $\Rightarrow$ 极限存在且等于函数值, 反之不一定成立(无需深究)[^cal2-266]
  * 函数在某点连续无法推出在该点的偏导数存在, 无法推出该点可微
  * 函数在某点偏导数存在只能意味着函数在其对应的坐标轴方向的变化率存在且相等, 但并不保证任意路径相等, 也就是无法保证在所有方向的变化率均相等即全微分(梯度)存在, **即偏导存在无法推出可微, 可微一定推出偏导, 若偏导数连续(即任意路径极限存在)则有在该点可微, 但可微性只保证存在线性逼近**，但这个线性逼近的斜率（偏导数）可能在该点处是存在的而不连续。故可微无法推出偏导数连续, 一个经典反例:[^cal2-268]

    $$
    f(x,y)=\begin{cases} \frac{x^2y}{x^4+y^2}, & (x,y)\neq (0,0) \\ 0, & (0,0) \end{cases},\,原点可微, 但偏导数在原点处并不连续
    $$

    [^cal2-269]
    * 若 ${f'_{x}(x_0,\,y_0),\,f'_{y}(x_0,\,y_0)}$ 存在, 求可以推出的部分:
      * 一个点处的信息无法推出一个圆域内的信息, 即:偏导存在无法推出该点连续, 无法推出该点极限存在, 无法推出在该点的邻域内有定义
      * 从表达式出发: ${f'_x(x_0,\,y_0)=\lim_{x\to x_0}\dfrac{f(x,\,y_0)-f(x_0,\,y_0)}{x-x_0}}$ 存在, 则我们得到当 $y=y_0$ 时, **函数在 $x_0$ 的邻域内有定义, 同理得到 $y_0$ 邻域有定义, 但函数在该点邻域有定义指的是圆域, 两个方向不能代表圆域**
      * 当一个变量固定时, 式子变成了一元函数, 利用一元函数性质知: 若函数可导, 则函数在该点连续, 即 ${\lim_{x\to x_0}f(x,\,y_0)=f(x_0,\,y_0)}$

<!-- added-heading -->

### 隐函数与混合偏导 {#implicit-functions-mixed-partials}

* 对于隐函数存在 $F_y \neq 0$ 只是一个充分条件, 并不意味着若等于零就不存在关于 $y=y(x)$ 的隐函数, 因为 $-\dfrac{F_x}{F_y}$ 可能是个未定式, 即极限可能存在, 此时也会推出[^cal2-274]
* 关于多元函数求偏导, 若无特殊约束可直接利用定义求偏导, 但是若要求二阶或者高阶偏导, 且是混合偏导, 例 $f''_{xy}$, 则需注意第一阶的另一个变量为已知数, 即利用定义: $f'_x=\lim_{x\to x_0}\dfrac{f(x+x_0,\,y)-f(x_0,\,y)}{x}$, 然后再去求 $f''_{xy}$[^cal2-275]

---

<!-- added-heading -->

### 全微分定义与线性逼近 {#total-differential-linear-approximation}

* 多元函数在某点可微意味着: 存在

$$
\begin{align*}
\Delta z &= f(x+\Delta x,\,y+\Delta y) - f(x,y) \\
\Delta z &= A\Delta x(dx) + B\Delta y(dy) + o(\sqrt{x^2+y^2})
\end{align*}
$$

  进而可以有 ${\dfrac{(\Delta z - A\Delta x - B\Delta y) \Rightarrow o(\sqrt{x^2+y^2})}{\sqrt{x^2+y^2}}}=0$, 由可微的必要条件知: $A=\frac{\partial z}{\partial x},\,B=\frac{\partial z}{\partial y}$, 故若题目给出相关的部分, 使用相关概念求解, 如:

  $$
  \lim_{x\to a,\,y\to b}\dfrac{f(x,\,y)-f(a,\,b)+3(x-a)-4(y-b)}{o(\sqrt{(x-a)^2+(y-b)^2})}=C(C\neq0)
  $$

  [^cal2-284]
  则我们根据高阶以及等价无穷小的定义可知, 分母若变成 $\sqrt{x^2+y^2}$, 则对于上述式子: $C=0$, $\Rightarrow\,\,f(x,\,y)在(a,\,b)$ 处可微, 即有 $A=-3,\,B=4$[^cal2-285]

---

<!-- added-heading -->

### 极值与最值的求解流程 {#multivariable-extrema-workflow}

* 多元函数极值与最值
  * 关于求极值与最值的步骤:
    * 对于一个多元函数来说, 先**观察不可导点是否为驻点, 然后利用偏导求驻点**, 此时的驻点我们设为 $M_i$, 此时我们的极值就求出来了[^cal2-290]
    * 若函数存在约束, 即给出另一个方程进行约束, 首先需要**去除 $M_i$ 中的不符合约束的点**, 然后进行拉格朗日法求极值即可, 得出 $M_j$
    * 最后再代几个端点处, 即约束的端点处, 得出 $M_k$
    * **比较最终的 $M_i,\,M_j,\,M_k$ 得出最大值与最小值, $M_i$ 为极值点**, 这里需注意得到的最值点一定是极值点, 但不一定是广义的极值点, 可能是在约束边界的**条件极值点, 即若在区域内部若出现单一极值点, 该极值点不一定为最值点**, 可能区域边界也会出现极值点, 此时可能边界极值点为最值[^cal2-293]
    * 关于上述理论这里做一个补充, 给定一函数 ${f(x,\,y)}$ 和一区域 ${D}$, 求最值的方法一般可分为两步:
      1. **若区域 ${D}$ 是一个区域范围而非约束条件, 则可以先求 ${f(x,\,y)}$ 在区域内的极值, 然后代入 ${D}$ 的条件转化为一元函数 ${g(x)}$ 求出边界最值, 最后比较得到极值与最值**
      2. 若一元函数较为复杂, 难以求出最值点, 仍可构造拉格朗日求边界最值, 其实若 ${D}$ 为一个约束条件, 此时就直接构造即可
      3. 注意 ${x\geq 0, y\geq 0}$ 的边界为 ${x=0,\,y=0}$

<!-- added-heading -->

### 拉格朗日乘数与代表例题 {#lagrange-multipliers-examples}

* 在构造拉格朗日求极值的过程中, 有时会遇到方程难解的问题, 此时存在以下几种思考方向
  * 这是最常用的: 若无法通过简单的加减消去未知量, 则通过变换得到 ${\begin{cases} \phi_1(\lambda)x+\mu_1(\lambda)y=0 \\ \phi_2(\lambda)x+\mu_2(\lambda)y=0 \end{cases}}$, 然后将 ${(0,\,0)}$ 代入观察是否满足约束条件, 由于约束条件的存在, 方程一定有非零解, 则可解出 ${\lambda}$, 然后解出 ${(x,\,y)}$[^cal2-299]
  * 通过对 ${f_x',\,f'_y..}$ 进行相乘或相除简化函数, 然后相加之后利用题目约束条件简化答案
* 例:
  * 求椭圆面 ${f(x,\,y,\,z)=\dfrac{x^2}{3}+\dfrac{y^2}{2}+z^2=1}$ 被平面 ${D(x,\,y,\,z)=x+y+z=0}$ 截得的椭圆长半轴和短半轴之长
    * **此题极具代表性**, 由于截得的椭圆面一定过原点, 故其椭圆长半轴与短半轴的距离一定是距离原点最大和最小, 故转换为极值问题, 即求 ${d^2=x^2+y^2+z^2}$ 的最大最小值[^cal2-303]
    * 构造拉格朗日乘数法, 得 ${L(x,\,y,\,z)=f(x,\,y,\,z)+\lambda D(x,\,y,\,z)}$, 然后利用 ${L'_x - L'_y,\,L'_y - L'_z,\,D(x,\,y,\,z)=0}$ 会得到 ${\begin{cases} (2+2\lambda)x+(4+3\lambda)y=0 \\ (6+2\lambda)x+(-6-3\lambda)y=0 \end{cases}}$, 由于方程组一定有非零解, 则利用 ${\begin{vmatrix}2+2\lambda & 4+3\lambda \\ 6+2\lambda & -6-3\lambda \end{vmatrix}=0}$ 解出 ${\lambda}$[^cal2-304]
    * 由于解出 ${\lambda}$ 之后仍然非常难求, 但由于只要求求出长度, 并非具体的点, 则考虑在 ${L'_x,\,L'_y,\,L'_z}$ 上分别乘 ${x,\,y,\,z}$ 然后相加利用约束条件相消, 最后得到 ${d^2=-\lambda}$
  * 在定向为逆时针方向的椭圆 ${C: \dfrac{x^2}{4} + y^2 = 1}$ 上选取一段曲线𝐿，使得曲线积分 ${\int dx+2dy}$ 最大
    * 容易求得积分值 ${x+2y\,|_{A}^{B}}$, 即找到起点 ${A}$ 使得 ${x+2y}$ 最小, 终点 ${B}$ 使得其最小, 故化为拉格朗日乘数法, 约束条件为椭圆方程, 求得值为 ${4\sqrt{2}}$[^cal2-307]
* 若多元函数在区域 $D$ 内二阶偏导数存在, 则**必存在区域最值**[^cal2-308]
* $F(x,\,y)$ 类比一元函数, 若区域 $D$ 内无极值点, 即
  1. 不存在 $F'_x=0,\,F'_y=0$ 的点
  2. 在偏导数不存在的点仍不符合极值点定义
  则其**最值一定在区域 $D$ 边界**, 类比一元函数就是定义域端点处[^cal2-312]

<!-- added-heading -->

### Hessian 判别失效时的处理 {#degenerate-hessian-extrema}

* 若 $AC-B^2 =0$, 并且题目要求讨论极值, 则可以用以下方法尝试发现:
  * 讨论函数在可疑点的泰勒展开, 即高阶导数
    * 例 $f(x,\,y)=x^2+y^2-6x+10$, 其可疑点为 $(2,\,0)$, 则讨论 $f(x,\,0)$ 处的导数情况, 发现 $f^{(3)} \neq 0$, 又由一元函数定义, 极值点存在对应偶数阶导数不等零, 所以 $x=2$ 不是其极值点, 即 $(2,\,0)$ 不是其极值点, 故极值[^cal2-315]
  * 可以取特殊路径验证
    * 如 $f(x,\,y)=x^4+y^4$, 可疑点为 $(0,\,0)$, 取路径 $y=kx$, 发现 $f(x,\,kx)=x^4(1+k^4)$, $x=0$ 处最小, 则存在极小值点[^cal2-317]

---

<!-- added-heading -->

### 定义求导与根号中的绝对值 {#partial-derivatives-square-root-signs}

* 多元函数的求导
  * **时刻注意利用定义求偏导的时候开根号出来带着绝对值**
    * ${f(x,\,y)}$ 在 ${(0,\,0)}$ 有定义且 ${\displaystyle\lim_{(x,\,y)\to (0,\,0)}\dfrac{f(x,\,y)-(x^2+y^2)}{\sqrt{x^2+y^2}}=1}$, 讨论 ${f'_x,\,f'_y}$ 是否存在
      * 考虑可微定义, 有 ${\displaystyle\lim_{(x,\,y)\to (0,\,0)}\dfrac{f(x,\,y)-f(0,\,0)-(0x+0y)}{\sqrt{x^2+y^2}}-\sqrt{x^2+y^2}=1}$, 即 ${f(x,\,y)=\sqrt{x^2+y^2}+o(\sqrt{x^2+y^2})}$, 利用定义对 ${x,\,y}$ 求偏导, ${\displaystyle\lim_{x\to0,\, y=0}\dfrac{\sqrt{x^2}}{x}}$, 当然不存在, ${y}$ 同理[^cal2-322]

<!-- added-heading -->

### 隐函数方程组与二阶导数信息 {#implicit-systems-second-derivatives}

* 若出现隐函数 ${x=x(y), z=z(y)}$ 由 ${\begin{cases}F(f_1(x, y, z),\,g_1(x, y,z))=0\\ G(f_2(x, y, z),\,g_2(x, y,z))=0\end{cases}}$, 求 ${\dfrac{dx}{dy},\,\dfrac{dz}{dy}}$ 这类问题直接对方程组求导, 命题方式有具体函数或抽象函数, 如:
  * 隐函数 ${x=x(y), z=z(y)}$ 由 ${\begin{cases}F(y-x,\,y-z)=0\\ G(xy,\,\frac{z}{y})=0\end{cases}}$,求 ${\dfrac{dx}{dy},\,\dfrac{dz}{dy}}$
    * 对于两个方程组, 分别对 $y$ 求导, 得到:

      $$
      {\begin{cases}F'_1\frac{dx}{dy}+F'_2\frac{dz}{dy}=F'_1+F'_2 \\ yG'_1\frac{dx}{dy}+\frac{1}{y}G'_2\frac{dz}{dy}=-xG'_1+\frac{z}{y^2}G'_2\end{cases}}
      $$

      **这里做额外补充, 关于这类求导, 尽量转化成上述形式, 然后使用克拉默法则求出答案**
  * 类似问题有 ${880_{base}(3.1),\,880_{comp}(3.15)}$
* 给出一阶导数部分数据, 要求二阶导数的数据, 此时有两种方法, 第一种是**时刻想着对其给出的等式两边求导**, 二是求出原函数再求导, 第二种方法比较难求
  * 设 ${u(x,\,y)}$ 有二阶偏导数, ${\dfrac{\partial^2u}{\partial x^2}=\dfrac{\partial^2u}{\partial y^2}}$, 且 ${u(x,\,2x)=x,\,u_1'(x,\,2x)=x^2}$, 求 ${u_{11}''(x,\,2x)}$
    * 时刻想着对等式两边求导得出额外信息, 发现: ${u_1'+2u_2'=1,\,u_{11}''+2u_{12}''=2x,\,u_{11}''+2u_{12}''+2u_{11}''+4u_{22}''=0}$, 又 ${u_{11}''=u_{22}''}$, 故得 ${u_{11}''=-\dfrac{4}{3}x}$[^cal2-330]

## 二重积分 {#calculus-h-05}

---

<!-- added-heading -->

### 积分中值定理与对称性 {#double-integral-mean-value-symmetry}

* 对于无法计算的二重积分, 且式子中出现区域面积, 考虑是否可以使用 **二重积分中值定理**进行简化从而得到答案

---

* 二重积分的计算
  * 在积分难处理或者**出现抽象函数**的时候, 明显的无法做, 则**一定要观察轮换对称性**, 看看是否存在符号交换之后积分区域不变, 抽象函数也是如此, 从而达到简化的目的$(1k_{base}(14.14),\,武_{base}(9.11),\,880_{base}(2.3,\,2.4))$
  * 对于抽象函数或者疑似对称积分, 观察积分区域, 可以进行区域划分, 截取部分关于 $x,\,y,\,y=x,\,y=-x\dots$ 对称的部分, 简化积分($1k_{base}(14.17,\,14.23)$)

<!-- added-heading -->

### 平移极坐标与凑微分 {#translated-polar-coordinates-exact-differential}

* 若转化极坐标的时候出现中心圆移动, 即类似 $(x-a)^2+(y-b)^2=a^2+b^2$, 可以令 $\begin{cases}x-a=rcos\theta\\ y-b=rsin\theta\end{cases}$ 从而进行圆偏移, 此时就有 $\int_{0}^{\sqrt{a^2+b^2}}$ 从而简化积分[^cal2-338]
* 注意积分:
  * 此积分令 $F(x)=\int_{x}^{b}f(x)dx$, 然后去凑微分

    $$
    {\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy=\int_{a}^{b}[\int_{x}^{b}f(y)dy]\,d\,[-\int_{x}^{b}f(y)dy]}
    $$

    [^cal2-340]

## 微分方程 {#calculus-h-06}

---

<!-- added-heading -->

### 变量替换与符号检查 {#ode-substitution-signs}

* 解微分方程
  * 若解方程时比较难解, 但若解 $x(y)$ 简单, 可以考虑取倒数求 $y^{-1}(x)$ 再通过反解求出 $y(x)$[^cal2-344]
  * 同上说法更一般的, 不一定要解出 $y(x)$ 或者 $x(y)$, 也可解出$x(g(y)),\,y(h(x))$ 然后再通过反解得出答案
    * $y'+1=e^{-y}sinx$, 该方程无论是想解出来单个的 $x$ 或者 $y$ 都很难, 观察方程, 发现同乘 $e^y$ 之后左边其实是 $(e^y)'$, 于是可以解出 $e^y=f(x)$[^cal2-346]
  * 若题目给出信息, 要求与 $f(x)$ 的 $n$ 阶等价无穷小, 则可利用泰勒展开, 得出常数项的关系
  * 观察是否可以构造为: ${\dfrac{x}{y},\,\dfrac{y}{x}}$ 等形式, 这类问题一般需要同除变量, **在同除变量的时候注意正负号**, 和求积分那里的根号正负注意事项一样:
    * 求方程 ${xy'=\sqrt{x^2+y^2}+y}$ 的通解
      * 同除 $x$ 之后做法显然, 但是**这里需注意分类讨论**
      * 若 $x \gt  0$, 有: ${y'=\sqrt{1+\tfrac{y^2}{x^2}}+\dfrac{y}{x}}$, 令 ${t = \dfrac{y}{x}}$ 即可
      * 若 $x \lt  0$, 有: ${y'=-\sqrt{1+\tfrac{y^2}{x^2}}+\dfrac{y}{x}}$, 令 ${t=\dfrac{y}{x}}$ 即可

---

<!-- added-heading -->

### 线性方程解的结构 {#linear-ode-solution-structure}

* 微分方程解的结构:
  * 给出若干方程的特解 ${y_1,\,y_2,\,y_3\dots}$ 此时两个特解相减就是齐次方程的通解形式, 故进行若干次的非线性相减, 即用 $y_1$ 减去其他的特解, 得出一些非线性的通解, 他们就是齐次通解, 至此得到齐次方程式, 后随便代入一个特解求导得出 $f(x)$, 最终为 $\dots=f(x)$[^cal2-355]
  * 若题目指明给出若干线性无关的特解如 ${y_1(x),\,y_2(x),\,y_3(x)}$, 则微分方程通解的结构一定是: ${C_1y_1+C_2y_2+C_3y^*}$, 注意这里的 ${C_1,\,C_2,\,C_3}$ 均不相同, 此时再运用第一条方法, 可知通解为: ${C_1[y_1(x)-y_3(x)]+C_2[y_2(x)-y_3(x)]+y_3(x)}$化简有: ${C_1y_1+C_2y_2+C_3y_3,\,(C_1+C_2+C_3=1)}$[^cal2-356]
    * 二阶非齐次线性微分方程有三个特解: ${x,\,e^x,\,e^{-x}}$, 求通解
      * 根据题设直接构造出齐次通解: ${C_1(e^x-x)+C_2(e^{-x}-x)}$, 再加上一个特解 $x$ 即可

---

<!-- added-heading -->

### 与积分条件联动 {#ode-integral-conditions}

* 若题目显式或者隐式地给出了函数的积分形式, 微分方程可以与积分联动, 对微分方程进行两边积分可能会出现一些启发, 类$(660-81)$

## 无穷级数 {#calculus-h-07}

---

> 薄弱: 函数在某处的幂级数展开式, 弄不清楚是非

---

<!-- added-heading -->

### 通项极限与收敛性 {#series-terms-convergence}

* 关于无穷级数解常量
  * 对于任意形式级数, 利用好 $\lim_{n\to \infty}u_n=0$ 解出常量[^cal2-366]
  * 若级数 $\displaystyle\sum_{n=a}^{\infty}u_n+\sum_{n=a}^{\infty}v_n$ 收敛, 则可以利用极限等于 $0$ 形式转化为等价无穷小求极限, 解出常量即可[^cal2-367]

---

* 关于证明敛散性
  * 利用好级数同敛散与等价无穷小的转换, **利用好均值不等式**, 出现 $f(x)g(x),\,\frac{f(x)}{g(x)}$ 可用均值不等式, 或是利用通项收敛得出 $u_nv_n=0 \Rightarrow \dfrac{u_n}{v_n}=0$ 得出等价无穷小的关系, 若 $v_n$ 收敛则 $u_n$ 收敛[^cal2-370]
  * 通过证明 $\lim_{n\to \infty}u_n \neq 0$ 证明发散, 证明时若出现 $|u_{n+1}| \gt  |u_n|$, 则有 $u_n \neq 0$
  * 若 $\displaystyle\sum_{n=a}^{\infty}u_n$ 收敛, 则有 $u_{n}+u_{n+1}$ 收敛, 反着推则不行, 因为在不知收敛的情况下, 改变计算优先级会影响敛散性[^cal2-372]
  * 对于 含有积分且无法积出 或 无法直接求出敛散, **考虑放缩比较**

<!-- added-heading -->

### 交错级数与泰勒展开 {#alternating-series-taylor-expansion}

* 关于交错级数
  * 若不存在 $(-1)^n$ 则需要制造出 $(-1)^n$
  * 若交错级数不好用莱布尼茨判别法, 可考虑使用泰勒展开, 通过分析其展开后的若干级数的敛散性判断, 若存在一个发散, 则发散. 或者通过证明绝对收敛[^cal2-376]
  * 可以用等价无穷小来进行敛散性判断, 若两者正项级数行为相似, 在知道其中一个的敛散性情况下, 且 $\displaystyle\sum_{n=1}^{\infty}\xi_n = \sum_{n=1}^{\infty}u_n-\sum_{n=1}^{\infty}v_n$ 收敛, 则证明两级数同条件敛散[^cal2-377]
* 关于交错级数拆项, 要会分解出交错级数求解, 例如

  $$
  \sum_{n=1}^{\infty}sin(n\pi+\dfrac{1}{\sqrt{n}}),\,\sum_{n=1}^{\infty}sin(\pi\sqrt{n^2+1})
  $$

   利用积化和差, 可以把第二个式子变成 $(-1)^nsin(\dfrac{\pi}{\sqrt{n^2+1}+n})$
* 对于无法使用比较法或者根式等正项级数判别法的级数, 考虑使用泰勒展开, 一般此类题都可以使用泰勒展开相消, 或者带有未知变量, 展开之后再通过比较法判别敛散即可
  * 讨论 ${\displaystyle\sum_{n=1}^{\infty}\dfrac{1}{n}-ln(1+\frac{1}{n}),\,\sum_{n=1}^{\infty}(a^{\frac{1}{n}}-\sqrt{1+\dfrac{1}{n}})}$ 的敛散性
    * 对于第一个级数, 对 $lnx$ 泰勒展开: ${\dfrac{1}{n}-(\dfrac{1}{n}-\dfrac{1}{2n^2}+o(\dfrac{1}{n^2}))}$, 此时敛散性同 $\dfrac{1}{n^2}$
    * 对于第二个级数, 一样的化为: ${\displaystyle e^{\tfrac{lna}{n}}-(1+\tfrac{1}{n})^{\tfrac{1}{2}}}$, 然后同时泰勒, 讨论即可[^cal2-382]

---

<!-- added-heading -->

### 积分求和与常数项 {#power-series-integration-constant}

* 关于和函数
  * **求和函数对其积分之后, 要记得 ${+C}$, 然后代入 ${x=0}$ 的值, 求出 $C$**
    * 求 ${\displaystyle\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^{n}}$ 的和函数
      * 设 ${G(x)=\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^{n+1}}$, 则 ${S(x)=\frac{G(x)}{x}}$, 对 $G(x)$ 求导展开后有: ${G'(x)=\sum_{n=1}^{\infty}n^2x^n-2nx^n+x^n}$, 此时我们有:

        $$
        \begin{gather*} S_1(x) =x^n= \frac{1}{1-x}, \\ S_2(x) = x \cdot S_1'(x) = \frac{x}{(1-x)^2}, \\S_3(x) = x \cdot S_2'(x)=\dfrac{x(1+x)}{(1-x)^3},\\ G(x)=\int S_1(x)-2S_2(x)+S_3(x)\,dx \end{gather*}
        $$

        [^cal2-387]
        令 $t=(1-x)$ 之后分别对其求导之后得到 $G(x)=f(x)+C$, 又 $f(0)=-4+C$, 且 $G(0)=0$, 故 $G(x)=f(x)+4,\,S(x)=\dfrac{G(x)}{x}$

<!-- added-heading -->

### 递推系数与和函数方程 {#recursive-coefficients-generating-function}

* 关于抽象形式的和函数
  这类往往需要通过条件求出和函数之间的关系
  * 可以通过积分或者求导来获得等价关系, 类似于分部积分的方法, 进一步得到和函数的关系, 然后可以通过积分或者求微分方程得到和函数
    * $①a_1=1,\,②a_{n+1}=(1-\dfrac{1}{2(n+1)})a_n$, 求 $\displaystyle\sum_{n=1}^{\infty}a_nx^n, |x|\lt 1$ 和函数:
      解: 设 $S(x)=\displaystyle\sum_{n=1}^{\infty}a_nx^n$, 对其求导, 有 $S'(x)=\displaystyle\sum_{n=1}^{\infty}(n+1)a_{n+1}x^{n}$又 $②\,\Rightarrow 1+\sum_{n=1}^{\infty}na_nx^n + \dfrac{1}{2}\sum_{n=1}^{\infty}a_nx^n$, 即 $S(x)=1+xS'(x)+\dfrac{1}{2}S(x)$, 然后解微分方程即可[^cal2-393]
  * 类似的, 还可通过其他, 例如利用华理士公式等等($张_{base}(16.34)$)

<!-- added-heading -->

### 收敛区间、母函数与拆项 {#power-series-domain-standard-functions}

* 由于幂函数连续，则求和仍连续，在求和函数的过程中由于积分求导可能会产生间断点，需要对间断点单独讨论[^cal2-395]
  * 对于 $0$，一般只需要带入即可, 一定注意结果可能不为零
  * 对于其余常数 $c$，若带入后无法求出，则用和函数在 $c$ 处取极限，求这个极限即可
* 还是类似于第一章, 抽象能力要足, 不能死板, 例如对于 $\dfrac{1}{1-x}$ 我们知道是等比 $x^n$ 的和函数, $\dfrac{1}{x-2}$ 也同理 ${\Rightarrow \dfrac{1}{1-\frac{x}{2}}}$[^cal2-398]
* 对于母型函数, 要会凑项拆项, 要化成 $\sum_{1}^{\infty}$ 型, 然后套用母型函数公式, 对于项数不够的, 仍按照公式求和, 后面要减去多的项
  * 如计算 $\displaystyle\sum_{n=2}^{\infty}\dfrac{1}{(n^2-1)2^n}$, $\dfrac{1}{2^n}$ 看作 $x^n$, 即求 ${S(\frac{1}{2})}$, 直接拆项求 ${\displaystyle2\sum_{n=2}^{\infty}(\dfrac{x^n}{n-1}-\dfrac{x^n}{n+1}) \Rightarrow 2\sum_{n=2}^{\infty}(x\dfrac{x^{n-1}}{n-1}-\dfrac{1}{x}\times\dfrac{x^{n+1}}{n+1}) \Rightarrow 2\sum_{n=1}^{\infty}(x\dfrac{x^n}{n}-\dfrac{1}{x}\times\dfrac{x^{n+2}}{n+2})}$此时我们就有 $2(x\times-ln(1-x) + \dfrac{-ln(1-x)-x-\frac{x^2}{2}}{x})$[^cal2-400]

<!-- added-heading -->

### 提取奇数项与阶乘级数 {#odd-terms-factorial-series}

* 关于缺偶数项级数, **通过 ${\displaystyle\sum_{n=1}^{\infty}u_n=\dfrac{\sum_{1}^{n}u_n-\sum_{1}^{n}(-u_n)}{2}}$ 得到**[^cal2-401]
  * 例: ${\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{2n+1}(\dfrac{x^2}{2})^{n}}$ 首先改成全奇数形式:

    $$
    {\dfrac{\sqrt{2}}{x}\sum_{n=0}^{\infty}\dfrac{2}{2n+1}\cdot\dfrac{x^{2n+1}}{(\sqrt{2})^{2n+1}}}
    $$

    利用上述公式, 我们有:

    $$
    {\dfrac{\sqrt{2}}{x}\times \dfrac{\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{n}\dfrac{x^{n}}{\sqrt{2}^{n}}\,-\,\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{n}\dfrac{(-x)^{n}}{\sqrt{2}^{n}}}{2}}
    $$

    [^cal2-402]
    再根据母函数 $\displaystyle\sum_{n=1}^{\infty}\dfrac{x^n}{n}=-ln(1-x)$, 得:

    $$
    {\dfrac{\sqrt{2}}{x}\times 2\times \dfrac{(-ln|1-\dfrac{x}{\sqrt{2}}|\,+\, ln|1+\dfrac{x}{\sqrt{2}}|)}{2}}
    $$

* 缺奇数项级数与上述相同, 但注意, **若是交错形式, 需要考虑用求导积分**, 如 ${\displaystyle \sum_{n=1}^{\infty}(-1)^n\dfrac{x^{2n-1}}{2n-1}}$[^cal2-404]

---

* 注意在求级数和的时候, 若出现 $n!$ 要考虑 $e^x$, 而同时要注意负数的阶乘是没意义的, 所以要灵活改变, 例:
  * 求 $\displaystyle\sum_{n=0}^{\infty}\dfrac{n+1}{n!}$, 将其拆开之后会有: $\displaystyle\sum_{n=0}^{\infty}\dfrac{1}{(n-1)!}+\dfrac{1}{n!}$, 由于负阶乘未定义, 要变成 $\displaystyle\sum_{n=1}^{\infty}\dfrac{1}{(n-1)!}+\sum_{n=0}^{\infty}\dfrac{1}{n!}=2e$

## 空间几何、多元曲线面、多元积分 {#calculus-h-08}

---

> **关于曲线面部分以及 ${Guess,\,Green,\,Stokes}$ 公式的运用和注意点, 放在了李范全书对应的第九、十章**[^cal2-410]

---

* 以下约定: 用 $\tau$ 来表示方向向量, 用 $n$ 来表示法向量

---

<!-- added-heading -->

### 平面方程与距离 {#planes-equations-distances}

* 对于一个平面
  * 求平面与平面间的距离 $d=\dfrac{|D_1-D_2|}{\sqrt{A^2+B^2+C^2}}$, 不要忘记有绝对值, 存在两个平面与某个平面的距离为 $d$[^cal2-415]
  * 给出三点 $A,\,B,\,C$, 求平面方程: 求出 ${\overrightarrow{AB},\,\overrightarrow{AC}}$, 求出 ${\vec{n}=\overrightarrow{AB}\times \overrightarrow{AC}}$, 然后代入随便一点即可[^cal2-416]

---

<!-- added-heading -->

### 空间直线、投影与距离 {#space-lines-projections-distances}

* 空间直线
  * 求空间直线方程一般有如下方法
    * 求出直线的 $\tau$ 以及直线上一点的坐标, 然后利用 $\dfrac{x-x_0}{l}=\dfrac{y-y_0}{m}=...$[^cal2-420]
    * 利用题设条件求出两个平面的交线, 一般对于点法直线存在平面束方程, 然后利用该平面束方程与已知坐标点代入求解
    * 若题设条件给出与所求直线垂直的两直线, 则可利用已知的 ${\tau_1\times \tau_2=\tau}$ 此时得到所求直线的 $\tau$, 由于所求直线与两直线均垂直, 两线确定一平面, 则利用 ${n_1=\tau\times\tau_1、n_2=\tau\times\tau_2}$, 分别求出两平面法向量, 然后代入对应两直线的点即可[^cal2-422]
  * 关于一直线在一平面的投影方程, 根据两平面建立平面束方程, 与投影平面的 $n$ 建立关系解出 $\lambda$, 然后即可求得另一平面, 两平面交线即所求投影直线. 类 ($张_{base}(习题17.5),\,880_{base}(4.3.4)$)[^cal2-423]
  * 给出两直线 $L_1,\,L_2$, 写出过 $L_2$ 平面约束方程 $\pi$, 然后利用与 $L_1$ 平行求出 $\lambda$, 从而得到具体平面, 然后随便代入 $L_1$ 的一点, 求点到平面距离即可[^cal2-424]

<!-- added-heading -->

### 线段上的第一类曲线积分 {#line-segment-scalar-line-integrals}

* 空间直线的曲线积分:
  * 若是给出了两个点, 那么只需要计算出 ${\tau}$ (这里注意方向), 然后利用 ${L}$ 的方程(两点式方程): ${\dfrac{x-x_0}{l}=\dfrac{y-y_0}{m}=...=t}$ 把 ${x,\,y,\,z}$ 参数化, 然后计算出 ${||\tau||}$, 计算 ${\displaystyle \int_Lf(x,\,y,\,z)\,ds=\int_{0}^{1} f\{x(t),\,y(t),\,z(t)}\}\,||\tau||dt$ 即可. (${880_{base}(2.6)}$)[^cal2-426]

<!-- added-heading -->

### 曲面投影的边界 {#surface-projection-boundaries}

* 关于一曲面在一区域的投影, 或者在坐标平面的投影区域(即投影曲线), 若给出的是曲面, 则把平面的法向量求出来, 设投影区域平面的法向量为 ${\tau}$, 那么利用 ${n\cdot \tau=0}$ 可建立关系, 设得到的关系为 ${g(x,\,y,\,z)=0}$, 于是有 ${\begin{cases} f(x,\,y,\,z)=0 \\ g(x,\,y,\,z)=0 \end{cases}}$, 这里假设投影到 ${xOy}$ 面, 然后把 ${z}$ 消去, 得到 ${{\begin{cases} h(x,\,y)=0 \\ z=0 \end{cases}}}$[^cal2-427]
  * 如曲面 ${\displaystyle \sum: x^2+y^2+z^2-yz=1}$ 在 ${\displaystyle xOy}$ 面的投影曲线, 则坐标轴法向量为 ${\displaystyle n=(0,\,0,\,1)}$, 曲面的切向量为 ${\displaystyle \tau=(2x,\,2y-z,\,2z-y)}$, 于是利用 ${\displaystyle n\cdot \tau =0}$ 得到 ${\displaystyle {\begin{cases} 2z-y=0 \\ x^2+y^2+z^2-yz=1 \end{cases}}}$, 消去 ${\displaystyle z}$ 即可[^cal2-428]

---

<!-- added-heading -->

### 旋转曲面的坐标平移 {#rotation-surfaces-coordinate-shifts}

* 求绕某处旋转时
  * 若是绕坐标轴旋转, 则利用给出的方程消去变量, 如绕 $z$ 轴旋转, 则利用 ${x=f(z)、y=g(z)\Rightarrow x^2+y^2=f^2(z)+g^2(z)}$ 即可
  * 若是绕某处, 如绕 ${x=1、\begin{cases}x=2\\ y=3\end{cases}}$ 旋转等, 可以利用平移变换, 即左加右减上加下减转化为坐标轴旋转, 然后利用上述方法求出旋转表达式, 随后逆变换回来, 如:[^cal2-432]
    * 求直线: ${\dfrac{x-1}{3}=\dfrac{y-2}{4}=\dfrac{z+1}{1}}$ 绕 ${\begin{cases}x=2\\ y=3\end{cases}}$ 旋转一周的方程
      * 先平移, 得到 ${\dfrac{x+1}{3}=\dfrac{y+1}{4}=\dfrac{z+1}{1}}$, 然后化成平面相交形式的方程: ${\begin{cases}x=3z+2\\ y=4z+3\end{cases}}$, 得到 ${x^2+y^2=25z^2+36z+13}$, 然后再平移回来: ${(x-2)^2+(y-3)^2=25z^2+36z+13}$ 即可

<!-- added-heading -->

### 相交直线旋转与球坐标范围 {#rotating-intersecting-lines-spherical-bounds}

* 更加一般的, 若**一直线绕给定轴**, 如 ${x=y=z}$ 这条直线旋转, 则采用定角度方法, 即利用 ${cos\theta=\dfrac{|r \cdot \tau|}{|r||\tau|}}$, 因为是一直线绕轴旋转, 其角度是一定的[^cal2-435]
  * ${\sum}$ 是直线 ${\begin{cases}x=0\\y=0\end{cases}}$ 绕 ${x=y=z}$ 旋转得到的曲面, 求 ${\sum}$ 的方程
    * 轴的方向向量为 ${\tau=(1,\,1,\,1)}$, 任找一点 ${r=(0,\,0,\,1)}$, 则对于直线任意点 $(x,\,y,\,z)$ 有 ${cos\theta=\dfrac{r\cdot \tau}{|r||\tau|}=\dfrac{1}{\sqrt{3}}=\dfrac{|x+y+z|}{|\tau||\sqrt{x^2+y^2+z^2}|}}$, 得到 $xy+xz+yz=0$

---

* 在计算时, 要时刻观察积分区域, 特别是球坐标系中的 $\phi$, 圆锥或者球心不在原点的 $\phi$ 会改变

---

<!-- added-heading -->

### 锥面、顶点与准线 {#cones-and-directrices}

* 关于给定锥面顶点和准线来求锥面方程
  * 给定顶点为原点, 准线为 ${\displaystyle \begin{cases} z=y^2 \\ x=1 \end{cases}}$, ${\displaystyle (|y|\leq 1)}$, 利用参数方程, 设锥面上的点为 ${\displaystyle P=(1,\,u,\,u^2)(|u|\leq 1)}$, 利用顶点与该点的连线 ${\displaystyle (x,\,y,\,z)= \lambda \overrightarrow{OP} =\lambda(1-0,\,u-0,\,u^2-0),\,\lambda \gt  0}$, 那么这个直线上的点就是锥面上的所有点, 于是消去 ${\displaystyle \lambda,\mu }$, 有 ${\displaystyle x=\lambda,\,y=\lambda u,\,z=\lambda u^2}$, 故 ${\displaystyle \lambda=x,\, u=\dfrac{y}{x},\,z=x\times \dfrac{y^2}{x^2}}$, **接下来注意各个取值**, 由于 ${\displaystyle |u|\leq 1}$, 于是 ${\displaystyle \lambda \leq 0 \rightarrow x\leq 0;\, |\dfrac{y}{x}|\leq 1 \rightarrow |y|\lt x}$[^cal2-442]
  * 若给定顶点 ${\displaystyle (a,\,b,\,c)}$ 不是原点, 则把顶点平移到原点, 还是上述的准线, 平移后的顶点为 ${\displaystyle x'=x-a,\,y'=y-b,\,z'=z-c}$, 于是准线上的点为 ${\displaystyle (1-a,\,u-b,\,u^2-c),\,|u| \leq 1}$, 那么锥面上的点 ${\displaystyle (x',\,y',\,z')=\lambda(1-a,\,u-b,\,u^2-c),\,\lambda \leq 0}$, 消元即可[^cal2-443]

<!-- source-content:calculus:end -->

## 线性代数 {#linear-algebra}


<!-- source-content:algebra:start -->
>   做到一个新的问题，想起与过去某个问题类似。发现在解答中，对此类问题，以及工具和方法的理解是存在缺陷的，或者发现理解不够深刻。于是通过解决新的问题，一并更新迭代过去的理解。

---

<!-- added-heading -->

### 初等变换、秩与线性方程组 {#algebra-rank-and-systems}

* 我们知道, 对于一个**可逆矩阵$A$**, 其可以是若干初等矩阵$P_1P_2\dots$变换得来, 相反的经过若干初等矩阵组成的矩阵一定可逆, 也就是如果 $P_1,\,P_2$ 可逆, 那么乘积也是可逆的(反证法即可, $P_1P_2=Q\Rightarrow Q(P_1P_2)^{-1}=E$), 那么对于两个等价的矩阵 $A,\,B$来说, 一定可以经过若干次初等行(列)互换[^alg-3]

---

* 见到 $A_{m\times n}B_{n \times m}=O$
  * 要想到 $r(A)+r(B)\leq n$, 这里的 $B$ 可以是 $A$.类($1k_{base}4.7$)
  * 若 ${AB=O}$, 且在 ${r(B)=r}$, 则**对 ${B}$ 矩阵列分块**, 可知 **${B}$ 有 ${r}$ 个线性无关的列向量**, 对应着 ${A\xi_i=0}$, 也就是 ${A}x=0$ 有 **${r}$ 个线性无关的解向量, ${r}$ 个线性无关的特征向量, 对应着特征值为 ${0}$**[^alg-7]

---

* 已知矩阵 $A$ 有 $r(A) = m$, 即有两个线性无关的解向量, 有 $Ax=b$ 的线性无关解向量的个数为 $m+1$.[^alg-9]
  * 证: 这里以 $r(A)=2$ 为例, 存在 $k_1\xi_{1},\,k_2\xi_{2}$ 线性无关, $Ax=b$ 的通解为 $k_1\xi_{1},\,k_2\xi_{2} + \eta$, 则存在 $\eta,\,\eta+k_1\xi_1,\,\eta+k_2\xi_2$, 即 $2 + 1$ 个线性无关的解向量[^alg-10]

---

* 对于 $Ax=B$ 或者给出一种更隐晦的说法 $A,\,B$ 为同解方程组, 那么我们有$A^Tx=0$ 与 $\left[\begin{array}{c}A^T \\ B^T \end{array}\right]x=0$, 即两者同解[^alg-12]
  * 证:
    1. $\left[\begin{array}{c}A^T \\ B^T \end{array}\right]x=0$ 显然包含 $A^Tx=0$
    2. 由 $r(A)=r(B)=r([A, B])$, 则 $r(A^T)=r(B^T)=r(B^TA^T)=r(A^TB^T)$[^alg-15]
       则两者包含的解向量个数相等, 又 $1$ 中两者包含, 则两者同解
* $Ax=0$ 与 $AA^Tx=0$ 也为同解方程组[^alg-17]

---

* 在解方程组令自由变量时, 要根据主元的位置而定, 即除主元之外的可以随便更改, 如:
  * $A$ 的每行元素之和为 $k$, 且为实对称矩阵, 具有二重特征值 $\lambda = 1$, 求出 $A$ 的所有特征值与特征向量
    解:
    首先关于这种每行元素和为 $k$, 或者给出隐晦说法: 实对称矩阵每列元素和为 $k$, 都隐含着当 $\xi = [1,1,1]^{T}$ 时, 有 $A\xi_3=k\xi_3\,\Rightarrow \lambda = k$, 又因为$A$ 为实对称矩阵, 则根据正交关系, 设 $\xi=[x_1,\,x_2,\,x_3]$, 有 $\xi\times\xi_3=0\Rightarrow x_1+x_2+x_3=0$, 此时由于对角线一定有值, 则主元的位置在第一列, 故设为 $\xi_1=[y,1,0],\,\xi_2=[y',0,1]$, 解出即可[^alg-22]

---

<!-- added-heading -->

### 秩一矩阵、特征值与伴随矩阵 {#algebra-rank-one}

* **秩一矩阵的总结:**
  * 给出两个 $n$ 维列向量 $\alpha,\,\beta$, 我们有
  * ${\alpha^T\beta=\beta^T\alpha=C}$, 又被称为内积 ${(\alpha,\,\beta)}$
  * ${\alpha\beta^T=A,\,\beta\alpha^T=B}$, 仔细观察两矩阵, 发现各行均成比例, 即为秩一矩阵: $r(A)=r(B)=1$, 进一步的, **主对角线之和 ${C=tr(A)=tr(B)=(\alpha,\,\beta)}$, 由此引出一个关键作用:** 秩一矩阵可被拆解为 $\alpha\beta^T$, 从而求 $A^n$ 极其方便[^alg-29]
  * ${A^2=\alpha(\beta^T\alpha)\beta^T=CA\Rightarrow \lambda=0\,或\,\lambda = C=tr(A)}$, 由 $\sum\lambda=tr(A)$, 得到 ${\lambda_1=tr(A),\,\lambda_{2,\,3,\,\dots}=0}$, 有了这些结论, 可以通过特征值连乘从而优化计算 $|A+kE|,\,|A^{-1}|$ 等行列式[^alg-30]
    * 由于 ${\alpha^T\beta=\beta^T\alpha=C}$ 的特殊性, 若矩阵 $M$ 与 $\alpha\beta^T$ 相关, 可以通过求 $M^2$ 然后利用 $C$ 来简化特性, 然后利用**长除法**除 ${(M+iE)}$ 得到 $(M+iE)(M+jE)=kE$, 得到 $(M+iE)^{-1}$ 的表达式
    * 由于 **$tr(A)$ 为一重根, $0$ 为 $n-1$ 重根**, 则特征向量直接解齐次方程即可[^alg-32]
  * 考虑相似对角化充要条件: $n$ 个线性无关的特征向量
    * **若 $tr(A)\neq0$**, 则可求出具体的 $\xi_1$, 然后由于 $\lambda_{2\dots n}$ 对应的形式为 ${A\xi_{2\dots n}=0}$, 又 ${r(A)=1\Rightarrow n - r(A)=n-1}$ 个线性无关的解向量, 故**此时 $A$ 可相似对角化**
    * **若 $tr(A)=0$**, 对应: ${A\xi_{1\dots n}=0}$, 则 ${r(A)=1\Rightarrow n - r(A)=n-1}$, 故一定存在线性相关解向量, **故 $A$ 不可相似对角化**
  * 更进一步的, 若 ${(\alpha,\,\beta)=0}$ 且 ${\alpha,\,\beta}$ 为单位列向量, 则两两正交, 此时有以下性质
    * 若 ${A=\alpha\beta^T + \beta\alpha^T}$, 则有 ${A=\alpha\beta^T + (\alpha\beta^T)^T=A^T}$, 即 $A$ 为是对称矩阵, 可相似对角化
    * 对于秩一的对称矩阵, 由于只存在一个 ${\lambda \neq 0}$, 设 ${Q^TAQ=\Lambda}$ 中对应**非零正交单位化后**的特征向量 ${\xi}$, **由于对角矩阵其余列向量均为零向量, 则有: ${A=\xi^T\lambda\xi}$, 可快速求出 ${A}$**, 否则还需要求出矩阵 ${P}$, 然后有 ${A=P\,\Lambda\,P^{-1}}$[^alg-38]
    * 等式两边左乘 ${\alpha,\,\beta}$, 有 ${①\,A\alpha=\alpha\beta^T\alpha+\beta\alpha^T\alpha=\beta,\,②\,A\beta=\alpha\beta^T\beta+\beta\alpha^T\beta=\alpha}$, 则:[^alg-39]
      * ${①+②=A(\alpha+\beta)=(\alpha+\beta)}$
      * ${①-②=A(\alpha-\beta)=-(\alpha-\beta)}$
      * 以上, **得到 ${A}$ 的两个特征值为 ${1,\,-1}$, 又 ${r(A)\leq r(\alpha\beta^T)+r(\beta\alpha^T)=2}$, 故其余特征值为 ${0}$, 特征向量为 ${(\alpha+\beta),\,(\alpha-\beta),\,\xi_i}$**
    * 若 ${\alpha=\beta}$, 则其特征向量为 ${\alpha}$, 且有 ${A=\alpha\alpha^T}$, 这里用到了正交相似的性质, 在正定部分有介绍[^alg-43]
  * 若 ${r(A)\neq 1}$, 即不符合上述情况, 此时还有一个特殊的秩一矩阵, 即 ${r(A)=n-1\Rightarrow r(A^*)=1}$, 那么对于 ${A^*X=0}$ 的解向量, 可以通过这样来求, 即: ${A^*A=|A|E=O}$, **则 ${A}$ 中一定有 ${n-1}$ 列为 ${A^*X=0}$ 的解向量**, 而同时的, 解向量一定线性无关, 且由于 ${r(A)=n-1}$ 知对应的特征值只有一个为 ${0}$, **故为除去对应 ${\lambda =0}$ 的 ${\xi}$ 之外的所有特征向量**[^alg-44]
  * 对于以上结论要会灵活运用, 出现秩一矩阵要想到上述结论, 例:
    * 求 ${A=\begin{vmatrix}0 & 2 & 3 & 4 \\2 & 3 & 6 & 8 \\3 & 6 & 8 & 12 \\4 & 8 & 12 & 15\end{vmatrix}}$[^alg-46]
      >观察发现 ${B=A+E}$ 为秩一矩阵, 则 ${|A|=|B-E|}$, 又 $\lambda_{B_i}$ 为 ${30,\,0,\,0,\,0}$, 则 $\lambda_{A_{i}}$ 为 $29,\,-1,\,-1,\,-1$, 故 ${|A|=-29}$
    * ${n(n\geq 2)}$ 阶矩阵 ${A=\begin{vmatrix} a & 1 & 1 & ... & 1 \\ 1 & a & 1 & ... & 1 \\ . & . & . & ... & . \\ . & . & . & ... & . \\ 1 & 1 & 1 & ... & a \end{vmatrix}}$, 求 ${A}$ 的特征值, 特征向量
      >直接法: 全部的行加到第一行, 然后提出倍数, 相减即可, 较为复杂
      化作秩一矩阵: $A=(a-1)E+B$, 又 ${r(B)=1,\,tr(B)=n}$, 故 ${B}$ 的特征值为 ${n,\,0,\dots}$, 特征向量为 ${\alpha_1,\,\alpha_2,\,\alpha_3}$, 故 $A$ 的特征值为 ${a+1+\lambda_i}$, 特征向量不变[^alg-50]

---

<!-- added-heading -->

### 基、坐标与向量组等价 {#algebra-bases-and-coordinates}

* 关于基坐标
  * 定义: ${\alpha = a_1\xi_1+a_2\xi_2+\dots+a_n\xi_n}$, 其中 ${\xi}$ 是基空间, ${[a_1,\,a_2...\,a_n]}$ 是在该基空间下的坐标[^alg-53]
    * 存在一组基为: ${\alpha_1=(a_1,\,a_2,\,a_3),\,\alpha_2=(b_1,\,b_2,\,b_3),\,\alpha_3=(c_1,\,c_2,\,c_3)}$, 求 ${\beta=(k_1,\,k_2,\,k_3)}$ 在该基空间下的坐标
      * 直接设出在空间下的坐标 ${(x_1,\,x_2,\,x_3)}$, 然后建立等价关系, 即: ${\beta=x_1\alpha_1+x_2\alpha_2+x_3\alpha_3}$, 由于只有 $x$ 未知, 故转化成三元一次方程组解方程即可
    * 存在两组三维基空间: ${\alpha_i,\,\beta_i}$, 求出在该两组基下坐标相同的向量
      * 仍是设出坐标, 然后建立等价关系: ${x_i\alpha_i=x_i\beta_i\Rightarrow x_i(\alpha_i-\beta_i)=0}$, 发现仍然是三元一次方程组, 转化成解线性方程组问题[^alg-57]
  * 对于由一组基组成的矩阵 ${P=(\xi_1,\,\xi_2...)}$, 若要求 ${AP}$ 到 ${BP}$ 的过渡矩阵, 有两个方法, 第一个是将基取转置看作行向量, 然后有 ${PB=PAC^T \Rightarrow C^T=(PA)^{-1}(PB)=A^{-1}B}$, 然后再对 ${C}$ 取回转置即可, 第二种就是凑矩阵, 计算量小[^alg-58]
    * 例: ${\beta_1,\,\beta_2,\,\beta_3}$ 是一组基, 求基 ${\beta_1,\,2\beta_2,\,3\beta_3}$ 到基 ${\beta_1-\beta_2,\,\beta_2+\beta_3,\,\beta_3-\beta_1}$ 的过度矩阵
      > 方法一: 利用上述做法, 求出 ${A^{-1}B}$, 然后再取转置, 其中 ${A,\,B}$ 分别是其进行矩阵分解后的系数矩阵
      > 方法二: 观察得, 有 ${[\beta_1-\beta_2,\,\beta_2+\beta_3,\,\beta_3-\beta_1]=[{\beta_1,\,2\beta_2,\,3\beta_3}]\begin{bmatrix} 1 & 0 & -1 \\ -\frac{1}{2} & \frac{1}{2} & 0 \\ 0 & \frac{1}{3} & \frac{1}{3} \end{bmatrix}}$
      > 明显的, 方法二要简单得多, 尽可能地使用方法二
    * **坐标之间的过渡矩阵**
      * 对于列向量坐标, 有 ${\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}=P \cdot \begin{bmatrix} y_1 \\ y_2 \\ y_3 \end{bmatrix}}$, 对于行向量坐标, 有 ${\begin{matrix}[x1 & x2 & x3]\end{matrix}=\begin{matrix}[y1 & y2 & y3]\end{matrix} \cdot P^T}$
      * **注意**: 上述的 ${P}$ 不是矩阵的过渡矩阵, 对于列向量来说, ${P}$ 是基坐标的过渡矩阵, 而对于行向量的 ${P}$ 来说, 由于过渡矩阵是对每一列进行变换, 所以而 ${P}$ 是对每一行进行变换, 因为真正的过度矩阵应该是 ${P^T}$[^alg-65]

---

* 向量 ${\alpha = \alpha_1,\,\alpha_2,\dots,\,\alpha_n}$ 线性无关
  * 若存在 ${\beta = \beta_1,\,\beta_2,\dots,\beta_n}$ 可表示出 ${\alpha}$, 则两者等价
    * 证: 若 $\beta$ 可表示 $\alpha$ 意味着 ${r(\beta)\geq r(\alpha) = n}$, 故等价. **此条为充分条件**
  * 条件同上, 则由 ${\alpha}$ 构成的矩阵 ${A=({\alpha = \alpha_1,\,\alpha_2,\dots,\,\alpha_n})^T}$ 和由 $\beta$ 构成的矩阵 $B$ 等价
    * 证: 由第一条知充分性显然, 下面**证明必要性**. 由两矩阵等价, 故行向量等价, 意味着 ${r(\begin{bmatrix} A \\ B \end{bmatrix})=r(A)=r(B)}$ 即 $A,\,B$ 同解, 故能互相表示, **此时也引出一个重要性质, 两矩阵行向量等价 ${\Leftrightarrow}$ 两矩阵同解**[^alg-71]
    * 矩阵 ${Q_{m\times n},\,P_{n\times m},\,r(Q)=n}$, 则可以构造出同解形式, 从而获得等价关系. 即: 设 ${\xi}$ 是 $PX=0$ 的解, 由于 $Q$ 只有零解, 故 $Q(PX)=0$, ${PX=0\Rightarrow P\xi}$ 是 $QY=0$ 的解, ${(QP)\xi=0}$, 则 $\xi$ 是 $QP$ 的解, 此时得出 ${QP}$ 与 $P$ 同解为 $\xi$.[^alg-72]
    * 例: $n$ 维列向量 $\alpha$ 满足 $\alpha^T\alpha=2$, $A,B$ 为 $n$ 阶矩阵, 已知条件:${A(E-2\alpha\alpha^T)=B}$, 则 ${A^TX=0,\,B^TX=0}$ 是否同解?
      > 由**秩一矩阵的性质知 $\alpha\alpha^T$ 的特征值为 ${2}$, 则 $|E-2\alpha\alpha^T|\neq0$**, 令其为 $C$, 则 $AC=B,\,A=BC^{-1}$, 即 **$B$ 的列向量可由 $A$ 的列向量线性表示**, 同样的 $A$ 的列向量可有 $B$ 的列向量表示, 即 $A,\,B$ 具有等价列向量, 则 $A^T,\,B^T$ 有等价行向量, 即 $A^T,\,B^T$ 同解

---

<!-- added-heading -->

### 二次型、合同与标准形 {#algebra-quadratic-forms}

* 关于矩阵的二次型
  * 这里仅讨论实对称矩阵的合同, 对于两个非对称矩阵, 也可以合同, **但一个对称矩阵和一个非对称矩阵不可能合同**
  * 矩阵的二次型
    * 二次型的标准型可以由配方法、求特征值特征向量正交变换、初等变换法(**待补充, 1k题中的那道题**) 这几种方法, **其中配方法可以得到规范型, 对于特征值只有 ${1,\,-1,\,0}$ 的矩阵才能通过正交变换得到规范型**
    * 若题目中标准型是**由可逆线性变换且非正交**的变换, 则只能用配方法, 此外还隐含着, ${f(x_1,\,x_2,\,...)}$ 经过变换后得到的 ${f(y_1,\,y_2,\,...)}$ 的系数矩阵是不相似的[^alg-80]
    * 关于二次型解的个数, 若已知 ${X^TAX}$ 中 ${A}$ 的特征值, 则 ${X^Tf(A)X}$ 的解的个数为 ${\lambda_{f(A)}=0}$ 的个数, 解向量即对应的特征向量[^alg-81]

<!-- added-heading -->

### 二次型的最值：普通与广义瑞利商 {#algebra-rayleigh-quotients}

* 二次型的最值问题
  * 求形如 ${\dfrac{f(x)}{g(x)}}$ 或 ${\dfrac{x^TAx}{x^TBx}}$ 的最值问题, 一般形式是 ${\dfrac{x^TAx}{x^Tx}}$ 的最值问题, 这类问题可以通过对 ${A}$ 进行正交变换, 即 ${A = Q^T\Lambda Q,\,x=Qy}$ 把原式化为 ${\dfrac{y^T\Lambda y}{y^Ty}}$, 这种形式可以写为: ${\dfrac{\displaystyle\sum_{i}^{n}\lambda_i\cdot y_i^2}{\displaystyle\sum_{i}^{n} y_i^2}}$ 此时最值即可取 ${max/min{\sum_{1}^{n}\lambda_i}}$, 取 ${y_i=1}$ 即可, 然后再通过 ${x=Qy}$ 得到 ${x}$ 的最值点[^alg-83]
    * 例: 设二次型：${f(x_1, x_2, x_3) = \mathbf{x}^\mathrm{T} \begin{bmatrix} 1 & 0 & 6 \\ 4 & 4 & 4 \\ 0 & 8 & 9 \end{bmatrix} \mathbf{x}}$，其中 ${\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}}$。
      (Ⅰ) 用正交变换 ${\mathbf{x} = Q\mathbf{y}}$ 将其化为标准形，并求出 ${Q}$；

      (Ⅱ) 求 ${g(x_1, x_2, x_3) = \dfrac{f(x_1, x_2, x_3)}{x_1^2 + x_2^2 + x_3^2}}$ 的最大值，并求出一个最大值点，其中 ${x_1^2 + x_2^2 + x_3^2 \ne 0}$。
    > 二次型系数矩阵 ${\begin{bmatrix} 1 & 0 & 6 \\ 4 & 4 & 4 \\ 0 & 8 & 9 \end{bmatrix}}$ 明显的**不好求其特征值, 故对其进行重新配方**, 得到 ${A=\begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \\ 3 & 6 & 9 \end{bmatrix}}$, 此时根据**秩一矩阵性质**得到其 ${\lambda=14,\,0,\,0}$, 特征向量求得 ${\xi_1,\,\xi_2,\,\xi_3}$, 由于实对称, 直接有 ${\xi_1=(1,\,2,\,3)^T}$, 为了避免施密特, 设 ${\xi_2=(0,\,-3,\,2)^T,\,\xi_3=(k,\,2,\,3)^T}$, 解 ${k}$ 即可, 然后分别单位化得到 ${Q}$[^alg-88]
    > 利用 ${x=Qy}$ 变换为 ${\dfrac{y^T(Q^TAQ)y=y^T\Lambda y}{y_1^2+y_2^2+y_3^2}=\dfrac{14y_1^2+0+0}{y_1^2+y_2^2+y_3^2}}$, 显然 ${y=(1,\,0,\,0)^T}$ 时取最大值 ${14}$, 此时 ${x=Qy=\xi_1}$, 这里的 ${\xi_1}$ 是单位化后的特征向量
  * 对于 ${\dfrac{x^TAx}{x^TBx}}$ 这种最值问题, 由于分母不再是一般的 ${x^Tx}$ 的形式, 所以需要先对分母做 ${x=Qy}$ 化为 ${\dfrac{y^T(Q^TAQ)y}{y^Ty}}$, 记 ${M=Q^TAQ}$, 则此时的 ${\dfrac{y^TMy}{y^Ty}}$ 已经是一般形式, 再对分子做一次变换即可, 对于求最值点, 利用 ${z=Py,\,x=Qy}$ 求得 ${x}$ 的最值点 ${}$[^alg-90]
    * 例: 设二次型 $f(x_1, x_2) = x_1^2 - 4x_1x_2 + 4x_2^2$，$g(x_1, x_2)$ 的二次型矩阵为 $B = \begin{bmatrix} 1 & -1 \\ -1 & 2 \end{bmatrix}$。
      (1) 是否存在可逆矩阵 $D$，使 $B = D^T D$？若存在，求出矩阵 $D$；若不存在，说明理由。
      (2) 求 $\displaystyle\max_{x \neq 0} \frac{f(x)}{g(x)}$ 以及对应的 ${x}$，其中 $x = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$。
    > **要让 ${B=D^TD}$, 则存在一个 ${y=Dx}$, 使得 ${y^Ty=x^T(D^TD)x=x^TBx}$ 即可**, 于是对 ${g(x_1,\,x_2)}$ 做分解配方得 ${g(x)=(x_1-x_2)^2+x_2^2}$, 故存在矩阵 ${y=Dx}$, 其中 ${D=  \begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix}}$
    > 代入发现是 ${\dfrac{x^TAx}{x^TBx}}$ 的形式, 于是对 ${B}$ 做正交变换, 发现 ${B}$ 的特征值难求, 但由于本质是要让分母化为 ${y^Ty}$ 的形式, 于是利用 ${(1)}$ 的形式(配方法也可以), 做 ${x=D^{-1}y}$ 变换, 于是有 ${y^T((D^{-1})^TBD^{-1})y}$, 又 ${B=D^TD}$, 故原式化为 ${y^Ty}$, 此时形式为: ${y^T((D^{-1})^TAD^{-1})y}$, 记 ${M=(D^{-1})^TAD^{-1}=\begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}}$, 利用**秩一矩阵**性质得 ${\lambda=2,\,0}$, 求出 ${\xi_1,\,\xi_2}$, 然后分别单位化得到 ${Q}$, 再做一次正交变换将原式化为 ${\dfrac{z^T\Lambda z}{z^Tz}=\dfrac{2z_1^2+0}{z_1^2+z_2^2}}$, 故最大值为 ${2}$, 其中 ${z=(1,\,0)^T,\,y=Qz,\,x=D^{-1}y}$
    * 通过上述例子可以发现, 第一次做变换的时候是要将其转化成规范形, 即**利用配方法或者初等变换法, 当然若特征值符合也可以**, 两次变换可归结为: **求出一个可逆矩阵 ${P}$, 使得 ${f(x)}$ 化为规范型, 使 ${g(x)}$ 化为标准型**[^alg-96]

<!-- added-heading -->

### 正定性、谱分解与矩阵平方根 {#algebra-positive-definiteness}

* 关于正定矩阵
  * 若矩阵为正定矩阵, 形如: ${f(x_1,\,x_2\dots)=(ax_1+bx_2+\dots)^2+(cx_1+dx_2+\dots)^2\dots}$ 的二次型, 若该二次型正定, 意味着其 ${f(x_i) = 0}$ 只有零解, 即其系数矩阵 ${{A=\begin{pmatrix}a & b & \dots \\ c & d & \dots \\ \dots & \dots &\dots \end{pmatrix}}}$ 可逆[^alg-98]
  * 若出现合同的不等式, 则可转化为正定矩阵求解, 如给出 ${|X^TAX|\lt |X^TX|}$, 意味着 ${X^T(-E)X\lt X^TAX\lt X^T(E)X}$, **即 ${X^T(A-E)X\lt 0,\,X^T(A+E)X\gt 0}$, 从而 ${A+E,\,E-A}$ 正定**[^alg-99]
    * 已知三阶实对称矩阵 ${A}$ 的特征值为 ${\lambda_1,\,\lambda_2,\,\lambda_3}$, ${A^*}$ 为其伴随矩阵, 对任意三维列向量 ${X}$ 有: ${|X^TA^*X-X^TAX|\leq aX^TX}$, 求 ${a}$ 的最小值
      > 将不等式转化为 ${X^T(-aE)X\leq X^T(A^*-A)X\leq X^T(aE)X}$, **即 ${B_1=A^*-A+aE,\,B_2=-A^*+A+aE}$ 正定**, 把这两个正定矩阵化成特征值多项式形式, 设 ${B_1}$ 的特征值为 ${\lambda_j}$, ${B_2}$ 的为 ${\lambda_k}$, 则由于正定, 需满足 ${\begin{cases}\lambda_j-\lambda_i+a\geq0 \\ -\lambda_k+\lambda_i+a\geq0\end{cases}}$ 求出 ${a}$ 的最小值即可, **注意这里的特征值相减要和特征向量一一对应.** 若 ${\lambda_i=2,\,3,\,4}$, 则此时 ${a_{min}=10}$[^alg-101]
  * 若 ${Q^TAQ=Λ}$, ${Q^TQ=E}$, 则有 ${A=\sum_i\lambda_iu_iu_i^T}$, 此时根据对应特征向量互相正交, 构成空间的一组基的特性, 有 ${\sum_iu_iu_i^T=E}$, 可以简化运算
    * ${{A=\begin{pmatrix}0 & 1 & -1 \\ 1 & 0 & -1 \\ -1 & -1 & 0 \end{pmatrix}}}$, 正定矩阵 ${B}$ 有 ${B^2=A+2E}$, 求 ${B}$
      > 由于 ${\lambda_{A}=2,\,-1,\,-1}$, 则 ${\lambda_{A+2E}=4,\,1,\,1}$, 又因为对 ${A+2E}$ 进行正交变化后有 ${Q^T(A+2E)Q=Λ\Rightarrow A+2E=QΛQ^T}$, 有 ${B^2=(QΛQ^T)(QΛQ^T)=QΛ^2Q^T}$, 则 ${B=QΛQ^T}$, 所以 ${\lambda_B=2,\,1,\,1}$, 我们有 ${B=QΛQ^T=2u_1u_1^T+u_2u_2^T+u_3u_3^T}$, **由于 ${u_1,\,u_2,\,u_3}$ 是空间的一组正交基, 即: ${u_1u_1^T+u_2u_2^T+u_3u_3^T=E}$, 故 ${B=E+u_1u_1^T}$**[^alg-104]
  * 关于上述做法, 有一个更具一般性的结论:
  > ${A}$ 为 ${n}$ 阶实对称矩阵, ${\alpha_1,\,\alpha_2,\dots,\,\alpha_n}$ 是 ${A}$ 的 ${n}$ 个单位正交特征向量, 分别对应 ${\lambda_1,\,\lambda_2\dots}$, 那么 ${\displaystyle A=\sum_{i=1}^{n}\lambda_i\alpha_i\alpha_i^{T}}$, 证明的话直接暴力证明即可, 具体在 ${880_{comp}(14-3.13)}$

<!-- added-heading -->

### 平方和二次型与线性方程组 {#sums-of-squares-and-systems}

* 关于二次型与方程组问题
  * 给出形式: ${\displaystyle f(x_1,\,x_2,\,...,\,x_n)=\sum_{i=1}^{m}(a_{i1}x_1+a_{i2}x_2+...+a_{in}x_n)^2}$, 计系数矩阵为 ${B}$, ${A=(a_{ij})_{m\times n}}$[^alg-108]
  * 根据二次型正定定义, 不难发现若要其正定, 那么 ${f(x_i)=0}$ 只有零解, 转化成 ${B\mathbf{x}=0}$ 只有零解, 所以要求列满秩 ${r(B)=n}$, 此时自然满足 ${m \geq n}$
  * 在列满秩的情况下, 若 ${m\gt n}$, 意味着行冗余, 多余的行其实是与其他行成比例的, 所以在二次型中会被合并到标准形中[^alg-110]
  * 若 ${m\lt n}$, 此时无论如何二次型不正定, 因为总会存在基础解系, 而对应的标准形中也会存在相应的 ${0}$系数变量, 如 ${y_1^2+0y_2^2+y_3^2}$

---

<!-- added-heading -->

### 分块矩阵的秩：哪些变换保持等价 {#algebra-block-rank}

注意最后的错误步骤, 不能直接进行左乘 A倍, 因为此时乘的是 [E O, O A], R=r(E)+r(A) 不一定可逆, 所以会改变性质, 不可以, 其余的可以进行初等变换(之后再总结这部分内容)

<figure class="fig"><img src="/blog/kaoyan-math/figures/block-matrix-rank.png" alt="原笔记配图 3" width="1120" height="754" loading="lazy" decoding="async"><figcaption>原笔记配图 3</figcaption></figure>

<!-- source-content:algebra:end -->

[^cal1-6]: **勘误或条件说明：** 应同乘 $e^x$，令 $u=e^x\gt 0$，得到一元二次方程 $u^2-2yu-1=0$；正根为 $u=y+\sqrt{y^2+1}$，故反函数为 $x=\ln(y+\sqrt{y^2+1})$。原文的 $e^y$ 和“二元一次方程组”均有误。

[^cal1-8]: **勘误或条件说明：** 令相邻两项都等于 $a$ 得到的是递推映射的不动点方程 $a=f(a)$，不是直接求任意第 $n$ 项的公式。若用于求数列极限，还须先证明收敛，并满足可把极限代入递推关系的连续性等条件。

[^cal1-16]: **勘误或条件说明：** 趋于无穷大不是每一项或每个函数值真的等于无穷大，而是：对任意给定的 $M\gt 0$，充分接近极限位置后均有 $|f|\gt M$（数列对应充分大的下标）。无界只说明能超过任意界限，不要求此后始终超过。

[^cal1-19]: **勘误或条件说明：** $x_n\to a\ne0$ 不表示 $x_n$ 恒定，而表示它最终远离零且符号与 $a$ 一致。若乘积趋于 $+\infty$，则 $a\gt 0$ 时 $y_n\to+\infty$，$a\lt 0$ 时 $y_n\to-\infty$；若“无穷大”只指绝对值，则可以推出 $|y_n|\to\infty$。

[^cal1-20]: **勘误或条件说明：** 函数与数列服从同样的乘积极限规则。若两个因子的绝对值都趋于无穷大，则乘积的绝对值也趋于无穷大；若都趋于 $+\infty$，乘积也趋于 $+\infty$。振荡导致该结论失效的例子，必定没有满足这里的两个无穷大前提。

[^cal1-21]: **勘误或条件说明：** 本条关于两个无穷大数列乘积的结论在相同的符号/绝对值约定下成立，但原因是极限定义和乘法规则，不是数列的离散性。连续变量的函数在相同条件下也有相同结论。

[^cal1-32]: **勘误或条件说明：** 原文给出的狄利克雷函数处处不连续，也处处不可导，不能作为“某点可导”的反例。可改用 $F(x)=(x-x_0)^2D(x-x_0)$，其中 $D$ 是原文的狄利克雷函数；它在 $x_0$ 可导且导数为零，但在邻域内其他点不连续。

[^cal1-36]: **勘误或条件说明：** 连续性是函数在某点的性质，不能直接说数值 $g(f(x_0))$ 不连续。这里应理解为 $g$ 在输入点 $f(x_0)$ 不连续；即便如此，复合函数仍可能连续，例如 $f$ 恒等于一个常数且 $g$ 在该常数处有定义时，复合函数就是常值函数。

[^cal1-41]: **勘误或条件说明：** 取低阶项必须排除主项抵消。两个同阶无穷小相加或相减时，首项系数可能相消，结果可以升阶甚至恒为零；两个变上限积分作差也须检查上下限对应主项的系数，不能只比较阶数。

[^cal1-45]: **勘误或条件说明：** 原式 $\lim_{x\to x_0}=A$ 漏写了被取极限的函数，应读为 $\lim_{x\to x_0}f(x)=A$。若未给出 $f(x_0)$，连续性是尚未确定，而不是必然不连续；原文具体的分段例子因 $C\ne0$ 才在零点不连续。

[^cal1-47]: **勘误或条件说明：** 由给定极限可得 $f'(x)\to0$，不是趋于 $A$；且在足够小的去心邻域中，$f'(x)$ 与 $A$ 同号。这保证左右两个连通的单侧区间各自单调，但不保证跨越两侧的整个去心集合单调，也不控制 $f(x_0)$。若另有该点连续性等条件，可进一步讨论。

[^cal1-50]: **勘误或条件说明：** 有限的左导数、右导数是以 $f(x_0)$ 为基点的差商极限 $f'_\pm(x_0)=\lim_{h\to0^\pm}[f(x_0+h)-f(x_0)]/h$；两者存在且相等，就必有 $f'(x_0)$ 存在，并保证 $f$ 在该点连续。这与 $\lim_{x\to x_0^\pm}f'(x)$ 完全不同。原分段例子中，去心处 $f'(x)=2x$ 的左右极限都为零，但由于 $f(0)=C\ne0$，原函数在零点不连续，左右差商不能给出相等的有限导数。

[^cal1-54]: **勘误或条件说明：** 本例的后续结论使用的是 $x_0=0$。由 $f(x)/x^2\to A$ 可知：$A=0$ 时 $f=o(x^2)$；$A\ne0$ 时 $f\sim Ax^2$，与 $x^2$ 同阶，但只有 $A=1$ 才与 $x^2$ 等价。结合零点可导带来的连续性，可得 $f(0)=f'(0)=0$。

[^cal1-55]: **勘误或条件说明：** 洛必达法则并不普遍要求导函数连续，也不存在原文所述统一的“可导到 $n$ 阶只能用到 $n-1$ 阶”规则。需检查不定式类型、去心区间上的可导性、分母导数非零以及导数比极限等条件。原函数比值极限存在，不能反向推出导数比值极限存在；重复使用时每一步都须满足相应条件。

[^cal1-56]: **勘误或条件说明：** 此处应比较邻域中的 $f(x)$ 与 $f(0)=0$，不能说固定数值 $f(0)$ “在邻域均大于零”。当 $A\gt 0$ 时零点为严格极小值点；当 $A\lt 0$ 时为严格极大值点；当 $A=0$ 时仅凭这些条件不能判定。

[^cal1-66]: **勘误或条件说明：** 若 $f$ 在 $x_0$ 可导且 $f(x_0)=0$，则 $|f|$ 在该点可导当且仅当 $f'(x_0)=0$；只有非零的一阶斜率才必然形成不可导的尖角。例如 $f(x)=x^3$ 虽穿过零点，$|x|^3$ 在零点仍可导。

[^cal1-69]: **勘误或条件说明：** 须补充 $g$ 的正则性。若 $\phi,g$ 均在 $x_0$ 可导且 $g(x_0)=0$，则 $\phi|g|$ 可导当且仅当 $\phi(x_0)g'(x_0)=0$，导数为零。只有 $g'(x_0)\ne0$ 的简单零点情形，$\phi(x_0)=0$ 才成为充要条件。令 $h=x-x_0$，正确的一阶主项是 $\phi(x_0)|g'(x_0)|\,|h|$，而不是在任意 $x_0$ 处直接用 $x$ 作差商分母；压掉一阶尖角也不表示整个图像真的变成直线。

[^cal1-72]: **勘误或条件说明：** 有限的高阶差商极限确实能推出这里的一阶导数为零，但不能自动保证二阶或更高阶导数存在。不能反向使用洛必达来补出这些导数；上文 $x^2+x^3\sin(1/x)$ 的例子已经说明二阶差商有极限而二阶导数未必存在。

[^cal1-73]: **勘误或条件说明：** 洛必达或柯西中值定理不要求导函数连续；问题在于仅有一点处可导不足以保证它们所需的区间条件。所写差商分解适用于 $g(x_0)$ 为有限值且相应表达式有意义的情形；若 $g\to\infty$，不能减去“$g(x_0)=\infty$”来构造差商。在 $f$ 连续且分子趋零、分母绝对值趋于无穷大的情形，原比值直接趋零。

[^cal1-75]: **勘误或条件说明：** 对幂型无穷小，有限比值通常要求分子阶数不低于分母，而不是原文的“不高于”。真正应检查的是能否恢复覆盖全部实数趋近路径的导数差商。例如 $f(0)=0$ 且 $\lim f(x)/x^p=L$ 有限、$p\ge1$ 时，$p=1$ 给出导数 $L$，$p\gt 1$ 给出导数零。

[^cal1-80]: **勘误或条件说明：** 原不等式直接除以 $x$ 时，$x\lt 0$ 会反向，而且单侧上界不足以夹出极限。正确做法是 $|f(x)/x|\le|x|\to0$，由夹逼定理得到 $f'(0)=0$。

[^cal1-84]: **勘误或条件说明：** 一点处的带 $o(x^2)$ 泰勒展开是局部信息，不能直接忽略余项来控制整个固定区间上的积分。若在整个对称区间上 $f''\ge0$，可由切线不等式 $f(x)\ge f(0)+f'(0)x$ 推得 $b\ge2af(0)$；若 $f''\le0$，不等号应反向。

[^cal1-85]: **勘误或条件说明：** 不同中文教材的“凹/凸”命名可能不同，应以图像和导数符号为准。标准英文术语中，$f''\ge0$ 的 convex（向上弯）函数图像在切线之上、弦线之下；$f''\le0$ 的 concave（向下弯）函数相反。原文的凹凸名称不宜直接套用英文定义。

[^cal1-93]: **勘误或条件说明：** 公式左侧缺少求导记号。应为 $\dfrac{d^n}{dx^n}(1/x)=(-1)^n n!/x^{n+1}$，不是普通幂 $(1/x)^n$ 等于右侧。

[^cal1-101]: **勘误或条件说明：** 提取 $x^2$ 后，括号内应是 $1-x^{-6}$，不是 $1-x^{-3}$。正确写法为 $(1-x^6)^{1/3}=-x^2(1-x^{-6})^{1/3}$，据此展开仍得原极限为零。

[^cal1-104]: **勘误或条件说明：** 偶次幂由 $|x|$ 与 1 的关系决定，不能仅按 $x\lt 1,x=1,x\gt 1$ 分类。此式在 $|x|\lt 1$、$|x|=1$、$|x|\gt 1$ 时的极限分别是 $1,0,-1$；下条各种幂数列也应分别检查 $x=-1$ 和 $x\lt -1$ 的情形。

[^cal1-108]: **勘误或条件说明：** 左端应为 $dy/dx$：在 $dx/dt\ne0$ 时，$dy/dx=(dy/dt)/(dx/dt)$。若由 $te^y+y+1=0$ 隐式确定 $y(t)$，先求 $dy/dt=-e^y/(te^y+1)$（分母非零），再除以 $dx/dt$。

[^cal1-111]: **勘误或条件说明：** 两个参数函数连续只保证参数曲线连续，不自动保证它定义一个连续的单值函数 $y=f(x)$。还须确认能在所讨论范围内把参数写成连续的 $t=t(x)$；例如 $\phi$ 连续且局部严格单调时，可通过其连续反函数得到结论。

[^cal1-118]: **勘误或条件说明：** 内点极大值满足 $f''(x_0)\le0$ 须以前提 $f''(x_0)$ 存在为基础，但极大值本身不要求 $f'$ 在整个邻域单调递减。$f'(x_0)=0$ 只说明一阶变化率为零；$f''(x_0)=0$ 也不能判定函数不变或是否取极值。

[^cal1-120]: **勘误或条件说明：** 极值的定义本身不要求连续，例如 $f(0)=1$、其他点 $f(x)=0$ 时，零点就是一个不连续的极大值点。通常考试中所说的图像拐点要求相应的连续性，但不能把这个要求同时套到所有极值点上。

[^cal1-121]: **勘误或条件说明：** 驻点指函数可导且一阶导数为零的点，不可导点不能称为驻点。适当连续性条件下，一阶导数的两侧符号变化用于判断极值；拐点则要检查凹凸性是否改变，通常通过邻域两侧二阶导数的符号变化来判断。

[^cal1-122]: **勘误或条件说明：** $f'(x_0)=0$ 已经是驻点条件；再有 $f''(x_0)\ne0$ 时，二阶判别法确认的是严格极值。$f''(x_0)=0$ 本身不能确认拐点；在相应可导条件下再有 $f'''(x_0)\ne0$ 才给出常用的充分判据。

[^cal1-125]: **勘误或条件说明：** 端点取到最值的结论需有端点属于定义域、相应连续性及最值确实存在等条件，通常在有限闭区间上使用。开区间或无穷区间上，端点极限可能只是上确界/下确界，函数未必取得最大值或最小值。

[^cal1-126]: **勘误或条件说明：** 函数在一点可导，首先必须在该点有定义。“无定义点可导”本身不成立；若题目要求补定义，应先指定该点函数值，再用差商检查连续性与可导性。分段连接点仍须单独按定义处理。

[^cal1-128]: **勘误或条件说明：** 此结论适用于多项式，或具有适当光滑分解 $f(x)=(x-a)^m h(x)$、$h(a)\ne0$ 的情形。特别地，$m=1$ 时导数在该点不为零；所谓“零重根”表示该点不再是导函数的根。

[^cal1-130]: **勘误或条件说明：** 含 $o(C)$ 的等式是 $C\to0$ 时的局部展开，不是对任意大小 $C$ 的全局线性关系。固定 $x$ 后除以 $C$ 取极限，才得到 $A=f'(x)$。

[^cal1-132]: **勘误或条件说明：** 应先由增量式得 $f'(x)=f(x)-1$，再解微分方程，得到 $f(x)=1+Ce^x$。微分应写作 $df=(f(x)-1)\,dx$；$o(\Delta x)$ 是函数增量中的余项，不是微分本身的一部分。

[^cal1-138]: **勘误或条件说明：** 本式混淆了两个求导公式。正确的是 $(f^2)'=2ff'$，而 $(ff')'=(f')^2+ff''$。遇到 $ff'$ 时可令辅助函数为 $f^2$，但不能把 $(ff')'$ 与 $(f^2)'$ 写成相等。

[^cal1-156]: **勘误或条件说明：** 拉格朗日余项中的 $\xi$ 应位于 $0$ 与 $x$ 之间。$x\gt 0$ 时写 $(0,x)$，$x\lt 0$ 时应写 $(x,0)$。原不等式对 $x\ne0$ 可按此证明；零点处需先作连续延拓才可写原商式。

[^cal1-160]: **勘误或条件说明：** 应选取满足 $f(c)\ne0$ 的内点 $c$，不能任取内点。这样的点由非常数性及两端函数值为零保证存在。再分别在 $[a,c]$、$[c,b]$ 使用中值定理，两条割线斜率才必定一正一负。

[^cal1-168]: **勘误或条件说明：** 左端应为 $m(b-a)$，不应再乘一个 $dx$。在 $f$ 连续、$m$ 和 $M$ 分别为区间最小值和最大值时，$m(b-a)\le\int_a^b f(x)\,dx\le M(b-a)$，再用介值定理。

[^cal1-174]: **勘误或条件说明：** 辅助函数第二个积分的上限应为 $x$。正确地取 $F(x)=\int_a^x tf(t)\,dt-\frac{a+x}{2}\int_a^x f(t)\,dt$，得 $F'(x)=\frac12[(x-a)f(x)-\int_a^x f(t)\,dt]=\frac{x-a}{2}[f(x)-f(\xi)]\ge0$（使用相应中值定理条件）；再由 $F(a)=0$ 完成证明。

[^cal1-177]: **勘误或条件说明：** 这里无需洛必达，直接把上一步的 $\Delta F=f(\xi)\Delta x$ 除以 $\Delta x$，再由连续性令 $\xi\to x$ 即可。在证明 $F$ 可导时预先对 $F$ 使用洛必达，可能循环假设了正在证明的可导性。

[^cal1-186]: **勘误或条件说明：** 有限个间断点保证有限闭区间上黎曼可积时，还必须有界。排除“极限为无穷大”的间断点并不自动排除无界振荡，例如在零点补定义的 $\sin(1/x)/x$。下文列出的“有界且只有有限个间断点”才是完整的常用充分条件。

[^cal1-196]: **勘误或条件说明：** $\cos x\gt \sin x$ 并非在整个 $[-\pi/2,\pi/2]$ 上成立，因此不能按原文直接放缩。用换元与奇偶性可得 $M=2/3$，$N=-2$，所以 $M\gt N$。

[^cal1-202]: **勘误或条件说明：** 估计须写成绝对值形式：$|F(x+\Delta x)-F(x)|\le M|\Delta x|$。原写法在 $\Delta x\lt 0$ 时不能作为所需的双侧控制；加上绝对值后再用夹逼即可证明连续。

[^cal1-204]: **勘误或条件说明：** 极限下标应为 $x\to x_0$，不是 $x\to x$。在可去间断点处，变上限积分的导数等于 $\lim_{x\to x_0}f(x)$，可以不同于原来人为指定的 $f(x_0)$。

[^cal1-206]: **勘误或条件说明：** 原上限是 $x^2$，故 $u=x^2-t^2$ 后应为 $\frac12\int_{x^2-x^4}^{x^2}f(u)\,du$；只有原上限为 $x$ 时才得到 $\frac12\int_0^{x^2}f(u)\,du$。若 $f$ 连续，题设原式的导数为 $xf(x^2)-(x-2x^3)f(x^2-x^4)$。

[^cal1-209]: **勘误或条件说明：** 同敛散不表示被积函数可以等价替换，也不保证积分的渐近系数相同。应检查真正的局部等价、固定符号或可控制的误差；泰勒展开后逐项积分也需要余项在积分区间内受到适当控制。

[^cal1-218]: **勘误或条件说明：** 正确恒等式是 $(1+\tan x)^2=\sec^2x+2\tan x$，原文漏了系数 2。利用求导关系做分部积分时，也应把这些系数一起匹配。

[^cal1-228]: **勘误或条件说明：** 第二段仍应在 $[\pi/4,\pi/2]$ 上积分：$I_2=\int_{\pi/4}^{\pi/2}(\pi/2-\theta)\,d(\sin^4\theta)$。若改到 $[0,\pi/4]$，必须同时按换元变换被积表达式，不能只改上下限。与第一段合并后，原题积分为 $1/2$。

[^cal1-237]: **勘误或条件说明：** 对三角有理式使用 $u=\tan x$ 的常用判据是同时变号后保持不变：$R(-\sin x,-\cos x)=R(\sin x,\cos x)$，不是原文写的取负值。一般情形仍可使用半角万能代换。

[^cal1-239]: **勘误或条件说明：** 积化和差漏了 $1/2$：$\sin x\cos(nx)=\frac12[\sin((1+n)x)+\sin((1-n)x)]$，积分右侧也须保留该系数。

[^cal1-241]: **勘误或条件说明：** 实数范围内应使用 $\ln|c\sin x+d\cos x|$，除非已知括号内恒正。在分母非零的区间上，配项后的原函数为 $Ax+B\ln|c\sin x+d\cos x|+C$。

[^cal1-243]: **勘误或条件说明：** 这里重复出现的恒等式也应为 $(1+\tan x)^2=\sec^2x+2\tan x$，不能漏掉 2。

[^cal1-249]: **勘误或条件说明：** 第二个 Beta 函数积分表达式漏写了微分 $du$，应为 $\int_0^1u^{p-1}(1-u)^{q-1}\,du$，并满足题中给出的 $p,q\gt 0$。

[^cal1-252]: **勘误或条件说明：** 本题被积式在 $\sin x,\cos x$ 同时变号后不变，故应是 $R(-\sin x,-\cos x)=R(\sin x,\cos x)$。$\tan x$ 换元和后续 $B(3,3)=1/30$ 的计算是正确的。

[^cal1-253]: **勘误或条件说明：** 两积分相等来自倒数代换 $x=1/t$ 及其雅可比因子，不是关于直线 $x=1$ 的普通镜像对称。做变量代换时必须同时变换 $dx$，才得到这里的面积相等。

[^cal1-255]: **勘误或条件说明：** 令 $u=1/x-x$、$v=1/x+x$ 后，第二个分母应为 $v^2-3$，不是 $v^2+3$；对数项系数应为 $-1/(4\sqrt3)$。对 $x\gt 0$，正确原函数为 $-\frac12\arctan(1/x-x)-\frac1{4\sqrt3}\ln\left|\frac{1/x+x-\sqrt3}{1/x+x+\sqrt3}\right|-\frac13\arctan(x^3)+C$。端点极限相减得原定积分 $\pi/3$。

[^cal1-257]: **勘误或条件说明：** 原积分 $\int_0^1x^t/\ln x\,dx$ 在 $x=1$ 发散，不能直接套用参数求导；且 $\int_0^1x^t\,dx=1/(t+1)$（$t\gt -1$），不是 $t+1$。收敛的差分形式 $J(t)=\int_0^1(x^t-1)/\ln x\,dx$ 满足 $J'(t)=1/(t+1)$、$J(0)=0$，故 $J(t)=\ln(1+t)$。因此下一条两幂相减的例子仍正确，结果为 $\ln2$。

[^cal1-262]: **勘误或条件说明：** 区域是全圆盘，角度应取 $0$ 到 $2\pi$，不是 $\pi/2$。令 $D_r$ 为半径 $r$ 的圆盘、$L_r$ 为其逆时针边界，正确线积分为 $\oint_{L_r}(-f_y\,dx+f_x\,dy)$，对应面积分区域也须随 $r$ 变化为 $D_r$。于是 $I=\int_0^1 r\,dr\iint_{D_r}(f_{xx}+f_{yy})\,dA=\pi\int_0^1r(1-e^{-r^2})\,dr=\pi/(2e)$。原最终数值正确，但中间角度、微分形式和区域记号有误。

[^cal2-266]: **勘误或条件说明：** 在函数于该点有定义的前提下，连续等价于极限存在且等于函数值，反向也成立；仅“极限存在”才不足以推出连续。

[^cal2-268]: **勘误或条件说明：** 不同坐标方向的偏导数不必相等，不同方向的方向导数也不必相等。梯度作为偏导数组成的向量存在，不等于全微分存在。常用充分条件是偏导数在邻域内存在并在该点连续，而不只是笼统地说各路径极限存在。

[^cal2-269]: **勘误或条件说明：** 沿 y=x² 有 f(x,x²)=1/2，而沿坐标轴函数为 0，因此该函数在原点不连续，更不可微；原点两个偏导数仍为 0。真正的“可微但偏导不连续”例子可用 g(x,y)=x²sin(1/x)（x≠0），并在 x=0 时定义为 0。

[^cal2-274]: **勘误或条件说明：** 隐函数定理还要求 F 在邻域内具有适当的连续偏导数、F(x₀,y₀)=0 等条件。F_y=0 不排除隐函数存在，但导数商呈未定式或某个极限存在本身不能证明隐函数存在。

[^cal2-275]: **勘误或条件说明：** 所写差商用 x 作增量时应令 x→0，而不是 x→x₀。较清楚的写法是 f_x(x₀,y)=lim_{h→0}[f(x₀+h,y)−f(x₀,y)]/h，再对 y 求导。

[^cal2-284]: **勘误或条件说明：** 全微分余项应按增量距离 ρ=√((Δx)²+(Δy)²) 写成 Δz=AΔx+BΔy+o(ρ)，并令 ρ→0。o(ρ) 是一类余项的记号；若它用作分母，应明确指某个非零函数 q=o(ρ)，不能把记号本身当作一个确定数。

[^cal2-285]: **勘误或条件说明：** 此处趋近 (a,b)，距离应为 √((x−a)²+(y−b)²)，不是 √(x²+y²)。若分子除以某个 q=o(该距离) 的极限是有限 C，则分子除以该距离的极限为 0，从而得 f_x(a,b)=−3、f_y(a,b)=4。

[^cal2-290]: **勘误或条件说明：** 不可微点不是通常定义的驻点；驻点满足各一阶偏导数存在且为 0。驻点与不可微点都只是极值候选，需进一步判别，不能求出驻点就断言极值已找到。

[^cal2-293]: **勘误或条件说明：** M_i、M_j、M_k 应理解为候选点或候选值，并非都已证实为极值。全局最值是相对于可行域的局部极值，但边界最值未必是原函数在整个开邻域内的无约束极值。还需检查约束奇异点以及最值是否实际取得。

[^cal2-299]: **勘误或条件说明：** 只有约束排除了 (0,0)，且所研究驻点满足该齐次线性方程组时，才可要求非零解并令系数行列式为 0；“存在约束”本身不能保证非零解。

[^cal2-303]: **勘误或条件说明：** 椭圆截线以原点为中心，但原点不在椭圆截线上；它位于截椭圆所围区域内。到中心的最大、最小距离确实分别给出长、短半轴。

[^cal2-304]: **勘误或条件说明：** 目标应为 d²，且需要两个约束：L=x²+y²+z²+λ(x²/3+y²/2+z²−1)+μ(x+y+z)。该 L 才能导出原文后续方程及 d²=−λ。最终两半轴为 √((11+√13)/6) 和 √((11−√13)/6)。

[^cal2-307]: **勘误或条件说明：** 终点 B 应使 x+2y 最大，而不是最小。起点取最小值、终点取最大值，积分最大值才是 4√2。

[^cal2-308]: **勘误或条件说明：** 二阶偏导存在不能保证全局最值存在。常用保证是函数在非空紧集（闭且有界区域）上连续；若区域开或无界，即使函数光滑也可能不取得最大值或最小值。

[^cal2-312]: **勘误或条件说明：** “一定在边界”以全局最值存在并实际取得为前提；若区域或函数不满足相应条件，可能只有上、下确界，甚至无界，不能直接断言边界最值存在。

[^cal2-315]: **勘误或条件说明：** 原函数为 (x−3)²+y²+1，唯一驻点是 (3,0)，最小值为 1；(2,0) 不是驻点，三阶导数为 0，Hessian 判别式为 4，并非退化例子。高阶导数判别法还须以首个非零导数存在等条件为前提。

[^cal2-317]: **勘误或条件说明：** 一般不能仅检查所有直线路径就证明多元极值，还需排除其他路径。本例可直接用 x⁴+y⁴≥0，且仅在原点等于 0，证明原点为严格全局最小值点。

[^cal2-322]: **勘误或条件说明：** 题设极限只能推出 f(x,y)→0，不能确定题目单独定义的 f(0,0)。若 f(0,0)≠0，坐标方向差商不收敛；若 f(0,0)=0，差商含 |h|/h，左右极限不同。因此两个偏导数均不存在的结论仍正确，但原推导不能无说明删去 f(0,0)。

[^cal2-330]: **勘误或条件说明：** 原式中的第二个 2u₁₁″ 应为 2u₂₁″。在足以使用链式法则并交换混合偏导的条件下（如邻域内 C²），关系为 u₁₁″+4u₁₂″+4u₂₂″=0，结合其余等式可得 −4x/3。仅说二阶偏导存在不足以自动保证这些操作。

[^cal2-338]: **勘误或条件说明：** 平移后的极坐标仍有面积元 dxdy=r dr dθ。半径与角度范围必须由整个积分区域确定；只有完整圆盘时才直接采用 0≤r≤√(a²+b²)、0≤θ≤2π。

[^cal2-340]: **勘误或条件说明：** 变限积分中的积分变量应使用哑变量：F(x)=∫ₓᵇf(t)dt，F′(x)=−f(x)。在适当可积条件下，所示二重积分等于 (1/2)(∫ₐᵇf(t)dt)²。

[^cal2-344]: **勘误或条件说明：** 交换因、变量并取导数倒数需要局部可逆、相关导数非零等条件；原函数的反函数写作 x(y) 更清楚。变形时要单独检查可能因除法而遗漏的常值解或特殊解。

[^cal2-346]: **勘误或条件说明：** 同乘 eʸ 后左边是 (eʸ)′+eʸ，而不只有 (eʸ)′。令 u=eʸ 得 u′+u=sin x，再解一阶线性方程；还须 u&gt;0 才能取 y=ln u。

[^cal2-355]: **勘误或条件说明：** 该结论针对同一个非齐次线性方程。两个特解之差是齐次方程的一个解，而非自动成为通解；须找到足够多个线性无关解才构成齐次通解。“非线性相减/通解”应理解为线性组合或线性无关的齐次解。

[^cal2-356]: **勘误或条件说明：** 三特解构造二阶非齐次线性方程通解时，需两个差解线性无关。仿射组合系数满足 C₁+C₂+C₃=1，不要求两两不同；第三个系数并非独立任意常数。一般阶数的方程不能只凭三个特解套用这一形式。

[^cal2-366]: **勘误或条件说明：** 通项趋于 0 是级数收敛的必要条件，不是任意级数都具备的条件，也不是收敛的充分条件。通常先用题设“收敛”来筛选参数，再检验候选值。

[^cal2-367]: **勘误或条件说明：** 若题意为 Σ(uₙ+vₙ) 收敛，能直接推出的是 uₙ+vₙ→0，不能分别推出 uₙ→0、vₙ→0。写成两个无穷和相加或相减前，应先确认各和有定义，避免把发散量形式相消。

[^cal2-370]: **勘误或条件说明：** 乘积趋于 0 不能推出商趋于 0，例如 uₙ=vₙ=1/n。等价无穷小比较通常用于最终同号的正项级数；序列收敛与级数收敛也应区分，需明确比较条件才能转移敛散结论。

[^cal2-372]: **勘误或条件说明：** 已收敛级数按原顺序作有限项分组仍收敛，但反向不能由分组和收敛推出原级数收敛，例如 1−1+1−1+⋯。这里应明确是在分组或构造相邻项之和的级数，不能把条件收敛级数任意重排。

[^cal2-376]: **勘误或条件说明：** 泰勒拆出的发散部分可能相互抵消；只有一个发散部分而其余部分已证明收敛等条件下，才能据此判发散。制造 (−1)ⁿ 也必须保持原通项恒等，不能任意改变符号。

[^cal2-377]: **勘误或条件说明：** 对有正负号的级数，通项等价本身不能直接推出同敛散。若差级数 Σ(uₙ−vₙ) 确已收敛，则可由部分和关系推出两者同收敛或同发散；要进一步判定条件收敛，还需检验绝对值级数。

[^cal2-382]: **勘误或条件说明：** 使用 ln a 需要 a&gt;0。此时展开首项为 (ln a−1/2)/n，只有 a=√e 时抵消；该值下余项首项为 1/(4n²)，故级数收敛，其余 a&gt;0 时发散。

[^cal2-387]: **勘误或条件说明：** 从 n=1 起求和时 S₁=Σxⁿ=x/(1−x)，不是 1/(1−x)。正确积分为 G=1/(1−x)²−5/(1−x)−4ln(1−x)−x+C，G(0)=0 给 C=4。下一行是在积分而非求导；S=G/x 在 x=0 处取连续延拓值 0，且需 |x|&lt;1。

[^cal2-393]: **勘误或条件说明：** 求导移指标时不能漏掉 a₁=1：S′=1+Σₙ₌₁∞(n+1)aₙ₊₁xⁿ。正确关系为 S′=1+xS′+S/2，而非 S=1+xS′+S/2。由 S(0)=0 得 S=2[(1−x)^(−1/2)−1]，|x|&lt;1。

[^cal2-395]: **勘误或条件说明：** 无穷个连续函数的逐点和不一定连续。幂级数在收敛半径内部局部一致收敛并可逐项求导积分；端点应单独检验收敛，只有满足如 Abel 定理的条件时，才能用内侧极限确定端点和。

[^cal2-398]: **勘误或条件说明：** 应保留系数：1/(x−2)=−(1/2)/(1−x/2)，对应展开在 |x|&lt;2 内成立。

[^cal2-400]: **勘误或条件说明：** 部分分式系数应为 1/2：1/(n²−1)=(1/2)[1/(n−1)−1/(n+1)]，不是 2；第二部分仍带减号。正确结果 S(1/2)=5/8−(3/4)ln 2。

[^cal2-401]: **勘误或条件说明：** 奇偶项提取针对幂级数 F(x)=Σaₙxⁿ：奇数次幂部分为 [F(x)−F(−x)]/2，偶数次幂部分为 [F(x)+F(−x)]/2。原式直接写 −uₙ 并不能筛选奇偶项，求和上下限也应一致。

[^cal2-402]: **勘误或条件说明：** 原级数从 n=1 起，改成 n=0 后多出常数项 2，必须减去。正确和为 (√2/x)ln[(1+x/√2)/(1−x/√2)]−2，|x|&lt;√2；在 x=0 处按极限取值 0。

[^cal2-404]: **勘误或条件说明：** 这里的例子含奇数次幂，实际上缺偶数次幂，不能与小标题字面混淆。对 |x|&lt;1 求导得 −1/(1+x²)，结合 S(0)=0 得原级数和为 −arctan x；x=±1 也分别条件收敛。

[^cal2-410]: **勘误或条件说明：** 原文 Guess 应为 Gauss（高斯公式）。

[^cal2-415]: **勘误或条件说明：** 该距离公式要求两平面平行且 A、B、C 采用相同的归一化表示。若两平面相交，距离为 0；距给定平面 d 的平行平面有两个还要求 d&gt;0，d=0 时只有原平面。

[^cal2-416]: **勘误或条件说明：** 三个点必须不共线，否则叉积为零，无法由它们唯一确定一个平面。

[^cal2-420]: **勘误或条件说明：** 对称式中只有非零方向分量才可作分母。若某分量为 0，应把对应坐标写成常数；参数式可统一处理。

[^cal2-422]: **勘误或条件说明：** 两条异面直线不确定同一个平面。若方向不平行，τ₁×τ₂ 可给出公垂线方向，分别通过每条已知直线及该方向建立两个辅助平面，其交线才是所求；平行情形需另行处理。

[^cal2-423]: **勘误或条件说明：** 用于正投影的辅助平面须包含原直线，并包含投影平面的法向方向。若原直线垂直投影平面，其投影退化为一点，不能强行写成两平面的交线。

[^cal2-424]: **勘误或条件说明：** 这种过 L₂ 且平行于 L₁ 的辅助平面法适合方向不平行的两直线。若两直线平行，这样的平面不唯一，任取一平面所得点面距离未必等于线线距离，应另用平行线距离公式或公垂线。

[^cal2-426]: **勘误或条件说明：** 第一类曲线积分对方向不敏感。若以端点 A、B 参数化为 r(t)=A+t(B−A)，0≤t≤1，则 ds=‖B−A‖dt；方向提示主要用于避免参数与区间配错，而非改变该积分符号。

[^cal2-427]: **勘误或条件说明：** 法向量点乘投影方向为 0，通常用于求光滑曲面的投影轮廓候选，而非直接得到整个投影区域。区域须由“存在被消去坐标使原条件成立”判断；曲面自身边界、奇点等也可能贡献投影边界。

[^cal2-428]: **勘误或条件说明：** (2x,2y−z,2z−y) 是曲面的梯度/法向量，不是切向量。配方为 x²+3y²/4+(z−y/2)²=1，故投影区域为 x²+3y²/4≤1，边界曲线为 x²+3y²/4=1、z=0。

[^cal2-432]: **勘误或条件说明：** 在三维空间中，单独 x=1 表示平面，不能唯一指定旋转轴。平移应明确写出新旧坐标关系，例如绕 x=a、y=b 的轴时令 x′=x−a、y′=y−b，再逆代换。

[^cal2-435]: **勘误或条件说明：** 用点的位置向量与轴方向保持定角来求旋转面，适用于母线与轴相交，并把交点作为向量起点的情形；不相交的异面母线绕轴旋转不能直接用此锥面公式。

[^cal2-442]: **勘误或条件说明：** 前面规定 λ&gt;0，后面却写 λ≤0，互相矛盾。按前面的正向射线约定，应为 x&gt;0、|y|≤x、xz=y²；边界 |u|=1 必须保留非严格不等式。若包含顶点，单独加入 (0,0,0)，不能只改成 x≥0 而放入多余 z 轴点。消去的参数是 λ、u，不是未定义的 μ。

[^cal2-443]: **勘误或条件说明：** x′=x−a 等是坐标变换，变换后的顶点实际为 (0,0,0)。原坐标参数式为 (a,b,c)+λ[(1,u,u²)−(a,b,c)]，|u|≤1。朝准线方向的射线取 λ≥0，整条母线取 λ∈R；λ≤0 则选了相反方向，必须先明确所求锥面的约定。

[^alg-3]: **勘误或条件说明：** 勘误：一般矩阵等价需要允许交换、非零倍乘、倍加三类初等行/列变换，单靠互换并不足够。乘积可逆的直接证明是 (P1*P2)^(-1)=P2^(-1)*P1^(-1)；原式直接使用乘积的逆不能作为其存在性的证明。

[^alg-7]: **勘误或条件说明：** 补充：B 的 r 个线性无关列确实给出 Ax=0 的 r 个线性无关解，但只有 A 为方阵时才能称为对应零特征值的特征向量。原段开头的 A 是一般 m×n 矩阵。

[^alg-9]: **勘误或条件说明：** 勘误：齐次解空间维数是零度 d=n-r(A)，不是秩 r(A)。若 Ax=b 相容且 b 非零，则非齐次解中最多有 d+1 个线性无关向量；若 b=0，则最多为 d 个。

[^alg-10]: **勘误或条件说明：** 勘误：例中的通解应写为 k1*xi1+k2*xi2+eta，两个齐次解项之间是加号。可取 eta、eta+xi1、eta+xi2 得到三个线性无关解的前提是零度为 2 且 b 非零，而非仅有 r(A)=2。

[^alg-12]: **勘误或条件说明：** 补充：若存在 X 使 AX=B，则 B 的列空间包含在 A 的列空间中，因此 A^T x=0 与上下堆叠 A^T、B^T 的方程确实同解。若“同解”仅指 Ax=0 与 Bx=0，则得到的是行空间等价，不能一般推出转置方程的上述结论。

[^alg-15]: **勘误或条件说明：** 勘误：AX=B 相容并不要求 r(A)=r(B)。应使用 r([A,B])=r(A) 以及转置不改变秩，得到 r([A^T;B^T])=r(A^T)。原文把分块拼接写成 B^T A^T、A^T B^T 的乘积，既不具有所需含义，也可能维数不相容。

[^alg-17]: **勘误或条件说明：** 勘误：实矩阵的一般结论是 Ax=0 与 A^T A x=0 同解；A^T x=0 与 A A^T x=0 同解。原文把转置的位置写错了。

[^alg-22]: **勘误或条件说明：** 勘误：正交条件应为内积 xi^T*xi3=0，不能写成向量叉积为零。该三阶例中还需区分特征值 k 与 1；当 k≠1 时，1 特征空间是 x1+x2+x3=0，主元在第一列是这条约束的结果，与 A 的对角元是否非零无关。

[^alg-29]: **勘误或条件说明：** 补充：r(alpha*beta^T)=1 要求 alpha、beta 都非零；若任一向量为零，则矩阵秩为零。后续关于非零秩一矩阵的结论使用这一前提。

[^alg-30]: **勘误或条件说明：** 勘误：n&gt;1 的秩一矩阵 A 不可逆，因此 |A^(-1)| 没有定义。可以计算 |A+kE|；若还要使用其逆，则需满足相应非零特征值条件。

[^alg-32]: **勘误或条件说明：** 勘误：只有 tr(A)≠0 时，tr(A) 才是一个单重特征值且零为 n-1 重特征值。tr(A)=0 时两者合并，零的代数重数为 n。

[^alg-38]: **勘误或条件说明：** 勘误：若 xi 为对应非零特征值 lambda 的单位列向量，则 A=lambda*xi*xi^T。原式 xi^T*lambda*xi 是标量，不能作为矩阵 A。

[^alg-39]: **勘误或条件说明：** 勘误：这里计算 A*alpha、A*beta，是把列向量乘在 A 的右侧，应称右乘 alpha、beta。

[^alg-43]: **勘误或条件说明：** 补充：alpha=beta 不能与前面的“两个非零单位向量正交”同时成立，必须作为另一个情形。若 A=alpha*beta^T，则得到 A=alpha*alpha^T；若沿用前面 A=alpha*beta^T+beta*alpha^T 的定义，则应为 2*alpha*alpha^T。

[^alg-44]: **勘误或条件说明：** 勘误：r(A)=n-1 只保证零特征空间维数为 1，不保证零特征值的代数重数为 1，也不保证 A 可对角化。A 的所有列都在 A^* 的零空间内，可选其中 n-1 个线性无关列作基；把该空间说成所有非零特征值的特征向量张成空间还需可对角化等额外条件。

[^alg-46]: **勘误或条件说明：** 补充：此处及后一个例子用 vmatrix 的竖线记号写 A，但后续又把 A 当作矩阵进行 A+E、求特征值等运算。应将竖线内数组区分为系数矩阵 A，将 |A| 用于其行列式；首例的行列式结果 -29 保持成立。

[^alg-50]: **勘误或条件说明：** 勘误：由 A=(a-1)E+B，A 的特征值应为 a-1+lambda_i，而不是 a+1+lambda_i。具体为 a+n-1 一个，以及 a-1 共 n-1 个，特征向量沿用 B 的。

[^alg-53]: **勘误或条件说明：** 补充：xi1、…、xin 是同一向量空间的一组基向量，不是“基空间”。a1、…、an 是向量相对于这组有序基的坐标；后文两组基可以属于同一个空间。

[^alg-57]: **勘误或条件说明：** 补充：这里应建立求和关系 sum_i xi*alpha_i=sum_i xi*beta_i，即 sum_i xi*(alpha_i-beta_i)=0。除非明确约定重复指标求和，不能把它读作每个 i 都分别满足 xi*(alpha_i-beta_i)=0。

[^alg-58]: **勘误或条件说明：** 勘误：先固定列基约定：旧、新基矩阵为 U、V，V=U*C 时 C=U^(-1)*V。若 U=P*A、V=P*B，则 C=A^(-1)*B；若确为 U=A*P、V=B*P，则 C=P^(-1)*A^(-1)*B*P。原文混用了乘法顺序与转置约定，不能无条件地在结果上再转置。

[^alg-65]: **勘误或条件说明：** 补充：若新基 V=U*P，则同一向量的列坐标满足 x=P*y，行坐标满足 x^T=y^T*P^T；反向坐标变换使用 P^(-1)。P 与 P^T 的区别来自列、行表示约定，不宜把其中一个无条件称为“真正的过渡矩阵”。

[^alg-71]: **勘误或条件说明：** 勘误：通常“矩阵等价”只要求秩相同，不推出行向量组等价。例如 [1,0] 与 [0,1] 秩相同却有不同零空间。正确结论是：在未知数个数相同的条件下，行向量组等价当且仅当对应齐次方程组同解。

[^alg-72]: **勘误或条件说明：** 补充：原证明还需补上反向包含。Q 列满秩意味着 Q*y=0 只有零解，因此 Q*P*xi=0 必推出 P*xi=0；与显然的正向包含合并，才证明两个方程组同解。

[^alg-80]: **勘误或条件说明：** 勘误：非正交的可逆变换也可用初等合同变换、正交对角化后再缩放等方法构造，不限于配方法。非正交也不必导致结果不相似，例如对 A=diag(1,0) 用 diag(1,2) 作合同变换，结果仍是 A。

[^alg-81]: **勘误或条件说明：** 勘误：二次型方程 x^T*f(A)*x=0 的解不能一般按零特征值个数计算；不定矩阵即使没有零特征值也可有非零解，例如 diag(1,-1) 对向量 (1,1)。若 f(A) 半正定或半负定，零集合才等于其零空间，零特征值重数给出的是空间维数，解还包括相应特征向量的线性组合。

[^alg-83]: **勘误或条件说明：** 勘误：与 x=Q*y 配合的分解应为 A=Q*Lambda*Q^T。瑞利商的极值是单个特征值中的最小值、最大值，不是特征值之和的最值。可令对应极值特征值的一个 yi=1、其余为 0，再还原 x；更一般地，任意非零极值特征空间向量均可取到极值。

[^alg-88]: **勘误或条件说明：** 补充：这里使用的合法步骤是对称化：对实向量 x，x^T*M*x=x^T*((M+M^T)/2)*x。因此二次型的对称系数矩阵就是文中给出的秩一矩阵 A，可对它应用实对称矩阵的正交对角化。

[^alg-90]: **勘误或条件说明：** 补充：把分母化为 y^T*y 的可逆变换要求 B 正定。应先取 S 使 S^T*B*S=E、令 x=S*y，这个 S 一般不是正交矩阵；再对 S^T*A*S 作正交对角化，令 y=U*z，最终 x=S*U*z。

[^alg-96]: **勘误或条件说明：** 勘误：该比值问题最后总结把 f、g 写反了。应把分母 g 化为单位平方和的规范型，并把分子 f 化为对角标准型。

[^alg-98]: **勘误或条件说明：** 补充：平方和中各线性式的系数矩阵若为 m×n 矩阵 A，则二次型矩阵是 A^T*A。正定等价于 A 列满秩；只有当 m=n 时才能进一步说 A 可逆。

[^alg-99]: **勘误或条件说明：** 补充：由该不等式推出 A+E、E-A 正定，要求不等式对所有非零列向量 X 都成立；对单个指定 X 成立不足以推出矩阵正定。

[^alg-101]: **勘误或条件说明：** 勘误：非严格不等式对应 B1、B2 半正定，不是正定。设与 lambda_i 同一特征向量对应的伴随矩阵特征值为 mu_i（本三阶情形为另外两个 lambda 的乘积），则 a_min=max_i |mu_i-lambda_i|。lambda=2、3、4 时差为 10、5、2，所以 a_min=10。原文将 B1、B2 的特征值再减 lambda 的记号也应相应区分。

[^alg-104]: **勘误或条件说明：** 勘误：原推导对 Lambda 重复赋予了不同含义。若 A+2E=Q*diag(4,1,1)*Q^T，则正定平方根是 B=Q*diag(2,1,1)*Q^T。最后 B=E+u1*u1^T 的结论正确，其中 u1 是 A 的特征值 2 对应的单位特征向量。

[^alg-108]: **勘误或条件说明：** 补充：按本段定义，A 是 m×n 的线性式系数矩阵，B 是 n×n 的二次型系数矩阵，二者满足 B=A^T*A。由此 ker(B)=ker(A)、r(B)=r(A)，才把下一行的 r(B)=n 与 m&gt;=n 联系起来。

[^alg-110]: **勘误或条件说明：** 勘误：冗余行通常是其他若干行的线性组合，不必与某一行成比例。例如 (1,0)、(0,1)、(1,1) 中第三行是前两行之和，但与任何一行都不成比例；化为标准形需按二次型的合同变换处理。
