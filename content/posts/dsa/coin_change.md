---
title: "The coin change problem"
date: 2020-07-29
draft: false
toc: true
images:
tags:
  - leetcode
  - data structures and algorithms
# markup: "mmark"
---
## The problem
https://leetcode.com/problems/coin-change/

> You are given coins of different denominations and a total amount of money. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

## Explanatory matrix

Given the following input:
```python
coins = 1, 2, 5
amount = 11
```
We can construct an explanatory matrix.

| COIN    | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1       | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  |
| 2, 1    | 0   | 1   | 1   | 2   | 2   | 3   | 3   | 4   | 4   | 5   | 5   | 6   |
| 5, 2, 1 | 0   | 1   | 1   | 2   | 2   | 1   | 2   | 2   | 3   | 3   | 2   | 3   |

The first column corresponds to the coin denominations. Every subsequent column corresponds to total amount. The table fields indicate how many coins of the corresponding denominations are required to reach the total amount. 

Once this matrix exists, returning an answer is as simple as looking up the intersection of the amount (11) and the denominations (1,2,5) which returns 3. 

The trick is knowing how to construct the matrix.

## A bottom-up dynamic solution

The explanatory matrix illustrates the underlying principle, but the ensuing implementation relies on an array instead of a matrix. This is made possible by a reoccuring invocation of the `min` function.

The array is initialized with a length of `amount + 1`, which is 12 given the input. The extra element is required for the 0 column/amount. The remaining elements are initialized to `amount + 1`, i.e. 12.

In the words of Leonid Vulakh:

>For you, this is infinity.

Since no value will ever reach or exceed 12, it will never be returned in the ensuing `min` invocations. 0 and -1 do not work as initialization values, because they will always be the minimum. `None` doesn't work because it cannot be compared to an integer. 

```python
# Guardian against the edge case of amount being 0
if amount == 0:
  return 0

# Construct the array
dp = [amount + 1] * (amount + 1)
dp[0] = 0
```

The resulting array looks like this:
```python
[0, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]
```

Now comes the tricky part. There are two nested loops.

```python
for i in range(amount + 1):
  for coin in coins:
      if coin <= index:
          dp[i] = min(dp[i], dp[i - coin] + 1)
```

The fourth line is deceptively complex. The best way to wrap your head around it is to walk through the iterations. Let's do that together.

| Code                                   | Explanation                                                                                                   |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `for i in range(amount + 1)`           | i = 0                                                                                                         |
| `for coin in coins`                    | coin = 1                                                                                                      |
| `if coin <= i`                         | `1 <= 0` is false so we do not update the value at the 0th index; this holds true for all three denominations |
| Next iteration of outer loop           |                                                                                                               |
| `for i in range(amount + 1)`           | i = 1 (we've moved on to the next iteration of the outer loop)                                                |
| `for coin in coins`                    | coin = 1                                                                                                      |
| `if coin <= i`                         | `1 <= 1` is true                                                                                              |
| `dp[i] = min(dp[i], dp[i - coin] + 1)` |                                                                                                               |
| = `min(dp[1], dp[1-1] + 1)`            |                                                                                                               |
| = `min(12, dp[0] + 1)`                 |                                                                                                               |
| = `min(12, 0 + 1)`                     |                                                                                                               |
| = `min(12, 1)`                         |                                                                                                               |
| = `1`                                  |                                                                                                               |

`min(dp[i], dp[i - coin] + 1)` boils down to `min(12, 1)`. Since `1 < 12` (in other news, the sky is blue), we update the value at the first index to 1. 

The updated array looks like this:
```python
[0, 1, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12]
```

Things get more interesting in the next outer loop.

| Code                                   | Explanation                                     |
| -------------------------------------- | ----------------------------------------------- |
| `for i in range(amount + 1)`           | i = 2                                           |
| `for coin in coins`                    | coin = 1                                        |
| `if coin <= i`                         | `1 <= 2` is true                                |
| `dp[i] = min(dp[i], dp[i - coin] + 1`) |                                                 |
| = `min(dp[2], dp[2-1] + 1)`            |                                                 |
| = `min(12, dp[1] + 1)`                 |                                                 |
| = `min(12, 1 + 1)`                     |                                                 |
| = `min(12, 2)`                         |                                                 |
| = `2`                                  |                                                 |
| Next iteration of inner loop           |                                                 |
| `for coin in coins`                    | coin = 2                                        |
| `if coin <= i`                         | `2 <= 2` is true                                |
| `dp[i] = min(dp[i], dp[i - coin] + 1`) |                                                 |
| = `min(dp[2], dp[2 - 2] + 1)`          |                                                 |
| = `min(2, dp[0] + 1)`                  |                                                 |
| = `min(2, 0 + 1)`                      |                                                 |
| = `min(2, 1)`                          |                                                 |
| = `1`                                  |                                                 |
| Next iteration of inner loop           |                                                 |
| `for coin in coins`                    | coin = 5                                        |
| `if coin <= i`                         | `5 <= 2` is false; the inner loops are complete |

The algorithm correctly determines that we can reach an amount of 2 with 2 (x 1) coins, but when it iterates the inner loop, the condition evaluates as true; consequently, the algorithm determines that we can actually reach the amount of 2 with just 1 (x 2) coin. 

Essentially, for each amount, we explore progressively higher denominations. If you think about it in terms of the explanatory matrix, we move right (with each outer loop), then explore each row in the given column (with each inner loop).

Let's clarify what `dp[i - coin] + 1` does, because in many ways, it's the heart of this solution. 

For a specified amount, we can lookup how many coins it takes to get that amount minus the current coin, then just add one to the total number of coins (for the current coin).

In this specific context, we're trying to reach an amount of 2 and the current coin is 2. `i - coin` equals 0, so we look up 0 in the `dp` array, which returns 0, then we add 1 to the total number of coins (which represents the 2 coin) to arrive at a total of 1. The call to `min` helps us prioritize the more efficient way of reaching an amount of 2: it's better to use 1 x 2 coin than 2 x 1 coins. 

The updated array looks like this:
```python
[0, 1, 1, 12, 12, 12, 12, 12, 12, 12, 12, 12]
```

When we complete the remaining iterations, the completed array looks like this:
```python
[0, 1, 1, 2, 2, 1, 2, 2, 3, 3, 2, 3]
```

Once the array is complete, all we have to do is return `dp[amount]` to look up the minimum amount of coins required to get to the specified amount. 

The full return statement looks like this:

```python
return dp[amount] if dp[amount] <= amount else -1
```

The `if dp[amount] <= amount else -1` is required in case no amount of coins can create the specified amount, which would indicate that the denominations are not canonical.

## Complexity

### Time

`O(amount * n)` where `n` is the number of coins.

We have to iterate through the array, which has a length of `amount + 1` (the 1 can be dropped), and, at each index, we have to iterate through the array of denominations. 

### Space

`0(amount)` since the dp array requires `amount + 1` elements and the 1 is dropped. 

## Code
```python
def coinChange(amount, coins):
    if amount == 0:
        return 0

    dp = [amount + 1] * (amount + 1)
    dp[0] = 0

    for i in range(amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    return dp[amount] if dp[amount] <= amount else -1
```

## To Do
* Walk through greedy method
* Walk through top-down approach
