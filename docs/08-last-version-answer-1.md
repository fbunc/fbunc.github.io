# First answer attempt — mod 10 vs mod 6

> First answer attempt to the *Last Version / Questions About Primes* question (prime atoms).
> Drafted 2026-04-20. Focus: what residue class is more fruitful to count factorizations into — mod 10 vs mod 6.

---

The last month I spent a lot of time trying to answer the obvious parts of the question. I realized that trying to hunt primes blindly or leveraging obvious correlations in 2D is not that interesting if you don't really understand what the act of "factorization" means when you are counting. It's a code that organizes numbers according to different properties.

So my answer will focus on addressing the main points I could discover to answer the question of what residue class could be more fruitful, and here I will compare mod 6 and mod 10.

Mod 10 will organize, as expected, the numbers based on their last digit and makes the golden ratio appear, and mod 6 makes the "complex golden ratio" appear.

<https://math.stackexchange.com/questions/1851698/imaginary-golden-ratio>

But first, we do factorization from scratch:

Consider a number $n$ with its prime factorization given by a dual representation as follows:

Using $\omega = e^{2\pi i / T_o}$ based on the values (periods $T_o = 10$ and $T_o = 6$). Primes $p$ always correspond to single vectors $\omega^p$, so $|s_p| = |\dot{s}_p| = |\omega^p| = 1$.

* $s_n$: Sum over prime factors with multiplicity ($b_k$).

$$n = \prod_{k=1}^{M_n} b_k \implies s_n = \sum_{k=1}^{M_n} \omega^{b_k}$$

* $\dot{s}_n$: Sum over distinct prime factors ($d_k$).

$$[ d_1, d_2, \dots, d_{m_n}] \text{ are the distinct prime factors} \implies \dot{s}_n = \sum_{k=1}^{m_n} \omega^{d_k}$$

Based on these two simple representations of the factorization process, some conclusions can be derived that, in my ignorance about deep math as an engineer, I find really beautiful because I find them meaningful.

Example for $n=60$:

$$ 60 = 2^2 \cdot 3^1 \cdot 5^1 $$

* $b_k = [2, 2, 3, 5] \implies M_{60} = 4$
* $d_k = [2, 3, 5] \implies m_{60} = 3$
* $e_k = [2, 1, 1] \implies \sum e_k = 2+1+1 = 4 = M_{60}$

$$ s_{60} = \omega^2 + \omega^2 + \omega^3 + \omega^5, \quad \dot{s}_{60} = \omega^2 + \omega^3 + \omega^5, \quad \ddot{s}_{60} =\omega^4 + \omega^3 + \omega^5 $$

When you see a lot of $\sqrt{5}$ and a lot of $618$ in the decimals of your tables the golden ratio is always around, so:

## The modulus of $s_n$ sieves primes in 2D naturally by simple factorization

| $n$ | Prime | Factorization | $s_n$ | $\dot{s}_n$ |
|----|----|--------------|------|-------------|
| 1 |  | $1$ | $0$ | $0$ |
| 2 | ✓ | $2$ | $\omega^{2}$ | $\omega^{2}$ |
| 3 | ✓ | $3$ | $\omega^{3}$ | $\omega^{3}$ |
| 4 |  | $2^{2}$ | $2\omega^{2}$ | $\omega^{2}$ |
| 5 | ✓ | $5$ | $\omega^{5}$ | $\omega^{5}$ |
| 6 |  | $2 \cdot 3$ | $\omega^{2} + \omega^{3}$ | $\omega^{2} + \omega^{3}$ |
| 7 | ✓ | $7$ | $\omega^{7}$ | $\omega^{7}$ |

### Mod 10

