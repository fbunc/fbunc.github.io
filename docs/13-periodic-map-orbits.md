# Periodic map orbits of $X_{n+1}\equiv 2X_n \pmod 1$

*Originally posted on [math.stackexchange.com](https://math.stackexchange.com/questions/4936156/number-of-periodic-map-orbits-and-why-in-x-n1-equiv-2-x-n-pmod1-x-0) (question 4936156).*

---

Q1:
I'm trying to understand the notion of  "number of periodic map orbits" reading here:

[https://mathworld.wolfram.com/2xmod1Map.html](https://mathworld.wolfram.com/2xmod1Map.html)

For K=1 the case is:

$$X_{n+1}\equiv(K+1) X_{n} \pmod{K}$$

$$ X_{0}=\frac{p_o}{q_o} \in \mathbb{Q}  \space [0,1]$$

What does $N_p$ for prime $p$  exactly indicate ? How can this relation be derived? What is the motivation to study these maps and call them orbits?

$$N_p=\frac{2^p-2}{p}$$

Q2: (edit)

This part of the question is partially answered:

The problem seems to be the accumulated precision problem as @zwim pointed out in his answer

I did some simulations where in all the tests  after around 50 iterations always goes to 1/4 then 1/2 and then 0.

$$X_{n+1}\equiv 2 X_{n} \pmod{1}$$

What is going on?  For a given precision N will it always end as :

$$2^{-2},2^{-1},0$$

i.e: $X_o=0.1$ the cycle has to be periodic as 0.2,0.4,0.8,06,0.2...
But in simulation's step 30 jumps from 0.2 to 0.40000001 and there the binary shifting caused by the iteration seems to always end up with the same 3 last numbers, after it goes to constant 0.

## Simulation details

The simulations used double-precision IEEE 754:
$$(-1)^{\text{sign}}(1.b_{51}b_{50}\ldots b_0)_2 \times 2^{e-1023}$$

$$(-1)^{\text{sign}}\left(1 + \sum_{i=1}^{52} b_{52-i}2^{-i}\right) \times 2^{e-1023}$$

$$2^{−53} ≈ 1.11 × 10{-16}$$

So we can manage around 15 significant digits.

### Trick found to significantly extend the numbers of iterations where a "almost-periodicity" is sustained.

I can't exactly explain why this works but:
Adding a $\delta=\alpha \space 10^{-15}$ works as a hack and maintains the "almost-periodicity" for a much larger number of iterations:

$$X_{n+1}\equiv (2+\delta) X_{n} \pmod{1}$$

## Example for $X_{0}\approx \varphi - 1$ without $\delta$

[![enter image description here](https://i.sstatic.net/AJbz21i8.png)](https://i.sstatic.net/AJbz21i8.png)

## Same example adding the $\delta$ :

[![enter image description here](https://i.sstatic.net/XW4dzjfc.png)](https://i.sstatic.net/XW4dzjfc.png)

[![enter image description here](https://i.sstatic.net/Jf37vo2C.png)](https://i.sstatic.net/Jf37vo2C.png)

