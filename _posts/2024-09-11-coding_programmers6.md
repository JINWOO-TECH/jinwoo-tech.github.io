---
layout: post
title: (프로그래머스 / python3) - 연속된 부분 수열의 합
description: >
  (프로그래머스 / python3) - 연속된 부분 수열의 합
categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>

비내림차순으로 정렬된 수열이 <span class="jjw_line">sequence</span> 주어질 때 합이 <span class="jjw_line">k</span>인 부분 수열 중, 길이가 가장 짧은 수열의 index를 return 하세요<br>
단, 길이가 같을 경우 제일 먼저 오는 수열을 반환하세요


[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/178870)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
투포인터 알고리즘 사용

1. 만약 total_value 값이 k 보다 작으면 end 값 이동 
2. 만약 total_value 값이 k 보다 크면 start 값 이동
3. 만약 total_value 값이 k 같으면 start 값 이동 + 저장



<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
import math
# 투포인터
# 만약 total_value 값이 k 보다 작으면 end 값 이동
# 만약 total_value 값이 k 보다 크면 start 값 이동
# 만약 total_value 값이 k 같으면 start 값 이동 + 저장
def solution(sequence, k):
    start = 0
    end = 0
    answer_dic = [''] * 2
    answer_between_len = math.inf
    
    total_value = 0
    
    seq_len = len(sequence)
    
    while True:
        if end > seq_len:
            break
            
        if start == 0 and end ==0 :
            total_value = sequence[0]
        
        # 만약 total_value 값이 k 보다 작으면 end 값 이동
        if total_value < k:
            end += 1
            if end < seq_len:
                total_value += sequence[end]
        # 만약 total_value 값이 k 보다 크면 start 값 이동
        elif total_value > k:
            total_value -= sequence[start]
            start += 1
        # 만약 total_value 값이 k 같으면 start 값 이동 + 저장
        else:
            
            if end - start < answer_between_len:
                answer_between_len = end - start
                answer_dic[0] = start
                answer_dic[1] = end
            
            total_value -= sequence[start]
            start +=1
        
    return answer_dic
~~~









