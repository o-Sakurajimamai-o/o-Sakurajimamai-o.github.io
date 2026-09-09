---
title: "Graduate Entrance Exam Math Notes: Calculus and Linear Algebra"
date: 2026-09-09
lastmod: 2026-09-09
description: "Complete revision notes for calculus and linear algebra."
translationKey: kaoyan-math
draft: false
---

This edition preserves every note, example, derivation, revision reminder, and figure from the latest backup, in its original order. Headings and formatting have been organized for reading. Corrections and assumptions are provided in footnotes beside the original statements.

<!-- source-content:calculus:start -->
<!-- source: calculus:L1-L4 -->

>   When working on a new problem, recognize its similarity to a previous problem. While solving it, discover gaps in your understanding of this type of problem, its tools, and its methods, or find that your understanding is not deep enough. By solving the new problem, update and refine your previous understanding at the same time.

## Limits, Derivatives, and Differentials {#calculus-h-01}

---

<!-- source: calculus:L5-L15 -->

### Algebraic Transformations, Recurrences, and Product Derivatives {#algebra-and-product-derivatives}

* For the hyperbolic sine function $y=\dfrac{e^x-e^{-x}}{2}$, solve for the inverse as follows:
  First rewrite it as $2y+e^{-x}=e^x$, multiply both sides by $e^y$, and solve the system of two linear equations in two unknowns involving $e^y$; $e^y$ is nonnegative.[^cal1-6]

---

* When finding the $n$th term of a sequence, use $x_n=f(x_{n-1})$ together with $x_n=x_{n-1}=a$ flexibly, and then turn them into a system of equations to solve. $(660_{18})$[^cal1-8]

---

* If $f(x)=u(x)v(x)$, the Taylor expansion of $f(x)$ is the product of the Taylor expansions of these two functions. This also shows that infinitesimals can be multiplied.

---

* The derivative of $f(x)=u(x)v(x)$
  $f'(x)=u(x)v'(x)+v(x)u'(x)$; the proof is on page $p_{167}$. If a product of many factors or an $n$th derivative cannot be found directly, try simplifying it into the form above, then **differentiate directly or use Leibniz's formula**:

  $$
  \frac{d^n}{dx^n} [u(x) v(x)] = \sum_{k=0}^{n} \binom{n}{k} u^{(k)}(x) v^{(n-k)}(x)
  $$

  Alternatively, use Taylor expansions, as with the multiplication of polynomials mentioned in the first point.

---

<!-- source: calculus:L16-L29 -->

### Infinity, Equivalent Infinitesimals, and Power Limits {#infinity-and-equivalents}

* **Tending to infinity means being infinite everywhere beyond some value.** If the value is sometimes infinite and sometimes constant, as in the oscillation of $\dfrac{1}{x^2}sin\dfrac{1}{x}$, it can only be called unbounded, not tending to infinity.[^cal1-16]
  * If ${\lim_{n\to \infty}x_ny_n=\infty}$, consider the following deductions:
    * At least one of ${\lim_{n\to\infty}x_n=\infty,\,\lim_{n\to\infty}y_n=\infty}$ holds. **This statement is false; replacing these with unbounded variables makes it true.** The reason is that we can construct a sequence with odd-indexed terms equal to $1$ and even-indexed terms equal to $n$. It does not tend to infinity, because there is no point after which it is infinite everywhere.
    * If ${\lim_{n\to\infty}x_n=a(a\neq0)}$, then ${\lim_{n\to\infty}y_n=\infty}$ follows: $x_n$ is constant in this case, so $y_n$ must also be constantly infinite, satisfying the definition of tending to infinity.[^cal1-19]
* For functions tending to infinity, even if both functions tend to infinity, their product need not tend to infinity, because **oscillation can occur**; the limit then actually does not exist.[^cal1-20]
* For sequences tending to infinity, if both sequences tend to infinity, their product must tend to infinity, because **sequences do not have the path dependence and everywhere-squeezing behavior of functions; their discreteness therefore ensures that they tend to infinity**.[^cal1-21]

---

* When differentiating a product of many factors, learn to **simplify**.
  * $f(x)=\prod_{n=1}^{100}\,(\tan(\frac{\pi\,x^{n}}{4})-n)$. Find $f'(1)$.
    Observation shows that when $n=1$, the value is $0$. Differentiating a product of many factors is complicated, and we do not want to differentiate repeatedly, so separate the factor for $n = 1$, giving

    $$
    (\tan(\frac{\pi\,x}{4})-1)\times\prod_{n=2}^{100}\,(\tan(\frac{\pi\,x^{n}}{4})-n)
    $$

    This now has the form $f(x)=u(x)v(x)$, making differentiation much easier.
  * $y=\frac{1-x}{1+x}$. Find $y^{(n)}(0)$.
    We know that the derivative of $(\frac{1}{ax+b})^{n}$ is easy to find, so transform the function into $y=-1+\frac{2}{1+x}$.

---

<!-- source: calculus:L30-L43 -->

### Information at a Point and Properties of a Neighborhood {#pointwise-and-neighborhood}

