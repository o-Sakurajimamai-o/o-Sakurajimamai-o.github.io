---
title: "Graduate Entrance Exam Math Notes: Calculus and Linear Algebra"
date: 2026-09-09
description: "My calculus and linear algebra revision notes: concepts, proof strategies, calculation techniques, worked examples, and common pitfalls."
translationKey: kaoyan-math
draft: false
---

This article combines my calculus and linear algebra notes from preparation for China’s postgraduate entrance exam. It keeps the examples, derivations, exercise references, and original handwritten figures, with topics organized into sections and subsections. Clear transcription errors and missing assumptions are noted where they occur. These are observations accumulated while solving problems, intended to be revisited alongside the textbooks and original exercises.

> A new problem often reminds me of an earlier one. When solving it exposes a gap in my understanding of a method or concept, I use the new problem to revise that earlier understanding as well.

For the computer science subjects, see my [408 exam notes](../kaoyan-408/).

<!-- source: calculus:L1-L3 -->
## Calculus: Limits, Derivatives, and Differentials {#limits-and-derivatives}

### Algebraic Transformations, Recurrences, and Product Derivatives {#algebra-and-product-derivatives}

<!-- source: calculus:L4-L14 -->
When finding an inverse function, first look for a substitution that turns exponentials or radicals into an algebraic equation. For the hyperbolic sine function

$$
y=\frac{e^x-e^{-x}}2,
$$

set $u=e^x\gt 0$. This gives $u^2-2yu-1=0$, hence $x=\ln(y+\sqrt{y^2+1})$. The original note's instruction to “multiply by $e^y$ and solve two simultaneous linear equations” should instead be to multiply by $e^x$ and solve a quadratic equation in $e^x$.

For a recurrence $x_n=f(x_{n-1})$, use the fixed-point equation $a=f(a)$ flexibly to guide algebraic transformations. However, $x_n=x_{n-1}=a$ describes only a constant solution, or can be used to find the limit after convergence has been proved; it is not a general formula for the sequence. Exercise: $660_{18}$.

If $f(x)=u(x)v(x)$, multiply the two Taylor expansions, paying attention to the truncation order and products involving remainder terms. When differentiating several factors or finding higher derivatives, first try grouping the expression into two factors:

$$
f'(x)=u(x)v'(x)+v(x)u'(x),\qquad
(uv)^{(n)}=\sum_{k=0}^{n}\binom nk u^{(k)}v^{(n-k)}.
$$

For a proof of the product rule, see p. 167 of the original textbook. Leibniz's formula and Taylor expansions offer complementary approaches.

<!-- source: calculus:L23-L29 -->
For example, to differentiate

$$
f(x)=\prod_{n=1}^{100}\left(\tan\frac{\pi x^n}{4}-n\right)
$$

at $x=1$, first notice that the factor with $n=1$ vanishes and separate it from the product:

$$
f(x)=\left(\tan\frac{\pi x}{4}-1\right)
\prod_{n=2}^{100}\left(\tan\frac{\pi x^n}{4}-n\right).
$$

Only the nonzero term in $u'v+uv'$ then needs to be evaluated. As another example, rewrite $y=(1-x)/(1+x)$ as $-1+2/(1+x)$ to obtain $y^{(n)}(0)=2(-1)^n n!$ directly, for $n\ge1$.

### Infinity, Equivalent Infinitesimals, and Power Limits {#infinity-and-equivalents}

<!-- source: calculus:L15-L22 -->
**Being unbounded is different from tending to infinity.** Tending to infinity in magnitude requires the absolute value to remain above every prescribed bound once the variable is sufficiently close to the limit point. The function $x^{-2}\sin(1/x)$ is unbounded as $x\to0$, but does not satisfy this condition.

- Even if $x_ny_n\to+\infty$, it does not follow that at least one sequence tends to infinity. Let $x_n$ equal $1$ for odd indices and $n$ for even indices, with $y_n$ taking the opposite choices. Their product is always $n$, but all that can be guaranteed is that at least one factor is unbounded.
- If $x_n\to a\ne0$, the limit of the product can determine the infinite-limit behavior of $y_n$; its sign also depends on $a$.
- **Correction: Functions and sequences obey the same rules for limits of products.** If both factors tend to infinity in absolute value, so does the absolute value of their product. If both tend to $+\infty$, the product also tends to $+\infty$. The original note's attempt to distinguish these conclusions using “continuous paths” versus “discrete sequences” is invalid.

<!-- source: calculus:L38-L43 -->
When working with equivalent infinitesimals, consider both the order and the leading coefficient:

- For $\int_0^{\phi(x)}f(t)\,dt$, if $\phi(x)\sim x^n$ and $f(t)\sim t^m$ on the relevant side, with $m\gt -1$, the integral typically has order $n(m+1)$.
- Split $\int_{g(x)}^{h(x)}f(t)\,dt$ into two integrals starting at zero. If their orders differ, the lower-order term dominates; **if the orders agree, check for cancellation**. The same restriction applies when adding infinitesimals.
- For fixed $\alpha$,

$$
[1+Ax^p+o(x^p)]^\alpha
=1+\alpha Ax^p+o(x^p).
$$

For example, when $\alpha\ne0$, we have $1-\cos^\alpha x\sim(\alpha/2)x^2$.

<!-- source: calculus:L94-L105 -->
For an indeterminate form $1^\infty$, check that $a(x)\to0$, $b(x)\to\infty$, $a(x)b(x)\to A$, and the base is eventually positive. Then

$$
[1+a(x)]^{b(x)}\to e^A.
$$

The $1$ in this expression is the key to recognizing the transformation. If an exponential has base $a\gt 0$, do not omit $\ln a$ from its expansion. For example,

$$
x^p\left(a^{1/x}-a^{1/(x+1)}\right)
=x^p a^{1/(x+1)}
\left(a^{1/[x(x+1)]}-1\right).
$$

As $x\to+\infty$, for $a\ne1$ this is equivalent to $(\ln a)x^{p-2}$; then classify the result according to $p$. If $a=1$, the original expression is identically zero.

For an $\infty-\infty$ form, try combining fractions, rationalizing, or factoring out the dominant term, then use a Taylor expansion to cancel leading terms. For example, as $x\to+\infty$,

$$
\sqrt[3]{1-x^6}+x^2
=-x^2\left(1-\frac1{x^6}\right)^{1/3}+x^2
\longrightarrow0.
$$

The term inside the parentheses should involve $x^{-6}$; the original $x^{-3}$ was a typo.

For limits involving radicals, absolute values, or a sequence with a parameter, first determine the signs and parameter ranges. For example,

$$
\lim_{n\to\infty}\frac{1-x^{2n}}{1+x^{2n}}
= \begin{cases}
1,&|x|\lt 1,\\
0,&|x|=1,\\
-1,&|x|\gt 1.
\end{cases}
$$

The expressions $x^n$, $x^{2n}$, and $x^{2n+1}$ behave differently at $x=1,-1$ and in the ranges $|x|\lt 1$, $x\gt 1$, and $x\lt -1$. In particular, do not omit the odd-even oscillation at $x=-1$ or, when $x\lt -1$, the alternating signs of $x^n$. Exercise: $660_{19}$.

<!-- source: calculus:L113-L116 -->
To compare two positive radicals, raise both to a common positive integer power. For example, compare $\sqrt2$ and $\sqrt[3]3$ by taking sixth powers: $8\lt 9$, so $\sqrt2\lt \sqrt[3]3$.

### Information at a Point and Properties of a Neighborhood {#pointwise-and-neighborhood}

<!-- source: calculus:L30-L37 -->
Distinguish differentiability and continuity at a single point from properties throughout a neighborhood:

- The existence of $f'(x_0)$ guarantees continuity of $f$ at $x_0$, but not at every point in a neighborhood. **Correction to the original counterexample: The Dirichlet function itself is nowhere continuous or differentiable.** Instead, use $f(x)=(x-x_0)^2D(x-x_0)$, where $D$ is $1$ at irrational inputs and $0$ at rational inputs. This function is differentiable at $x_0$ with derivative zero, yet remains discontinuous at every other point.
- The condition $f'(0)\gt 0$ does not guarantee strict monotonicity on any neighborhood. For example,

$$
f(x)=
\begin{cases}
x^2\sin(1/x)+x/2,&x\ne0,\\
0,&x=0
\end{cases}
$$

has $f'(0)=1/2$, but its derivative keeps changing sign at nearby points. The condition guarantees only that sufficiently close points to the left satisfy $f(x)\lt f(0)$, and those to the right satisfy $f(x)\gt f(0)$.
- Even if $f$ is continuous at $x_0$ and $g$ is discontinuous at $f(x_0)$, it does not follow that $g\circ f$ is discontinuous. If $f$ is constant, the composition is still continuous: the inner function may never reach the inputs along which the outer function's discontinuity appears.

<!-- source: calculus:L44-L70 -->
The definition of a derivative is itself a limit, but **the existence of a limit does not supply a function's missing value at the limit point**. Knowing only $\lim_{x\to x_0}f(x)=A$ is insufficient for continuity; one also needs $f(x_0)=A$. The example $f(x)=x^2$ for $x\ne0$, with $f(0)=C\ne0$, illustrates this distinction.

If

