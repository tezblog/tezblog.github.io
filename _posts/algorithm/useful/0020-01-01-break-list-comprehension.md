---
layout: post
title: "List Comprehension 표현식에 Break 사용하기"
updated: 2021-04-01
tags: [algorithm,useful]
---

## Comprehension 표현식 개체 순회 도중 Break 사용

결론부터 얘기하자면 아래와 같은 구문은 에러를 발생시킨다.

```py
arr = [1, 3, 5, 7, 9, 8, 6, 4, 2]

print([x for x in arr if x < 8 or break])    # SyntaxError
```
{:.python}

break 는 Statement 이다. Expression 만 허용하는 곳에 사용할 수 없기 때문이다. 이 때는 보통 itertools 모듈의 takewhile 함수를 사용하면 된다.

## Comprehension 으로 리스트 순회 도중 Break 강제

어쨌든 Break 를 강제하고 싶다면 편법으로 구현할 수는 있다.

```py
arr = [1, 3, 5, 7, 9, 8, 6, 4, 2]

print([x for x in arr if x < 8])                   # [1, 3, 5, 7, 6, 4, 2]
print([x for x in arr if x < 8 or arr.clear()])    # [1, 3, 5, 7]
```
{:.python}

arr 을 순서대로 순회하는데, `x < 8` 이 False 가 되면 or 이후의 `arr.clear()` 구문이 실행되도록 꾸몄다. 이는 해당 리스트를 지워버리는 명령이기에 더 이상 순회가 불가능하게 된다.

마치 break 구문을 사용한 것과 비슷한데 부작용이 있다. 원본에 해당하는 `arr` 리스트는 빈 리스트가 되어버린다.

참고로, 딕셔너리나 집합 자료형을 위와 같이 사용하면 에러가 발생한다.

## Comprehension 으로 제너레이터 순회 도중 Break

```py
def gn(i=0):
    while 1:
        yield i
        i += 1
        
g = gn()        
print([x for x in g if x < 5 or g.close()])    # [0, 1, 2, 3, 4]
```
{:.python}

위 제너레이터는 숫자를 0 부터 무한히 yield 한다. 하지만 `x < 5` 가 False 가 되는 시점에서는 `g.close()` 구문이 실행되고, 이는 제너레이터 작동을 중단시킨다. 자세한 내용은 [Python 공식문서](https://docs.python.org/ko/3/reference/expressions.html#generator.close)를 참고해보자.

리스트 순회 Break 에는 clear 함수를, 제너레이터 순회 Break 에는 close 함수를 사용했다는 차이가 있다.

## 참고

리스트 부작용을 막고, 딕셔너리나 집합 자료형도 comprehension break 를 하고 싶다면, 이를 제너레이터로 감싸서 하면 된다.

```py
arr = (x for x in [1, 3, 5, 7, 9, 8, 6, 4, 2])     # comprehension 표현식 자체는 제너레이터

print([x for x in arr if x < 8 or arr.close()])    # [1, 3, 5, 7]
```
{:.python}