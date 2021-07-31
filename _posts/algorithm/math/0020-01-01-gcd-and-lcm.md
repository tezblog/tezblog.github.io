---
layout: post
title: "GCD(최대공약수)와 LCM(최소공배수)를 구하는 함수"
updated: 2021-02-23
tags: [algorithm,math]
---

## gcd 와 lcm

gcd 와 lcm 은 각각 최대공약수와 최소공배수를 뜻하는 영어 약어다. 나무위키의 [최대공약수](https://namu.wiki/w/%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98), [최소공배수](https://namu.wiki/w/%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98) 부분을 참고해보면, 어떤 두 자연수 x, y 가 있다고 할 때 아래와 같은 수식이 성립한다고 한다.

```plaintext
x * y == gcd(x, y) * lcm(x, y)
```
{:.pseudo}

따라서 gcd 든, lcm 이든 어느 한쪽만 구할 수 있다면 다른 나머지도 알 수 있다. 보통 gcd 를 먼저 구하고, 위 수식 관계를 이용하여 lcm 을 구한다.

어떤 두 수 x, y (단, x > y) 의 gcd 를 구하는 가장 유명한 알고리즘은 [유클리드 호제법](https://namu.wiki/w/%EC%9C%A0%ED%81%B4%EB%A6%AC%EB%93%9C%20%ED%98%B8%EC%A0%9C%EB%B2%95)이다. 링크 안에 있는 내용을 간략하게 요약하면 아래와 같다.

```plaintext
gcd(x, y) == gcd(y, x%y)
단 x%y == 0 이면 gcd(x, y) == y
```
{:.pseudo}

## 유클리드 호제법 알고리즘

```py
def gcd(x, y):
    return gcd(y, x%y) if x%y else y
	
def lcm(x, y):
    return x*y // gcd(x, y)
```
{:.python}

호제법 방식으로 재귀호출을 사용하여 gcd 함수를 구현했다.

참고할 점이 있다. 위에서는 x > y 라는 단서를 달았지만, 코드 내부에는 이에 대한 고려가 전혀 없다. 만일 x < y 라면 `x, y = y, x%y` 의 결과는 두 수를 서로 바꿔버린다. 결국에는 x > y 가 되므로 고려할 필요가 없는 것이다.

## 여러 수의 gcd, lcm 구하기

예를들어, 자연수 a, b, c, d 의 gcd (혹은 lcm) 을 구한다고 해보자. 먼저, a 와 b 의 gcd (혹은 lcm) 를 구하고, 이 결과와 c 의 gcd (혹은 lcm) 을 구하고, 다시 이 결과와 d 의 gcd (혹은 lcm) 를 구하면 된다.

## Python 의 gcd, lcm 구하는 함수

math 모듈에서 두 수의 gcd 함수만 지원했었으나, Python 3.9 버전부터는 여러 수의 gcd, lcm 함수를 지원하기 시작했다. [Python 공식문서](https://docs.python.org/ko/3/library/math.html)를 참고하자.

그리고 NumPy 모듈에서도 gcd, lcm 함수를 지원하는데, 이는 [NumPy 공식문서](https://numpy.org/doc/stable/reference/generated/numpy.gcd.html)를 참고해보자.