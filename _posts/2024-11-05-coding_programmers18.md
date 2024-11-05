---
layout: post
title: (프로그래머스 / python3) - 게임 맵 최단거리
description: >
  (프로그래머스 / python3) - 
categories: programming
tags: 코딩테스트_문제_python_bfs
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

검은색 부분은 벽으로 막혀있어 갈 수 없는 길이며, 흰색 부분은 갈 수 있는 길입니다. 캐릭터가 움직일 때는 동, 서, 남, 북 방향으로 한 칸씩 이동하며, 게임 맵을 벗어난 길은 갈 수 없습니다.
아래 예시는 캐릭터가 상대 팀 진영으로 가는 두 가지 방법을 나타내고 있습니다.

첫 번째 방법은 11개의 칸을 지나서 상대 팀 진영에 도착했습니다.

두 번째 방법은 15개의 칸을 지나서 상대팀 진영에 도착했습니다.

위 예시에서는 첫 번째 방법보다 더 빠르게 상대팀 진영에 도착하는 방법은 없으므로, 이 방법이 상대 팀 진영으로 가는 가장 빠른 방법입니다.

만약, 상대 팀이 자신의 팀 진영 주위에 벽을 세워두었다면 상대 팀 진영에 도착하지 못할 수도 있습니다. 예를 들어, 다음과 같은 경우에 당신의 캐릭터는 상대 팀 진영에 도착할 수 없습니다.

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/1844)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
bfs 알고리즘 사용

1. 초기화

   * 주어진 maps를 통해 맵의 행(rows)과 열(cols) 크기를 계산합니다.
   * 방문 여부를 체크하기 위한 visited 배열을 False로 초기화하여 생성합니다.
   * 이동 가능한 방향을 상하좌우로 설정하여, dx, dy 배열을 정의합니다.
   * 시작 위치 (0, 0)에서 탐색을 시작하기 위해 visited[0][0]을 True로 설정하고, deque를 사용해 큐에 시작 위치를 추가합니다.

2. BFS 함수 정의
   * BFS 탐색을 위한 큐를 생성하여 (0, 0)에서부터 시작합니다.
   * 큐에서 노드를 하나씩 꺼내며, 해당 위치에서 상하좌우 네 방향으로 이동 가능한지 확인합니다.

3. 방문하지 않은 노드로 이동하며 최단 거리 갱신

   * 다음 위치 (nx, ny)가 맵 범위 내에 있고, 방문하지 않은 길(maps[nx][ny] == 1)인 경우에만 이동합니다.
   * 이동할 수 있다면 visited[nx][ny]를 True로 갱신하고, 현재 위치의 거리 값에 1을 더해 maps[nx][ny]에 저장합니다.
   * 새로 방문한 위치 (nx, ny)를 큐에 추가하여 계속 탐색을 진행합니다.

4. 도착 지점의 거리 계산
   * BFS 탐색이 완료되면, maps[rows - 1][cols - 1]에 도착 지점까지의 최단 거리가 저장됩니다.
   * 만약 도착 지점의 값이 초기 값 1에서 변경되지 않았다면 도달할 수 없는 경로이므로 -1을 반환합니다.
   * 도착 지점까지의 최단 거리가 갱신된 경우 해당 값을 반환합니다.
   
<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
from collections import deque

def solution(maps):
    rows = len(maps)
    cols = len(maps[0])
    
    visited = [[False] * cols for _ in range(rows)]
    
    q = deque()
    
    dx = [0, 0, -1, 1]
    dy = [1, -1, 0, 0]
    
    # 초기 처리
    visited[0][0] = True
    q.append((0, 0))
    
    while q:
        x, y = q.popleft()
        
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            
            # 범위 안 일 때
            if 0 <= nx < rows and 0 <= ny < cols and maps[nx][ny] == 1 and not visited[nx][ny]:
                visited[nx][ny] = True
                q.append((nx, ny))
                maps[nx][ny] = maps[x][y] + 1
    print(maps)
    
    # 최종 경로가 갱신되었는지 확인
    return maps[rows - 1][cols - 1] if maps[rows - 1][cols - 1] != 1 else -1

~~~









