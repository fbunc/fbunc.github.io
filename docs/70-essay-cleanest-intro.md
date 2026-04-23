# Questions about primes and factorization — the cleanest intro

> The shortest way I have found to describe *what I am doing* when I draw the prime factorization of a number as a vector in the complex plane. Written so that a friend who has never seen the construction can, by the end, read $s_n$, $\dot s_n$, and $\ddot s_n$ out loud and know what they mean.
>
> **Source:** `QUESTIONS ABOUT PRIMES/000_FACTORIZATION_MAN/000000000000000_CLEANEST_INTRO.md`.

---


## The process

Imagine something like a coupons collector problem, but instead of collecting coupons, we are collecting numbers while counting and organizing them according to their prime factors. 

Each time we encounter a new number, we factor it into its prime factors and add those factors to our collection using a specific encoding scheme. One encoding scheme could be based on the idea of a "decimal fingerprint" where we use the last digit of the prime factors to determine their position in a 2D grid, but we extend this to composite numbers as well.

I've been set to the quest of a naive exploration of the subject, with expectations of  just finding pretty representations of "random walks". For my surprise I ended up listening a MIDI representation of the prime factorization of numbers, and I found it to be quite beautiful, and it doesn't sound like noise.



## Intro

I don't use $p$ for prime factors because later on I will use p to represent the sequences of all primes themselves. I add this disclaimer because I don't want to cause confusion between the prime factors of a number and the sequence of all primes.

So please allow me to express the prime factorization of a number $n$ in terms of big omega and little omega,but with a a different notation. Here I will use $M_n=M(n)$ to represent big omega, which is the total number of prime factors of $n$ with multiplicity, and $m_n=m(n)$ to represent little omega, which is the number of distinct prime factors of $n$. I like to use the letter "m" because it resembles the idea of "mass" as well. 

Then we can express the prime factorization of $n$ as:

$$n = \prod_{k=1}^{M_n} d_k^{e_k}$$

| Sequence | Definition | Description |
|----------|-----------|-------------|
| $s_n$ | $\sum_{k=1}^{M_n} e_k  \omega^{d_k}$ |Phase representation of all prime factors of $n$ with multiplicity |
| $\dot{s}_n$ | $\sum_{k=1}^{m_n} \omega^{d_k}$ | Phase representation of distinct prime factors of $n$ only |
| $\ddot{s}_n$ | $\sum_{k=1}^{m_n} \omega^{e_k^{d_k}}$ | Phase representation of distinct prime factors of $n$ with their exponents |


where $\omega = e^{2\pi i / T_o}$, for $T_o \in \mathbb{Q}$, the $d_k$ are the distinct prime factors, the $e_k$ are the exponents of each $d_k$, and $M_n$ is the total number of prime factors of $n$ with multiplicity, which can be expressed as:


$$m_n \space  \text{is the number of distinct prime factors of } n$$

Or phrased differently, if we denote the D distinct prime factors of  $n$ as $d_1, d_2, \dots, d_{D}$, then:

$$D = m_n = m(n) \space \text{is the size of the set}\space of \space n \space \{d_1, d_2, \dots, d_{D}\}$$

And

$$M_n = M(n) = \sum_{k=1}^{m_n} e_k$$

For example for $60$, we have:
- Prime factorization: $2^2 \cdot 3^1 \cdot 5^1$
- $\{d_k\}$ Distinct prime factors: $\{2, 3, 5\}$
- $\{e_k\}$ Exponents:$\{2, 1, 1\}$
- $M_{60} =2 + 1 + 1 = 4$ (since we have two 2's, one 3, and one 5)
- $m_{60} =1 + 1 + 1 = 3$ (since we have three distinct prime factors: 2, 3, and 5)
- $s_{60} = 2 \cdot \omega^{2} + 1 \cdot \omega^3 + 1 \cdot \omega^5$
- $\dot{s}_{60} = \omega^2 + \omega^3 + \omega^5$   
- $\ddot{s}_{60} = \omega^{2^2} + \omega^{3^1} + \omega^{5^1} = \omega^4 + \omega^3 + \omega^5$

So, $s_n$ tries to capture the full prime factorization of $n$ including multiplicities, while $\dot{s}_n$ tries to capture only the distinct prime factors without considering their multiplicities. $\ddot{s}_n$ tries to capture the distinct prime factors along with their exponents in a different way.


$$n = \prod_{k=1}^{M_n} d_k^{e_k} \implies s_n = \sum_{k=1}^{M_n} e_k \omega^{d_k}$$

And:

$$\dot{s}_n = \sum_{k=1}^{m_n} \omega^{d_k}$$

### The blueprint of the process for $n$ up to $N$:

The idea was something like we are getting big numbers (in order) out of a box, and we don't yet know where we are going to place them on a shelf or position in space  $(Re(s_n),Im(s_n),f(n))$. meaning we don't know their factorization yet. 

You draw a number and before placing it somewhere (factorizing it ) we could ask for each unique position of $s_n$  (place in the shelf) that is already in the history up to N, what is the probability that the new number will be placed in any of the previously used positions , vs the probability that I will need a new position. This can be analyzed for many variations of $s_n$ and $\dot{s}_n$ and for different values of $T_o$.
