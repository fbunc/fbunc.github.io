# About the question

I'm sorry, I think the question was vague, because I really wanted to make a wider question but while writing it I focused on the part about periodicity with some big omissions. 

I fix that now, and I think this answers the title of my question.Maybe someone can add a better answer after this. 

The idea in the question about using mirrored curves is related to an idea of other way of watching correlation or  periodicity using the superposed orbits for that. But maybe that requires a new question.



# Rationality and Periodicity 


$$\omega=e^{\frac{2\pi i}{T_o}}$$

$$n \in \mathbb{N}_0$$

Let's analyze periodicity of $\omega^n$ with these restrictions being $p_o$ and $q_o$ integers:

$$T_o=\frac{p_o}{q_o}$$

$$T_o \in \mathbb{Q}$$



For a chosen $p_o$ and $q_o$ we want to find values of P such as 

$$\omega^{n+P}=\omega^{n}$$

$$\omega^{n}\omega^{P}=\omega^{n}$$

As we are going to choose $p_o$ and $q_o$ as integers that  provide a rational number as close as possible to an irrational.So we have to add an extra restriction :


$$ p_o \not\equiv 0 \pmod{q_o}$$


For 
$$k,k_o \in \mathbb{Z}$$

$$\omega^{P}=e^{2\pi i k}=\omega^{kT_o}$$

Meaning:

$$P=kT_o=k\frac{p_o}{q_o}$$ 

it's valid only for restricted $k$ 
$$ k \equiv 0 \pmod{q_o}$$

For $k=k_oq_o$ we have

$$P=k_o q_o\frac{p_o}{q_o}=k_op_o$$

So the periods can be multiples of $p_o$


In the example of  $T_o=\varphi$ and the $M_c=13$ in the coloring being in the Fibonacci sequence, it made a nice plot but  not periodic in the sense I was looking for, but it made me lose myself into the spirals instead of properly thinking that for example taking a worse approximation of $\varphi$ using: 

$$T_o=\frac{F_N}{F_{N-1 }}$$  

$F_n$ is the nth Fibonacci number

For big values of $N$ we get better and better approximations 

# $M=1$  $M_c=13$ and $T_o=\frac{2584}{1597}$ 

This shows how good the approximation of the irrational is, as you get a similar plot as in the question. 

[![enter image description here][1]][1]

## $M=1$ $M_c=p_o=2584$ and $T_o=\frac{2584}{1597}$ 

So If the analysis I made before works, I should see the colors growing along the string, not in other spirals as it was in using the wrong $M_c$ for the purpose.

The resulting image should look like the examples given in the question where $T_o$ was a big integer. 

[![enter image description here][2]][2]

## The rational approximation of the value of $\varphi$ in the question
In the question plots the approximation is : 

$$T_o=\frac{1618033988749895}{ 10^{15}}$$

But using that same $T_o$ but considering $M_c=2548$ you still get a similar ordering of the colors:

## $M=1$ $M_c=2584$ $T_o=\frac{1618033988749895}{10^{15}}$

[![enter image description here][3]][3]

It looks like it works. Looking closely we see :

[![enter image description here][4]][4]


## Here  $M=1$ $M_c=p_o=2584$ and $T_o=\frac{2584}{1597}$  in hsv from the beginning
[![enter image description here][5]][5]

# Rationality of continuous time frequency 

We can imagine a continuous-time function as a 2D number like:

$$\theta=e^{\frac{2\pi i}{T}}$$

For simplicity, the   continuous-time can be considered as $t \in \mathbb{R}$ and let's add some more restrictions

$$T \in \mathbb{Q}$$

$$f \in \mathbb{Q}$$

$$n \in \mathbb{N}_0$$

$$ q,p \space \in \mathbb{Z}$$

$$T=1/f$$

$$f=q/p$$


$$\theta^t=e^{\frac{2\pi i}{T}t}$$

We will consider cases where $q/p$ is integer,rational and make it also a rational approximation of an irrational number.


To get a valid discrete-time sequence representing this continuous time function we need to choose a sampling frequency:

$$f_s=\frac{1}{T_s}$$

$$\theta^{nT_s}=e^{\frac{2\pi i}{T}nT_s}$$

$$\theta^{nT_s}=e^{\frac{2\pi i}{\frac{T}{T_s}}n}$$

So by Nyquist-Shannon sampling theorem we know that to avoid aliasing we need :

$$f_s \geq 2f$$

We want $f_s$ to be positive integer, allowing us to find an integer period  $P$ for our discrete-time signal.

Discrete-time function  as a 2D number $\omega$ 

