# Equation with complex exponents — answer

*My answer on [math.stackexchange.com](https://math.stackexchange.com/a/4911472) — score 1.*

## Question: Solving equation with complex exponents

[Original question](https://math.stackexchange.com/q/4911448)

I have been looking for a way to solve the following equation:

$a=1-|r|^{2}=1-r_r^2-r_i^2$ with $r=\frac{H_{12}-H_{I}}{H_{R}-H_{12}}e^{2ik_{0}x_{1}}$ and $r_r$ and $r_i$ denoting the real and imaginary components of $r$.

$H_{12}$ denotes a complex transfer function of the form $H_{12}=H_x+H_yi$, where $H_x$ and $H_y$ are known real quantities. $H_I$ and $H_R$ are given by the following: $H_I=e^{-ik_0s}, H_R=e^{ik_0s}$

Further, $k_0$ is a complex number of the form $k_0=k_R+k_Ii$ where $k_R$ and $k_I$ are known real quantities. $x_1$ and $s$ are also known real quantities.

What would be the best way to approach solving for $a$? Is there a key simplification that can be made?


---

## My answer

I understand you need the value of $a$ so you need $r_r$ and $r_i$ so:
$$a=1-|r|^2$$
$$|r|^2=r_r^2+r_i^2$$

$$\omega =e^{2 k_o i}$$

$$k_o=k_R+ik_I$$

$$\omega =e^{2i(k_R+ik_I) }=e^{2 ik_R }e^{- 2 k_I }$$

$$r=T_f\omega^{x_1}=T_fe^{- 2 k_I x_1 }e^{2 k_R x_1 i }$$

You have all the values $k_o$,$s$ , etc to calculate and separate real from imaginary part of the $T_f$

$$T_f=\frac{H_{12}-H_{I}}{H_{R}-H_{12}}=T_R+iT_I$$

$$r=(T_R+iT_I)e^{- 2 k_I x_1 }(cos(2 k_R x_1)+isin(2 k_R x_1))$$

$$r_r =T_R e^{- 2 k_I x_1 }cos(2 k_R x_1)-T_Ie^{- 2 k_I x_1 }sin(2 k_R x_1)$$

$$r_i =T_R e^{- 2 k_I x_1 }sin(2 k_R x_1)+T_Ie^{- 2 k_I x_1 }cos(2 k_R x_1)$$

