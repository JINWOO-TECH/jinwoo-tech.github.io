---
layout: post
title: (프로그래머스 / python3) - 귤 고르기
description: >
  (프로그래머스 / python3) - 
categories: programming
tags: 코딩테스트_문제_python_그리디
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
경화는 과수원에서 귤을 수확했습니다. 경화는 수확한 귤 중 'k'개를 골라 상자 하나에 담아 판매하려고 합니다. 그런데 수확한 귤의 크기가 일정하지 않아 보기에 좋지 않다고 생각한 경화는 귤을 크기별로 분류했을 때 서로 다른 종류의 수를 최소화하고 싶습니다.

예를 들어, 경화가 수확한 귤 8개의 크기가 [1, 3, 2, 5, 4, 5, 2, 3] 이라고 합시다. 경화가 귤 6개를 판매하고 싶다면, 크기가 1, 4인 귤을 제외한 여섯 개의 귤을 상자에 담으면, 귤의 크기의 종류가 2, 3, 5로 총 3가지가 되며 이때가 서로 다른 종류가 최소일 때입니다.

경화가 한 상자에 담으려는 귤의 개수 k와 귤의 크기를 담은 배열 tangerine이 매개변수로 주어집니다. 경화가 귤 k개를 고를 때 크기가 서로 다른 종류의 수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/138476)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
그리디 알고리즘 사용

* 귤을 크기별로 분류했을 때, 개수가 많은 크기부터 우선 선택하면 서로 다른 종류의 수를 최소화할 수 있습니다.
* 주어진 `tangerine` 배열에서 각 크기별로 귤의 개수를 계산한 후, 개수가 많은 순서대로 k개를 채워가며 사용된 종류의 수를 계산합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import Counter

def solution(k, tangerine):
    # 귤 크기별 개수 계산
    count_tangerine = Counter(tangerine)
    
    # 개수 기준 내림차순 정렬
    sorted_tangerine = sorted(count_tangerine.values(), reverse=True)
    
    # 필요한 종류의 수 계산
    total = 0  # 현재까지 선택한 귤 개수
    answer = 0 # 사용한 귤 크기 종류 수
    
    for count in sorted_tangerine:
        total += count
        answer += 1
        if total >= k:  # k개 이상 채우면 종료
            break
    
    return answer

~~~









