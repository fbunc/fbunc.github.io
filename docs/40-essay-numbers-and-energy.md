# Numbers and Energy — the pun $E = mc^2$ for polygons

> A narrated essay on symmetry, roots of unity, and how the identity $\sum Z_k^2 = \sum Z_k Z_{k+1}$ for regular $m$-gons plays the role of a "conservation of symmetry" — the pun behind an $E = mc^2$ for numbers.
> 
> **Source:** `MATH_THEATER/ENERGY_AND_NUMBERS/final_material/0_last/numbers_and_energy.md` (hand-written master script, voice-over + animation tracks merged here for reading).

## Puzzle shared by S. Strogatz in X

One always remembers a good teacher, that with a simple elegant problem introduces you to a vast world of ideas if you follow the thread. This puzzle is simple, and easy to solve for someone with undergraduate knowledge of complex numbers. The idea is to take advantage of the two dimensional nature of complex numbers, and to take advantage of the cartesian and polar coordinates to describe the collection of points that form a triangle, or any polygon in the plane.

If you are are familiar with basic trigonometry and vectors, and you are interested in a swift introduction to complex numbers, taking notes with pencil, straightedge and compass while watching this video, is recommended. For now just think as complex numbers as points in the 2D plane.

A collection of points can be described by their cartesian coordinates, X and Y, in the plane, or by their polar coordinates. If you are not familiar with polar coordinates, you can think of them as using a compass to measure the distance from the origin and the angle with the positive x-axis.

That is why a compass is useful to follow the video.

Consider an equilateral triangle in the complex plane whose vertices are given by three complex numbers $Z_1$, $Z_2$, $Z_3$:

Show that the following identity holds for any equilateral triangle :

$$
Z_1^2 + Z_2^2 + Z_3^2 = Z_1Z_2 + Z_2Z_3 + Z_3Z_1
$$

This symmetry hints at a deeper structure, one that can might be extended to regular $m$-gons.

## The general triangle centered at $c$

I tried to solve the puzzle, and found a simple solution that was valid only if the equilateral triangle was centered at the origin. I shared the problem and my solution in Discord, and they found a general solution that was valid for any triangle centered at a complex number c. I was really inspired by this solution, and it provided me with an enhanced understanding of how complex numbers operate when used in Fourier Series and Fourier Transforms.

I'd recommend you pause, ponder and try to find a solution that is valid for any triangle, and explore what happens if the triangle is not equilateral. Adding complex numbers is like adding vectors in the plane. So we put one on top of the other, and the result is the vector that goes from the origin to the tip of the sum.

Always measure angles from the horizontal right side of the plane going counter-clockwise.

Let’s consider a regular $m$-gon centered at a complex number $c$

Considering a general $m$-gon centered at a complex number $c$, we can describe it as follows:
$$
\omega = e^{\frac{2\pi i}{m}},
$$

$$
Z_k = c + r\,\omega^{\,k-1},\quad k = 1, 2, \ldots, m,
$$

$c$ is a fixed center in $\mathbb{C}$, and $r$ is a complex “radius.” Meaning it can encode an initial phase, along with it's fixed modulus. 

We will see visually how this works in next sections, starting from a simple triangle to build intuition.

## Complex numbers, vectors, roots of unity and regular polygons

If you draw an equilateral triangle centered at the origin, then the sum of the complex numbers representing the vertices results in zero. In this workshop we are going to see why this is true, and how it can be extended to regular polygons. The magic of something called, Roots of Unity, and if you are not familiar with them, and follow the thread you will undestand the core of the matter.

This is because they end up being 120 degrees apart, and if you do this in pen and paper you will see that the sum of the vectors is zero.

If an equilateral triangle is centered at a complex number $c$, then the sum of the vertices is: 

$$
Z_1 + Z_2 + Z_3 = 3c
$$

You can easily verify this property setting $c=0$ and $r=1$ using your straightedge and compass.

$$
Z_1 = c + r e^{\frac{2\pi i}{3}0} = 1
$$

$$
Z_2 = c + r e^{\frac{2\pi i}{3}1} = \frac{1}{2} + \frac{\sqrt{3}}{2}i
$$

$$
Z_3 = c + r e^{\frac{2\pi i}{3}2} = \frac{1}{2} - \frac{\sqrt{3}}{2}i
$$

## Remember the goal

This is the goal of this video, to show that for such a polygon the following holds, don't worry if you don't understand the notation, it will be explained throughout the video.

We wish to show that for such a polygon the following holds:
$$
\sum_{k=1}^{m} Z_k^2 = \sum_{k=1}^{m} Z_k Z_{k+1},
$$

Consider that when we are going around the polygon, when we reach the last vertex, we go back to the first one, so 

$$
Z_{m+1}=Z_1
$$
