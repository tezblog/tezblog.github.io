---
layout: post
title: "바이너리 힙(Heap) 자료구조"
updated: 2021-04-02
tags: [algorithm,graph]
---

## 바이너리 힙 (Binary Heap) 자료구조

[나무위키](https://namu.wiki/w/%ED%9E%99%20%ED%8A%B8%EB%A6%AC)에 소개한 것처럼 여러 개의 값 중에서 가장 크거나 작은 값을 빠르게 찾기 위해 고안된 자료구조다. 힙 정렬, 우선순위 큐 등 구현에 주로 사용되며, 부모 노드 아래에 최대 두개의 자식 노드가 달린 이진 트리 형태를 띄고 있다. 

![그림00](/img/algorithm/graph/graph-0001.svg)

위쪽에 위치한 노드일수록 더 작은 값이 위치한다면 "최소 힙(Min Heap)", 더 큰 값이 위치한다면 "최대 힙(Max Heap)이라 한다. 가장 위에 위치한 Root 노드는, 최소 힙이라면 트리에서 가장 작은 값을, 최대 힙이라면 가장 큰 값을 가지게 된다.

트리 안에 중복값이 포함될 수도 있다. 또한 부모-자식 간에는 정렬이 보장되지만, 그 외에는 정렬이 되어있음을 보장하지는 않는 특징을 보인다.

## 힙과 리스트 관계

힙의 각 노드를 아래 그림처럼 순서대로 인덱스화 했을 때,

![그림01](/img/algorithm/graph/graph-0002.svg)

어떤 노드 n 의 부모, 자식 노드 관계는 아래와 같다.

```plaintext
n 노드의 부모 노드는 (n-1)//2
n 노드의 왼쪽 자식 노드는 n*2+1
n 노드의 오른쪽 자식 노드는 n*2+2
```
{:.pseudo}

부모, 자식 간 노드 관계를 수식으로 명확하게 구할 수 있기 때문에, 힙 자료구조는 보통 리스트(배열)로 구현한다.

## 데이터 삽입과 추출

"최소 힙" 자료구조를 가정한다. 데이터 삽입은 아래와 같다.

```plaintext
리스트 제일 뒤에 새로운 value 삽입
삽입한 value 와 부모 value 비교, 삽입 value < 부모 value 라면 둘을 스왑
계속해서 상위의 부모 value 와 비교, 더 이상 스왑이 일어나지 않을 때까지 진행
```
{:.pseudo}

![그림02](/img/algorithm/graph/graph-0003.svg)

이번에는 "최소 힙" 자료구조에서 데이터를 추출 해보자. Root 노드의 값을 가져오므로 당연히 가장 작은 값이 추출된다. 그리고 다시 힙을 아래와 같이 재정비한다.

```txt
리스트 제일 앞 value 를 리턴하고 리스트에서 삭제한 후, 리스트 제일 뒤 value 를 제일 앞으로 옮김
옮겨진 value 와 자식 value 비교, min(왼쪽 자식 value, 오른쪽 자식 value) < 옮겨진 value 면, 더 작은 value 와 스왑
계속해서 하위의 자식 value 와 비교, 더 이상 스왑이 일어나지 않을 때까지 진행
```
{:.pseudo}

그림으로 표현하면 아래와 같다.

![그림03](/img/algorithm/graph/graph-0004.svg)

## Python 코드로 구현

"최소 힙" 자료구조를 가정한다.

```py
def heappush(heap, item):
    heap += [item]
    n = len(heap)-1
    p = (n-1)//2
    while n > 0:
        if heap[p] <= heap[n]: break
        else:
            heap[p], heap[n] = heap[n], heap[p]
            n = p
            p = (n-1)//2

def heappop(heap):
    item = heap[0]
    tmp = heap.pop()
    if heap:
        heap[0] = tmp
        n = 0
        lc, rc = n*2+1, n*2+2
        while rc <= len(heap):
            c = lc if rc == len(heap) or heap[lc] < heap[rc] else rc
            if heap[n] <= heap[c]: break
            else:
                heap[n], heap[c] = heap[c], heap[n]
                n = c
                lc, rc = n*2+1, n*2+2
    return item

def heapify(lst):
    tmp = []
    for x in lst: heappush(tmp, x)
    lst[:] = tmp
    
h = [7, 13, 3, 9, 11, 9, 15, 17, 14, 16]
heapify(h)
print(h)        # [3, 9, 7, 13, 11, 9, 15, 17, 14, 16]

heappush(h, 5)
print(h)        # [3, 5, 7, 13, 9, 9, 15, 17, 14, 16, 11]

heappop(h)      # 3 을 Pop
print(h)        # [5, 9, 7, 13, 11, 9, 15, 17, 14, 16]
```
{:.python}

heappush 는 데이터 삽입을, heappop 은 데이터 추출을, heapify 는 인수로 넘겨진 리스트를 힙 자료구조로 전환하는 역할을 한다.

## 참고

실제로 Python 에는 최소 힙 자료구조를 구현해주는 모듈이 존재한다. `heapq` 모듈인데, 위에서 구현해 본 코드들은 `heapq` 모듈에 있는 함수들의 사용방식과 동일하도록 만든 것이다. (물론 Python 에서 제공하는 함수가 더 효율적이고 정교할 것이다.) 자세한 사항은 [Python 공식문서](https://docs.python.org/ko/3.10/library/heapq.html)를 참고해보자.