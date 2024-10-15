---
layout: post
title: (코딩테스트_복기 / python3) - 추출 순서

description: >
  (코딩테스트_복기 / python3) - 추출 순서

categories: programming
tags: 코딩테스트_문제_python_heapq
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
주어진 커피 추출 시간을 이용하여 커피를 추출하는 순서를 결정하는 문제입니다. 각 커피의 추출 시간은 리스트 형태 `coffee_time`으로 주어지며, 동시에 `N`개의 커피를 추출할 수 있습니다.

예를 들어 `coffee_time = [4, 2, 2, 5, 3]` 이고 `N = 3` 이면

| time | remain          | current_coffee_time | result      |
|:-----|:----------------|:--------------------|-------------|
| 0    | [4, 2, 2, 5, 3] | -                   | -           |
| 1    | [5, 3]          | [4, 2, 2]           | -           |
| 3    | []              | [1, 5, 3]           | [2,3]       |
| 4    | []              | [4, 2]              | [2,3,1]     |
| 7    | []              | [1]                 | [2,3,1,5]   |
| 9    | []              | []                  | [2,3,1,5,4] |


<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
heapq

1. 주어진 coffee_time 리스트에서 커피의 추출 시간을 인덱스와 함께 튜플 형태로 변환하여 정렬합니다. 
2. 동시에 추출할 수 있는 N개의 커피를 선택하고, 최소 추출 시간을 가진 커피부터 처리합니다. 
3. 가장 빨리 추출 완료된 커피를 결과 리스트에 추가하고, 해당 커피의 추출 시간을 다른 커피들의 추출 시간에서 차감합니다. 
4. 다음 추출할 커피가 있는 경우, 현재 배치에서 가장 늦게 완료될 커피를 확인하고 새로운 커피를 추가합니다. 
5. 이 과정을 반복하여 모든 커피가 추출될 때까지 진행합니다. 
6. 최종적으로 추출된 커피의 순서를 포함한 결과 리스트를 반환합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
import heapq


def coffee_order(coffee_time, N):
    # 커피 추출 순서를 담을 리스트
    result = []

    # (커피 추출 시간, 인덱스) 형태로 리스트를 생성
    indexed_times = [(time, idx + 1) for idx, time in enumerate(coffee_time)]

    # 현재 추출 중인 커피 리스트
    current_batch = []

    # 처음 N개 커피를 우선 처리
    for i in range(min(N, len(coffee_time))):
        heapq.heappush(current_batch, indexed_times[i])

    # 추출된 커피의 개수
    next_coffee_index = N

    # 처리할 커피가 남아 있을 때까지 반복
    while current_batch:
        # 현재 배치에서 가장 빨리 끝나는 커피 추출
        min_time, idx = heapq.heappop(current_batch)
        result.append(idx)

        # 현재 배치에서 추출된 만큼의 시간을 다른 커피들에 반영
        for i in range(len(current_batch)):
            current_batch[i] = (current_batch[i][0] - min_time, current_batch[i][1])

        # 배치가 끝난 후 남아 있는 커피들 중에서 새로 추출할 커피 추가
        if next_coffee_index < len(coffee_time):
            heapq.heappush(current_batch, indexed_times[next_coffee_index])
            next_coffee_index += 1

    return result


# 예시
coffee_time = [4, 2, 2, 5, 3]
N = 4
print(coffee_order(coffee_time, N))

~~~









