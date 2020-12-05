## What is a permutation?
A permutation is an ordered arrangement of the elements in a set. For instance, consider the set of the numbers 1 through 3. There are six possible permutations of this set: (1,2,3), (1,3,2), (2,1,3), (2,3,1), (3,1,2), and (3,2,1). On the set of numbers {1, ..., n}, there are n! permutations.

## Generating a random permutation
Let's say we wanted to write a program that would simulate shuffling a standard deck of 52 cards. Well, this seems simple enough: number each card, and then generate a random permutation of the set of numbers from 1 to 52. The position of each number in our permutation then becomes the position of the corresponding card in our shuffled deck. 

But how do we actually generate the permutation? 52 isn't a very large number, but the number of possible permutations - 52 factorial - is huge. Using a "brute force" method of listing all possible permutations and then picking one at random is very computationally expensive. Here's one approach that's easier for the computer to handle:

1) Begin with all the numbers {1, 2, ..., 52} listed in ascending order.

2) Pick two indices (between 1 and 52 inclusive) uniformly at random.

3) If the same index is picked twice, do nothing.

4) If two different indices are picked, swap the numbers at those indices.

5) Repeat steps 2-4 for some number of iterations.

The obvious question here is, how many times do we need to repeat the process? If we just do one or two swaps, we'll end up with a permutation very close to our starting state - the order of the cards will barely change at all. It turns out that for a set of n elements, it takes about 0.6nlog(n) steps for this process to converge to a random permutation. So for our deck of cards, we'll want to run the program for around 0.6 * 52log(52) = 123 steps. 

## Is it really random?
###   Footrule
###   Spearman's rank correlation
###   Hamming distance
###   Kendall's tau

## Examples
###   Running for 0.3nlog(n) steps
###   Running for 0.7nlog(n) steps

