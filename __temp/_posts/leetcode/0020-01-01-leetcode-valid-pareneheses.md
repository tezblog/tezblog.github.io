---
layout: post
title: "20. Valid Parentheses"
updated: 2021-08-23
tags: [leetcode,array]
---

## 문제

[https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)

세가지 형태의 괄호가 올바르게 맞물려있는지를 판단하는 문제다.

올바른 형태의 괄호란, 여는괄호와 닫는괄호가 제대로 맞물려 있는 경우를 뜻한다. 구체적으로는 주어지는 문자열 s 안에서 `()`, `{}`, `[]` 문자열을, 빈 문자열이 될 때까지 계속 지워나갈 수 있는 경우이다.

이 방식을 직접 코드로 적용하면 문제를 해결할 수 있다.

## 문자열에서 직접 제거

```py
import re

class Solution:
    def isValid(self, s: str) -> bool:
        while 1:
            p = s
            s = re.sub(r'\(\)|\{\}|\[\]', '', s)
            if len(s) == len(p): break
                
        return s == ''
```
{:.python}

s 안에서 `()`, `{}`, `[]` 인 패턴을 정규식으로 찾아서 계속 삭제해 나간다. 더 이상 삭제가 불가능하다면 (삭제전 문자열인 p 와 삭제후 문자열인 s 의 길이가 같다면) 반복을 종료하며, 종료 후 남게 되는 s 가 빈 문자열인지를 판단하는 구조다.

`re.sub(r'\(\)|\{\}|\[\]', '', s)` 대신 `s.replace('()', '').replace('{}', '').replace('[]', '')` 을 사용해도 된다. (실행속도는 replace 함수 쪽이 훨씬 빨랐다.)

## Stack 을 사용하여 제거

```py
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        d = {')': '(', '}': '{', ']': '['}
        
        for x in s:
            if stack and x in d.keys():
                if stack[-1] == d[x]: stack.pop()
                else: return False
            else:
                stack.append(x)
                
        return stack == []
```
{:.python}

문자열을 x 로 순회하면서, stack 이 비어있거나 x가 닫는괄호가 아니라면 (즉 여는괄호라면) 이를 stack 에 추가한다. 그리고 stack 에 뭔가 채워져 있으면서 닫는괄호가 나온다면, stack 의 제일 마지막 문자와 비교하여, 괄호 짝이 제대로 맞는 경우에는 stack 을 pop 하고, 맞지 않다면 더 이상 검증할 필요없이 즉시 False 를 리턴한다.

최종적으로는 stack 이 빈 리스트인지 여부를 판별한다.