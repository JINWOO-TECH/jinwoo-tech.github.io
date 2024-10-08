---
layout: post
title: (프로그래머스 / python3) - 미로탈출
description: >
  (프로그래머스 / python3) - 미로탈출
categories: programming
tags: 코딩테스트_문제_python_bfs
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
1 x 1 크기의 칸들로 이루어진 직사각형 격자 형태의 미로에서 탈출하려고 합니다. <br>
각 칸은 통로 또는 벽으로 구성되어 있으며, 벽으로 된 칸은 지나갈 수 없고 통로로 된 칸으로만 이동할 수 있습니다. <br>
<span class="jjw_line">출발 지점에서 먼저 레버가 있는 칸으로 이동하여 레버를 당긴 후 미로를 빠져나가는 문이 있는 칸으로 이동</span>하면 됩니다. <br>
미로에서 한 칸을 이동하는데 1초가 걸린다고 할 때, 최대한 빠르게 미로(maps)를 빠져나가는데 걸리는 시간을 반환하세요 <br>

maps[i]는 5개의 문자로 이루어져 있습니다.
> S : 시작 지점 <br>
> E : 출구 <br>
> L : 레버 <br>
> O : 통로 <br>
> X : 벽 

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/159993)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
bfs 사용

1. 시작 지점(S) <-> 레버(L) 와  레버(L) <-> 출구(E)의 각각의 거리를 bfs 알고리즘을 사용해서 구한다. 
2. 만약 둘 중 하나라도 길이 없을 경우 -1 return 

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque

# BFS 탐색 함수
def bfs(start, end, maps):
    rows = len(maps)
    cols = len(maps[0])
    
    visited = [[False] * cols for _ in range(rows)]
    que = deque()
    flag = False
    
    # 탐색 방향 (우, 좌, 하, 상)
    dy = [0, 0, 1, -1]
    dx = [1, -1, 0, 0]
    
    # 초기 값 설정
    for i in range(rows):
        for j in range(cols):
            if maps[i][j] == start:
                que.append((i, j, 0))  # (행, 열, 이동 거리)
                visited[i][j] = True
                flag = True
                break
        if flag:
            break
                
    if not flag: 
        return -1
    
    # BFS 탐색 시작
    while que:
        x, y, cost = que.popleft()
        
        if maps[x][y] == end:
            return cost
        
        for i in range(4):
            ny = y + dy[i]
            nx = x + dx[i]
            
            # 유효한 인덱스 범위 확인 및 방문 여부 확인
            if 0 <= nx < rows and 0 <= ny < cols and not visited[nx][ny] and maps[nx][ny] != 'X':
                que.append((nx, ny, cost + 1))
                visited[nx][ny] = True  # 방문 처리
                
    return -1

# 문제 해결 함수
def solution(maps):
    path1 = bfs('S', 'L', maps)  # S에서 L까지의 경로
    path2 = bfs('L', 'E', maps)  # L에서 E까지의 경로
    
    if path1 != -1 and path2 != -1:
        return path1 + path2
    
    return -1

~~~









