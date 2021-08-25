---
layout: post
title: "53. Maximum Subarray"
updated: 2021-08-25
tags: [leetcode,easy,array,divide_and_conquer,dynamic_programming]
---

## 문제

[https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/)

Array 에서 연속된 수의 부분합들 중 가장 최대값을 구하는 문제다.

## Brute Force

```py
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        
        maxsub = float('-inf')
        
        for i, x in enumerate(nums):
            sub = 0
            for j, y in enumerate(nums[i:], i):
                sub += y
                maxsub = max(sub, maxsub)
                
        return maxsub
```
{:.python}

이중루프를 구현하여 모든 부분합 케이스를 탐색한 뒤, 가장 컸던 부분합을 리턴하는 구조다.

실행결과 시간초과로 문제를 통과할 수 없었다.

## Dynamic Programming (Kadane's Algorithm)

```py
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        
        maxsubs = [None] * len(nums)
        
        maxsubs[0] = nums[0]
        for i, x in enumerate(nums[1:], 1):
            maxsubs[i] = max(maxsubs[i-1] + nums[i], nums[i])
            
        return max(maxsubs)
```
{:.python}

주어지는 nums 리스트에서 인덱스 n 까지의 최대 부분합을 나타내는 maxsubs 리스트를 상정했다. maxsubs 는 아래와 같은 점화식을 적용하여 DP 방식으로 구현되며, 이 중 최대값이 결국 최대 부분합이 된다.

```plaintext
maxsubs[0] = nums[0]
maxsubs[n] = max(maxsubs[n-1] + nums[n], nums[n])
```
{:.pseudo}

nums 리스트의 n 번 인덱스까지의 최대 부분합은, n-1 번 인덱스까지의 최대부분합에 n 번 인덱스 값을 더한 것과, n 번 인덱스만의 값 중 더 큰 값에 해당한다는 의미다. 직전까지의 최대값에 현재값을 더했을 때 오히려 더 작아진다면, 끊고 새롭게 부분합을 시작하는 것이 당연할 것이다.

실행시간은 60 ms 이었다.

## Divide and Conquer

```py
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        
        def fn(i, j):
            if i == j: return nums[i], nums[i], nums[i], nums[i]
            
            ll, lm, lr, lt = fn(i, (i+j)//2)
            rl, rm, rr, rt = fn((i+j)//2+1, j)
            
            return max(ll, lt+rl), max(lm, lr+rl, rm), max(lr+rt, rr), lt+rt
        
        return fn(0, len(nums)-1)[1]
```
{:.python}

각 변수가 나타내는 값의 의미는 아래와 같다.

```plaintext
어떤 리스트가 있을 때...
l == 리스트의 가장 왼쪽값으로부터 시작하는, 즉 가장 왼쪽값이 포함되었을 때의 최대 부분합
m == 리스트의 최대 부분합
r == 리스트의 가장 오른쪽값으로 끝나는, 즉 가장 오른쪽값이 포함되었을 때의 최대 부분합
t == 리스트의 전체합

각각을 왼쪽 리스트라면 ll, lm, lr, lt 로, 오른쪽 리스트라면 rl, rm, rr, rt 로 표현
```
{:.pseudo}

왼쪽 리스트와 오른쪽 리스트의 조립으로 생성되는 리스트의 최대 부분합 m 은 `max(lm, lr+rl, rm)` 으로 구할 수 있다. 즉 왼쪽, 오른쪽의 원래 최대 부분합 lm, rm 과 두 리스트에 걸쳐서 생성되는 최대 부분합 `lr+rl` 들 중 최대값이다.

이를 구하려면 결국 리스트의 l 과 r 을 알아야하므로, 재귀호출 과정에서 이 들 값도 계산을 해준다. 조립된 후의 l 은 `max(ll, lt+rl)` 으로, 왼쪽 리스트의 원래 최대 부분합 ll 과 오른쪽 리스트까지 합쳐졌을 때의 왼쪽부터 시작하는 최대 부분합 `lt+rl` 로 구할 수 있다. 비슷하게 생각하면 조립된 후의 r 은 `max(lr+rt, rr)` 로 구할 수 있음을 알 수 있다.

다시 l 과 r 을 구할때는 t 가 필요함을 알 수 있다. 따라서 재귀호출 과정에서도 t 를 `lt+rt` 로 계산해준다.

즉 재귀호출로 리스트 조립을 하면서, l, m, r, t 값을 계속 구해나가고, 최종적으로는 답에 해당하는 m 값만을 리턴하는 구조다.

실행시간은 88 ms 이었다.