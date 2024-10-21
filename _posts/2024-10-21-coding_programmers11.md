---
layout: post
title: (프로그래머스 / python3) - 정수 삼각형

description: >
  (프로그래머스 / python3) - 정수 삼각형

categories: programming
tags: 코딩테스트_문제_python_dp
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
dp

1. 삼각형의 맨 아래 부분부터 시작하여 맨 위까지 거슬러 올라가며 최대 경로 합을 계산합니다.
2. 각 위치에서 아래로 갈 수 있는 두 가지 경로 중 더 큰 값을 선택하여 현재 위치에 더합니다.
   - 즉, `triangle[i][j]` 위치에서 아래의 `triangle[i+1][j]`와 `triangle[i+1][j+1]` 중 더 큰 값을 선택해 더합니다.
3. 맨 위까지 올라가면, 가장 첫 번째 값이 최대 경로 합이 됩니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(triangle):
    dp = triangle[:]
    
    # 아래에서 위로 올라가며 계산
    for i in range(len(dp) -2, -1, -1):
        for j in range(len(dp[i])):
            dp[i][j] += max(dp[i + 1][j], dp[i + 1][j + 1])
    
    return dp[0][0]
~~~









