---
layout: post
title: "점화식과 재귀함수와 메모이제이션(동적계획법)"
updated: 2021-04-17
tags: [algorithm,design]
---

## 점화식

고등학교 수학 시간때 배웠던 [점화식](https://namu.wiki/w/%EC%A0%90%ED%99%94%EC%8B%9D)이란 것이 있다. 어떤 수열을 표현할 때, "초기값"과 "일반항"의 관계로 표현하는 방식이다. 잘 알려져 있는 피보나치 수열을 점화식으로 표현하면 아래와 같다.

```plaintext
초기값: fib(0), fib(1) = 0, 1
일반항: fib(n) = fib(n-2) + fib(n-1)
```
{:.pseudo}

그리고 점화식은 재귀함수로 쉽게 코딩할 수 있다고 한다.

## 피보나치 수열 구하는 재귀함수

```py
def fib(n):
    fib.counter += 1

    if n <= 1:
        return n
    else:
        return fib(n-2) + fib(n-1)
        
fib.counter = 0                 # 호출횟수 초기화
print(fib(20), fib.counter)     # 6765 21891
```
{:.python}

`fib.counter += 1` 코드는 함수 호출 횟수를 알아보기 위한 코드다.

`fib(20)` 을 실행하면 21891 번이나 함수가 호출된다. fib(20) 을 구하려면 fib(18) 과 fib(19) 를 알아야 하는데, fib(18) 을 재귀호출로 구했다고 하더라도 fib(19) 구할 땐 또 fib(18) 을 재귀호출 방식으로 구한다. 즉 전에 했던 계산을 또 해야하니 함수호출이 많을 수밖에 없다.

## 메모이제이션

메모이제이션 (Memoization) 은 글자그대로 "기억하며 풀기" 다. [나무위키](https://namu.wiki/w/%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)를 보면 자세한 사항을 알 수 있는데, 알고리즘 기법 중 [동적계획법](https://namu.wiki/w/%EB%8F%99%EC%A0%81%20%EA%B3%84%ED%9A%8D%EB%B2%95)을 구현하기 위한 실제 기법에 해당한다고 한다.

```py
def fib(n, cache):
    fib.counter += 1

    if n not in cache:
        if n <= 1:
            cache[n] = n
        else:
            cache[n] = fib(n-2, cache) + fib(n-1, cache)

    return cache[n]

fib.counter = 0                     # 호출횟수 초기화
print(fib(20, {}), fib.counter)     # 6765 39
```
{:.python}

cache 딕셔너리가 추가되었다. 계산의 일부분을 재활용하기 위한 저장소 역할을 한다. cache 안에 이미 계산된 fib(n) 결과가 없는 경우에만 재귀호출을 하는 구조다.

실행결과를 보면, 호출횟수가 상당히 줄어들었음을 알 수 있다.

## 데코레이터를 사용한 메모이제이션

Python 에는 데코레이터 문법이 있다. 기존에 만들어져있는 함수를 감싸서 부가기능을 구현할 수 있도록 도와주는 기능이다. 데코레이터를 잘 모른다면 [코딩도장](https://dojang.io/mod/page/view.php?id=2427) 사이트를 참고해보자.

```py
def memoize(fn):
    cache = {}
    def wrapper(n):
        if n not in cache:
            cache[n] = fn(n)
        return cache[n]
    return wrapper

@memoize
def fib(n):
    fib.counter += 1

    if n <= 1:
        return n
    else:
        return fib(n-2) + fib(n-1)

fib.counter = 0                 # 호출횟수 초기화
print(fib(20), fib.counter)     # 6765 21
```
{:.python}

memoize 라는 함수를 만들어 데코레이터로 사용하였다. 함수 안에서는 cache 에 `fn(n)` 의 결과가 없을 때만 fn 함수를 호출하도록 되어있다.

실행한 결과, 이 역시 데코레이터를 사용하기 전보다 호출횟수가 많이 줄어들었음을 알 수 있다. 

## Python 제공 lru_cache 데코레이터

Python functools 모듈에는 위와 같은 메모이제이션을 편하게 사용할 수 있도록 lru_cache 데코레이터를 기본으로 제공한다. 위에서 만든 memoize 보다 범용적이면서 보다 효율적으로 만들어져있다.

```py
from functools import lru_cache

@lru_cache
def fib(n):
    fib.counter += 1

    if n <= 1:
        return n
    else:
        return fib(n-2) + fib(n-1)

fib.counter = 0                 # 호출횟수 초기화
print(fib(20), fib.counter)     # 6765 21
```
{:.python}

이 역시 함수 호출횟수는 21 번이다.

## 반복문으로 해결

재귀호출 코드는 상당부분 반복문으로 해결할 수 있다고 한다. 피보나치 수열도 재귀호출 말고 반복문으로 구현할 수 있다.

```py
def fib(n):
    fib.counter += 1
    
    cache = {}
    cache[0] = 0
    cache[1] = 1
    for i in range(2, n+1):
        cache[i] = cache[i-2] + cache[i-1]

    return cache[n]

fib.counter = 0                 # 호출횟수 초기화
print(fib(20), fib.counter)     # 6765 1
```
{:.python}

당연히 함수호출 횟수는 1 이다.

## 참고

보통 메모이제이션을 사용한 재귀호출이 Top-Down 방식의 동적계획법이고, 반복문만을 사용한 것이 Bottom-Up 방식의 동적계획법이라고 한다. 동적계획법 개념이 너무 추상적이라 그냥 메모이제이션이라는 방법을 기억해두는 것이 좋을 것 같다.