---
layout: post
title: "BFS (너비우선탐색) 과 DFS (깊이우선탐색) 구현 코드"
updated: 2021-03-30
tags: [algorithm,graph]
---

## 그래프 탐색

그래프의 모든 노드(정점)를 탐색하는 방식은 크게 두가지가 있다. BFS(너비우선탐색)과 DFS(깊이우선탐색)이 그것인데, 개념을 모른다면 [나무위키](https://namu.wiki/w/%EB%84%93%EC%9D%B4%20%EC%9A%B0%EC%84%A0%20%ED%83%90%EC%83%89)나 포털 등에서 검색을 먼저 해보기 바란다.

이 포스팅에서는 [이 pdf 문서](http://web.cs.unlv.edu/larmore/Courses/CSC477/bfsDfs.pdf) 와 [geeksforgeeks](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/) 내용을 주로 참고하여, BFS 및 DFS 코드 구현을 다뤄본다.

여기에 있는 코드들은 모두 아래 그래프를 탐색한다. 0 번부터 8 번까지 모든 노드들을 일정한 순서대로 탐색한다.

![그림00](/img/algorithm/graph/graph-0000.svg)

그래프를 코드로 옮겨보았다.

```python
g = {
    0: [1, 3, 7],
    1: [0, 2],
    2: [1],
    3: [0, 4, 6],
    4: [3, 5],
    5: [4],
    6: [3],
    7: [0, 8],
    8: [7],
}
```
{:.python}

그래프 g 는 `노드: [인접노드, 인접노드, 인접노드, ...]` 형태의 딕셔너리 자료형이다.

## BFS 코드

```python
def bfs(n, g):
    #1
    checked, queue = set(), []
    checked.add(n)
    queue.append(n)

    while queue:
        #2
        n = queue.pop(0)
        print(n, end=' ')

        #3
        for x in g[n]:
            if x not in checked:
                checked.add(x)
                queue.append(x)

bfs(0, g)   # 0 1 3 7 2 4 6 8 5
```
{:.python}

크게 세부분으로 구성되어 있다.

`#1` 부분은 초기화로, 탐색대상으로 지정됐는지를 나타내는 checked 집합자료형, 탐색노드들을 담을 queue 리스트를 생성한 다음, 탐색 스타트 노드를 checked 와 queue 에 담고, queue 가 빌 때까지 (더 이상 탐색할 노드가 없을 때까지) 반복을 시작한다.

`#2` 부분은 queue 에서 노드를 하나씩 꺼낸다. Queue 자료구조의 개념은 FIFO 이므로, 먼저 입력이 된 제일 앞 데이터요소부터 꺼낸다. 그리고 하고 싶은 처리를 하는데, 여기서는 단순히 어떤 노드를 출력하도록 했다.

`#3` 부분은 다음으로 탐색할 노드를 지정하여 queue 에 담는다. `#2` 에서 처리한 노드의 인접노드를 순회하여, 만일 checked 가 아니라면 (즉 탐색대상으로 지정된 적이 없다면) checked 와 queue 에 해당노드를 담는다.

실행결과와 그래프 모양을 비교해보면 BFS 가 어떤 순서로 탐색을 진행하는지 알 수 있을 것이다.

## DFS_iterative 코드

DFS 방식은 크게 두가지가 있는데, 하나는 반복문을 사용한 iterative 방식이고, 다른 하나는 재귀호출을 사용한 recursive 방식이다. iterative 방식을 먼저 소개한다.

```python
def dfs_iterative(n, g):
    #1
    checked, stack = set(), []
    checked.add(n)
    stack.append(n)

    while stack:
        #2
        n = stack.pop()
        print(n, end=' ')

        #3
        for x in g[n]:
            if x not in checked:
                checked.add(x)
                stack.append(x)

dfs_iterative(0, g)   # 0 7 8 3 6 4 5 1 2
```
{:.python}

위 bfs 함수와 구성은 동일하다. 사실상 다른점은 딱 한가지인데, `#2` 에서 노드를 하나씩 꺼낼 때, Stack 자료구조 개념에 따라 LIFO 방식, 즉 제일 나중에 삽입된 데이터요소부터 꺼낸다.

다만 익히 DFS 를 아는 분들은 탐색 순서가 좀 다르다고 느낄 수 있는데, 이 땐 `for x in g[n]:` 구문을 `for x in g[n][::-1]:` 로 바꿔서 역순으로 순회를 하도록 해보자. 그럼 출력이 0 1 2 3 4 5 6 7 8 로 나오게 된다.

## DFS_recursive 코드

```python
def dfs_recursive(n, g):
    def fn(n, checked):
        #2
        print(n, end=' ')
        
        #3
        for x in g[n]:
            if x not in checked:
                checked.add(x)
                fn(x, checked)

    #1
    checked = set()
    checked.add(n)
    fn(n, checked)

dfs_recursive(0, g)     # 0 1 2 3 4 5 6 7 8
```
{:.python}

dfs_recursive 함수 안에 재귀호출을 위한 fn 함수가 들어있다. 모양은 좀 달라보이지만 `#1`, `#2`, `#3` 부분이 동일하게 코딩되어있는 것은 같다.

재귀호출이 함수스택을 사용하는 것이기에, Stack 자료구조를 사용하는 DFS 와 원리는 비슷하다.