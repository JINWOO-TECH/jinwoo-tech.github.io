---
layout: post
title: (프로그래머스 / python3) - [PCCE 기출문제] 10번 / 데이터 분석
description: >
  (프로그래머스 / python3) - [PCCE 기출문제] 10번 / 데이터 분석
categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>

이차원 배열 <span class="jjw_line">data</span>에 [code,data,maximun,remain] 으로 구성되어 있는 데이터가 들어가 있습니다. <br>
어떤 정보를 기준으로 데이터를 뽑아낼지를 의미하는 문자열 <span class="jjw_line">ext</span>, <br>
뽑아낼 정보의 기준값을 나타내는 정수 <span class="jjw_line">val_ext</span>, <br>
정보를 정렬할 기준이 되는 문자열 <span class="jjw_line">sort_by</span>가 주어집니다.<br>
<span class="jjw_line">data</span>에서 <span class="jjw_line">ext</span> 값이
<span class="jjw_line">val_ext</span>보다 작은 데이터만 뽑은 후, sort_by에 해당하는 값을 기준으로 <span class="jjw_line">오름차순</span>으로 정렬하여 return 하세요 <br><br>
[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/250121)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>

1. data의 컬럼 명에 따라 index를 반환할 수 있는 column_dict을 생성
2. ext과 column_dict을 활용하여 val_ext와 비교할 데이터의 인덱스를 알아내고 비교
3. sort_by와 column_dict을 활용 오름차순할 데이터의 인덱스를 알아내고 오름차순 진행



<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(data, ext, val_ext, sort_by):
    column_dict = {'code' : 0, 'date' : 1, 'maximum' : 2, 'remain' : 3}
    
    
    con_data = []
    for row in data:
        con_index =  column_dict[ext]
        if row[con_index] < val_ext:
            con_data.append(row)
        
    sorted_data = sorted(con_data, key = lambda x : x[column_dict[sort_by]])
    
    return sorted_data
~~~









