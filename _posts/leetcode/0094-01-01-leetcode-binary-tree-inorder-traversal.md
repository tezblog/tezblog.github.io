---
layout: post
title: "94. Binary Tree Inorder Traversal"
updated: 2021-08-25
tags: [leetcode,easy,stack,tree,depth_first_search,binary_tree]
---

## 문제

[https://leetcode.com/problems/binary-tree-inorder-traversal/](https://leetcode.com/problems/binary-tree-inorder-traversal/)

주어진 이진 트리를 Inorder 순회하는 코드를 구현하면 된다.

## Recursive

```py
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
        a = []
        
        def fn(tree, a):
            if tree:
                fn(tree.left, a)
                a += [tree.val]
                fn(tree.right, a)
                
        fn(root, a)
        
        return a
```
{:.python}

실행시간은 28 ms 이었다.

탐색한 노드 처리를 a 리스트에 그 값을 담는 것으로 하고있는데, 노드 처리를 하는 위치에 따라 순회 방식이 Preorder, Inorder, Postorder 로 구분된다. 아래와 같다.

```py
# Preorder 순회
def fn(tree, a):
    if tree:
        a += [tree.val]    # 탐색노드 처리
        fn(tree.left, a)
        fn(tree.right, a)
        
# Inorder 순회
def fn(tree, a):
    if tree:
        fn(tree.left, a)
        a += [tree.val]    # 탐색노드 처리
        fn(tree.right, a)
        
# Postorder 순회
def fn(tree, a):
    if tree:
        fn(tree.left, a)
        fn(tree.right, a)
        a += [tree.val]    # 탐색노드 처리
```
{:.python}

## Iterative

```py
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        
        a = []
        stack = [(root, False)]
        
        while stack:
            n, v = stack.pop()
            
            if n:
                if v:
                    a += [n.val]
                else:
                    stack.append((n.right, False))
                    stack.append((n, True))
                    stack.append((n.left, False))
                
        return a
```
{:.python}

노드를 탐색할 때, 다음 노드를 탐색해야할 순서인지, 노드값을 처리해야 할 순서인지를 담기 위해 `(노드, 처리여부)` 의 튜플자료형을 stack 에 담아 처리한다.

구글링을 해보면 다양한 반복문 코드들을 볼 수 있는데, 위 코드가 재귀호출 방식과도 유사하고, 다른 순회 방식과도 유사하기 때문에 개인적으로는 이 코드를 선호한다.

실행시간은 32 ms 가 나왔다.

반복문으로 구현된 Preorder, Inorder, Postorder 탐색들은 아래와 같다.

```py
# Preorder 순회
while stack:
    n, v = stack.pop()
    
    if n:
        if v:
            print(n.data, end=' ')
        else:
            stack += [(n.right, False)]
            stack += [(n.left, False)]
            stack += [(n, True)]

# Inorder 순회
while stack:
    n, v = stack.pop()
    
    if n:
        if v:
            print(n.data, end=' ')
        else:
            stack += [(n.right, False)]
            stack += [(n, True)]
            stack += [(n.left, False)]
            
            
# Postorder 순회
while stack:
    n, v = stack.pop()
    
    if n:
        if v:
            print(n.data, end=' ')
        else:
            stack += [(n, True)]
            stack += [(n.right, False)]
            stack += [(n.left, False)]
```
{:.python}

## Morris Traversal

TBD