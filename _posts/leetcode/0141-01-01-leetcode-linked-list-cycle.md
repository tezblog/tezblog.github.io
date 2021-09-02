---
layout: post
title: "141. Linked List Cycle"
updated: 2021-09-02
tags: [leetcode,graph]
---

## 문제

[https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)

주어지는 Linked List 가 반복적인 회전 (Cycle) 구조로 이뤄져 있는지를 검사하면 된다. 그리고 문제 말미에는 공간복잡도 O(1) 로 해결해 볼 것도 제시하고 있다.

가장 간단한 방법은 가쳐간 노드를 기록해두고는, 앞으로 거쳐갈 노드마다 한번 지나쳐 간 노드인지를 검사하면 된다. 하지만 이 방식은 방문을 했던 노드를 기록하기 위한 별도의 메모리 공간을 필요로 한다.

공간복잡도가 O(1) 이 되려면, 별도의 자료구조나 메모리 공간을 만들지 말고 해결해야 하는데, 가장 유명한 알고리즘은 플로이드의 거북이와 토끼 알고리즘 (Floyd's tortoise and hare) 이다. [위키피디아](https://en.wikipedia.org/wiki/Cycle_detection#Floyd's_tortoise_and_hare)를 참고해보자.

간단하게 말하자면, 두 개의 포인터 (거북이, 토끼) 를 두고는, 거북이는 한 노드씩, 토끼는 두 노드씩 전진한다. 회전 구조인 경우 언젠가 둘은 반드시 만나게 된다고 한다.

## Floyd's tortoise and hare

```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        try:
            t = head
            h = head.next
            
            while t != h:
                t = t.next
                h = h.next.next
                
            return True
        
        except AttributeError:
            return False
```
{:.python}

두 포인터 t, h 를 상정하고 head, head.next 를 가리키도록 한다. t, h 가 서로 같은 노드를 가리킬 때까지 t 는 한 노드를, h 는 두 노드를 전진한다.

만일 더 이상 길이 없는 경우 (None 을 만나는 경우) 에는 에러를 발생시키는데, 이 때는 회전 구조가 아니므로, 예외 처리로 넘겨 False 를 리턴한다.

## Hash Table 사용

```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        d = {}
        n = head
        
        while n:
            if id(n) in d: return True
            
            d[id(n)] = True
            n = n.next
            
        return False
```
{:.python}

거쳐간 노드를 d 딕셔너리에 담는다. 노드를 탐색하다가, 이미 거쳐간 노드라면 즉시 True 를 리턴한다. d 딕셔너리에 담을 때는, 개체의 고유번호를 반환하는 id 함수를 사용하였다.

이 방식은 별도의 메모리공간을 사용하므로, 문제가 의도하는 바는 아니지만, 가장 쉽게 생각할 수 있는 풀이인 듯 하다.

## Linked List 뒤집기

```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if head and head.next:
            p, n = None, head
            
            while n:
                p, n.next, n = n, p, n.next
            return p is head
        
        return False
```
{:.python}

회전 구조를 지닌 Linked List 는, 노드의 연결 방향을 뒤집다보면 결국은 다시 출발 노드 (head) 로 돌아오게 되는 특징을 가지고 있다. 이를 이용한 풀이다.

주어진 head 와 head.next 가 None 이 아닐때만 유용한데, head.next 까지 살펴야 하는 이유는, head.next 가 None 이라면, 노드라 1 개라는 뜻이므로 뒤집기의 효과가 없기 때문이다.

모든 경로를 뒤집고 나서, 뒤집에 사용한 포인터 p 가 다시 head 에 위치하고 있는지를 검사하여 최종리턴 한다.

이 방법은 공간복잡도가 O(1) 이지만, 주어진 Linked List 자료구조를 변형시켜버려, 재탐색이 불가능하다는 단점도 있다.