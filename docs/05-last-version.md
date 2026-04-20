The question requires some introduction, and I will try to make it as clear and short as possible. 

This arises in the context of the informal study of a set of simple symbolic structures in space used to visualize multivariable correlation. 

Parallel to the basic calculation of the lags with peaks in correlation, the idea I'm playing with is to investigate families of curves in 2D or 3D that might show that correlation as a graph embedded in an ad-hoc symmetric structure that evolves with the index of the sequence. 

## Relation to log(z)

I was playing with the first term of the bilinear expansion of $log(z)$ and realized that it works as a bounded log. 

This fit the idea I was playing with of numerical atoms that grow forever but stay contained. 

I'm interested in prime hunting techniques and in understanding how the best approximations of $\pi(n)$ were constructed.

https://math.stackexchange.com/questions/585154/taylor-series-for-logx/3535630#3535630

$$\log(z) = 2\left[\left({z-1\over z+1}\right)+ {1\over3} \left({z-1\over z+1}\right)^3 + {1\over5} \left({z-1\over z+1}\right)^5+ \cdots\right]$$

Converges for $\Re(z) > 0$.

Can the first term of the bilinear be a good enough representation of log(Z) for certain uses?

Consider:

$$r_n=\frac{(n-1)}{(n+1)}+p_o$$

for $n \geq 0 \in \mathbb{N}$ and the first prime as the initial probe $p_o=2$

$$r_n=\frac{b_n}{q_n}$$

The gcd between $b_n$ and $q_n$ $\in \mathbb{N}$ has to be 1.

I checked if either $b_n$ or $q_n$ is prime and compared it with the natural density and with different prime frequency expressions based on log() or Li() up to 10^5.

## filtered for unique prime discovery by $b_n$ or $q_n$ as a markdown table 

| n | b_n | q_n | n_is_prime | pi(n)/n | (Li(n)-0.5Li(sqrt(n)))/n | p_unique | p_unique_cum | p_unique_freq | q_unique | q_unique_cum | q_unique_freq | r_n (decimal) | r_n (fraction) | b_n_is_prime | q_n_is_prime | b_n_cum_primes | b_n_freq | q_n_cum_primes | q_n_freq | either_prime | either_cum_primes | either_freq |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 1 | 1 | 0 | ... | ... | 0 | 0 | 0.000000 | 0 | 0 | 0.000000 | 1.000000 | 1/1 | 0 | 0 | 0 | 0.000000 | 0 | 0.000000 | 0 | 0 | 0.000000 |
| 1 | 2 | 1 | 0 | 0.000000 | 0.000000 | 1 | 1 | 0.500000 | 0 | 0 | 0.000000 | 2.000000 | 2/1 | 1 | 0 | 1 | 0.500000 | 0 | 0.000000 | 1 | 1 | 0.500000 |
| 2 | 7 | 3 | 1 | 0.500000 | 0.548425 | 1 | 2 | 0.666667 | 1 | 1 | 0.333333 | 2.333333 | 7/3 | 1 | 1 | 2 | 0.666667 | 1 | 0.333333 | 1 | 2 | 0.666667 |
| 3 | 5 | 2 | 1 | 0.666667 | 0.619012 | 1 | 3 | 0.750000 | 1 | 2 | 0.500000 | 2.500000 | 5/2 | 1 | 1 | 3 | 0.750000 | 2 | 0.500000 | 1 | 3 | 0.750000 |
| 4 | 13 | 5 | 0 | 0.500000 | 0.611251 | 1 | 4 | 0.800000 | 1 | 3 | 0.600000 | 2.600000 | 13/5 | 1 | 1 | 4 | 0.800000 | 3 | 0.600000 | 1 | 4 | 0.800000 |
| 5 | 8 | 3 | 1 | 0.600000 | 0.590866 | 0 | 4 | 0.666667 | 0 | 3 | 0.500000 | 2.666667 | 8/3 | 0 | 1 | 4 | 0.666667 | 4 | 0.666667 | 1 | 5 | 0.833333 |
| 6 | 19 | 7 | 0 | 0.500000 | 0.569408 | 1 | 5 | 0.714286 | 1 | 4 | 0.571429 | 2.714286 | 19/7 | 1 | 1 | 5 | 0.714286 | 5 | 0.714286 | 1 | 6 | 0.857143 |
| 7 | 11 | 4 | 1 | 0.571429 | 0.549465 | 1 | 6 | 0.750000 | 0 | 4 | 0.500000 | 2.750000 | 11/4 | 1 | 0 | 6 | 0.750000 | 5 | 0.625000 | 1 | 7 | 0.875000 |
| 8 | 25 | 9 | 0 | 0.500000 | 0.531520 | 0 | 6 | 0.666667 | 0 | 4 | 0.444444 | 2.777778 | 25/9 | 0 | 0 | 6 | 0.666667 | 5 | 0.555556 | 0 | 7 | 0.777778 |
| 9 | 14 | 5 | 0 | 0.444444 | 0.515494 | 0 | 6 | 0.600000 | 0 | 4 | 0.400000 | 2.800000 | 14/5 | 0 | 1 | 6 | 0.600000 | 6 | 0.600000 | 1 | 8 | 0.800000 |
| 10 | 31 | 11 | 0 | 0.400000 | 0.501168 | 1 | 7 | 0.636364 | 1 | 5 | 0.454545 | 2.818182 | 31/11 | 1 | 1 | 7 | 0.636364 | 7 | 0.636364 | 1 | 9 | 0.818182 |
| 11 | 17 | 6 | 1 | 0.454545 | 0.488311 | 1 | 8 | 0.666667 | 0 | 5 | 0.416667 | 2.833333 | 17/6 | 1 | 0 | 8 | 0.666667 | 7 | 0.583333 | 1 | 10 | 0.833333 |
| 12 | 37 | 13 | 0 | 0.416667 | 0.476713 | 1 | 9 | 0.692308 | 1 | 6 | 0.461538 | 2.846154 | 37/13 | 1 | 1 | 9 | 0.692308 | 8 | 0.615385 | 1 | 11 | 0.846154 |



