---
tags: ["dynamic programming", "leetcode", "tree"]
---

# 95. Unique Binary Search Trees II

## Links

- [Description](https://leetcode.com/problems/unique-binary-search-trees-ii/)
- [Solution](https://leetcode.com/problems/unique-binary-search-trees-ii/solution/)
- [Discuss](https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/)

## Clarification

Given

- An integer n.

Goal

- Generate `all structurally unique` BST(binary search trees)'s that store values 1 ... n.

Constraints

- 0 <= n <= 8

## Reflection

- A unique BST consists of a root, a left subtree, and a right subtree.
  - The left subtree contains values that are less than their root value.
  - The right subtree contains values that are greater than their root value.
- A set of values of a tree or a subtree can form up many BST's.
  - Different trees may contain the same subtrees, so there are redundant computations.

## Test cases

```yml
0
1
2
3
4
```

## Possible solutions

### Recursion

```python
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if n == 0:
            return []
        return self.build_bst(1, n)

    def build_bst(self, start: int, end: int) -> List[TreeNode]:
        if start > end:
            return [None]

        return [
            TreeNode(val, lft, rht)
            for val in range(start, end+1)
            for lft in self.build_bst(start, val-1)
            for rht in self.build_bst(val+1, end)
        ]
```

### Dynamic programming: memoization

```python
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if n == 0:
            return []
        self.memo = {}
        return self.build_bst(1, n)

    def build_bst(self, start: int, end: int) -> List[TreeNode]:
        if end < start:
            return [None]

        key = (start, end)
        if key not in self.memo:
            tree = [
                TreeNode(i, lft, rht)
                for i in range(start, end+1)
                for lft in self.build_bst(start, i-1)
                for rht in self.build_bst(i+1, end)
            ]
            self.memo[key] = tree
        return self.memo[key]
```

### Dynamic programming

- Idea: reuse subtrees.
- Credit: Java Solution with DP ([Link](https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31493/))

```python
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if n == 0:
            return []

        dp = [[] for _ in range(n+1)]
        dp[0].append(None)
        for size in range(1, n+1):
            for i in range(1, size+1):
                for lft in dp[i-1]:
                    for rht in dp[size-i]:
                        root = TreeNode(i, lft, self.clone_and_shift(rht, i))
                        dp[size].append(root)
        return dp[n]

    def clone_and_shift(self, root: TreeNode, shift: int) -> TreeNode:
        if not root:
            return None

        lft = self.clone_and_shift(root.left, shift)
        rht = self.clone_and_shift(root.right, shift)
        return TreeNode(root.val+shift, lft, rht)
```

### Dynamic programming: tabulation

- Idea: insert a new node into the left/right subtrees because its value is always less/greater than others.
- Credit
  - Share a C++ DP solution with O(1) space ([Link](https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31516/))
  - JAVA DP Solution and Brute Force Recursive Solution ([Link](https://leetcode.com/problems/unique-binary-search-trees-ii/discuss/31552/))

```python
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if n == 0:
            return []

        result = [TreeNode(1)]
        for i in range(2, n+1):
            tmp = []
            for tree in result:
                # As a new root
                node = TreeNode(i, tree)
                tmp.append(node)

                # As descendants of right subtrees
                sub_root = tree
                node = TreeNode(i)
                while sub_root.right:
                    sub_rht = sub_root.right
                    node.left = sub_rht
                    sub_root.right = node
                    tmp.append(self.clone(tree))
                    sub_root.right = sub_rht
                    sub_root = sub_root.right

                # As a leaf
                node.left = None
                sub_root.right = node
                tmp.append(self.clone(tree))
                sub_root.right = None
            result = tmp
        return result

    def clone(self, root: TreeNode) -> TreeNode:
        if not root:
            return None

        lft = self.clone(root.left)
        rht = self.clone(root.right)
        return TreeNode(root.val, lft, rht)
```
