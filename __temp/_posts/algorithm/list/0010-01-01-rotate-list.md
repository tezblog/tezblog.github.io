---
layout: post
title: "리스트 데이터요소 로테이션"
updated: 2021-03-27
tags: [algorithm,list]
---

## 리스트 로테이션

순서대로 나열되어 있는 데이터요소를 n 만큼 왼쪽 혹은 오른쪽으로 로테이션을 시킬 때가 있다. 아래 왼쪽 그림은 `rotate(arr, -2)`, 오른쪽 그림은 `rotate(arr, 2)` 와 같은 명령을 수행한 결과이다.

![그림00](/img/algorithm/list/list-0000.svg)

로테이션은 아래와 같은 특징이 있다.

```plaintext
n 로테이션 결과는 배열길이-n 만큼 반대방향 로테이션 결과와 동일
n 이 리스트길이 이상일 때 로테이션 결과는 n%배열길이 로테이션 결과와 동일
```
{:.pseudo}

첫번째 특징 덕분에, 로테이션 알고리즘은 한쪽 방향만 생각해서 구현해도 된다.

두번째 특징도 이해가 어렵지는 않으나, Python 의 나머지를 구하는 % 연산자는 음수일 때 다른 언어와 다른 결과를 내보인다는 것은 미리 알고 있어야 한다. [stackoverflow](https://stackoverflow.com/questions/3883004/the-modulo-operation-on-negative-numbers-in-python) 사이트 중간에 보면 % 연산자는 아래 수식과 같다고 한다.

```py
x%y == x - math.floor(x/y)*y    # Python
x%y == x - math.trunc(x/y)*y    # 다른 언어
```
{:.pseudo}

일반적으로 음수의 % 연산은 위 "다른 언어" 와 같이 동작할 것이라고 생각할 것이다. 즉 `-10%7 == -3` 이 되고, 이렇게 되어야 "리스트 길이가 7 인 리스트의 -10 로테이션 결과는 -3 로테이션 결과와 같다" 가 성립할 것이다.

하지만 Python % 연산은 `-10%7 == 4` 의 결과를 보이는데, 언뜻 길이가 7 인 리스트의 -10 로테이션과 4 로테이션의 결과가 같을 것이라고 이해되지는 않지만, 로테이션 첫번째 특징으로 -3 로테이션과 4 로테이션의 결과가 같기에 최종적으로는 Python % 연산이 다른 언어와 다르게 동작한다 하더라도 최종 결과는 같아지는 것이다.

## 기본 알고리즘

로테이션 결과로 리스트 범위를 벗어나는 부분을 잘라서, 반대쪽에 붙이는 방식이다.

```py
def rotate(arr, n):
    n = n%len(arr)
    return arr[-n:]+arr[:-n]

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

print(arr)                # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
print(rotate(arr, -2))    # ['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B']
print(rotate(arr, 2))     # ['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F']
```
{:.python}

## Reversal 알고리즘

로테이션 결과로 리스트 범위를 벗어나는 부분과 그렇지 않은 부분을 구분한 뒤, 각각의 순서를 뒤집는다. 그리고 다시 전체 리스트의 순서를 뒤집으면 되는 방식이다. [geeksforgeeks](https://www.geeksforgeeks.org/program-for-array-rotation-continued-reversal-algorithm/) 사이트 내용을 참고하였다.

아래 그림을 보면 이해하기 쉽다.

![그림01](/img/algorithm/list/list-0001.svg)

```py
def rotate(arr, n):
    n = n%len(arr)
    return (arr[:-n][::-1]+arr[-n:][::-1])[::-1]

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

print(arr)                # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
print(rotate(arr, -2))    # ['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B']
print(rotate(arr, 2))     # ['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F']
```
{:.python}

## deque 클래스의 rotate 함수

Python 의 collections 모듈에서 양방향 Queue 를 구현한 deque 클래스를 제공하는데, 이 클래스는 rotate 함수를 자체 내장하고 있다.

```py
from collections import deque

arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
dq1 = deque(arr)
dq2 = deque(arr)

print(arr)        # ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']

dq1.rotate(-2)
print(dq1)        # deque(['C', 'D', 'E', 'F', 'G', 'H', 'A', 'B'])

dq2.rotate(2)
print(dq2)        # deque(['G', 'H', 'A', 'B', 'C', 'D', 'E', 'F'])
```
{:.python}

보다 위에서 보인 rotate 함수는 원본 조작없이 복사본을 리턴하지만, deque.rotate 함수는 원본을 직접 조작하는 식으로 설계되어있다. 구체적인 사항은 [Python 공식문서](https://docs.python.org/ko/3.9/library/collections.html#collections.deque)를 참고하자.