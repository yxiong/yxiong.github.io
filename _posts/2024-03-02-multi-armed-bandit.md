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
* There are only $m=2$ arms to choose from.

### Algorithm

The key insight in our algorithm is we will start from the last round of the game to compute the expected reward for
each situation, and then work backwards.

#### The last round


## References

* <https://en.wikipedia.org/wiki/Multi-armed_bandit>