### Question 1

Why are $q_n$ unique prime events matching so perfectly the real frequency? 
It looks like $b_n$ is the same shape but shifted

[![enter image description here][1]][1]

This was answered by Thomas in the comments:

If n is odd

$$b_n=\frac{(p_o+1)n+(p_o-1)}2$$

$$q_n=\frac{n+1}2$$

Otherwise

$$b_n=(p_o+1)n+p_o-1$$

$$q_n=n+1$$

So $q_n$ behaves like $n$.

## Question 2

Consider a hexagonal spiral with a radius $r_n$ that grows forever but never reaches 3.

$$r_n=\frac{(n-1)}{(n+1)}+p_o$$
$$r_n\omega^n$$
$$\omega=e^{\frac{2\pi i}{T_o}}$$
$$s_n=r_n\omega^n$$

### Plot of the oscillation between primes that are either 1 or 5 modulo 6

* The gray lines are between consecutive number events starting from 1

* The red lines are between consecutive prime number events

This graph shows that all primes $p \geq 5$ are either $p \equiv 5 \mod{6}$ or $p \equiv 1 \mod{6}$
I'm aware this is a known simple fact based on realizing that if a number is $0;2;4 \mod{6}$ is even and if it's $3 \mod{6}$ it's divisible by 3, so that leaves us with only $1$ or $5$ as options.
 
I wasn't aware of this fact, until it became obvious while playing with more symmetries added to the following plot. 
In this plot the behavior of prime events transitions after 5 becomes binary with 4 possible transitions:  $1\rightarrow 1$; $1\rightarrow 5$; $5\rightarrow 1$; $5\rightarrow 5$. 

Is the distribution across these $2^2$ possible transitions known. Should it be flat? It looks there is an initial bias towards "polar" transitions between $1\rightarrow 5$ and $5\rightarrow 1$ when tested up to $10^5$.
\begin{align*}
1 \to 1 &: 0.1997 \\
1 \to 5 &: 0.2991 \\
5 \to 1 &: 0.2992 \\
5 \to 5 &: 0.2020
\end{align*}

