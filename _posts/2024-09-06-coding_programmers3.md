---
layout: post
title: (프로그래머스 / python3) - 요격 시스템
description: >
  (프로그래머스 / python3) - 요격 시스템
categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>


A나라와 B나라가 싸우고 있는데 A나라에서 발사한 미사일은 x축에 평평한 직선형태의 모양입니다. <br>
정수쌍 <span style="background-color: yellow;">[s,e]</span>의 형태로 표현됩니다. <br>
B 나라는 특정 x 좌표에서 y 축에 수평이 되도록 미사일을 발사하며, 발사된 미사일은 해당 x 좌표에 걸쳐있는 모든 폭격 미사일을 관통하여 한 번에 요격할 수 있습니다.<br>
A나라에서 발사한 미사일이 targets라는 변수로 주어질때 B나라에서 A나라의 미사일을 요격하기 필요한 미사일의 수의 최솟값을 return하세요  <br>
[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/181188#)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>

1. A나라에서 발사한 미사일을 처음 발사한 <span style="background-color: yellow;">s</span>로 정렬한다.
2. 정렬 후 end_value에 미사일의 마지막 좌표인 <span style="background-color: yellow;">e</span>를 저장한다. 
3. 이 때 정렬한 미사일 좌표를 돌면서 end_value에 저장한 값이 새로운 미사일의 시작좌표보다 크면 end_value를 기존 end_value와 새로운 미사일의 끝좌표를 비교하여 작은 값을 넣어준다
4. 만약 end_value에 저장한 값이 새로운 미사일의 시작좌표보다 작으면 cnt를 추가해주고 end_value를 새로운 미사일의 끝좌표로 바꿔준다

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(targets):
    # targets값 정렬
    sorted_target = sorted(targets)
    
    end_value = 0
    cnt = 0
    for target in sorted_target:
        # 처음 시작
        if end_value == 0:
            # 끝 범위 추가
            end_value = target[1]
            cnt += 1 
            continue
        
        # end_value가 새로운 미사일의 끝좌표보다 클 경우 end_value값을 새로운 미사일의 끝좌표와 비교해 더 작은 값을 넣어준다.
        if end_value > target[0]:
            end_value = min(end_value, target[1])
            continue
        # end_value값을 새로운 미사일의 끝좌표로 교체해주고 미사일 값을 늘려준다
        else:
            end_value = target[1]
            cnt += 1 
                 
    return cnt
~~~