$$\omega^n$$

$$T_o \in \mathbb{Q}$$

$$ P,k_o,k \space \in \mathbb{Z}$$

$$T_o \geq 2$$

$$T_o=\frac{f_s}{f}=\frac{T}{T_s}$$


$$\omega=e^{\frac{2\pi i}{T_o}}$$


$$\theta^{nT_s}=e^{\frac{2\pi i}{T_o}n}=\omega^{n}$$

$$\omega^{n+P}=\omega^{n}$$

$$\omega^{n}\omega^{P}=\omega^{n}$$


$$\omega^{P}=e^{2k\pi i}=\omega^{kT_o}$$

$$P=kT_o$$


$$P=k \frac{f_s}{f}$$


$$P=k \frac{f_s}{\frac{q}{p}}$$

$$P=k \frac{p}{q}f_s$$

This is valid por restricted k

$$k=k_oq$$



$$P=k_o p f_s$$

So the periods are going to be affected by the combination of $k_o,q,p$ and the selected $f_s$


# Not all irrationals are created equal and Why are Most Polygons Impossible to Construct?:

I think a similar approach can be used for $T_o=\sqrt{2}$ using a good approximation . Im sure I'm missing something in this explanation, but it's a good start. 

Another Roof channel made a video that is a gem about the formal foundations of all this.

https://www.youtube.com/watch?v=Gdy1u4lsjDw

# Almost Periodic Functions 

The comment from Gerry has really interesting reads on for example: 
https://en.wikipedia.org/wiki/Almost_periodic_function

This led me to make a similar construction using:
$$ s  \in \mathbb{C}$$

$$ n  \in \mathbb{N}$$

In this case we have to remove 0 from the natural numbers: 

$$n \geq 1$$

$$ s=\sigma+i \omega t$$


$$\omega=\frac{2\pi}{T}$$

$$\zeta_{n}(\sigma,\omega,t)= n^{-s}=e^{-\sigma log(n)}e^{-ilog(n) \frac{2\pi}{T}t} $$

For a "carefully selected" $T_o=\frac{T}{T_s}$ and $\sigma$ we could write:

$$ n^{-s} \approx Z_n=e^{-\sigma log(n)}e^{-i (log(n) \frac{2\pi}{T}T_s) n} $$

$$\omega_n= -log(n) \frac{2\pi}{T_o}$$ 

$$ n^{-s} \approx e^{-\sigma log(n)}e^{i \omega_n n} $$


Or instead of using a fixed $T_o$ and $\sigma$ we make them sequences as well as carefully curated  "modular" functions  $f$ and $g$

$$T_n=f[n]$$ 

and 

$$\sigma_n=g[n]$$ 

# Backward differences sequences -  Newton discrete calculus & meaning of the discrete-j*rk

I imagine this is somehow related to this fact you easily realize while doing the higher order backward differences on any sequence and see the pascal triangle pattern emerge:

$$\Delta^N Z_n = \sum_{k=0}^{N} (-1)^k \binom{N}{k} Z_{n-k}$$

Note that this is closely related to :
$$
\sum_{k = 0}^{N}{N \choose k}\sin\left(\frac{\pi k}{N}\right) = \Im\sum_{k = 0}^{N}{N \choose k}\left(\exp\left(\frac{i \pi}{N}\right)\right)^{k} =
\Im\left(1 + \exp\left(\frac{i \pi}{N}\right)\right)^{N}
$$
So
 $$
2^{N}\left|\cos\left(\frac{\pi}{2N}\right)\right|^{N}
$$

## The natural numbers represented as colors $C_n$  are cyclic and produce an interesting set of loop sequences for the initial example with initial conditions $T_o=\varphi$ and $M_c=13$ 

$$C_n \equiv n \pmod{M_{c}}$$

If we call the first difference speed, the second acceleration the third  receives an unfortunate name we all try to avoid being or being around.


0,8,3,11,6,1,9,4,12,7,2,10,5 being in A257961 and A025636 should explain how n mod 13 worked as a combination machine , being 13 the constant value of  the first backward difference of enough order   to provide a constant integer sequence? 

For loop 0,8,3,11,6,1,9,4,12,7,2,10,5 if I remember correctly it's the 3'd order difference that  is constant 13. Being as well the result of adding of the (0+1)th and (13-1)th elements. 

* The same happens for the $(0+k)$th and the $(M_c-k)$th elements added are also $M_c$


 


# OEIS - Cellular automata sequences and binary counting

I started reading about Cantor's dust, and ended up here:

https://oeis.org/wiki/Index_to_OEIS:_Section_Ce#cell

