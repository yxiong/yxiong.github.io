---
title: Multi-Armed Bandit
date: 2024-03-02
---

* TOC
{:toc}

## General Problem Statement

(From Wikipedia)

> The multi-armed bandit problem is a problem in which a decision maker iteratively selects one of multiple fixed
> choices (i.e. arms or actions) when the properties of each choice are only partially known at the time of allocation,
> and may become better understood as time passes.

## Brute Force Programmatic Solution

We first try to solve the problem in a brute force approach to build some intuitions.

### Assumptions

* The number of rounds $n$ is fixed and known ahead of time.
* When pulling each arm, there are two possible outcomes: we get a unit reward (it "pays off") or we get nothing (it
  "doesn't pay off"). All arms follow Bernoulli distributions: there is a preset but unknown probably $p_i$ for $i$-th
  arm to pay off.
* For simplicity, we will start with $m=2$ arms, but most of the notes in this section applies to general $m$.

### Notations

* Let $x_i$ denote the number of times $i$-th arm paid off, and $y_i$ the number of times $i$-th arm did not pay off. 
* At $k$-th round (where $1 \leq k \leq n$):
  * We have played $k-1$ rounds before: $x_1 + y_1 + x_2 + y_2 = k-1$, and accumulated a reward of $x_1+x_2$.
  * Given the previous information $x_1, x_2, y_1, y_2$, we will make a decision on which arm to pull at this round,
    denoted as $A_k(x_1, y_1; x_2, y_2)\in\{1,2\}$.
  * After making that decision and observe the outcome (updating $x_1, x_2, y_1, y_2$), we have an expected reward 
    _at the end of the game_ given the information when finishing $k$-th round, denoted as 
    $E_k(x_1, y_1; x_2, y_2)\in\mathbb{R}_{\ge0}$.

### Algorithm

The key insight in our algorithm is we will start from the last round of the game to compute the expected reward for
each situation, and then work backwards.

#### Last round: the boundary condition

When all $n$ rounds have been played and we observed the outcome, the "expected" reward is simply:

$$E_n(x_1, y_1; x_2, y_2) = x_1 + x_2$$

#### $k$-th round: the iteration

At $k$-th round, when we observed $x_1, x_2, y_1, y_2$, and assume we know $E_k$ (since we work backwards), we need to
calculate the expected reward when we pull each arm.

If we pull the first arm, there is a $\frac{x_1}{x_1+y_1}$ chance it pays off, and a $\frac{y_1}{x_1+x_2}$ chance that
it doesn't. So the expected reward is

$$S_k^{(1)}=\frac{x_1}{x_1+y_1}(1+E_k(x_1+1,y1;x_2,y_2))+\frac{y_1}{x_1+y_1}E_k(x_1,y_1+1;x_2,y_2)$$

Similarly, we can calculate $S_k^{(2)}$ as

$$S_k^{(2)}=\frac{x_2}{x_2+y_2}(1+E_k(x_1,y1;x_2+1,y_2))+\frac{y_2}{x_2+y_2}E_k(x_1,y_1;x_2,y_2+1)$$

And then we just need to pull the arm with higher reward:

$$E_{k-1}=\max(S_k^{(1)},S_k^{(2)})$$

#### $x_i=y_i=0$: another boundary condition

When we the arm is never pulled, $x_i=y_i=0$, the probability above is undefined, and we need some other prior.
For simplicity, we just assume the probability of each case is 50%.

### Complexity

* At last round, we need $O(n^4)$ to construction $E_n(x_1, y_1; x_2, y_2)$ with $1 \leq x_1,x_2,y_1,y_2 \leq n$. (Maybe
  it is just $O(n^3)$ since $x_1+x_2+y_1+y_2=n$, but we are interested in a upper bound here.)
* At $k$-th step, similarly, the complexity is $O(k^4)$ since we need to fill in that many cells.

So the overall complexity is $O((n!)^4)$ for $m=2$ arms.

Generalizing this to $m$ arms, we need to fill in $O(n^{2m})$ cells at the last round, and $O(k^2m\cdot m)$ for $k$-th
round (the additional $m$ is because we need to calculate $m$ different expected reward for each cell). So the general
complexity is $O((n!)^2m\cdot m^n)$.

## References

* <https://en.wikipedia.org/wiki/Multi-armed_bandit>