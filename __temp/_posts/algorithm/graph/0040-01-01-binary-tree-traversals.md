---
layout: post
title: "이진 트리 순회하는 세가지 방법"
updated: 2021-04-19
tags: [algorithm,graph]
---

## Preorder, Inorder, Postorder

이진 트리를 순회하는 방법에는 여러가지가 있겠지만, 보통 전위(Preorder), 중위(Inorder), 후위(Postorder) 방식의 탐색을 얘기한다.

트리 구조가 아래와 같다면, 각 방식 순회의 결과는 그림 아래와 같다.

![그림00](/img/algorithm/graph/graph-0006.svg)

```plaintext
Preorder: Root -> Left -> Right (0, 1, 3, 4, 6, 7, 2, 5, 8)
Inorder: Left -> Root -> Right (3, 1, 6, 4, 7, 0, 2, 8, 5)
Postorder: Left -> Right -> Root (3, 6, 7, 4, 1, 8, 5, 2, 0)
```
{:.pseudo}

## 코드로 구현

우선 Node 클래스를 생성, 위 트리와 같은 형태로 자료를 넣은 `tree` 라는 변수를 생성한다.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

tree = Node(0)
tree.left = Node(1)
tree.left.left = Node(3)
tree.left.right = Node(4)
tree.left.right.left = Node(6)
tree.left.right.right = Node(7)
tree.right = Node(2)
tree.right.right = Node(5)
tree.right.right.left = Node(8)
```

보통 재귀함수를 사용하여 Preorder, Inorder, Postorder 탐색을 한다. 다른 방법에 대해서는 [위키피디아](https://en.wikipedia.org/wiki/Tree_traversal) 내용을 참고하자.

아래는 이를 구현한 코드로, 위에서 생성한 `tree` 를 인수로 넘기면 된다.

```python
# 전위 탐색
def preorder(tree):
    if tree:
        print(tree.data)
        preorder(tree.left)
        preorder(tree.right)

preorder(tree)    # 0, 1, 3, 4, 6, 7, 2, 5, 8

# 중위 탐색
def inorder(tree):
    if tree:
        inorder(tree.left)
        print(tree.data)
        inorder(tree.right)

inorder(tree)     # 3, 1, 6, 4, 7, 0, 2, 8, 5

# 후위 탐색
def postorder(tree):
    if tree:
        postorder(tree.left)
        postorder(tree.right)
        print(tree.data)

postorder(tree)   # 3, 6, 7, 4, 1, 8, 5, 2, 0
```

코드를 보면 전위, 중위, 후위 코드가 모두 닮았는데, 탐색한 노드의 처리 (위 코드에서는 노드 data 를 print) 가 어디에 들어가냐에 따라 구별된다.