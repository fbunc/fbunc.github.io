# Numbers as naturally associated with energy (soft question)

> **Source:** [math.stackexchange.com/q/4942109](https://math.stackexchange.com/q/4942109)  ·  **archived (deleted from MSE)**
> **Tags:** `geometry`, `complex-numbers`, `soft-question`, `roots-of-unity`
> **Asked:** 2024-07-05 16:21:27Z

## Question

# Introduction

This is a soft-question divided in two parts, and it contains a pun that shows the symmetry of the roots of unity in a fun way.

I'd love to see how mathematicians think about this things. Maybe it's more suitable question to make in Philosophy SE. Let me know.

I came across this question after playing with this puzzle posted in twitter by Steven Strogatz:

-   If a triangle has vertices represented by complex numbers $Z_{1}$,$Z_{2}$ , $Z_{3}$ prove that :

$$E = Z_{1}^{2} + Z_{2}^{2} + Z_{3}^{2} = Z_{1}Z_{2} + Z_{2}Z_{3} + Z_{1}Z_{3}$$

## Why using the world "naturally" in the title of the question

**Q1:**
I'm investigating as a newbie the notion of "natural" in mathematics, as in natural-transform for example. My intuition tells me that the notion of some construction being thought as "naive" correlates sometimes to cases where the construction is "natural"? I'd love to see clear examples of the word "natural" being used in different contexts.

Math for an outsider, appears to be a game about the extension of simple notions as counting, to infinitely-complex abstract worlds that present beauty in it's structure. Sometimes this beauty is left only to trained-mathematicians to discover, but it's easy to at least realize that:

-   "essentially everything in math can be interpreted as an object or a functor":

[what is the meaning of a "free" and a "natural"](https://math.stackexchange.com/questions/4402164/what-is-the-meaning-of-a-free-and-a-natural)

-   "naive refers to what is usually the easiest approach, the one that takes the least effort to attempt, or the one that doesn't take into account the potential intricacies that are revealed by a more careful study of the question"

[What does it mean to call something "Naive" in mathematics?](https://math.stackexchange.com/questions/4218546/what-does-it-mean-to-call-something-naive-in-mathematics)

## The pun : $E = mc^{2}$ of mathematics. A pun about symmetry and it's conservation

-   $c$ can be imagined as a probably-moving center of a "perturbable-polygon" that has vertex expressed by a general "rotating" complex number of the form $Z_{k} = c + r\omega^{k - 1}$

-   To keep things simple,we can naively associate to each vertex the same unit we can call "mass-unit", to be able to say for the pun that if we accumulate $m$ numbers describing a curve around a center $c$ we can think that we have a **total "mass" of $m$ units**

-   We are going to look at a generalization of this fact looking at previous expression LHS and RHS separately and see under what conditions we "conserve" symmetry conditions to prepare the ground to see growing Schläfli star polygons for the rational values of $m$ and add some "perturbations" to the system.

-   For a general $m$-gon we can index and label the vertices of the resulting polygons, and associate some information/computation to them.

-   Labeling is the action of associating a set of simple computations to each input-number,that maps each index to higher-dimensional representations encoding some information related to the input-number behavior after a transformation.

$$k = 1,2,3,\ldots,m$$

$$Z_{k} = c + r\omega^{k - 1}$$

-   Where $c$ (center) and $r$ (radius) are complex constants in the case of simple polygons, and **in general they can be complex sequences associated to continuos-time-functions** that modulate the $m$-gons shape according to another set of rules.

$$\omega = e^{\frac{2\pi i}{m}}$$

### LHS

$$\sum\limits_{k = 1}^{m}Z_{k}^{2} = \sum\limits_{n = 1}^{m}\left( c^{2} + 2cr\omega^{n - 1} + r^{2}\omega^{2(n - 1)} \right)$$

$$= mc^{2} + 2cr\left( 1 + \omega + \omega^{2} + \ldots + \omega^{m - 1} \right) + r^{2}\left( 1 + \omega^{2} + \omega^{4} + \ldots + \omega^{2(m - 1)} \right)$$

Using symmetry, you'll see that both these sums of roots of unity are 0, regardless of whether $n$ is even or odd.

-   If we define as a sequence's complex-energy E the following expression:

$$E = \sum\limits_{k = 1}^{m}Z_{k}^{2} = mc^{2} + 2cr\sum\limits_{n = 0}^{m - 1}\omega^{n} + r^{2}\sum\limits_{n = 0}^{m - 1}\omega^{2n}$$

-   Note for context the example of the energy associated to a discrete-time signal is a similar expression of the square of the modulus , and the sum going from $- \infty$ to $+ \infty$

So the LHS for $m$-gons :

$$E = mc^{2}$$

### RHS

Note that by definition

$$Z_{k} = c + r\omega^{k - 1}$$
$$Z_{k + 1} = c + r\omega^{k}$$

So RHS is:

$$E = \sum\limits_{k = 1}^{m}Z_{k}Z_{k + 1} = mc^{2} + cr(\omega + 1)\sum\limits_{n = 0}^{m - 1}\omega^{n} + r^{2}\sum\limits_{n = 1}^{m}\omega^{2k - 1}$$

So for $m$-gons (and symmetry of the roots of unity) we are only left with:

$$E = mc^{2}$$

Showing the LHS and RHS are equal for this simplified example of regular $m$-gons.

$$E = \sum\limits_{k = 1}^{m}Z_{k}Z_{k + 1} = \sum\limits_{k = 1}^{m}Z_{k}^{2} = mc^{2}$$

-   Now the interesting math comes apparent when you try to adapt this construction to use variable complex sequences $c = c_{n}$ and $r = r_{n}$ .

-   More interesting is when you start playing with the idea of how to modify the structure to allow using a rational value of $m$ that approximates an irrational number through a sequence of transformations $m_{n} = f\lbrack n\rbrack$

**Q2:**

-   What can be added to this pun and meandering question to add mathematical depth and context, and guide people that,like me, are interested in mathematics in a "naive" way, but are willing to study hard and learn more about these topics and peripheral areas of study?

The same type of answer is what I'm looking for in this question : [Irrational numbers as "periods" in discrete sequences based on complex exponentials](https://math.stackexchange.com/questions/4911385/irrational-numbers-as-periods-in-discrete-sequences-based-on-complex-exponenti)
