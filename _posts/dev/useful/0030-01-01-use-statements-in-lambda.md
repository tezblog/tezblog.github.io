---
layout: post
title: "Lambda 표현식 안에 Statement 를 사용하기"
updated: 2021-07-30
tags: [dev,useful]
---

## Lambda 표현식

Python 에는 함수 문법을 축약한 lambda 표현식이 있다. 다른 언어의 익명함수와 비슷한 역할을 수행하는데, 아쉽게도 expression 만 허용한다. 즉 lambda 표현식에서는 대입문, break, return, while, global 등을 사용할 수 없다.

## Lambda 에서 Statement 사용하기

아래는 프로그래머스의 [시저암호](https://programmers.co.kr/learn/courses/30/lessons/12926?language=python3)라는 문제의 해답으로 작성해본 코드다.

```py
import re

def solution(s, n):
    fn = lambda m: (m := m.group(0), o := ord('a' if m.islower() else 'A'), chr(o+(ord(m)-o+n)%26))[-1]
    
    return re.sub(r'[a-zA-Z]', fn, s)
```
{:.python}

fn 함수를 보면 lambda 표현식 내부는 `(expression, expression, expression)[-1]` 과 같은 형태로 되어있다. 전체적으로 Tuple 의 제일 마지막 요소를 리턴하는 형태이기에 expression 이고, Tuple 안 요소들도 모두 expression 이다.

참고로 Python 3.8 부터는 := 연산자로 expression 형식의 대입을 사용할 수 있다. 또한, 3 항연산자 식으로 if else 구문을 사용하면 이 역시 expression 이다.

[이 사이트](http://p-nand-q.com/python/lambda.html)를 보면 보다 다양한 트릭들을 소개하고 있다. 다소 과거 버전의 Python 이지만 충분히 참고할만 하다.