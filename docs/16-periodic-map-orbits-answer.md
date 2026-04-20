# 2x mod 1 orbits — answer by zwim

*Answer by **zwim** on [math.stackexchange.com](https://math.stackexchange.com/a/4936171) — score 2 · accepted. Parent question: 4936156.*

---

One possible explanation to this would be limitless binary representation in memory of floating numbers for computers:

i.e. $x=\sum\limits_{k=1}^N x_k2^{-k}$ where $x_k\in\{0,1\}$ and $N$ is the precision.

Note that the operation $X_{n+1}=\{2X_n\}$ operates a reduction on binary development, it eliminates the first $x_k$ in the summation:

$2x=\sum\limits_{k=1}^N x_k2^{1-k}=\sum\limits_{k=0}^{N-1} x_{k+1}2^{-k}=x_1+\underbrace{\sum\limits_{k=1}^{N-1} x_{k+1}2^{-k}}_{

Taking the $\pmod 1$ or fractional part eliminates $x_1$ (since its value is $0$ or $1$, $\{x_1\}=0$), and the rest is less than $1$ so it equals its fractional part.

Therefore $\{2x\}=\sum\limits_{k=1}^{N-1} x_{k+1}2^{-k}$

Next time you'll apply the operation $x_2$ will be eliminated, then $x_3$ and so on, until you are left with $0$.

Written differently $\overline{x_1x_2x_3\cdots x_N}\,\mapsto\, \overline{x_2x_3x_4\cdots x_{N-1}}$

Note:

*This is only an approximative explanation because numbers are most likely represented $x=M\,2^{-p}$ where $M$ is an integer and $p$ the precision. So it will not operate exactly as I described above, might be a little more subtle, but probably the error correction will suffer this fate.*
