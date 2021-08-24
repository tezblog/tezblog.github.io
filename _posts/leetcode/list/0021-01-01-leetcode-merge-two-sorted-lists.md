---
layout: post
title: "21. Merge Two Sorted Lists"
updated: 2021-08-23
tags: [leetcode,list]
---

## 문제

[https://leetcode.com/problems/merge-two-sorted-lists/](https://leetcode.com/problems/merge-two-sorted-lists/)

두 개의 Linked List 를, 오름차순으로 정리하면서 하나의 Linked List 로 합치는 문제다.

## 주어진 Linked List 연결 순서를 재조정

```py
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        
        merged = n = ListNode()
        
        while l1 and l2:
            if l1.val < l2.val:
                n.next = l1
                n, l1 = n.next, l1.next
            else:
                n.next = l2
                n, l2 = n.next, l2.next
                
        n.next = l1 or l2
        
        return merged.next
```
{:.python}

l1 과 l2 가 모두 끝에 다다르지 않았다면, 값을 비교하면서 보다 적은 값 쪽의 노드를 연결하고, 다음 노드 비교를 준비한다. l1, l2 어느 한쪽이라도 끝에 다다랐다면, 아직 끝이 아닌 쪽을 나머지로 이어붙이는 식이다.

실행을 하고나면 원본인 l1, l2 는 사라지고, 새로운 Linked List 만이 남는다.

시간복잡도는 O(n) 이고, 실행속도는 40 ms 이었다.

## 새로운 Linked List 생성

```py
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        
        n = merged = ListNode()
        
        while l1 and l2:
            n.next = ListNode()
            n = n.next
            
            if l1.val < l2.val:
                n.val = l1.val
                l1 = l1.next
            else:
                n.val = l2.val
                l2 = l2.next
                
        r = l1 or l2
        while r:
            n.next = n = ListNode()
            n.val = r.val
            r = r.next
        
        return merged.next
```
{:.python}

앞선 코드와는 다르게 원본인 l1, l2 를 변형시키지 않고, 새로운 Linked List 를 생성하도록 되어있다.

시간복잡도는 O(n) 이고, 실행속도는 40 ms 으로 같았다.