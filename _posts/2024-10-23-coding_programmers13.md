---
layout: post
title: (프로그래머스 / python3) - 기능개발

description: >
  (프로그래머스 / python3) - 기능개발

categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.
[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>

1. 각 기능이 완료되는 날짜를 계산하여 `end_date_list`에 저장합니다. 예를 들어, progresses[i]가 30%이고 speeds[i]가 20%라면, 해당 기능이 완료되기까지는 (100 - 30) / 20 = 3.5일이 필요합니다. 소수점을 올림 처리하여 4일 후에 완료되도록 계산합니다. 
2. `end_date_list`를 `deque`로 변환하여 큐 방식으로 관리합니다.
3. 종료일을 `now_date`에 저장하고 기능 개수를 `now_cnt`에 1로 설정합니다.
이때, 배포가 가능한 기능들을 그룹으로 묶어 계산하므로, q의 첫 번째 요소를 `now_date`로 설정해 해당 그룹의 배포 기준일로 삼습니다. 
4. 큐에서 기능이 종료될 때까지 반복합니다: <br>
   4-1. 종료일이 `now_date`보다 작거나 같으면 해당 기능은 앞의 기능들과 같은 날 배포될 수 있으므로 `now_cnt`를 1씩 증가시키고 큐에서 제거합니다. <br>
   4-2. 종료일이 `now_date`보다 크면 `now_cnt`를 `answer`에 추가하여 현재까지의 배포 개수를 저장하고, `now_date`를 업데이트하여 다음 기능 그룹으로 넘어갑니다. 
5. 반복이 끝난 후, 마지막으로 배포할 기능 그룹의 개수인 `now_cnt`를 `answer`에 추가하고 반환합니다.
<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque

def solution(progresses, speeds):
    answer = []
    end_date_list = []

    # 각 기능의 완료 날짜 계산
    for i in range(len(progresses)):
        if (100 - progresses[i]) % speeds[i] == 0:
            end_date = (100 - progresses[i]) // speeds[i]
        else:
            end_date = ((100 - progresses[i]) // speeds[i]) + 1
        end_date_list.append(end_date)

    q = deque(end_date_list)

    # 첫 기능의 배포 기준일과 카운트 초기화
    now_date = q.popleft()
    now_cnt = 1

    # 큐가 빌 때까지 반복
    while q:
        if now_date >= q[0]:
            q.popleft()
            now_cnt += 1
        else:
            answer.append(now_cnt)
            now_date = q.popleft()
            now_cnt = 1

    # 마지막 배포 그룹 추가
    answer.append(now_cnt)
    return answer


~~~









