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

## Programmatic Solution -- Finite Number of Rounds

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

$$S_k^{(1)}=\frac{x_1}{x_1+y_1}(1+E_k(x_1+1,y_1;x_2,y_2))+\frac{y_1}{x_1+y_1}E_k(x_1,y_1+1;x_2,y_2)$$

Similarly, we can calculate $S_k^{(2)}$ as

$$S_k^{(2)}=\frac{x_2}{x_2+y_2}(1+E_k(x_1,y_1;x_2+1,y_2))+\frac{y_2}{x_2+y_2}E_k(x_1,y_1;x_2,y_2+1)$$

And then we just need to pull the arm with higher reward:

$$E_{k-1}=\max(S_k^{(1)},S_k^{(2)})$$

#### $x_i=y_i=0$: another boundary condition

When the arm is never pulled, $x_i=y_i=0$, the probability above is undefined, and we need some other prior.
For simplicity, we can just assume the probability of each case is 50%.

Another way to look at this is to assume the winning probability $p_i$ itself is a random variable, say with uniform
prior in $[0,1]$. Then after observation $x_i, y_i$, the posterior distribution of $p_i$ will be a Beta distribution
whose mean being $\frac{x_i+1}{x_i+y_i+2}$ (instead of $\frac{x_i}{x_i+y_i}$).

### Complexity

* At last round, we need $O(n^4)$ to construction $E_n(x_1, y_1; x_2, y_2)$ with $1 \leq x_1,x_2,y_1,y_2 \leq n$. (Maybe
  it is just $O(n^3)$ since $x_1+x_2+y_1+y_2=n$, but we are interested in a upper bound here.)
* At $k$-th step, similarly, the complexity is $O(k^4)$ since we need to fill in that many cells.

So the overall complexity is $O((n!)^4)$ for $m=2$ arms.

Generalizing this to $m$ arms, we need to fill in $O(n^{2m})$ cells at the last round, and $O(k^{2m}\cdot m)$ for $k$-th
round (the additional $m$ is because we need to calculate $m$ different expected reward for each cell). So the general
complexity is $O((n!)^{2m}\cdot m^n)$.

## Gittins Index -- Infinite Number of Rounds with Discounted Rewards

### One Arm Bandit

In this setup, we are playing a one arm bandit (risky arm) against a fixed payoff (safe arm):

* When pulling the risky arm, there is a $p$ probability we get a unit reward, and $1-p$ probability we get nothing.
  We model $p$ as a random variable with uniform prior, and its posterior is a Beta distribution.
  In particular, after observing $x$ wins and $y$ losses, the expected win rate becomes $\frac{x+1}{x+y+2}$.
* When pulling the safe arm, we get a constant reward of $\lambda$.

In addition, there is a geometric discount in the rewards of $\beta$ at each round. In other words, the reward we get
at the second round needs to multiply by a discount factor $\beta<1$, and that we get at the third round by $\beta^2$,
etc., regardless of which arm we pull. The $\beta$ value is fixed and known ahead of time, say 0.9 or 0.95.

### Motivation, Definition, and Properties

It can be shown (and rigorously proved, but not here) that

* The optimal solution for one arm bandit is to pull the risky arm a number of times to observe its distribution, and
  then switch to safe arm if we believe the latter is better (here _better_ includes opportunity cost), or never switch
  if such criteria is never reached (risky arm is better).
* Once we switch, we will always stick to the safe arm and never go back to the risky arm.
* Obviously, the larger safe arm's payoff $\lambda$ is, the more likely we will switch to the safe arm, and the earlier.

Assume we observed the risky arm $n$ times and got $x$ wins, $y=n-x$ losses. For simplicity, we call this the initial
state, or round 0. Based on the information $n, x, \lambda$, our choices for this round are:

1. Keep playing the risky arm for at least one round
2. Switch to the safe arm and never look back

Again, the larger the $\lambda$ is, the more likely we will go for option 2. The definition of Gittins Index (roughly)
is the smallest $\lambda$ such that one is indifferent of these two options.

To make this more rigorous, define a value function $V(n, x; \lambda)$ as the expected optimal _total_ payoff of the
game. In other words, if we play the game indefinitely following the optimal strategy, $V(n, x; \lambda)$ is the
expected total payoff. Now, let's look at the two choices:

1. If we keep playing the risky arm 
   * there is a $\frac{x+1}{n+2}$ chance we get a unit payoff plus a $\beta\cdot V(n+1, x+1; \lambda)$ future payoff;
   * and a $1-\frac{x+1}{n+2}$ chance of not winning this round, leaving us only a $\beta\cdot V(n+1, x; \lambda)$
     future payoff;
2. If we play the safe arm, we will get $\lambda + \beta\lambda + \beta^2\lambda + \cdots = \frac{\lambda}{1-\beta}$
   total payoff.

Therefore, by definition, the expected optimal total payoff is

