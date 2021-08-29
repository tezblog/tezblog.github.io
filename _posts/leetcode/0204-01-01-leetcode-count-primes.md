---
layout: post
title: "204. Count Primes"
updated: 2021-08-30
tags: [leetcode,math]
---

## 문제

[https://leetcode.com/problems/count-primes/](https://leetcode.com/problems/count-primes/)

주어진 n 미만의 자연수 중, 소수의 개수를 구하는 문제다.

소수를 구하는 가장 유명한 알고리즘은 "에라토스테네스의 체" 이다. 소수의 배수들을 지워내는 것이 마치 체로 소수만을 걸러내는 듯한 모습에서 붙여진 이름인데, 자세한 건 [나무위키](https://namu.wiki/w/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98%20%EC%B2%B4)를 참고해보자.

## 반복문으로 에라토스테네스의 체 구현

```py
class Solution:
    def countPrimes(self, n: int) -> int:
        if n < 2: return 0
        
        primes = [1] * n
        primes[0] = primes[1] = 0
        
        for i in range(2, int(n**0.5)+1):
            if primes[i] == 1:
                primes[i*i:n:i] = [0 for _ in range(i*i, n, i)]
            
        return sum(primes)
```
{:.python}

primes 리스트를 상정하고, 인덱스가 소수라면 1 을, 아니라면 0 을 넣기로 한다. 먼저 모두 1 로 초기화를 한 뒤, 반복문으로 어떤 수 i 가 소수라면 i 의 제곱부터 i 를 더한 수 (즉 i 의 배수) 에 해당하는 인덱스의 값을 0 으로 바꿔버린다.

Python 언어의 리스트 슬라이싱 및 리스트 Comprehension 문법으로 간결하게 표현할 수 있다.

## Generator 로 에라토스테네스의 체 구현

```py
class Solution:
    def countPrimes(self, n: int) -> int:
        iterator = gen_primes()
        
        return sum(1 for x in iterator if x < n or iterator.close())
        
def gen_primes():
    i, sieve = 2, {}
    
    while 1:
        if i in sieve:
            for x in sieve[i]:
                sieve.setdefault(i+x, []).append(x)
            del sieve[i]
        else:
            yield i
            sieve[i*i] = [i]
        i += 1
```
{:.python}

구글링으로 어느 한 [사이트](https://code.activestate.com/recipes/117119-sieve-of-eratosthenes/)에서, 소수를 2 부터 순서대로 yield 하는 제너레이터 코드를 찾을 수 있었다. n 이라는 숫자에 상관없이 지속 생성이 가능한 코드로 이 또한 에라토스테네스의 체 방식으로 소수를 생성한다.

제너레이터를 iterator 변수에 할당한 다음, 리스트 Comprehension 표현식으로 소수의 개수를 구하는데, `if x < n or iterator.close()` 구문으로 yield 된 소수 x 가 n 보다 더이상 작지 않을때는 제너레이터를 break 하도록 했다. 이에 대해서는 [별도 포스팅](/post/break-list-comprehension)을 참고해보자.