---
tags: ["hash table", "leetcode", "stack", "tree"]
---

# 94. Binary Tree Inorder Traversal

## Links

- [Description](https://leetcode.com/problems/binary-tree-inorder-traversal/)
- [Solution](https://leetcode.com/problems/binary-tree-inorder-traversal/solution/)
- [Discuss](https://leetcode.com/problems/binary-tree-inorder-traversal/discuss/)

## Clarification

Given

- A **binary tree**.
  - May be empty.

Goal

- Return the `inorder` traversal of its nodes' values.

## Follow up

Recursive solution is trivial, could you do it iteratively?

## Test cases

```yml
# Empty
[]
# Only root
[1]
# Only left
[1, 2, null, 4]
# Only right
[1, null, 3, null, 7]
# Partial
[1, 2, 3, null, 5, 6]
# Full
[1, 2, 3, 4, 5, 6, 7]
```

## Possible solutions

### Recursion

- Time: O(N)
- Space: O(N)

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        self.helper(root, result)
        return result

    def helper(self, node: TreeNode, result: List[int]) -> None:
        if node:
            self.helper(node.left, result)
            result.append(node.val)
            self.helper(node.right, result)
```

### Iteration

- Time: O(N)
- Space: O(N)

#### Style 1

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = []
        while root or stack:
            if root:
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                result.append(root.val)
                root = root.right
        return result
```

#### Style 2

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        stack = []
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            result.append(root.val)
            root = root.right
        return result
```

### Morris Traversal

- Time: O(N)
- Space: O(N)
- Resource
  - Threaded binary tree ([Wiki](https://en.wikipedia.org/wiki/Threaded_binary_tree))

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        while root:
            if not root.left:
                result.append(root.val)
                root = root.right
            else:
                right_most = root.left
                while right_most.right:
                    right_most = right_most.right
                right_most.right = root
                root.left, root = None, root.left
        return result
```
