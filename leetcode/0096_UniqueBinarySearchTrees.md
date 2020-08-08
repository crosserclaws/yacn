---
tags: ["dynamic programming", "leetcode", "tree"]
---

# 96. Unique Binary Search Trees

## Links

- [Description](https://leetcode.com/problems/unique-binary-search-trees/)
- [Solution](https://leetcode.com/problems/unique-binary-search-trees/solution/)
- [Discuss](https://leetcode.com/problems/unique-binary-search-trees/discuss/)

## Clarification

Given

- An integer n.

Goal

- Calculate how many `structurally unique` BST's (binary search trees) that store values 1 ... n.

Constraints

- 1 <= n <= 19

## Reflection

## Test cases

```yml
1
2
3
4
```

## Possible solutions

### Dynamic programming: tabulation

- Time: O(N^2)
- Space: O(N)
- Idea
  - The total number of structurally unique BST's with n nodes is the sum of trees with every node as root.
  - For a tree with a fixed root, its combination is the combination of the left subtree multiplies the combination of the right subtree.
    - For example, a tree with n node and a root of value i.
  - Considering the property of a BST, values less than root are in the left subtree while the greater ones are in the right subtree.
    - The combination of the left subtree is the total number of structurally unique BST's with i-1 nodes.
    - The combination of the right subtree is the total number of structurally unique BST's with n-i nodes.
- Credit
  - DP Solution in 6 lines with explanation. F(i, n) = G(i-1) \* G(n-i) ([Link](https://leetcode.com/problems/unique-binary-search-trees/discuss/31666/))

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[0] = dp[1] = 1
        for i in range(2, n+1):
            for j in range(i):
                dp[i] += dp[j] * dp[i-j-1]
        return dp[n]
```

### Math: catalan number

- Time: O(N)
  - Evaluate all factorials.
- Space: O(1)
- Resource
  - Catalan number ([Wiki](https://en.wikipedia.org/wiki/Catalan_number))
- Credit
  - [Python] Math oneliner O(n), using Catalan number, explained ([Link](https://leetcode.com/problems/unique-binary-search-trees/discuss/703049/))

```python
from math import factorial
class Solution:
    def numTrees(self, n: int) -> int:
        return factorial(n*2)//(factorial(n) * factorial(n+1))
```