$$
\lim_{x\to x_0}\frac{f'(x)}{(x-x_0)^2}=A\ne0,
$$

then $f'$ has a fixed sign on a sufficiently small punctured neighborhood, so $f$ is monotone separately on the small intervals to the left and right. One cannot directly infer monotonicity across $x_0$, where the function might not even be continuous. Nor can $f'(x_0)$ simply be identified with $A$.

In contrast, if $f$ is already known to be differentiable on a closed interval, interpreting endpoint derivatives as one-sided derivatives, the corresponding one-sided continuity follows. For example, given $\lim_{x\to a^+}f(x)/x=A$, continuity yields $f(a)=aA$. If both one-sided derivatives exist, the function is continuous from both sides and therefore continuous at that point.

When a question gives only an abstract differentiable function and

$$
\lim_{x\to0}\frac{f(x)}{x^2}=A,
$$

separate the stated assumptions from any additional ones. Continuity, which follows from differentiability, gives $f(0)=0$, and the derivative definition then gives $f'(0)=0$. If $A\gt 0$, zero is a strict local minimum; if $A\lt 0$, it is a strict local maximum. For a specific expression, first use the limit to determine its parameters, then analyze its properties.

These assumptions alone do not imply $f''(0)=2A$, nor can l'Hôpital's rule be used backward to assert $\lim f'(x)/x=2A$. **L'Hôpital's rule does not generally require continuous derivatives, but it does require its own differentiability conditions, a nonzero derivative of the denominator, and an appropriate limit of the derivative ratio. Existence of the original ratio's limit does not imply existence of the derivative ratio's limit.** If $f''(0)$ is additionally known to exist, a second-order expansion together with the derivative definition establishes both conclusions.

A counterexample is

$$
f(x)=
\begin{cases}
x^2+x^3\sin(1/x),&x\ne0,\\
0,&x=0.
\end{cases}
$$

Here $f(x)/x^2\to1$, but $f'(x)/x=2+3x\sin(1/x)-\cos(1/x)$ has no limit.

For absolute values, if $f$ is differentiable at $x_0$ and $f(x_0)\ne0$, local preservation of its sign makes $|f|$ differentiable there. If $f(x_0)=0$, then $|f|$ is differentiable if and only if $f'(x_0)=0$; a zero need not produce a corner. Continuity of $f$ implies continuity of $|f|$, but the converse is false.

For $F(x)=\phi(x)|g(x)|$, the original claim that “$\phi(x_0)=0$ is necessary and sufficient” requires the additional assumptions that **$g$ has a simple zero at $x_0$**, meaning $g(x_0)=0,\ g'(x_0)\ne0$, and that both functions are differentiable. Then $\phi(x_0)\ne0$ preserves the corner, while $\phi(x_0)=0$ raises the first-order corner to a higher-order term. If $g'(x_0)=0$, $|g|$ itself may already be differentiable.

<!-- source: calculus:L71-L80 -->
If $f$ is continuous at $x_0$ and

$$
\lim_{x\to x_0}\frac{f(x)-f(x_0)}{(x-x_0)^n}=A,\qquad n\ge2,
$$

the definition yields $f'(x_0)=0$. Higher derivatives still require additional assumptions; continuity alone does not justify repeated applications of l'Hôpital's rule.

When $f'(x_0)=A$ is known, split a quotient into difference quotients:

$$
\frac{f(x)-f(x_0)}{g(x)-g(x_0)}
= \frac{[f(x)-f(x_0)]/(x-x_0)}
{[g(x)-g(x_0)]/(x-x_0)}.
$$

This requires the relevant conditions, including a finite defined value of $g$ at $x_0$. If $g$ tends to infinity, analyze it separately; one cannot form a difference quotient by subtracting “$g(x_0)=\infty$.”

Under the assumption $f(0)=0$, check the following when using limits to establish differentiability:

1. **Can the expression be converted to a first-order difference quotient?** Finite limits of $f(x)/x$ or $f(x)/x^2$, or a valid substitution in $f(\ln(1-x))/x$, can provide the relevant information. Do not rely on a slogan about “orders” alone.
2. **Are both sides covered?** The expression $f(\sqrt{1+x^2}-1)/x^2$ examines only nonnegative inputs and says nothing about the behavior of $f$ to the left of zero.
3. **Is the value at the point being overlooked?** The expressions $[f(x)-f(-x)]/x$ and $[f(x)-f(x^2)]/x$ may cancel the same constant near zero. Taking $f(x)=1$ for $x\ne0$ and $f(0)=0$ provides a counterexample.
4. **Are only selected paths being examined?** The limit $\lim nf(1/n)$ samples just one sequence approaching from the right. Even allowing positive and negative nonzero integers samples only countably many points and cannot replace the full limit. The Dirichlet function can distinguish rational and irrational paths.

References: Exercise 164 in $660$; $880_{base}$ 2.1.11. Also look for implicit derivative values: if $|f(x)|\le x^2$, then $f(0)=0$ and $|f(x)/x|\le|x|$, so the squeeze theorem gives $f'(0)=0$.

### Higher Derivatives and Geometric Information {#higher-derivatives-and-geometry}

<!-- source: calculus:L81-L93 -->
When an abstract function's second or third derivatives appear, first connect them to stationary points, inflection points, the sign of curvature, and Taylor expansions. For example, if $f''\gt 0$, the graph lies above each of its tangent lines. If

$$
\int_{-a}^{a}f(x)\,dx=b,\qquad a\gt 0,
$$

integrating $f(x)\ge f(0)+f'(0)x$ gives $b\ge2af(0)$. A Taylor expansion can suggest this argument, but integrating a local remainder across the entire interval does not replace a proof. If $f''\lt 0$, the inequality reverses.

{{< fig src="figures/convexity-and-tangent.png" alt="Comparing the curve and its tangent to bound an integral using the sign of the second derivative" caption="Comparing the curve and its tangent to bound an integral using the sign of the second derivative" >}}

The original diagram compares the area under the curve, $S_2=b$, with the signed area under the tangent at zero, $S_1=2af(0)$, illustrating that $f''\gt 0$ implies $f(0)\le b/(2a)$.

For derivatives of moderately high order, try parity, direct differentiation to find a pattern, or taking one or two derivatives before expanding. For example,

$$
f(x)=\ln(\sqrt{1+x^2}-x),\qquad
f'(x)=-\frac1{\sqrt{1+x^2}}
=-1+\frac{x^2}{2}-\frac{3x^4}{8}+o(x^4),
$$

so $f^{(5)}(0)=-9$. Partial fractions can also greatly simplify higher partial derivatives:

$$
\frac{2x}{x^2-y^2}=\frac1{x+y}+\frac1{x-y},
\qquad
\frac{d^n}{dx^n}\frac1x=(-1)^n\frac{n!}{x^{n+1}}.
$$

For $f(x)/(ax^2-by^2)$ or a similar expression with numerator $f(y)$, first look for a difference-of-squares factorization, then take partial derivatives while treating the other variable as constant.

### Parametric Curves, Extrema, and the Definition of a Differential {#parametric-curves-and-extrema}

<!-- source: calculus:L106-L112 -->
For parametric equations $x=\phi(t)$ and $y=\psi(t)$, when $\phi'(t)\ne0$,

$$
\frac{dy}{dx}=\frac{dy/dt}{dx/dt}.
$$

The original note incorrectly wrote $dx/dy$ on the left. If $y$ is defined implicitly by $te^y+y+1=0$, first treat $y$ as a function of $t$ to obtain $dy/dt=-e^y/(te^y+1)$, then divide by $dx/dt$.

Continuity and differentiability can be studied by obtaining an explicit function $y=f(x)$ or by using the parameter. However, continuity of $\phi,\psi$ must be accompanied by a suitable local inverse before the parametric curve can be interpreted as a continuous, single-valued function $y=f(x)$.

To find an oblique asymptote, identify the parameter limit for which $x(t)\to\infty$ and calculate

$$
a=\lim\frac{y(t)}{x(t)},\qquad b=\lim[y(t)-ax(t)].
$$

Exercise: $880_{base}$ 2.2.11.

<!-- source: calculus:L117-L126 -->
If $f'\ge0$ on an interval and vanishes at only finitely many points, the function is strictly increasing. The condition $f'(x_0)=0$ says only that the first-order rate of change is zero; it does not mean the function is constant. If an interior point is a local maximum and $f''(x_0)$ exists, then $f''(x_0)\le0$. Equality does not rule out a local maximum, as $-x^4$ shows.

Assess local extrema, stationary points, inflection points, and absolute extrema separately:

- A stationary point is a point where the function is differentiable and its first derivative is zero. A nondifferentiable point can also be a local extremum, as zero is for $|x|$. The original note grouped these together as stationary points; they need to be distinguished. The definition of a local extremum does not itself require continuity, whereas exam discussions of inflection points usually assume the appropriate continuity.
- For a piecewise smooth function, inspect whether the first derivative changes sign across the point to identify local extrema, and whether concavity changes to identify inflection points. The graph of the second derivative can also provide this information.
- If $f'(x_0)=0$ and $f''(x_0)\ne0$, the second-derivative test determines the type of extremum. If $f''(x_0)=0$, the point is only a candidate for an inflection point; under the relevant smoothness assumptions, $f'''(x_0)\ne0$ is a common sufficient criterion.
- To find absolute extrema, compare function values at stationary points, nondifferentiable points, and endpoints. On unbounded intervals, examine the limits at the ends as well, and distinguish an attained extremum from a supremum or infimum that is not attained.
- Under common assumptions such as continuous differentiability and piecewise monotonicity, a “unique local extremum” can help determine the overall behavior. If a closed interval contains only one local maximum and no local minimum in its interior, check the endpoints for the absolute minimum. Do not ignore the domain or transfer this reasoning directly to multivariable functions.
- When differentiating a piecewise function, handle the joining points separately using the definition. Differentiability cannot be discussed at a point where the function is undefined; a value must first be assigned there.

<!-- source: calculus:L127-L132 -->
For a polynomial, or a local factorization $f(x)=(x-a)^m h(x)$ with $h(a)\ne0$ under appropriate smoothness assumptions, differentiation turns a root of multiplicity $m$ into one of multiplicity $m-1$. If $m=1$, the derivative is nonzero at the root.

An expression of the form

$$
f(x+C)-f(x)=AC+o(C)\qquad(C\to0),
$$

can be read by treating $C$ as an increment and recognizing $A=f'(x)$. For example,

$$
f(x+y)-f(x)=(f(x)-1)y+o(y)
$$

gives the differential equation $f'=f-1$, whose solution is $f(x)=1+Ce^x$. The remainder describes behavior as the increment tends to zero; it is not an ordinary differential term that can simply be integrated.

## Calculus: Mean Value Theorems and Auxiliary Functions {#mean-value-theorems}

### Constructing Auxiliary Functions from Derivative Rules {#auxiliary-functions}

<!-- source: calculus:L133-L150 -->
When combinations of derivatives appear, first try recognizing a product, quotient, or chain rule in reverse.

| Form appearing in the problem | Possible auxiliary function |
| --- | --- |
| $f(x)f'(x)$ | $F(x)=f^2(x)$, since $F'=2ff'$ |
| $[f'(x)]^2+f(x)f''(x)$ | $F(x)=f(x)f'(x)$ |
| $f'(x)+f(x)\varphi'(x)$ | $F(x)=f(x)e^{\varphi(x)}$ |
| $f'(x)+f(x)$, $f'(x)-f(x)$, $f'(x)+kf(x)$ | Use $fe^x$, $fe^{-x}$, and $fe^{kx}$, respectively |
| $xf'(x)-f(x)$ | $F(x)=f(x)/x$, away from $x=0$ |
| $f''(x)f(x)-[f'(x)]^2$ | $F(x)=f'(x)/f(x)$, requiring $f(x)\ne0$ |
| The same expression, with $f(x)\gt 0$ | Alternatively, examine the second derivative of $\ln f(x)$ |

The original note's first derivative identity confused $(ff')'$ with $(f^2)'$; the table separates them. Also remember

$$
(uv)''=u''v+2u'v'+uv'',\qquad
\left(\frac uv\right)'=\frac{u'v-uv'}{v^2}.
$$

### Starting from Endpoints, Taylor Remainders, and Hidden Points {#mean-value-proof-patterns}

<!-- source: calculus:L151-L161 -->
If a proposition contains $a+b$ or $a-b$, connect difference quotients to the Lagrange or Cauchy mean value theorem. For example, when the denominators are nonzero and the interval and differentiability conditions permit,

$$
\frac{f'(\xi)}{2\xi}\frac1{f'(\eta)}
=\frac{f(a)-f(b)}{a^2-b^2}
\frac{a-b}{f(a)-f(b)}
=\frac1{a+b}.
$$

For inequalities involving $1/n!$ or higher derivatives, also consider Taylor's theorem with a Lagrange remainder. For example, for $x\ne0$,

$$
\sin x=x-\frac{x^2}{2}\sin\xi
\quad\Longrightarrow\quad
\left|\frac{\sin x}{x}-1\right|
\le\frac{|x|}{2},
$$

where $\xi$ lies between $0$ and $x$.

Some proofs need only a point known to exist, without finding it explicitly. Suppose $f$ is continuous on $[a,b]$, differentiable on $(a,b)$, nonconstant, and satisfies $f(a)=f(b)=0$. Choose $c\in(a,b)$ such that $f(c)\ne0$, then apply the mean value theorem on each side:

$$
f'(\xi_1)=\frac{f(c)-f(a)}{c-a},\qquad
f'(\xi_2)=\frac{f(b)-f(c)}{b-c}.
$$

The two values have opposite signs, so the derivative takes both positive and negative values within the interval.

## Calculus: Integration and Calculation Techniques {#integrals}

### Antiderivatives, Integrability, and Integral Mean Value Theorems {#antiderivatives-and-integrability}

<!-- source: calculus:L162-L180 -->
For an ordinary Riemann integral, if $f$ is integrable on a finite closed interval, then

$$
F(x)=\int_a^x f(t)\,dt
$$

is continuous. If $f$ is also continuous, then $F'=f$, so it is guaranteed to be an antiderivative of $f$.

For a continuous function, the integral mean value theorem follows from

$$
m(b-a)\le\int_a^b f(x)\,dx\le M(b-a)
$$

and the intermediate value theorem, giving $\xi\in[a,b]$. To obtain a mean value point in the open interval, define $F(x)=\int_a^x f(t)\,dt$ and apply the Lagrange mean value theorem:

$$
\int_a^b f(x)\,dx=F(b)-F(a)=f(\eta)(b-a),
\qquad\eta\in(a,b).
$$

The weighted version states that, if $f,g$ are continuous on $[a,b]$ and $g$ does not change sign, then

$$
\int_a^b f(t)g(t)\,dt=f(\xi)\int_a^b g(t)\,dt.
$$

When proving inequalities, one can also replace a constant upper limit with a variable. If $f$ is increasing, to prove

$$
\int_a^b xf(x)\,dx\ge\frac{a+b}{2}\int_a^b f(x)\,dx,
$$

if $f$ is also continuous, define

$$
H(x)=\int_a^x tf(t)\,dt-\frac{a+x}{2}\int_a^x f(t)\,dt.
$$

Then $H(a)=0$ and

$$
H'(x)=\frac{x-a}{2}f(x)-\frac12\int_a^x f(t)\,dt\ge0.
$$

This can also be interpreted using the integral mean value theorem. If only monotonicity is assumed, so that $f$ may have jump discontinuities, use reflection across the interval's midpoint instead. Set $c=(a+b)/2$; then

$$
\int_a^b(x-c)f(x)\,dx
=\int_a^c(c-x)[f(a+b-x)-f(x)]\,dx\ge0.
$$

This avoids differentiating at discontinuities and establishes the original conclusion for monotone functions.

The proof that every continuous function has an antiderivative likewise starts from

$$
F(x+\Delta x)-F(x)
=\int_x^{x+\Delta x}f(t)\,dt
=f(\xi)\Delta x.
$$

Letting $\Delta x\to0$ and using continuity gives $F'(x)=f(x)$ directly. **L'Hôpital's rule is unnecessary here.**

The derivative of an antiderivative has the intermediate value property. Therefore, a function with a jump discontinuity, a removable discontinuity, or a usual infinite discontinuity cannot have an antiderivative on an entire interval crossing that point. For piecewise antiderivatives, check whether the constants can make the pieces continuous with matching derivatives. Do not omit $+C$ from an indefinite integral on each connected interval.

<!-- source: calculus:L181-L197 -->
On a finite closed interval, boundedness is necessary for Riemann integrability. Continuity, monotonicity, or boundedness with only finitely many discontinuities are common sufficient conditions. This does not mean an improper integral must also have a finite interval and a bounded integrand.

When using additivity over intervals, track the endpoints in the sum:

$$
\lim_{n\to\infty}\sum_{k=1}^{n}\int_k^{k+1}f(x)\,dx
=\lim_{n\to\infty}\int_1^{n+1}f(x)\,dx
=\int_1^\infty f(x)\,dx
$$

(the final equality requires convergence of the corresponding improper integral).

Riemann sums for a definite integral need not use the right endpoint of every subinterval. Right endpoints, left endpoints, and midpoints can all be used. For example,

$$
\int_a^b f(x)\,dx
=\lim_{n\to\infty}\frac{b-a}{n}
\sum_{i=1}^{n}f\left(a+\frac{b-a}{n}i\right).
$$

On $[0,1]$, the midpoint version is $\frac1n\sum f((2i-1)/(2n))$. Dividing into $2n$ or $3n$ parts also works, provided the sampling points match the subinterval widths.

{{< fig src="figures/riemann-sum.png" alt="Sample points and subinterval widths in a Riemann sum" caption="Sample points and subinterval widths in a Riemann sum" >}}

The original diagram reminds us that the sample point can be chosen anywhere within each subinterval. For midpoints, write both the sampling point and the subinterval width correctly.

To compare definite integrals, try splitting terms, parity, inequalities, and properties of the functions:

- Split $\int_{-1/2}^{1/2}(x^2+x+1)/(x^2+1)\,dx$ into a constant and an odd function; the result is $1$.
- The original note compares

$$
M=\int_{-\pi/2}^{\pi/2}\sin^2x\cos x\,dx,\qquad
N=\int_{-\pi/2}^{\pi/2}(\sin^3x-\cos x)\,dx.
$$

  The inequality $\cos x\gt \sin x$ does not hold throughout this interval. Substitution and parity give $M=2/3\gt N=-2$ directly.
- For expressions such as $\int_0^1(1+x)\ln^2(1+x)/x^2\,dx$, first examine the numerator's structure, the denominator's sign, and their orders at the endpoints to avoid unnecessary case analysis.

### Variable-Limit and Improper Integrals {#variable-limit-integrals}

<!-- source: calculus:L198-L209 -->
A variable-limit integral is a function of its upper limit. If $|f|\le M$ and it is integrable, then

$$
|F(x+\Delta x)-F(x)|
=\left|\int_x^{x+\Delta x}f(t)\,dt\right|
\le M|\Delta x|,
$$

so $F$ is continuous. A function $f$ with finitely many discontinuities can still be integrable, but $F$ cannot then be called its antiderivative everywhere without further conditions.

At a jump discontinuity, the one-sided derivatives of the integral equal the corresponding one-sided limits of $f$. They generally differ, so $F$ is not differentiable there. At a removable discontinuity, $F'(x_0)=\lim_{x\to x_0}f(x)$, which may differ from the assigned value $f(x_0)$.

For $\int_{F_2(x)}^{F_1(x)}f(g(x)-h(t))\,dt$, a substitution that removes the parameter from the integrand can be easier than direct differentiation ($888_{comp}$ 3.3.8). In the original example, take care with the bounds:

$$
\int_0^{x^2}t f(x^2-t^2)\,dt
=\frac12\int_{x^2-x^4}^{x^2}f(u)\,du,\qquad u=x^2-t^2.
$$

Only an original upper limit of $x$ would give $\frac12\int_0^{x^2}f(u)\,du$. If $f$ is continuous, the derivative of the former expression is $xf(x^2)-(x-2x^3)f(x^2-x^4)$.

On a symmetric domain, if $f$ is odd, then $F(x)=\int_a^x f(t)\,dt$ is even. If $f$ is even, $\int_0^x f(t)\,dt$ is odd, but changing the lower limit adds a constant and may destroy oddness.

For improper integrals, the rule that “an odd function integrates to zero over a symmetric interval” is insufficient. For example, the one-sided integrals in $\int_{-\infty}^{\infty}e^{|x|}\sin x\,dx$ do not converge, so the ordinary improper integral does not exist. A principal value obtained through symmetric truncation is a different concept.

Limits of variable-limit integrals can be studied using equivalent infinitesimals or Taylor expansions valid near the interval of integration. However, having the same convergence behavior does not permit arbitrary replacement of the integrand. When using asymptotic equivalence of integrals, check the relevant conditions, such as a fixed sign, local equivalence, or a controlled error.

### Radical Signs, Integration by Parts, and Interval Symmetry {#integration-signs-and-substitution}

<!-- source: calculus:L210-L222 -->
**Retain absolute values when taking square roots.** Suppose a curve passes through $(-2,0)$ and has slope $1/(x\sqrt{x^2-1})$. Its relevant branch is $x\lt -1$. Set $x=\sec t$ with $t\in(\pi/2,\pi)$. Then $\tan t\lt 0$, so $\sqrt{\tan^2t}=-\tan t$, giving

$$
y=-\arccos(1/x)+\frac{2\pi}{3},\qquad x\lt -1.
$$

Similarly, for $R\gt 0$,

$$
\int_{-\pi/2}^{\pi/2}\int_0^{R\cos\theta}
\frac{r}{\sqrt{R^2-r^2}}\,dr\,d\theta
=\int_{-\pi/2}^{\pi/2}R(1-|\sin\theta|)\,d\theta.
$$

The $|\sin\theta|$ produced by the inner integral cannot simply be replaced with $\sin\theta$.

During calculations, consider these checks in turn:

- After splitting terms, is one part exactly the derivative of another, allowing cancellation through integration by parts? This often occurs in integrals involving $e^{f(x)}$ and trigonometric functions. The correct identity is $(1+\tan x)^2=\sec^2x+2\tan x$; the original note omitted the coefficient $2$. References: Zhang Yu, Foundation, 9.5 and 9.11; $880_{base}$ 3.3.4.6; $880_{comp}$ 3.3.3.
- In $\int x^{n-1}\sqrt{1+x^n}\,dx$, combine $x^{n-1}dx$ into a multiple of $d(x^n)$.
- After substitution, integration by parts can also be used directly between $\int f(t)\,dg(t)$ and $\int g(t)\,df(t)$ (Zhang Yu, Foundation, 9.7).
- For $f(x)g(\sin x)$, $f(x)g(\cos x)$, or certain expressions $f(x)e^x$, check whether the interval-reflection substitution $x\mapsto a+b-x$ produces a useful expression to add to the original. References: Zhang Yu, Foundation, 9.18 and 9.12; Wu Zhongxiang, Advanced, p. 115, Exercise 9.
- For $1/(1+\sin x)$ or $1/(1+\cos x)$, try multiplying the numerator and denominator by the conjugate expression. For example, definite integrals related to $\int x/(1+\sin x)\,dx$ can first be rationalized; then inspect the stated interval to decide whether the interval-reflection substitution is useful.

### Inverse Trigonometric Branches and Trigonometric Integrals {#inverse-trigonometric-branches}

<!-- source: calculus:L223-L240 -->
If a substitution produces $\arcsin(\sin x)$ or $\arctan(\tan x)$, first establish the inverse function's principal-value range and split the interval if necessary. For example,

$$
I=\int_0^1x\arcsin(2\sqrt{x-x^2})\,dx.
$$

With $x=\sin^2\theta$,

$$
I=\int_0^{\pi/2}2\sin^3\theta\cos\theta\,
\arcsin(\sin2\theta)\,d\theta,
\qquad
\arcsin(\sin2\theta)=
\begin{cases}
2\theta,&0\le\theta\le\pi/4,\\
\pi-2\theta,&\pi/4\le\theta\le\pi/2.
\end{cases}
$$

Therefore,

$$
I=\int_0^{\pi/4}2\sin^3\theta\cos\theta(2\theta)\,d\theta
+\int_{\pi/4}^{\pi/2}2\sin^3\theta\cos\theta(\pi-2\theta)\,d\theta.
$$

Write each part in terms of $d(\sin^4\theta)$, then integrate by parts to obtain $I=1/2$. This retains the original exercise's piecewise method while correcting the later typo in the transformed intervals.

The familiar arctangent identities also require conditions:

$$
\arctan x+\arctan y=
\begin{cases}
\arctan\frac{x+y}{1-xy},&xy\lt 1,\\
\pi+\arctan\frac{x+y}{1-xy},&xy\gt 1,\ x\gt 0,\\
-\pi+\arctan\frac{x+y}{1-xy},&xy\gt 1,\ x\lt 0,
\end{cases}
$$

$$
\arctan x-\arctan y=
\begin{cases}
\arctan\frac{x-y}{1+xy},&xy\gt -1,\\
\pi+\arctan\frac{x-y}{1+xy},&xy\lt -1,\ x\gt 0,\\
-\pi+\arctan\frac{x-y}{1+xy},&xy\lt -1,\ x\lt 0.
\end{cases}
$$

At the boundary $xy=1$,

$$
\arctan x+\arctan(1/x)=
\begin{cases}\pi/2,&x\gt 0,\\-\pi/2,&x\lt 0.\end{cases}
$$

When $xy=-1$, $\arctan x-\arctan(-1/x)$ has the same values as above.

A few other transformations are easy to overlook:

- Use parity, splitting terms first if necessary. Recognize transformed versions of the standard pattern $1/(a^2+x^2)$: for example, complete the square in $x^2-x+1$ to obtain $(x-1/2)^2+3/4$. Include all constant factors introduced by substitution.
- For a rational trigonometric expression $R(\sin x,\cos x)$, if it is odd in $\sin x$, set $u=\cos x$; if it is odd in $\cos x$, set $u=\sin x$. If reversing both signs leaves it unchanged, that is, $R(-\sin x,-\cos x)=R(\sin x,\cos x)$, try $u=\tan x$. The original note mistakenly said that the last condition required a sign change. In the general case, the universal substitution $u=\tan(x/2)$ is also available, with $\sin x=2u/(1+u^2)$ and $\cos x=(1-u^2)/(1+u^2)$. References: the original textbook, p. 289; Wu Zhongxiang, Foundation, p. 81, Exercises 13 and 14; $1k_{base}$ 14.20.
- Remember product-to-sum identities and the reverse use of sum-to-product identities:

$$
\int\sin x\cos(nx)\,dx
=\frac12\int[\sin((1+n)x)+\sin((1-n)x)]\,dx.
$$

  The original note omitted the factor $1/2$ here.
- For $e^{ax}\cos bx$ and $e^{ax}\sin bx$, become comfortable applying

$$
\int e^{ax}\cos bx\,dx
=\frac{e^{ax}(a\cos bx+b\sin bx)}{a^2+b^2}+C,
$$

$$
\int e^{ax}\sin bx\,dx
=\frac{e^{ax}(a\sin bx-b\cos bx)}{a^2+b^2}+C
$$

  with $a^2+b^2\ne0$. Reference: Zhang Yu, Foundation, p. 285, 10.4.
- For

$$
\int\frac{a\sin x+b\cos x}{c\sin x+d\cos x}\,dx,
$$

  solve $Ac-Bd=a$ and $Ad+Bc=b$ to write the numerator as $A(c\sin x+d\cos x)+B(c\cos x-d\sin x)$. The result is $Ax+B\ln|c\sin x+d\cos x|+C$. References: Li–Fan 3.40; $880_{base}$ 3.3.3 and 3.3.4.

### Reduction of Order, Area, and Special Functions {#area-beta-gamma}

<!-- source: calculus:L241-L251 -->
When $f(x)\int_a^xg(t)\,dt$ or higher derivatives appear, try integration by parts to reduce the order. References: Zhang Yu, Foundation, 11.6; $1k_{base}$ 10.7. Be fluent with basic trigonometric identities, such as the earlier $(1+\tan x)^2=\sec^2x+2\tan x$.

Geometric area cannot be negative. For $y=e^{-x}\sin x$, split the domain at successive zeros and use absolute values:

$$
\int_0^\infty e^{-x}|\sin x|\,dx
=\lim_{n\to\infty}\sum_{k=0}^{n}
\int_{k\pi}^{(k+1)\pi}e^{-x}|\sin x|\,dx.
$$

Check the sign on every subinterval; a signed integral is not automatically an area.

For a nested integral, define $F(x)=\int_x^b f(t)\,dt$. Then $F'=-f$, so

$$
\int_a^b dx\int_x^b f(x)f(y)\,dy
=\int_a^bF(x)\,d[-F(x)]
=\frac12\left(\int_a^b f(t)\,dt\right)^2.
$$

The Gamma and Beta functions convert some complicated integrals to standard forms:

$$
\Gamma(\alpha)=\int_0^\infty x^{\alpha-1}e^{-x}\,dx
=2\int_0^\infty t^{2\alpha-1}e^{-t^2}\,dt,\qquad\alpha\gt 0,
$$

$$
\Gamma(\alpha+1)=\alpha\Gamma(\alpha),\quad
\Gamma(n+1)=n!,\quad\Gamma(1/2)=\sqrt\pi,
$$

$$
B(p,q)=\int_0^1u^{p-1}(1-u)^{q-1}\,du
=\int_0^\infty\frac{t^{p-1}}{(1+t)^{p+q}}\,dt
=\frac{\Gamma(p)\Gamma(q)}{\Gamma(p+q)},\quad p,q\gt 0.
$$

A useful extension is

$$
\int_0^\infty\frac{x^{p-1}}{1+x^q}\,dx
=\frac{\pi}{q\sin(p\pi/q)},\qquad0\lt p\lt q.
$$

For example, set $t=\tan x$:

$$
\int_0^{\pi/2}\frac{\sin^2x\cos^2x}{(\sin x+\cos x)^6}\,dx
=\int_0^\infty\frac{t^2}{(1+t)^6}\,dt
=B(3,3)=\frac{2!\,2!}{5!}=\frac1{30}.
$$

### Reciprocal Substitutions and Parameter Integrals {#reciprocal-and-parameter-integrals}

<!-- source: calculus:L252-L258 -->
Using $x=1/t$ gives

$$
\int_0^\infty\frac{x^2}{1+x^4}\,dx
=\int_0^\infty\frac{1}{1+x^4}\,dx
=\frac12\int_0^\infty\frac{1+x^2}{1+x^4}\,dx.
$$

This follows from **reciprocal substitution and the Jacobian factor**, not ordinary mirror symmetry about the line $x=1$. For integrands such as $(1+x^2)/(1+x^4+ax^2)$, divide numerator and denominator by $x^2$ and look for $d(x-1/x)$ or $d(x+1/x)$.

The original notes also include an exercise on $\int_0^\infty1/(1+x^6)\,dx$ (Wu Zhongxiang, Advanced, note on p. 102). Retain the factorization approach:

$$
\frac1{1+x^6}
=\frac1{(1+x^2)(1-x^2+x^4)}
=\frac1{1-x^2+x^4}-\frac{x^2}{1+x^6}.
$$

Split the first integral's numerator into $(1+x^2)/2$ and $(1-x^2)/2$, then use $u=1/x-x$ and $v=1/x+x$, respectively. For $x\gt 0$, this gives

$$
\begin{aligned}
\int\frac{dx}{1+x^6}
={}&-\frac12\arctan(1/x-x)\\
&-\frac1{4\sqrt3}
\ln\left|\frac{1/x+x-\sqrt3}{1/x+x+\sqrt3}\right|
-\frac13\arctan(x^3)+C.
\end{aligned}
$$

The second denominator in the original derivation should be $v^2-3$, and the logarithm's coefficient also needs correction. Taking endpoint limits gives the definite integral $\pi/3$, which can also be checked directly using the Beta-function extension.

For parameter integrals, try Feynman's method, but first verify convergence of the original integral. The original expression $\int_0^1x^t/\ln x\,dx$ diverges at $x=1$, so it cannot simply be differentiated and then integrated back. The valid difference form is

$$
J(t)=\int_0^1\frac{x^t-1}{\ln x}\,dx,\qquad t\gt -1.
$$

Under conditions that permit interchanging differentiation and integration,

$$
J'(t)=\int_0^1x^t\,dx=\frac1{t+1},
\qquad J(0)=0,
\qquad J(t)=\ln(1+t).
$$

The original numerical example therefore remains valid:

$$
\int_0^1\frac{x^7-x^3}{\ln x}\,dx=\ln8-\ln4=\ln2.
$$

### Using Green's Theorem in Reverse on Disks {#green-formula-on-disks}

<!-- source: calculus:L259-L261 -->
When an integral involves a disk and conditions on first or second partial derivatives, try converting it to a boundary integral, then use Green's theorem or the divergence theorem to return to an easier area integral.

For example, suppose $f$ has continuous second partial derivatives on the closed unit disk and satisfies

$$
(f_{xx}+f_{yy})e^{x^2+y^2}=1.
$$

Find $I=\iint_{x^2+y^2\le1}(xf_x+yf_y)\,dx\,dy$. Let $D_r$ be the disk of radius $r$ and $L_r$ its counterclockwise boundary. A full circle requires an angular range from $0$ to $2\pi$; the original $\pi/2$ does not apply here. Thus

$$
\begin{aligned}
I
&=\int_0^1r\,dr\int_0^{2\pi}
(r\cos\theta f_x+r\sin\theta f_y)\,d\theta\\
&=\int_0^1r\,dr\oint_{L_r}(-f_y\,dx+f_x\,dy)\\
&=\int_0^1r\,dr\iint_{D_r}(f_{xx}+f_{yy})\,dx\,dy\\
&=\pi\int_0^1r(1-e^{-r^2})\,dr
=\frac{\pi}{2e}.
\end{aligned}
$$

The key is to integrate over concentric circles of varying radii and match each area-integral domain to its corresponding boundary $L_r$.

## Multivariable Differentiation {#multivariable-differentiation}

### Continuity, Partial Derivatives, and Differentiability {#continuity-partials-differentiability}

<!-- source: calculus:L262-L272 -->

A multivariable function is continuous at a point if and only if its limit there exists and equals its value. Existence of the limit alone is insufficient. Continuity does not imply the existence of partial derivatives or differentiability.

Partial derivatives describe rates of change along the respective coordinate axes; the two rates need not be equal. Existence of partial derivatives does not imply differentiability. Differentiability implies both continuity and the existence of partial derivatives. If the partial derivatives exist near a point and are continuous at that point, the function is differentiable there. **Differentiability guarantees a single linear approximation, but does not guarantee continuity of the partial derivatives.** A gradient consists of partial derivatives; existence of the gradient alone cannot replace differentiability.

The original notes use the following example:

$$
f(x,y)=
\begin{cases}
\dfrac{x^2y}{x^4+y^2},&(x,y)\ne(0,0),\\
0,&(x,y)=(0,0).
\end{cases}
$$

The function vanishes along both coordinate axes, so $f_x(0,0)=f_y(0,0)=0$. However, along $y=x^2$, it satisfies $f(x,x^2)=1/2$. It is therefore discontinuous, and hence not differentiable, at the origin.

> Correction: The original notes describe this function as differentiable at the origin with discontinuous partial derivatives. It actually demonstrates that partial derivatives can exist without continuity or differentiability. The following function demonstrates the intended distinction between differentiability and continuous partial derivatives.

$$
g(x,y)=
\begin{cases}
x^2\sin(1/x),&x\ne0,\\
0,&x=0.
\end{cases}
$$

The bound $|g(x,y)|\le x^2$ shows that $g$ is differentiable at the origin with zero total differential. For $x\ne0$, however,

$$
g_x(x,y)=2x\sin(1/x)-\cos(1/x),
$$

which has no limit at the origin.

When only $f_x(x_0,y_0)$ and $f_y(x_0,y_0)$ are known to exist, distinguish information along axes through one point from information throughout a disk. The definition

$$
f_x(x_0,y_0)=\lim_{x\to x_0}
\frac{f(x,y_0)-f(x_0,y_0)}{x-x_0}
$$

directly gives differentiability of the one-variable restriction with $y=y_0$, and therefore

$$
\lim_{x\to x_0}f(x,y_0)=f(x_0,y_0).
$$

The same reasoning applies with $x=x_0$ fixed. Definitions and continuity along two coordinate directions do not establish a definition or continuity throughout a two-dimensional neighborhood. In ordinary multivariable problems, being defined on a neighborhood is often an assumption, not a consequence of the existence of partial derivatives.

### Implicit Functions and Mixed Partial Derivatives {#implicit-functions-mixed-partials}

<!-- source: calculus:L273-L274 -->

For $F(x,y)=0$, assuming the remaining hypotheses of the implicit function theorem, $F_y\ne0$ is sufficient for a local differentiable implicit function $y=y(x)$ to exist. If $F_y=0$, its existence is not ruled out. However, an indeterminate expression $-F_x/F_y$ does not by itself establish either an implicit function or its derivative; examine the original equation.

When using the definition to calculate mixed partial derivatives, retain the other variable as a parameter. For example, first calculate

$$
f_x(x_0,y)=\lim_{h\to0}
\frac{f(x_0+h,y)-f(x_0,y)}{h},
$$

then differentiate with respect to $y$ to obtain $f_{xy}=\partial_y(\partial_x f)$. For second or higher partial derivatives, do not substitute the coordinates of a point for every variable too early.

### The Total Differential and Linear Approximation {#total-differential-linear-approximation}

<!-- source: calculus:L275-L286 -->

Take $(x_0,y_0)$ as the base point and let $r=\sqrt{(\Delta x)^2+(\Delta y)^2}$. Differentiability there means that constants $A,B$ exist such that

$$
\begin{aligned}
\Delta z&=f(x_0+\Delta x,y_0+\Delta y)-f(x_0,y_0),\\
\Delta z&=A\Delta x+B\Delta y+o(r).
\end{aligned}
$$

Equivalently,

$$
\lim_{(\Delta x,\Delta y)\to(0,0)}
\frac{\Delta z-A\Delta x-B\Delta y}{r}=0,
\qquad
A=f_x(x_0,y_0),\quad B=f_y(x_0,y_0).
$$

The total differential is $dz=A\,dx+B\,dy$. **The distance in the remainder term must be the distance from the base point.**

For example, let $\rho=\sqrt{(x-a)^2+(y-b)^2}$. Suppose the actual denominator in a problem is a function $q(x,y)=o(\rho)$, the relevant quotient is defined, and

$$
\lim_{(x,y)\to(a,b)}
\frac{f(x,y)-f(a,b)+3(x-a)-4(y-b)}{q(x,y)}=C,
\qquad 0\lt |C|\lt \infty.
$$

The same numerator divided by $\rho$ then has limit $C\cdot0=0$. Consequently, $f$ is differentiable at $(a,b)$, with

$$
f_x(a,b)=-3,\qquad f_y(a,b)=4.
$$

The denominator written as $o(\rho)$ in the original notes is named $q$ here to avoid treating little-o notation as a uniquely specified function.

### Finding Local and Global Extrema {#multivariable-extrema-workflow}

<!-- source: calculus:L287-L295 -->

Organize candidate points for local extrema, maxima, and minima in the following order:

1. Check interior points where differentiability or partial derivatives fail, and solve $f_x=f_y=0$ for stationary points. Denote the interior candidates by $M_i$. **A nondifferentiable point is not called a stationary point, and a stationary point need not be an extremum.**
2. If there are constraints, remove interior candidates that violate them, then use Lagrange multipliers on smooth constraints or boundaries to obtain candidates $M_j$.
3. Check boundary endpoints, corners, and points where constraint regularity fails separately; denote these by $M_k$.
4. Once existence of global extrema is established and all candidates have been included, compare their function values. Local extrema still require classification.

If $D$ is a region with an interior, first handle its interior, then substitute the boundary conditions to reduce the boundary problem to a one-variable function $g(x)$. If that function is difficult to optimize directly, use Lagrange multipliers on the boundary. If $D$ itself is an equality constraint, optimize on that constraint directly.

A global extremum is also a local extremum relative to the feasible set. A boundary extremum may be only a constrained extremum, not an unconstrained extremum in the entire plane. **Even if the interior contains just one local extremum, do not automatically identify it as the global extremum**; the boundary still needs checking.

<!-- source: calculus:L306-L310 -->

> Correction: Existence of second partial derivatives does not guarantee a maximum or minimum on a region. A standard sufficient condition is that $D$ is nonempty, closed, and bounded, and $f$ is continuous on $D$.

Under conditions guaranteeing global extrema, if no interior point is an extremum—neither a stationary point nor a point where partial derivatives fail—global extrema must occur on the boundary. This parallels checking interior candidates and endpoints for a one-variable function on a closed interval.

### Lagrange Multipliers and Representative Examples {#lagrange-multipliers-examples}

<!-- source: calculus:L296-L305 -->

When direct elimination in the Lagrange equations is difficult, try arranging them into a homogeneous linear system in the original variables:

$$
\begin{cases}
\phi_1(\lambda)x+\mu_1(\lambda)y=0,\\
\phi_2(\lambda)x+\mu_2(\lambda)y=0.
\end{cases}
$$

First check whether $(0,0)$ satisfies the constraint. If the constraint excludes the zero solution, the system must have a nonzero solution. Set its coefficient determinant to zero, solve for $\lambda$, then find $(x,y)$. Another useful approach is to multiply or divide derivative equations, or multiply each equation by its corresponding variable and add, then simplify using the constraints. Before dividing, check cases where the divisor vanishes.

**Example: A plane section of an ellipsoid.** Find the semimajor and semiminor axes of the ellipse cut from

$$
\frac{x^2}{3}+\frac{y^2}{2}+z^2=1
$$

by the plane $x+y+z=0$.

This is a particularly useful example: the cutting plane passes through the ellipsoid's center, so the ellipse is centered at the origin. The ellipse itself does not pass through the origin. Its semiaxis lengths are the maximum and minimum distances from the origin, so optimize $d^2=x^2+y^2+z^2$.

> Correction: The original Lagrangian omitted the distance objective and one constraint. There are two constraints, requiring two multipliers.

$$
L=x^2+y^2+z^2
+\lambda\left(\frac{x^2}{3}+\frac{y^2}{2}+z^2-1\right)
+\mu(x+y+z).
$$

Use $L_x=L_y=L_z=0$ and $z=-x-y$ to eliminate $\mu,z$. This gives the system retained from the original notes:

$$
\begin{cases}
(2+2\lambda)x+(4+3\lambda)y=0,\\
(6+2\lambda)x+(-6-3\lambda)y=0.
\end{cases}
$$

The constraints exclude the zero solution, so

$$
\begin{vmatrix}
2+2\lambda&4+3\lambda\\
6+2\lambda&-6-3\lambda
\end{vmatrix}=0
\quad\Longrightarrow\quad
3\lambda^2+11\lambda+9=0.
$$

There is no need to determine every coordinate. Multiply the three stationary equations by $x,y,z$, respectively, and add. The constraints give $d^2=-\lambda$. The semimajor and semiminor axes are therefore

$$
a=\sqrt{\frac{11+\sqrt{13}}6},
\qquad
b=\sqrt{\frac{11-\sqrt{13}}6}.
$$

**Example: An arc maximizing a line integral.** On the counterclockwise-oriented ellipse

$$
C:\quad \frac{x^2}{4}+y^2=1,
$$

choose an arc $L$ that maximizes $\int_L(dx+2\,dy)$. Since

$$
\int_L(dx+2\,dy)=(x+2y)\big|_A^B,
$$

choose a starting point $A$ that minimizes $x+2y$ and an endpoint $B$ that maximizes it. Lagrange multipliers with the ellipse constraint give values $-2\sqrt2$ and $2\sqrt2$, respectively. The maximum integral is $4\sqrt2$. The original notes mistakenly said to minimize the endpoint value as well.

### When the Hessian Test Is Inconclusive {#degenerate-hessian-extrema}

<!-- source: calculus:L311-L316 -->

At a stationary point with continuous second partial derivatives, write $A=f_{xx}$, $B=f_{xy}$, and $C=f_{yy}$. If $AC-B^2=0$, the second derivative test is inconclusive. Try higher-order Taylor expansion or restrictions to particular paths.

For a one-variable restriction with a first nonzero higher derivative, an odd order typically rules out an extremum, while an even order allows classification by its sign; all preceding derivatives must vanish. In several variables, one path can help disprove an extremum, but testing a collection of straight lines generally cannot prove that an extremum exists.

The function in the original notes is

$$
f(x,y)=x^2+y^2-6x+10=(x-3)^2+y^2+1.
$$

It attains its strict minimum $1$ at $(3,0)$, and $AC-B^2=4$. The originally recorded point $(2,0)$ is not stationary, and the third derivative of $f(x,0)$ is zero.

> Correction: This quadratic is not an example of a degenerate Hessian. The original function and the recorded point $(2,0)$ are retained, but the original third-derivative argument and conclusion cannot be used.

A genuine degenerate example is $f(x,y)=x^4+y^4$. At $(0,0)$, the Hessian determinant is zero. Along $y=kx$,

$$
f(x,kx)=x^4(1+k^4).
$$

More directly, $x^4+y^4\gt 0$ at every nonzero point proves that the origin is a strict local minimum. The proof depends on this inequality in all directions, not only on straight-line checks.

### Definition-Based Differentiation and Absolute Values {#partial-derivatives-square-root-signs}

<!-- source: calculus:L317-L320 -->

**When calculating partial derivatives from the definition, retain the absolute value in $\sqrt{x^2}=|x|$.**

Example: Suppose $f$ is defined at the origin and

$$
\lim_{(x,y)\to(0,0)}
\frac{f(x,y)-(x^2+y^2)}{\sqrt{x^2+y^2}}=1.
$$

Determine whether $f_x(0,0)$ and $f_y(0,0)$ exist. Let $r=\sqrt{x^2+y^2}$. Away from the origin,

$$
f(x,y)=r^2+r+o(r)=r+o(r).
$$

If $f(0,0)=0$, the difference quotient along $y=0$ is

$$
\frac{f(x,0)-f(0,0)}x
=\frac{\sqrt{x^2}}x+o(1)
=\frac{|x|}x+o(1).
$$

Its left and right limits differ, so $f_x(0,0)$ does not exist. The same argument applies to $f_y(0,0)$. If $f(0,0)\ne0$, the coordinate restrictions are already discontinuous, so neither partial derivative exists in that case either.

> Assumption note: The original argument directly used $f(0,0)=0$, but the stated limit does not prescribe the value at the origin. After separating the two cases, the conclusion that neither partial derivative exists remains valid.

### Implicit Systems and Information About Second Derivatives {#implicit-systems-second-derivatives}

<!-- source: calculus:L321-L328 -->

If $x=x(y)$ and $z=z(y)$ are determined by

$$
\begin{cases}
F(f_1(x,y,z),g_1(x,y,z))=0,\\
G(f_2(x,y,z),g_2(x,y,z))=0,
\end{cases}
$$

calculate $dx/dy$ and $dz/dy$ by differentiating both equations with respect to $y$. This works for both explicit and abstract functions.

For example,

$$
\begin{cases}
F(y-x,y-z)=0,\\
G(xy,z/y)=0
\end{cases}
$$

gives, when $y\ne0$ and the required differentiation conditions hold,

$$
\begin{cases}
F_1'\dfrac{dx}{dy}+F_2'\dfrac{dz}{dy}=F_1'+F_2',\\
yG_1'\dfrac{dx}{dy}+\dfrac1yG_2'\dfrac{dz}{dy}
=-xG_1'+\dfrac{z}{y^2}G_2'.
\end{cases}
$$

Here $F_i'$ and $G_i'$ mean partial derivatives with respect to the respective function's $i$th argument, evaluated at the corresponding composite arguments. **Arrange the result as a linear system like this, then apply Cramer's rule when its coefficient determinant is nonzero.** Related exercises: `880_base(3.1)` and `880_comp(3.15)`.

When some first-derivative information is given and second derivatives are requested, first consider differentiating both sides of the existing identities to obtain additional relations. Recovering the original function before differentiating is usually harder.

Example: Suppose $u_{xx}=u_{yy}$ and

$$
u(x,2x)=x,\qquad u_x(x,2x)=x^2.
$$

Find $u_{xx}(x,2x)$. When the chain rule and interchange of mixed partials are justified, the following identities hold along $y=2x$:

$$
\begin{aligned}
u_x+2u_y&=1,\\
u_{xx}+2u_{xy}&=2x,\\
u_{xx}+2u_{xy}+2u_{yx}+4u_{yy}&=0.
\end{aligned}
$$

Using $u_{xy}=u_{yx}$ and $u_{xx}=u_{yy}$ gives

$$
u_{xx}(x,2x)=-\frac43x.
$$

> Correction and assumption note: One $u_{xx}$ in the original third identity should be $u_{yx}$. The excerpted problem states only that second partial derivatives exist; it does not fully specify the conditions for the chain rule and interchange of mixed partials. The usual assumption $u\in C^2$ justifies the calculation, so check the complete problem statement when reviewing it.

## Double Integrals {#double-integrals}

### The Mean Value Theorem and Symmetry {#double-integral-mean-value-symmetry}

<!-- source: calculus:L329-L335 -->

If a double integral is difficult to evaluate and the region's area appears in the problem, consider the mean value theorem for double integrals. Under standard conditions, such as a bounded connected closed region $D$ of positive area and a continuous function $f$,

$$
\iint_D f(x,y)\,dA=f(\xi,\eta)\operatorname{Area}(D),
\qquad (\xi,\eta)\in D.
$$

When an integral is difficult, especially when an abstract function occurs, **check symmetry under exchanges of variables first**. Does the region remain unchanged after exchanging variables or changing signs? Can the transformed integrands be added or canceled? Related exercises: `1k_base(14.14)`, `Wu_base(9.11)`, and `880_base(2.3, 2.4)`.

For abstract functions or suspected symmetry, it may help to partition the region and isolate pieces symmetric about the $x$-axis, $y$-axis, $y=x$, or $y=-x$. Related exercises: `1k_base(14.17, 14.23)`.

### Shifted Polar Coordinates and Exact Differentials {#translated-polar-coordinates-exact-differential}

<!-- source: calculus:L336-L338 -->

If a circle is not centered at the origin, for example,

$$
(x-a)^2+(y-b)^2=a^2+b^2,
$$

use the substitution

$$
\begin{cases}
x-a=r\cos\theta,\\
y-b=r\sin\theta.
\end{cases}
$$

For the entire enclosed disk, the bounds are $0\le r\le\sqrt{a^2+b^2}$ and $0\le\theta\le2\pi$, with $dA=r\,dr\,d\theta$. If the region includes only part of the disk, determine the angular bounds separately.

Also watch for double integrals that can be converted into exact differentials. Set

$$
F(x)=\int_x^b f(y)\,dy.
$$

Under the usual integrability and differentiation conditions, $d[-F(x)]=f(x)\,dx$, so

$$
\begin{aligned}
\int_a^b dx\int_x^b f(x)f(y)\,dy
&=\int_a^b\left[\int_x^b f(y)\,dy\right]
\,d\left[-\int_x^b f(y)\,dy\right]\\
&=-\frac12[F(x)^2]_a^b
=\frac12\left(\int_a^b f(y)\,dy\right)^2.
\end{aligned}
$$

The dummy integration variable is consistently written as $y$ here to avoid confusion with the variable endpoint $x$ in the original notes.

## Differential Equations {#differential-equations}

### Substitution and Sign Checks {#ode-substitution-signs}

<!-- source: calculus:L339-L350 -->

If solving directly for $y=y(x)$ is difficult but solving for $x=x(y)$ is easier, exchange dependent and independent variables on an interval where local inversion is valid. Use $dx/dy=1/(dy/dx)$, solve, and invert the result to recover $y(x)$. Check the conditions for inversion, including a nonzero derivative.

More generally, it is not always necessary to solve directly for $y(x)$ or $x(y)$. It may be easier to obtain a relation between $x$ and $g(y)$, or between $y$ and $h(x)$—the possibilities recorded as $x(g(y))$ and $y(h(x))$ in the original notes—and then restore the original variables.

For example,

$$
y'+1=e^{-y}\sin x.
$$

Multiply by $e^y$ and set $u=e^y$ to get

$$
e^y y'+e^y=\sin x
\quad\Longrightarrow\quad
u'+u=\sin x.
$$

Solving this first-order linear equation gives

$$
e^y=u=\frac{\sin x-\cos x}{2}+Ce^{-x}.
$$

On an interval where the right-hand side is positive, take its logarithm to obtain $y$.

> Correction: After multiplication by $e^y$, the left-hand side is $(e^y)'+e^y$. The additional $e^y$ term must not be dropped.

If a problem asks for an equivalent infinitesimal of order $n$ associated with $f(x)$, consider Taylor expansion and compare the lower-order coefficients to obtain relations between constants.

Check whether the equation can be arranged in terms of $x/y$, $y/x$, or a similar ratio. Whenever dividing by a variable, **check its sign**, especially when $\sqrt{x^2}=|x|$ appears.

Example: Find the general solution of

$$
xy'=\sqrt{x^2+y^2}+y.
$$

Set $t=y/x$, but first separate the cases according to the sign of $x$:

$$
\begin{cases}
y'=\sqrt{1+(y/x)^2}+y/x,&x\gt 0,\\
y'=-\sqrt{1+(y/x)^2}+y/x,&x\lt 0.
\end{cases}
$$

Substituting $y'=t+xt'$ and separating variables gives

$$
\begin{cases}
\ln\bigl(t+\sqrt{1+t^2}\bigr)=\ln x+C,&x\gt 0,\\
\ln\bigl(t+\sqrt{1+t^2}\bigr)=-\ln|x|+C,&x\lt 0.
\end{cases}
$$

The division above is valid only on intervals where $x\ne0$. If the problem includes $x=0$, return to the original equation and examine that point separately.

### The Structure of Solutions to Linear Equations {#linear-ode-solution-structure}

<!-- source: calculus:L351-L357 -->

For the same nonhomogeneous **linear** differential equation $L[y]=f(x)$, the difference of two particular solutions is a solution of the corresponding homogeneous equation $L[y]=0$. From particular solutions $y_1,y_2,y_3,\ldots$, find sufficiently many linearly independent differences to form the homogeneous general solution, then add any particular solution. If the equation itself must be recovered, use these homogeneous solutions to determine coefficient relations, then substitute one particular solution to determine the right-hand side $f(x)$.

For a second-order equation, if $y_1-y_3$ and $y_2-y_3$ are linearly independent, the general solution is

$$
\begin{aligned}
y&=C_1[y_1(x)-y_3(x)]+C_2[y_2(x)-y_3(x)]+y_3(x)\\
&=C_1y_1+C_2y_2+C_3y_3,
\qquad C_1+C_2+C_3=1.
\end{aligned}
$$

If the three particular solutions are already known to be linearly independent as functions, these two differences are necessarily linearly independent.

> Correction: A single difference is not the homogeneous general solution; linear combinations are required. The constants $C_1,C_2,C_3$ need not be pairwise distinct. Their sum is constrained to equal $1$, so only two are independent.

Example: A second-order nonhomogeneous linear equation has particular solutions $x,e^x,e^{-x}$. First construct the homogeneous general solution

$$
C_1(e^x-x)+C_2(e^{-x}-x),
$$

then add the particular solution $x$:

$$
y=C_1(e^x-x)+C_2(e^{-x}-x)+x.
$$

### Combining Differential Equations with Integral Conditions {#ode-integral-conditions}

<!-- source: calculus:L358-L358 -->

If the problem explicitly or implicitly gives an integral representation of a function, use it together with the differential equation. Integrating both sides of the equation may reveal a more direct relation. Related exercise: `660-81`.

## Infinite Series {#infinite-series}

<!-- source: calculus:L359-L362 -->

> Study weakness: Power-series expansions about a specified point, especially deciding whether related statements are true or false.

### Term Limits and Convergence {#series-terms-convergence}

<!-- source: calculus:L363-L371 -->

Use the necessary condition for convergence,

$$
\sum_{n=a}^{\infty}u_n\text{ converges}
\quad\Longrightarrow\quad
\lim_{n\to\infty}u_n=0,
$$

to determine unknown constants. If the intended condition is convergence of $\sum_{n=a}^{\infty}(u_n+v_n)$, use $u_n+v_n\to0$ and equivalent infinitesimals to solve for the parameters.

> Notation reminder: The original notes write $\sum u_n+\sum v_n$. Before both series have individually been shown to converge, interpret the intended object as the series obtained by adding terms. Do not directly perform algebra on two potentially divergent infinite sums.

When proving convergence or divergence, use equivalent infinitesimals and comparison tests. Products and quotients may also suggest the **arithmetic–geometric mean inequality**. For eventually positive terms, $u_n/v_n\to c\in(0,\infty)$ gives the same convergence behavior. If $u_n/v_n\to0$ and $\sum v_n$ converges, comparison gives convergence of $\sum u_n$.

> Correction: $u_nv_n\to0$ does not imply $u_n/v_n\to0$. For example, $u_n=v_n=1/n$ gives a product tending to zero but a quotient identically equal to $1$. The corresponding step in the original notes is missing hypotheses and cannot be used as written.

A series diverges whenever its terms fail to tend to zero. If eventually $|u_{n+1}|\gt |u_n|$ and the magnitudes are positive from some point onward, they have a positive lower bound and cannot tend to zero. Merely observing that every term is nonzero is insufficient.

A convergent series can be grouped into finite blocks, and $\sum(u_n+u_{n+1})$ also converges. The converse fails. For example, $1-1+1-1+\cdots$ diverges, whereas grouping adjacent pairs gives zero in every block. **Without knowing that the original series converges, do not infer convergence from a grouped version.**

If terms contain integrals that cannot be evaluated directly, or standard tests are difficult to apply, try bounds and comparison.

### Alternating Series and Taylor Expansion {#alternating-series-taylor-expansion}

<!-- source: calculus:L372-L380 -->

When the factor $(-1)^n$ is hidden, try extracting it with identities. If the alternating series test is difficult to apply, use Taylor expansion to analyze the series corresponding to the leading terms and remainders, or prove absolute convergence instead.

> Test reminder: If a decomposition has exactly one divergent series and all other parts converge, the original series diverges. If several parts diverge, check for cancellation; one divergent part alone does not settle the question.

For signed series, equivalent terms alone do not generally guarantee identical convergence behavior. If

$$
\sum_{n=1}^{\infty}\xi_n
=\sum_{n=1}^{\infty}(u_n-v_n)
$$

converges, then $\sum u_n$ and $\sum v_n$ converge or diverge together. If additionally $|u_n|\sim|v_n|$, their absolute convergence behavior also agrees, allowing conclusions about conditional convergence.

For example, consider

$$
\sum_{n=1}^{\infty}\sin\left(n\pi+\frac1{\sqrt n}\right),
\qquad
\sum_{n=1}^{\infty}\sin\left(\pi\sqrt{n^2+1}\right).
$$

Trigonometric identities and rationalization give the respective terms

$$
(-1)^n\sin\frac1{\sqrt n},
\qquad
(-1)^n\sin\frac{\pi}{\sqrt{n^2+1}+n}.
$$

Both positive factors decrease to zero, so the alternating series test proves convergence. Their absolute values are equivalent to $1/\sqrt n$ and $\pi/(2n)$, respectively, so neither converges absolutely. Both are conditionally convergent.

When comparison, root tests, or similar tests are not immediately useful, particularly with cancellation or unknown parameters, consider Taylor expansion.

**Example 1:**

$$
\sum_{n=1}^{\infty}\left[\frac1n-\ln\left(1+\frac1n\right)\right].
$$

Expanding the logarithm gives the term

$$
\frac1n-\left(\frac1n-\frac1{2n^2}+O(n^{-3})\right)
=\frac1{2n^2}+O(n^{-3}),
$$

which has the same order as $1/n^2$, so the series converges.

**Example 2:**

$$
\sum_{n=1}^{\infty}\left(a^{1/n}-\sqrt{1+\frac1n}\right).
$$

The original notes do not state the allowed range of $a$. Following their approach, assume $a\gt 0$ so that $a^{1/n}=e^{(\ln a)/n}$ can be used. Expanding both terms gives

$$
a^{1/n}-\sqrt{1+\frac1n}
=\frac{\ln a-1/2}{n}
+\frac{(\ln a)^2/2+1/8}{n^2}+O(n^{-3}).
$$

Thus the series converges when $a=e^{1/2}$, with terms equivalent to $1/(4n^2)$. For every other $a\gt 0$, the nonzero harmonic leading term causes divergence.

### Summation by Integration and the Constant of Integration {#power-series-integration-constant}

<!-- source: calculus:L381-L386 -->

**After integrating a sum function, remember $+C$ and use its value at $x=0$ to determine the constant.**

Example: Find

$$
S(x)=\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^n,
\qquad |x|\lt 1.
$$

Set

$$
G(x)=\sum_{n=1}^{\infty}\frac{(n-1)^2}{n+1}x^{n+1}=xS(x).
$$

Then

$$
G'(x)=\sum_{n=1}^{\infty}(n^2-2n+1)x^n.
$$

Use the following standard generating functions in sequence:

$$
\begin{aligned}
S_1(x)&=\sum_{n=1}^{\infty}x^n=\frac{x}{1-x},\\
S_2(x)&=xS_1'(x)=\frac{x}{(1-x)^2},\\
S_3(x)&=xS_2'(x)=\frac{x(1+x)}{(1-x)^3},\\
G(x)&=\int\bigl(S_1(x)-2S_2(x)+S_3(x)\bigr)\,dx.
\end{aligned}
$$

Substitute $t=1-x$ and integrate:

$$
G(x)=\frac1{(1-x)^2}-\frac5{1-x}-4\ln(1-x)-x+C.
$$

Since $G(0)=0$, we have $-4+C=0$, so $C=4$. Therefore

$$
S(x)=\frac1x\left[\frac1{(1-x)^2}-\frac5{1-x}
-4\ln(1-x)-x+4\right],\qquad x\ne0.
$$

The value $S(0)=0$ must be added separately.

> Correction: The original notes used $1/(1-x)$ for a geometric series starting at $n=1$, overlooking the index difference. The correct expression here is $x/(1-x)$, which also requires the $-x$ term in the antiderivative.

### Recurrences and Equations for Generating Functions {#recursive-coefficients-generating-function}

<!-- source: calculus:L387-L392 -->

With abstract coefficients, first derive a relation from the given conditions. Differentiation, integration, or transformations resembling integration by parts may convert a recurrence into a differential equation for the sum function.

Example: Given

$$
a_1=1,\qquad
a_{n+1}=\left(1-\frac1{2(n+1)}\right)a_n,
$$

find $S(x)=\sum_{n=1}^{\infty}a_nx^n$ for $|x|\lt 1$. Differentiate term by term and use the recurrence:

$$
\begin{aligned}
S'(x)&=1+\sum_{n=1}^{\infty}(n+1)a_{n+1}x^n\\
&=1+\sum_{n=1}^{\infty}na_nx^n
+\frac12\sum_{n=1}^{\infty}a_nx^n\\
&=1+xS'(x)+\frac12S(x).
\end{aligned}
$$

Consequently,

$$
(1-x)S'(x)-\frac12S(x)=1,\qquad S(0)=0.
$$

Solving gives

$$
S(x)=2\left(\frac1{\sqrt{1-x}}-1\right).
$$

> Correction: Retain the contribution $a_1=1$ when shifting indices after differentiation. The left-hand side is $S'(x)$; the original notes incorrectly wrote $S(x)$.

Other identities can also help with related problems, including Wallis's formula. Related exercise: `Zhang_base(16.34)`.

### Convergence Intervals, Standard Functions, and Partial Fractions {#power-series-domain-standard-functions}

<!-- source: calculus:L393-L398 -->

The sum of a power series is continuous inside its interval of convergence and may be differentiated and integrated term by term there. If the resulting expression appears to have a discontinuity at a point, handle that point separately. At $x=0$, evaluate the original series directly; its value need not be zero. At another point $c$, if direct substitution is inconvenient, take the limit once continuity of the sum function is known.

> Assumption reminder: Continuity of every term does not by itself imply continuity of an infinite sum. These results hold inside a power series's interval of convergence. Endpoints require separate convergence checks and an applicable endpoint continuity theorem.

Recognize transformed versions of standard functions. For example,

$$
\frac1{x-2}
=-\frac12\frac1{1-x/2}
=-\frac12\sum_{n=0}^{\infty}\left(\frac x2\right)^n,
\qquad |x|\lt 2.
$$

The original transformation of $1/(x-2)$ omitted the factor $-1/2$. After rearranging or splitting terms, align the indices. If extra terms were introduced to use a standard formula, subtract them afterward.

Example: Evaluate

$$
\sum_{n=2}^{\infty}\frac1{(n^2-1)2^n}.
$$

Treat $2^{-n}$ as $x^n$: first find $S(x)=\sum_{n=2}^{\infty}x^n/(n^2-1)$, then evaluate $S(1/2)$. Partial fractions give

$$
\begin{aligned}
S(x)&=\frac12\sum_{n=2}^{\infty}
\left(\frac{x^n}{n-1}-\frac{x^n}{n+1}\right)\\
&=\frac12\left[
x\sum_{m=1}^{\infty}\frac{x^m}{m}
-\frac1x\sum_{k=3}^{\infty}\frac{x^k}{k}\right]\\
&=\frac12\left[
-x\ln(1-x)-\frac{-\ln(1-x)-x-x^2/2}{x}\right].
\end{aligned}
$$

This uses $\sum_{n=1}^{\infty}x^n/n=-\ln(1-x)$, subtracting the first two terms from the second sum. Hence

$$
S(1/2)=\frac58-\frac34\ln2.
$$

> Correction: The partial-fraction coefficient for $1/(n^2-1)$ is $1/2$, not $2$. The minus sign before the second fraction must also be retained after summation.

### Extracting Odd Terms and Summing Factorial Series {#odd-terms-factorial-series}

<!-- source: calculus:L399-L404 -->

If $H(t)=\sum_{n=0}^{\infty}c_nt^n$, its odd-power part, wherever both series converge, is

$$
\frac{H(t)-H(-t)}2
=\sum_{n=0}^{\infty}c_{2n+1}t^{2n+1}.
$$

This is the correct way to extract a series with its even powers missing. Do not confuse $H(-t)$ with simply negating every term of $H(t)$.

Example: Find

$$
S(x)=\sum_{n=1}^{\infty}\frac2{2n+1}
\left(\frac{x^2}2\right)^n.
$$

Set $t=x/\sqrt2$. First include the $n=0$ term to form the complete odd-power series, then subtract the added value $2$:

$$
S(x)=\frac{\sqrt2}{x}
\sum_{n=0}^{\infty}\frac2{2n+1}
\frac{x^{2n+1}}{(\sqrt2)^{2n+1}}-2.
$$

Using $H(t)=\sum_{n=1}^{\infty}2t^n/n=-2\ln(1-t)$ gives

$$
\begin{aligned}
S(x)&=\frac{\sqrt2}{x}\,
\frac{\displaystyle\sum_{n=1}^{\infty}\frac2n
\left(\frac{x}{\sqrt2}\right)^n
-\displaystyle\sum_{n=1}^{\infty}\frac2n
\left(\frac{-x}{\sqrt2}\right)^n}{2}-2\\
&=\frac{\sqrt2}{x}
\left[-\ln\left|1-\frac{x}{\sqrt2}\right|
+\ln\left|1+\frac{x}{\sqrt2}\right|\right]-2.
\end{aligned}
$$

This expression holds for $0\lt |x|\lt \sqrt2$; the original series supplies $S(0)=0$.

> Correction: After changing the starting index from $n=1$ to $n=0$, the original notes forgot to subtract the additional constant term $2$.

When $n!$ appears in a sum, think of the series for $e^x$. Negative integer factorials are undefined, so splitting a sum may require adjusting its starting index. For example,

$$
\begin{aligned}
\sum_{n=0}^{\infty}\frac{n+1}{n!}
&=\sum_{n=1}^{\infty}\frac1{(n-1)!}
+\sum_{n=0}^{\infty}\frac1{n!}\\
&=e+e=2e.
\end{aligned}
$$

The original $n=0$ term of the first part is $0/0!=0$, which cannot be rewritten as $1/(-1)!$. That part must therefore start at $n=1$.

## Spatial Geometry, Curves, Surfaces, and Multivariable Integrals {#space-geometry-multivariable-integration}

<!-- source: calculus:L405-L410 -->

> Notes on curves and surfaces, including applications and cautions for the Gauss, Green, and Stokes formulas, are recorded separately in Chapters 9 and 10 of the Li–Fan comprehensive review book. This section retains the spatial geometry and integration reminders present in this notebook.

Below, $\tau$ denotes a direction vector and $n$ a normal vector.

### Plane Equations and Distances {#planes-equations-distances}

<!-- source: calculus:L411-L414 -->

Write two parallel planes with the same normal-vector coefficients:

$$
Ax+By+Cz+D_1=0,\qquad Ax+By+Cz+D_2=0.
$$

Their distance is

$$
d=\frac{|D_1-D_2|}{\sqrt{A^2+B^2+C^2}}.
$$

**Do not omit the absolute value.** For a given plane and positive distance $d$, there are two parallel planes at distance $d$, one on each side.

Given three noncollinear points $A,B,C$, first find $\overrightarrow{AB}$ and $\overrightarrow{AC}$, then take

$$
n=\overrightarrow{AB}\times\overrightarrow{AC}.
$$

Substitute any of the points to write a point-normal equation of the plane. If the points are collinear, the cross product is zero and the plane is not uniquely determined.

### Lines in Space, Projections, and Distances {#space-lines-projections-distances}

<!-- source: calculus:L415-L421 -->

The following approaches are useful for finding a line in space:

1. Find a direction vector $\tau=(l,m,n)$ and a point $(x_0,y_0,z_0)$, then use a parametric equation. When all denominators are nonzero, the symmetric form is

$$
\frac{x-x_0}{l}=\frac{y-y_0}{m}=\frac{z-z_0}{n}.
$$

2. Express the line as the intersection of two planes. If the given line is the intersection of $P_1=0$ and $P_2=0$, use the pencil $P_1+\lambda P_2=0$ and substitute a known point or direction condition to determine $\lambda$. When necessary, separately check the plane $P_2=0$, which this parameterization omits.
3. For the common perpendicular to two nonparallel lines, first set $\tau=\tau_1\times\tau_2$, then construct auxiliary planes with normals

$$
n_1=\tau\times\tau_1,\qquad n_2=\tau\times\tau_2.
$$

Make each plane pass through a point on its respective given line. Their intersection is the common perpendicular.

> Correction: Two lines in space can be skew, so they do not necessarily determine a plane. Here, each given line and the common-perpendicular direction determine their own auxiliary plane. Parallel lines require a separate construction because the direction-vector cross product vanishes.

To find the orthogonal projection of a line onto a plane, determine its direction $\tau$, then use the pencil of planes containing the original line to construct an auxiliary plane containing the target plane's normal direction $n$. After determining the parameter, intersect the auxiliary plane with the target plane. If the original line is perpendicular to the plane, the projection is a point rather than a line. Related exercises: `Zhang_base(Exercise 17.5)` and `880_base(4.3.4)`.

For two lines $L_1,L_2$ with nonparallel directions, construct a pencil of planes through $L_2$ and impose parallelism to the direction of $L_1$ to determine an auxiliary plane $\pi$. The distance from any point of $L_1$ to $\pi$ is the distance between the lines. Intersecting lines have distance zero; parallel lines require the corresponding point-to-line distance method.

### Scalar Line Integrals Along a Segment {#line-segment-scalar-line-integrals}

<!-- source: calculus:L422-L424 -->

For endpoints $A,B$, take $\tau=B-A$ and parameterize in the chosen start-to-end direction:

$$
r(t)=A+t\tau=(x(t),y(t),z(t)),\qquad 0\le t\le1.
$$

The corresponding symmetric form is $(x-x_0)/l=(y-y_0)/m=(z-z_0)/n=t$, but use the parametric equations directly when a direction component is zero. Since $ds=\|\tau\|\,dt$,

$$
\int_L f(x,y,z)\,ds
=\int_0^1 f(x(t),y(t),z(t))\,\|\tau\|\,dt.
$$

Keeping track of the parameter direction prevents endpoint mistakes. This scalar line integral is itself independent of orientation, whereas an oriented line integral requires particular attention to direction. Related exercise: `880_base(2.6)`.

### Coordinate Shifts for Surfaces of Revolution {#rotation-surfaces-coordinate-shifts}

<!-- source: calculus:L425-L429 -->

When revolving a curve about a coordinate axis, eliminate variables using its equations. For example, revolving a curve given by $x=f(z)$ and $y=g(z)$ about the $z$-axis gives

$$
x^2+y^2=f(z)^2+g(z)^2.
$$

If the axis lies elsewhere, such as the spatial line $x=2,y=3$, first translate it to a coordinate axis, derive the surface equation, and reverse the translation. Check the signs of left/right and up/down shifts with an explicit coordinate substitution. The original notes also mention rotation about $x=1$: this is an axis in two dimensions, but in three dimensions this single equation describes a plane, so another condition is needed to specify an axis.

Example: Revolve the line

$$
\frac{x-1}{3}=\frac{y-2}{4}=\frac{z+1}{1}
$$

once around the axis $x=2,y=3$. Set $X=x-2$, $Y=y-3$, and $Z=z$. The line becomes

$$
\frac{X+1}{3}=\frac{Y+1}{4}=\frac{Z+1}{1},
\qquad
\begin{cases}
X=3Z+2,\\
Y=4Z+3.
\end{cases}
$$

Revolution about the new $Z$-axis gives

$$
X^2+Y^2=(3Z+2)^2+(4Z+3)^2
=25Z^2+36Z+13.
$$

Reversing the translation yields

$$
(x-2)^2+(y-3)^2=25z^2+36z+13.
$$

### Revolving Intersecting Lines and Spherical-Coordinate Bounds {#rotating-intersecting-lines-spherical-bounds}

<!-- source: calculus:L430-L435 -->

When a line intersecting the axis revolves about it, use the constant-angle method with vectors originating at the intersection. If $r$ points from the intersection to a point on the generating line and $\tau$ is the axis direction, then

$$
\cos\theta=\frac{|r\cdot\tau|}{\|r\|\,\|\tau\|}.
$$

> Assumption reminder: The position vector must originate at the intersection of the two lines. A constant angle between line directions alone does not justify applying this method to an arbitrary skew generating line.

Example: Let $\Sigma$ be the surface generated by revolving $x=0,y=0$ around $x=y=z$. The lines meet at the origin, the axis direction is $\tau=(1,1,1)$, and a vector on the original line is $r_0=(0,0,1)$. Every nonzero point on the surface therefore satisfies

$$
\cos\theta=\frac1{\sqrt3}
=\frac{|x+y+z|}{\sqrt3\sqrt{x^2+y^2+z^2}}.
$$

Squaring and simplifying gives the surface equation, including its vertex at the origin:

$$
xy+xz+yz=0.
$$

Finally, keep checking the integration region when evaluating multivariable integrals. Pay particular attention to the spherical-coordinate angle $\phi$: a conical constraint or a sphere centered away from the origin affects the angular bounds. Do not automatically reuse the bounds for a sphere centered at the origin.

## Linear Algebra {#linear-algebra}

<!-- source: algebra:L1-L2 -->

> When a new problem reminds me of an earlier one, a gap in the solution often reveals a gap in my understanding of the tools. Solving the new problem is also a chance to revise that earlier understanding.

This section is organized from my linear algebra notes. I use $E$ for the identity matrix, $O$ for the zero matrix, $r(A)$ for rank, and $A^*$ for the adjugate. Corrections to the original shorthand, including rank versus nullity, transpose placement, and eigenvalue shifts, are identified where they occur.

### Elementary Operations, Rank, and Linear Systems {#algebra-rank-and-systems}

<!-- source: algebra:L3-L25 -->

**Invertible matrices and elementary matrices.** Every invertible matrix is a product of elementary matrices, and every such product is invertible. In particular, if $P_1$ and $P_2$ are invertible, then

$$
(P_1P_2)^{-1}=P_2^{-1}P_1^{-1}.
$$

Two matrices of the same dimensions are equivalent if and only if one can be obtained from the other by finitely many elementary row and column operations, equivalently if they have the same rank. These operations include swaps, multiplication by a nonzero scalar, and adding a multiple of another row or column; swaps alone are not enough.

**When $AB=O$ appears, think about rank and null spaces.** If $A$ is $m\times n$ and $B$ is $n\times p$, every column of $B$ lies in the null space of $A$. Hence

$$
\operatorname{Col}(B)\subseteq\ker A,
\qquad r(A)+r(B)\leq n.
$$

The original example $A_{m\times n}B_{n\times m}=O$ is a special case; when the dimensions permit, $B$ can also be $A$. If $r(B)=r$, we can select $r$ linearly independent columns of $B$ as solutions of $Ax=0$. Only when $A$ is square can these nonzero solutions also be called eigenvectors for eigenvalue $0$. Exercise reference: $1k_{\mathrm{base}}$, 4.7.

**How many linearly independent solutions can an inhomogeneous system have?** Suppose $Ax=b$ is consistent, $b\neq0$, and $A$ has $n$ columns. Write its nullity as

$$
d=n-r(A).
$$

If $\xi_1,\ldots,\xi_d$ form a basis of the solutions of $Ax=0$ and $\eta$ is one particular solution of $Ax=b$, the general solution is

$$
x=\eta+\sum_{i=1}^{d}k_i\xi_i.
$$

The $d+1$ solutions $\eta,\eta+\xi_1,\ldots,\eta+\xi_d$ are linearly independent, and no larger independent set of solutions is possible. To prove independence, apply $A$ to a linear combination of these vectors: the sum of its coefficients must vanish. Independence of the $\xi_i$ then makes every coefficient vanish. The original note uses two independent homogeneous solutions, namely $d=2$, giving three independent inhomogeneous solutions.

> Correction: the relevant quantity is nullity $d$, not rank $r(A)$. When $b=0$, the maximum number of independent solutions is $d$.

**Column spaces, row spaces, and systems with the same solutions.** If the matrix equation $AX=B$ has a solution, every column of $B$ is a linear combination of columns of $A$. Consequently,

$$
A^Ty=0
\quad\Longleftrightarrow\quad
\begin{bmatrix}A^T\\B^T\end{bmatrix}y=0.
$$

If, in addition,

$$
r(A)=r(B)=r([A\ B]),
$$

then $A$ and $B$ have the same column space, so $A^Ty=0$ and $B^Ty=0$ also have the same solutions. Here $[A\ B]$ denotes horizontal concatenation, not multiplication.

For real matrices, two other useful identities are

$$
\ker(A^TA)=\ker A,
\qquad
\ker(AA^T)=\ker A^T.
$$

For example, $A^TAx=0$ implies $x^TA^TAx=\|Ax\|^2=0$, and therefore $Ax=0$.

> Correction: the original statement that $Ax=0$ and $AA^Tx=0$ have the same solutions is generally false. The system corresponding to $Ax=0$ is $A^TAx=0$.

**Choose free variables according to pivot positions.** After row reduction, nonpivot columns determine the free variables. As an example, suppose a real symmetric $3\times3$ matrix has every row sum equal to $k$. Then

$$
A\begin{bmatrix}1\\1\\1\end{bmatrix}
=k\begin{bmatrix}1\\1\\1\end{bmatrix}.
$$

Thus $k$ is an eigenvalue. For a symmetric matrix, saying that every column sum is $k$ supplies the same information. If the other two eigenvalues are both $1$ and $k\neq1$, orthogonality of eigenspaces for distinct eigenvalues gives the eigenspace for $\lambda=1$ as

$$
x_1+x_2+x_3=0.
$$

Take $x_2,x_3$ as free variables and use $(-1,1,0)^T$, $(-1,0,1)^T$ as a basis. The orthogonality condition uses an inner product, not a cross product. If $k=1$, all eigenvalues of the real symmetric matrix are $1$, hence $A=E$; this case must be handled separately.

### Rank-One Matrices, Eigenvalues, and Adjugates {#algebra-rank-one}

<!-- source: algebra:L26-L51 -->

**An outer product exposes rank-one structure.** For nonzero real column vectors $\alpha,\beta$, set

$$
A=\alpha\beta^T,\qquad
B=\beta\alpha^T,\qquad
c=\alpha^T\beta=\beta^T\alpha=(\alpha,\beta).
$$

Then $r(A)=r(B)=1$, and

$$
\operatorname{tr}(A)=\operatorname{tr}(B)=c,
\qquad A^2=cA,
\qquad A^m=c^{m-1}A\quad(m\geq2).
$$

Factoring a rank-one matrix into an outer product makes powers especially easy. Its eigenvalues are $c,0,\ldots,0$. When $c\neq0$, $c$ is a simple eigenvalue and $0$ has algebraic multiplicity $n-1$; when $c=0$, all eigenvalues are zero.

These facts also simplify determinants and inverses. For example,

$$
\det(kE+A)=k^{n-1}(k+c),
$$

and, when $k(k+c)\neq0$,

$$
(kE+A)^{-1}=\frac1kE-\frac{1}{k(k+c)}A.
$$

For $n\gt 1$, a rank-one matrix $A$ itself is singular, so $A^{-1}$ is unavailable. For a more general matrix $M$ involving an outer product, first calculate $M^2$ to find a low-degree polynomial identity, then use polynomial division to simplify matrix functions. If this yields $(M+iE)(M+jE)=kE$ with $k\neq0$, then $(M+iE)^{-1}=(M+jE)/k$.

**Diagonalizability depends on having enough independent eigenvectors.** For a nonzero rank-one matrix:

- If $c\neq0$, $\alpha$ is an eigenvector for $c$, while $\dim\ker A=n-1$. Together these give $n$ independent eigenvectors, so $A$ is diagonalizable.
- If $c=0$, every eigenvector lies in the null space of dimension $n-1$, so $A$ is not diagonalizable.

**Reconstructing a symmetric rank-one matrix.** If a real symmetric rank-one matrix has one nonzero eigenvalue $\lambda$ with unit eigenvector $\xi$, then

$$
A=\lambda\xi\xi^T.
$$

There is no need to complete an orthogonal eigenvector matrix and multiply out $Q\Lambda Q^T$. The original note placed the transpose on the wrong side: $\xi^T\lambda\xi$ is a scalar, so it cannot reconstruct a matrix.

**A symmetric combination of two orthonormal vectors.** Suppose $\alpha^T\beta=0$ and $\|\alpha\|=\|\beta\|=1$, and define

$$
M=\alpha\beta^T+\beta\alpha^T.
$$

Then $M^T=M$, with

$$
M\alpha=\beta,\qquad M\beta=\alpha,
$$

$$
M(\alpha+\beta)=\alpha+\beta,
\qquad
M(\alpha-\beta)=-(\alpha-\beta).
$$

Its eigenvalues are therefore $1,-1$, with all remaining eigenvalues zero. The corresponding unit eigenvectors are $(\alpha+\beta)/\sqrt2$ and $(\alpha-\beta)/\sqrt2$; choose the others from their orthogonal complement. The bound $r(M)\leq2$ also identifies the remaining directions as null-space directions.

Another common structure is the projection matrix $P=\alpha\alpha^T$ for a unit vector $\alpha$, whose eigenvalues are $1,0,\ldots,0$. Substituting $\beta=\alpha$ into the preceding symmetric sum instead gives $2\alpha\alpha^T$, so the factor of $2$ must be retained.

**Adjugates provide another rank-one structure.** For $n\geq2$, if $r(A)=n-1$, then

$$
r(A^*)=1,\qquad A^*A=\det(A)E=O.
$$

Every column of $A$ is therefore a solution of $A^*x=0$, and we can choose $n-1$ independent columns as a basis of that solution space. In fact, $\operatorname{Col}(A)=\ker A^*$.

> Correction: $r(A)=n-1$ guarantees only $\dim\ker A=1$. It does not imply that zero has algebraic multiplicity one or that $A$ is diagonalizable. With diagonalizability as an additional assumption, the $n-1$ eigenvectors belonging to nonzero eigenvalues can describe this space.

**Example 1: add the identity to reveal an outer product.** Let

$$
A=\begin{bmatrix}
0&2&3&4\\
2&3&6&8\\
3&6&8&12\\
4&8&12&15
\end{bmatrix}.
$$

Set $B=A+E$. Then $B=(1,2,3,4)^T(1,2,3,4)$ has rank one and trace $30$. Its eigenvalues are $30,0,0,0$, so the eigenvalues of $A$ are $29,-1,-1,-1$, giving

$$
\det A=-29.
$$

**Example 2: diagonal entries $a$, all other entries $1$.** Let $J=\mathbf1\mathbf1^T$ be the $n\times n$ all-ones matrix. Then

$$
A=(a-1)E+J.
$$

Since $J$ has eigenvalues $n,0,\ldots,0$, the eigenvalues of $A$ are $a+n-1$ and $a-1$, the latter with multiplicity $n-1$. The first corresponds to $\mathbf1$; the eigenspace for the second is $\{x:\sum_i x_i=0\}$. This is more direct than repeatedly adding and subtracting rows in the characteristic determinant.

> Correction: the shift from the eigenvalues of $J$ to those of $A$ is $a-1$. The original expression $a+1+\lambda_i$ should read $a-1+\lambda_i$.

### Bases, Coordinates, and Equivalent Vector Families {#algebra-bases-and-coordinates}

<!-- source: algebra:L52-L75 -->

**Coordinates are expansion coefficients in a chosen basis.** If $\xi_1,\ldots,\xi_n$ form a basis and

$$
\alpha=a_1\xi_1+\cdots+a_n\xi_n,
$$

then $[\alpha]_{\xi}=(a_1,\ldots,a_n)^T$. For example, to find the coordinates of $\beta$ in a three-dimensional basis $\alpha_1,\alpha_2,\alpha_3$, introduce $x_1,x_2,x_3$ and solve

$$
\beta=x_1\alpha_1+x_2\alpha_2+x_3\alpha_3.
$$

To find vectors that have the same coordinates in two bases $\alpha_i$ and $\beta_i$, let $x$ denote the common coordinates. Then

$$
\sum_i x_i\alpha_i=\sum_i x_i\beta_i,
\qquad
\sum_i x_i(\alpha_i-\beta_i)=0.
$$

Solve for the common coordinates first, then substitute into either basis to obtain the vector itself.

**Use column vectors consistently to avoid transpose confusion.** Let the basis matrices be $S=(\xi_1,\ldots,\xi_n)$ and $T=(\eta_1,\ldots,\eta_n)$, and define

$$
T=SC.
$$

Each column of $C=S^{-1}T$ gives a new basis vector in old-basis coordinates. If $S=PA$ and $T=PB$, then $C=A^{-1}B$. For every vector $v$,

$$
[v]_S=C[v]_T.
$$

If coordinates are written as row vectors instead, the corresponding relation is $[v]_S^T=[v]_T^TC^T$. Fix this convention before converting bases or coordinates.

**Example: read the change-of-basis matrix directly.** Suppose $\beta_1,\beta_2,\beta_3$ form a basis. For the bases $(\beta_1,2\beta_2,3\beta_3)$ and $(\beta_1-\beta_2,\beta_2+\beta_3,\beta_3-\beta_1)$,

$$
\begin{bmatrix}
\beta_1-\beta_2&\beta_2+\beta_3&\beta_3-\beta_1
\end{bmatrix}
= \begin{bmatrix}\beta_1&2\beta_2&3\beta_3\end{bmatrix}
\begin{bmatrix}
1&0&-1\\
-\frac12&\frac12&0\\
0&\frac13&\frac13
\end{bmatrix}.
$$

The coefficient matrix on the right is $C$. When the expansion of each new basis vector is immediately visible, this approach saves the inversion and transpose bookkeeping of first factoring separate coefficient matrices.

**Equivalent vector families and homogeneous systems.** Suppose $\alpha_1,\ldots,\alpha_n$ are independent and another family of exactly $n$ vectors, $\beta_1,\ldots,\beta_n$, can express every $\alpha_i$. Then

$$
\operatorname{span}(\alpha)\subseteq\operatorname{span}(\beta),
\qquad n=r(\alpha)\leq r(\beta)\leq n.
$$

The two families therefore span the same space and can express each other. This sufficient condition relies on equal family sizes and independence of the first family.

For matrices with the same number of columns, equivalence of their row-vector families is equivalent to equality of the solution sets of their homogeneous systems. In terms of rank,

$$
\ker A=\ker B
\quad\Longleftrightarrow\quad
\operatorname{Row}(A)=\operatorname{Row}(B)
\quad\Longleftrightarrow\quad
r\!\begin{bmatrix}A\\B\end{bmatrix}=r(A)=r(B).
$$

> Correction: the condition here is equality of row spaces. General matrix equivalence or equal rank alone does not imply equal solution sets, because column operations change the coordinates of the unknowns.

**Left multiplication by a full-column-rank matrix preserves the null space.** If $Q$ is $m\times n$ with $r(Q)=n$ and $P$ is $n\times p$, then

$$
QPx=0\quad\Longleftrightarrow\quad Px=0,
$$

because $Qy=0$ has only the zero solution. Thus the homogeneous systems for $QP$ and $P$ have the same solutions.

**Example: an invertible factor proves equivalence of transposed systems.** Suppose $\alpha^T\alpha=2$ and

$$
A(E-2\alpha\alpha^T)=B.
$$

The eigenvalues of $\alpha\alpha^T$ are $2,0,\ldots,0$, so those of $C=E-2\alpha\alpha^T$ are $-3,1,\ldots,1$. Hence $C$ is invertible. Since $B=AC$ and $A=BC^{-1}$, the column spaces of $A$ and $B$ coincide, and $A^Tx=0$ and $B^Tx=0$ have the same solutions.

### Quadratic Forms, Congruence, and Diagonal Forms {#algebra-quadratic-forms}

<!-- source: algebra:L76-L81 -->

**Start with a symmetric coefficient matrix.** For every real matrix $M$,

$$
x^TMx=x^T\frac{M+M^T}{2}x.
$$

We may therefore always use a real symmetric matrix $A$ when studying a quadratic form. An invertible substitution $x=Cy$ produces the congruence transformation $A\mapsto C^TAC$. Two nonsymmetric matrices can be congruent, but a symmetric and a nonsymmetric matrix cannot be related by an invertible congruence transformation.

**Diagonal standard form and inertia normal form.** Completing squares, orthogonal transformations, and paired elementary row and column operations can all diagonalize a quadratic form. The original note leaves the elementary-congruence method for later expansion alongside its corresponding problem in the 1000-question collection.

A real symmetric matrix satisfies $Q^TAQ=\Lambda$. Under the orthogonal substitution $x=Qy$, the form becomes $\sum_i\lambda_i y_i^2$. An additional invertible rescaling changes each nonzero coefficient to $1$ or $-1$, giving the inertia normal form. An orthogonal transformation alone can produce this normal form only if the original eigenvalues already belong to $\{1,-1,0\}$.

> Correction: a general invertible substitution need not be orthogonal; it guarantees congruence, not similarity. However, being nonorthogonal does not by itself prove that the resulting matrices are not similar, nor does it restrict the available method to completing squares. Elementary congruence operations remain available.

**A quadratic form equal to zero is not always a homogeneous linear system.** If $A$ is real symmetric, then $f(A)$ shares its orthonormal eigenvectors, with eigenvalues $f(\lambda_i)$. The dimension of the solution space of $f(A)x=0$ equals the number of zero values $f(\lambda_i)$, counted with multiplicity.

For the quadratic equation $x^Tf(A)x=0$, the solution set equals that null space only when $f(A)$ is positive or negative semidefinite. If positive and negative eigenvalues coexist, nonzero terms can cancel. For example, $x_1^2-x_2^2=0$ has nonzero solutions even though its coefficient matrix has no zero eigenvalue. Under the appropriate semidefiniteness condition, the original note's “number of solutions” should be understood as the dimension of the solution space.

### Extrema of Quadratic Forms: Ordinary and Generalized Rayleigh Quotients {#algebra-rayleigh-quotients}

<!-- source: algebra:L82-L96 -->

**The ordinary Rayleigh quotient.** For real symmetric $A$, consider

$$
R(x)=\frac{x^TAx}{x^Tx},\qquad x\neq0.
$$

Choose $Q^TAQ=\Lambda$ and set $x=Qy$. Then

$$
R(x)=\frac{\sum_i\lambda_i y_i^2}{\sum_i y_i^2}.
$$

This is a weighted average of eigenvalues, so

$$
\min R=\lambda_{\min}(A),\qquad
\max R=\lambda_{\max}(A).
$$

Any nonzero eigenvector for the corresponding extreme eigenvalue attains the extremum. Alternatively, select the appropriate coordinate unit vector in $y$ coordinates and transform back with $x=Qy$.

> Correction: the extrema are the smallest and largest individual eigenvalues, not a sum of eigenvalues. Keep the decomposition consistent as $A=Q\Lambda Q^T$, $x=Qy$.

**Example: symmetrize first, then recognize rank one.** Let

$$
f(x)=x^T
\begin{bmatrix}1&0&6\\4&4&4\\0&8&9\end{bmatrix}x.
$$

Find an orthogonal diagonalization and the maximum of $f(x)/(x_1^2+x_2^2+x_3^2)$. Symmetrization gives

$$
A=\begin{bmatrix}1&2&3\\2&4&6\\3&6&9\end{bmatrix}
=uu^T,\qquad u=(1,2,3)^T.
$$

The eigenvalues are $14,0,0$. To avoid a separate Gram–Schmidt calculation, choose mutually orthogonal eigenvectors directly:

$$
\xi_1=(1,2,3)^T,\qquad
\xi_2=(0,-3,2)^T,\qquad
\xi_3=(-13,2,3)^T.
$$

The third follows the original note's choice $(k,2,3)^T$: orthogonality to $\xi_1$ gives $k=-13$. Normalize the vectors to obtain

$$
Q=\begin{bmatrix}
\xi_1/\sqrt{14}&\xi_2/\sqrt{13}&\xi_3/\sqrt{182}
\end{bmatrix},
\qquad Q^TAQ=\operatorname{diag}(14,0,0).
$$

With $x=Qy$, the form becomes $f=14y_1^2$, and the quotient has maximum $14$. One maximizing point is $y=(1,0,0)^T$, equivalently $x=(1,2,3)^T/\sqrt{14}$. Every nonzero multiple of $u$ also maximizes it.

**For a generalized quotient, first turn the denominator into a sum of squares.** Consider

$$
R(x)=\frac{x^TAx}{x^TBx}.
$$

If $A$ is real symmetric and $B$ is positive definite, choose an invertible $D$ with $B=D^TD$ and set $y=Dx$. This gives

$$
R(x)=\frac{y^TMy}{y^Ty},
\qquad M=D^{-T}AD^{-1}.
$$

Next orthogonally diagonalize $M$ as $Q^TMQ=\Lambda$ and set $y=Qz$. The result is an ordinary Rayleigh quotient, and extremizing points return to the original coordinates through $x=D^{-1}Qz$.

Completing squares or elementary congruence operations can supply $D$ in the first step. Orthogonal diagonalization alone usually makes $B$ diagonal; an additional rescaling is needed to make the denominator $y^Ty$. Together the two substitutions put the denominator $g$ into its positive inertia normal form and the numerator $f$ into a diagonal standard form.

**Example: completing squares is easier than finding the denominator's eigenvalues.** Let

$$
f(x_1,x_2)=x_1^2-4x_1x_2+4x_2^2,
\qquad
B=\begin{bmatrix}1&-1\\-1&2\end{bmatrix}.
$$

First determine whether an invertible $D$ exists with $B=D^TD$. Since

$$
g(x)=x^TBx=(x_1-x_2)^2+x_2^2,
$$

we may take

$$
D=\begin{bmatrix}1&-1\\0&1\end{bmatrix},
\qquad D^{-1}=\begin{bmatrix}1&1\\0&1\end{bmatrix}.
$$

The numerator has symmetric matrix $A=\begin{bmatrix}1&-2\\-2&4\end{bmatrix}$. After $x=D^{-1}y$,

$$
M=D^{-T}AD^{-1}=\begin{bmatrix}1&-1\\-1&1\end{bmatrix}.
$$

This rank-one matrix has eigenvalues $2,0$. Take the columns of $Q$ as $(1,-1)^T/\sqrt2$ and $(1,1)^T/\sqrt2$, and then set $y=Qz$. The quotient becomes

$$
\frac{f(x)}{g(x)}=\frac{2z_1^2}{z_1^2+z_2^2}.
$$

Its maximum is $2$, attained at $z=(1,0)^T$, corresponding to $x=D^{-1}Qz=(0,-1/\sqrt2)^T$. Equivalently, every $x=(0,t)^T$ with $t\neq0$ is a maximizing point.

### Positive Definiteness, Spectral Decomposition, and Matrix Square Roots {#algebra-positive-definiteness}

<!-- source: algebra:L97-L106 -->

**Sums of squares and invertibility.** Suppose

$$
f(x)=(a_1^Tx)^2+\cdots+(a_m^Tx)^2=\|Cx\|^2,
$$

where the rows of $C$ contain the coefficients of the linear forms. Then $f$ is positive definite if and only if $Cx=0$ has only the zero solution, equivalently if $C$ has full column rank. When $C$ is square, this is exactly invertibility of $C$.

**Convert quadratic inequalities into definiteness conditions.** For real symmetric $A$, suppose that every nonzero $x$ satisfies

$$
|x^TAx|\lt x^Tx.
$$

This is equivalent to $-x^Tx\lt x^TAx\lt x^Tx$, or to positive definiteness of both $E+A$ and $E-A$. Equivalently, every eigenvalue of $A$ lies in $(-1,1)$.

**Example: an inequality involving the adjugate.** A real symmetric $3\times3$ matrix $A$ has eigenvalues $\lambda_1,\lambda_2,\lambda_3$. Find the smallest $a$ such that

$$
|x^TA^*x-x^TAx|\leq a\,x^Tx
$$

holds for all $x$. Set $H=A^*-A$. The inequality is equivalent to positive semidefiniteness of both $aE+H$ and $aE-H$. The matrices $A^*$ and $A$ share orthonormal eigenvectors, and the eigenvalue of $H$ in the $i$th eigenvector direction is

$$
\mu_i=\prod_{j\neq i}\lambda_j-\lambda_i.
$$

Therefore,

$$
a_{\min}=\max_i|\mu_i|.
$$

For $\lambda_1,\lambda_2,\lambda_3=2,3,4$, the values are $\mu_i=10,5,2$, so $a_{\min}=10$.

> Correction: the original inequality is non-strict, so the associated matrices must be positive semidefinite, not necessarily positive definite. Eigenvalues may be subtracted only when they correspond to the same eigenvector; arbitrary pairing is invalid.

**Spectral decomposition can eliminate unnecessary matrix products.** If real symmetric $A$ has orthonormal eigenvectors $u_1,\ldots,u_n$, then

$$
A=\sum_{i=1}^n\lambda_i u_i u_i^T,
\qquad
\sum_{i=1}^n u_i u_i^T=E.
$$

These identities follow directly from $A=Q\Lambda Q^T$ and $QQ^T=E$. The original exercise reference is $880_{\mathrm{comp}}$, 14-3.13.

**Example: finding a positive definite square root.** Let

$$
A=\begin{bmatrix}
0&1&-1\\1&0&-1\\-1&-1&0
\end{bmatrix},
\qquad B^2=A+2E,
$$

where $B$ is positive definite. The eigenvalues of $A$ are $2,-1,-1$, so those of $A+2E$ are $4,1,1$. Its positive definite square root has eigenvalues $2,1,1$ with the same eigenvectors. Consequently,

$$
B=2u_1u_1^T+u_2u_2^T+u_3u_3^T=E+u_1u_1^T.
$$

For eigenvalue $2$ of $A$, take $u_1=(1,1,-1)^T/\sqrt3$. Thus

$$
B=\frac13\begin{bmatrix}
4&1&-1\\1&4&-1\\-1&-1&4
\end{bmatrix}.
$$

This also explains why the other two unit eigenvectors need not be calculated: their projection matrices already sum to $E-u_1u_1^T$.

### Ranks of Block Matrices: Which Operations Preserve Equivalence? {#algebra-block-rank}

<!-- source: algebra:L107-L110 -->

The final exercise retains the original handwritten working. Let $A,B$ be $n\times n$ matrices, and denote the ranks of three block matrices by

$$
r_1=r\!\begin{bmatrix}O&A\\B&E\end{bmatrix},\qquad
r_2=r\!\begin{bmatrix}A&B\\O&E\end{bmatrix},\qquad
r_3=r\!\begin{bmatrix}A&AB\\E&B\end{bmatrix}.
$$

Invertible block row and column additions expose diagonal blocks, giving

$$
r_1=n+r(AB),\qquad
r_2=n+r(A),\qquad
r_3=n.
$$

For the first identity, subtract $A$ times the second block row from the first, then subtract the second block column multiplied on the right by $B$ from the first block column. The result is $\operatorname{diag}(-AB,E)$. For the third, subtract $A$ times the second block row from the first, then subtract the first block column multiplied on the right by $B$ from the second block column. The result is $\begin{bmatrix}O&O\\E&O\end{bmatrix}$. Since $r(AB)\leq r(A)$ and $r(AB)\leq r(B)$, the required ordering is

$$
r_2\geq r_1\geq r_3.
$$

{{< fig src="figures/block-matrix-rank.png" alt="Block-matrix ranks: the original exercise and an invalid step involving a possibly singular transformation" caption="Block-matrix ranks: the original exercise and an invalid step involving a possibly singular transformation" >}}

**The last red calculation contains the mistake to avoid.** Multiplying one block row directly by a possibly singular $A$ does not necessarily preserve rank. This amounts to left multiplication by $\operatorname{diag}(E,A)$, whose rank is $n+r(A)$ and need not be $2n$. Adding $A$ times one block row to another, by contrast, uses an invertible block triangular matrix and is valid. The original notes leave a fuller account of these block operations for later study.
