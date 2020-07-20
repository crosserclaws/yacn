---
tags: ["array", "binary search", "leetcode", "sliding window", "two pointers"]
---

# 209. Minimum Size Subarray Sum

## Links

- [Description](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [Solution](https://leetcode.com/problems/minimum-size-subarray-sum/solution/)
- [Discuss](https://leetcode.com/problems/minimum-size-subarray-sum/discuss/)

## Clarification

Given

- An `array` of n **positive integers**
  - Array may be empty.

Goal

- Find the `minimal` length of a `contiguous subarray` of which the sum ≥ s.
  - If there isn't one, return 0 instead.

## Reflection

The term, `minimal length of a contiguous subarray`, implies it may be related to the sliding window/two pointers.

## Test cases

```python
# An empty array
4
[]
# Never
100
[2, 3, 4, 5]
# Satisfied with the 1st element.
2
[2, 3, 4]
# Satisfied with the first n elements.
11
[5, 4, 3, 2, 1]
# Satisfied with the last element.
4
[2, 3, 8]
# Satisfied with the last n elements.
11
[1, 2, 3, 4, 5]
# Satisfied with some elements.
6
[2, 3, 4, 5]
# All
15
[1, 2, 3, 4, 5]
```

## Possible solutions

### Brute force

- Time: O(N^2)
- Space: O(1)
- Note: TLE(Time Limit Exceeded)

```python
def minSubArrayLen(self, s: int, nums: List[int]) -> int:
    result = len(nums)+1
    for i, n in enumerate(nums):
        sum_ = 0
        for j in range(i, len(nums)):
            sum_ += nums[j]
            if sum_ >= s:
                result = min(result, j-i+1)
                break
    return result % (len(nums)+1)
```

### Binary search

- Time: O(N log N)
- Space: O(N)

Style 1

```python
def minSubArrayLen(self, s: int, nums: List[int]) -> int:
    boundary = len(nums)+1
    acc = [0] * boundary
    for i, n in enumerate(nums):
        acc[i+1] = acc[i] + n

    result = boundary
    for i in range(len(nums)):
        l, r = i, len(nums)-1
        while l <= r:
            mid = (l+r) >> 1
            if acc[mid+1]-acc[i] >= s:
                r = mid-1
            else:
                l = mid+1
        if l < len(nums):
            result = min(result, l-i+1)
    return result % boundary
```

Style 2

```python
def minSubArrayLen(self, s: int, nums: List[int]) -> int:

    def binary_search(acc: List[int], start: int, target: int) -> int:
        result = boundary
        l, r = start, len(nums)-1
        while l <= r:
            mid = (l+r) >> 1
            if acc[mid+1] - acc[start] >= target:
                result = mid-start+1
                r = mid-1
            else:
                l = mid+1
        return result

    boundary = len(nums)+1
    acc = [0] * boundary
    for i, n in enumerate(nums):
        acc[i+1] = acc[i] + n

    result = boundary
    for i in range(len(nums)):
        ret = binary_search(acc, i, s)
        if ret < result:
            result = ret
    return result % boundary
```

### Sliding window / Two pointers

- Time: O(N)
- Space: O(1)
- Credit: lee215 ([Link](https://leetcode.com/problems/minimum-size-subarray-sum/discuss/433123/))

```python
def minSubArrayLen(self, s: int, nums: List[int]) -> int:
    l = 0
    result = len(nums)+1
    for r in range(len(nums)):
        s -= nums[r]
        while s <= 0:
            result = min(result, r-l+1)
            s += nums[l]
            l += 1
    return result % (len(nums)+1)
```

## Follow up

If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).