$$\begin{eqnarray}
V(n, x; \lambda)&=& \max\left\{\frac{x+1}{n+2}\left(1 + \beta V(n+1,x+1;\lambda)\right) + \left(1-\frac{x+1}{n+2}\right)\beta V(n+1,x;\lambda)), \frac{\lambda}{1-\beta}\right\} \nonumber \\
&=& \max\left\{p + \beta(pV(n+1,x+1;\lambda) + (1-p)V(n+1,x;\lambda)), \frac{\lambda}{1-\beta}\right\} \nonumber \\
\textrm{where} & & p=\frac{x+1}{n+2} \nonumber
\end{eqnarray}$$

The definition of Gittins Index is

$$\nu = \sup \left\{ \lambda: V(n, x; \lambda) = \frac{\lambda}{1-\beta} \right\}$$

* The $\sup$ means supremum, (roughly) the smallest $\lambda$ in the set 
* The set of $\lambda$ such that $V(n, x; \lambda) = \frac{\lambda}{1-\beta}$ can be read as we choose the safe arm
  since its payoff is higher (no lower) than the risky arm.

Note that Gittins Index tells us the value of pulling the risky arm at _this_round, captures both the potential
immediate payoff and its benefit for future rounds. This means it is a also general solution for the multi-arm bandit
problem: when facing multiple arms, we can calculate the Gittins Index for each arm, and pull the one with the largest
value.

### Calculation

The calculation of Gittins Index is non-trivial. We will present a numerical calculation based on its definition.

#### Approximate the value function $V(n,x;\lambda)$

The main challenge for calculating $V(n,x;\lambda)$ is that it relies on future values $V(n+1,x+1;\lambda)$ and
$V(n+1,x;\lambda)$. To resolve this, we make a key assumption that when $n$ is large enough, we will have observed
enough and get close to know the true value of the risky arm, so the benefit of further exploration become negligible.
In other words, when $n$ is large enough, $V(n,x;\lambda)$ is really just $p$ (potential immediate payoff) plus
$\beta$ times itself. With that insight, we can construct an approximation of the value function as

$$
V_N(n,x;\lambda) = 
\begin{cases}
\frac{\lambda}{1-\beta} & \text{if } n\geq N \textrm{ and } p \leq \lambda \\
\frac{p}{1-\beta} & \text{if } n\geq N \textrm{ and } p > \lambda \\
V(n,x;\lambda) & \text{otherwise, i.e. } n < N
\end{cases}
$$

With this "boundary condition" of $n\geq N$, we can easily calculate $V_N$ backwards with dynamic programing like the
previous section. Here is a simple piece of code based on memorization:

```python
class VCalculator:
  def __init__(self, lambda_, beta, N):
    self.lambda_ = lambda_
    self.beta = beta
    self.N = N
    self.values = dict()  # (n,x) --> V(n,x)

  def calculate(self, n, x):
    if (n,x) in self.values:
      return self.values[(n, x)]
    value = self._calculate(n, x)
    self.values[(n, x)] = value
    return value

  def _calculate(self, n, x):
    p = (x+1) / (n+2)
    if n >= self.N:
      return max(self.lambda_, p) / (1 - self.beta)
    v1 = p + self.beta * (p * self.calculate(n+1,x+1) + (1-p) * self.calculate(n+1,x))
    v2 = self.lambda_ / (1 - self.beta)
    return max(v1, v2)

## TESTING ##
VCalculator(lambda_=0.5, beta=0.9, N=100).calculate(2, 1) # --> 5.556773453807154
```

#### Binary Search for $\nu$

Now that we have a way to approximate $V(n,x;\lambda)$, finding optimal $\lambda$ is a matter of binary search.
Starting from a lower/upper bound interval $[l,u]$ (e.g. $[0,1]$), we iteratively test the middle point $(l+u)/2$
and tighten the interval, until it is small enough (for practical matter). Here is the code

```python
def gittins_index(n, x, beta=0.9, N=100, epsilon=1e-6):
  l, u = 0, 1
  while u - l > epsilon:
    lambda_ = (l+u) / 2
    v = VCalculator(lambda_, beta, N).calculate(n, x)
    if v > lambda_ / (1 - beta): l = lambda_
    else: u = lambda_
  return lambda_

## TESTING ##
gittins_index(0, 0) # --> 0.7028894424438477
gittins_index(2, 1) # --> 0.6346330642700195
```

## References

* <https://en.wikipedia.org/wiki/Multi-armed_bandit>
* <https://en.wikipedia.org/wiki/Gittins_index>
* J. C. Gittins (1979). Bandit Processes and Dynamic Allocation Indices.
  <https://doi.org/10.1111/j.2517-6161.1979.tb01068.x>
* James Edwards (2019). Practical Calculation of Gittins Indices for Multi-armed Bandits.
  <https://arxiv.org/pdf/1909.05075.pdf>