Can we expect to find predictive patterns (of the same kind as before with initial spikes in the densities) by playing with another starting probe prime different than $p_o=2$ and trying, for example, $p_o=2^{136279841}-1$ or a **known big prime that is either 1 or 5 modulo 6**? I found that for manageable sizes it's easy to predict when the next prime event is about to happen if you start with the right $p_o$ and see the animation without the numbers in it and superposed orbits of $r_n \omega^{b_n}$ as a dummy prime predictor. 
You can test it by converting these plots into animations that go at the desired frame rate or add step-by-step functionality.

[![enter image description here][2]][2]

#### Colored transitions
[![enter image description here][3]][3]
[![enter image description here][4]][4]
## A periodic table for numbers with it's microscope

The last plot is the top view of the $\vec{A_n}$ curve of the following atomic shape that emerges from the act of counting with $T_o=6$, coloring modulo 6 with $M_c=6$ different colors so color $C_n \equiv n \mod{M_c}$:

$$\vec{\mathbf{A_n}}=(u_n,v_n,w_n)$$


$$\Omega_o=\frac{2\pi }{T_o}$$

And the spatial coordinates arrays for index $n$

$$u_n=\frac{1}{2}(s_n+\overline{s_n})=r_n \cos(\Omega_o n)$$

$$v_n=\frac{1}{2i}(s_n-\overline{s_n})=r_n \sin(\Omega_o n)$$

$$w_n=r_n$$

## Atomic structure created by 48 reflections of a main guide curve

Can playing with structures like this help in the quest of predicting the next primes for manageable sizes? 

I see superposed orbits that may or may not have some information associated with them that can be thought of as "interactions" between numbers, especially the red line prime oscillation inside the eternally growing bounded log radius hexagon.
#### $F_0$

* xyz x4

$$\vec{A_n}=(u_n,v_n,w_n)$$

$$\vec{B_n}=(u_n,-v_n,w_n)$$

$$\vec{C_n}=(-u_n,v_n,w_n)$$

$$\vec{D_n}=(-u_n,-v_n,w_n)$$

* yxz x4

$$\vec{E_n}=(v_n,u_n,w_n)$$

$$\vec{F_n}=(v_n,-u_n,w_n)$$

$$\vec{G_n}=(-v_n,u_n,w_n)$$

$$\vec{H_n}=(-v_n,-u_n,w_n)$$

#### $F_1$

* xy(-z) x4
$$\vec{I_n}=(u_n,v_n,-w_n)$$

$$\vec{J_n}=(u_n,-v_n,-w_n)$$

$$\vec{K_n}=(-u_n,v_n,-w_n)$$

$$\vec{L_n}=(-u_n,-v_n,-w_n)$$



* yx(-z) x4
$$\vec{M_n}=(v_n,u_n,-w_n)$$

$$\vec{N_n}=(v_n,-u_n,-w_n)$$

$$\vec{O_n}=(-v_n,u_n,-w_n)$$

$$\vec{b_n}=(-v_n,-u_n,-w_n)$$

#### $F_2$
* zxy x4

$$\vec{Q_n}=(w_n,u_n,v_n)$$

$$\vec{R_n}=(w_n,u_n,-v_n)$$

$$\vec{S_n}=(w_n,-u_n,v_n)$$

$$\vec{T_n}=(w_n,-u_n,-v_n)$$


* zyx x4

$$\vec{U_n}=(w_n,v_n,u_n)$$

$$\vec{V_n}=(w_n,v_n,-u_n)$$

$$\vec{W_n}=(w_n,-v_n,u_n)$$

$$\vec{X_n}=(w_n,-v_n,-u_n)$$
  

#### $F_3$

* (-z)xy x4

$$\vec{\alpha_n}=(-w_n,u_n,v_n)$$

$$\vec{\beta_n}=(-w_n,u_n,-v_n)$$

$$\vec{\gamma_n}=(-w_n,-u_n,v_n)$$

$$\vec{\delta_n}=(-w_n,-u_n,-v_n)$$



* (-z)yx x4

$$\vec{\epsilon_n}=(-w_n,v_n,u_n)$$

$$\vec{\zeta_n}=(-w_n,v_n,-u_n)$$

$$\vec{\eta_n}=(-w_n,-v_n,u_n)$$

