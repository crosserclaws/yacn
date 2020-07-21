---
tags: ["hash table", "leetcode", "sliding window", "string", "two pointers"]
---

# 76. Minimum Window Substring

## Links

- [Description](https://leetcode.com/problems/minimum-window-substring/)
- [Solution](https://leetcode.com/problems/minimum-window-substring/solution/)
- [Discuss](https://leetcode.com/problems/minimum-window-substring/discuss/)

## Clarification

Given

- A **string** S and a **string** T.
  - T may contain repeat characters.

Goal

- Find the `minimum` window in S which will contain `all` the characters in T in complexity `O(N)`.

Note

- If there is _no such window_ in S that covers all characters in T, return the _empty_ string "".
- If there is _such window_, you are guaranteed that there will always be _only one unique_ minimum window in S.

## Reflection

The term, `minimal window`, implies it may be related to the sliding window/two pointers.

- We must determine if the window of a string contains all characters in constant time.

## Test cases

```python
# Empty
""
"abc"
# No match
"ABC"
"abc"
# One
"a"
"a"
# Match the 1st element.
"ABC"
"A"
# Match the first n elements
"abac"
"ab"
# Match some elements.
"ap-ppa-cp"
"app"
# Match some elements.
"xa-xxx-ox"
"xxx"
# Match some elements.
"ao-moon-cd"
"mn"
# Match the last element.
"ABC"
"C"
# Match the last n elements.
"abcdac"
"ac"
# Match all
"ABC"
"ABC"
```

## Possible solutions

### Sliding window / Two pointers

- Time: O(|S| + |T|)
- Space: O(|S| + |T|)

```python
from collections import Counter
def minWindow(self, s: str, t: str) -> str:
    count = collections.Counter(t)
    missing = len(count)

    l = 0
    start, end = 0, len(s)+1
    for r, c in enumerate(s, 1):
        count[c] -= 1
        if count[c] == 0:
            missing -= 1
        while l < r and missing == 0:
            if r-l < end-start:
                start, end = l, r
            ch = s[l]
            count[ch] += 1
            if count[ch] > 0:
                missing += 1
            l += 1
    return '' if end > len(s) else s[start:end]
```

### Optimized sliding window / two pointers

- Time: O(|S| + |T|)
- Space: O(|S| + |T|)

```python
from collections import Counter
def minWindow(self, s: str, t: str) -> str:
    count = collections.Counter(t)
    missing = len(count)

    l = 0
    fs = [(i, c) for i, c in enumerate(s) if c in t]
    start, end = 0, len(s)+1
    for r, (idx_r, c) in enumerate(fs, 1):
        count[c] -= 1
        if count[c] == 0:
            missing -= 1
        while l < r and missing == 0:
            idx_l = fs[l][0]
            if idx_r-idx_l+1 < end-start:
                start, end = idx_l, idx_r+1
            ch = fs[l][1]
            count[ch] += 1
            if count[ch] > 0:
                missing += 1
            l += 1
    return '' if end > len(s) else s[start:end]
```
