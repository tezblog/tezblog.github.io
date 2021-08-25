---
layout: post
title: "70. Climbing Stairs"
updated: 2021-08-25
tags: [leetcode,easy,math,dynamic_programming,memoization]
---

## 문제

[https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/submissions/)

계단 수 n 이 주어졌을 때, 계단을 오를 수 있는 다양한 케이스의 가짓수를 구하는 문제다.

## Recursive (Fibonacci Number)

```py
class Solution:
    def climbStairs(self, n: int) -> int:
        
        def f(n):
            if n < 3: return n
            
            return f(n-2) + f(n-1)
        
        return f(n)
```
{:.python}

총 계단 수가 n 일 때, 계단을 오르는 총 케이스 f(n) 은 아래와 같은 점화식으로 구할 수 있다.

```plaintext
f(1), f(2) = 1, 2
f(n) = f(n-2) + f(n-1)
```
{:.pseudo}

Fibonacci 수열과 상당히 닮았다. 간단히 생각하면, 계단이 n-2 일 때의 케이스들에서 2 계단을 오르기만하면 n 개를 오르게 되고, 또한 n-1 일 때의 케이스들에서 1 계단을 오르기만 하면 n 개를 오르게 되는 것이기 때문에, 위와 같은 관계가 도출되는 것이다.

시간복잡도는 O(2^n) 이다. 무수히 많은 재귀호출을 구하기에, 시간초과로 문제를 통과할 수 없었다.

## Recursive with Memoization

```py
class Solution:
    def climbStairs(self, n: int) -> int:
        
        def memoize(f):
            cache = {}
            
            def wrapper(n):
                if n not in cache: cache[n] = f(n)
                return cache[n]
                
            return wrapper
        
        @memoize
        def f(n):
            if n < 3: return n
            
            return f(n-2) + f(n-1)
        
        return f(n)
```

Python 의 데코레이터를 사용하여 Memoization 을 구현하였다. `f(n)` 을 구할 때 필요한 `f(n-2)` 와 `f(n-1)` 가 cache 에 없을 때만 재귀호출로 계산하는 구조다. 계산된 결과는 cache 에 담겨, 나중에 다시 필요할 땐 cache 에 있는 값을 그대로 가져가는 구조다.

시간복잡도가 O(n) 으로 대폭 줄어들며, 실행시간은 20 ms 이었다.

참고로 Python 은 Memoization 을 위한 자체 데코레이터를 제공한다. functools 모듈의 lru_cache 가 그것으로, 아래와 같이 사용하면 된다.

```py
class Solution:
    def climbStairs(self, n: int) -> int:
        
        from functools import lru_cache
        
        @lru_cache
        def f(n):
            if n < 3: return n
            
            return f(n-2) + f(n-1)
        
        return f(n)
```
{:.python}