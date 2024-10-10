---
layout: post
title: (프로그래머스 / python3) - 호텔 대실
description: >
  (프로그래머스 / python3) - 호텔 대실
categories: programming
tags: 코딩테스트_문제_python_우선순위큐
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
최소한의 객실만을 사용하여 예약 손님들을 받으려고 합니다. <br>
한 번 사용한 객실은 퇴실 시간을 기준으로 10분간 청소를 하고 다음 손님들이 사용할 수 있습니다. <br>
예약 시각이 문자열 형태로 담긴 2차원 배열 `book_time`이 매개변수로 주어질 때 <br>
최소 객실의 수를 반환하세요 <br>


[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/155651)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
우선순위 큐 사용

1. `book_time`을 시작 시간, 끝나는 시간 순으로 정렬한다.
2. 각 예약의 시작 시간을 분 단위로 변환하고, 종료 시간에는 청소 시간을 고려하여 10분을 더해 계산한다.
3. `room_list`는 우선순위 큐로 관리되며, 방의 종료 시간을 저장한다. 예약이 들어올 때마다 가장 빨리 끝나는 방을 먼저 확인한다.
4. 새로운 예약의 시작 시간이 현재 우선순위 큐에 있는 방의 종료 시간 이후라면 그 방을 재사용할 수 있다. 이 경우 해당 방을 큐에서 제거한다.
5. 그렇지 않으면 새로운 방이 필요하므로 `answer` 값을 증가시킨다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
import heapq

# 분으로 바꿔주는 함수
def toMin(s):
    hh, mm = map(int,s.split(":"))
    return hh * 60 + mm
    
def solution(book_time):
    # 시작, 종료 시간 순으로 정렬
    sorted_book_time = sorted(book_time, key = lambda x : (x[0],x[1]))
    
    room_list = []
    answer = 0
    
    for s,e in sorted_book_time:
        s_time = toMin(s)
        e_time = toMin(e) + 10
        
        # 현재 사용할 수 있는 방이 있고, 가장 빨리 끝나는 방의 종료 시간보다 예약 시작 시간이 같거나 늦다면 해당 방 제거
        if room_list and s_time >= room_list[0]:
            heapq.heappop(room_list)
        else:
            answer += 1 # 새로운 방이 필요하므로 방 개수 증가
            
        # 해당 예약의 종료 시간을 우선순위 큐에 삽입
        heapq.heappush(room_list,e_time)
        
    return answer
~~~









