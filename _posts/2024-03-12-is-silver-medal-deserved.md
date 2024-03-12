---
title: Is Silver Medal Deserved in Single-Elimination Tournament?
date: 2024-03-12
---

* TOC
{:toc}

## Motivation

In [single-elimination tournament](https://en.wikipedia.org/wiki/Single-elimination_tournament), a team is immediately
eliminated when they lose a game. This means if a team is the actual second best, they could be eliminated early in the
game by the very best game if they were unlucky, and thus not reach the final to get the silver medal. Conversely, the
team who did get the silver medal does not deserve it.

So this blog post attempts to answer, _to what extent is the silver medal deserved by merit other than luck?_

## Two Concrete Questions, Different Answers

The question above is actually deceptively ambiguous, and can be reformulated in one of the two concrete forms:

1. If we know a team won silver medal, what is the probability that they are actually the second best in the tournament?
2. If a team is the second best in the tournament, what chance does it have to win the silver medal?

Counter-intuitively, the answers to these two questions are different. In a tournament of $N$ teams (assume $N$ is 2 to
an integer power for simplicity), the answer to the first question is $\frac{1}{\log(N)}$; while the
answer to the second question is $\frac{N}{2(N-1)}$, i.e. close to 50%.

For illustration purpose, we will assume $N=8$ for the rest of this blog post, but the reasoning applies to general $N$
as well. The graph below shows 8 teams, $a, b, c, d, e, f, g, h$. In this tournament, team $a$ wins the gold medal, and
team $e$ wins the silver.

### $P(e\textrm{ is second best} | e\textrm{ wins silver medal})$

Given the outcome of the game as shown above, we know possible 2nd best teams are $b$, $c$ and $e$, because they have
only been defeated by team $a$, the gold medal, but no one else. We will argue that there is equal possibility for any
of these three team to be 2nd best, and therefore $e$'s probability of being the second best is $1/3$.

## Conclusion