$$\vec{\theta_n}=(-w_n,-v_n,-u_n)$$

#### $F_4$

* xzy x4

$$\vec{\iota_n}=(u_n,w_n,v_n)$$

$$\vec{\kappa_n}=(u_n,w_n,-v_n)$$

$$\vec{\lambda_n}=(-u_n,w_n,v_n)$$

$$\vec{\mu_n}=(-u_n,w_n,-v_n)$$


* yzx x4

$$\vec{\nu_n}=(v_n,w_n,u_n)$$

$$\vec{\xi_n}=(v_n,w_n,-u_n)$$

$$\vec{O_n}=(-v_n,w_n,u_n)$$

$$\vec{\pi_n}=(-v_n,w_n,-u_n)$$




#### $F_5$
* x(-z)y x4


$$\vec{\rho_n}=(u_n,-w_n,v_n)$$

$$\vec{\sigma_n}=(u_n,-w_n,-v_n)$$

$$\vec{\tau_n}=(-u_n,-w_n,v_n)$$

$$\vec{\upsilon_n}=(-u_n,-w_n,-v_n)$$




* y(-z)x x4

$$\vec{\phi_n}=(v_n,-w_n,u_n)$$

$$\vec{\chi_n}=(v_n,-w_n,-u_n)$$

$$\vec{\psi_n}=(-v_n,-w_n,u_n)$$

$$\vec{\omega_n}=(-v_n,-w_n,-u_n)$$



## Some examples of atoms for different numbers following the previous logic

The same $s_n$ as in the hexagon plot of one curve, but now with its 47 reflections added, seen in 3 dimensions.

### Atom for $N=1000$ with $T_o=6$ and $M_c=6$

[![enter image description here][5]][5]

### View from inside of the previous atom

[![enter image description here][6]][6]

### Better view of the same structure without prime oscillations

[![enter image description here][7]][7]

### Now using an irrational $T_o=\varphi$ and with $n$ up to $N=39$ and then $N=1000$

Using an irrational number approximation as $T_o$ will create a pattern that as $N \to \infty$ eventually fills a radius 3 circumference with the visualized events. Events in this case are primes, but we could be working with any kind of event observed in any kind of sequence. 

[![enter image description here][8]][8]

[![enter image description here][9]][9]

[![enter image description here][10]][10]

### $\vec{A}_n$ top view with $C_n \equiv n \mod{18}$  with smoothed $r_n$, $M=1$ and irrational $T_o=\varphi$

$$
r_k=
\begin{cases}
0 & k < 0 \\
r_{k-1}+\Delta r_o & k \geq 0 \text{ and } k \equiv 0 \pmod{M} \\
r_{k-1} & k > 0 \text{ and } k \not\equiv 0 \pmod{M}
\end{cases}
$$
$$u_n=\frac{1}{2}(s_n+\overline{s_n})=r_n \cos(\Omega_o n)$$

$$v_n=\frac{1}{2i}(s_n-\overline{s_n})=r_n \sin(\Omega_o n)$$

$$w_n=\log_{2}(|s_n|)$$

[![enter image description here][11]][11]

### Added prime oscillations

[![enter image description here][12]][12]

#### From the inside of previous plot

What is the radius of this emergent circumference?

[![enter image description here][13]][13] 


  [1]: https://i.sstatic.net/nSvg0aBP.png
  [2]: https://i.sstatic.net/KnSxmHeG.png
  [3]: https://i.sstatic.net/TpKQgDlJ.png
  [4]: https://i.sstatic.net/bJymv9Ur.png
  [5]: https://i.sstatic.net/pzIyqx9f.png
  [6]: https://i.sstatic.net/OPgiMO18.png
  [7]: https://i.sstatic.net/JLNPWY2C.png
  [8]: https://i.sstatic.net/82Ni4hmT.png
  [9]: https://i.sstatic.net/CbeDfGqr.png
  [10]: https://i.sstatic.net/gYVCB9OI.png
  [11]: https://i.sstatic.net/zODRVcm5.png
  [12]: https://i.sstatic.net/pz9evDYf.png
  [13]: https://i.sstatic.net/zOCUFID5.png