---
layout: post
title: "1. Two Sum"
updated: "2021-08-23"
tags: [leetcode,hash]
---

## 문제

[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

주어지는 Array 에서, 두 요소를 골랐을 때, 그 합이 target 이 되는 케이스를 리턴하는 문제다. 반드시 하나의 정답만이 존재하도록 문제가 꾸며져 있다.

## Brute Force

```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        for i, x in enumerate(nums[:-1]):
            for j, y in enumerate(nums[i+1:], i+1):
                if x+y == target:
                    return [i, j]
```
{:.python}

모든 케이스를 순회하면서, 두 요소의 합이 target 이 될 때 리턴한다.

시간복잡도는 O(n^2) 이며, 실행속도는 3808 ms 가 나왔다.

## Hash Table

```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        
        hashtable = {}
        
        for i, x in enumerate(nums):
            y = target - x
            if y in hashtable:
                return [hashtable[y], i]
            else:
                hashtable[x] = i
```
{:.python}

nums 를 한번만 순회하면서, `y = target - x` 를 계산하여, hashtable 에 없다면 저장해 둔다. 순회를 하다가 y 가 hashtable 에 있다면 저장된 내용과 함께 리턴한다. 반드시 `x + y == target` 이 되는 케이스가 한개 존재하기 때문에 가능한 풀이다.

시간복잡도는 O(n) 이며, 실행속도는 56 ms 가 나왔다.