Specially About the pascal triangle pattern emerging:  A047999 and A102037 showing how it can be implemented with logic gates see this by Wolfram: 

https://mathworld.wolfram.com/SierpinskiSieve.html 

"Gardner (1977) and independently Watkins (Conway and Guy 1996, Krížek et al. 2001) noticed that the number of sides for constructible polygons with odd Numbers of sides are given by the first 32 rows of the Sierpiński sieve interpreted as binary numbers, giving 1, 3, 5, 15, 17, 51, 85, 255, ... (OEIS A004729, Conway and Guy 1996, p. 140). In other words, every row is a product of distinct Fermat primes, with terms given **by binary counting**.
"

## Circle of fifths  creating Sierpiński triangle shape algorithm by counting:

Saw this initially here:

https://youtube.com/shorts/A7xMJ639gAw?si=vr9JzHWUt7zdFeHt

0. Draw the circle of fifths as an HSV colored wheel with M=12 slots using M colors and add M labels for the notes. radius=1
  ['A','D','G','C','F','Bb','Eb','Ab','Db','Gb','B','E'] is mapped to an index 

$$k=[0,1,2,3,4,5,6,7,8,9,10,11]$$
  The position in the complex plane for the circle of fifths and its 12 notes is given by
 
  $$S_k=e^{i\frac{2\pi}{M}k}$$
  
  


  

1. Randomly choose a point inside a unitary circle - Express it as a complex number 
    
    $$Z_o=r_o e^{\phi_o}$$ 
    
    There are many ways to select a random point inside a unitary circle,we try selecting a radius and and an angle, as if we where doing it with a compass.   
    
    $$r_{min}<r_o<r_{max}$$ 
    
    and 
    
    $$-\pi<\phi_o<\pi$$

    With a resolution $M_c$ points per interval  

    

2. Randomly choose between this three notes $(A_b,E,C) or (7,11,3) $ note in the circle of fifths 
   and express it as a complex number $Z_{note}=S_k$ using the mapping given by the index
    
    $$S_k=e^{i\frac{2\pi}{M}k}$$ 
    
    always expressing the notes in this order:
    
    ['A','D','G','C','F','Bb','Eb','Ab','Db','Gb','B','E']

    
    $$0<k<M-1$$
    $$k=[0,1,2,3,4,5,6,7,8,9,10,11]$$

    * A_b is k=7

    * E is k=11

    * C is k=3


3. Draw a line between $Z_o$ and $Z_{note}$

4. Find the mid point between $Z_o$ and $Z_{note}$ and call it $Z_{\frac{1}{2}}$

5. Set $Z_o=Z_{\frac{1}{2}}$ and repeat $N_{iter}$ times from step 2 using this new value of $Z_o$ in step 3

[![enter image description here][6]][6]

[![enter image description here][8]][8]


## Computational Complexity and irrational numbers

Here in MS I found this interesting context :

https://math.stackexchange.com/q/307157 

"
There are only a countable number of Turing Machines, thus a countable number of algorithms for computing irrational numbers. But there are an uncountable number of irrational numbers. So we cannot compute them all.
"

# How to create ellipses and square orbits. From introducing the most basic epicycles to the Gibbs phenomenon

What would have happened if Ptolemy met Fourier and Lissajous ?

Epicycles and the heat equation would have met as well(or any other feature we can model as a color in the proposed model). 
  
Maybe they ended up trying to add more and more epicycles to end up describing a square orbit just for the pleasure of it. 

Using $Z_t$ as a short way to write some $z(t)$ we can imagine they wrote something like this after a long night:

$$ R_{p} , R_{q} \in \mathbb{C}$$  

$$Z_t=R_{p} e^{i\omega t} +R_{q} e^{-i\omega t}$$ 

$$Z_t=R_{p} e^{i\omega t} +R_{q} e^{-i\omega t}$$ 



$$|R_p|>|R_q|$$ 



$$F_{pq}=\pm 2\sqrt{R_p R_q}$$

[![enter image description here][7]][7]




  [1]: https://i.sstatic.net/x2MCIeiI.png
  [2]: https://i.sstatic.net/MEtQTspB.png
  [3]: https://i.sstatic.net/65Ne4yfB.png
  [4]: https://i.sstatic.net/ftgxbL6t.png
  [5]: https://i.sstatic.net/KXbOPmGy.png
  [6]: https://i.sstatic.net/p8psi9fg.png
  [7]: https://i.sstatic.net/OBYZfG18.png
  [8]: https://i.sstatic.net/Zbn5ltmS.png