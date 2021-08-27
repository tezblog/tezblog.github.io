---
layout: post
title: "108. Convert Sorted Array to Binary Search Tree"
updated: 2021-08-27
tags: [leetcode,easy,array,divide_and_conquer,tree,binary_search_tree,binary_tree]
---

## 문제

[https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

주어지는 오름차순 Array 를 BST 형태로 변환하는 문제다.

## Recursive

```py
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        
        def fn(node, i, j):
            if i > j: return None
            
            mid = (i+j)//2
            node.val = nums[mid]
            
            node.left = fn(TreeNode(), i, mid-1) 
            node.right = fn(TreeNode(), mid+1, j)
        
            return node
        
        return fn(TreeNode(), 0, len(nums)-1)
```
{:.python}

중간값 mid 를 구해서 BST 노드값으로 삼고, mid 를 기준으로 좌/우로 나눠 다시 재귀호출을 하는 구조다.

실행시간은 145ms 이었다.

## Iterative

```py
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        
        if not nums: return None
        
        bst = TreeNode()
        stack = [(bst, 0, len(nums)-1)]
        
        while stack:
            n, i, j = stack.pop()
            
            mid = (i+j)//2
            n.val = nums[mid]
            
            if i <= mid-1:
                n.left = TreeNode()
                stack.append((n.left, i, mid-1))
                
            if mid+1 <= j:
                n.right = TreeNode()
                stack.append((n.right, mid+1, j))
                
        return bst
```
{:.python}

실행시간은 133 ms 로 나왔다.