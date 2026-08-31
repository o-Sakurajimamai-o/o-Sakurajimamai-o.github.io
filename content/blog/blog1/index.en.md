---
title: "Hello, World"
date: 2026-08-26
description: "The first post, and a test of everything"
---

This is the first post. Let me test a few things.

Inline math: $e^{i\pi} + 1 = 0$.

Display math:

$$\int_0^1 x^2 \, dx = \frac{1}{3}$$

$$
\begin{aligned}
a &= b + c \\
d &= e \, f
\end{aligned}
$$

And some code:

```python
def hello(name: str) -> str:
    return f"Hello, {name}"
```

That's it.

## First Section

This section tests the figure component and the table of contents. The image below should
appear inside a rounded, bordered box. Click it to open the lightbox, then click outside
the image or press ESC to close.

{{< fig src="figures/test.png" caption="A test image" >}}

### A Subsection

Subsection content. In the sidebar it should be indented one level below its parent. As you
scroll, the active entry follows the section you are currently reading, marked with an accent
coloured rule on its left edge.

One more paragraph to give the page some height, so that scrolling actually has something to
show. A static site generator compiles Markdown into HTML: no database, no backend, and
deployment is just copying files onto a CDN.

## Second Section

This section tests the boxed table component.

{{< figbox caption="Effective context length" >}}
| Model | Advertised | Measured |
|---|---|---|
| A | 1M | 64K |
| B | 500K | 64K |
| C | 200K | 96K |
{{< /figbox >}}

English text is left aligned rather than justified. Justification stretches the spaces
between words, and on a short measure that produces distractingly large gaps.

## Third Section

The last section, used to verify that scrolling to the very bottom correctly highlights the
final entry in the table of contents.

The end.
