---
layout: post
title: (프로그래머스 / python3) - 프로세스

description: >
  (프로그래머스 / python3) - 프로세스

categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
운영체제의 역할 중 하나는 컴퓨터 시스템의 자원을 효율적으로 관리하는 것입니다. 이 문제에서는 운영체제가 다음 규칙에 따라 프로세스를 관리할 경우 특정 프로세스가 몇 번째로 실행되는지 알아내면 됩니다.

> 1. 실행 대기 큐(Queue)에서 대기중인 프로세스 하나를 꺼냅니다.
> 2. 큐에 대기중인 프로세스 중 우선순위가 더 높은 프로세스가 있다면 방금 꺼낸 프로세스를 다시 큐에 넣습니다.
> 3. 만약 그런 프로세스가 없다면 방금 꺼낸 프로세스를 실행합니다.<br>
>     3.1 한 번 실행한 프로세스는 다시 큐에 넣지 않고 그대로 종료됩니다.
예를 들어 프로세스 4개 [A, B, C, D]가 순서대로 실행 대기 큐에 들어있고, 우선순위가 [2, 1, 3, 2]라면 [C, D, A, B] 순으로 실행하게 됩니다.

현재 실행 대기 큐(Queue)에 있는 프로세스의 중요도가 순서대로 담긴 배열 `priorities`와, 몇 번째로 실행되는지 알고싶은 프로세스의 위치를 알려주는 `location`이 매개변수로 주어질 때, 해당 프로세스가 몇 번째로 실행되는지 return 하도록 solution 함수를 작성해주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>

1. 프로세스들의 우선순위가 담긴 배열 priorities와 각 프로세스의 인덱스를 추적하기 위한 배열을 deque로 변환하여 각각 `priorities_q`와 `idx_q`에 저장합니다
2.  현재 큐에 남아있는 프로세스들 중에서 가장 높은 `우선순위(max_pri)`를 확인합니다.
3. 큐에서 꺼낸것의 우선순위가 `우선순위(max_pri)` 보다 작으면 다시 큐에 넣습니다.
4. 큐에서 꺼낸것의 우선순위가 `우선순위(max_pri)` 보다 작지 않으면 프로세스를 실행한 것으로 처리합니다. 실행된 프로세스의 인덱스를 `answer` 리스트에 추가하고, 만약 이 프로세스가 우리가 찾고자 하는 프로세스라면(location과 비교), 몇 번째로 실행되었는지를 반환합니다

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque  
def solution(priorities, location):
    answer = []  
    priorities_q = deque(priorities)  # 우선순위 리스트를 deque로 변환
    idx_q = deque([i for i in range(len(priorities))])  # 각 문서의 인덱스를 deque로 변환하여 저장
    
    # 큐가 빌 때까지 반복
    while priorities_q:
        max_pri = max(priorities_q)  # 현재 남아있는 문서 중 최대 우선순위 확인
        
        now_pri = priorities_q.popleft()  # 현재 우선순위를 꺼냄
        now_idx = idx_q.popleft()  # 현재 인덱스를 꺼냄
        
        # 현재 문서의 우선순위가 최대 우선순위가 아니면 다시 큐에 넣음
        if now_pri != max_pri:
            priorities_q.append(now_pri)  
            idx_q.append(now_idx)  
    
        # 현재 문서의 우선순위가 최대 우선순위와 같으면 해당 문서를 인쇄함
        else:
            answer.append(now_idx)  # 인쇄된 문서의 인덱스를 기록
            if now_idx == location:  # 인쇄된 문서가 목표 문서인지 확인
                return len(answer)  # 목표 문서가 인쇄된 순서를 반환
    
    return -1  # 예외 처리 (이론적으로는 도달하지 않음)

~~~









