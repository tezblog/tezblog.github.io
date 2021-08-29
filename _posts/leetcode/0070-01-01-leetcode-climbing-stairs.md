---
layout: post
title: "70. Climbing Stairs"
updated: 2021-08-25
tags: [leetcode,design]
---

## 문제

[https://leetcode.com/problems/climbing-stairs/](https://leetcode.com/problems/climbing-stairs/submissions/)

계단 수 n 이 주어졌을 때, 계단을 오를 수 있는 다양한 케이스의 가짓수를 구하는 문제다.

총 계단 수가 n 일 때, 계단을 오르는 총 케이스 f(n) 은 아래와 같은 점화식으로 구할 수 있다.

```plaintext
f(1), f(2) = 1, 2
f(n) = f(n-2) + f(n-1)
```
{:.pseudo}

간단히 생각하면, 계단이 n-2 일 때의 케이스들에서 2 계단을 오르기만하면 n 개를 오르게 되고, 또한 n-1 일 때의 케이스들에서 1 계단을 오르기만 하면 n 개를 오르게 되는 것이기에, 위와 같은 관계가 도출되는 것이다.

점화식은 DP (Dynamic Programming) 로 풀어낼 수 있는 경우가 많다. 그리고 이 점화식은 Fibonacci 수열과 상당히 닮은 형태다. Fibonacci 수열을 소개한 [나무위키](https://namu.wiki/w/%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98%20%EC%88%98%EC%97%B4) 사이트를 참고해보면 이 문제와 관련된 "계단 오르는 가짓수" 도 같이 소개하고 있다.

## Memoization 을 사용한 재귀호출 방식 (Top-Down DP)

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

Python 의 데코레이터를 사용하여 Memoization 을 구현하였다. `f(n)` 값을 찾아야 할 때 우선 cache 에 결과값이 저장되어 있는지를 검사하고, cache 에 없을 때 재귀호출로 계산하여 저장해두는 구조다.

Memoization 을 사용하지 않아도 구할 수 있지만, 지나친 재귀호출로 시간초과가 되어 문제를 통과할 수 없다.

참고로 Python 은 Memoization 을 위한 자체 데코레이터를 제공한다. functools 모듈의 lru_cache 가 그것으로, 아래와 같이 사용해도 된다.

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

## 반복문을 사용 (Bottom-Up DP)

```py
class Solution:
    def climbStairs(self, n: int) -> int:
        
        cache = {}
        
        cache[1] = 1
        cache[2] = 2
        
        for x in range(3, n+1):
            cache[x] = cache[x-2] + cache[x-1]
            
        return cache[n]
```
{:.python}

for 반복문으로 점화식의 일반항을 계속 구해나간다.
