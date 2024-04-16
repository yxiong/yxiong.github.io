---
title: 24 Points
date: 2024-02-25
tags:
- algorithm
- code
---

## Problem Statement

Given 4 single digit numbers, e.g. `1`, `3`, `6`, `9`, find an arithmetic expression using `+`, `-`, `*` or `/` to make
the number 24. An example solution of above can be `(6-3)*(9-1)`.

## Helper Functions

First let's create two helper functions:

1. Get all possible `n`-choose-2 combinations, returning the `chosen, remaining` tuple.

    ```python
    def n_choose_2(xs):
      for i in range(len(xs)):
        for j in range(i + 1, len(xs)):
          yield (xs[i], xs[j]), xs[:i] + xs[i+1:j] + xs[(j+1):]

    ## TESTING ##
    for x in n_choose_2(['a', 'b', 'c']):
      print(x)
    ========
    (('a', 'b'), ['c'])
    (('a', 'c'), ['b'])
    (('b', 'c'), ['a'])
    ```

2. Get all possible binary arithmetic operations of two numbers.

    ```python
    def get_all_binary_op_vals(x, y):
      yield (x+y, f"{x}+{y}")
      yield (x*y, f"{x}*{y}")
      if x > y:  yield (x-y, f"{x}-{y}")
      else:      yield (y-x, f"{y}-{x}")
      if y != 0: yield (x/y, f"{x}/{y}")
      if x != 0: yield (y/x, f"{y}/{x}")

    ## TESTING ##
    for x in get_all_binary_op_vals(3, 5):
      print(x)
    ========
    (8, '3+5')
    (15, '3*5')
    (2, '5-3')
    (0.6, '3/5')
    (1.6666666666666667, '5/3')
    ```

## Solution

To solve this problem, we rely on the observation that one must first choose two numbers from the input and do a binary
operation on them. After that, the result can be merged into the remaining inputs, and then the problem can be solved
recursively.

```python
def compute_24_points(xs):
  for two, rest in n_choose_2(xs):
    for val, ops in get_all_binary_op_vals(two[0], two[1]):
      if len(rest) > 0:
        for result in compute_24_points([val] + rest):
          yield result.replace(f"{val}", f"({ops})", 1)
      else:  # rest == []
        if val == 24: yield ops

## TESTING ##
for x in compute_24_points([1,3,6,9]):
  print(x)
========
((3-1)*9)+6
((6-1)*3)+9
((1+9)*3)-6
(6-3)*(9-1)
(9-1)*(6-3)
((9/3)+1)*6
```

## Complexity

Assuming there are $n$ input numbers ($n=4$ in the original problem statement), and $m$ possible binary operations (
$m=5$ in our case). Denote the complexity of the algorithm is $T(n;m)$, then the recursion is

$$T(n;m) = \binom{n}{2}\cdot m \cdot T(n-1;m) = \frac{n(n-1)}{2}\cdot m \cdot T(n-1;m)$$

From this, we can derive that

$$T(n;m) = \Theta\left((n!)^2 \left(\frac{m}{2}\right)^n\right)$$

## Notes

* There is one piece in the solution that I particularly do not like: the string replacement
  line `result.replace(f"{val}", f"({ops})", 1)`. I believe it will work out since the replacement is only done once,
  but welcome suggestions or feedback for more elegant treatments.
* We assume there are $m=5$ possible binary operations because we only take one of `x-y` and `y-x` but both `x/y`
  and `y/x`.
