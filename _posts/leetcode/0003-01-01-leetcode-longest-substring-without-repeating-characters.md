---
layout: post
title: "3. Longest Substring Without Repeating Characters"
updated: 2021-08-29
tags: [leetcode,list]
---

## 문제

[https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

주어진 문자열에서, 문자 중복없는 가장 긴 sub 문자열을 찾아 그 길이를 리턴하면 되는 문제다.

이중루프를 구현해서 모든 sub 문자열을 찾아, 문자 중복없는 케이스들만 골라, 가장 긴 길이를 찾아낼 수도 있다. 하지만 좀 더 효율적인 방법을 사용할 수 있는데, 여기서는 Two Pointer 알고리즘을 사용했다.

## Two Pointer 알고리즘 사용

```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        maxsubsize = 0
        i, j = 0, 0
        
        while j < len(s):
            if s[j] in s[i:j]:
                i += 1
            else:
                maxsubsize = max(maxsubsize, j-i+1)
                j += 1
        
        return maxsubsize
```
{:.python}

maxsubsize 는 sub 문자열 길이의 최대치로, 0 으로 초기화되어있다.

다음으로 문자열 s 의 특정 위치를 가리키는 두개의 포인터 i, j (항상 i <= j) 를 설정한다. i, j 가 가리키는 문자를 포함하여 그 사이에 있는 문자들을 sub 문자열로 삼는다.

처음에는 i, j 모두 0 인덱스에 있는 문자를 가리키다가, sub 문자열이 모두 고유문자로만 구성돼있다면 j 를 증가시키고, 중복 문자가 있다면 i 를 증가시킨다. j 가 문자열의 끝을 넘어서면 반복을 종료한다. sub 문자열이 고유문자열만으로 되어있는지는 `s[j] in s[i:j]` 로 판단하는데, 새로 추가된 j 인덱스의 문자가, 그 보다 앞에 있는 문자 s[i:j] 안에 들어있는지를 검사하는 방식이다.

매 반복마다, sub 문자열이 모두 고유문자열이라면, sub 문자열의 길이 `j-i+1` 을 최대치로 계속 갱신한다.