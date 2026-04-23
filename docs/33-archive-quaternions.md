# Quaternions' geometrical intuitions from three complex numbers

> **Source:** [math.stackexchange.com/q/4949694](https://math.stackexchange.com/q/4949694)  ·  **archived (deleted from MSE)**
> **Tags:** `complex-numbers`, `quaternions`, `symmetry`
> **Asked:** 2024-07-24 17:14:51Z

## Question

# Introduction

Recently I made a question here about the binary-tetrahedral-group and I started by analyzing quaternion-projections in the complex plane and their associated symmetries.

[Introduction to the Binary Tetrahedral group and the 24-cell](https://math.stackexchange.com/questions/4946966/introduction-to-the-binary-tetrahedral-group-and-the-24-cell)

I'm really interested in understanding how the logic works in these constructions.
Specially in how they can be thought as arising from a logical process through the association of symmetry actions in higher dimensions following a set of binary counting processes.

### Attempt to summarize previous MSE questions working on the subject and a few simple questions.

$$\mathbf{i}^{2} = \mathbf{j}^{2} = \mathbf{k}^{2} = \mathbf{i}\mathbf{j}\mathbf{k} = - 1$$

  ×              1              $\mathbf{i}$     $\mathbf{j}$     $\mathbf{k}$
  -------------- -------------- ---------------- ---------------- ----------------
  1              1              $\mathbf{i}$     $\mathbf{j}$     $\mathbf{k}$
  $\mathbf{i}$   $\mathbf{i}$   -1               $\mathbf{k}$     \-$\mathbf{j}$
  $\mathbf{j}$   $\mathbf{j}$   \-$\mathbf{k}$   -1               $\mathbf{i}$
  $\mathbf{k}$   $\mathbf{k}$   $\mathbf{j}$     \-$\mathbf{i}$   -1

$$\mathbf{u} = u_{x}\mathbf{i} + u_{y}\mathbf{j} + u_{z}\mathbf{k}$$

$$\mathbf{v} = v_{x}\mathbf{i} + v_{y}\mathbf{j} + v_{z}\mathbf{k}$$

-   Following the multiplication table
    $$\mathbf{u}\mathbf{v} = - (u_{x}v_{x} + u_{y}v_{y} + u_{z}v_{z}) + (u_{y}v_{z} - u_{z}v_{y})\mathbf{i} + (u_{z}v_{x} - u_{x}v_{z})\mathbf{j} + (u_{x}v_{y} - u_{y}v_{x})\mathbf{k}$$

$$\mathbf{u}\mathbf{v} = - \mathbf{u} \cdot \mathbf{v} + \mathbf{u} \times \mathbf{v}$$

$$\mathbf{u} \cdot \mathbf{v} = u_{x}v_{x} + u_{y}v_{y} + u_{z}v_{z}$$

$$\mathbf{u} \times \mathbf{v} = \left| \begin{matrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
u_{x} & u_{y} & u_{z} \\
v_{x} & v_{y} & v_{z}
\end{matrix} \right| = (u_{y}v_{z} - u_{z}v_{y})\mathbf{i} + (u_{z}v_{x} - u_{x}v_{z})\mathbf{j} + (u_{x}v_{y} - u_{y}v_{x})\mathbf{k}$$

#### References

-   Hamilton's multiplication rules, letter:
    [https://www.maths.tcd.ie/pub/HistMath/People/Hamilton/QLetter/QLetter.pdf](https://www.maths.tcd.ie/pub/HistMath/People/Hamilton/QLetter/QLetter.pdf)

### Q: How do I get Euler expression and yaw, pitch, roll for quaternions from the previous analysis?:

I'm interested in arriving to the expressions for yaw, pitch and roll from previous steps.

$$\mathbf{q} = e^{\frac{1}{2}\theta(u_{x}\mathbf{i} + u_{y}\mathbf{j} + u_{z}\mathbf{k})} = \cos\frac{\theta}{2} + (u_{x}\mathbf{i} + u_{y}\mathbf{j} + u_{z}\mathbf{k})\sin\frac{\theta}{2}$$

### From the Euler representation elements to $\mathbf{q}$

[How to convert Euler angles to Quaternions and get the same Euler angles back from Quaternions?](https://math.stackexchange.com/questions/2975109/how-to-convert-euler-angles-to-quaternions-and-get-the-same-euler-angles-back-fr)

$$\mathbf{q} = R + u_{x}\mathbf{i} + u_{y}\mathbf{j} + u_{z}\mathbf{k}$$

-   **Yaw**:

$$\hat{\psi} = e^{i\psi}$$

-   **Pitch**:

$$\hat{\theta} = e^{i\theta}$$

-   **Roll**:

$$\hat{\phi} = e^{i\phi}$$

$$R = \sin\left( \frac{\phi}{2} \right)\cos\left( \frac{\theta}{2} \right) - \cos\left( \frac{\phi}{2} \right)\sin\left( \frac{\theta}{2} \right)\sin\left( \frac{\psi}{2} \right)$$

$$u_{x} = \cos\left( \frac{\phi}{2} \right)\sin\left( \frac{\theta}{2} \right)\cos\left( \frac{\psi}{2} \right) + \sin\left( \frac{\phi}{2} \right)\cos\left( \frac{\theta}{2} \right)\sin\left( \frac{\psi}{2} \right)$$

$$u_{y} = \cos\left( \frac{\phi}{2} \right)\cos\left( \frac{\theta}{2} \right)\sin\left( \frac{\psi}{2} \right) - \sin\left( \frac{\phi}{2} \right)\sin\left( \frac{\theta}{2} \right)\cos\left( \frac{\psi}{2} \right)$$

$$u_{z} = \cos\left( \frac{\phi}{2} \right)\cos\left( \frac{\theta}{2} \right)\cos\left( \frac{\psi}{2} \right) + \sin\left( \frac{\phi}{2} \right)\sin\left( \frac{\theta}{2} \right)\sin\left( \frac{\psi}{2} \right)$$

#### From the quaternion elements to Euler representation

$$t_{0} = 2 \cdot (u_{z} \cdot R + u_{x} \cdot u_{y})$$

$$t_{1} = 1 - 2 \cdot (R^{2} + u_{x}^{2})$$

$$t_{a} = 2 \cdot (u_{z} \cdot u_{x} - u_{y} \cdot R)$$

$$t_{b} = \left\{ \begin{matrix}
1 & {\text{if}\ t_{a} > 1} \\
t_{a} & \text{otherwise}
\end{matrix} \right.$$

$$t_{2} = \left\{ \begin{matrix}
{- 1} & {\text{if}\ t_{b} < - 1} \\
t_{b} & \text{otherwise}
\end{matrix} \right.$$

$$t_{3} = 2 \cdot (u_{z} \cdot u_{y} + R \cdot u_{x})$$

$$t_{4} = 1 - 2 \cdot (u_{x}^{2} + u_{y}^{2})$$

Allowing us to get:

$$z_{a} = t_{0} + it_{1}$$

$$z_{b} = t_{3} + it_{4}$$

$$\phi = \arg(z_{a})$$

$$\theta = \arcsin(t_{2})$$

$$\psi = \arg(z_{b})$$

-   Q2: Is this so far correct? How can I derive these expressions starting with 3 complex numbers each of one representing rotation in 3 different orthogonal planes?. I'd be happy with some hints.

-   Q3: I'd like to complete this and create the multiplication table and associate it with the 6 degrees of freedom in terms of the 3 planes of rotation in terms of the pitch , yaw and roll arriving to a clear way to see these rules but still didn't figure it out completely and some help would be much appreciated.

### Related Wikipedia images

[https://commons.wikimedia.org/w/index.php?curid=145997978](https://commons.wikimedia.org/w/index.php?curid=145997978)

[https://commons.wikimedia.org/w/index.php?curid=10878582](https://commons.wikimedia.org/w/index.php?curid=10878582)

[https://commons.wikimedia.org/w/index.php?curid=9441238](https://commons.wikimedia.org/w/index.php?curid=9441238)

(edit) I see a up vote and then a close vote and I'd like to know what to improve or clean in the question
