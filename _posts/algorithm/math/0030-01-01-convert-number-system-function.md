---
layout: post
title: "10진법을 다른 진법으로 변환, 다른 진법을 10진법으로 환원하는 함수"
updated: 2021-03-25
tags: [algorithm,math]
---

## 진법 변환

10 진법을 컴퓨터가 사용하는 2 진법 등으로 변환하는 방법은 [위키피디아](https://ko.wikipedia.org/wiki/%EA%B8%B0%EC%88%98%EB%B2%95#%EC%A7%84%EC%88%98) 링크로 들어가 아랫부분을 찾아보면 나오는데 생각보다 심플하다.

```plaintext
r 진법으로 변환: 10진수를 계속 r 로 나눠서, 나머지들을 이어붙임
10진법으로 환원: r 진수, 제일 뒷자리 숫자부터, (숫자 * r ** 자릿수) 계산하여 합산
```
{:.pseudo}

## 10 진법 -> 다른 진법 변환 함수

[stackoverflow 사이트](https://stackoverflow.com/questions/2267362/how-to-convert-an-integer-to-a-string-in-any-base#answer-53675480)에서 재귀함수를 사용한 어떤 코드를 아래에 옮겨보았다.

```py
def to_base(n, b, d='01234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ'):
    return d[n%b] if n<b else to_base(n//b, b, d)+d[n%b]

print(to_base(255, 2))     # 11111111
print(to_base(255, 5))     # 2010
print(to_base(255, 10))    # 255
print(to_base(255, 16))    # FF
print(to_base(255, 29))    # 8N
print(to_base(255, 32))    # 7V
print(to_base(255, 36))    # 73
```
{:.python}

0 ~ 9 는 물론 알파벳 A ~ Z 까지도 숫자로 취급하기 때문에, 최대 36 진법까지 변환이 가능하다.

## 다른 진법 -> 10 진법 환원 함수

```py
def from_base(n, r, d='0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'):
    return sum(d.index(x)*r**i for i, x in enumerate(n[::-1]))

print(from_base('11111111', 2))    # 255
print(from_base('513', 7))         # 255
print(from_base('255', 10))        # 255
print(from_base('FF', 16))         # 255
print(from_base('8N', 29))         # 255
print(from_base('7V', 32))         # 255
print(from_base('73', 36))         # 255
```
{:.python}

## Python 제공 진법 변환 함수

사실 Python 에는 이런 기능을 하는 함수를 이미 제공하고 있다. bin, oct, hex 내장함수는 각각 2, 8, 16 진법으로 변환해준다. numpy 모듈의 base_repr 함수를 사용하면 위처럼 2 ~ 36 진법으로 자유로운 변환도 가능하다.

그리고 10진법으로 환원하는 함수는 내장함수인 int 를 사용하면 된다. int 의 두번째 인수에 숫자가 어느 진법인지만 알려주면 된다.

```py
import numpy as np

print(np.base_repr(255, 29))    # 8N
print(np.base_repr(15, 2))      # 1111

print(int('8N', 29))            # 255
print(int('1111', 2))           # 15
```
{:.python}

base_repr 함수의 자세한 사항은 [numpy 공식문서](https://numpy.org/doc/stable/reference/generated/numpy.base_repr.html)를, int 함수는 [Python 공식문서](https://docs.python.org/ko/3/library/functions.html#int)를 참고하기 바란다.