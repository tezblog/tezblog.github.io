---
layout: post
title: "104. Maximum Depth of Binary Tree"
updated: 2021-08-26
tags: [leetcode,easy,tree,depth_first_search,breadth_first_search,binary_tree]
---

## 문제

[https://leetcode.com/problems/maximum-depth-of-binary-tree/](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

트리를 모든 노드를 탐색하면서 최대 깊이를 구하면 된다.

## Breadth First Search

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

Queue 자료구조를 사용하여 BFS 탐색을 시행한다. queue 에 `(노드, 깊이)` 형태로 담고, 자식노드를 탐색할 때마다 깊이를 늘려나가면서, 최대깊이인지 탐색한다.

실행시간은 65 ms 이었다.

## Depth First Search

```py
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
    
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right)) if root else 0
```
{:.python}

재귀호출로 DFS 를 구현하였다. 자식노드가 None 이 아니라면 1 씩 계속 합산하면서 재귀호출 하는 구조다. 그 중 최대값을 리턴한다.

실행시간은 36 ms 로 나왔다.