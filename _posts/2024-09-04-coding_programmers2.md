---
layout: post
title: (프로그래머스 / python3) - [PCCP 기출문제] 2번 / 석유 시추
description: >
  (프로그래머스 / python3) - [PCCP 기출문제] 2번 / 석유 시추
categories: programming
tags: 코딩테스트_문제_python_bfs
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>


세로길이가 n, 가로 길이가 m인 격자 모양의 땅 속에서 석유가 발견 되었습니다. <br>
석유는 여러 덩어리로 나누어 묻혀있고, 시추관을 수직으로 1개만 뚫을 수 있습니다. <br>
만약 시추관이 석유 덩어리의 일부를 지나면 해당 덩어리에 속한 모든 석유를 뽑을 수 있습니다. <br>
시추관이 뽑을 수 있는 석유량은 시추관이 지나는 석유 덩어리들의 크기를 모두 합한 값입니다. <br>
시추관 하나를 설치해 뽑을 수 있는 가장 많은 석유량을 return하세요 <br><br>
[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/250136)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>

1. 방문 정보를 담을 수 있는 이차원 배열 생성(visited)
2. 세로, 가로 길이 만큼 이중 for 문을 돌리며 석유가 있는(1)곳과 방문하지 않은 곳일 때, bfs 실행
3. bfs에는 석유가 발견되면 상하좌우를 검색해 석유가 있을 경우 cnt를 늘려주고 oil_discovered에 col 기록
4. oil_discovered에 해당하는 col들을 연결된 석유의 최종 길이 cnt를 더해주고 oil배열에 추가 해준다
5. oil배열의 max값 출력

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque


# bfs
def solution(land):
    rows, cols = len(land), len(land[0])
    
    # 방문 정보
    visited = [[False] * cols for _ in range(rows)]
    
    # 방향
    dx = [1,-1,0,0]
    dy = [0,0,1,-1]
    
    # 오일 총량 
    oil = [0] * cols
    
    def bfs(row,col):
        queue = deque()
        queue.append((row,col))
        
        # 방문처리
        visited[row][col] = True
        
        cnt = 1
        
        # 오일 발견 col - 중복 방지를 위해 set 사용
        oil_discovered = {col}
        
        while queue:
            now_row, now_col = queue.popleft()
            
            # 상하좌우 검색
            for i in range(4):
                next_row = now_row + dx[i]
                next_col = now_col + dy[i]
                
                if  0 <= next_row < rows and 0 <= next_col < cols and not visited[next_row][next_col]:
                    if land[next_row][next_col] == 1:
                        queue.append((next_row,next_col))
                        visited[next_row][next_col] = True
                        cnt += 1
                        oil_discovered.add(next_col)
        
        oil_discovered_list = list(oil_discovered)
        
        # 해당 열에 최종 cnt값 추가 
        for oil_cols in oil_discovered_list:
            oil[oil_cols] += cnt
    
    # (rows,cols)
    for i in range(rows):
        for j in range(cols):
            if land[i][j] == 1 and not visited[i][j]:
                bfs(i,j)
            
    return max(oil)

~~~









