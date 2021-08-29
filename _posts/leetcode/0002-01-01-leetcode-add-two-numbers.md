---
layout: post
title: "2. Add Two Numbers"
updated: 2021-08-29
tags: [leetcode,node]
---

## 문제

[https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)

뒤집힌 Linked List 형태로 되어 있는 두 숫자의 덧셈 결과를 Linked List 로 리턴하면 되는 문제다.

가장 먼저 든 생각은, Linked List 를 순회하여 정수나 리스트로 나타낸 다음, 덧셈을 하고, 이를 다시 Linked List 로 변환하여 리턴하면 되겠구나 였다. 하지만 이는 이 문제가 의도하는 바는 아닐 것이다.

Linked List 를 순회하면서, 각 노드가 나타내는 숫자들을 더하고, 그 결과를 새로운 Linked List 로 직접 만들어간 뒤, 이를 리턴하라는 의도일 것이다.

## Linked List 순회 풀이

```py
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        added = n = ListNode()
        carry = 0
        
        while l1 or l2 or carry:
            n.next = n = ListNode()
            t = carry
            
            if l1:
                t += l1.val
                l1 = l1.next
            if l2:
                t += l2.val
                l2 = l2.next
                
            carry, n.val = divmod(t, 10)
            
        return added.next
```
{:.python}

added 라는 Linked List 노드를 먼저 생성한다. 그리고 이를 n 이라는 변수가 이를 가리키도록 했다. 덧셈 자릿수가 늘어날 때마다 새로운 노드가 만들어질 것이고, 계속 n 이 새로운 노드를 가리키도록 할 것이다.

주어진 l1, l2 Linked List 와 결과 added 는 모두 뒤집혀진 형태 (일의 자리가 먼저 나오는 형태) 이므로 자연스럽게 순서대로 계산해나가면 된다.

carry 라는 변수도 설정했는데, 이는 올림을 의미한다. 초등학교 때 배웠던 덧셈을 생각하면, 예를들어 8 + 9 같은 경우, 같은 자리에는 7 이 그리고 자릿수 올림으로 1 을 계산했던 것을 기억할 것이다. 하나의 노드에는 한자리의 숫자를 저장해야하므로, 올림이 발생하는 경우 이를 별도의 변수인 carry 에 담아 다음 자릿수 계산할 때 참조할 것이다.

while 반복문이 코드 구현의 핵심이다. l1, l2, carry 중 하나라도 None 이나 0 이 아니라면 덧셈이 안 끝난 것이므로 while 내부를 실행한다. 덧셈을 위한 노드를 만들고 n 이 이를 가리키도록 한다. 그리고 t 라는 임시변수에 carry, l1, l2 를 더한다. 그리고 이를 10 으로 나눈 나머지를 n 의 값으로 지정하고, 몫은 올림이므로 이를 다시 carry 에 담는 식이다.