| $n$ | $\operatorname{Re}(s_n)$ | $\operatorname{Im}(s_n)$ | $\operatorname{Re}(\dot{s}_n)$ | $\operatorname{Im}(\dot{s}_n)$ |
|----:|:--------------------------|:--------------------------|:-------------------------------|:-------------------------------|
| 1 | $0$ | $0$ | $0$ | $0$ |
| 2 | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 3 | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 4 | $\varphi - 1$ | $\sqrt{2+\varphi}$ | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 5 | $-1$ | $0$ | $-1$ | $0$ |
| 6 | $0$ | $\sqrt{2+\varphi}$ | $0$ | $\sqrt{2+\varphi}$ |
| 7 | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ |
| 8 | $\frac{3\varphi - 3}{2}$ | $\frac{3\sqrt{2+\varphi}}{2}$ | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 9 | $-\varphi + 1$ | $\sqrt{2+\varphi}$ | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 10 | $\frac{\varphi - 3}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $\frac{\varphi - 3}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 11 | $\frac{\varphi}{2}$ | $\frac{\sqrt{3-\varphi}}{2}$ | $\frac{\varphi}{2}$ | $\frac{\sqrt{3-\varphi}}{2}$ |
| 12 | $\frac{\varphi - 1}{2}$ | $\frac{3\sqrt{2+\varphi}}{2}$ | $0$ | $\sqrt{2+\varphi}$ |
| 13 | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 14 | $0$ | $0$ | $0$ | $0$ |
| 15 | $-\frac{\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $-\frac{\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 16 | $2\varphi - 2$ | $2\sqrt{2+\varphi}$ | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 17 | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ |
| 18 | $\frac{-\varphi + 1}{2}$ | $\frac{3\sqrt{2+\varphi}}{2}$ | $0$ | $\sqrt{2+\varphi}$ |
| 19 | $\frac{\varphi}{2}$ | $-\frac{\sqrt{3-\varphi}}{2}$ | $\frac{\varphi}{2}$ | $-\frac{\sqrt{3-\varphi}}{2}$ |
| 20 | $\varphi - 2$ | $\sqrt{2+\varphi}$ | $\frac{\varphi - 3}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 21 | $-\varphi + 1$ | $0$ | $-\varphi + 1$ | $0$ |
| 22 | $\frac{2\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi} + \sqrt{3-\varphi}}{2}$ | $\frac{2\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi} + \sqrt{3-\varphi}}{2}$ |
| 23 | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 24 | $\varphi - 1$ | $2\sqrt{2+\varphi}$ | $0$ | $\sqrt{2+\varphi}$ |
| 25 | $-2$ | $0$ | $-1$ | $0$ |
| 26 | $0$ | $\sqrt{2+\varphi}$ | $0$ | $\sqrt{2+\varphi}$ |
| 27 | $\frac{-3\varphi + 3}{2}$ | $\frac{3\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 28 | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ | $0$ | $0$ |
| 29 | $\frac{\varphi}{2}$ | $-\frac{\sqrt{3-\varphi}}{2}$ | $\frac{\varphi}{2}$ | $-\frac{\sqrt{3-\varphi}}{2}$ |
| 30 | $-1$ | $\sqrt{2+\varphi}$ | $-1$ | $\sqrt{2+\varphi}$ |
| 31 | $\frac{\varphi}{2}$ | $\frac{\sqrt{3-\varphi}}{2}$ | $\frac{\varphi}{2}$ | $\frac{\sqrt{3-\varphi}}{2}$ |
| 32 | $\frac{5\varphi - 5}{2}$ | $\frac{5\sqrt{2+\varphi}}{2}$ | $\frac{\varphi - 1}{2}$ | $\frac{\sqrt{2+\varphi}}{2}$ |
| 33 | $\frac{1}{2}$ | $\frac{\sqrt{2+\varphi} + \sqrt{3-\varphi}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{2+\varphi} + \sqrt{3-\varphi}}{2}$ |
| 34 | $0$ | $0$ | $0$ | $0$ |
| 35 | $-\frac{\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ | $-\frac{\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ |
| 36 | $0$ | $2\sqrt{2+\varphi}$ | $0$ | $\sqrt{2+\varphi}$ |
| 37 | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ | $\frac{-\varphi + 1}{2}$ | $-\frac{\sqrt{2+\varphi}}{2}$ |

### Plots Mod 10

#### Process

[![mod 10 raw process][1]][1]

#### mod 10 with primes highlighted using mod 6 color

* Red points are the unique prime positions in 2D for the set of possible last digits of $n$ organized by that last digit. Oscillations between the obvious $1,3,7,9$ are expected, but extra information might be given by the construction.

For primes $p \ge 5$, only four last digits are possible: $\{1, 3, 7, 9\}$.

Each maps to a fixed angle on the unit circle. Including small primes $2, 3, 5$:

[![mod 10 with primes highlighted with mod 6 color][2]][2]

#### The modulus of $s_n$ seems to be bounded by $2^a$

[![modulus of s_n][3]][3]

#### $\dot s_n$ mod 10

It seems to group twin primes.

[![real part of dot_s_n mod 10][4]][4]

## A1: So yes, there is some information in space encoded in the simple act of counting and factorizing

But the weeds are deep and I've been way over my head in the literature I read just to not reinvent the wheel, and now I'm a little more familiar with basic primes literature that explores primes over residue classes.

For example this paper explores formally the bias in the transitions of primes from 1 to -1 mod 6:

<https://www.pnas.org/doi/10.1073/pnas.1605366113>

## A2: Predictive patterns

If any construction really predicts something there are many ways to test it. I'm not that skilled or at the level that I'm interested on testing predictions, I'm more interested on thinking if we had the 2D representation of the plots without the labels how we can go back to the number $n$ it represents.

One would be try to predict this natural $s_n$ or its variations, and compare it with the simpler construction in the question. Or to predict the symbolic table representation as variations of multiples of $\sqrt{5}$ for the mod 10 case and of $\sqrt{3}$ for the mod 6 case.

#### Some conclusions mod 10

Symmetries: conjugate pairs and $\omega^5 = -1$.

The four prime angles (for $p \ge 5$) pair into conjugates:

- $\omega^1$ and $\omega^9$ are conjugates ($36° + 324° = 360°$)
- $\omega^3$ and $\omega^7$ are conjugates ($108° + 252° = 360°$)

Also: $\omega^5 = e^{i\pi} = -1$, so $\omega^{a+5} = -\omega^a$ (antipodal on the circle).
$s_n = \sum \omega^{b_k}$ and each $\omega^{b_k} = \omega^{b_k \bmod 10}$.

By the beautiful Dirichlet's theorem on primes in arithmetic progressions, for each residue class $a \in \{1, 3, 7, 9\}$, the density of primes $p \equiv a \pmod{10}$ is $\frac{1}{\varphi(10)} = \frac{1}{4}$.

Among prime last digits $\{1, 2, 3, 5, 7, 9\}$, the only viable antipodal pair seems to be $\{2, 7\}$.

Semiprimes 2 × (prime ending in 7) give $s_n = 0$:

`[14, 34, 74, 94, 134, 194, 214, 254, 274, 314, …]`

**Type B: 5th-root-of-unity cancellations**

Beyond pairwise $(2\leftrightarrow7)$, another way to get $s_n = 0$ uses the identity:

$$\sum_{k=0}^{4} (\omega^2)^k = 1 + \omega^2 + \omega^4 + \omega^6 + \omega^8 = 0$$

(sum of all 5th roots of unity, since $\omega^2$ is a primitive 5th root).

Multiplying by $\omega$:

$$\omega + \omega^3 + \omega^5 + \omega^7 + \omega^9 = 0$$

The odd residues mod 10 form a rotated set of 5th roots!

## Now the beauty — primes mod 6

| $n$ | $\operatorname{Re}(s_n)$ | $\operatorname{Im}(s_n)$ | $\operatorname{Re}(\dot{s}_n)$ | $\operatorname{Im}(\dot{s}_n)$ |
|----:|:--------------------------|:--------------------------|:-------------------------------|:-------------------------------|
| 1 | $0$ | $0$ | $0$ | $0$ |
| 2 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 3 | $-1$ | $0$ | $-1$ | $0$ |
| 4 | $-1$ | $\sqrt{3}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 5 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 6 | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 7 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 8 | $\frac{-3}{2}$ | $\frac{3\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 9 | $-2$ | $0$ | $-1$ | $0$ |
| 10 | $0$ | $0$ | $0$ | $0$ |
| 11 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 12 | $-2$ | $\sqrt{3}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 13 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 14 | $0$ | $\sqrt{3}$ | $0$ | $\sqrt{3}$ |
| 15 | $\frac{-1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 16 | $-2$ | $2\sqrt{3}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 17 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 18 | $\frac{-5}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 19 | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 20 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $0$ | $0$ |
| 21 | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{-1}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 22 | $0$ | $0$ | $0$ | $0$ |
| 23 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 24 | $\frac{-5}{2}$ | $\frac{3\sqrt{3}}{2}$ | $\frac{-3}{2}$ | $\frac{\sqrt{3}}{2}$ |
| 25 | $1$ | $-\sqrt{3}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 26 | $0$ | $\sqrt{3}$ | $0$ | $\sqrt{3}$ |
| 27 | $-3$ | $0$ | $-1$ | $0$ |
| 28 | $\frac{-1}{2}$ | $\frac{3\sqrt{3}}{2}$ | $0$ | $\sqrt{3}$ |
| 29 | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $-\frac{\sqrt{3}}{2}$ |
| 30 | $-1$ | $0$ | $-1$ | $0$ |

### Modulus of $s_n$ mod 6

This one is related to many OEIS sequences I will explain later.

[![Modulus of s_n mod 6][5]][5]

#### The gem here seems to be in the real part of $s_p$ as $\Re(s_p)=\frac{1}{2}$

[![The real part of s_p = 1/2][6]][6]

#### Extending the answer

My idea is to make a much more broad answer with more conclusions I gathered this month and plots of the new 3D versions of this $s_n$ and using lag 48 to leverage the crossing orbits, but I need some feedback regarding what I've done so far. I'd also like to know if some edit is necessary in the question. (This only addresses the question about how counting works in space.) The red radius of the last question is possible I get a symbolic expression but I couldn't yet focus to find an easy way. I think it's just the radius that appears for odd $n$-gons when you interconnect all the vertices with each other?

  [1]: https://i.sstatic.net/eAqdwTHv.png
  [2]: https://i.sstatic.net/46bNInLj.png
  [3]: https://i.sstatic.net/JHHtTO2C.png
  [4]: https://i.sstatic.net/xFjBUA7i.png
  [5]: https://i.sstatic.net/IY0rdgjW.png
  [6]: https://i.sstatic.net/0bk9suSC.png
