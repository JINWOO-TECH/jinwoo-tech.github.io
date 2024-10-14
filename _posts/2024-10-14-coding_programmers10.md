---
layout: post
title: (프로그래머스 / python3) - 시소 짝꿍

description: >
  (프로그래머스 / python3) - 시소 짝꿍

categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
어느 공원 놀이터에는 시소가 하나 설치되어 있습니다. 이 시소는 중심으로부터 2(m), 3(m), 4(m) 거리의 지점에 좌석이 하나씩 있습니다.
이 시소를 두 명이 마주 보고 탄다고 할 때, 시소가 평형인 상태에서 각각에 의해 시소에 걸리는 토크의 크기가 서로 상쇄되어 완전한 균형을 이룰 수 있다면 그 두 사람을 시소 짝꿍이라고 합니다. 즉, 탑승한 사람의 무게와 시소 축과 좌석 간의 거리의 곱이 양쪽 다 같다면 시소 짝꿍이라고 할 수 있습니다.
사람들의 몸무게 목록 `weights`이 주어질 때, 시소 짝꿍이 몇 쌍 존재하는지 구하여 return 하도록 solution 함수를 완성해주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/152996)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>

1. 주어진 weights 리스트에서 두 물체의 무게가 주어진 조건(1:1, 4:3, 4:2, 3:2 비율)을 만족하는 경우를 찾습니다.
2. 먼저 Counter를 사용하여 각 무게의 발생 빈도를 기록한 후, 동일한 무게 쌍에 대해 조합을 계산하여 답을 구합니다. 
3. 동일한 무게에 대해서는 조합을 통해 가능한 쌍의 수를 계산하고, 이 값은 nC2 = count * (count - 1) // 2로 계산합니다. 
4. 그 후 서로 다른 두 무게에 대해 비율을 만족하는지 확인하고, 만약 비율이 맞다면 두 무게의 발생 빈도수를 곱하여 가능한 쌍의 수를 더합니다. 
5. 주어진 무게들의 비율 조건(1:1, 4:3, 4:2, 3:2)을 모두 확인한 후, 조건을 만족하는 경우의 수를 합산하여 최종 결과를 반환합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import Counter

def solution(weights):
    answer = 0
    weight_count = Counter(weights)
    
    # 가능한 비율: 1:1, 4:3, 4:2, 3:2
    ratios = [(1, 1), (4, 3), (4, 2), (3, 2)]
    
    # 같은 무게에 대해 먼저 처리
    for weight, count in weight_count.items():
        if count > 1:
            answer += count * (count - 1) // 2  # nC2 (조합 계산)
    
    # 다른 무게 비율 처리
    unique_weights = sorted(weight_count.keys())
    
    for i in range(len(unique_weights)):
        for j in range(i + 1, len(unique_weights)):
            for r1, r2 in ratios:
                if unique_weights[i] * r1 == unique_weights[j] * r2:
                    # 두 무게의 발생 빈도수를 곱해서 가능한 쌍의 수를 더하는 방식
                    answer += weight_count[unique_weights[i]] * weight_count[unique_weights[j]]
                    
                    break
    
    return answer


~~~









