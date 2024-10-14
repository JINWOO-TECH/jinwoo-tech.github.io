---
layout: post
title: (프로그래머스 / python3) - 무인도 여행

description: >
  (프로그래머스 / python3) - 무인도 여행

categories: programming
tags: 코딩테스트_문제_python_bfs
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
메리는 여름을 맞아 무인도로 여행을 가기 위해 지도를 보고 있습니다. 지도에는 바다와 무인도들에 대한 정보가 표시돼 있습니다. 지도는 1 x 1크기의 사각형들로 이루어진 직사각형 격자 형태이며, 격자의 각 칸에는 'X' 또는 1에서 9 사이의 자연수가 적혀있습니다. 지도의 'X'는 바다를 나타내며, 숫자는 무인도를 나타냅니다. 이때, 상, 하, 좌, 우로 연결되는 땅들은 하나의 무인도를 이룹니다. 지도의 각 칸에 적힌 숫자는 식량을 나타내는데, 상, 하, 좌, 우로 연결되는 칸에 적힌 숫자를 모두 합한 값은 해당 무인도에서 최대 며칠동안 머물 수 있는지를 나타냅니다. 어떤 섬으로 놀러 갈지 못 정한 메리는 우선 각 섬에서 최대 며칠씩 머물 수 있는지 알아본 후 놀러갈 섬을 결정하려 합니다.

지도를 나타내는 문자열 배열 `maps`가 매개변수로 주어질 때, 각 섬에서 최대 며칠씩 머무를 수 있는지 배열에 오름차순으로 담아 return 하는 solution 함수를 완성해주세요. 만약 지낼 수 있는 무인도가 없다면 -1을 배열에 담아 return 해주세요.


[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/154540)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
bfs 사용

1. 주어진 maps에서 각 좌표의 값이 숫자인 곳을 방문하면서 연결된 영역의 값을 합산합니다.
2. 각 좌표를 방문할 때, 상하좌우로 이동 가능한 좌표를 확인하고, 이미 방문하지 않은 곳 중에 'X'가 아닌 좌표로만 이동합니다.
3. visited 리스트는 방문 여부를 기록하여, 한 번 방문한 좌표는 다시 방문하지 않도록 처리합니다.
4. BFS 탐색을 위해 deque(덱)를 사용하여, 탐색할 좌표를 큐에 저장하고 먼저 들어온 좌표부터 순차적으로 꺼내면서 상하좌우로 이동합니다.
5. 연결된 좌표의 값이 모두 더해지면 그 값을 결과 리스트에 저장하고, 맵 전체를 탐색한 후, 결과 리스트를 오름차순으로 정렬하여 반환합니다.
6. 만약 연결된 숫자가 없는 경우, -1을 반환합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque

# BFS 함수: 시작 좌표 (x, y)에서 연결된 모든 좌표를 방문하면서 숫자를 더하는 함수
def bfs(maps, x, y, visited):
    
    rows = len(maps)  # 맵의 행 개수
    cols = len(maps[0])  # 맵의 열 개수
    
    # 상하좌우 방향을 나타내는 리스트 (dx는 행 변화, dy는 열 변화)
    dx = [1, -1, 0, 0]
    dy = [0, 0, 1, -1]
    
    # BFS를 위한 큐 생성 및 시작점 추가 (x, y)
    q = deque([(x, y)])
    visited[x][y] = True  # 시작점 방문 처리
    
    cnt = int(maps[x][y])  # 시작점의 값 (숫자) 저장
    
    # 큐가 빌 때까지 반복 (BFS 탐색)
    while q:
        x, y = q.popleft()  # 큐에서 좌표를 꺼내기
        
        # 상하좌우로 이동할 수 있는 모든 좌표 확인
        for i in range(4):
            nx = x + dx[i]  # 이동한 후의 행 좌표
            ny = y + dy[i]  # 이동한 후의 열 좌표
            
            # 새로운 좌표가 맵 안에 있고, 방문하지 않았으며 'X'가 아닌 경우
            if 0 <= nx < rows and 0 <= ny < cols and visited[nx][ny] == False and maps[nx][ny] != 'X':
                visited[nx][ny] = True  # 방문 처리
                cnt += int(maps[nx][ny])  # 해당 좌표의 값을 더함
                q.append((nx, ny))  # 큐에 좌표 추가하여 다음 탐색 진행
    
    return cnt  # 연결된 모든 좌표의 숫자의 합 반환


# 전체 맵을 순회하며 BFS로 연결된 영역의 숫자를 구하는 함수
def solution(maps):
    answer = []  # 결과 값을 저장할 리스트
    visited = [[False] * len(maps[0]) for _ in range(len(maps))]  # 방문 여부를 확인할 2차원 리스트 초기화
    
    # 모든 좌표를 순회
    for x in range(len(maps)):
        for y in range(len(maps[0])):
            # 아직 방문하지 않았고, 해당 좌표가 'X'가 아닌 경우 BFS 실행
            if visited[x][y] == False and maps[x][y] != 'X':
                answer.append(bfs(maps, x, y, visited))  # BFS로 얻은 값을 결과 리스트에 추가
    
    # 결과 리스트가 비어 있지 않으면 오름차순 정렬 후 반환
    if answer:
        return sorted(answer)
    else:
        return [-1]  # 맵에 숫자가 없거나 연결된 곳이 없는 경우 -1 반환

~~~









