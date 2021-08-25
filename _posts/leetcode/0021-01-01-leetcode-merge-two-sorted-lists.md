---
layout: post
title: "21. Merge Two Sorted Lists"
updated: 2021-08-24
tags: [leetcode,easy,linked_list,recursion]
---

## 문제

[https://leetcode.com/problems/merge-two-sorted-lists/](https://leetcode.com/problems/merge-two-sorted-lists/)

두 개의 Linked List 를, 오름차순으로 정리하면서 하나의 Linked List 로 합치는 문제다.

## Iterative

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

l1, l2 가 정의하고 있던 각 노드의 연결고리가 재조정되어, 새로운 Linked List 가 만들어지는 셈이다.

실행시간은 40 ms 이었다.

## Recursive

```py
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        
        def fn(n, l1, l2):
            if l1 and l2:
                if l1.val < l2.val:
                    n.next = l1
                    fn(n.next, l1.next, l2)
                else:
                    n.next = l2
                    fn(n.next, l1, l2.next)
            else:
                n.next = l1 or l2
                
        merged = ListNode()
        fn(merged, l1, l2)
        
        return merged.next
```
{:.python}

앞선 반복문 방식을 재귀호출 방식으로 변형한 풀이다. 실행시간은 40 ms 으로 같았다.