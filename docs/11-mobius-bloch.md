# Möbius-band shape from the Bloch sphere

*Originally posted on [math.stackexchange.com](https://math.stackexchange.com/questions/4949243/m%c3%b6bius-band-alike-shape-found-trying-to-understand-the-bloch-sphere) (question 4949243).*

---

# Colored/Labeled finite set of natural numbers describing a curve in space resembling a Möbius strip found by playing with the idea of the bloch-sphere.

The title is too long but the question is simple:

Does this set of  curves resembling a Möbius-strip have a name? Where could I read more about them and related shapes?

I was playing with this construction trying to understand better the bloch-sphere:

- Consider for example $|\alpha_n|$ growing according to a recursive rule of the form:

$$
|\alpha_k|=
\begin{cases}
0 &  k   0 \text{ and } k \not\equiv 0 \pmod{M}
\end{cases}
$$

$$k  \in \space \mathbb{Z}$$

- The index $n$ is the countable set input to a function that has an output of at least 4 dimensions, 3 that can be considered spatial for simplicity and at least one label we show as a color in each vertex in  this case being a simple function of the form $f(n,M_c)$.

$$n\geq 0  \in \space \mathbb{N}$$

$$\alpha_n=x_n+iy_n=|\alpha_n|e^{i\omega_{\alpha} n}$$
$$\beta_n=z_n+i\tau_n =|\beta_n|e^{i\omega_{\beta} n}$$

$$u_n=y_nz_n=|\alpha_n \beta_n| \sin{(\omega_{\alpha} n)} \cos{(\omega_{\beta}n)}$$
$$v_n=y_n \tau_n=|\alpha_n \beta_n| \sin{(\omega_{\alpha} n)}\sin{(\omega_{\beta} n)}$$
$$w_n=x_n=|\alpha_n|  \cos{(\omega_{\alpha} n)}$$

And then thought of using this with this conditions:

$$\alpha_n=x_n+iy_n=|\alpha_n|e^{i\omega_{\alpha} n}$$
$$\beta_n=\frac{1}{\alpha_n} =\frac{1}{|\alpha_n|}e^{-i\omega_{\alpha} n}$$

$$u_n=y_nz_n= \sin{(\omega_{\alpha} n)} \cos{(\omega_{\alpha}n)}$$
$$v_n=y_n \tau_n= -\sin{(\omega_{\alpha} n)}\sin{(\omega_{\alpha} n)}=-\sin^2{(\omega_{\alpha} n)}$$
$$w_n=x_n=|\alpha_n|  \cos{(\omega_{\alpha} n)}$$

- The color of each vertex of the resluting discrete-curve

$$C_n= n \pmod{M_c}$$

$$\omega_{\alpha}=\frac{2 \pi}{T_o}$$

- Total number of samples for the following plotted curve :

$$N=100 M$$

- $M_c=M=T_o$

[![colored-band-no-name](https://i.sstatic.net/GsVDgduQ.png)](https://i.sstatic.net/GsVDgduQ.png)

The resulting shape can be thought as an ant walking in the Möbius-strip where if you assume one set of  colors is the interior , then other set of colors can be thought as the exterior after the crossing point.

Can the crossing-point be thought as the added point at $\infty$ to which we can associate a rule that decides what happens when crossed?

- $M=T_o=16000$ and $M_c=10^{2}  M $

[![other-mobius-band-max-color-res](https://i.sstatic.net/TMUx3GbJ.png)](https://i.sstatic.net/TMUx3GbJ.png)

*$M=1600$ and $T_o=p/q\approx \varphi$ and $M_c=13$

[![phi-approximation-and-mod-13-color](https://i.sstatic.net/pz5PRvOf.png)](https://i.sstatic.net/pz5PRvOf.png)

## Bloch-sphere  alike surface  using the elements of a basic Möbius transform as coordinates in space.

$$\alpha_n=x_n+ i y_n=|\alpha_n|e^{i\omega_{\alpha} n}$$
$$u_n=\frac{2 x_n}{1+x^2_n +y^2_n}$$
$$v_n=\frac{2 y_n}{1+x^2_n +y^2_n}$$
$$w_n=\frac{1-x^2_n -y^2_n}{1+x^2_n +y^2_n}$$

[![colored-inverted-bloch-surface](https://i.sstatic.net/fzRKPci6.png)](https://i.sstatic.net/fzRKPci6.png)

### Relation with quaternions and Euler:

It's interesting to note that from here is easy to imagine a "euler-quaternion" object of the form:

$$\mathbf{q_n} =
  e^{\frac{1}{2}\theta_n(u_n\mathbf{i} + v_n\mathbf{j} + w_n\mathbf{k})} =
  \cos \frac{\theta_n}{2} + (u_n\mathbf{i} + v_n\mathbf{j} + w_n\mathbf{k}) \sin \frac{\theta_n}{2}
$$

This allow us to imagine rotation actions about a general axis as quaternion multiplications, as  we do with complex numbers in the 2D plane:

[https://en.wikipedia.org/wiki/Bloch_sphere#Rotations_about_a_general_axis](https://en.wikipedia.org/wiki/Bloch_sphere#Rotations_about_a_general_axis)

### $M_{\alpha}=M_{\beta}=1$ $M_c=13$

This is a complete example with an approximation of an irrational  using $T_x=p/q$ and two complex number sequences.

- $T_{\alpha}=\frac{1618033988749895}{ 10^{15}} \approx \varphi$ and  $T_{\beta} \approx \varphi^{-1}$
$$n \geq 0 \in \mathbb{N}$$

[![strange_4d_object](https://i.sstatic.net/IJQBe8Wk.png)](https://i.sstatic.net/IJQBe8Wk.png)

$$\alpha_n=x_n+iy_n=|\alpha_n|e^{i\omega_{\alpha} n}$$

$$\beta_n=z_n+i\tau_n =|\beta_n|e^{i\omega_{\beta} n}$$

$$u_n=y_nz_n=|\alpha_n \beta_n| \sin{(\omega_{\alpha} n)} \cos{(\omega_{\beta}n)}$$

$$v_n=y_n \tau_n=|\alpha_n \beta_n| \sin{(\omega_{\alpha} n)}\sin{(\omega_{\beta} n)}$$

$$w_n=x_n=|\alpha_n|  \cos{(\omega_{\alpha} n)}$$

$$C_n= n \pmod{M_c}$$

$$\omega_{\alpha}=\frac{2 \pi}{T_{\alpha}}$$

$$\omega_{\beta}=\frac{2 \pi}{T_{\beta}}$$

$$
|\alpha_k|=
\begin{cases}
0 &  k   0 \text{ and } k \not\equiv 0 \pmod{M_{\alpha}}
\end{cases}
$$

$$
|\beta_k|=
\begin{cases}
0 &  k   0 \text{ and } k \not\equiv 0 \pmod{M_{\beta}}
\end{cases}
$$

$$N=10^{5}$$

