# Möbius transform: half-disc to first quadrant — answer

*My answer on [math.stackexchange.com](https://math.stackexchange.com/a/4943929) — score 1 · accepted.*

## Question: Möbius transformation mapping the upper half of the unit disc to the first quadrant

[Original question](https://math.stackexchange.com/q/4942850)

Find the Möbius transformation that maps the region $D_1 = \{ z \in \mathbb{C} \mid |z|  0 \}$ to $D_2 = \{ z \in \mathbb{C} \mid \operatorname{Re} z > 0 \land \operatorname{Im} z > 0 \}$.

**Attempt:** To find the Möbius transformation that maps the upper half of the unit disk $ D_1 = \{ z \in \mathbb{C} \mid |z|  0 \} $ to the first quadrant $ D_2 = \{ z \in \mathbb{C} \mid \operatorname{Re} z > 0 \wedge \operatorname{Im} z > 0 \} $, I use the transformation $ f(z) = i \frac{1 - z}{1 + z} $. This transformation maps the key point $ z = i $ to $ w = 1 $ and $ z = 0 $ to $ w = i $, effectively mapping $ D_1 $ to $ D_2 $. Is this conclusion ok?


---

## My answer

The idea is to show that for the  upper half of the unit disc as input ($D_1$) to $f(z)$ you have only values in the 1st quadrant as an output ($D_2$).

For $z$ being in the upper half of the unit disc we have

$$|z| 

and

$$\operatorname{Im} z > 0$$

$$z = x + iy$$

Meaning we define this restrictions to the input:

$$x^2 + y^2 

The previous inequality can be written as:
$$1-x^2 - y^2  >0 $$

And we also have the restriction over $y$:

$$y > 0$$

So $f(z)$ can be expressed in terms of $x$ and $y$:

$$ f(z) = i \frac{1 - z}{1 + z} $$

$$f(z) = i \frac{(1 - x) - iy}{(1 + x) + iy}$$

$$f(z) = i \frac{(1 - x) - iy}{(1 + x) + iy}\frac{(1 + x) - iy}{(1 + x) -iy}$$

$$f(z) = i \frac{((1 - x) - iy)((1 + x) -iy)}{(1 + x)^2 + y^2}$$

$$f(z) = i \frac{(1 - x^2)-iy(1 - x)-iy(1+x)-y^2}{(1 + x)^2 + y^2}$$

$$f(z) = i \frac{1 - x^2-iy + iyx-iy -iyx-y^2}{(1 + x)^2 + y^2}$$

$$f(z) = i \frac{1 - x^2 -y^2 -i2y }{(1 + x)^2 + y^2}$$

$$f(z) =  \frac{ 2y+i(1 - x^2 -y^2) }{(1 + x)^2 + y^2}$$

Arriving to the final expression:

$$f(z) =  \frac{ 2y }{(1 + x)^2 + y^2}+i\frac{ 1 - x^2 -y^2 }{(1 + x)^2 + y^2}$$

From here is easy to see that given $y>0$ and $1 - x^2 -y^2>0$ we are going to have both the real and imaginary parts of $f(z)$ in the first quadrant.

[![enter image description here](https://i.sstatic.net/rEmmC7ak.png)](https://i.sstatic.net/rEmmC7ak.png)

