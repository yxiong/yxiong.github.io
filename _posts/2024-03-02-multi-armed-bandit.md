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
* There are only $m=2$ arms to choose from.

### Notations

* Let $x_i$ denote the number of times $i$-th arm paid off, and $y_i$ the number of times $i$-th arm did not pay off. 
* At $k$-th round (where $1 \leq k \leq n$):
  * We have played $k-1$ rounds before: $x_1 + y_1 + x_2 + y_2 = k-1$, and accumulated a reward of $x_1+x_2$.
  * Given the previous information $x_1, x_2, y_1, y_2$, we will make a decision on which arm to pull at this round,
    denoted as $A_k(x_1, y_1; x_2, y_2)\in\{1,2\}$.
  * After making that decision and observe the outcome (updating $x_1, x_2, y_1, y_2$), we have an expected reward 
    _at the end of the game_ given the information when finishing $k$-th round, denoted as 
    $E_k(x_1, y_1; x_2, y_2)\in\mathbb{R}^{\ge0}$.

### Algorithm

The key insight in our algorithm is we will start from the last round of the game to compute the expected reward for
each situation, and then work backwards.

#### The last round: the "boundary condition"


## References

* <https://en.wikipedia.org/wiki/Multi-armed_bandit>