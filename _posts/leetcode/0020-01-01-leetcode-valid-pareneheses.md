---
layout: post
title: "20. Valid Parentheses"
updated: 2021-08-23
tags: [leetcode,easy,string,stack]
---

## 문제

[https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)

세가지 형태의 괄호가 올바르게 맞물려있는지를 판단하는 문제다.

## 문자열에서 직접 제거 1

```py
class Solution:
    def isValid(self, s: str) -> bool:
        
        import re
        
        while 1:
            p = s
            s = re.sub(r'\(\)|\{\}|\[\]', '', s)
            if len(s) == len(p): break
                
        return False if s else True
```
{:.python}

정규식을 사용하여, `()`, `{}`, `[]` 인 경우를 계속 삭제해 나간다. 더 이상 삭제가 불가능하다면 (삭제전 문자열인 p 와 삭제후 문자열인 s 의 길이가 같다면) 반복을 종료하며, 종료 후 남게 되는 s 가 빈 문자열일 때만 True 를 리턴하는 구조다.

정규식에 대한 시간복잡도는 [위키피디아](https://en.wikipedia.org/wiki/Regular_expression#Implementations_and_running_times)를 참고해보면 여러가지로 표현하고는 있지만, 문자열을 순회하면서 패턴에 해당되는지를 판단하기에, 문자열 반복이 필수이므로 대략 O(n) 이라 볼 수 있을 것이다.

따라서 전체적인 시간복잡도는 O(n^2) 이며, 실행속도는 160 ms 가 나왔다.

## 문자열에서 직접 제거 2

```py
class Solution:
    def isValid(self, s: str) -> bool:
        
        while 1:
            p = s
            s = s.replace('()', '').replace('{}', '').replace('[]', '')
            if len(s) == len(p): break
                
        return False if s else True
```
{:.python}

정규식이 아닌 replace 함수를 사용하였다. 시간복잡도는 O(n^2) 으로 같지만, 실행속도는 44 ms 로 정규식보다 월등히 빨랐다.

## Stack

```py
class Solution:
    def isValid(self, s: str) -> bool:
        
        stack = []
        d = {')': '(', '}': '{', ']': '['}
        
        for x in s:
            if stack and x in d.keys():
                if stack[-1] == d[x]: stack.pop()
                else: return False
            else: stack.append(x)
                
        return False if stack else True
```
{:.python}

문자열을 x 로 순회하면서, stack 이 비어있거나 x가 여는 괄호라면 이를 stack 에 추가한다. 그리고 stack 에 뭔가 채워져있는 상태에서 닫는 괄호가 나온다면, stack 의 제일 마지막 문자와 비교하여, 짝이 맞는 경우네는 stack 을 pop 하고, 아닌 경우에는 즉시 False 를 리턴한다.

반복 종료에도 stack 이 남아있다면 False 를, 비어있다면 True 를 리턴하는 구조다.

시간복잡도는 O(n) 이며, 실행속도는 32 ms 가 나왔다.