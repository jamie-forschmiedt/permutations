## What is a permutation?
A permutation is an ordered arrangement of the elements in a set. For instance, consider the set of the numbers 1 through 3. There are six possible permutations of this set: (1, 2, 3), (1, 3, 2), (2, 1, 3), (2, 3, 1), (3, 1, 2), and (3, 2, 1). On the set of numbers 1 through n, there are n! permutations.

## Generating a random permutation
Let's say we wanted to write a program that would simulate shuffling a standard deck of 52 cards. Well, this seems simple enough: number each card, and then generate a random permutation of the set of numbers from 1 to 52. The position of each number in our permutation then becomes the position of the corresponding card in our shuffled deck. 

But how do we actually generate the permutation? 52 isn't a very large number, but the number of possible permutations - 52! - is huge. Using a "brute force" method of listing all possible permutations and then picking one at random is very computationally expensive. Here's one approach that's easier for the computer to handle:

1) Begin with all the numbers {1, 2, . . ., 52} listed in ascending order.<br/>
2) Pick two indices (between 1 and 52 inclusive) uniformly at random.<br/>
3) If the same index is picked twice, do nothing.<br/>
4) If two different indices are picked, swap the numbers at those indices.<br/>
5) Repeat steps 2-4 for some number of iterations.<br/>

The obvious question here is, how many times do we need to repeat the process? If we just do one or two swaps, we'll end up with a permutation very close to our starting state - the order of the cards will barely change at all. It turns out that for a set of n elements, it takes about 0.6nlog(n) steps for this process to converge to a random permutation. So for our deck of cards, we'll want to run the program for around 0.6 * 52log(52) = 123 steps. 

## Is it really random?
###   Footrule
Consider the identity permutation π = (1, 2, 3, 4, 5, 6) and the permutation σ = (4, 1, 2, 5, 3, 6). The first test statistic we will compute is the sum of the absolute value of the difference between π(i) and σ(i), where 1 ≤ i ≤ n.

Footruld has these properties:

Mean = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{1}{3}(n^2 - 1)">

Variance = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{1}{45}(n+1)(2n^2+7)">

<img src="https://render.githubusercontent.com/render/math?math=\dfrac{\rho - Mean}{SD}"> has a standard normal limiting distribution.

###   Spearman's rank correlation
Using the same π and σ, The second test statistic we will compute is the sum of the square of the difference between π(i) and σ(i), where 1 ≤ i ≤ n.
###   Hamming distance
Using the same π and σ, The third test statistic we will compute is the total number of fixed point in σ, where π(i) = σ(i) and 1 ≤ i ≤ n. 
###   Kendall's tau
Consider the identity permutation π = (1, 2, 3, 4, 5, 6) and the permutation σ = (4, 1, 2, 5, 3, 6). The final test statistic we will compute is the number of inversions in σ compared to the identity permutation, or the number of pairs (i, j) where i < j and the number in the ith position is greater than the number in the jth position in σ. 

What does this look like concretely? Note that in a set of n elements, there are <img src="https://render.githubusercontent.com/render/math?math={n \choose 2}"> pairs of elements, so in our example, where n = 6, there are <img src="https://render.githubusercontent.com/render/math?math={6 \choose 2}"> = 15 pairs. How many of these are inversions? 

Let's look at the pair (1, 2). The first entry in σ is 4 and the second entry is 1. 4 > 1, so this is an inversion.

Now let's consider (1, 3). The first entry is 4 and the third entry is 2. 4 > 2, so this is also an inversion.

How about (1, 4)? The first entry is 4 and the fourth entry is 5. 4 < 5, so this is not an inversion.

Continue this process for all 15 pairs and we will find that there are 4 inversions: (1, 2), (1, 3), (1, 5), and (4, 5). 

The number of inversions is also equal to the minimum number of pairwise adjacent transpositions required to bring <img src="https://render.githubusercontent.com/render/math?math=\pi^{-1}"> to <img src="https://render.githubusercontent.com/render/math?math=\sigma^{-1}">. This statistic is known as I(π, σ), or Kendall's tau.

Kendall's tau has these properties:

Mean = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{n \choose 2}{2}">

Variance = <img src="https://render.githubusercontent.com/render/math?math=\dfrac{n(n-1)(2n %2B 5)}{72}">

<img src="https://render.githubusercontent.com/render/math?math=\dfrac{I - Mean}{SD}"> has a standard normal limiting distribution.

We are therefore able to calculate a p value for our test statistic. In our example, I = 4, mean = 15/2 = 7.5, variance = 6(6-1)(12+5)/72 = 7.083, and SD = <img src="https://render.githubusercontent.com/render/math?math=\sqrt{Variance}"> = 2.66. Our standardized variable is therefore -1.32. Using a standard normal distribution Z table, we can calculate that the two-tailed p value is about 0.187.

## Examples
###   Running for 0.3nlog(n) steps

n = 12:<br/>
<img src="12Permutation0.3nlogn.png">

| Test                        | p value      |
| --------------------------- | ------------ |
| Footrule                    | 0.0103574528 |
| Spearman's rank correlation | 0.0861079997 |
| Hamming distance            | 0.0005941848 |
| Kendall's tau               | 0.0548539399 |


n = 50:<br/>
<img src="50Permutation0.3nlogn.png">

| Test                        | p value      |
| --------------------------- | ------------ |
| Footrule                    | 0.0510076617 |
| Spearman's rank correlation | 0.1775827162 |
| Hamming distance            | 0.0005941848 |
| Kendall's tau               | 0.1300158834 |

###   Running for 0.7nlog(n) steps

