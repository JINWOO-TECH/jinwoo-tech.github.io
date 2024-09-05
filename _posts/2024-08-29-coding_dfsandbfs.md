---
layout: post
title: 4. 코딩테스트 기초(python) - DFS/BFS
description: >
  코테 기초 DFS/BFS
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> DFS/BFS? </span>
</h2>

DFS(Depth-First Search)와 BFS(Breadth-First Search)는 그래프와 트리 자료 구조를 탐색하거나 순회하는 데 사용되는 두 가지 주요 알고리즘입니다. 각 알고리즘은 탐색 순서와 전략에서 차이가 있습니다.

<h3>DFS(Depth-First Search, 깊이 우선 탐색)</h3>
* 기본 개념: DFS는 가능한 한 깊이 들어가면서 노드를 탐색하는 알고리즘입니다. 말 그대로 한 방향으로 끝까지 탐색한 후, 더 이상 갈 곳이 없으면 뒤로 돌아와서 다른 방향을 탐색합니다.
* 탐색 방법:
  * 스택(Stack) 자료 구조를 사용하거나, 재귀(Recursive) 호출을 통해 구현할 수 있습니다.
  * 시작 노드에서 출발하여 다음으로 깊이 갈 수 있는 노드로 계속 이동합니다.
  * 더 이상 깊이 갈 수 없을 때, 이전 노드로 돌아가고 다른 경로를 탐색합니다.
* 특징:
  * 경로를 찾는 문제에서 유용할 수 있으며, 특히 해가 깊숙이 존재할 가능성이 있을 때 효과적입니다.
  * 그래프의 크기가 크고 깊이가 매우 깊다면, 재귀를 사용한 DFS는 스택 오버플로우(Stack Overflow)를 유발할 수 있습니다.

<h3> BFS(Breadth-First Search, 너비 우선 탐색)</h3>
* 기본 개념: BFS는 한 레벨의 모든 노드를 먼저 탐색한 후, 그 다음 레벨로 이동하는 방식의 알고리즘입니다. 먼저 근처에 있는 모든 노드를 탐색한 후, 한 단계씩 더 멀리 있는 노드를 탐색합니다.
* 탐색 방법:
    * 큐(Queue) 자료 구조를 사용하여 구현됩니다.
    * 시작 노드에서 출발하여 인접한 모든 노드를 탐색하고, 그 다음에는 그 인접 노드의 인접 노드들을 탐색합니다.
* 특징:
    * 최단 경로를 찾는 문제에서 유용하며, 특히 그래프가 넓고 깊이가 얕을 때 효과적입니다.
    * BFS는 최단 경로를 보장하지만, 메모리 사용량이 많을 수 있습니다.

<h3>DFS와 BFS의 차이점</h3>
* 탐색 순서: DFS는 깊이 우선으로, BFS는 너비 우선으로 탐색합니다.
* 자료 구조: DFS는 주로 스택(혹은 재귀), BFS는 큐를 사용합니다.
* 응용 분야: DFS는 경로 탐색, 미로 찾기 등에 유리하며, BFS는 최단 경로 탐색에 유리합니다.

![Xixia](/assets/images/programming/20240827dfs1.png)

DFS 처리 순서 : 1 -> 2 -> 7 -> 6 -> 8 -> 3 -> 4 -> 5
~~~python
def solution(graph,v,visited):
    # 방문처리
    visited[v] = True
    print(v, end=' ')

    for i in graph[v]:
        if not visited[i]:
            solution(graph,i,visited)

graph = [
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]

visited = [False] * 9

solution(graph,1,visited)
~~~

BFS 처리 순서 : 1 -> 2 -> 3 -> 8 -> 7 -> 4 -> 5 -> 6
~~~python
from collections import deque
def solution(graph,start,visited):
    queue = deque([start])

    visited[start] = True

    while queue:
        v = queue.popleft()
        print(v, end= ' ')
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True

graph = [
    [],
    [2,3,8],
    [1,7],
    [1,4,5],
    [3,5],
    [3,4],
    [7],
    [2,6,8],
    [1,7]
]

visited = [False] * 9

solution(graph,1,visited)
~~~


<h2>
    <span class = "jjw_h2_style"> DFS/BFS  예시 </span>
</h2>

문제 : 음료수 얼려먹기 문제 (DFS)
> N * M 크키기의 얼음틀 존재, 구멍이 뚤려 있는 부분은 0, 칸막이가 존재하는 부분은 1로 표시<br>
> 구멍이 뚫려 있는 부분은 상하좌우로 붙어 있는 경우 서로 연결되어 있는 것으로 간주   <br>
> 얼음 틀 모양이 주어졌을 때 생성되는 총 아이스크림의 개수를 구하는 프로그램 작성 

~~~python
def solution(x, y):
    if x <= -1 or x >= N or y <= -1 or y >= M :
        return False

    if ice[x][y] == 0:
        ice[x][y] = 1

        solution(x - 1, y)
        solution(x, y - 1)
        solution(x + 1, y)
        solution(x, y + 1 )
        return True
    return False

N = 4
M = 5
ice = [[0,0,1,1,0],[0,0,0,1,1],[1,1,1,1,1],[0,0,0,0,0]]


result = 0
for x in range(N):
    for y in range(M):
        if solution(x,y) == True:
            result += 1

print(result)
~~~




문제 : 미로 탈출 (BFS)
> N * M 크키기의 직사각형 형태의 미로가 존재<br>
> 갈 수 있는 곳은 1, 갈 수 없는 곳은 0일때   <br>
> 탈출하기 위해 움직여야 하는 최소 칸의 개수 

~~~python
from collections import deque


def solution(x, y, rows, columns):
    queue = deque()
    queue.append((x, y))

    while queue:
        x, y = queue.popleft()
        # 4방향 확인
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            #  공간 확인
            if nx < 0 or nx >= rows or ny < 0 or ny >= columns:
                continue

            # 벽인 경우
            if maps[nx][ny] == 0:
                continue



            if maps[nx][ny] == 1:
                maps[nx][ny] = maps[x][y] + 1
                queue.append((nx, ny))
                print('---------------------')
                for map in maps:
                    print(map)

    return maps[rows - 1][columns - 1]


maps = [[1, 0, 1, 0, 1, 0], [1, 1, 1, 1, 1, 1], [0, 0, 0, 0, 0, 1], [1, 1, 1, 1, 1, 1], [1, 1, 1, 1, 1, 1]]

rows = len(maps)
columns = len(maps[0])

# 이동할 방향
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]


print(solution(0, 0, rows, columns))


~~~


<hr>
<div>
    {% assign mydata = site.posts | where: "categories", "programming" %}
    {% assign has_content = "False" %}
      <h2>
    <span class = "jjw_h2_style"> 프로그래머스 예시 </span> <br><br>
      </h2>
      {% for data in mydata %}
         {% if data.tags[0] contains "코딩테스트_문제_python_bfs" %}
            {% assign has_content = "True" %}
            <a href="{{ site.baseurl}}{{ data.url }}">{{ data.title }}</a> <br>
        {% endif %}
    {% endfor %}
</div>
<div>

{% if has_content == "False" %}
  <b>To be continue....</b> 
{% endif %}
</div>

















