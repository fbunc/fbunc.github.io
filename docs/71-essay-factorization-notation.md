# Factorization in 2D — Notation and What We Are Plotting

> A strict reference for the notation used across the Lab, the MIDI Studio, and every doc that plots $s_n$. If something in one of the other essays looks ambiguous, this is the page that settles it.
>
> **Source:** `QUESTIONS ABOUT PRIMES/000_FACTORIZATION_MAN/factorization_notation.md`.

---


## Prime factorization: the building blocks

Every integer $n \geq 2$ has a unique factorization into prime powers.  
We write it explicitly keeping track of the **distinct** primes and their exponents:

$$n = \prod_{k=1}^{m_n} d_k^{e_k}$$

where:

| Symbol | Meaning |
|--------|---------|
| $d_k$ | The distinct prime factors of $n$, sorted in ascending order |
| $e_k$ | The exponent of $d_k$ in the factorization of $n$ |
| $m_n$ | Number of **distinct** prime factors — little omega $\omega(n)$ |
| $M_n = \displaystyle\sum_{k=1}^{m_n} e_k$ | Total prime factors **counting multiplicity** — big omega $\Omega(n)$ |

**Example — $n = 60 = 2^2 \cdot 3^1 \cdot 5^1$:**

- $\{d_k\} = \{2, 3, 5\}$, $\{e_k\} = \{2, 1, 1\}$
- $m_{60} = 3$ (three distinct primes), $M_{60} = 2+1+1 = 4$ (four prime factors total)

---

## The unit of rotation: $\omega$

We pick a period $T_o \in \mathbb{Q}$ and define the primitive phase unit:

$$\omega = e^{2\pi i / T_o}$$

This places $\omega$ on the unit circle.  
Raising it to an integer power $j$ rotates by angle $\frac{2\pi j}{T_o}$, so $\omega^j$ is a point on the unit circle at that angle.  
Each prime factor $d_k$ is therefore mapped to a **direction** in the complex plane via $\omega^{d_k}$.

Since only $d_k \bmod T_o$ matters for the angle, choosing a small integer $T_o$ collapses many different primes to the same few directions — producing discrete, symmetric fingerprint geometries.

---

## The three sequences

We define three ways to accumulate the prime factors of $n$ into a single complex number:

| Sequence | Definition | Description |
|----------|-----------|-------------|
| $s_n$ | $\displaystyle\sum_{k=1}^{m_n} e_k\, \omega^{d_k}$ | Each prime $d_k$ contributes once **per multiplicity** — if $d_k$ appears $e_k$ times in the factorization, it adds $e_k$ copies of $\omega^{d_k}$ |
| $\dot{s}_n$ | $\displaystyle\sum_{k=1}^{m_n} \omega^{d_k}$ | Each **distinct** prime factor contributes exactly one unit vector, regardless of how many times it appears |
| $\ddot{s}_n$ | $\displaystyle\sum_{k=1}^{m_n} \omega^{e_k^{d_k}}$ | Each distinct prime contributes one unit vector, but the angle index is the prime **raised to its own exponent**: $d_k^{e_k}$ |

So $s_n$ is the multiplicity-aware version, $\dot{s}_n$ ignores repetition, and $\ddot{s}_n$ encodes both the prime and its exponent together in the angle.

**Example — $n = 60 = 2^2 \cdot 3^1 \cdot 5^1$:**

$$s_{60} = 2\,\omega^{2} + 1\,\omega^3 + 1\,\omega^5$$

$$\dot{s}_{60} = \omega^2 + \omega^3 + \omega^5$$

$$\ddot{s}_{60} = \omega^{2^2} + \omega^{3^1} + \omega^{5^1} = \omega^4 + \omega^3 + \omega^5$$

**Primes are always unit vectors:** For any prime $p$, we have $s_p = \dot{s}_p = \omega^p$, so $|s_p| = |\omega^p| = 1$.  
This means all primes land on the **unit circle** in the complex plane — they are sieved out naturally by their modulus.

---

## What the 2D plot shows

The scatter plot places each $n$ at the point $\bigl(\operatorname{Re}(s_n),\, \operatorname{Im}(s_n)\bigr)$ in the complex plane.  
Color encodes $n \bmod M_c$ so periodic patterns become visible.

Because $\omega^{d_k}$ depends only on $d_k \bmod T_o$, many numbers share the same complex value — these are the **reused positions**.  
The geometry that emerges is entirely determined by which residue classes of primes exist modulo $T_o$.

---

## Why $T_o = 10$ and $T_o = 6$

**$T_o = 10$ (last decimal digit):**  
$\omega^p$ depends only on the last digit of $p$.  
Primes (except 2 and 5) always end in 1, 3, 7, or 9, so each prime lands at one of four unit-circle angles.  
The resulting geometry is shaped by the **golden ratio** $\varphi = \frac{1+\sqrt{5}}{2}$, since $\cos(2\pi/10) = \cos(36°) = \varphi/2$.

**$T_o = 6$ (last digit mod 6):**  
Primes (except 2 and 3) are always $\equiv 1$ or $5 \pmod{6}$, so each prime lands at angle $\frac{2\pi}{6}$ or $\frac{4\pi}{6}$ — corresponding to $\pm 60°$.  
The resulting geometry is hexagonal and shaped by $\sqrt{3}$.

For large $T_o$ the angles spread continuously, the number of distinct positions grows rapidly, and the scatter looks like a dense cloud rather than a discrete lattice.

---

## What the 3D plot shows

A third coordinate $w_n$ lifts each point out of the complex plane.  
The three natural choices from the factorization are:

| $w_n$ | Meaning |
|-------|---------|
| $w_n = n$ | The "time" axis — shows how the pattern grows as we count |
| $w_n = M_n$ | The "mass" axis — total prime factors with multiplicity; heavier composite numbers sit higher |
| $w_n = m_n$ | The "diversity" axis — number of distinct prime factors; highly composite numbers with many different primes sit higher |

---

## The coupon-collector question

As we count $n = 2, 3, 4, \ldots, N$, how many **distinct** positions in the complex plane do we actually visit?

Imagine drawing numbers out of a box: before placing $n$ on the shelf (i.e., before factorizing it), we can ask — will this number land somewhere we have already visited, or will it need a new position?

- For small integer $T_o$: only finitely many angles are possible, so positions get reused very quickly.
- For large or irrational $T_o$: positions keep appearing for longer before repetitions dominate.

The **occupancy probability** tracks, as a running fraction of all $n$ seen so far, what proportion landed on a previously occupied position. This is the "reuse probability" plot.
