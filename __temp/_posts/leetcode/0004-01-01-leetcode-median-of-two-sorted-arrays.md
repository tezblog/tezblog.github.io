---
layout: post
title: "4. Median of Two Sorted Arrays"
updated: 2021-09-01
tags: [leetcode,array]
---

## 문제

[https://leetcode.com/problems/median-of-two-sorted-arrays/](https://leetcode.com/problems/median-of-two-sorted-arrays/)

오름차순으로 정렬되어 있는 nums1, nums2 라는 Array 가 주어지고, 이 두 Array 들이 가지는 값들 중에서 중간값을 찾아 리턴하는 문제다. O(log(m+n)) 시간복잡도로 해결해보라 하고 있다.

가장 간단히 해결할 수 있는 방법은, 반복문으로 nums1 과 nums2 의 앞에 있는 숫자들을 하나씩 pop 해가면서 비교하여 찾아내면 된다. 하지만 이는 시간복잡도가 O(m+n) 으로 이 문제가 의도하는 답안은 아닐 것이다.

처음부터 오름차순으로 정렬된 Array, log 시간복잡도 라는 의미를 생각하면 결국 이진탐색 (Binary Search) 알고리즘을 응용하라는 이야기로 보인다. 두 개 Array 를 이진탐색하는 방법은 구글링해보면 여러가지가 나오는데, 아래와 같은 방법으로 시도하였다.

이제, A, B 안에 있는 모든 요소를 오름차순으로 정렬했을 때, k 인덱스에 해당하는 값을 찾는다. 예를들어 k 가 6 이라고 했을때, `(A+B)[6]` 을 찾는 것이라고 해보자. 그리고 이는 앞에서부터 7 번째에 해당하는 요소이다.

우선 6 의 절반은 3 이므로, A 앞에서부터 3 개를, 나머지 4 개를 B 앞에서부터 골라낸다고 해보자. A 의 3 번째 요소의 인덱스는 2, B 의 4 번째 요소의 인덱스는 3 이므로, `A[2]` 와 `B[3]` 의 값을 아래와 비교해 볼 수 있다.

```plaintext
만일 A[2] < B[3] 이면, A[2] 이하의 모든 요소를 삭제한다.

     v  v  v
A = [x, x, x, x, x, x, x, x, ... ]
     -------
        | A[2] 이하의 모든 요소는 B[3] 보다 앞부분에 위치하며,
        | 따라서 반드시 (A+B)[6] 보다 앞에 위치하기 때문이다.
        V
     -------   
     v  v  v  v
B = [x, x, x, x, x, x, x, x]


만일 A[2] > B[3] 이면, B[3] 이하의 모든 요소를 삭제한다.

     v  v  v
A = [x, x, x, x, x, x, x, x, ... ]
     ----
       ^
        \ B[3] 이하의 모든 요소는 A[2] 보다 앞부분에 위치하며,
         \  따라서 반드시 (A+B)[6] 보다 앞에 위치하기 때문이다.
     ----------   
     v  v  v  v
B = [x, x, x, x, x, x, x, x]

```
{:.pseudo}

예를들어 `A[2]` 이하의 모든 요소를 삭제했다면 3 개의 요소를 삭제한 셈이므로, A 에서 앞 3 개의 요소를 제외한 `A[3:]` 과, 온전한 B, 그리고 앞선 3 개가 제외되었으므로 k 는 3 으로 하여 재귀호출을 한다.

## 이진탐색 응용

```py
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        length = len(nums1) + len(nums2)
        
        return ( get_k(nums1, nums2, (length-1)//2) + get_k(nums1, nums2, length//2) ) / 2
    
def get_k(A, B, k):
    if not(A and B): return (A or B)[k]
    if k == 0: return min(A[0], B[0])
    
    if len(A) > len(B): A, B = B, A
        
    ia = min(k//2, len(A)-1)
    ib = k - ia - 1
    
    if A[ia] < B[ib]: return get_k(A[ia+1:], B, ib)
    else: return get_k(A, B[ib+1:], ia)
```
{:.python}

`(A+B)[k]` 를 찾아내는 핵심 로직은 get_k 함수에 담겨있다.

A 또는 B 가 빈 배열이라면, 이진탐색을 할 필요없이 즉시 비어있지 않은 Array 의 k 인덱스 값을 리턴한다. 그리고 k 가 0 이라면, `A[0]` 또는 `B[0]` 중 작은 값을 리턴한다.

다음으로는 A, B 크기를 비교하여 짧은 Array 가 A 가 되도록 한다. 다음으로는 A 에서 x 개, B 에서 y 개를 골라내야 하는데, 골라낸 요소들의 제일 오른쪽 인덱스를 각각 ia, ib 로 설정하였다. 참고로 ia 이하의 요소개수 + ib 이하의 요소개수는 k 이하의 요소개수가 되도록 되어있다.

그리고 위에서 언급했던대로, `A[ia]` 와 `B[ib]` 를 비교하여, 필요없는 부분을 삭제한 다음, 다시 재귀호출을 한다.