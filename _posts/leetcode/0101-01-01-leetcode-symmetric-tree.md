---
layout: post
title: "101. Symmetric Tree"
updated: 2021-08-26
tags: [leetcode,easy,tree,depth_first_search,breadth_first_search,binary_tree]
---

## 문제

[https://leetcode.com/problems/symmetric-tree/](https://leetcode.com/problems/symmetric-tree/)

좌우를 둘로 나눈다음, 좌우가 대칭되도록 탐색하면서, 구조와 노드값이 끝까지 같은지를 판단하면 되는 문제다.

## Breadth First Search

```py
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        
        queue = [(root.left, root.right)]
        
        while queue:
            n1, n2 = queue.pop(0)
            
            if n1 and n2:
                if n1.val != n2.val: return False
                
                queue.append((n1.left, n2.right))
                queue.append((n1.right, n2.left))
                
            elif n1 or n2:
                return False
            
        return True
```
{:.python}

root 를 n1, n2 로 나눈다음, Queue 자료구조를 사용하여 BFS 방식으로 탐색을 진행한다. n1, n2 중 한쪽만 None 이거나, 대칭적으로 탐색되는 n1, n2 노드값이 서로 다르다면 즉시 False 를 리턴한다.

실행시간은 84 ms 가 나왔다.

## Depth First Search

```py
class Notsymmetric(Exception):
    pass

class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        
        try:
            def fn(n1, n2):
                if n1 and n2:
                    if n1.val != n2.val:raise Notsymmetric

                    fn(n1.left, n2.right)
                    fn(n1.right, n2.left)

                elif n1 or n2: raise Notsymmetric

            fn(root.left, root.right)
            
            return True
        
        except Notsymmetric:
            return False
```
{:.python}

재귀함수로 DFS 를 구현하여 탐색한다. isSymmetric 안에 재귀함수인 fn 함수가 있는데, fn 내부에서 대칭이 아니라는 판단이 내려지면, 외부함수인 isSymmetric 이 즉시 False 를 리턴하도록 했다.

내부함수가 외부함수를 리턴하는 직접적인 문법은 없으므로, try ~ except 구문을 이용, 예외 raise 하는 식으로 처리했다.

실행시간은 48 ms 이었다.