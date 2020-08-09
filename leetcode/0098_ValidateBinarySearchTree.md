---
tags: ["depth first search", "dfs", "leetcode", "tree"]
---

# 98. Validate Binary Search Tree

## Links

- [Description](https://leetcode.com/problems/validate-binary-search-tree/)
- [Solution](https://leetcode.com/problems/validate-binary-search-tree/solution/)
- [Discuss](https://leetcode.com/problems/validate-binary-search-tree/discuss/)

## Clarification

Given

- A binary search tree.

```text
Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
```

Goal

- Determine if it is a valid binary search tree (BST).

## Test cases

```yml
# Empty tree
[]
# Single node
[1]
# A BST
[2,1,3]
# Invalid same values
[1,1]
# An invalid left child
[3, 5, 6]
# An invalid right child
[5,1,4,null,null,3,6]
```

## Possible solutions

### Recursion

- Time: O(N)
- Space: O(N)

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        return self.is_bst(root, float('-inf'), float('inf'))

    def is_bst(self, node: TreeNode, lower_bound: float, upper_bound: float) -> bool:
        if not node:
            return True
        return lower_bound < node.val < upper_bound \
            and self.is_bst(node.left, lower_bound, node.val) \
            and self.is_bst(node.right, node.val, upper_bound)
```

### Iteration

- Time: O(N)
- Space: O(N)

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        stack = [(root, float('-inf'), float('inf'))] if root else []
        while stack:
            node, lower_bound, upper_bound = stack.pop()
            if lower_bound < node.val < upper_bound:
                if node.left:
                    stack.append((node.left, lower_bound, node.val))
                if node.right:
                    stack.append((node.right, node.val, upper_bound))
            else:
                return False
        return True
```

### Inorder traversal

- Time: O(N)
- Space: O(N)

#### Style 1

```python
class Solution  :
    def isValidBST(self, root: TreeNode) -> bool:
        stack = []
        lower_bound = float('-inf')
        while root or stack:
            if root:
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                if root.val <= lower_bound:
                    return False
                lower_bound = root.val
                root = root.right
        return True
```

#### Style 2

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        stack = []
        lower_bound = float('-inf')
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            if root.val <= lower_bound:
                return False
            lower_bound = root.val
            root = root.right
        return True
```
