# Complex sequences based on initial conditions and rules

> **Source:** [math.stackexchange.com/q/5125565](https://math.stackexchange.com/q/5125565)  ·  **archived (deleted from MSE)**
> **Tags:** `complex-numbers`
> **Asked:** 2026-02-21 05:11:22Z

## Question

I was playing with $n^{- s}$ and testing different ways to generate a value $r_{n}$ that captures some information about how counting evolves in 2D or 3D:

Consider $n \in \mathbb{N}$ and $s,b_{n},q_{n} \in \mathbb{C}$ with the following restriction:

$$1 \leq n \leq N$$

$r_{n}$ is constructed with $x\ \text{and}\ y \in \mathbb{N}$

$$r_{n} = \sum\limits_{xy = n}^{N}b_{x}q_{y}$$

This seems to hold for large N on many occasions, with both real and complex sequences:

$$\sum\limits_{n = 1}^{N}\frac{r_{n}}{n^{s}} = \sum\limits_{n = 1}^{N}\frac{b_{n}}{n^{s}}\sum\limits_{n = 1}^{N}\frac{q_{n}}{n^{s}}$$

Does this hold all the way up to $N\rightarrow\infty$? What are the restrictions needed for $b_{n}$ and $q_{n}$?

If it does, can this type of thing be extended to use $x,y \in \mathbb{Q}$?
