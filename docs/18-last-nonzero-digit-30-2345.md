# Last non-zero digit of 30^2345 — answer

*My answer on [math.stackexchange.com](https://math.stackexchange.com/a/4939079) — score 0.*

## Question: Find the last non-zero digit of $30^{2345}$

[Original question](https://math.stackexchange.com/q/1052885)

> 
  Find the last non-zero digit of $30^{2345}$

Source: Athena Healthcare Interview Questions


---

## My answer

## Periodicity of the last digit of $a^b$

I came across this question while analyzing the mod 4 periodicity in various contexts, one of them being the last digit of $a^b$.

It's interesting to note that period 4 appears everywhere, for example in the complex plane the rotation of $i^n$ being $1,i,-1,-i,1,...$ for $n$ being $0,1,2,3,...$:

I'd suggest to change the title of the question to $a^b$ instead of using a specific number, so the answer is more useful.

## The last digit of $a^b$ depends on the last digit of a and its periodic mod 4 :

Just by doing simple examples of a and b in pen and paper you easily deduce the following facts:

- $a\space\pmod{10}$ is the last digit of a

- The last digit of $3^8$ is going to be the same as the last digit of $3^4$. Doing examples as this you can create a simple $10$ rows by $4$ columns table with all the possible cases

- This table indicates the last digit of $a^b$ given by the last digit of $a$ and the reminder $b\%4$

$a\space\pmod{10}$
$a^1$
$a^2$
$a^3$
$a^4$

0
0
0
0
0

1
1
1
1
1

2
2
4
8
6

3
3
9
7
1

4
4
6
4
6

5
5
5
5
5

6
6
6
6
6

7
7
9
3
1

8
8
4
2
6

9
9
1
9
1

## Expression to summarize the conclusions that are encoded in the previous table:

Asking in discord about this, a kind mathematician helped me in the struggle to define this periodicity as an expression, and he ended up very generously teaching me the notion of p-adic valuations:

$$\begin{equation}
a^b \equiv 
\begin{cases} 
a^{b \% 4} \pmod{10} & \text{if } b \% 4 \ne 0 \text{ or } a \text{ is coprime with } 10 \\
2^{b \nu_2(a)} \pmod{10} & \text{if } b \% 4 = 0 \text{ and } 2 \mid a \\
5^{b \nu_5(a)} \pmod{10} & \text{if } b \% 4 = 0 \text{ and } 5 \mid a 
\end{cases}
\end{equation}
$$

- $b\% 4$ being the reminder of $b/4$

### The p-adic valuation of a $v_p(a)$

- $v_2(a)$ is the number of times 2 divides a

- $v_5(a)$ is the number of times 5 divides a

[https://en.wikipedia.org/wiki/P-adic_valuation](https://en.wikipedia.org/wiki/P-adic_valuation)

### The last non-zero digit of $a^b$ being $a=10*q$ as in your question $a=30=3*10$ and $b=2345$

$$a^b=3^b*10^b$$

- $10^b$ just adds zeroes at the end so we just have to care about $q$

In this case you could just search in the table and see that for the last digit of $q=3$ in the rows ,using the remainder $b\%4$ for the column.

$$b=2345\equiv 1 \pmod{4}$$  and see that the last digit must be $3$.

