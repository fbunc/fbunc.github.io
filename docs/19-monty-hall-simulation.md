# Simulating the Monty Hall problem — answer

*My answer on [math.stackexchange.com](https://math.stackexchange.com/a/4677997) — score 1.*

## Question: How to simulate Monty Hall problem in computer?

[Original question](https://math.stackexchange.com/q/152006)

Suppose you're on a game show, and you're given the choice of three doors: Behind one door is a car; behind the others, goats. You pick a door, say No. 1 [but the door is not opened], and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" Is it to your advantage to switch your choice?

This is Monty Hall problem, and we know that a switching strategy really does win two out of three times on the average. But is it possible to simulate it in Matlab?


---

## My answer

# Python code n doors k revealed

Here another way in python to compare simulations with the general result in

[Monty hall problem extended.](https://math.stackexchange.com/questions/608957/monty-hall-problem-extended)

If extended to $n$ doors, $k$ doors are revealed by the host after the first choice, the probability of winning with switching and without switching changes slightly.

Here is the general formula for the Monty Hall problem with $n$ doors and $k$ revealed doors:

- If the player does not switch :

$$P(win) = 1/n$$

- If the player switches and the number of revealed doors is $k=n-2$ :

$$P(win) =\frac{n-1}{n}$$

- And in general for any number k  of revealed doors:

$$0\leq k\leq n-2$$

$$P(win)=\frac{n-1}{n}\cdot \frac{1}{n-k-1} = \frac{1}{n} \cdot \frac{n-1}{n-k-1} \geq \frac{1}{n}$$

`import random
from fractions import Fraction
n=15
k=13# To simulate the original case k should be k=n-2 where p(win)=(n-1)/n
p_win_sw=((n-1)/n)*(1/(n-k-1))
p_win_no_sw=1/n

print('Total number of doors: ',n,'Number of revealed doors: ',k)
print("**********************************************************")
print('Probability of winning with switching: ',p_win_sw," = ", Fraction.from_float(p_win_sw).limit_denominator())
print('Probability of winning with NO switching:',p_win_no_sw," = ", Fraction.from_float(p_win_no_sw).limit_denominator())

def simulate_game(switch, num_doors, k_revealed):
    # Define the doors as a list of goats and a car
    doors = ['goat'] * (num_doors - 1) + ['car']

    # Shuffle the doors randomly
    random.shuffle(doors)

    # User chooses a door
    first_choice = random.randint(0, num_doors - 1)

    # Host reveals k_revealed doors with goats
    revealed_doors = []
    for i in range(num_doors):
        if i != first_choice and doors[i] == 'goat' and len(revealed_doors)

