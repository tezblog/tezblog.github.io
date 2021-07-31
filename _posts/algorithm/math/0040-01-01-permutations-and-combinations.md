---
layout: post
title: "순열, 중복순열, 조합, 중복조합 구하는 함수"
updated: 2021-03-20
tags: [algorithm,math]
---

## 순열과 조합

어떤 집합에서 원소들을 뽑아서 나열하는 법에 대해 [순열(Permutation)](https://namu.wiki/w/%EC%88%9C%EC%97%B4)과 [조합(Combination)](https://namu.wiki/w/%EC%A1%B0%ED%95%A9) 이라는 방법이 있다. 만일 한번 뽑았던 원소도 다시뽑기가 가능하도록 한다면, 중복순열, 중복조합이 된다.

## 순열과 조합의 관계

예를들어 원소로 [0, 1, 2] 가 주어지고, 이 중에서 중복을 허용하면서 2 개를 고르라고 하면 (즉 중복순열) 아래와 같이 9 개의 case 들이 나온다.

```py
cases = [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]    # 중복순열
```
{:.python}

여기에서 한법 뽑은 원소를 또 뽑을 수 없다고 하면 (즉 순열), 위 cases 리스트에서 같은 원소가 포함된 `(0, 0), (1, 1), (2, 2)` case 들을 삭제하면 된다.

```py
cases = [(0, 1), (0, 2), (1, 0), (1, 2), (2, 0), (2, 1)]    # 순열
```
{:.python}

한편, 조합은 순열과는 다르게 순서가 중요하지 않다. 위 cases 리스트에서 `(0, 1)` 과 `(1, 0)` 은 동일한 case 인 것이다.

한번 뽑은 원소도 다시 뽑을 수 있게 허용하면서 순서 무관하게 2 개의 짝을 찾는다면 (즉 중복조합), 중복순열의 cases 에서 `(1, 0), (2, 0), (2, 1)` 을 제외하면 된다. 이들은 `(0, 1), (0, 2), (1, 2)` 와 동일한 case 이기 때문이다.

```py
cases = [(0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2)]    # 중복조합
```
{:.python}

여기에서 한번 뽑은 원소를 또 뽑을 수 없다고 하면 (즉 조합) 위 cases 리스트에서 중복요소가 들어간 `(0, 0), (1, 1), (2, 2)` case 들을 제외하면 된다.

```py
cases = [(0, 1), (0, 2), (1, 2)]    # 조합
```
{:.python}

## Python 코드로 구현

위에서 언급한 관계를 바탕으로, 먼저 중복순열을 구한다음, 이 결과에 추가적인 조건을 가하는 식으로 나머지들을 구현할 수 있다. 주로 [Python 공식문서](https://docs.python.org/ko/3/library/itertools.html) 안의 함수 설명부분에 있는 코드를 참고하여 적당히 변형하였다.

위쪽부터 순서대로 중복순열, 순열, 중복조합, 조합을 구하는 함수들이다.

```py
def permus_repeat(pool, r):
    a = [()]
    for _ in range(r):
        a = [x+(y,) for x in a for y in pool]
    return a

def permus(pool, r):
    return [x for x in permus_repeat(pool, r) if len(x) == len(set(x))]

def combis_repeat(pool, r):
    return [x for x in permus_repeat(pool, r) if x == tuple(sorted(x))]

def combis(pool, r):
    return [x for x in combis_repeat(pool, r) if len(x) == len(set(x))]
```
{:.python}

중복제거에는 중복값을 허용하지 않는 set 자료형의 특성을 이용하였으며, 조합으로 변경할 때는 오름차순의 case 만을 허용하는 방식을 사용하였다.

## 제너레이터로 구현

위 코드들은 일단 모든 case 를 처음부터 끝까지 만들어낸 뒤, 리턴하는 방식이다. 그리고 combis 함수를 호출하면, combis_repeat, permus_repeat 까지도 호출이 되는 구조라 때에 따라서는 비효율 적이다.

각 함수를 독립적으로 구현하되, 필요할 때마다 case 를 yield 하는 제너레이터로 만들면 좀 더 효율적일 것이다. 이 역시 [Python 공식문서](https://docs.python.org/3/library/itertools.html) 안의 함수 코드를 기본으로, 적당히 변형하였다.

```py
def permus_repeat(pool, r):
    n = len(pool)
    a = [0]*r
    while True:
        yield tuple(pool[i] for i in a)
        for i in reversed(range(r)):
            a[i] += 1
            if a[i] > n-1: continue
            else:
                a[i+1:] = [0]*(r-1-i)
                break
        else: return
```
{:.python}

중복순열을 yield 하는 제너레이터다. 예를들어 `permus_repeat([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 3)` 과 같이 사용했다면, yield 해야 할 case 는 `(0, 0, 0)` 부터 `(9, 9, 9)` 까지이다.

먼저 yield 대상이 되는 case 를 나타내는 `a` 리스트를 `[0, 0, 0]` 으로 초기화한다. 이후 `a` 리스트의 가장 뒷자리를 계속 늘려간다. `[0, 0, 1], [0, 0, 2], ... ,[0, 0, 9]` 까지는 허용되는 case 이므로 yield 한다.

하지만 `[0, 0, 10]` 이 되는 시점에서는 올바른 case 가 아니게 된다. 이 때는 마지막에서 두번째 자릿수를 늘린다. 그리고 늘린 자릿수 뒷 자릿수는 다시 0 으로 초기화 한다. 즉 `[0, 0, 10]` 이라면, `[0, 1, 10]` 이 되고, 다시 `[0, 1, 0]` 이 된다는 뜻이다.

이제 `[0, 9, 9]` 까지 yield 가 되었다고 해보자. 다음은 `[0, 9, 10]` 이다. 10 이 나오는 경우는 case 위반이므로 마지막에서 두번째 자릿수를 늘려서 `[0, 10, 10]` 이 된다. 이 역시 case 위반이므로 마지막에서 세번째 자릿수를 늘린다. `[1, 10, 10]` 그리고 늘린 자릿수 뒷부분은 다시 0 으로 초기화를 하므로 `[1, 0, 0]` 이 된다.

이제 `[9, 9, 9]` 라면 어떨까? 마지막에서 첫번째, 두번째, 세번째 자릿수를 늘려도 모두 case 위반이다. 이제 더이상 늘릴 자리가 없으므로 이 시점에서 반복문을 종료한다.

이 로직을 구현한 부분이 위 코드에서 while 내부에 있는 for 반복문이다. reversed 함수를 사용하여 가장 뒷 자릿수부터 추적한다.

순열, 중복조합, 조합을 생성하는 제너레이터들도 위 함수와 비슷하게 코딩할 수 있다. 다만 `a` 리스트 초기화 값, case 위반 판별식, 위반이 아닐 경우 뒷 자릿수 처리가 조금씩 다르다.

```py
def permus(pool, r):
    n = len(pool)
    a = [i for i in range(r)]
    while True:
        yield tuple(pool[i] for i in a)
        for i in reversed(range(r)):
            a[i] += 1
            while a[i] in a[:i]:
                a[i] += 1
            if a[i] > n-1: continue
            else:
                for j in range(i+1, r):
                    a[j] = 0
                    while a[j] in a[:j]:
                        a[j] += 1
                break
        else: return
        
def combis_repeat(pool, r):
    n = len(pool)
    a = [0]*r
    while True:
        yield tuple(pool[i] for i in a)
        for i in reversed(range(r)):
            a[i] += 1
            while i and a[i-1] > a[i]:
                a[i] += 1
            if a[i] > n-1: continue
            else:
                a[i+1:] = [a[i]]*(r-1-i)
                break
        else: return
        
def combis(pool, r):
    n = len(pool)
    a = [i for i in range(r)]
    while True:
        yield tuple(pool[i] for i in a)
        for i in reversed(range(r)):
            a[i] += 1
            while i and a[i-1] >= a[i]:
                a[i] += 1
            if a[i] > n-r+i: continue
            else:
                for j in range(i+1, r):
                    a[j] = 0
                    while j and a[j-1] >= a[j]:
                        a[j] += 1
                break
        else: return
```
{:.python}

## Python 제공 함수

사실 Python 에는 중복순열, 순열, 중복조합, 조합을 편리하게 사용할 수 있도록, itertools 모듈에서 제너레이터 함수를 제공하고 있다. 위 Python 공식문서 링크 안에 이미 그 사용법이 있다. 함수 이름은 순서대로 product, permutations, combinations_with_replacement, combinations 이다.