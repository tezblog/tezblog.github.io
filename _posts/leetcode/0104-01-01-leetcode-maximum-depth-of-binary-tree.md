---
layout: post
title: "104. Maximum Depth of Binary Tree"
updated: 2021-08-26
tags: [leetcode,graph]
---

## 문제

[https://leetcode.com/problems/maximum-depth-of-binary-tree/](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

트리의 모든 노드를 탐색하고, 최대 깊이 (트리의 높이) 를 구하면 된다.

Stack 을 사용하는 DFS (Depth First Search) 방식과, Queue 를 사용하는 BFS (Breadth First Search) 방식으로 탐색할 수 있다. 자식 노드를 탐색할 때마다 깊이를 1 씩 늘려주고, 이 중 최대값을 리턴하면 된다.

## 함수 Stack 을 사용한 DFS 방식

```py
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right)) if root else 0
```
{:.python}

인수로 전달받은 root 가 None 이라면 0 을 리턴하고, 아니라면 `1 + 자식노드들 중 최대 깊이` 를 리턴하는 재귀호출 구조다.

함수 호출도 Stack 자료구조를 사용하므로, DFS 방식의 탐색이다.

## BFS 방식

```py
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root is None: return 0
        
        maxdepth = 1
        queue = [(root, 1)]
        
        while queue:
            n, d = queue.pop(0)
            maxdepth = max(maxdepth, d)
            
            if n.left: queue.append((n.left, d+1))
            if n.right: queue.append((n.right, d+1))
                
        return maxdepth
```
{:.python}

인수로 전달받은 root 가 자식노드가 있을 때마다, 깊이를 늘려가면서 BFS 방식으로 탐색한다.

root 가 None 이 아니라면, 초기값으로 maxdepth 에 1 을 할당하고, queue 에 `(root, 1)` 를 삽입한다. 이는 `(노드, 깊이)` 를 나타낸다.

queue 가 비어있지 않다면 (즉 탐색해야 할 노드가 남아있다면), 하나씩 노드를 꺼내고 최대 깊이 maxdepth 를 갱신한다. 그리고 꺼낸 노드에 자식 노드가 달려있다면, 길이를 1 단계 늘려서 다시 queue 에 삽입한다.

모든 노드 탐색이 끝나면 queue 는 비게 되어 반복문이 종료된다. 계산된 maxdepth 를 최종리턴하면 된다.

참고로 `n, d = queue.pop(0)` 을 `n, d = queue.pop()` 으로 고치면 Stack 자료구조가 되므로, "반복문을 사용한 DFS 방식" 이 된다. 탐색 순서만 달라질 뿐, 모든 노드를 탐색하는 것은 변함이 없으므로 결과는 변동없다.