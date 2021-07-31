---
layout: post
title: "소수 (Prime Number) 를 구하는 알고리즘"
updated: 2021-02-16
tags: [algorithm,math]
---

## 소수 구하는 알고리즘

가장 유명한 알고리즘은 [에라토스테네스의 체](https://ko.wikipedia.org/wiki/%EC%97%90%EB%9D%BC%ED%86%A0%EC%8A%A4%ED%85%8C%EB%84%A4%EC%8A%A4%EC%9D%98_%EC%B2%B4)이다. 링크 안에 들어가서 보이는 그림을 보면 어떤 식으로 구하는지 금방 알 수 있다.

2 부터 숫자를 나열하고, 2 의 배수, 3 의 배수, ... 계속해서 지워나가면, 지워지지 않고 남아있는 숫자들이 소수가 된다는 원리다. 마치 체로 걸러서 남아있는 숫자들만 추린다는 의미에서 에라토스테네스의 체 라는 이름이 붙은 것 같다.

## 에라토스테네스의 체 알고리즘

아래는 set 자료형을 사용하여 에라토스테네스의 체 알고리즘을 구현해 본 코드다.

```py
def get_primes(n):
    primes = {x for x in range(2, n+1)}
    for i in range(2, n+1):
        primes -= {j for j in range(i*i, n+1, i)}
    
    return sorted(primes)
```
{:.python}

아래는 set 자료형 대신, 리스트의 인덱스가 소수인지 아닌지 여부를 나타내는 리스트를 사용한 방식이다.

```py
def get_primes(n):
    primes = [False, False] + [True]*(n+1-2)
    for i in range(2, n+1):
        for j in range(i*i, n+1, i):
            primes[j] = False
            
    return [i for i, x in enumerate(primes) if x]
```
{:.python}

`n = 50000` 일 때의 속도를 계산해보았다. set 자료형 사용한 코드는 40.5 밀리초, 리스트 인덱스를 사용한 코드는 24.3 밀리초 였다.

set 자료형 방식은 set 자료형을 만들고, 차집합 연산을 수행하는 과정에 부하가 많이 걸리는 반면, 리스트 인덱스 방식은 리스트는 한번만 생성하며, 인덱스의 값만 바꾸기 때문에 속도가 빠른 듯 하다.

## 다른 소수와의 나눗셈 나머지를 사용하는 알고리즘

소수는 자신이 아닌 다른 소수로는 절대 나누어떨어지지 않는다는 성질을 이용하였다. 이제까지 구한 모든 소수로 나누어떨어지지 않는 수를 새로운 소수로 리스트에 삽입한다.

```py
def get_primes(n):
    primes = []
    for i in range(2, n+1):
        for j in primes:
            if i%j == 0: break
        else:
            primes += [i]
            
    return primes
```
{:.python}

`n = 50000` 일 때의 속도는 692 밀리초 였다. 에라토스테네스의 체 방식보다 퍼포먼스가 매우 떨어지는데, % 연산이 다른 연산에 비해서 상대적으로 매우 느리기 때문이라고 한다.

## 제너레이터로 구현

이제까지의 코드들은 모두 n 이라는 일정한 숫자를 지정해야지만 그 이하의 소수를 구해주는 코드였다.

n 이라는 숫자에 관계없이, 2 부터 소수를 계속 구해주는 방법은 없을까 구글링 해보다가 [한 사이트](http://code.activestate.com/recipes/117119-sieve-of-eratosthenes/)에서 찾을 수 있었다. 이 코드를 살짝 바꿔 나타내보았다.

```py
def get_primes():
    sieve = {}
    n = 2
    while 1:
        if n in sieve:
            for x in sieve[n]:
                sieve.setdefault(n+x, []).append(x)
            del sieve[n]
        else:
            yield n
            sieve[n*n] = [n]
        n += 1
```
{:.python}

눈여겨 볼 점은, n 이라는 숫자를 지정해주지 않아도 알아서 2 부터 무한히 소수를 생성해낸다. 코드를 잘 살펴보면 이 또한 에라토스테네스의 체 알고리즘을 응용했다는 것을 알 수 있다.

`n = 50000` 까지 소수를 생성해봤는데, 속도가 제일 빨랐다. 0.0016 밀리초가 걸렸다.