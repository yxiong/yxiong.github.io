---
title: Secretary Problem
date: 2024-01-24
tags:
- algorithm
---

* TOC
{:toc}

## Formulation and Notation

* Objective: find the very best applicant from the pool.
* $n$: number of applicants, known as a prior.
* $r$: the starting point of "leap". The strategy is to look at the first $(r-1)$ applicants without committing, and starting from $r$-th, offer the position if he/she is the best so far.
  * $r$ is the unknown variable we want to look for, and it is a function of $n$.

## Finding the Optimal $r$

$$\begin{eqnarray} 
P(r)&=& \mathbb{P}(\textrm{find the best candidate when starting to leap at $r$}) \nonumber \\
&=& \sum_{i=1}^n \mathbb{P}(\textrm{stop at $i$ } \cap \textrm{ $i$ is the best candidate})\nonumber \\
&=& \sum_{i=1}^n \mathbb{P}(\textrm{$i$ is the best candidate}) \cdot \mathbb{P} (\textrm{stop at $i$ } | \textrm{ $i$ is the best candidate})\nonumber \\
&=& \sum_{i=1}^n \frac{1}{n} \cdot \mathbb{P}(\textrm{stop at $i$ } | \textrm{ $i$ is the best candidate})\nonumber \\
&=& \frac{1}{n} \left( \sum_{i=1}^{r-1} \mathbb{P}(\textrm{stop at $i$ } | \textrm{ $i$ is the best candidate}) + \sum_{i=r}^{n} \mathbb{P}(\textrm{stop at $i$ } | \textrm{ $i$ is the best candidate}) \right) \nonumber \\
&=& \frac{1}{n} \left( \sum_{i=1}^{r-1} 0 + \sum_{i=r}^{n} \mathbb{P}(\textrm{did not stop at $r$, $r+1$, ..., $i-1$ }) \right) \nonumber \\
&=& \frac{1}{n} \sum_{i=r}^{n} \mathbb{P}(\textrm{best of \{1,2,...,$i-1$\} is in \{1,2,...,$r-1$\} }) \nonumber \\
&=& \frac{1}{n} \sum_{i=r}^{n} \frac{r-1}{i-1} \nonumber \\
&=& \frac{r-1}{n} \sum_{i=r}^{n} \frac{1}{i-1} \nonumber
\end{eqnarray}$$

The optimal $r$ needs to maximize the probability above: $\hat{r} = \max P(r)$.

### Solving programmatically

Given an $n$, $\sum_{i=r}^n\frac{1}{i-1}$ can be computed with dynamic programing in $O(n)$ time for every $r\in[1,n]$, and hence so do $P(r)$ and $\max P(r)$:

```python
def secretary_problem(n):
  # Compute \sum_{i=r}^n (1 / (i-1)) with dynamic programing
  s = [0] * (n + 1)
  s[n] = 1 / (n - 1)  # Starting point.
  for r in range(n-1, 1, -1):
    s[r] = 1 / (r - 1) + s[r+1]

  # Compute P(r) = (r-1) / n * s[r]
  P = [0] * (n + 1)
  P[1] = 1 / n  # special case
  for r in range(2, n+1):
    P[r] = (r - 1) / n * s[r]

  return max(enumerate(P), key=lambda x: x[1])

r_hat, pr = secretary_problem(6)  # ==> 3, 0.42778
```

### Solving analytically as $n \to \infty$

Let $t=\frac{i-1}{n}$ and $dt=\frac{1}{n}$, we get $\frac{1}{i-1}=\frac{dt}{t}$, and therefore

$$P(r)= \frac{r-1}{n} \sum_{i=r}^{n} \frac{1}{i-1}\  \xrightarrow[n \to \infty]{}\ \frac{r-1}{n} \int_{t=\frac{r-1}{n}}^1 \frac{dt}{t}$$

Now let $x=\frac{r-1}{n}$, we have

$$\begin{eqnarray} 
P(x)&=& x\int_x^1 \frac{dt}{t}\nonumber \\
&=& x\cdot\ln(t)\Big|_x^1\nonumber \\
&=& x(\ln(1) - \ln(x))\nonumber \\
&=& -x\ln(x)
\end{eqnarray}$$

Taking the derivative and setting it to 0, we get $\hat{x}=1/e\approx 37\%$ and $P(\hat{x})=1/e\approx 37\%$.

## Things for Further Investigation

* How to prove that the look-then-leap strategy is optimal of all possible strategies?
* If we follow the optimal strategy, what can we say about the applicant we found even if he/she is not the best? Is there any guarantee that we will always find a "good" applicant by certain measure?

## References

* <https://en.wikipedia.org/wiki/Secretary_problem>
* [Odds Algorithm] Bruss, F. Thomas (2000). "Sum the odds to one and stop" - <https://doi.org/10.1214%2Faop%2F1019160340>