* Information at a single point does not imply properties on an interval.
  * If ${f'(x_0)}$ exists, can we conclude that ${f(x)}$ is continuous in a neighborhood of $x_0$?
    * No. Consider the Dirichlet function $f(x)=\begin{cases} 1, x\text{ is irrational} \\ 0, x\text{ is rational}\end{cases}$: it is differentiable at $x_0$ with $f'(x_0)=0$, but is clearly not continuous.[^cal1-32]
  * If ${f'(0)\gt 0}$, does there exist ${\delta\gt 0}$ such that ${f(x)}$ is monotonically increasing on ${(-\delta,\,\delta)}$?
    * No. Consider the following function: its derivative oscillates in a neighborhood of $x=0$, although $f'(0)=\dfrac{1}{2}$.

      $$
      f(x)=\begin{cases}{x^2sin\frac{1}{x} +\frac{1}{2}x,\,x\neq0}\\ {0,\,x=0}\end{cases}
      $$

    * However, we can conclude that there exists ${\delta}$ such that all values in the left neighborhood are smaller than the value at the point, and all values in the right neighborhood are larger. This **does not imply strict monotonicity on the interval**.
  * Distinguish continuity in a neighborhood from continuity overall. If ${f(x)}$ is continuous and ${g(f(x_0))}$ is discontinuous, **${g(f(x))}$ need not be discontinuous at ${x_0}$**.[^cal1-36]
    * This is because ${f(x_0)}$ is continuous, while ${g(x)}$ is discontinuous at ${x=f(x_0)}$. However, this is concluded from the surrounding neighborhood: at ${x_0}$, discontinuity may result from a jump, a removable discontinuity, or infinite oscillation. If **${f(x)}$ is identically equal to ${C}$**, the composition is still continuous, because the neighborhood discontinuity disappears.

---

* Equivalent infinitesimals
  * For expressions of the form ${\int_{0}^{\phi(x)}f(t)dt}$, if ${\phi(x)}$ ~ ${x^n}$ and ${f(t)}$ ~ ${t^m}$, the order is ${n\times(m+1)}$.
  * When infinitesimals are added, the final order is that of the lower-order infinitesimal. Similarly, if the above form becomes ${}$: ${\int_{g(x)}^{h(x)}f(t)dt}$, take the lower of the orders ${n\times (m + 1),\,k\times (m+1)}$.[^cal1-41]
  * For any Taylor expansion of the form ${[1+Ax^p + o(x^p)]^\alpha}$, the order stays unchanged and only the coefficient becomes ${A\alpha}$. For example, ${1-cos^\alpha x}$ ~ ${\dfrac{\alpha}{2}x^2}$.

---

<!-- source: calculus:L44-L64 -->

* The definition of a derivative is essentially the same as **finding a limit**; the two can be used in reverse.
  * If all that is given is $\lim_{x\to x_0}=A$, this only says that the limits on both sides of $x_0$ are equal; it does not imply continuity. The function must be defined at $x_0$ and satisfy $f(x_0) = A$ to be continuous there. When the value at the point is not given and the function is **abstract** (consider a piecewise function such as $\begin{cases}f(x)=x^2,\,x\neq0\\ C(C\neq 0),\,(x=0)\end{cases}$), the point is discontinuous, and therefore the function is not differentiable there.[^cal1-45]
    * ${\lim_{x\to x_0}\dfrac{f'(x)}{(x-x_0)^2}=A,\,A\neq 0}$. Does this imply monotonicity in a neighborhood of $x_0$?
      * From the equation, we can only conclude that $f'(x)$ has a limit in a neighborhood of $x_0$ excluding $x_0$ itself; that is, $f(x)$ is monotone in this region. This does not imply monotonicity at $x_0$, because **we cannot deduce $f'(x_0)=A$; the function may not even be differentiable at this point. Thus monotonicity in a neighborhood of $x_0$ does not follow, although monotonicity in a punctured neighborhood does**.[^cal1-47]
  * Conversely, if $f(x)$ is differentiable on $[a,\, b]$, this establishes the existence of $\lim_{x\to a^{+}}f(x)$; the analogous statement applies from the left at the right endpoint. If there is an implicit condition such as $\lim_{x\to a^{+}}\,\frac{f(x)}{x}=A$, then, because $f(x)$ is right-continuous at $a$, we naturally have $f(a)=a\times A$.
    * Furthermore, **if ${f(x)}$ has both a left derivative and a right derivative, it is necessarily both left-continuous and right-continuous, so ${f(x)}$ is continuous at the point**.
  * Having a value for ${f'(x_0)}$ does not mean that the derivative is continuous there. Do not confuse this with the left and right derivatives in the derivative definition: equality of the left and right derivatives means that the limit of the derivative exists at the point, not that it is continuous; the derivative at the point may not even exist. A simple example is $\begin{cases}f(x)=x^2,\,x\neq0\\ C(C\neq 0),\,(x=0)\end{cases}$.[^cal1-50]
  * In more depth, given $g(x)$, if the problem only says that $f(x)$ is differentiable and supplies an equivalent limit for the function, for example $\lim_{x\to x_0}\dfrac{f(x)}{g(x)}=A,\,g(x)=x^2$, distinguish the following cases:
    * If $f(x)$ is a concrete function, with a specific expression for $f(x)$, use the existence of the limit to determine some constants and then investigate subsequent properties.
    * If $f(x)$ is an abstract function with no explicit expression, a careful discussion is required.
      * The existence of the limit implies $f(x)=Ax^2+o(x^2)$, so $f(x)$ is a higher-order infinitesimal than, or an equivalent infinitesimal to, $g(x)$. Substitution into the derivative definition gives ${\lim_{x\to x_0}\dfrac{f(x)}{g(x)}\times \dfrac{g(x)}{x}}$, yielding **information about the first derivative**. In this example, suppose ${A\neq0 \Rightarrow f'(0)=0}$. However, the problem **does not specify whether a second derivative exists**, so properties of higher derivatives cannot be deduced.[^cal1-54]
      * Similarly, because we do not know whether the second derivative exists, we cannot directly use l'Hôpital's rule: it requires **continuous derivatives (second-order differentiability; if the function is ${n}$ times differentiable, it can be applied through order ${n-1}$, whereas if the function is ${n}$ times continuously differentiable, it can be applied through order $n$)** to guarantee replacement of the limit. Thus the existence of ${\lim_{x\to x}\dfrac{f'(x)}{g'(x)}}$ cannot be deduced.[^cal1-55]
      * Next consider preservation of sign under a limit and the sign of $g(x)$ at $x_0$. If $f'(x_0)=0$ is known, use the definition of an extremum to check whether it is an extremum point (extrema do not require a derivative or continuity). In this example, $g(x)=x^2$, so $f(0)$ is greater than $0$ throughout the neighborhood, satisfying the definition of a local minimum.[^cal1-56]
      * To collect these conclusions, for the example $g(x)=x^2$:
        * **Conclusions that necessarily follow:**
          - $f(0)=0,\,f'(0)=0$
          - If $A\gt 0$, $f(0)$ is a local minimum.
        * **Conclusions requiring extra assumptions, such as twice differentiability:**
          * $f''(0)=2A$
          * By l'Hôpital's rule or Taylor expansion: $\lim_{x \to 0} \dfrac{f'(x)}{x} = 2A$.
      * To demonstrate the conclusions above that do not follow, construct the function

        $$
        f(x)=\begin{cases}{x^3sin\frac{1}{x} +x^2,\,x\neq0}\\ {0,\,x=0}\end{cases}
        $$

<!-- source: calculus:L65-L81 -->

  * For differentiability questions involving absolute values, understand that when taking an absolute value makes a function nondifferentiable, the absolute value has made its values relatively "not as close." If a function is continuously differentiable and nonzero throughout a neighborhood, its values all have the same sign; after taking the absolute value, the sign is also consistent, so it is of course differentiable.
    * For a **neighborhood where the function is differentiable and its value is zero**, taking the absolute value changes the sign on one side (the absolute value must enclose the entire function here; otherwise, still use the definition), increases the distance, and makes it nondifferentiable.[^cal1-66]
    * **We always have: continuity of $f(x_0)$ $\rightarrow$ continuity of $|f(x_0)|$.** The converse need not hold.
    * Based on this reasoning, if ${\phi(x)}$ is differentiable, then ${f(x)=\phi(x)|g(x)|}$ has the following property at every point where $g(x_0)=0$:
      * **If ${\phi(x_0)=0}$ and ${\phi(x_0)}$ is differentiable, then $f(x_0)$ is differentiable; this condition is both necessary and sufficient.**[^cal1-69]
        * For a theoretical proof, expand at $x_0$ using Taylor's formula. If $\phi(x_0)\neq 0$, the expression there is $A|x-x_0|$, giving the derivative quotient ${\dfrac{A|x-x_0|+o(x)}{x}}$, which clearly is not differentiable. If ${\phi(x_0)=0}$, the derivative is ${\dfrac{0+o(x)}{x}=0}$, so it is clearly differentiable.
        * Geometrically, because ${|g(x_0)|}$ has a **corner** at $x_0$, if ${\phi(x_0)}$ is nonzero, multiplying by a constant naturally leaves a corner. Multiplying by zero, however, turns it into a straight line.
  * If $f(x)$ is continuous at $x_0$ and $\lim_{x\to0}\dfrac{f(x+x_0)-f(x_0)}{x^n(n\geq2)}=A$ or $\lim_{x\to x_0}\dfrac{f(x)-f(x_0)}{(x-x_0)^n(n\geq2)}=A$ exists, then $f(x_0)$ is differentiable and $f'(x_0)=0$. Furthermore, applying l'Hôpital's rule to the expression above may reveal properties of $f''(x_0),\,\,f'''(x_0)$.[^cal1-72]
  * Suppose the derivative exists at $x=x_0$, with $f'(x_0)=A$, and we want to find $\lim_{x\to x_0^{+(-)}}\dfrac{f(x)-f(x_0)}{g(x)}$, where $g(x_0)=0\text{ or }\infty$. Because the derivative need not be continuous at $x_0$, neither l'Hôpital's rule nor Cauchy's mean value theorem can be used. Consider constructing $\lim_{x\to x_0^{+(-)}}\dfrac{\frac{f(x)-f(x_0)}{x-x_0}}{\frac{g(x)-g(x_0)}{x-x_0}}$, with $g(x)$ continuous at $x_0$.[^cal1-73]
  * Under the condition $f(0)=0$, a limit expression must satisfy all of the following to imply differentiability; otherwise, differentiability cannot be deduced ($660-164,\,880_{base}-2.1.11$).
    * **The orders must be the same, or the numerator's order must be $\leq$ the denominator's order and the limit must exist**, as in $\lim_{x\to 0}\dfrac{f(x)}{x},\,\dfrac{f(x)}{x^2},\,\dfrac{f[ln(1-x)]}{x}$.[^cal1-75]
    * **Both sides must be defined**: ensure that $x\to x_0$ includes $x\to x^{+},\,x\to x_0^{-}$. For example, with $\lim_{x\to 0}\dfrac{f(\sqrt{x^2 + 1}-1)}{x^2}$, because ${(\sqrt{x^2 + 1}-1)\gt 0}$, the existence of the limit for $x\to x_0^{-}$ cannot be guaranteed.
    * Do not bypass the fixed point $x_0$. For example, $\lim_{x\to 0}\dfrac{f(x)-f(-x)}{x},\,\dfrac{f(x)-f(x^2)}{x}$ bypasses $0$, so differentiability cannot be deduced. A counterexample is $f(x)=\begin{cases} 1, x\neq 0 \\ 0, x=0\end{cases}$.
    * Approach along real-valued paths must be covered; this is similar to the second requirement. For example, in $\lim_{n \to \infty}nf(\frac{1}{n})$, $n$ cannot be negative, so the expression only describes existence when approaching $0^{+}$. Even $\lim_{m\to\infty}mf(\frac{1}{m}),\, m\text{ is a nonzero integer}$ is insufficient, because irrational numbers are not included. The Dirichlet function provides a counterexample: $f(x)=\begin{cases} 1, x\text{ is irrational} \\ 0, x\text{ is rational}\end{cases}$.
  * Look for implicit derivative values in the problem's information. Try bounds and observations at special points, for example:
    * ${|f(x)|\leq x^2 \Rightarrow f(0)=0\Rightarrow f'(0)=\lim_{x\to0}\dfrac{f(x)}{x}\leq \lim_{x\to0}\dfrac{x^2}{x}=0}$[^cal1-80]

---

<!-- source: calculus:L82-L94 -->

### Higher Derivatives and Geometric Information {#higher-derivatives-and-geometry}

* Higher derivatives
  * When higher derivatives appear, possibly together with an abstract function, "higher" means order at least $2$, especially when the order ranges from $1\to3$. Since only an abstract function $f(x)$ is given, there is no direct starting point; **consider stationary points, inflection points, concavity/convexity, Taylor expansions, and similar information** to find clues.
    * Given that $f(x)$ is twice differentiable with $f''(x)\gt (\lt )0$, use its Taylor expansion at $x=0$ to obtain the bound $f(x)=f(0)+f'(0)x+\dfrac{f''(0)}{2!}x^2+o(x^2)$. Combine this with the problem's constraint $\int_{-a}^{a}f(x)dx=b$: because the $x$ term is odd and the $x^2$ term is always greater than $0$, integration gives $b\geq 2a\times f(0)$.[^cal1-84]
      * You can also **use concavity/convexity: the line joining two points must lie above (below) the tangent**. For a concave function it is greater than or equal to the tangent, while for a convex function it is less than or equal to the tangent. We then have:

        <figure class="fig"><img src="/blog/kaoyan-math/figures/convexity-and-tangent.png" alt="Original note figure 1" width="1321" height="587" loading="lazy" decoding="async"><figcaption>Original note figure 1</figcaption></figure>

        [^cal1-85]
  * When a higher derivative of moderate order is required but direct differentiation is difficult, the following methods are commonly available:
    * If finding $f^{(n)}(0)$, examine the function's parity and check whether it is odd.
    * Differentiate and look for a pattern or regularity.
    * Use Taylor expansion directly, or differentiate once or twice and use it if a suitable form appears. For example:

      $$
      f(x)=ln(\sqrt{1+x^2}-x), \text{find } f^{(5)}(0)
      $$

      Direct differentiation or Taylor expansion is difficult, so differentiate once: $\to f'(x)=-\dfrac{1}{\sqrt{1+x^2}}$. Now the Taylor expansion of $(1+x)^{α}$ can be used, expanding directly through $-\dfrac{3}{8}x^4+o(x^4)$. Differentiation is then simple, giving $f^{(5)}=f'^{(4)}(0)=-9$.
    * Some common techniques for finding higher derivatives are listed below. They are usually used for series or integrals, so it is easy to overlook their use in differentiation; pay attention to this.
      * For ${F(x,\,y)=\dfrac{f(x)(\text{ or }f(y))}{ax^2-by^2}}$, use the difference of squares to arrange terms. For example:

        $$
        {F(x,\,y)=\dfrac{2x}{x^2-y^2}=\dfrac{1}{x+y}+\dfrac{1}{x-y}}
        $$

        Use ${(\dfrac{1}{x})^n=(-1)^n\dfrac{n!}{x^{n+1}}}$. To find ${\dfrac{\partial F}{\partial x}}$, substitute for $x$ as a whole; do the same for ${\dfrac{\partial F}{\partial y}}$.[^cal1-93]

---

<!-- source: calculus:L95-L106 -->

* Look flexibly for the form $1^{\infty}$ and check whether $\lim a(x)b(x)=A$, $lima(x)=0$, and $limb(x)=\infty$ hold. Then directly obtain

  $$
  lim(1+a(x))^{b(x)}=e^{A}
  $$

  . Note: after simplification, the $1$ here is **the key**, and many transformations are possible.
* If the base is not $e$ but $a$, multiply by $lna$. For example:

  $$
  {\lim_{x\to+\infty}x^P(a^{\frac{1}{x}}-a^{\frac{1}{x+1}})}
  $$

  Factor out the common factor to obtain directly:

  $$
  {{\lim_{x\to+\infty}x^P\times a^{\frac{1}{x+1}}(a^{\frac{1}{x(x+1)}}-1)}\Rightarrow x^P\times a^{\frac{1}{x+1}}\times \dfrac{lna}{x(x+1)}}
  $$

---

* For limits of the form $\infty-\infty$, there are generally two methods: first, combine terms over a common denominator; second, when that is difficult, multiply numerator and denominator by the same expression to eliminate radicals and obtain the form $\dfrac{\infty}{\infty}$. There is also a less obvious method: factor or simplify, then cancel terms using a Taylor expansion. For example:
  * ${\lim_{x\to \infty}(1-x^6)^{\frac{1}{3}}+x^2}$: factor out $x^2 \Rightarrow \lim_{x\to\infty}-x^2(1-\dfrac{1}{x^3})^{\frac{1}{3}}+x^2$, then cancel $x^2$ using Taylor expansion to obtain the limit $0$.[^cal1-101]

---

* For an expression under a radical (or inside an absolute value), when taking the root (or **introducing a square root of a square to remove an absolute value**), always watch **the sign of the value**. In some problems involving limits with radicals, discontinuities, asymptotes, and similar topics, the answer is the maximum of an expression. In other words, always **watch the sizes of particular expressions under the given conditions**, since they can lead to different answers.
  * $lim_{n\to\infty}\dfrac{1-x^{2n}}{1+x^{2n}}$: note the different values of $x$ in the cases $\lt 1,\,1,\,\gt 1$.[^cal1-104]
  * $lim_{n\to\infty}x^{2n+1},\,\,x^{2n},\,\,x^n$ gives different results in the cases $(1,\, -1,\, \gt 1,\, \lt 1)$. The limits should differ, so write a piecewise function. **Be sure to pay attention to this!** See problems such as $660_{19}$.

---

<!-- source: calculus:L107-L117 -->

### Parametric Curves, Extrema, and the Definition of a Differential {#parametric-curves-and-extrema}

* When differentiating parametric equations, always remember

  $$
  y=f(x),\,\,\, \begin{cases} x=\phi(t) \\y=\psi(t)  \end{cases}
  $$

  * Note: **only when both are functions of $t$** does $\frac{dx}{dy}$ equal $\dfrac{\frac{dy}{dt}}{\frac{dx}{dt}}$. If an equation such as $te^{y}+y+1=0$ appears, do not differentiate directly this way. First find $\frac{dy}{dt}$ (differentiate once with respect to $t$; $y$ is also a function of $t$), and then proceed.[^cal1-108]
  * Discussing the continuity and differentiability of $f(x)$:
    * For differentiability, transform it into $y=f(x)$ and solve, or examine $\frac{dy}{dx}$ to determine differentiability.
    * For continuity, similarly transform it into $f(x)$ and discuss continuity. Differentiability guarantees continuity. Another method is to **check whether $\phi(t),\,\,\,\psi(t)$ are continuous**; if both functions are continuous, $f(x)$ is naturally continuous.[^cal1-111]
  * To find an oblique asymptote, identify the $x\to\infty$ limit and the corresponding $t$ values. Use $\dfrac{y(t)}{x(t)}$ to find $a$, then use $y(t)-ax(t)=b$. ($880_{base}-2.2.11$)

---

* To compare two radical expressions, raise both sides to the $x$th power.
  * Compare $2^{\frac{1}{2}},\,\,\,3^{\frac{1}{3}}$.
    Simply cube both sides, or raise both to the sixth power, to obtain the result.

---

<!-- source: calculus:L118-L133 -->

* For $f(x)$, if $f'(x) \geq 0$ and equality holds only at finitely many points, the function is strictly increasing. In this case, $f'(x) = 0$ does **not mean that the function is not increasing; it means that it increases more slowly than $x \to x_0$**. This leads to another idea: if $f'(x_0)=0$ and the function attains a maximum at $x_0$, we must have $f''(x_0)\leq 0$. At this stage we can only make a broad statement: $f'(x_0)$ must not be increasing. Following the preceding reasoning, we only need $f'(x_0)$ to decrease, that is, $f''(x_0) \leq 0$, with equality at finitely many points. From $f''(x_0) = 0$, **we cannot conclude that there is no change**.[^cal1-118]

---

* For local and global extrema of $f(x)$, check **whether there are nondifferentiable points and whether they are extrema** (extremum points and inflection points do not require differentiability at the point, but do require continuity).[^cal1-120]
  * At a nondifferentiable point, check whether the derivative values on the two sides satisfy $f'(x_{0}^{+})\times f'(x_{0}^{-}) \lt  0$; this is the condition for a stationary point. Similarly, examine the second derivative for the condition for an inflection point. If a graph of the second derivative is given, look for $x_0$ with opposite signs on its two sides.[^cal1-121]
  * At differentiable points, use $f'(x) = 0$ to find possible stationary points, then check whether $f''(x)$ is nonzero; if so, it is a stationary point. Use $f''(x) = 0$ to find possible inflection points, then verify them using $f'''(x) \neq 0$.[^cal1-122]
  * If a function is differentiable in the interior of an interval and has only one local extremum, that extremum is the corresponding global extremum. Otherwise, the function's global maximum is at an interval endpoint or is the largest of its local maxima.
  * After finding values at stationary points, extremum points, and nondifferentiable points, also find the values at interval endpoints (including ${lim_{\infty}f(x)}$), then compare them to obtain the global extrema.
  * If the function is known to have only one local extremum, suppose it is a local maximum; then the minimum must occur at an endpoint, and conversely. This can be used in proof problems.[^cal1-125]
  * For a piecewise function, if the function is known to be differentiable on the interval, or differentiable at a joining point or a point without a defined value, pay particular attention to **joining points and undefined points** when differentiating. The derivative should also be written piecewise; use the definition at a break point.[^cal1-126]

---

* If $a$ is a root of $f(x) = 0$ of multiplicity $m(m\geq1)$, then $a$ is a root of $f'(x) = 0$ of multiplicity $m-1$.[^cal1-128]

---

* If a condition resembles the definition of a differential, namely $f(x+C)-f(x)=AC+o(C)$, then $C$ is arbitrary and $C$ can serve as $\Delta x$, giving $A=f'(x)$.[^cal1-130]
  * For any $f(x)$ satisfying, for arbitrary $x,\, y$, $f(x+y)-f(x)=(f(x)-1)y+o(y)$, find $f(x)$.
    * Use the definition of a differential directly: $d(f(x))=(f(x)-1)dx+o(\Delta x)$. Integrate both sides to solve.[^cal1-132]

---

<!-- source: calculus:L134-L135 -->

## Mean Value Theorems {#calculus-h-02}

---

<!-- source: calculus:L136-L151 -->

### Constructing Auxiliary Functions from Derivative Rules {#auxiliary-functions}

* **Construct auxiliary functions for proofs. Common auxiliary functions are listed below.**
  Formula: $(uv)' = u'v + uv'$. Use it in reverse as follows:
  * $[f(x)f'(x)]' = [f^2(x)]' = 2f(x) \cdot f'(x)$. When you see $f(x)f'(x)$, set $F(x) = f^2(x)$.[^cal1-138]
  - $[f(x) \cdot f'(x)]' = [f'(x)]^2 + f(x)f''(x)$. When you see $[f'(x)]^2 + f(x)f''(x)$, set $F(x) = f(x)f'(x)$.
  - $[f(x)e^{\varphi(x)}]' = [f'(x) + f(x)\varphi'(x)]e^{\varphi(x)}$. When you see $f'(x) + f(x)\varphi'(x)$, set $F(x) = f(x)e^{\varphi(x)}$.
    - $\varphi(x) = x \Rightarrow$ When you see $f'(x) + f(x)$, set $F(x) = f(x)e^x$.
    - $\varphi(x) = -x \Rightarrow$ When you see $f'(x) - f(x)$, set $F(x) = f(x)e^{-x}$.
    - $\varphi(x) = kx \Rightarrow$ When you see $f'(x) + kf(x)$, set $F(x) = f(x)e^{kx}$.
  Note: $(uv)'' = u''v + 2u'v' + uv''$ may also be tested.

---

* Formula: $\left(\frac{u}{v}\right)' = \frac{u'v - uv'}{v^2}$. Use it in reverse as follows:
  - $\left[\frac{f(x)}{x}\right]' = \frac{f'(x)x - f(x)}{x^2}$
    When you see $f'(x)x - f(x)$ ($x \neq 0$), set $F(x) = \frac{f(x)}{x}$.
  - $\left[\frac{f'(x)}{f(x)}\right]' = \frac{f''(x)f(x) - [f'(x)]^2}{f^2(x)}$. When you see $f''(x)f(x) - [f'(x)]^2$ ($f(x) \neq 0$), set $F(x) = \frac{f'(x)}{f(x)}$.
  - $(\ln f(x))' = \frac{f'(x)}{f(x)}, \quad \left(\frac{f'(x)}{f(x)}\right)' = \frac{f''(x)f(x) - [f'(x)]^2}{f^2(x)}$. When you see $f''(x)f(x) - [f'(x)]^2$ $(f(x) \gt  0)$, set $F(x) = \ln f(x)$.

---

<!-- source: calculus:L152-L162 -->

### Starting from Endpoints, Taylor Remainders, and Hidden Points {#mean-value-proof-patterns}

* If a proposition to be proved contains $a+b$ or $a-b$, try the $Lagrange$ mean value theorem. The appearance of $a+b$ calls for constructing an abstract function $f(x)$. If $Lagrange$ can be applied and $x\neq0$, Cauchy's mean value theorem can then be used: $\dfrac{f'(\xi)}{2\xi}\times \dfrac{1}{f'(\eta)}\,=\,\dfrac{f(a)-f(b)}{a^2-b^2}\times\dfrac{a-b}{f(a)-f(b)}$

---

* For difficult proofs that Taylor expansion can simplify, such as proving $f(x) \leq \dfrac{1}{n!}g(x)$ or statements involving $f^{n(n\geq2)}$, consider the $Lagrange$ remainder formula.
  * Prove $|\dfrac{sinx}{x}-1|\,\leq\,\dfrac{1}{2}|x|$.
    Expand $sinx$ at $x_0=0$ using the $lagrange$ remainder, giving

    $$
    sin0+(sinx)'|_{x=0}*x+\dfrac{(sinx)''|_{x=\xi}}{2!}*x^2=x-\dfrac{x^2sin\xi}{2},\,\,\xi\in(0,x)
    $$

    . We then obtain $|-\dfrac{xsin\xi}{2}|$, and the proof follows naturally.[^cal1-156]

---

* In proof problems involving mean value theorems, sometimes find hidden points: their existence alone can be enough to prove the result.
  * A nonconstant function $f(x)$ is differentiable on $[a,\,b]$, with $f(a)=f(b)=0$. Prove $f'(\xi)\gt 0(\lt 0)$.
    Take a point $c$ with $a\lt c\lt b$. Then $f'(\xi_1)=\dfrac{f(a)-f(c)}{a-c}$, $f'(\xi_2)=\dfrac{f(b)-f(c)}{b-c}$. If $f'(\xi_1)\gt 0(\lt 0)$, then $f'{\xi_2}\lt 0(\gt 0)$, completing the proof.[^cal1-160]

---

<!-- source: calculus:L163-L164 -->

## Integration {#calculus-h-03}

---

<!-- source: calculus:L165-L180 -->

### Antiderivatives, Integrability, and Integral Mean Value Theorems {#antiderivatives-and-integrability}

* For an antiderivative $F(x)$ and its derivative $f(x)$ in indefinite integration, we know the following:
  1. For the variable-limit integral $F(x)=\int_{a}^{x}f(t)\,dt$, existence implies continuity.
  2. **Proof of the integral mean value theorem**
     * $m(b-a)\,dx\,\leq\,\int_{a}^{b}f(x)\,dx\,\leq\,M(b-a)$. By the intermediate value theorem, there exists $\xi\in[a, b]$ such that $\int_{a}^{b}f(x)\,dx=f(\xi)(b-a)$.[^cal1-168]
     * To prove that there exists $\eta\in(a,b)$ such that $\int_{a}^{b}f(x)\,dx=f(\eta)(b-a)$, consider Lagrange's mean value theorem. Because $f(x)$ is continuous, define $F(x)=\int_{a}^{x}f(t)\,dt$, giving $F(b)-F(a)=f(\eta)(b-a)\,\Rightarrow\,\int_{a}^{b}f(x)\,dx-0=f(\eta)(b-a),\,\,\eta\in(a,b)$.
     * **The weighted form of the integral mean value theorem:**
       * If $f(x),\,\,g(x)$ are continuous on $[a,b]$ and $g(x)$ does not change sign, then

         $$
         \int_{a}^{b}f(t)g(t)dt=f(\xi)\int_{a}^{b}g(t)dt,\,\,\xi\in[a,b]
         $$

     * During a proof, turn a constant into a variable, then use the integral mean value theorem and monotonicity to prove an inequality. For example:
       * $f(x)$ is monotonically increasing. Prove ${\int_{a}^{b}xf(x)dx \geq \dfrac{a+b}{2}\int_{a}^{b}f(x)dx}$.
         * Try turning the constant into a variable and constructing ${F(x)={\int_{a}^{x}tf(t)dt - \dfrac{a+x}{2}\int_{a}^{t}f(t)dt}}$. Differentiation gives the following expression, and monotonicity then proves the result:

           $$
           {F'(x)=\dfrac{x-a}{2}f(x)-\dfrac{1}{2}\int_{a}^{x}f(t)dt(\Rightarrow \dfrac{x-a}{2}f(\xi)),\,a\lt \xi\lt x}
           $$

           [^cal1-174]
  3. A continuous function $f(x)$ necessarily has an antiderivative $F(x)$.
     Proof: $f(x)$ is continuous on $[a, b]$. Take $x\in(a,b),\,\, x+\Delta x\in(a, b)$; then $\Delta F\,=\,F(x+\Delta x)-F(x)=\int_{a}^{x+\Delta x}f(t)\,dt-\int_{a}^{x}f(t)\,dt\,=\,\int_{x}^{x+\Delta x}f(t)\,dt\,=\,f(\xi)(\Delta x),\,\xi\in(x, x+\Delta x)$.
     Also, $F'(x)\,=\,\lim_{\Delta x \to0}\dfrac{F(x+\Delta x)-F(x)}{\Delta x}$. After applying l'Hôpital's rule, because $\Delta x \to 0 \Rightarrow \xi\to x \Rightarrow \lim_{\xi\to x}f(\xi)=f(x)$, the proof is complete.[^cal1-177]
  4. If $f(x)$ has a discontinuity of the first kind or an infinite discontinuity, it has no antiderivative $F(x)$.
  5. If $f(x)$ exists as a derivative, its antiderivative must be differentiable and continuous everywhere in its domain. When finding the antiderivative $F(x)$, pay attention to potential discontinuities (where $f(x)$ is piecewise), and use $\int f(x)+C$ to join the pieces of the antiderivative.
  6. When finding an indefinite integral, always include **$+C$**.

<!-- source: calculus:L181-L198 -->

---

* Definite integrals
  * **Integrability of $f(x)$ implies $|f(x)|\leq M$.**
  * Remember additivity of definite integrals. For example, find ${\lim_{n\to\infty}\sum_{k=1}^{n}\int_{k}^{k + 1}f(x)}$. Pay attention to the information inside the summation; writing out several terms reveals ${\int_{k}^{k + 1}+\int_{k + 1}^{k + 2}+\dots=\lim_{n\to+\infty}\int_{1}^{n + 1}f(x)=\int_{1}^{+\infty}f(x)dx}$.
  * For the geometric meaning of a definite integral, any function value within each small subinterval can be used as its sampled value. For convenience we often choose the $i$th value, $f(\frac{i}{n})$, but we can also use the midpoint value $f(\frac{2i-1}{2n})$ or another value. The general formula is

    $$
    \int_{a}^{b} f(x) \,dx \,= \, \lim_{n \to \infty} \sum_{i=1}^{n}\,f(a+\frac{b-a}{n}i)\frac{b-a}{n}
    $$

    . Note that we can divide the interval into not only $n$ equal parts but also $2n, 3n\dots$; the definition remains unchanged. In particular, $\frac{b-a}{n}i$ must correspond to the width $\frac{b-a}{n}$.

    <figure class="fig"><img src="/blog/kaoyan-math/figures/riemann-sum.png" alt="Original note figure 2" width="636" height="205" loading="lazy" decoding="async"><figcaption>Original note figure 2</figcaption></figure>

  * If $f(x)$ has only finitely many discontinuities (**excluding infinite discontinuities**), then $\int_{a}^{b}f(x)\,dx$ exists.[^cal1-186]
  * Sufficient conditions for the existence of a definite integral
    1. $f(x)$ is monotone on $I$.
    2. $f(x)$ is continuous on $I$.
       1. $f(x)$ is bounded on $I$ and has only finitely many discontinuities.
  * Necessary conditions for the existence of a definite integral
    1. $f(x)$ is bounded on $I$.
    2. The interval of integration has finite length.
  * For comparison problems, investigate the following possibilities (the list is incomplete, but it includes essentially everything that comes to mind):
    * Check whether oddness can simplify the expression, or whether splitting it simplifies some terms. For example,

      $$
      \int_{-\frac{1}{2}}^{\frac{1}{2}}\dfrac{x^2+x+1}{x^2+1}
      $$

       splits directly into a constant and an odd function, giving $1$.
    * Use bounds. For example, to compare $M=\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}}sin^2xcosx\,dx$ and $N=\int_{-\frac{\pi}{2}}^{\frac{\pi}{2}}sin^3x-cosx\,dx$, take $M$, replace its $cosx$ by $sinx$, then subtract, since throughout the domain we have $cosx\gt sinx$.[^cal1-196]
    * Discuss the function's properties. A denominator whose behavior is already determined need not be discussed, simplifying the analysis. For example, for $\displaystyle\int_{0}^{1}\dfrac{(1+x)ln^2(1+x)}{x^2}\,dx$, directly examine the numerator.

---

<!-- source: calculus:L199-L210 -->

### Variable-Limit and Improper Integrals {#variable-limit-integrals}

* Variable-limit integrals
  * The variable-limit integral $\int_{a}^{x}f(t)\,dt$ is a function of $x$.
  * If $f(x)$ is integrable, then $F(x) = \int_{a}^{x}f(t)\,dt$ is continuous. Note that $F(x)$ need not be an antiderivative, and $f(x)$ need not be continuous: a function with finitely many discontinuities can also be integrable. Thus **a variable-limit integral is necessarily continuous whenever it exists**. The proof is as follows:
    - $f(x)$ is integrable, so $|f(x)|\leq M$. We have

      $$
      F(x+\Delta x)-F(x)=\int_{x}^{x+\Delta x}f(t)\,dt\leq M\Delta x
      $$

      . Taking the limit gives $\lim_{\Delta x \to 0}F(x+\Delta x)-F(x)=\lim_{\Delta x \to 0}M\Delta x = 0$, that is, $\lim_{\Delta x \to 0} F(x+\Delta x) =  F(x)$, proving the result.[^cal1-202]
  * If $f(x)$ has a jump discontinuity, then $F(x)$ is not differentiable there, and the corresponding derivatives correspond to the limits of $f(x)$.
  * If $f(x)$ has a removable discontinuity, then $F(x)$ is differentiable there and $F'(x_0)=\lim_{x\to x}f(x)$.[^cal1-204]
  * For differentiation of a variable-limit integral of an abstract function, such as $\int_{F_2(x)}^{F_1(x)}f(g(x)-h(t))dt$, first substitute for the inner expression $g(x)-h(t)$ to make differentiation easier. See $(888_{comp}(3.3.8))$.
    * To differentiate $\int_{0}^{x^2}tf(x^2-t^2)dt$, we can find an antiderivative in the usual way and subtract its endpoint values; that works. However, a simpler method is $\to$ to set $u=x^2-t^2$, obtaining $\frac{1}{2}\int_{0}^{x^2}f(u)du$, which is much more concise.[^cal1-206]
  * If ${f(x)}$ is odd, then $\int_{a}^{x}f(t)dt$ is necessarily even. If $f(x)$ is even, but **$a\neq0$ or $F(a)\neq0$**, we cannot conclude that $\int_{a}^{x}f(t)dt$ is odd.
  * For convergence or divergence of improper integrals over infinite intervals, such as $\int_{-\infty}^{\infty}e^{|x|}sinx$, remember that the usual odd-function property does not apply here. If the function's integral diverges at one end, adding or subtracting two divergent integrals still leaves divergence.
  * For limits involving variable-limit integrals, if the integrands ${f(x)}$ and ${g(x)}$ have the same convergence or divergence behavior, they can be substituted as equivalents. Alternatively, expand ${f(x)}$ in a Taylor series at the limiting point and integrate term by term. That is, ${\lim_{x\to 0}\int_{0}^{x}f(x)dx=\lim_{x\to 0}\int_{0}^{x}g(x)dx}$.[^cal1-209]

---

<!-- source: calculus:L211-L223 -->

### Radical Signs, Integration by Parts, and Interval Symmetry {#integration-signs-and-substitution}

* Calculating integrals
  * **Always watch the sign when taking square roots.** Because of restrictions on the integrand's domain, if $x$ must be moved inside a radical or taken out of it and $x$ is negative, add a minus sign.
    * A curve passes through $(-2, 0)$ and has slope ${\dfrac{1}{x\sqrt{x^2-1}}}$ at every point. Find the curve.
      * Nonnegativity inside the radical gives two possible ranges for $x$. Use the point on the curve to determine the specific range of $x$, then find the indefinite integral. Set $x=sect$, giving

        $$
        {\dfrac{1}{x\sqrt{x^2-1}}dx \Rightarrow \int\dfrac{sect\,tant}{sec\,(-tant)}dt=-arccos(\frac{1}{x})+C}
        $$

        .
        **Pay particular attention to $-tant$ here. The range of $x$ obtained above gives $tant\lt 0$, so we need $\sqrt{tan^2t}=-tant$. This applies not only to trigonometric functions but also to $x$.**
    * Evaluate ${\displaystyle\int_{-\tfrac{\pi}{2}}^{\tfrac{\pi}{2}}\int_{0}^{Rcos\theta}\dfrac{r}{\sqrt{R^2-r^2}}\,dr}$.
      * Finding an antiderivative of the integrand is straightforward, giving ${R-Rsin\theta}$. However, **the outer ${\theta}$ range contains negative values, so this becomes ${R-R|sin\theta|}$**. Then integrate.
  * Split terms and cancel them through integration by parts. First inspect the expression: this works if differentiating one split part produces a later part. Typical forms include ${e^{f(x)}sinx/cosx/tanx\dots}$, and most involve trigonometric functions. For example, when ${(1+tanx)^2=sec^2x+tanx}$ appears, observe that the first term is the derivative of the second and try cancellation. $(\mathrm{Zhang}_{base}(9.5, \,\,T_{9.11}),\,880_{base}(3.3.4.6),\,880_{comp}(3.3.3))$[^cal1-218]
  * If an abstract integral such as $\int x^{n-1}\sqrt{1+x^n}dx$ appears, seeing $n-1$ and $n$ should suggest **matching a differential**.
  * After substitution, integrate $f(t)dg(t)$ by parts to turn it into $g(t)df(t)$. Example: ($\mathrm{Zhang}_{base}9.7$).
  * Interval reflection: for $f(x)g(sinx),\,\,f(x)g(cosx)$, and sometimes expressions containing ${f(x)e^x}$, if integration is difficult, tentatively check whether reflection of the interval can be used. Examples: ($\mathrm{Zhang}_{base}9.18,\,\,9.12$, ${\mathrm{Wu}_{comp}P_{115}-9}$).
  * For $\dfrac{1}{1+sinx},\,\,\dfrac{1}{1+cosx}$ and similar expressions, frequently use rationalization of the numerator and denominator, multiplying both by the same expression.
    * Example: $\int\dfrac{x}{1+sinx}$. Rationalize the denominator directly; when $xf(sinx/tanx/cosx)$ appears, use interval reflection.

<!-- source: calculus:L224-L241 -->

### Inverse Trigonometric Branches and Trigonometric Integrals {#inverse-trigonometric-branches}

* When substituting in an integral, check whether the inverse function's interval must be split. For expressions such as $arcsinsinx,\,\,arctantanx$, **watch the range**; check whether the function is monotone.
  * Evaluate

    $$
    {\int_{0}^{1}x\,arcsin(2\sqrt{x-x^2})\,dx}
    $$

    .
    * For an expression such as ${\sqrt{x-x^2}}$, set ${x=sin^2\theta}$, giving

      $$
      {\int_{0}^{\frac{\pi}{2}}sin^2\theta\cdot arcsin(sin2\theta)\cdot2sin\theta\cdot cos\theta\,d\theta}
      $$

      .
      Because $\arcsin(\sin 2\theta) =\begin{cases}2\theta, & 0 \leq \theta \leq \frac{\pi}{4} \\\pi - 2\theta, & \frac{\pi}{4} \leq \theta \leq \frac{\pi}{2}\end{cases}$, we have

      $$
      {I = \int_0^{\frac{\pi}{4}} 2 \sin^3 \theta \cos \theta (2\theta) \, d\theta + \int_{\frac{\pi}{4}}^{\frac{\pi}{2}} 2 \sin^3 \theta \cos \theta (\pi - 2\theta) \, d\theta}
      $$

      .
      At this point, ${\displaystyle I_1=\int_{0}^{\frac{\pi}{4}}\theta\,dsin^4\theta,\,I_2=\int_{0}^{\frac{\pi}{4}}(\dfrac{\pi}{2}-\theta)\,dsin^4\theta}$. Integrate by parts.[^cal1-228]
  * Some commonly used inverse-trigonometric formulas are listed here:
    * ${\begin{align} \arctan x+\arctan y&=\arctan\frac{x+y}{1-xy}\quad(xy\lt 1)\\ &=\pi+\arctan\frac{x+y}{1-xy}\quad(x\gt 0,xy\gt 1)\\ &=-\pi+\arctan\frac{x+y}{1-xy}\quad(x\lt 0,xy\gt 1)\\ \end{align}}$
    * ${\begin{align} \arctan x-\arctan y&=\arctan\frac{x-y}{1+xy}\quad(xy\gt -1)\\ &=\pi+\arctan\frac{x-y}{1+xy}\quad(x\gt 0,xy\lt -1)\\ &=-\pi+\arctan\frac{x-y}{1+xy}\quad(x\lt 0,xy\lt -1)\\ \end{align}}$
    * When ${xy = 1}$ and ${x \ne 0}$, ${\arctan x + \arctan \dfrac{1}{x} = \begin{cases} \dfrac{\pi}{2}, & x \gt  0 \\\\-\dfrac{\pi}{2}, & x \lt  0\end{cases}}$.
    * When ${xy = -1}$ and ${x \ne 0}$, we have ${{\arctan x - \arctan \left( -\dfrac{1}{x} \right) = \arctan x + \arctan \dfrac{1}{x} = \begin{cases} \dfrac{\pi}{2}, & x \gt  0 \\\\ -\dfrac{\pi}{2}, & x \lt  0 \end{cases}}}$.
* Use the integration properties of odd and even functions, splitting the expression if necessary.
* For $\dfrac{1}{a^2+x^2}dx$, recognize its general structure, as in $\dfrac{1}{x^2-x+1}\rightarrow \dfrac{1}{(x-\frac{1}{2})^2+(\frac{\sqrt{3}}{2})^2}$. Be sure to match the differential $d(x-\frac{1}{2})$ here; similarly, for $dAx$, supply the constant factor.
* For expressions such as $R(-sinx,\,cosx)=-R(sinx,\,cosx)\dots$, learn substitution and matching differentials. Also remember the universal substitution formulas $(P_{289})$. See ($\mathrm{Wu}_{base}P_{81}-13,14$).
  * When you see $R(-sinx,\,-cosx)=-R(sinx,\,cosx)$, simply set $u=tanx$ ($1k_{base}14.20$).[^cal1-237]
* For ${sinax \times cosbx}$, consider product-to-sum identities; sum-to-product identities can likewise be used in reverse.
  * ${\int sinx \times cosnx \,dx}={\int sin(1+n)x+sin(1-n)xdx}$[^cal1-239]
* For $e^{ax}cos/sinbx$, apply the formula directly $(\mathrm{Zhang}_{base}{285})$. See ($\mathrm{Zhang}_{base}(10.4)$).
* If ${\int\dfrac{asinx + bcosx}{csinx + dcosx}}$ appears, rearrange terms to match a differential. Solve ${\begin{cases}Ac-Bd=a \\ Ad+Bc =b \end{cases}}$ for ${A,\,B}$ and rewrite the expression as ${\dfrac{A(c\,sinx+d\,cosx)+B(c\,cosx-d\,sinx)}{c\,sinx+d\,cosx}}$. Then match ${d(ln(csinx+dcosx))}$. Examples: Li/Fan (3.40), $880_{base}(3.3.3,\,3.3.4)$.[^cal1-241]

<!-- source: calculus:L242-L252 -->

### Reduction of Order, Area, and Special Functions {#area-beta-gamma}

* For some variable-limit integrals such as $f(x)\int_{a}^{x}g(t)dt$, or higher derivatives $f^{n}$, try integration by parts to reduce the order and find a solution ($\mathrm{Zhang}_{base}11.6,\,1k_{base}{10.7}$).
* Become familiar enough with certain trigonometric expressions to recognize them instinctively, for example ${(1+tanx)^2=sec^2x+tanx}$.[^cal1-243]
* If the problem asks for the area associated with a function, remember that area is nonnegative: **watch the function's sign on the specified interval and, when necessary, calculate by taking a limit in the definition of the definite integral**.
  * A good example is $y=e^{-x}sinx$. Calculate the area on one interval and then use the definition of the definite integral, namely

    $$
    \lim_{n\to \infty}\sum_{k=0}^{n}\int_{k\pi}^{(k+1)\pi}e^{-x}|sinx|dx
    $$

    . **Do not take this for granted: always watch the function's sign, and keep paying attention to its sign.**
* Pay attention to the integral ${\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy}$. Set $F(x)=\int_{x}^{b}f(x)dx$, then match differentials:

  $$
  {\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy=\int_{a}^{b}[\int_{x}^{b}f(y)dy]\,d\,[-\int_{x}^{b}f(y)dy]}
  $$

  .
* The Beta and Gamma functions:
  * For expressions of the form ${\displaystyle \int_{0}^{+\infty}x^{\alpha-1}e^{-x}dx,\,2\int_{0}^{+\infty}t^{2\alpha-1}e^{-t^2}dt}$, consider the ${\Gamma}$ function. Some resulting identities are ${\Gamma(\alpha+1)=\alpha\Gamma(\alpha),\,\Gamma(n+1)=n!,\,\Gamma(\dfrac{1}{2})=\sqrt{\pi}}$.
  * For expressions of the form ${\displaystyle B(p,q) = \int_0^{\infty} \dfrac{t^{p-1}}{(1+t)^{p+q}}dt,\int_0^1 u^{p-1}(1-u)^{q-1}(p\gt 0,\, q\gt 0)}$, consider the Beta function. Looking at the Gamma function gives ${B(p,q) = \dfrac{\Gamma(p)\Gamma(q)}{\Gamma(p+q)}}$. Such expressions commonly arise in trigonometric substitutions.[^cal1-249]
    * Generalization: ${\displaystyle \int_0^{+\infty} \frac{x^{p-1}}{1 + x^q} \, \mathrm{d}x = \frac{\pi}{q\,\sin\left( \frac{p\pi}{q} \right)}, \quad 0 \lt  p \lt  q}$.
    * Evaluate

      $$
      {\int_{0}^{\frac{\pi}{2}}\dfrac{sin^2x\,cos^2x}{(sinx+cosx)^6} dx}
      $$

      .
      When you see $R(-sinx,\,-cosx)=-R(sinx,\,cosx)$, set $u=tanx$, giving

      $$
      {\int_0^{\infty} \dfrac{t^2}{(1+t)^6}dt=B(3,3) = \dfrac{\Gamma(3)\Gamma(3)}{\Gamma(6)} = \dfrac{2! \cdot 2!}{5!} = \dfrac{4}{120} = \dfrac{1}{30}}
      $$

      .[^cal1-252]

<!-- source: calculus:L253-L259 -->

### Reciprocal Substitutions and Parameter Integrals {#reciprocal-and-parameter-integrals}

* Consider integrals of the form ${\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}$. Geometrically, its integral and ${{\displaystyle \int_{0}^{+\infty} \dfrac{1}{1+x^4}}}$ are mirror-symmetric about ${x=1}$, so the graphs are merely reversed and the area is unchanged; that is, ${{\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}={\displaystyle \int_{0}^{+\infty} \dfrac{1}{1+x^4}}}$. Consequently, evaluating ${{\displaystyle \int_{0}^{+\infty} \dfrac{x^2}{1+x^4}}}$ amounts to evaluating ${{\displaystyle \dfrac{1}{2} \int_{0}^{+\infty}\dfrac{1+x^2}{1+x^4}}}$. For integrals such as ${\displaystyle \int_{0}^{+\infty}\dfrac{1+x^2}{1+x^4+ax^2}}$, divide numerator and denominator by ${x^2}$ and match ${d(x+\frac{1}{x}),\,d(x-\frac{1}{x})}$.[^cal1-253]
  * Evaluate ${\displaystyle \int_{0}^{+\infty}\dfrac{1}{1+x^6}}$ (${\mathrm{Wu}_{comp}P_{102}-\text{note}}$).
    *

      $$
      \begin{aligned}\int \frac{1}{1+(x^2)^3}\,dx&= \int \frac{1}{(1+x^2)(1 - x^2 + x^4)}\,dx\\&= \int \frac{(1+x^2)-x^2}{(1+x^2)(1 - x^2 + x^4)}\,dx\\&= \int\Bigl(\frac{1}{1 - x^2 + x^4} - \frac{x^2}{1 + x^6}\Bigr)\,dx\\&= \int \frac{1}{1 - x^2 + x^4}\,dx \;-\; \int \frac{x^2}{1 + x^6}\,dx\\&= \tfrac12\int \frac{1 + x^2 - x^2 + 1}{1 - x^2 + x^4}\,dx \;-\; \tfrac13\int \frac{1}{1+(x^3)^2}\,d(x^3)\\&= \tfrac12\int \frac{1 + x^2}{1 - x^2 + x^4}\,dx \;+\; \tfrac12\int \frac{1 - x^2}{1 - x^2 + x^4}\,dx \;-\; \tfrac13\int \frac{1}{1+(x^3)^2}\,d(x^3)\\&= \tfrac12\int \frac{\tfrac1{x^2}+1}{\tfrac1{x^2}-1+x^2}\,dx \;+\; \tfrac12\int \frac{\tfrac1{x^2}-1}{\tfrac1{x^2}-1+x^2}\,dx \;-\; \tfrac13\arctan(x^3)\\&= -\tfrac12\int \frac{1}{\bigl(\tfrac1x-x\bigr)^2+1}\,d\Bigl(\tfrac1x - x\Bigr)\;-\;\tfrac12\int \frac{1}{\bigl(\tfrac1x+x\bigr)^2+3}\,d\Bigl(\tfrac1x + x\Bigr)\;-\;\tfrac13\arctan(x^3)\\&= -\tfrac12\arctan\!\Bigl(\tfrac1x - x\Bigr)\;-\;\tfrac1{2\sqrt3}\ln\!\Bigl|\frac{\tfrac1x + x - \sqrt3}{\tfrac1x + x + \sqrt3}\Bigr|\;-\;\tfrac13\arctan(x^3)+C\\\end{aligned}
      $$

      [^cal1-255]
  * Of course, these integrals can also be evaluated using the generalized ${Beta}$-function formula.
* Evaluate ${\displaystyle \int_{0}^{1}\dfrac{x^t}{lnx}dx}$. This type of integral calls for Feynman's method: differentiate with respect to ${t}$ inside the integral sign to obtain ${\displaystyle \int_{0}^{1}\dfrac{lnx\,x^t}{lnx}dx}$. Integration with respect to ${x}$ is now easy, giving ${\left. \dfrac{x^{t+1}}{t+1} \right|_{0}^{1}=t+1}$. Since this was obtained after differentiating with respect to ${t}$, integrate back with respect to ${t}$ to obtain ${ln(1+t)}$, the answer.[^cal1-257]
  * Evaluate ${\displaystyle \int_{0}^{1}\dfrac{x^7-x^3}{lnx}dx}$.
    * Apply the formula directly to obtain ${ln(1+7)-ln(1+3)=ln2}$.

<!-- source: calculus:L260-L262 -->

### Using Green's Theorem in Reverse on Disks {#green-formula-on-disks}

* Sometimes an integral is difficult and involves a disk together with properties of first and second derivatives. Consider using Green's theorem or the divergence theorem in reverse: turn an area integral into a line integral, then simplify using the problem's conditions to calculate the result.
  * ${f(x,\,y)}$ has continuous second partial derivatives on ${D(x,\,y)|x^2+y^2\leq1}$ and satisfies ${(f''_{xx}+f''_{yy})e^{x^2+y^2}=1}$. Find ${\displaystyle \iint_D\,(xf'_x+yf'_y)\,dxdy}$.
    * Use the given conditions fully. Moving to a circular region clearly makes polar coordinates more useful, so express the original integral in polar coordinates as ${\displaystyle I=\int_{0}^{\tfrac{\pi}{2}}\,d\theta\int_{0}^{1}(rcos\theta f'_x+rsin\theta f'_y)\,r\,dr}$. Use reverse parametrization for line and area integrals: ${-rsin\theta\,d\theta=dx}$. Rewrite the original integral as ${\displaystyle I=\int_{0}^{1}\,r\,dr\int_{0}^{\tfrac{\pi}{2}}(rcos\theta f'_x+rsin\theta f'_y)\,\,d\theta=\int_{0}^{1}\,r\,dr\oint_{L_r}\,(-f'_y+f'_x)d\theta}$, where ${L_r}$ is the boundary of ${D}$. Green's theorem then gives ${\displaystyle I=\int_{0}^{1}\,[\iint_D\,f''_{xx}+f''_{yy}\,dxdy]\,r\,dr}$. Substitute the original condition into this integral, then evaluate in polar coordinates to obtain ${\dfrac{\pi}{2e}}$.[^cal1-262]

<!-- source: calculus:L263-L264 -->

## Multivariable Differentiation {#calculus-h-04}

---

<!-- source: calculus:L265-L273 -->

### Continuity, Partial Derivatives, and Differentiability {#continuity-partials-differentiability}

* Concepts related to multivariable functions:
  * By the definition of continuity for multivariable functions, continuity $\Rightarrow$ the limit exists and equals the function value; the converse does not necessarily hold. There is no need to investigate this further.[^cal2-266]
  * Continuity at a point does not imply that partial derivatives exist there, nor that the function is differentiable there.
  * Existence of partial derivatives at a point only means that rates of change along the corresponding coordinate-axis directions exist and are equal. It does not guarantee equality along arbitrary paths, meaning that it cannot guarantee equal rates of change in all directions or the existence of the total differential, or gradient. **Thus, partial derivatives do not imply differentiability; differentiability does imply partial derivatives. If the partial derivatives are continuous, meaning that limits along arbitrary paths exist, the function is differentiable at that point. However, differentiability guarantees only a linear approximation.** The slopes of this linear approximation, namely the partial derivatives, may exist at the point without being continuous there. Thus differentiability does not imply continuity of the partial derivatives. A classic counterexample follows:[^cal2-268]

    $$
    f(x,y)=\begin{cases} \frac{x^2y}{x^4+y^2}, & (x,y)\neq (0,0) \\ 0, & (0,0) \end{cases},\,\text{differentiable at the origin, but its partial derivatives are not continuous there}
    $$

    [^cal2-269]
    * If ${f'_{x}(x_0,\,y_0),\,f'_{y}(x_0,\,y_0)}$ exist, determine what can be deduced:
      * Information at one point cannot establish information throughout a disk. Existence of partial derivatives does not imply continuity at the point, existence of the limit there, or that the function is defined throughout a neighborhood of the point.
      * Start from the expression: if ${f'_x(x_0,\,y_0)=\lim_{x\to x_0}\dfrac{f(x,\,y_0)-f(x_0,\,y_0)}{x-x_0}}$ exists, then with $y=y_0$, **the function is defined in a neighborhood of $x_0$. Similarly, it is defined in a neighborhood of $y_0$ along the other coordinate direction. But being defined in a neighborhood of the point means a disk, and two directions do not represent a disk.**
      * Fixing one variable turns the expression into a function of one variable. By the properties of single-variable functions, differentiability implies continuity at the point, so ${\lim_{x\to x_0}f(x,\,y_0)=f(x_0,\,y_0)}$.

<!-- source: calculus:L274-L276 -->

### Implicit Functions and Mixed Partial Derivatives {#implicit-functions-mixed-partials}

* For the existence of an implicit function, $F_y \neq 0$ is only a sufficient condition. If it is zero, this does not mean that an implicit function $y=y(x)$ does not exist, because $-\dfrac{F_x}{F_y}$ may be an indeterminate form whose limit exists, which would also yield the conclusion.[^cal2-274]
* For partial derivatives of multivariable functions, use the definition directly when there are no special restrictions. When finding second- or higher-order mixed partial derivatives, such as $f''_{xy}$, note that the other variable is fixed during the first differentiation. Using the definition, $f'_x=\lim_{x\to x_0}\dfrac{f(x+x_0,\,y)-f(x_0,\,y)}{x}$, then find $f''_{xy}$.[^cal2-275]

---

<!-- source: calculus:L277-L287 -->

### The Total Differential and Linear Approximation {#total-differential-linear-approximation}

* Differentiability of a multivariable function at a point means that there exist:

$$
\begin{align*}
\Delta z &= f(x+\Delta x,\,y+\Delta y) - f(x,y) \\
\Delta z &= A\Delta x(dx) + B\Delta y(dy) + o(\sqrt{x^2+y^2})
\end{align*}
$$

  This further gives ${\dfrac{(\Delta z - A\Delta x - B\Delta y) \Rightarrow o(\sqrt{x^2+y^2})}{\sqrt{x^2+y^2}}}=0$. By necessary conditions for differentiability, $A=\frac{\partial z}{\partial x},\,B=\frac{\partial z}{\partial y}$. If a question supplies related information, solve it using these concepts. For example:

  $$
  \lim_{x\to a,\,y\to b}\dfrac{f(x,\,y)-f(a,\,b)+3(x-a)-4(y-b)}{o(\sqrt{(x-a)^2+(y-b)^2})}=C(C\neq0)
  $$

  [^cal2-284]
  By the definitions of higher-order and equivalent infinitesimals, if the denominator becomes $\sqrt{x^2+y^2}$, the expression above has $C=0$, $\Rightarrow\,\,f(x,\,y)\text{ at }(a,\,b)$ is differentiable, with $A=-3,\,B=4$.[^cal2-285]

---

<!-- source: calculus:L288-L297 -->

### Finding Local and Global Extrema {#multivariable-extrema-workflow}

* Local and global extrema of multivariable functions:
  * Steps for finding local and global extrema:
    * For a multivariable function, first **check whether points where it is not differentiable are stationary points, then find stationary points using partial derivatives**. Denote these stationary points by $M_i$. At this stage, we have found the local extrema.[^cal2-290]
    * If the function is constrained by another equation, first **remove from $M_i$ the points that do not satisfy the constraint**, then use Lagrange multipliers to find extrema, obtaining $M_j$.
    * Finally, evaluate at several endpoints, namely endpoints of the constraint, obtaining $M_k$.
    * **Compare the final $M_i,\,M_j,\,M_k$ to obtain the maximum and minimum; $M_i$ are local-extremum points.** Note that a global-extremum point must be a local-extremum point, but not necessarily in the unrestricted sense: it may be a **constrained local extremum on the boundary. If a region has only one interior local extremum, it need not be the global extremum.** There may also be boundary extrema, and one of these may be the global extremum.[^cal2-293]
    * Supplement to the discussion above: given a function ${f(x,\,y)}$ and a region ${D}$, finding the global extrema can generally be divided into two steps:
      1. **If ${D}$ is a region rather than an equality constraint, first find the local extrema of ${f(x,\,y)}$ in the region. Then substitute the conditions defining ${D}$ to obtain a single-variable function ${g(x)}$ and find the boundary extrema. Finally, compare them to identify local and global extrema.**
      2. If the single-variable function is complicated and its extrema are difficult to find, Lagrange multipliers can still be used for boundary extrema. If ${D}$ is itself an equality constraint, construct the Lagrangian directly.
      3. Remember that the boundary of ${x\geq 0, y\geq 0}$ is ${x=0,\,y=0}$.

<!-- source: calculus:L298-L312 -->

### Lagrange Multipliers and Representative Examples {#lagrange-multipliers-examples}

* When using Lagrange multipliers to find extrema, the equations can sometimes be difficult to solve. Consider the following approaches:
  * Most commonly, if simple addition or subtraction cannot eliminate the unknowns, transform the equations into ${\begin{cases} \phi_1(\lambda)x+\mu_1(\lambda)y=0 \\ \phi_2(\lambda)x+\mu_2(\lambda)y=0 \end{cases}}$. Substitute ${(0,\,0)}$ to check whether it satisfies the constraint. Because a constraint exists, the equations must have a nonzero solution, allowing ${\lambda}$ to be found and then ${(x,\,y)}$.[^cal2-299]
  * Simplify by multiplying or dividing ${f_x',\,f'_y..}$, then add the resulting expressions and use the constraints to simplify the answer.
* Examples:
  * Find the semimajor and semiminor axes of the ellipse cut from the ellipsoid ${f(x,\,y,\,z)=\dfrac{x^2}{3}+\dfrac{y^2}{2}+z^2=1}$ by the plane ${D(x,\,y,\,z)=x+y+z=0}$.
    * **This is a highly representative problem.** Because the elliptical section necessarily passes through the origin, its semimajor and semiminor axes correspond to the maximum and minimum distances from the origin. Thus it becomes an extremum problem: maximize and minimize ${d^2=x^2+y^2+z^2}$.[^cal2-303]
    * Construct the Lagrangian ${L(x,\,y,\,z)=f(x,\,y,\,z)+\lambda D(x,\,y,\,z)}$. Using ${L'_x - L'_y,\,L'_y - L'_z,\,D(x,\,y,\,z)=0}$ gives ${\begin{cases} (2+2\lambda)x+(4+3\lambda)y=0 \\ (6+2\lambda)x+(-6-3\lambda)y=0 \end{cases}}$. Since this system must have a nonzero solution, solve ${\begin{vmatrix}2+2\lambda & 4+3\lambda \\ 6+2\lambda & -6-3\lambda \end{vmatrix}=0}$ for ${\lambda}$.[^cal2-304]
    * Even after finding ${\lambda}$, the coordinates remain difficult to obtain. But only the lengths, not the specific points, are required. Multiply ${L'_x,\,L'_y,\,L'_z}$ by ${x,\,y,\,z}$, respectively, add, and use the constraints to cancel terms. Finally, ${d^2=-\lambda}$.
  * Choose an arc 𝐿 on the counterclockwise-oriented ellipse ${C: \dfrac{x^2}{4} + y^2 = 1}$ to maximize the line integral ${\int dx+2dy}$.
    * The integral is readily found to be ${x+2y\,|_{A}^{B}}$. Thus choose the starting point ${A}$ to minimize ${x+2y}$ and the endpoint ${B}$ to minimize it. The problem therefore becomes a Lagrange-multiplier problem constrained by the ellipse equation, giving ${4\sqrt{2}}$.[^cal2-307]
* If a multivariable function has second partial derivatives in a region $D$, **global extrema in the region must exist**.[^cal2-308]
* By analogy with a single-variable function, if $F(x,\,y)$ has no local-extremum points in region $D$, meaning:
  1. There is no point with $F'_x=0,\,F'_y=0$.
  2. Points where partial derivatives do not exist also fail the definition of a local-extremum point.
  Then **its global extrema must occur on the boundary of $D$**, analogous to the endpoints of the domain in one variable.[^cal2-312]

<!-- source: calculus:L313-L318 -->

### When the Hessian Test Is Inconclusive {#degenerate-hessian-extrema}

* If $AC-B^2 =0$ and the question asks about local extrema, try the following methods:
  * Examine the Taylor expansion at the candidate point, meaning higher-order derivatives.
    * Example: $f(x,\,y)=x^2+y^2-6x+10$ has candidate point $(2,\,0)$. Examine the derivatives of $f(x,\,0)$ and find $f^{(3)} \neq 0$. By the single-variable criterion, a local extremum requires a nonzero derivative of even order, so $x=2$ is not a local-extremum point, and neither is $(2,\,0)$; hence the extremum.[^cal2-315]
  * Check along particular paths.
    * For example, $f(x,\,y)=x^4+y^4$ has candidate point $(0,\,0)$. Along $y=kx$, $f(x,\,kx)=x^4(1+k^4)$, which is minimized at $x=0$, so a local minimum exists.[^cal2-317]

---

<!-- source: calculus:L319-L322 -->

### Definition-Based Differentiation and Absolute Values {#partial-derivatives-square-root-signs}

* Differentiation of multivariable functions:
  * **Always remember the absolute value that appears when taking a square root while using the definition of a partial derivative.**
    * ${f(x,\,y)}$ is defined at ${(0,\,0)}$, and ${\displaystyle\lim_{(x,\,y)\to (0,\,0)}\dfrac{f(x,\,y)-(x^2+y^2)}{\sqrt{x^2+y^2}}=1}$. Discuss whether ${f'_x,\,f'_y}$ exist.
      * Using the definition of differentiability, ${\displaystyle\lim_{(x,\,y)\to (0,\,0)}\dfrac{f(x,\,y)-f(0,\,0)-(0x+0y)}{\sqrt{x^2+y^2}}-\sqrt{x^2+y^2}=1}$, so ${f(x,\,y)=\sqrt{x^2+y^2}+o(\sqrt{x^2+y^2})}$. Use the definition to differentiate with respect to ${x,\,y}$: ${\displaystyle\lim_{x\to0,\, y=0}\dfrac{\sqrt{x^2}}{x}}$ clearly does not exist, and the same holds for ${y}$.[^cal2-322]

<!-- source: calculus:L323-L330 -->

### Implicit Systems and Information About Second Derivatives {#implicit-systems-second-derivatives}

* If implicit functions ${x=x(y), z=z(y)}$ are defined by ${\begin{cases}F(f_1(x, y, z),\,g_1(x, y,z))=0\\ G(f_2(x, y, z),\,g_2(x, y,z))=0\end{cases}}$ and the question asks for ${\dfrac{dx}{dy},\,\dfrac{dz}{dy}}$, differentiate the system directly. Questions may give explicit or abstract functions. For example:
  * Implicit functions ${x=x(y), z=z(y)}$ satisfy ${\begin{cases}F(y-x,\,y-z)=0\\ G(xy,\,\frac{z}{y})=0\end{cases}}$. Find ${\dfrac{dx}{dy},\,\dfrac{dz}{dy}}$.
    * Differentiate each equation with respect to $y$ to obtain:

      $$
      {\begin{cases}F'_1\frac{dx}{dy}+F'_2\frac{dz}{dy}=F'_1+F'_2 \\ yG'_1\frac{dx}{dy}+\frac{1}{y}G'_2\frac{dz}{dy}=-xG'_1+\frac{z}{y^2}G'_2\end{cases}}
      $$

      **Additional reminder: For this type of differentiation, try to obtain the form above, then use Cramer's rule to find the answer.**
  * Similar problems: ${880_{base}(3.1),\,880_{comp}(3.15)}$.
* If some first-derivative information is given and second-derivative information is required, there are two approaches. First, **always consider differentiating both sides of the given equality**. Second, recover the original function and then differentiate it; this second approach is harder.
  * Suppose ${u(x,\,y)}$ has second partial derivatives, ${\dfrac{\partial^2u}{\partial x^2}=\dfrac{\partial^2u}{\partial y^2}}$, and ${u(x,\,2x)=x,\,u_1'(x,\,2x)=x^2}$. Find ${u_{11}''(x,\,2x)}$.
    * Always consider differentiating both sides of an equality for more information. This yields ${u_1'+2u_2'=1,\,u_{11}''+2u_{12}''=2x,\,u_{11}''+2u_{12}''+2u_{11}''+4u_{22}''=0}$. Since ${u_{11}''=u_{22}''}$, we obtain ${u_{11}''=-\dfrac{4}{3}x}$.[^cal2-330]

<!-- source: calculus:L331-L332 -->

## Double Integrals {#calculus-h-05}

---

<!-- source: calculus:L333-L337 -->

### The Mean Value Theorem and Symmetry {#double-integral-mean-value-symmetry}

* If a double integral cannot be evaluated and the expression involves the region's area, consider simplifying it with the **mean value theorem for double integrals** to obtain the answer.

---

* Evaluating double integrals:
  * If the integral is difficult, or **an abstract function appears** and direct evaluation is clearly impossible, **always examine symmetry under interchange of variables**. Check whether interchanging symbols leaves the integration region unchanged. This also applies to abstract functions and can simplify the problem. $(1k_{base}(14.14),\,Wu_{base}(9.11),\,880_{base}(2.3,\,2.4))$
  * For abstract functions or suspected symmetric integrals, examine the integration region and partition it, isolating portions symmetric about $x,\,y,\,y=x,\,y=-x\dots$ to simplify the integral. ($1k_{base}(14.17,\,14.23)$)

<!-- source: calculus:L338-L340 -->

### Shifted Polar Coordinates and Exact Differentials {#translated-polar-coordinates-exact-differential}

* If converting to polar coordinates involves a shifted circle, such as $(x-a)^2+(y-b)^2=a^2+b^2$, set $\begin{cases}x-a=rcos\theta\\ y-b=rsin\theta\end{cases}$ to shift its center. This gives $\int_{0}^{\sqrt{a^2+b^2}}$ and simplifies the integral.[^cal2-338]
* Note the following integral:
  * Set $F(x)=\int_{x}^{b}f(x)dx$, then match an exact differential:

    $$
    {\int_{a}^{b}dx\int_{x}^{b}f(x)f(y)dy=\int_{a}^{b}[\int_{x}^{b}f(y)dy]\,d\,[-\int_{x}^{b}f(y)dy]}
    $$

    [^cal2-340]

<!-- source: calculus:L341-L342 -->

## Differential Equations {#calculus-h-06}

---

<!-- source: calculus:L343-L353 -->

### Substitution and Sign Checks {#ode-substitution-signs}

* Solving differential equations:
  * If the equation is difficult to solve but solving for $x(y)$ is easy, consider taking the reciprocal to obtain $y^{-1}(x)$, then invert the relation to find $y(x)$.[^cal2-344]
  * More generally, it is not necessary to solve directly for $y(x)$ or $x(y)$. One can instead solve for $x(g(y)),\,y(h(x))$, then invert the relation to obtain the answer.
    * For $y'+1=e^{-y}sinx$, directly solving for either $x$ or $y$ is difficult. Multiplying the equation by $e^y$ reveals that the left-hand side is actually $(e^y)'$, allowing us to solve for $e^y=f(x)$.[^cal2-346]
  * If the question supplies information and asks for an infinitesimal equivalent to $f(x)$ of order $n$, use Taylor expansion to obtain relations among the constant terms.
  * Check whether the equation can be rewritten using ${\dfrac{x}{y},\,\dfrac{y}{x}}$ or similar forms. Such problems generally require dividing by a variable. **When dividing by a variable, pay attention to its sign**, just as with square-root signs in integration:
    * Find the general solution of ${xy'=\sqrt{x^2+y^2}+y}$.
      * The method is evident after dividing by $x$, but **a case distinction is needed here**.
      * If $x \gt  0$, then ${y'=\sqrt{1+\tfrac{y^2}{x^2}}+\dfrac{y}{x}}$; set ${t = \dfrac{y}{x}}$.
      * If $x \lt  0$, then ${y'=-\sqrt{1+\tfrac{y^2}{x^2}}+\dfrac{y}{x}}$; set ${t=\dfrac{y}{x}}$.

---

<!-- source: calculus:L354-L359 -->

### The Structure of Solutions to Linear Equations {#linear-ode-solution-structure}

* Structure of solutions to differential equations:
  * If several particular solutions ${y_1,\,y_2,\,y_3\dots}$ are given, subtracting two particular solutions gives the form of the homogeneous equation's general solution. Perform several nonlinear subtractions: subtract the other particular solutions from $y_1$ to obtain some nonlinear general solutions. These are the homogeneous general solutions, from which the homogeneous equation is obtained. Then substitute any particular solution and differentiate to find $f(x)$, giving the final equation $\dots=f(x)$.[^cal2-355]
  * If the question specifies several linearly independent particular solutions, such as ${y_1(x),\,y_2(x),\,y_3(x)}$, the general solution must have the structure ${C_1y_1+C_2y_2+C_3y^*}$. Note that ${C_1,\,C_2,\,C_3}$ are all different. Applying the first method gives ${C_1[y_1(x)-y_3(x)]+C_2[y_2(x)-y_3(x)]+y_3(x)}$, which simplifies to ${C_1y_1+C_2y_2+C_3y_3,\,(C_1+C_2+C_3=1)}$.[^cal2-356]
    * A second-order nonhomogeneous linear differential equation has three particular solutions: ${x,\,e^x,\,e^{-x}}$. Find its general solution.
      * Construct the homogeneous general solution directly from the data: ${C_1(e^x-x)+C_2(e^{-x}-x)}$. Then add the particular solution $x$.

---

<!-- source: calculus:L360-L360 -->

### Combining Differential Equations with Integral Conditions {#ode-integral-conditions}

* If the question explicitly or implicitly provides an integral representation of the function, combine the differential equation with that integral information. Integrating both sides of the differential equation may suggest a way forward. Similar example: $(660-81)$.

<!-- source: calculus:L361-L364 -->

## Infinite Series {#calculus-h-07}

---

> Weak area: power-series expansions of functions about a point; difficulty deciding which statements are true.

---

<!-- source: calculus:L365-L373 -->

### Term Limits and Convergence {#series-terms-convergence}

* Finding constants using infinite series:
  * For a series of any form, make good use of $\lim_{n\to \infty}u_n=0$ to determine constants.[^cal2-366]
  * If the series $\displaystyle\sum_{n=a}^{\infty}u_n+\sum_{n=a}^{\infty}v_n$ converges, use the condition that the limit equals $0$ to convert the expression into equivalent infinitesimals, evaluate the limit, and solve for the constants.[^cal2-367]

---

* Proving convergence or divergence:
  * Make good use of shared convergence behavior and equivalent infinitesimals. **Use mean inequalities effectively.** When $f(x)g(x),\,\frac{f(x)}{g(x)}$ appears, use a mean inequality, or use convergence of the terms to infer $u_nv_n=0 \Rightarrow \dfrac{u_n}{v_n}=0$ and obtain an equivalent-infinitesimal relation. If $v_n$ converges, then $u_n$ converges.[^cal2-370]
  * Prove divergence by showing $\lim_{n\to \infty}u_n \neq 0$. If $|u_{n+1}| \gt  |u_n|$ arises during the proof, then $u_n \neq 0$.
  * If $\displaystyle\sum_{n=a}^{\infty}u_n$ converges, then $u_{n}+u_{n+1}$ converges. The converse does not hold, because changing the order of evaluation without knowing convergence can affect convergence or divergence.[^cal2-372]
  * For expressions containing integrals that cannot be evaluated, or whose convergence cannot be determined directly, **consider bounding and comparison**.

<!-- source: calculus:L374-L383 -->

### Alternating Series and Taylor Expansion {#alternating-series-taylor-expansion}

* Alternating series:
  * If $(-1)^n$ is absent, introduce $(-1)^n$.
  * If the Leibniz test is difficult to apply, try Taylor expansion and analyze the convergence of the resulting series. If one diverges, the original series diverges. Alternatively, prove absolute convergence.[^cal2-376]
  * Equivalent infinitesimals can be used to determine convergence. If the two corresponding positive-term series behave similarly, the convergence behavior of one is known, and $\displaystyle\sum_{n=1}^{\infty}\xi_n = \sum_{n=1}^{\infty}u_n-\sum_{n=1}^{\infty}v_n$ converges, this establishes that the two series have the same conditional convergence/divergence behavior.[^cal2-377]
* When splitting alternating-series terms, learn to expose the alternating form. For example:

  $$
  \sum_{n=1}^{\infty}sin(n\pi+\dfrac{1}{\sqrt{n}}),\,\sum_{n=1}^{\infty}sin(\pi\sqrt{n^2+1})
  $$

   Using product-to-sum identities, the second expression can be changed into $(-1)^nsin(\dfrac{\pi}{\sqrt{n^2+1}+n})$.
* For series where comparison, root tests, or other positive-term-series tests cannot be applied, consider Taylor expansion. These problems can generally be simplified by canceling terms in their expansions. If an unknown parameter appears, expand first and then apply comparison to determine convergence.
  * Discuss convergence of ${\displaystyle\sum_{n=1}^{\infty}\dfrac{1}{n}-ln(1+\frac{1}{n}),\,\sum_{n=1}^{\infty}(a^{\frac{1}{n}}-\sqrt{1+\dfrac{1}{n}})}$.
    * For the first series, expand $lnx$ in a Taylor series: ${\dfrac{1}{n}-(\dfrac{1}{n}-\dfrac{1}{2n^2}+o(\dfrac{1}{n^2}))}$. It then has the same convergence behavior as $\dfrac{1}{n^2}$.
    * For the second series, similarly rewrite it as ${\displaystyle e^{\tfrac{lna}{n}}-(1+\tfrac{1}{n})^{\tfrac{1}{2}}}$, expand both terms, and discuss the cases.[^cal2-382]

---

<!-- source: calculus:L384-L388 -->

### Summation by Integration and the Constant of Integration {#power-series-integration-constant}

* Sum functions:
  * **When integrating to find a sum function, remember ${+C}$. Then substitute the value at ${x=0}$ to determine $C$.**
    * Find the sum function of ${\displaystyle\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^{n}}$.
      * Set ${G(x)=\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^{n+1}}$, so ${S(x)=\frac{G(x)}{x}}$. Differentiating and expanding $G(x)$ gives ${G'(x)=\sum_{n=1}^{\infty}n^2x^n-2nx^n+x^n}$. We then have:

        $$
        \begin{gather*} S_1(x) =x^n= \frac{1}{1-x}, \\ S_2(x) = x \cdot S_1'(x) = \frac{x}{(1-x)^2}, \\S_3(x) = x \cdot S_2'(x)=\dfrac{x(1+x)}{(1-x)^3},\\ G(x)=\int S_1(x)-2S_2(x)+S_3(x)\,dx \end{gather*}
        $$

        [^cal2-387]
        After setting $t=(1-x)$ and differentiating the expressions individually, we obtain $G(x)=f(x)+C$. Since $f(0)=-4+C$ and $G(0)=0$, we have $G(x)=f(x)+4,\,S(x)=\dfrac{G(x)}{x}$.

<!-- source: calculus:L389-L394 -->

### Recurrences and Equations for Generating Functions {#recursive-coefficients-generating-function}

* Abstract forms of sum functions:
  These usually require deriving a relationship between sum functions from the given conditions.
  * Obtain equivalent relations through integration or differentiation, similarly to integration by parts, then derive a relation involving the sum function. Integration or solving a differential equation can then give the sum function.
    * $①a_1=1,\,②a_{n+1}=(1-\dfrac{1}{2(n+1)})a_n$. Find the sum function of $\displaystyle\sum_{n=1}^{\infty}a_nx^n, |x|\lt 1$:
      Solution: Set $S(x)=\displaystyle\sum_{n=1}^{\infty}a_nx^n$. Differentiate to get $S'(x)=\displaystyle\sum_{n=1}^{\infty}(n+1)a_{n+1}x^{n}$. Also, $②\,\Rightarrow 1+\sum_{n=1}^{\infty}na_nx^n + \dfrac{1}{2}\sum_{n=1}^{\infty}a_nx^n$, so $S(x)=1+xS'(x)+\dfrac{1}{2}S(x)$. Solve the differential equation.[^cal2-393]
  * Other methods can be used similarly, such as Wallis' formula. ($Zhang_{base}(16.34)$)

<!-- source: calculus:L395-L400 -->

### Convergence Intervals, Standard Functions, and Partial Fractions {#power-series-domain-standard-functions}

* Because power functions are continuous, their sum is also continuous. Integration and differentiation during the summation process may introduce discontinuity points, which must be discussed separately.[^cal2-395]
  * At $0$, direct substitution is generally enough. Remember that the result need not be zero.
  * For any other constant $c$, if direct substitution fails, take the limit of the sum function at $c$ and evaluate that limit.
* As in Chapter 1, develop abstraction and avoid rigid thinking. For example, $\dfrac{1}{1-x}$ is the sum function of the geometric series $x^n$, and $\dfrac{1}{x-2}$ can be handled similarly: ${\Rightarrow \dfrac{1}{1-\frac{x}{2}}}$.[^cal2-398]
* For standard generating functions, learn to combine and split terms to obtain a $\sum_{1}^{\infty}$ form, then apply the standard formula. If some terms are missing, still sum with the formula and subtract the extra terms afterward.
  * For example, evaluate $\displaystyle\sum_{n=2}^{\infty}\dfrac{1}{(n^2-1)2^n}$. Treat $\dfrac{1}{2^n}$ as $x^n$, so the target is ${S(\frac{1}{2})}$. Split the terms directly: ${\displaystyle2\sum_{n=2}^{\infty}(\dfrac{x^n}{n-1}-\dfrac{x^n}{n+1}) \Rightarrow 2\sum_{n=2}^{\infty}(x\dfrac{x^{n-1}}{n-1}-\dfrac{1}{x}\times\dfrac{x^{n+1}}{n+1}) \Rightarrow 2\sum_{n=1}^{\infty}(x\dfrac{x^n}{n}-\dfrac{1}{x}\times\dfrac{x^{n+2}}{n+2})}$. This gives $2(x\times-ln(1-x) + \dfrac{-ln(1-x)-x-\frac{x^2}{2}}{x})$.[^cal2-400]

<!-- source: calculus:L401-L407 -->

### Extracting Odd Terms and Summing Factorial Series {#odd-terms-factorial-series}

* For a series missing even-index terms, **use ${\displaystyle\sum_{n=1}^{\infty}u_n=\dfrac{\sum_{1}^{n}u_n-\sum_{1}^{n}(-u_n)}{2}}$**.[^cal2-401]
  * Example: ${\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{2n+1}(\dfrac{x^2}{2})^{n}}$. First rewrite it entirely in odd powers:

    $$
    {\dfrac{\sqrt{2}}{x}\sum_{n=0}^{\infty}\dfrac{2}{2n+1}\cdot\dfrac{x^{2n+1}}{(\sqrt{2})^{2n+1}}}
    $$

     Using the formula above gives:

    $$
    {\dfrac{\sqrt{2}}{x}\times \dfrac{\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{n}\dfrac{x^{n}}{\sqrt{2}^{n}}\,-\,\displaystyle\sum_{n=1}^{\infty}\dfrac{2}{n}\dfrac{(-x)^{n}}{\sqrt{2}^{n}}}{2}}
    $$

    [^cal2-402]
    Then use the standard generating function $\displaystyle\sum_{n=1}^{\infty}\dfrac{x^n}{n}=-ln(1-x)$ to obtain:

    $$
    {\dfrac{\sqrt{2}}{x}\times 2\times \dfrac{(-ln|1-\dfrac{x}{\sqrt{2}}|\,+\, ln|1+\dfrac{x}{\sqrt{2}}|)}{2}}
    $$

* Series missing odd-index terms are handled similarly, but note that **an alternating form may require differentiation or integration**, as in ${\displaystyle \sum_{n=1}^{\infty}(-1)^n\dfrac{x^{2n-1}}{2n-1}}$.[^cal2-404]

---

* When summing a series, if $n!$ appears, consider $e^x$. Also remember that factorials of negative integers are undefined, so adjust the expression flexibly. Example:
  * Find $\displaystyle\sum_{n=0}^{\infty}\dfrac{n+1}{n!}$. Splitting it gives $\displaystyle\sum_{n=0}^{\infty}\dfrac{1}{(n-1)!}+\dfrac{1}{n!}$. Because negative factorials are undefined, rewrite this as $\displaystyle\sum_{n=1}^{\infty}\dfrac{1}{(n-1)!}+\sum_{n=0}^{\infty}\dfrac{1}{n!}=2e$.

<!-- source: calculus:L408-L413 -->

## Spatial Geometry, Curves and Surfaces, and Multivariable Integration {#calculus-h-08}

---

> **Applications and cautions for curves, surfaces, and the ${Guess,\,Green,\,Stokes}$ formulas are recorded in the corresponding Chapters 9 and 10 of the Li–Fan comprehensive review book.**[^cal2-410]

---

* Notation below: use $\tau$ for a direction vector and $n$ for a normal vector.

---

<!-- source: calculus:L414-L417 -->

### Plane Equations and Distances {#planes-equations-distances}

* For a plane:
  * The distance between two planes is $d=\dfrac{|D_1-D_2|}{\sqrt{A^2+B^2+C^2}}$. Do not forget the absolute value. There are two planes at distance $d$ from a given plane.[^cal2-415]
  * Given three points $A,\,B,\,C$, find the plane equation by computing ${\overrightarrow{AB},\,\overrightarrow{AC}}$ and ${\vec{n}=\overrightarrow{AB}\times \overrightarrow{AC}}$, then substituting any one of the points.[^cal2-416]

---

<!-- source: calculus:L418-L424 -->

### Lines in Space, Projections, and Distances {#space-lines-projections-distances}

* Lines in space:
  * Common methods for finding a spatial line's equation:
    * Find its direction vector $\tau$ and the coordinates of a point on it, then use $\dfrac{x-x_0}{l}=\dfrac{y-y_0}{m}=...$.[^cal2-420]
    * Use the given conditions to find the intersection of two planes. For a line given in point-normal form, a plane-pencil equation is generally available; substitute the known point into this equation to solve.
    * If two lines perpendicular to the required line are given, use ${\tau_1\times \tau_2=\tau}$ to find its direction vector $\tau$. Since the required line is perpendicular to both given lines, those two lines determine a plane. Use ${n_1=\tau\times\tau_1、n_2=\tau\times\tau_2}$ to obtain the normals of two planes, then substitute points from the corresponding given lines.[^cal2-422]
  * For the equation of a line's projection onto a plane, form a pencil of planes from the two planes defining the line. Establish a relation with the projection plane's normal $n$ to solve for $\lambda$ and obtain the other plane. The intersection of these two planes is the required projected line. Similar examples: ($Zhang_{base}(\text{Exercise }17.5),\,880_{base}(4.3.4)$).[^cal2-423]
  * Given two lines $L_1,\,L_2$, write a plane-pencil equation through $L_2$, denoted by $\pi$, then use parallelism with $L_1$ to determine $\lambda$ and the specific plane. Substitute any point from $L_1$ and calculate its distance to the plane.[^cal2-424]

<!-- source: calculus:L425-L426 -->

### Scalar Line Integrals Along a Segment {#line-segment-scalar-line-integrals}

* Line integrals along a spatial line:
  * If two points are given, calculate ${\tau}$, paying attention to its direction. Use the two-point equation of ${L}$, ${\dfrac{x-x_0}{l}=\dfrac{y-y_0}{m}=...=t}$, to parameterize ${x,\,y,\,z}$. Then compute ${||\tau||}$ and evaluate ${\displaystyle \int_Lf(x,\,y,\,z)\,ds=\int_{0}^{1} f\{x(t),\,y(t),\,z(t)}\}\,||\tau||dt$. (${880_{base}(2.6)}$)[^cal2-426]

<!-- source: calculus:L427-L429 -->

### Boundaries of surface projections {#surface-projection-boundaries}

* For the projection of a surface onto a region, or its projection region on a coordinate plane, meaning the projected curve, first find the plane's normal vector. Let the normal to the projection plane be ${\tau}$. Use ${n\cdot \tau=0}$ to establish a relation ${g(x,\,y,\,z)=0}$, giving ${\begin{cases} f(x,\,y,\,z)=0 \\ g(x,\,y,\,z)=0 \end{cases}}$. Assuming projection onto the ${xOy}$ plane, eliminate ${z}$ to obtain ${{\begin{cases} h(x,\,y)=0 \\ z=0 \end{cases}}}$.[^cal2-427]
  * For example, consider the projection curve of surface ${\displaystyle \sum: x^2+y^2+z^2-yz=1}$ on the ${\displaystyle xOy}$ plane. The coordinate-plane normal is ${\displaystyle n=(0,\,0,\,1)}$, and the surface tangent vector is ${\displaystyle \tau=(2x,\,2y-z,\,2z-y)}$. Using ${\displaystyle n\cdot \tau =0}$ gives ${\displaystyle {\begin{cases} 2z-y=0 \\ x^2+y^2+z^2-yz=1 \end{cases}}}$; eliminate ${\displaystyle z}$.[^cal2-428]

---

<!-- source: calculus:L430-L434 -->

### Coordinate Shifts for Surfaces of Revolution {#rotation-surfaces-coordinate-shifts}

* Finding a surface obtained by revolution about an axis:
  * If revolving about a coordinate axis, eliminate variables from the given equations. For example, revolving about the $z$ axis gives ${x=f(z)、y=g(z)\Rightarrow x^2+y^2=f^2(z)+g^2(z)}$.
  * If revolving about another location, such as ${x=1、\begin{cases}x=2\\ y=3\end{cases}}$, use a translation: add for leftward or upward shifts, and subtract for rightward or downward shifts. Convert the problem to revolution about a coordinate axis, use the preceding method, then reverse the translation. For example:[^cal2-432]
    * Find the equation obtained by revolving line ${\dfrac{x-1}{3}=\dfrac{y-2}{4}=\dfrac{z+1}{1}}$ once around ${\begin{cases}x=2\\ y=3\end{cases}}$.
      * First translate to get ${\dfrac{x+1}{3}=\dfrac{y+1}{4}=\dfrac{z+1}{1}}$. Rewrite this as intersecting planes: ${\begin{cases}x=3z+2\\ y=4z+3\end{cases}}$. Then ${x^2+y^2=25z^2+36z+13}$. Translate back to obtain ${(x-2)^2+(y-3)^2=25z^2+36z+13}$.

<!-- source: calculus:L435-L440 -->

### Revolving Intersecting Lines and Spherical-Coordinate Bounds {#rotating-intersecting-lines-spherical-bounds}

* More generally, if **a line revolves around a given axis**, such as ${x=y=z}$, use the fixed-angle method, ${cos\theta=\dfrac{|r \cdot \tau|}{|r||\tau|}}$, because the angle stays fixed when a line revolves around an axis.[^cal2-435]
  * ${\sum}$ is the surface formed by revolving line ${\begin{cases}x=0\\y=0\end{cases}}$ around ${x=y=z}$. Find the equation of ${\sum}$.
    * The axis direction vector is ${\tau=(1,\,1,\,1)}$. Choose a point ${r=(0,\,0,\,1)}$. For any point $(x,\,y,\,z)$ on the line, ${cos\theta=\dfrac{r\cdot \tau}{|r||\tau|}=\dfrac{1}{\sqrt{3}}=\dfrac{|x+y+z|}{|\tau||\sqrt{x^2+y^2+z^2}|}}$, giving $xy+xz+yz=0$.

---

* During calculations, always inspect the integration region, particularly $\phi$ in spherical coordinates. The range of $\phi$ changes for a cone or a sphere whose center is not at the origin.

---

<!-- source: calculus:L441-L443 -->

### Cones, vertices, and directrices {#cones-and-directrices}

* Finding a cone's equation from its vertex and directrix:
  * The vertex is the origin, and the directrix is ${\displaystyle \begin{cases} z=y^2 \\ x=1 \end{cases}}$, ${\displaystyle (|y|\leq 1)}$. Use parametric equations. Let a point on the cone be ${\displaystyle P=(1,\,u,\,u^2)(|u|\leq 1)}$. The line through the vertex and this point is ${\displaystyle (x,\,y,\,z)= \lambda \overrightarrow{OP} =\lambda(1-0,\,u-0,\,u^2-0),\,\lambda \gt  0}$. Points on this line are all points of the cone. Eliminate ${\displaystyle \lambda,\mu }$: since ${\displaystyle x=\lambda,\,y=\lambda u,\,z=\lambda u^2}$, we have ${\displaystyle \lambda=x,\, u=\dfrac{y}{x},\,z=x\times \dfrac{y^2}{x^2}}$. **Next, pay attention to all parameter ranges.** Because ${\displaystyle |u|\leq 1}$, ${\displaystyle \lambda \leq 0 \rightarrow x\leq 0;\, |\dfrac{y}{x}|\leq 1 \rightarrow |y|\lt x}$.[^cal2-442]
  * If the vertex ${\displaystyle (a,\,b,\,c)}$ is not the origin, translate it to the origin, keeping the same directrix. The translated vertex is ${\displaystyle x'=x-a,\,y'=y-b,\,z'=z-c}$, and a point on the directrix is ${\displaystyle (1-a,\,u-b,\,u^2-c),\,|u| \leq 1}$. Points on the cone then satisfy ${\displaystyle (x',\,y',\,z')=\lambda(1-a,\,u-b,\,u^2-c),\,\lambda \leq 0}$. Eliminate the parameters.[^cal2-443]

<!-- source-content:calculus:end -->

## Linear Algebra {#linear-algebra}


<!-- source-content:algebra:start -->
<!-- source: algebra:L1-L2 -->

> When working on a new problem, I recall a similar problem from the past. In solving it, I discover gaps in my understanding of this type of problem and of the tools and methods involved, or find that my understanding is not deep enough. By solving the new problem, I also update and refine my earlier understanding.

---

<!-- source: algebra:L3-L4 -->

### Elementary Operations, Rank, and Linear Systems {#algebra-rank-and-systems}

* We know that an **invertible matrix $A$** can be obtained through transformations by several elementary matrices $P_1P_2\dots$. Conversely, a matrix formed as a product of elementary matrices must be invertible. In other words, if $P_1,\,P_2$ are invertible, their product is invertible too (proof by contradiction suffices: $P_1P_2=Q\Rightarrow Q(P_1P_2)^{-1}=E$). Thus, two equivalent matrices $A,\,B$ can always be related through a sequence of elementary row (column) interchanges.[^alg-3]

---

<!-- source: algebra:L5-L8 -->

* When you see $A_{m\times n}B_{n \times m}=O$:
  * Think of $r(A)+r(B)\leq n$. Here, $B$ can be $A$. Similar problem: ($1k_{base}4.7$).
  * If ${AB=O}$ and ${r(B)=r}$, **partition matrix ${B}$ by columns**. We know that **${B}$ has ${r}$ linearly independent column vectors**, corresponding to ${A\xi_i=0}$. That is, ${A}x=0$ has **${r}$ linearly independent solution vectors and ${r}$ linearly independent eigenvectors corresponding to eigenvalue ${0}$**.[^alg-7]

---

<!-- source: algebra:L9-L11 -->

* Suppose matrix $A$ has $r(A) = m$, that is, two linearly independent solution vectors. The number of linearly independent solution vectors of $Ax=b$ is $m+1$.[^alg-9]
  * Proof: take $r(A)=2$ as an example. There are linearly independent vectors $k_1\xi_{1},\,k_2\xi_{2}$, and the general solution of $Ax=b$ is $k_1\xi_{1},\,k_2\xi_{2} + \eta$. Then there are $\eta,\,\eta+k_1\xi_1,\,\eta+k_2\xi_2$, giving $2 + 1$ linearly independent solution vectors.[^alg-10]

---

<!-- source: algebra:L12-L18 -->

* For $Ax=B$, or the more implicit statement that $A,\,B$ are systems with the same solutions, we have $A^Tx=0$ and $\left[\begin{array}{c}A^T \\ B^T \end{array}\right]x=0$; that is, the two systems have the same solutions.[^alg-12]
  * Proof:
    1. $\left[\begin{array}{c}A^T \\ B^T \end{array}\right]x=0$ clearly contains $A^Tx=0$.
    2. From $r(A)=r(B)=r([A, B])$, we have $r(A^T)=r(B^T)=r(B^TA^T)=r(A^TB^T)$.[^alg-15]
       Thus, the two systems contain the same number of solution vectors, and the containment in $1$ means that they have the same solutions.
* $Ax=0$ and $AA^Tx=0$ also have the same solutions.[^alg-17]

---

<!-- source: algebra:L19-L25 -->

* When assigning free variables while solving a linear system, use the pivot positions: variables other than the pivots can be changed freely. For example:
  * The entries in every row of $A$ sum to $k$, and it is real symmetric with the double eigenvalue $\lambda = 1$. Find all eigenvalues and eigenvectors of $A$.
    Solution:
    First, a row sum of $k$, or the more implicit statement that every column of the real symmetric matrix sums to $k$, implies that when $\xi = [1,1,1]^{T}$, we have $A\xi_3=k\xi_3\,\Rightarrow \lambda = k$. Since $A$ is real symmetric, orthogonality allows us to set $\xi=[x_1,\,x_2,\,x_3]$, giving $\xi\times\xi_3=0\Rightarrow x_1+x_2+x_3=0$. Because the diagonal must have a value, the pivot is in the first column. Set $\xi_1=[y,1,0],\,\xi_2=[y',0,1]$ and solve.[^alg-22]

---

<!-- source: algebra:L26-L51 -->

### Rank-One Matrices, Eigenvalues, and Adjugates {#algebra-rank-one}

* **A summary of rank-one matrices:**
  * Given two $n$-dimensional column vectors $\alpha,\,\beta$, we have:
  * ${\alpha^T\beta=\beta^T\alpha=C}$, also called the inner product ${(\alpha,\,\beta)}$.
  * ${\alpha\beta^T=A,\,\beta\alpha^T=B}$. Examining the two matrices closely, all rows are proportional, so both are rank-one matrices: $r(A)=r(B)=1$. Furthermore, **the sum of the main-diagonal entries is ${C=tr(A)=tr(B)=(\alpha,\,\beta)}$. This leads to a key application:** a rank-one matrix can be decomposed as $\alpha\beta^T$, making $A^n$ extremely easy to calculate.[^alg-29]
  * ${A^2=\alpha(\beta^T\alpha)\beta^T=CA\Rightarrow \lambda=0\,\text{or}\,\lambda = C=tr(A)}$. Since $\sum\lambda=tr(A)$, we obtain ${\lambda_1=tr(A),\,\lambda_{2,\,3,\,\dots}=0}$. With these conclusions, products of eigenvalues can simplify the calculation of determinants such as $|A+kE|,\,|A^{-1}|$.[^alg-30]
    * Because of the special property ${\alpha^T\beta=\beta^T\alpha=C}$, if matrix $M$ is related to $\alpha\beta^T$, we can calculate $M^2$ and then use $C$ to simplify its properties. Next, use **long division** by ${(M+iE)}$ to obtain $(M+iE)(M+jE)=kE$, giving an expression for $(M+iE)^{-1}$.
    * Because **$tr(A)$ is a root of multiplicity one and $0$ is a root of multiplicity $n-1$**, the eigenvectors can be found directly by solving the homogeneous system.[^alg-32]
  * Consider the necessary and sufficient condition for diagonalization by similarity: $n$ linearly independent eigenvectors.
    * **If $tr(A)\neq0$**, a specific $\xi_1$ can be found. Since $\lambda_{2\dots n}$ correspond to ${A\xi_{2\dots n}=0}$, and ${r(A)=1\Rightarrow n - r(A)=n-1}$ linearly independent solution vectors exist, **$A$ is diagonalizable by similarity in this case**.
    * **If $tr(A)=0$**, the corresponding equation is ${A\xi_{1\dots n}=0}$. Then ${r(A)=1\Rightarrow n - r(A)=n-1}$, so linearly dependent solution vectors must exist, and **$A$ is not diagonalizable by similarity**.
  * Going further, if ${(\alpha,\,\beta)=0}$ and ${\alpha,\,\beta}$ are unit column vectors, they are mutually orthogonal, giving the following properties:
    * If ${A=\alpha\beta^T + \beta\alpha^T}$, then ${A=\alpha\beta^T + (\alpha\beta^T)^T=A^T}$. Thus, $A$ is symmetric and diagonalizable by similarity.
    * For a symmetric rank-one matrix, there is only one ${\lambda \neq 0}$. In ${Q^TAQ=\Lambda}$, let the corresponding **nonzero, orthonormalized eigenvector** be ${\xi}$. **Because all other column vectors of the diagonal matrix are zero, ${A=\xi^T\lambda\xi}$ gives ${A}$ quickly**. Otherwise, matrix ${P}$ must also be found, and then ${A=P\,\Lambda\,P^{-1}}$.[^alg-38]
    * Multiply both sides on the left by ${\alpha,\,\beta}$ to obtain ${①\,A\alpha=\alpha\beta^T\alpha+\beta\alpha^T\alpha=\beta,\,②\,A\beta=\alpha\beta^T\beta+\beta\alpha^T\beta=\alpha}$. Thus:[^alg-39]
      * ${①+②=A(\alpha+\beta)=(\alpha+\beta)}$
      * ${①-②=A(\alpha-\beta)=-(\alpha-\beta)}$
      * Therefore, **two eigenvalues of ${A}$ are ${1,\,-1}$. Since ${r(A)\leq r(\alpha\beta^T)+r(\beta\alpha^T)=2}$, the remaining eigenvalues are ${0}$, and the eigenvectors are ${(\alpha+\beta),\,(\alpha-\beta),\,\xi_i}$**.
    * If ${\alpha=\beta}$, its eigenvector is ${\alpha}$ and ${A=\alpha\alpha^T}$. This uses the property of orthogonal similarity introduced in the positive-definiteness section.[^alg-43]
  * If ${r(A)\neq 1}$, so the situation above does not apply, there is another special rank-one matrix: ${r(A)=n-1\Rightarrow r(A^*)=1}$. Solution vectors of ${A^*X=0}$ can then be found as follows: ${A^*A=|A|E=O}$. **Thus, ${A}$ must have ${n-1}$ columns that are solution vectors of ${A^*X=0}$**, and the solution vectors must also be linearly independent. Since ${r(A)=n-1}$ tells us that only one corresponding eigenvalue is ${0}$, **these are all eigenvectors excluding the one for ${\lambda =0}$, namely ${\xi}$**.[^alg-44]
  * Use these conclusions flexibly. Whenever a rank-one matrix appears, think of the conclusions above. Examples:
    * Find ${A=\begin{vmatrix}0 & 2 & 3 & 4 \\2 & 3 & 6 & 8 \\3 & 6 & 8 & 12 \\4 & 8 & 12 & 15\end{vmatrix}}$.[^alg-46]
      > Observe that ${B=A+E}$ is a rank-one matrix, so ${|A|=|B-E|}$. Since $\lambda_{B_i}$ are ${30,\,0,\,0,\,0}$, $\lambda_{A_{i}}$ are $29,\,-1,\,-1,\,-1$. Hence ${|A|=-29}$.
    * For the order-${n(n\geq 2)}$ matrix ${A=\begin{vmatrix} a & 1 & 1 & ... & 1 \\ 1 & a & 1 & ... & 1 \\ . & . & . & ... & . \\ . & . & . & ... & . \\ 1 & 1 & 1 & ... & a \end{vmatrix}}$, find the eigenvalues and eigenvectors of ${A}$.
      > Direct method: add all rows to the first row, factor out the common multiplier, and then subtract. This is relatively complicated.
      Convert it into a rank-one matrix: $A=(a-1)E+B$. Since ${r(B)=1,\,tr(B)=n}$, the eigenvalues of ${B}$ are ${n,\,0,\dots}$, with eigenvectors ${\alpha_1,\,\alpha_2,\,\alpha_3}$. Thus, the eigenvalues of $A$ are ${a+1+\lambda_i}$, and the eigenvectors remain unchanged.[^alg-50]

---

<!-- source: algebra:L52-L66 -->

### Bases, Coordinates, and Equivalent Vector Families {#algebra-bases-and-coordinates}

* Basis coordinates
  * Definition: ${\alpha = a_1\xi_1+a_2\xi_2+\dots+a_n\xi_n}$, where ${\xi}$ is the basis space, and ${[a_1,\,a_2...\,a_n]}$ gives the coordinates in that basis space.[^alg-53]
    * There is a basis ${\alpha_1=(a_1,\,a_2,\,a_3),\,\alpha_2=(b_1,\,b_2,\,b_3),\,\alpha_3=(c_1,\,c_2,\,c_3)}$. Find the coordinates of ${\beta=(k_1,\,k_2,\,k_3)}$ in this basis space.
      * Directly set the coordinates in this space to ${(x_1,\,x_2,\,x_3)}$, then establish the equivalence ${\beta=x_1\alpha_1+x_2\alpha_2+x_3\alpha_3}$. Since only $x$ is unknown, this becomes a linear system in three unknowns to solve.
    * There are two three-dimensional basis spaces, ${\alpha_i,\,\beta_i}$. Find the vectors whose coordinates are the same in the two bases.
      * Again, set the coordinates and establish the equivalence: ${x_i\alpha_i=x_i\beta_i\Rightarrow x_i(\alpha_i-\beta_i)=0}$. This is still a linear system in three unknowns, so convert the problem into solving a linear system.[^alg-57]
  * For a matrix formed from a basis, ${P=(\xi_1,\,\xi_2...)}$, there are two methods for finding the transition matrix from ${AP}$ to ${BP}$. The first transposes the basis and treats it as row vectors, giving ${PB=PAC^T \Rightarrow C^T=(PA)^{-1}(PB)=A^{-1}B}$, after which ${C}$ is transposed back. The second constructs the matrix by inspection and requires less calculation.[^alg-58]
    * Example: ${\beta_1,\,\beta_2,\,\beta_3}$ form a basis. Find the transition matrix from basis ${\beta_1,\,2\beta_2,\,3\beta_3}$ to basis ${\beta_1-\beta_2,\,\beta_2+\beta_3,\,\beta_3-\beta_1}$.
      > Method 1: use the method above to calculate ${A^{-1}B}$, then transpose. Here, ${A,\,B}$ are the respective coefficient matrices after the matrix factorizations.
      > Method 2: by inspection, ${[\beta_1-\beta_2,\,\beta_2+\beta_3,\,\beta_3-\beta_1]=[{\beta_1,\,2\beta_2,\,3\beta_3}]\begin{bmatrix} 1 & 0 & -1 \\ -\frac{1}{2} & \frac{1}{2} & 0 \\ 0 & \frac{1}{3} & \frac{1}{3} \end{bmatrix}}$.
      > Clearly, Method 2 is much simpler. Use Method 2 whenever possible.
  * **Transition matrices between coordinates**
    * For column-vector coordinates, ${\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}=P \cdot \begin{bmatrix} y_1 \\ y_2 \\ y_3 \end{bmatrix}}$. For row-vector coordinates, ${\begin{matrix}[x1 & x2 & x3]\end{matrix}=\begin{matrix}[y1 & y2 & y3]\end{matrix} \cdot P^T}$.
    * **Note:** ${P}$ above is not the matrix's transition matrix. For column vectors, ${P}$ is the transition matrix of basis coordinates. For row-vector ${P}$, since a transition matrix transforms each column while ${P}$ transforms each row, the true transition matrix should be ${P^T}$.[^alg-65]

---

<!-- source: algebra:L67-L75 -->

* The vectors ${\alpha = \alpha_1,\,\alpha_2,\dots,\,\alpha_n}$ are linearly independent.
  * If there are vectors ${\beta = \beta_1,\,\beta_2,\dots,\beta_n}$ that can represent ${\alpha}$, the two families are equivalent.
    * Proof: if $\beta$ can represent $\alpha$, then ${r(\beta)\geq r(\alpha) = n}$, so the two families are equivalent. **This is a sufficient condition.**
  * Under the same conditions, ${\alpha}$ forms the matrix ${A=({\alpha = \alpha_1,\,\alpha_2,\dots,\,\alpha_n})^T}$, and $\beta$ forms the matrix $B$; the two matrices are equivalent.
    * Proof: sufficiency follows immediately from the first point. Next, **prove necessity**. Since the two matrices are equivalent, their row-vector families are equivalent. This means ${r(\begin{bmatrix} A \\ B \end{bmatrix})=r(A)=r(B)}$, that is, $A,\,B$ have the same solutions, so they can represent each other. **This also gives an important property: equivalent row-vector families of two matrices ${\Leftrightarrow}$ the two matrices have the same solutions.**[^alg-71]
    * For matrices ${Q_{m\times n},\,P_{n\times m},\,r(Q)=n}$, construct systems with the same solutions to obtain an equivalence. Let ${\xi}$ be a solution of $PX=0$. Since $Q$ has only the zero solution, $Q(PX)=0$, and ${PX=0\Rightarrow P\xi}$ is a solution of $QY=0$. With ${(QP)\xi=0}$, $\xi$ is a solution for $QP$. Thus, ${QP}$ and $P$ have the same solution $\xi$.[^alg-72]
    * Example: an $n$-dimensional column vector $\alpha$ satisfies $\alpha^T\alpha=2$, and $A,B$ are order-$n$ matrices. Given ${A(E-2\alpha\alpha^T)=B}$, do ${A^TX=0,\,B^TX=0}$ have the same solutions?
      > By **the rank-one-matrix property, $\alpha\alpha^T$ has eigenvalue ${2}$, so $|E-2\alpha\alpha^T|\neq0$**. Denote it by $C$. Then $AC=B,\,A=BC^{-1}$: **the columns of $B$ can be expressed as linear combinations of the columns of $A$**. Likewise, the columns of $A$ can be expressed using those of $B$. Thus, $A,\,B$ have equivalent column-vector families, so $A^T,\,B^T$ have equivalent row-vector families, and $A^T,\,B^T$ have the same solutions.

---

<!-- source: algebra:L76-L81 -->

### Quadratic Forms, Congruence, and Diagonal Forms {#algebra-quadratic-forms}

* Quadratic forms of matrices
  * Only congruence of real symmetric matrices is discussed here. Two nonsymmetric matrices can also be congruent, **but a symmetric matrix and a nonsymmetric matrix cannot be congruent**.
  * Quadratic forms of matrices
    * A diagonal form of a quadratic form can be obtained by completing the square, an orthogonal transformation using eigenvalues and eigenvectors, or elementary transformations (**to be added later: that problem in the 1k problem set**). **Completing the square can produce the canonical form, while an orthogonal transformation can produce the canonical form only for a matrix whose eigenvalues are ${1,\,-1,\,0}$.**
    * If the problem's diagonal form is obtained **by an invertible but nonorthogonal linear transformation**, only completing the square can be used. It also implies that transforming ${f(x_1,\,x_2,\,...)}$ into ${f(y_1,\,y_2,\,...)}$ produces a coefficient matrix that is not similar to the original coefficient matrix.[^alg-80]
    * For the number of solutions of a quadratic form, if, in ${X^TAX}$, the eigenvalues of ${A}$ are known, the number of solutions of ${X^Tf(A)X}$ equals the number of ${\lambda_{f(A)}=0}$, and the solution vectors are the corresponding eigenvectors.[^alg-81]

<!-- source: algebra:L82-L96 -->

### Extrema of Quadratic Forms: Ordinary and Generalized Rayleigh Quotients {#algebra-rayleigh-quotients}

* Extrema of quadratic forms
  * For extremum problems of the form ${\dfrac{f(x)}{g(x)}}$ or ${\dfrac{x^TAx}{x^TBx}}$, the usual form is the extremum problem for ${\dfrac{x^TAx}{x^Tx}}$. Such problems can be handled by an orthogonal transformation of ${A}$, namely ${A = Q^T\Lambda Q,\,x=Qy}$, converting the expression to ${\dfrac{y^T\Lambda y}{y^Ty}}$. Write this as ${\dfrac{\displaystyle\sum_{i}^{n}\lambda_i\cdot y_i^2}{\displaystyle\sum_{i}^{n} y_i^2}}$. The extreme values can then be taken as ${max/min{\sum_{1}^{n}\lambda_i}}$. Set ${y_i=1}$, then use ${x=Qy}$ to obtain the extremum points ${x}$.[^alg-83]
    * Example: consider the quadratic form ${f(x_1, x_2, x_3) = \mathbf{x}^\mathrm{T} \begin{bmatrix} 1 & 0 & 6 \\ 4 & 4 & 4 \\ 0 & 8 & 9 \end{bmatrix} \mathbf{x}}$, where ${\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}}$.
      (Ⅰ) Use an orthogonal transformation ${\mathbf{x} = Q\mathbf{y}}$ to put it into diagonal form, and find ${Q}$.

      (Ⅱ) Find the maximum of ${g(x_1, x_2, x_3) = \dfrac{f(x_1, x_2, x_3)}{x_1^2 + x_2^2 + x_3^2}}$ and give one point where it is attained, with ${x_1^2 + x_2^2 + x_3^2 \ne 0}$.
    > The quadratic-form coefficient matrix ${\begin{bmatrix} 1 & 0 & 6 \\ 4 & 4 & 4 \\ 0 & 8 & 9 \end{bmatrix}}$ clearly has **eigenvalues that are difficult to find, so rearrange the quadratic form by completing the square**. This gives ${A=\begin{bmatrix} 1 & 2 & 3 \\ 2 & 4 & 6 \\ 3 & 6 & 9 \end{bmatrix}}$. By the **rank-one-matrix property**, its eigenvalues are ${\lambda=14,\,0,\,0}$. Find the eigenvectors ${\xi_1,\,\xi_2,\,\xi_3}$. Since it is real symmetric, directly take ${\xi_1=(1,\,2,\,3)^T}$. To avoid Gram–Schmidt, set ${\xi_2=(0,\,-3,\,2)^T,\,\xi_3=(k,\,2,\,3)^T}$ and solve for ${k}$. Normalize the vectors separately to obtain ${Q}$.[^alg-88]
    > Use ${x=Qy}$ to transform the quotient into ${\dfrac{y^T(Q^TAQ)y=y^T\Lambda y}{y_1^2+y_2^2+y_3^2}=\dfrac{14y_1^2+0+0}{y_1^2+y_2^2+y_3^2}}$. Clearly, ${y=(1,\,0,\,0)^T}$ gives the maximum ${14}$. Then ${x=Qy=\xi_1}$, where ${\xi_1}$ here is the normalized eigenvector.
  * For an extremum problem of the form ${\dfrac{x^TAx}{x^TBx}}$, the denominator no longer has the usual ${x^Tx}$ form. First transform the denominator using ${x=Qy}$ to obtain ${\dfrac{y^T(Q^TAQ)y}{y^Ty}}$. Write ${M=Q^TAQ}$. Now ${\dfrac{y^TMy}{y^Ty}}$ has the usual form, so perform another transformation on the numerator. To obtain the extremum points, use ${z=Py,\,x=Qy}$ to find the extremum points ${x}$ ${}$.[^alg-90]
    * Example: consider the quadratic form $f(x_1, x_2) = x_1^2 - 4x_1x_2 + 4x_2^2$, and suppose the quadratic-form matrix of $g(x_1, x_2)$ is $B = \begin{bmatrix} 1 & -1 \\ -1 & 2 \end{bmatrix}$.
      (1) Does an invertible matrix $D$ exist such that $B = D^T D$? If so, find $D$; if not, explain why.
      (2) Find $\displaystyle\max_{x \neq 0} \frac{f(x)}{g(x)}$ and the corresponding ${x}$, where $x = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$.
    > **For ${B=D^TD}$, it is enough to have ${y=Dx}$ such that ${y^Ty=x^T(D^TD)x=x^TBx}$.** Complete the square in ${g(x_1,\,x_2)}$ to obtain ${g(x)=(x_1-x_2)^2+x_2^2}$. Hence there is a matrix ${y=Dx}$, with ${D=  \begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix}}$.
    > Substitution gives the form ${\dfrac{x^TAx}{x^TBx}}$. Apply an orthogonal transformation to ${B}$ and find that the eigenvalues of ${B}$ are difficult to calculate, but the essential aim is to put the denominator into the form ${y^Ty}$. Use the result of ${(1)}$ (completing the square also works), applying ${x=D^{-1}y}$. This gives ${y^T((D^{-1})^TBD^{-1})y}$. Since ${B=D^TD}$, the expression becomes ${y^Ty}$. The numerator now has the form ${y^T((D^{-1})^TAD^{-1})y}$. Let ${M=(D^{-1})^TAD^{-1}=\begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}}$. The **rank-one-matrix** property gives ${\lambda=2,\,0}$. Find ${\xi_1,\,\xi_2}$ and normalize them separately to obtain ${Q}$. A second orthogonal transformation puts the quotient into ${\dfrac{z^T\Lambda z}{z^Tz}=\dfrac{2z_1^2+0}{z_1^2+z_2^2}}$, so the maximum is ${2}$, with ${z=(1,\,0)^T,\,y=Qz,\,x=D^{-1}y}$.
    * The example above shows that the first transformation aims to produce a canonical form, **using completion of squares or elementary transformations; an eigenvalue method can also work when the eigenvalues meet the requirements**. The two transformations can be summarized as: **find an invertible matrix ${P}$ that puts ${f(x)}$ into canonical form and ${g(x)}$ into diagonal form**.[^alg-96]

<!-- source: algebra:L97-L106 -->

### Positive Definiteness, Spectral Decomposition, and Matrix Square Roots {#algebra-positive-definiteness}

* Positive-definite matrices
  * For a positive-definite matrix, consider a quadratic form of the form ${f(x_1,\,x_2\dots)=(ax_1+bx_2+\dots)^2+(cx_1+dx_2+\dots)^2\dots}$. If this quadratic form is positive definite, then ${f(x_i) = 0}$ has only the zero solution; that is, its coefficient matrix ${{A=\begin{pmatrix}a & b & \dots \\ c & d & \dots \\ \dots & \dots &\dots \end{pmatrix}}}$ is invertible.[^alg-98]
  * An inequality involving congruence can be converted into a positive-definite-matrix problem. For example, ${|X^TAX|\lt |X^TX|}$ means ${X^T(-E)X\lt X^TAX\lt X^T(E)X}$. **That is, ${X^T(A-E)X\lt 0,\,X^T(A+E)X\gt 0}$, so ${A+E,\,E-A}$ are positive definite**.[^alg-99]
    * A real symmetric matrix ${A}$ of order three has eigenvalues ${\lambda_1,\,\lambda_2,\,\lambda_3}$, and ${A^*}$ is its adjugate. For every three-dimensional column vector ${X}$, ${|X^TA^*X-X^TAX|\leq aX^TX}$. Find the minimum value of ${a}$.
      > Rewrite the inequality as ${X^T(-aE)X\leq X^T(A^*-A)X\leq X^T(aE)X}$. **Thus, ${B_1=A^*-A+aE,\,B_2=-A^*+A+aE}$ are positive definite**. Express these two positive-definite matrices as eigenvalue polynomials. Let the eigenvalues of ${B_1}$ be ${\lambda_j}$ and those of ${B_2}$ be ${\lambda_k}$. Positive definiteness requires ${\begin{cases}\lambda_j-\lambda_i+a\geq0 \\ -\lambda_k+\lambda_i+a\geq0\end{cases}}$. Solve for the minimum ${a}$. **Note that eigenvalue subtraction must pair eigenvalues with the same eigenvector.** If ${\lambda_i=2,\,3,\,4}$, then ${a_{min}=10}$.[^alg-101]
  * If ${Q^TAQ=Λ}$ and ${Q^TQ=E}$, then ${A=\sum_i\lambda_iu_iu_i^T}$. Because the corresponding eigenvectors are mutually orthogonal and form a basis of the space, ${\sum_iu_iu_i^T=E}$, which can simplify calculations.
    * Given ${{A=\begin{pmatrix}0 & 1 & -1 \\ 1 & 0 & -1 \\ -1 & -1 & 0 \end{pmatrix}}}$, a positive-definite matrix ${B}$ satisfies ${B^2=A+2E}$. Find ${B}$.
      > Since ${\lambda_{A}=2,\,-1,\,-1}$, ${\lambda_{A+2E}=4,\,1,\,1}$. Orthogonally transforming ${A+2E}$ gives ${Q^T(A+2E)Q=Λ\Rightarrow A+2E=QΛQ^T}$. Then ${B^2=(QΛQ^T)(QΛQ^T)=QΛ^2Q^T}$, so ${B=QΛQ^T}$, and hence ${\lambda_B=2,\,1,\,1}$. We have ${B=QΛQ^T=2u_1u_1^T+u_2u_2^T+u_3u_3^T}$. **Because ${u_1,\,u_2,\,u_3}$ form an orthogonal basis of the space, ${u_1u_1^T+u_2u_2^T+u_3u_3^T=E}$, so ${B=E+u_1u_1^T}$**.[^alg-104]
  * There is a more general conclusion behind the method above:
  > Let ${A}$ be an order-${n}$ real symmetric matrix, and let ${\alpha_1,\,\alpha_2,\dots,\,\alpha_n}$ be ${A}$'s ${n}$ orthonormal eigenvectors, corresponding to ${\lambda_1,\,\lambda_2\dots}$. Then ${\displaystyle A=\sum_{i=1}^{n}\lambda_i\alpha_i\alpha_i^{T}}$. A direct brute-force calculation proves it; see ${880_{comp}(14-3.13)}$ for details.

<!-- source: algebra:L107-L112 -->

### Sums of squares and linear systems {#sums-of-squares-and-systems}

* Quadratic forms and linear-system problems
  * Given the form ${\displaystyle f(x_1,\,x_2,\,...,\,x_n)=\sum_{i=1}^{m}(a_{i1}x_1+a_{i2}x_2+...+a_{in}x_n)^2}$, denote its coefficient matrix by ${B}$, and let ${A=(a_{ij})_{m\times n}}$.[^alg-108]
  * By the definition of a positive-definite quadratic form, for it to be positive definite, ${f(x_i)=0}$ must have only the zero solution. This becomes the condition that ${B\mathbf{x}=0}$ has only the zero solution. Thus, full column rank, ${r(B)=n}$, is required, naturally implying ${m \geq n}$.
  * With full column rank, if ${m\gt n}$, there are redundant rows. The extra rows are actually proportional to other rows, so in the quadratic form they are combined into the diagonal form.[^alg-110]
  * If ${m\lt n}$, the quadratic form cannot be positive definite in any case, because a fundamental system of solutions always exists. Correspondingly, its diagonal form contains variables with coefficient ${0}$, such as ${y_1^2+0y_2^2+y_3^2}$.

---

<!-- source: algebra:L113-L115 -->

### Ranks of Block Matrices: Which Operations Preserve Equivalence? {#algebra-block-rank}

Pay attention to the incorrect final step. You cannot directly multiply on the left by A, because the multiplier at this point is [E O, O A], with R=r(E)+r(A), and it is not necessarily invertible. It therefore changes the relevant properties and is not allowed. The other steps may use elementary transformations (summarize this part later).

<figure class="fig"><img src="/blog/kaoyan-math/figures/block-matrix-rank.png" alt="Original note figure 3" width="1120" height="754" loading="lazy" decoding="async"><figcaption>Original note figure 3</figcaption></figure>

<!-- Editorial corrections and clarifications; additions to the complete source translation. -->

<!-- source-content:algebra:end -->

[^cal1-6]: **Correction or assumption:** Multiply by $e^x$ and set $u=e^x\gt 0$, giving the quadratic equation $u^2-2yu-1=0$ in one unknown. Its positive root is $u=y+\sqrt{y^2+1}$, so the inverse is $x=\ln(y+\sqrt{y^2+1})$. The original $e^y$ and “system of two linear equations” are incorrect.

[^cal1-8]: **Correction or assumption:** Setting consecutive terms equal to $a$ produces the fixed-point equation $a=f(a)$, not a formula for an arbitrary $n$th term. To use it to find a sequence's limit, first prove convergence and justify passing the limit through the recurrence, for example through continuity.

[^cal1-16]: **Correction or assumption:** Tending to infinity does not mean that individual values actually equal infinity. It means that for every $M\gt 0$, sufficiently close to the limiting point we always have $|f|\gt M$; for sequences, this holds for every sufficiently large index. Unboundedness only requires exceeding arbitrarily large bounds somewhere, not remaining beyond them afterward.

[^cal1-19]: **Correction or assumption:** The condition $x_n\to a\ne0$ does not make $x_n$ constant; eventually it is bounded away from zero and has the sign of $a$. If the product tends to $+\infty$, then $y_n\to+\infty$ for $a\gt 0$, but $y_n\to-\infty$ for $a\lt 0$. If infinity means divergence in absolute value, the valid conclusion is $|y_n|\to\infty$.

[^cal1-20]: **Correction or assumption:** Functions and sequences obey the same product-limit rules. If both factors tend to infinity in absolute value, their product also does; if both tend to $+\infty$, so does the product. An oscillating example that defeats this conclusion necessarily fails one of these hypotheses.

[^cal1-21]: **Correction or assumption:** The product conclusion for sequences is valid under consistent sign/absolute-value conventions, but it follows from the definition of a limit and the multiplication rule, not from discreteness. Functions of a continuous variable satisfy the same conclusion under the same hypotheses.

[^cal1-32]: **Correction or assumption:** The displayed Dirichlet function is nowhere continuous or differentiable, so it cannot illustrate differentiability at one point. A valid example is $F(x)=(x-x_0)^2D(x-x_0)$, where $D$ is the displayed Dirichlet function. Then $F'(x_0)=0$, but $F$ is discontinuous at every other nearby point.

[^cal1-36]: **Correction or assumption:** Continuity is a property of a function at a point, not of the number $g(f(x_0))$. The intended condition is that $g$ is discontinuous at the input $f(x_0)$. The composition can nevertheless be continuous: if $f$ is constant and $g$ is defined at that constant, the composition is constant.

[^cal1-41]: **Correction or assumption:** Taking the lower order requires that leading terms do not cancel. When same-order infinitesimals are added or subtracted, their leading coefficients may cancel, increasing the order or producing zero. For a difference of variable-limit integrals, inspect the leading coefficients associated with both endpoints, not only their orders.

[^cal1-45]: **Correction or assumption:** The expression $\lim_{x\to x_0}=A$ omits the function and should be read as $\lim_{x\to x_0}f(x)=A$. If $f(x_0)$ is unspecified, continuity is undetermined, not necessarily false. The specific piecewise example is discontinuous at zero because $C\ne0$.

[^cal1-47]: **Correction or assumption:** The stated limit gives $f'(x)\to0$, not $A$, and makes $f'(x)$ have the sign of $A$ sufficiently near the point. This ensures monotonicity separately on the left and right component intervals, not necessarily across the entire punctured set, and does not control $f(x_0)$. Additional continuity at the point allows further conclusions.

[^cal1-50]: **Correction or assumption:** Finite one-sided derivatives are difference-quotient limits based on $f(x_0)$: $f'_\pm(x_0)=\lim_{h\to0^\pm}[f(x_0+h)-f(x_0)]/h$. If both exist and are equal, then $f'(x_0)$ exists and $f$ is continuous there. These are different from $\lim_{x\to x_0^\pm}f'(x)$. In the displayed example, $f'(x)=2x$ off zero has both one-sided limits equal to zero, but $f(0)=C\ne0$ makes $f$ discontinuous and prevents equal finite one-sided difference-quotient derivatives.

[^cal1-54]: **Correction or assumption:** The later conclusions in this example use $x_0=0$. From $f(x)/x^2\to A$, we get $f=o(x^2)$ if $A=0$, and $f\sim Ax^2$ if $A\ne0$. The latter has the same order as $x^2$ but is equivalent to $x^2$ itself only when $A=1$. Differentiability and hence continuity at zero give $f(0)=f'(0)=0$.

[^cal1-55]: **Correction or assumption:** L'Hôpital's rule does not generally require continuous derivatives, and there is no universal rule that $n$ derivatives permit only $n-1$ applications. Check the indeterminate form, differentiability on the punctured interval, the nonvanishing denominator derivative, and the derivative-ratio limit. Existence of the original ratio limit does not imply existence of the derivative-ratio limit in reverse. Every repeated application needs its own hypotheses.

[^cal1-56]: **Correction or assumption:** Compare nearby values of $f(x)$ with $f(0)=0$; the fixed number $f(0)$ cannot be “greater than zero throughout a neighborhood.” If $A\gt 0$, zero is a strict local minimum; if $A\lt 0$, it is a strict local maximum. If $A=0$, these conditions alone do not decide the issue.

[^cal1-66]: **Correction or assumption:** If $f$ is differentiable at $x_0$ and $f(x_0)=0$, then $|f|$ is differentiable there exactly when $f'(x_0)=0$. A nonzero first-order slope produces the nondifferentiable corner. For example, $f(x)=x^3$ crosses zero, yet $|x|^3$ is differentiable there.

[^cal1-69]: **Correction or assumption:** Regularity of $g$ is also needed. If both $\phi$ and $g$ are differentiable at $x_0$ and $g(x_0)=0$, then $\phi|g|$ is differentiable exactly when $\phi(x_0)g'(x_0)=0$, and its derivative is zero. The condition $\phi(x_0)=0$ is necessary and sufficient only for a simple zero with $g'(x_0)\ne0$. With $h=x-x_0$, the first-order term is $\phi(x_0)|g'(x_0)|\,|h|$; the difference quotient uses $h$, not $x$ at an arbitrary base point. Removing the first-order corner does not make the entire graph a straight line.

[^cal1-72]: **Correction or assumption:** The finite higher-order quotient limit does give a zero first derivative here, but does not automatically give second or higher derivatives. L'Hôpital's rule cannot be reversed to supply them. The earlier example $x^2+x^3\sin(1/x)$ already has a finite quadratic quotient limit without a second derivative at zero.

[^cal1-73]: **Correction or assumption:** Neither l'Hôpital's rule nor Cauchy's mean value theorem requires a continuous derivative; the issue is that differentiability at one point alone does not supply their interval hypotheses. The displayed difference-quotient factorization applies when $g(x_0)$ is finite and the expressions are meaningful. If $g$ tends to infinity, do not subtract “$g(x_0)=\infty$.” When continuity makes the numerator tend to zero and the denominator diverges in absolute value, the original ratio directly tends to zero.

[^cal1-75]: **Correction or assumption:** For power-order infinitesimals, a finite ratio generally requires the numerator's order to be at least the denominator's, not at most. The actual test is whether the derivative difference quotient is determined along all real approaches. For example, if $f(0)=0$ and $\lim f(x)/x^p=L$ is finite with $p\ge1$, then $p=1$ gives derivative $L$ and $p\gt 1$ gives derivative zero.

[^cal1-80]: **Correction or assumption:** Dividing the original inequality by $x$ reverses the order when $x\lt 0$, and an upper bound alone does not squeeze the limit. Use $|f(x)/x|\le|x|\to0$ instead; the squeeze theorem gives $f'(0)=0$.

[^cal1-84]: **Correction or assumption:** A pointwise Taylor expansion with an $o(x^2)$ remainder is local information; its remainder cannot simply be ignored to bound an integral over a fixed interval. If $f''\ge0$ throughout the symmetric interval, the tangent inequality $f(x)\ge f(0)+f'(0)x$ yields $b\ge2af(0)$. If $f''\le0$, the inequality reverses.

[^cal1-85]: **Correction or assumption:** Chinese textbooks can use differing concave/convex terminology; follow the graph and derivative sign. In standard English terminology, a convex function with $f''\ge0$ lies above its tangents and below its chords. A concave function with $f''\le0$ has the reverse relationships. The source's labels should not be mapped uncritically onto the English definitions.

[^cal1-93]: **Correction or assumption:** The left side is missing differentiation notation. The identity is $\dfrac{d^n}{dx^n}(1/x)=(-1)^n n!/x^{n+1}$; the ordinary power $(1/x)^n$ is not equal to the right side.

[^cal1-101]: **Correction or assumption:** After factoring out $x^2$, the parenthesis is $1-x^{-6}$, not $1-x^{-3}$. The correct expression is $(1-x^6)^{1/3}=-x^2(1-x^{-6})^{1/3}$, and its expansion still gives the original limit zero.

[^cal1-104]: **Correction or assumption:** Even powers depend on $|x|$ relative to 1, not merely on $x\lt 1$, $x=1$, and $x\gt 1$. The displayed expression has limits $1,0,-1$ for $|x|\lt 1$, $|x|=1$, and $|x|\gt 1$, respectively. The following power sequences likewise require separate attention to $x=-1$ and $x\lt -1$.

[^cal1-108]: **Correction or assumption:** The left side should be $dy/dx$: when $dx/dt\ne0$, $dy/dx=(dy/dt)/(dx/dt)$. If $te^y+y+1=0$ implicitly defines $y(t)$, first find $dy/dt=-e^y/(te^y+1)$ where the denominator is nonzero, then divide by $dx/dt$.

[^cal1-111]: **Correction or assumption:** Continuity of both parameter functions guarantees a continuous parametric curve, not automatically a continuous single-valued function $y=f(x)$. Verify that the relevant parameter can be written continuously as $t=t(x)$; for example, a continuous locally strictly monotone $\phi$ has the needed continuous inverse.

[^cal1-118]: **Correction or assumption:** At an interior local maximum, $f''(x_0)\le0$ requires that $f''(x_0)$ exist. A local maximum does not require $f'$ to decrease monotonically throughout a neighborhood. The equation $f'(x_0)=0$ only makes the first-order rate zero, and $f''(x_0)=0$ does not show that the function is constant or decide whether an extremum occurs.

[^cal1-120]: **Correction or assumption:** The definition of a local extremum does not require continuity. For example, $f(0)=1$ and $f(x)=0$ elsewhere gives a discontinuous local maximum at zero. The usual graph-based definition of an inflection point requires appropriate continuity, but that requirement does not apply to every extremum.

[^cal1-121]: **Correction or assumption:** A stationary point is a point where the function is differentiable and its first derivative is zero; a nondifferentiable point is not stationary. Under appropriate continuity conditions, a first-derivative sign change tests for an extremum. An inflection point requires a change of concavity, commonly checked through second-derivative signs on the neighboring sides.

[^cal1-122]: **Correction or assumption:** The condition $f'(x_0)=0$ already defines a stationary point. Adding $f''(x_0)\ne0$ lets the second-derivative test establish a strict extremum. The equation $f''(x_0)=0$ does not by itself establish an inflection point; with the appropriate differentiability assumptions, $f'''(x_0)\ne0$ supplies a common sufficient test.

[^cal1-125]: **Correction or assumption:** Attainment of an extremum at an endpoint requires endpoints in the domain, appropriate continuity, and existence of the extremum; the rule is normally used on a finite closed interval. On an open or infinite interval, endpoint limits may give only a supremum or infimum, without an attained maximum or minimum.

[^cal1-126]: **Correction or assumption:** Differentiability at a point first requires the function to be defined there. An “undefined differentiable point” is not possible. If an extension is requested, assign a value first, then check continuity and differentiability using difference quotients. Piecewise joining points still need separate treatment.

[^cal1-128]: **Correction or assumption:** This holds for polynomials or an appropriately smooth local factorization $f(x)=(x-a)^m h(x)$ with $h(a)\ne0$. In particular, when $m=1$, the derivative is nonzero there: “multiplicity zero” means the point is no longer a root of the derivative.

[^cal1-130]: **Correction or assumption:** An equation containing $o(C)$ is a local expansion as $C\to0$, not a global linear identity for arbitrarily large $C$. Fix $x$, divide by $C$, and take the limit to obtain $A=f'(x)$.

[^cal1-132]: **Correction or assumption:** First obtain $f'(x)=f(x)-1$ from the increment formula, then solve the differential equation to get $f(x)=1+Ce^x$. The differential is $df=(f(x)-1)\,dx$; the $o(\Delta x)$ term belongs to the function increment, not to the differential itself.

[^cal1-138]: **Correction or assumption:** Two derivative rules have been mixed together. The correct identities are $(f^2)'=2ff'$ and $(ff')'=(f')^2+ff''$. An occurrence of $ff'$ suggests the auxiliary function $f^2$, but $(ff')'$ and $(f^2)'$ are not equal in general.

[^cal1-156]: **Correction or assumption:** The remainder point $\xi$ lies between $0$ and $x$: use $(0,x)$ when $x\gt 0$ and $(x,0)$ when $x\lt 0$. This proves the inequality for $x\ne0$; the quotient at zero requires its continuous extension.

[^cal1-160]: **Correction or assumption:** Choose an interior point $c$ with $f(c)\ne0$, not an arbitrary interior point. Such a point exists because the function is nonconstant and zero at both endpoints. Applying the mean value theorem on $[a,c]$ and $[c,b]$ then gives secant slopes with opposite signs.

[^cal1-168]: **Correction or assumption:** The left side should be $m(b-a)$, without an extra $dx$. For continuous $f$, with interval minimum $m$ and maximum $M$, use $m(b-a)\le\int_a^b f(x)\,dx\le M(b-a)$ and then apply the intermediate value theorem.

[^cal1-174]: **Correction or assumption:** The second integral in the auxiliary function should have upper limit $x$. Set $F(x)=\int_a^x tf(t)\,dt-\frac{a+x}{2}\int_a^x f(t)\,dt$. Then $F'(x)=\frac12[(x-a)f(x)-\int_a^x f(t)\,dt]=\frac{x-a}{2}[f(x)-f(\xi)]\ge0$, under the mean value theorem's hypotheses. Use $F(a)=0$ to finish the proof.

[^cal1-177]: **Correction or assumption:** L'Hôpital's rule is unnecessary: divide the preceding equality $\Delta F=f(\xi)\Delta x$ by $\Delta x$, then use continuity as $\xi\to x$. Applying l'Hôpital to $F$ while proving its differentiability risks assuming the very property being established.

[^cal1-186]: **Correction or assumption:** For finitely many discontinuities to guarantee Riemann integrability on a finite closed interval, boundedness is also required. Excluding infinite-limit discontinuities does not automatically exclude unbounded oscillation, such as $\sin(1/x)/x$ with an assigned value at zero. The later condition “bounded with finitely many discontinuities” is the complete standard sufficient condition.

[^cal1-196]: **Correction or assumption:** The inequality $\cos x\gt \sin x$ does not hold throughout $[-\pi/2,\pi/2]$, so the proposed bound is invalid. Substitution and parity give $M=2/3$ and $N=-2$, hence $M\gt N$.

[^cal1-202]: **Correction or assumption:** Use the absolute-value bound $|F(x+\Delta x)-F(x)|\le M|\Delta x|$. The original expression does not give the required two-sided control when $\Delta x\lt 0$. With absolute values, the squeeze argument proves continuity.

[^cal1-204]: **Correction or assumption:** The limit should be $x\to x_0$, not $x\to x$. At a removable discontinuity, the variable-limit integral's derivative equals $\lim_{x\to x_0}f(x)$, which may differ from the assigned value $f(x_0)$.

[^cal1-206]: **Correction or assumption:** The original upper limit is $x^2$, so $u=x^2-t^2$ gives $\frac12\int_{x^2-x^4}^{x^2}f(u)\,du$. Only an original upper limit of $x$ gives $\frac12\int_0^{x^2}f(u)\,du$. If $f$ is continuous, the derivative of the stated original integral is $xf(x^2)-(x-2x^3)f(x^2-x^4)$.

[^cal1-209]: **Correction or assumption:** Having the same convergence or divergence behavior does not allow equivalent substitution of integrands or guarantee the same asymptotic coefficient. Check actual local equivalence, a fixed sign, or a controlled error. Termwise integration of a Taylor expansion also requires appropriate control of the remainder over the integration interval.

[^cal1-218]: **Correction or assumption:** The correct identity is $(1+\tan x)^2=\sec^2x+2\tan x$; the source omits the coefficient 2. Match the coefficients as well as the derivative relationship when using integration by parts.

[^cal1-228]: **Correction or assumption:** The second piece retains the interval $[\pi/4,\pi/2]$: $I_2=\int_{\pi/4}^{\pi/2}(\pi/2-\theta)\,d(\sin^4\theta)$. Moving it to $[0,\pi/4]$ requires transforming the integrand as well, not just changing the limits. Combining both pieces gives the original integral $1/2$.

[^cal1-237]: **Correction or assumption:** The common criterion for the substitution $u=\tan x$ in a trigonometric rational function is invariance under simultaneous sign reversal: $R(-\sin x,-\cos x)=R(\sin x,\cos x)$, not the negative of the original value. The tangent-half-angle substitution remains available in the general case.

[^cal1-239]: **Correction or assumption:** The product-to-sum identity is missing $1/2$: $\sin x\cos(nx)=\frac12[\sin((1+n)x)+\sin((1-n)x)]$. The right-hand integral must retain this factor.

[^cal1-241]: **Correction or assumption:** For real-valued integration use $\ln|c\sin x+d\cos x|$ unless the argument is known to be positive. On an interval where the denominator is nonzero, the decomposition gives $Ax+B\ln|c\sin x+d\cos x|+C$.

[^cal1-243]: **Correction or assumption:** The repeated identity here is also $(1+\tan x)^2=\sec^2x+2\tan x$; the factor 2 must not be omitted.

[^cal1-249]: **Correction or assumption:** The second Beta integral is missing $du$: it should be $\int_0^1u^{p-1}(1-u)^{q-1}\,du$, with the stated conditions $p,q\gt 0$.

[^cal1-252]: **Correction or assumption:** The integrand here is unchanged when both $\sin x$ and $\cos x$ change sign, so the relation is $R(-\sin x,-\cos x)=R(\sin x,\cos x)$. The $\tan x$ substitution and the subsequent result $B(3,3)=1/30$ are correct.

[^cal1-253]: **Correction or assumption:** The equality follows from the reciprocal substitution $x=1/t$ and its Jacobian factor, not ordinary mirror symmetry about the line $x=1$. The differential $dx$ must also be transformed to obtain the equality of integrals.

[^cal1-255]: **Correction or assumption:** With $u=1/x-x$ and $v=1/x+x$, the second denominator is $v^2-3$, not $v^2+3$, and the logarithmic coefficient is $-1/(4\sqrt3)$. For $x\gt 0$, the correct antiderivative is $-\frac12\arctan(1/x-x)-\frac1{4\sqrt3}\ln\left|\frac{1/x+x-\sqrt3}{1/x+x+\sqrt3}\right|-\frac13\arctan(x^3)+C$. Taking endpoint limits gives the original definite integral $\pi/3$.

[^cal1-257]: **Correction or assumption:** The original integral $\int_0^1x^t/\ln x\,dx$ diverges at $x=1$, so parameter differentiation cannot be applied to it directly. Also, $\int_0^1x^t\,dx=1/(t+1)$ for $t\gt -1$, not $t+1$. The convergent difference form $J(t)=\int_0^1(x^t-1)/\ln x\,dx$ has $J'(t)=1/(t+1)$ and $J(0)=0$, giving $J(t)=\ln(1+t)$. Thus the following difference-of-powers example remains valid and equals $\ln2$.

[^cal1-262]: **Correction or assumption:** The region is the full disk, so the angular range is $0$ to $2\pi$, not $\pi/2$. Let $D_r$ be the radius-$r$ disk and $L_r$ its counterclockwise boundary. The correct line integral is $\oint_{L_r}(-f_y\,dx+f_x\,dy)$, and its corresponding area-integral domain must vary with $r$ as $D_r$. Thus $I=\int_0^1 r\,dr\iint_{D_r}(f_{xx}+f_{yy})\,dA=\pi\int_0^1r(1-e^{-r^2})\,dr=\pi/(2e)$. The original final value is correct, but the intermediate angular range, differentials, and domain notation are not.

[^cal2-266]: **Correction or assumption:** Provided the function is defined at the point, continuity is equivalent to the limit existing and equaling the function value. The converse does hold. Existence of the limit alone is insufficient.

[^cal2-268]: **Correction or assumption:** Partial derivatives along different coordinate directions need not be equal, nor must directional derivatives in different directions agree. Existence of a gradient as a vector of partial derivatives is weaker than differentiability. A standard sufficient condition is that the partial derivatives exist nearby and are continuous at the point.

[^cal2-269]: **Correction or assumption:** Along y=x², f(x,x²)=1/2, while the function is zero along the coordinate axes. It is therefore discontinuous and not differentiable at the origin, although both partial derivatives there equal zero. A valid example of differentiability with discontinuous partials is g(x,y)=x²sin(1/x) for x≠0, extended by zero when x=0.

[^cal2-274]: **Correction or assumption:** The implicit function theorem also requires assumptions such as suitable continuous partial derivatives nearby and F(x₀,y₀)=0. F_y=0 does not rule out an implicit function, but an indeterminate derivative quotient or existence of some quotient limit does not by itself prove existence.

[^cal2-275]: **Correction or assumption:** When x denotes the increment in the displayed quotient, it must tend to 0, not x₀. More clearly, f_x(x₀,y)=lim_{h→0}[f(x₀+h,y)−f(x₀,y)]/h; then differentiate with respect to y.

[^cal2-284]: **Correction or assumption:** The differentiability remainder uses the increment distance ρ=√((Δx)²+(Δy)²): Δz=AΔx+BΔy+o(ρ), as ρ→0. Little-o notation describes a class of remainders. If used as a denominator, it should denote a specified nonzero function q=o(ρ), not an unspecified fixed quantity.

[^cal2-285]: **Correction or assumption:** Because the base point is (a,b), use √((x−a)²+(y−b)²), not √(x²+y²). If the numerator divided by a specified q=o(this distance) has finite limit C, dividing it by the distance gives limit zero. This establishes differentiability with f_x(a,b)=−3 and f_y(a,b)=4.

[^cal2-290]: **Correction or assumption:** A nondifferentiable point is not a stationary point in the usual sense; stationarity requires existing first partial derivatives that vanish. Stationary and nondifferentiable points are only candidates and require further classification.

[^cal2-293]: **Correction or assumption:** Treat M_i, M_j, and M_k as candidates, not automatically established extrema. A global extremum is local relative to the feasible domain, but a boundary extremum need not be an unconstrained extremum in an open neighborhood. Constraint singularities and whether extrema are attained also require checking.

[^cal2-299]: **Correction or assumption:** A nonzero solution and zero coefficient determinant are justified only when the constraint excludes (0,0) and the candidate satisfies the homogeneous linear system. The mere presence of a constraint does not guarantee a nonzero solution.

[^cal2-303]: **Correction or assumption:** The elliptical section is centered at the origin, but its boundary does not pass through the origin; the origin lies inside the enclosed section. Maximum and minimum distances from the center do give the semimajor and semiminor axes.

[^cal2-304]: **Correction or assumption:** The objective is d² and there are two constraints: L=x²+y²+z²+λ(x²/3+y²/2+z²−1)+μ(x+y+z). This Lagrangian yields the subsequent system and d²=−λ. The semiaxes are √((11+√13)/6) and √((11−√13)/6).

[^cal2-307]: **Correction or assumption:** The endpoint B must maximize x+2y, not minimize it. Taking its minimum at the starting point and maximum at the endpoint gives the stated maximum integral, 4√2.

[^cal2-308]: **Correction or assumption:** Existence of second partial derivatives does not guarantee global extrema. A standard sufficient condition is continuity on a nonempty compact set, such as a closed bounded region. A smooth function on an open or unbounded region may fail to attain a maximum or minimum.

[^cal2-312]: **Correction or assumption:** The boundary conclusion assumes that global extrema exist and are attained. Otherwise there may be only a supremum or infimum, or the function may be unbounded; existence of boundary extrema does not follow automatically.

[^cal2-315]: **Correction or assumption:** The function is (x−3)²+y²+1, with unique stationary point (3,0) and minimum value 1. The point (2,0) is not stationary, the third derivative is zero, and the Hessian determinant is 4, so this is not a degenerate-Hessian example. Higher-derivative tests also require conditions such as existence of a first nonzero derivative.

[^cal2-317]: **Correction or assumption:** Checking only straight-line paths is not generally sufficient to prove a multivariable extremum. Here, x⁴+y⁴≥0 with equality only at the origin directly proves a strict global minimum.

[^cal2-322]: **Correction or assumption:** The given limit implies f(x,y)→0 but does not determine the separately assigned value f(0,0). If that value is nonzero, the coordinate difference quotients do not converge. If it is zero, they contain |h|/h and have different one-sided limits. Both partials still fail to exist, but f(0,0) cannot be silently discarded.

[^cal2-330]: **Correction or assumption:** The second 2u₁₁″ term should be 2u₂₁″. With sufficient assumptions for the chain rule and interchange of mixed partials, such as C² regularity nearby, the relation is u₁₁″+4u₁₂″+4u₂₂″=0. Combined with the other equations, this yields −4x/3. Existence of second partials alone does not automatically justify all these operations.

[^cal2-338]: **Correction or assumption:** Shifted polar coordinates still have area element dxdy=r dr dθ. Both radial and angular bounds must come from the full integration region. The bounds 0≤r≤√(a²+b²), 0≤θ≤2π directly apply to the complete disk.

[^cal2-340]: **Correction or assumption:** Use a dummy integration variable: F(x)=∫ₓᵇf(t)dt and F′(x)=−f(x). Under appropriate integrability assumptions, the displayed double integral equals (1/2)(∫ₐᵇf(t)dt)².

[^cal2-344]: **Correction or assumption:** Interchanging dependent and independent variables and taking reciprocal derivatives requires local invertibility and a nonzero relevant derivative. Writing the inverse as x(y) is clearer. Check constant or exceptional solutions that division may discard.

[^cal2-346]: **Correction or assumption:** Multiplying by eʸ gives (eʸ)′+eʸ on the left, not just (eʸ)′. Setting u=eʸ yields u′+u=sin x, a first-order linear equation. Recovering y=ln u also requires u&gt;0.

[^cal2-355]: **Correction or assumption:** This applies to particular solutions of the same nonhomogeneous linear equation. Their difference is one homogeneous solution, not automatically the general solution. Enough linearly independent differences are needed for a basis. The references to 'nonlinear' subtraction or general solutions should be understood as linear combinations or independent homogeneous solutions.

[^cal2-356]: **Correction or assumption:** For the three-particular-solution construction in a second-order nonhomogeneous linear equation, the two differences must be linearly independent. The affine coefficients satisfy C₁+C₂+C₃=1 and need not be pairwise different; the third coefficient is not independently arbitrary. Three particular solutions do not establish this form for equations of arbitrary order.

[^cal2-366]: **Correction or assumption:** Terms tending to zero is necessary for a convergent series, not a property of every series and not a sufficient convergence test. Use an assumed convergence condition to identify parameter candidates, then verify them.

[^cal2-367]: **Correction or assumption:** If the intended series is Σ(uₙ+vₙ), convergence directly gives uₙ+vₙ→0, not separate zero limits for uₙ and vₙ. Before adding or subtracting two infinite sums, establish that they are defined; do not formally cancel divergent quantities.

[^cal2-370]: **Correction or assumption:** A product tending to zero does not imply its quotient tends to zero; take uₙ=vₙ=1/n. Equivalent-term comparison usually concerns eventually positive terms. Also distinguish convergence of a sequence from convergence of its series, and state the required comparison conditions.

[^cal2-372]: **Correction or assumption:** Grouping finitely many consecutive terms of a convergent series preserves convergence, but the converse fails, as with 1−1+1−1+⋯. Specify whether terms are being grouped or an adjacent-sum series is being formed. Conditional convergence does not permit arbitrary rearrangement.

[^cal2-376]: **Correction or assumption:** Divergent parts of a Taylor decomposition can cancel each other. A divergence conclusion is justified, for example, when one part diverges and all remaining parts have been proved convergent. Introducing (−1)ⁿ must also preserve the original term identically; signs cannot be changed arbitrarily.

[^cal2-377]: **Correction or assumption:** For signed series, asymptotic equivalence of terms alone does not guarantee the same convergence behavior. If Σ(uₙ−vₙ) is established to converge, partial-sum relations transfer ordinary convergence or divergence. Classifying conditional convergence additionally requires checking the absolute-value series.

[^cal2-382]: **Correction or assumption:** Using ln a requires a&gt;0. The leading term is (ln a−1/2)/n, which cancels only when a=√e. For that value, the next term is 1/(4n²), giving convergence; all other a&gt;0 give divergence.

[^cal2-387]: **Correction or assumption:** Because the sum starts at n=1, S₁=Σxⁿ=x/(1−x), not 1/(1−x). Correct integration gives G=1/(1−x)²−5/(1−x)−4ln(1−x)−x+C, and G(0)=0 gives C=4. The following step integrates rather than differentiates. S=G/x extends continuously to S(0)=0, with |x|&lt;1.

[^cal2-393]: **Correction or assumption:** Reindexing the derivative must retain a₁=1: S′=1+Σₙ₌₁∞(n+1)aₙ₊₁xⁿ. The correct equation is S′=1+xS′+S/2, not S=1+xS′+S/2. With S(0)=0, this gives S=2[(1−x)^(−1/2)−1] for |x|&lt;1.

[^cal2-395]: **Correction or assumption:** A pointwise infinite sum of continuous functions need not be continuous. Power series converge locally uniformly inside their convergence radius and permit termwise differentiation and integration there. Check endpoints separately; use an interior limit for an endpoint sum only under conditions such as those of Abel's theorem.

[^cal2-398]: **Correction or assumption:** Retain the factor: 1/(x−2)=−(1/2)/(1−x/2). The corresponding geometric expansion holds for |x|&lt;2.

[^cal2-400]: **Correction or assumption:** The partial-fraction factor is 1/2: 1/(n²−1)=(1/2)[1/(n−1)−1/(n+1)], not 2. The second contribution retains its minus sign. The correct value is S(1/2)=5/8−(3/4)ln 2.

[^cal2-401]: **Correction or assumption:** For F(x)=Σaₙxⁿ, the odd-power part is [F(x)−F(−x)]/2 and the even-power part is [F(x)+F(−x)]/2. Simply replacing uₙ by −uₙ does not select odd terms, and summation limits must be consistent.

[^cal2-402]: **Correction or assumption:** Changing the original lower limit from n=1 to n=0 adds a constant term 2, which must be subtracted. The sum is (√2/x)ln[(1+x/√2)/(1−x/√2)]−2 for |x|&lt;√2, with value 0 at x=0 by continuity.

[^cal2-404]: **Correction or assumption:** This example contains odd powers and therefore omits even powers, despite the preceding label. For |x|&lt;1, differentiation gives −1/(1+x²); with S(0)=0, its sum is −arctan x. It also converges conditionally at x=±1.

[^cal2-410]: **Correction or assumption:** The original 'Guess' should be 'Gauss'.

[^cal2-415]: **Correction or assumption:** This formula requires parallel planes with consistently scaled A, B, C coefficients. Intersecting planes have distance zero. There are two parallel planes at distance d only when d&gt;0; for d=0, there is only the original plane.

[^cal2-416]: **Correction or assumption:** The three points must be noncollinear; otherwise the cross product vanishes and they do not determine a unique plane.

[^cal2-420]: **Correction or assumption:** Only nonzero direction components may appear as denominators in the symmetric equation. A zero component fixes the corresponding coordinate; a parametric equation handles all cases.

[^cal2-422]: **Correction or assumption:** Two skew lines do not determine a single plane. For nonparallel directions, τ₁×τ₂ gives the common-perpendicular direction. Construct a separate auxiliary plane through each given line and that direction; their intersection is the desired common perpendicular. Parallel cases require separate treatment.

[^cal2-423]: **Correction or assumption:** For orthogonal projection, the auxiliary plane must contain the original line and the target plane's normal direction. If the line is perpendicular to the target plane, its projection is a point, not a line obtained from two planes.

[^cal2-424]: **Correction or assumption:** This auxiliary-plane method applies when the two line directions are nonparallel. For parallel lines, a plane through L₂ parallel to L₁ is not unique, and an arbitrary point-to-plane distance need not equal the line-to-line distance. Use a parallel-line distance formula or common perpendicular instead.

[^cal2-426]: **Correction or assumption:** A scalar line integral is orientation independent. With endpoints A and B, use r(t)=A+t(B−A), 0≤t≤1, so ds=‖B−A‖dt. Direction bookkeeping prevents mismatched parameters and bounds; reversing orientation does not change this integral's sign.

[^cal2-427]: **Correction or assumption:** A surface normal perpendicular to the projection direction generally identifies candidate silhouette points of a smooth surface, not the entire projected region. Determine the region by requiring an eliminated coordinate to exist satisfying the original conditions. Surface boundaries and singular points may also contribute projected boundaries.

[^cal2-428]: **Correction or assumption:** The vector (2x,2y−z,2z−y) is the surface gradient/normal, not a tangent vector. Completing the square gives x²+3y²/4+(z−y/2)²=1. Thus the projected region is x²+3y²/4≤1, with boundary x²+3y²/4=1, z=0.

[^cal2-432]: **Correction or assumption:** In three dimensions, x=1 alone denotes a plane and does not specify a rotation axis. State the coordinate translation explicitly; for an axis x=a, y=b, use x′=x−a and y′=y−b, then substitute back.

[^cal2-435]: **Correction or assumption:** The fixed-angle construction using a point's position vector applies when the generating line intersects the axis and vectors start at their intersection. A skew generating line revolving about the axis cannot directly be treated with this cone formula.

[^cal2-442]: **Correction or assumption:** The earlier λ&gt;0 contradicts the later λ≤0. Under the stated forward-ray convention, the conditions are x&gt;0, |y|≤x, and xz=y². The endpoints |u|=1 require a non-strict inequality. If including the vertex, add (0,0,0) separately; merely replacing x&gt;0 by x≥0 introduces unwanted z-axis points. The parameters to eliminate are λ and u, not an undefined μ.

[^cal2-443]: **Correction or assumption:** The equations x′=x−a and so on are coordinate transformations; the transformed vertex is (0,0,0). In original coordinates, the parametrization is (a,b,c)+λ[(1,u,u²)−(a,b,c)], with |u|≤1. Rays toward the directrix use λ≥0, complete generating lines use λ∈R, and λ≤0 selects the opposite rays. The intended cone convention must be specified.

[^alg-3]: **Correction or assumption:** Correction: General matrix equivalence allows all three elementary row/column operations: interchange, multiplication by a nonzero scalar, and adding a multiple of another row/column. Interchanges alone are insufficient. A direct proof of invertibility is (P1*P2)^(-1)=P2^(-1)*P1^(-1); assuming the product's inverse exists does not prove its existence.

[^alg-7]: **Correction or assumption:** Clarification: The r independent columns of B do give r independent solutions of Ax=0. They can be called eigenvectors for eigenvalue zero only when A is square; the opening statement allows a general m-by-n matrix.

[^alg-9]: **Correction or assumption:** Correction: The homogeneous solution-space dimension is the nullity d=n-r(A), not the rank r(A). If Ax=b is consistent and b is nonzero, its solution set contains at most d+1 linearly independent vectors; if b=0, the maximum is d.

[^alg-10]: **Correction or assumption:** Correction: The general solution in this example is k1*xi1+k2*xi2+eta, with addition between the homogeneous terms. The three independent solutions eta, eta+xi1, eta+xi2 require nullity 2 and nonzero b, not merely r(A)=2.

[^alg-12]: **Correction or assumption:** Clarification: If AX=B has a solution X, the column space of B lies in that of A, so A^T x=0 and the system formed by stacking A^T above B^T do have the same solutions. If “same solutions” means only Ax=0 and Bx=0, it establishes equality of row spaces and does not generally imply the corresponding claim for transposes.

[^alg-15]: **Correction or assumption:** Correction: Consistency of AX=B does not require r(A)=r(B). Use r([A,B])=r(A) and invariance of rank under transposition to obtain r([A^T;B^T])=r(A^T). The products B^T A^T and A^T B^T are not the required block concatenations and may not even be dimensionally defined.

[^alg-17]: **Correction or assumption:** Correction: For a real matrix, Ax=0 and A^T A x=0 have the same solutions, while A^T x=0 and A A^T x=0 have the same solutions. The transpose is misplaced in the original statement.

[^alg-22]: **Correction or assumption:** Correction: Orthogonality uses the inner product xi^T*xi3=0, not a zero cross product. In this three-dimensional example, k and 1 must be distinct for the stated eigenspace argument: when k is not 1, the eigenspace for 1 is x1+x2+x3=0. Its first-column pivot follows from that constraint, not from whether A has nonzero diagonal entries.

[^alg-29]: **Correction or assumption:** Clarification: r(alpha*beta^T)=1 requires both vectors to be nonzero. If either is zero, the matrix has rank zero. The later conclusions about nonzero rank-one matrices use this assumption.

[^alg-30]: **Correction or assumption:** Correction: A rank-one matrix A of order n&gt;1 is singular, so |A^(-1)| is undefined. The determinant |A+kE| can be calculated; using its inverse additionally requires its eigenvalues to be nonzero.

[^alg-32]: **Correction or assumption:** Correction: Only when tr(A) is nonzero is it a simple eigenvalue with zero of multiplicity n-1. If tr(A)=0, they coincide and zero has algebraic multiplicity n.

[^alg-38]: **Correction or assumption:** Correction: If xi is a unit column eigenvector for the nonzero eigenvalue lambda, then A=lambda*xi*xi^T. The original xi^T*lambda*xi is a scalar, not matrix A.

[^alg-39]: **Correction or assumption:** Correction: The operations A*alpha and A*beta place the column vectors on the right of A; they are right multiplication by alpha and beta.

[^alg-43]: **Correction or assumption:** Clarification: alpha=beta is incompatible with the preceding assumption that two nonzero unit vectors are orthogonal, so it must be a separate case. For A=alpha*beta^T it gives A=alpha*alpha^T; for the preceding sum A=alpha*beta^T+beta*alpha^T, it gives 2*alpha*alpha^T.

[^alg-44]: **Correction or assumption:** Correction: r(A)=n-1 guarantees a one-dimensional eigenspace for zero, not algebraic multiplicity one or diagonalizability. Every column of A lies in the nullspace of A^*, and n-1 independent columns can be chosen as a basis. Identifying that space with the span of eigenvectors for nonzero eigenvalues requires additional conditions such as diagonalizability.

[^alg-46]: **Correction or assumption:** Clarification: This and the next example use determinant bars for A but subsequently treat A as a matrix in A+E and eigenvalue calculations. Distinguish the array as matrix A and |A| as its determinant. The first example's determinant remains -29.

[^alg-50]: **Correction or assumption:** Correction: From A=(a-1)E+B, A's eigenvalues are a-1+lambda_i, not a+1+lambda_i. They are a+n-1 once and a-1 with multiplicity n-1, with the same eigenvectors as B.

[^alg-53]: **Correction or assumption:** Clarification: xi1 through xin are basis vectors of a vector space, not a “basis space.” The numbers a1 through an are coordinates relative to that ordered basis, and the two later bases may belong to the same vector space.

[^alg-57]: **Correction or assumption:** Clarification: The intended relation is sum_i xi*alpha_i=sum_i xi*beta_i, or sum_i xi*(alpha_i-beta_i)=0. Unless an implicit-summation convention is explicitly adopted, it must not be read as a separate equation xi*(alpha_i-beta_i)=0 for every i.

[^alg-58]: **Correction or assumption:** Correction: Fix a column-basis convention first: for old and new basis matrices U and V with V=U*C, C=U^(-1)*V. If U=P*A and V=P*B, then C=A^(-1)*B. If U=A*P and V=B*P, then C=P^(-1)*A^(-1)*B*P. The original mixes multiplication order and transpose conventions; an extra transpose is not universally valid.

[^alg-65]: **Correction or assumption:** Clarification: If the new basis satisfies V=U*P, the same vector's column coordinates satisfy x=P*y and its row coordinates satisfy x^T=y^T*P^T. The reverse coordinate change uses P^(-1). The distinction between P and P^T comes from column versus row conventions, not an unconditional claim that only one is the “true” transition matrix.

[^alg-71]: **Correction or assumption:** Correction: Ordinary matrix equivalence means equal rank and does not imply equivalent row spaces. For example, [1,0] and [0,1] have equal rank but different nullspaces. With the same number of unknowns, equivalent row spaces are equivalent to having the same homogeneous solution set.

[^alg-72]: **Correction or assumption:** Clarification: The proof also needs the reverse inclusion. Full column rank of Q means Q*y=0 has only the zero solution, so Q*P*xi=0 implies P*xi=0. Together with the immediate forward inclusion, this proves equality of the solution sets.

[^alg-80]: **Correction or assumption:** Correction: An invertible nonorthogonal transformation can also be constructed through elementary congruence operations or orthogonal diagonalization followed by rescaling, not only by completing the square. Nonorthogonality does not force nonsimilarity: applying diag(1,2) by congruence to A=diag(1,0) leaves A unchanged.

[^alg-81]: **Correction or assumption:** Correction: Solutions of x^T*f(A)*x=0 cannot generally be counted by zero eigenvalues. An indefinite matrix can have nonzero solutions without any zero eigenvalue, as diag(1,-1) does at (1,1). For a positive- or negative-semidefinite f(A), the zero set equals its nullspace; the zero-eigenvalue multiplicity gives its dimension, and solutions include linear combinations of the corresponding eigenvectors.

[^alg-83]: **Correction or assumption:** Correction: The decomposition consistent with x=Q*y is A=Q*Lambda*Q^T. The Rayleigh quotient's extremes are the smallest and largest individual eigenvalues, not extrema of their sum. One may set a coordinate for an extreme eigenvalue to 1 and the others to 0, then recover x. More generally, any nonzero vector in the corresponding extreme eigenspace attains it.

[^alg-88]: **Correction or assumption:** Clarification: The valid operation here is symmetrization: for real x, x^T*M*x=x^T*((M+M^T)/2)*x. The symmetric coefficient matrix of the quadratic form is therefore the stated rank-one A, to which orthogonal diagonalization of real symmetric matrices applies.

[^alg-90]: **Correction or assumption:** Clarification: An invertible change that turns the denominator into y^T*y requires B to be positive definite. First choose S with S^T*B*S=E and set x=S*y; S is generally not orthogonal. Then orthogonally diagonalize S^T*A*S using y=U*z, giving the final relation x=S*U*z.

[^alg-96]: **Correction or assumption:** Correction: The final summary swaps f and g. The denominator g is put into the canonical sum of unit squares, while the numerator f is put into diagonal form.

[^alg-98]: **Correction or assumption:** Clarification: If the coefficient matrix of the linear forms in a sum of squares is an m-by-n matrix A, the quadratic-form matrix is A^T*A. Positive definiteness is equivalent to full column rank of A; it is equivalent to invertibility of A only when m=n.

[^alg-99]: **Correction or assumption:** Clarification: Inferring positive definiteness of A+E and E-A requires the inequality to hold for every nonzero column vector X. Its validity for one specified X is insufficient.

[^alg-101]: **Correction or assumption:** Correction: The non-strict inequalities require B1 and B2 to be positive semidefinite, not positive definite. Let mu_i be the adjugate eigenvalue paired with lambda_i through the same eigenvector; in this three-dimensional case it is the product of the other two eigenvalues. Then a_min=max_i |mu_i-lambda_i|. For 2, 3, 4, the differences are 10, 5, 2, giving a_min=10. The original notation that subtracts lambda again from eigenvalues already assigned to B1/B2 must be distinguished accordingly.

[^alg-104]: **Correction or assumption:** Correction: The derivation reuses Lambda for different diagonal matrices. If A+2E=Q*diag(4,1,1)*Q^T, its positive-definite square root is B=Q*diag(2,1,1)*Q^T. The final formula B=E+u1*u1^T is correct, with u1 a unit eigenvector of A for eigenvalue 2.

[^alg-108]: **Correction or assumption:** Clarification: Under this paragraph's definitions, A is the m-by-n coefficient matrix of the linear forms and B is the n-by-n quadratic-form matrix, with B=A^T*A. Hence ker(B)=ker(A) and r(B)=r(A), connecting the next line's r(B)=n to m&gt;=n.

[^alg-110]: **Correction or assumption:** Correction: A redundant row is generally a linear combination of other rows, not necessarily proportional to one row. For example, among (1,0), (0,1), and (1,1), the third is the sum of the first two but proportional to neither. Reduction to diagonal form proceeds through congruence transformations of the quadratic form.
