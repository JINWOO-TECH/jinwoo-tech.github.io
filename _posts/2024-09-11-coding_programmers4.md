---
layout: post
title: (프로그래머스 / python3) - [PCCE 기출문제] 9번 / 이웃한 칸
description: >
  (프로그래머스 / python3) - [PCCE 기출문제] 9번 / 이웃한 칸
categories: programming
tags: 코딩테스트_문제_python_기타
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>


2차원 격자 보드판 <span class="jjw_line">board</span>과 고른 칸 <span class="jjw_line">h</span>,<span class="jjw_line">w</span>가 주어질 때 <br>
<span class="jjw_line">board[h][w]</span>와 상,하,좌,우로 인접한 칸들 중에 같은 색으로 칠해져 있는 칸의 개수를 return 하세요 <br>

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/250125)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
1. 상하좌우의 좌표 같이 0보다 크고 board 밖으로 나가지 않게 조건을 설정한다
2. 색이 같으면 cnt + 1을 해준다

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
def solution(board, h, w):
    target_color = board[h][w]
    
    dx = [1,-1,0,0]
    dy = [0,0,1,-1]
    
    cnt = 0
    for i in range(4):
        nx = h + dx[i]
        ny = w + dy[i]
        if 0 <= nx < len(board) and 0 <= ny < len(board[0]):
            if board[nx][ny] == target_color:
                cnt +=1
    
    return cnt
~~~









