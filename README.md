# get-rnd-elem-uniformly-distributed-prob
A very efficient way of selecting a random element from a collection whose elements have their probability distributed uniformly. Particularly useful in simulations and games.
## Problem statement
Let's imagine a collection of pairs (element, probability) that are more-or-less uniformly distributed by their probability. For instance, this one:

{ (A, 0.10), (B, 0.15), (C, 0.20), (D, 0.25), (E, 0.30) }   

How could we select a random element from such collection ? A classic approach might well consist in creating another collection of 100 elements and include:
- 10 A elements
- 15 B elements
- 20 C elements
- 25 D elements
- 30 E elements

Then it will only require to select an element in [0, 100] interval. This is an easy-to-implement approach, but... What if the cardinality of elements is extremely big ? Unfortunately, this is a common scenario in some simulations and games.
## Proposed solution
Let's now try a twist. Firstly, we could model the probability values using simple integer numbers that are being incremented by one as they increase their probability:
{ (A, 1), (B, 2), (C, 3), (D, 4), (E, 5) }   
But now, for this model, instead of constructing another collection, we could build a "triangle" in which each of its rows would represent a set of values that are modelling the probability of one of the elements. So, in our example:
| Element | Values                 |
|---------|------------------------|
| (E, 5)  | { 11, 12, 13, 14, 15 } |
| (D, 4)  | { 7, 8, 9, 10 }        |
| (C, 3)  | { 4, 5, 6 }            |
| (B, 2)  | { 2, 3 }               |
| (A, 1)  | { 1 }                  |

If we look at the rows carefully, we can find the following invariant:

_"Maximum value of each row is half the product of its row number by the next one"_

So:

![formula](https://render.githubusercontent.com/render/math?math=\color{red}r%20*%20(r%20%2B%201)%20=%202n%20\rightarrow%20r^2%20%2B%20r%20-%202n%20=%200\rightarrow%20r%20=%20\frac{\sqrt{8n%20%2B%201}%20-%201}{2})

This way, we could just generate a number between [1, 15] and applying the formula above, determine which is its corresponding row and consequently, its element. We only must bare in mind that if our result it is not an exact number, we should apply then a ceiling to the next integer number.
