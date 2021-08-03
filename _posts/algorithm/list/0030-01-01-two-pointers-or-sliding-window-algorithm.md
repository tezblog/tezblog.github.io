---
layout: post
title: "투 포인터/슬라이딩 윈도우 알고리즘"
updated: 2021-04-27
tags: [algorithm,list]
---

## 연속된 자연수 합계가 N 이 되는 문제

예를들어 "연속된 자연수의 합계가 특정한 수 N 이 되는 케이스 개수"를 구해야하는 경우를 생각해보자. 이는 프로그래머스의 [숫자의 표현](https://programmers.co.kr/learn/courses/30/lessons/12924?language=python3) 문제이기도 하다.

아래처럼 풀어보았다.

```py
def solution(n):
    a = []

    for i in range(1, n+1):
        for j in range(i, n+1):
            if sum(range(i, j+1)) == n:
                a += [(i, j+1)]

    return len(a)
```
{:.python}

삼중루프로 되어 있다. i, j, 그리고 sum 안의 range 함수 구문이다. 연속 자연수의 합계가 n 이 되면, `(이상, 미만)` 형태의 튜플로 결과를 저장한다.

위 식을 이중루프로 개선시켜 보았다.

```py
def solution(n):
    a = []
    
    for i in range(1, n+1):
        subtotal = 0
        for j in range(i, n+1):
            subtotal += j
            if subtotal == n:
                a += [(i, j+1)]
                
    return len(a)
```
{:.python}

이를 더 개선시킬 수 없을까?

위와 같이 일정한 규칙으로 나열된 데이터요소를 순회하면서, 순회 도중 그 안에서 다시 순회가 필요할 땐, 위처럼 다중루프 대신 투 포인터 알고리즘을 사용할 수 있는데, 루프 한번만으로 문제를 해결할 수 있게 해준다.

## 투 포인터 알고리즘

N 이 15일 때를 상정해보자. "연속된 자연수 합이 15 가 되는 경우" 를 모두 찾아주면 된다. 

먼저 두개의 포인터를 상정하고 아래와 같이 초기화한다. s 는 부분합의 시작점을, e 는 부분합의 끝점을 나타내는데, Python 의 range 함수 범위 규칙과 같이 ~이상, ~미만 으로 정의했다. 그리고 이 초기상태의 부분합 (연속된 자연수 합) 은 1 이다.

![그림00](/img/algorithm/list/list-0003.svg)

이제 아래와 같은 규칙을 적용하여 두개의 포인터를 조작해가며 부분합이 15 가 되는 케이스들을 찾을 것이다.

```plaintext
s < e 인 동안 반복한다. (즉 s < e 가 성립이 안되는 순간 알고리즘 종료)
부분합 < 15 이면, e 를 증가시킨다. (그만큼 부분합이 증가하게 된다.)
부분합 > 15 이면, s 를 증가시킨다. (그만큼 부분합이 감소하게 된다.)
부분합 == 15 이면, 해당 케이스를 기록해두고는 s 를 증가시킨다. (s 대신 e 를 증가시켜도 문제는 없다.)
```
{:.pseudo}

초기화 상태는 부분합 < 15 이므로 e 를 증가시킨다. 증가시켜도 여전히 부분합 < 15 이므로 계속 증가시킨다. 어느 순간에 아래와 같이 부분합 == 15 가 된다.

![그림01](/img/algorithm/list/list-0004.svg)

부분합 == 15 이므로 해당 케이스를 별도로 기록해두고 s 를 증가시킨다.

![그림02](/img/algorithm/list/list-0005.svg)

이후에도 s, e 를 조정해나가면 아래와 같이 부분합 == 15 가 되는 케이스들을 계속해서 찾을 수 있다.

![그림03](/img/algorithm/list/list-0006.svg)

부분합 == 15 이므로, 다시 s 를 증가시킨다.

![그림03](/img/algorithm/list/list-0007.svg)

이제 s < e 가 성립하지 않게 되었다. 여기서 알고리즘을 종료한다.

## Python 코드로 구현

```py
def solution(n):
    a = []
    
    s, e, subtotal = 1, 2, 1
    while s < e:
        if subtotal < n:
            e, subtotal = e+1, subtotal+e
        else:
            if subtotal == n: a += [(s, e)]
            s, subtotal = s+1, subtotal-s
            
    return len(a)
```
{:.python}

N 이 1000 일 때 기준으로 속도를 측정해보니, 248 마이크로초가 걸렸다. 삼중루프 2160000 마이크로초, 이중루프 35800 마이크로초 보다도 월등히 우수하다.