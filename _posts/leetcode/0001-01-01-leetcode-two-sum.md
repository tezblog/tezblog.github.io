---
layout: post
title: "1. Two Sum"
updated: "2021-08-23"
tags: [leetcode,list]
---

## 문제

[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)

주어지는 Array 에서, 두 요소를 골랐을 때, 그 합이 target 이 되는 케이스를 리턴하는 문제다.

단순하게 생각한다면, 이중루프를 사용하면 된다. 반드시 합이 target 이 되는 오직 하나의 케이스가 존재한다고 되어 있으므로, Brute Force 방식으로 찾다보면 언젠가는 정답이 얻어걸리게 되어 있다.

하지만 문제 말미에도, 이중루프 즉 시간복잡도가 O(n^2) 이 되는 로직이 아니라, 그보다 더 작은 시간복잡도를 가지는 로직으로 구해보라고 권하고 있다. 즉, 더 효율적인 방법이 존재한다.

## Hash Table 을 활용

```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        
        for i, x in enumerate(nums):
            y = target - x
            
            if y in d: return [d[y], i]
            else: d[x] = i
```
{:.python}

nums 를 x 로 순회하며서, x 의 인덱스 i 를 d 딕셔너리에 저장해둔다. 계속 순회하면서 x 의 보수 (즉 `target - x`) 가 d 에 저장되어있음을 발견한다면, `x + y == target` 이 되는 케이스를 찾은 셈이므로 즉시 리턴을 한다. 합이 target 이 되는 x, y 쌍이 반드시 존재한다고 했으므로, 순회를 하다보면 언젠가는 정답이 걸리게 되어 있다.

여기서 d 가 Hash Table 역할을 하며, 이론적으로 Hash Table 데이터에 접근하는 시간복잡도는 O(1) 이므로, 전체적으로 루프 1 번만 하게 되어, 이 풀이의 시간복잡도는 O(n) 이 된다.