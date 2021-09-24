---
layout: post
title: "리스트 데이터요소 랜덤하게 셔플"
updated: 2021-04-19
tags: [algorithm,list]
---

## 리스트 셔플

셔플 (Shuffle) 은 어떤 리스트의 데이터요소 순서를 무작위로 뒤섞어 재배치하는 것을 의미한다. 가장 유명한 알고리즘은 [Fisher–Yates 알고리즘](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle#Fisher_and_Yates'_original_method)으로, 리스트의 제일 앞 인덱스부터 시작하여, 그보다 뒤에 있는 인덱스를 랜덤으로 선택하여 서로의 요소값을 교환해가는 방식이다.

아래 그림을 보면 어떤 방식인지 이해가 될 것이다.

![그림00](/img/algorithm/list/list-0002.svg)

그림을 보면 자기 자신과의 교환도 허용한다. 그리고 반드시 앞에서만이 아니라 뒤에서 출발하여, 보다 앞 인덱스의 요소와 교환하는 식으로도 구현할 수 있다.

## Fisher-Yates 알고리즘

```py
from random import randrange

def shuffle(arr):
    for i in range(len(arr)-1):
        j = randrange(i, len(arr))
        arr[i], arr[j] = arr[j], arr[i]
        
a = [1, 2, 3, 4, 5]
shuffle(a)
print(a)    # 결과 다양함...
```

어느 인덱스와 교환할지 랜덤선택이 필요하기에 `random` 모듈에서 `randrange` 함수를 import 했다.

## random 모듈의 shuffle 함수

사실 Python 은 위에서 사용한 `random` 모듈 안에 `shuffle` 함수를 자체 제공하고 있다. 사용방식도 위 `shuffle` 함수와 동일하다. 보다 자세한 사항은 [Python 공식문서](https://docs.python.org/3/library/random.html)를 참고해보자.