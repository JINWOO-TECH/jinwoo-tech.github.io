---
layout: post
title: 8. 코딩테스트 기초(python) - 최단 경로 알고리즘
description: >
  코테 기초 최단 경로 알고리즘
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> 최단 경로 알고리즘 </span>
</h2>
그래프에서 두 노드 간의 최단 경로를 찾는 알고리즘입니다. 이러한 알고리즘은 네트워크 라우팅, 지도 및 네비게이션 시스템, 그리고 다양한 최적화 문제에서 중요한 역할을 합니다.

여기서는 가장 널리 사용되는 세 가지 최단 경로 알고리즘은
다익스트라 알고리즘(Dijkstra's Algorithm), 벨만-포드 알고리즘(Bellman-Ford Algorithm), 그리고 플로이드-워셜 알고리즘(Floyd-Warshall Algorithm)이 있습니다.

* 다익스트라 알고리즘 (Dijkstra's Algorithm)
다익스트라 알고리즘은 가중치가 있는 그래프에서 특정 노드로부터 다른 모든 노드까지의 최단 경로를 찾는 알고리즘입니다. 이 알고리즘은 음수 가중치가 없는 그래프에서만 동작합니다.

    * 작동 원리:
      * 시작 노드를 설정하고, 해당 노드에서 다른 노드까지의 거리를 초기화합니다.
      * 방문하지 않은 노드 중 가장 짧은 거리를 가진 노드를 선택하여, 그 노드를 거쳐 다른 노드로 가는 경로를 확인하며, 필요한 경우 더 짧은 경로로 업데이트합니다.
      * 이 과정을 반복하여 모든 노드를 방문합니다.


~~~python
INF = int(1e9)

# 노드 수
n = 10

visited = [False] * (n + 1)

distance = [INF] * (n + 1)

graph = [
    [],  # 0번 노드 (사용하지 않음)
    [(2, 3), (3, 5), (4, 9)],  # 1번 노드와 연결된 노드들 (노드 2로 가는 비용 3, 노드 3으로 가는 비용 5, 노드 4로 가는 비용 9)
    [(1, 3), (4, 2)],  # 2번 노드와 연결된 노드들
    [(1, 5), (4, 1), (5, 7)],  # 3번 노드와 연결된 노드들
    [(1, 9), (2, 2), (3, 1), (5, 3)],  # 4번 노드와 연결된 노드들
    [(3, 7), (4, 3), (6, 4)],  # 5번 노드와 연결된 노드들
    [(5, 4), (7, 6)],  # 6번 노드와 연결된 노드들
    [(6, 6), (8, 8)],  # 7번 노드와 연결된 노드들
    [(7, 8), (9, 5)],  # 8번 노드와 연결된 노드들
    [(8, 5), (10, 2)],  # 9번 노드와 연결된 노드들
    [(9, 2)]  # 10번 노드와 연결된 노드들
]

start = 1


def get_smallest_node():
    min_vaule = INF
    index = 0
    for i in range(1, n + 1):
        if distance[i] < min_vaule and not visited[i]:
            min_vaule = distance[i]
            index = i
    return index

def dijkstra(start):
    # 시작 노드 초기화
    distance[start] = 0
    visited[start] = True
    for j in graph[start]:
        distance[j[0]] = j[1]

    # 시작노드 제외 전체 n-1개 노드 반복
    for i in range(n - 1):
        # 현재 최단 거리가 가장 짧은 노드 방문처리
        now = get_smallest_node()
        visited[now] = True

        # 현재 노드와 연결된 다른 노드 확인
        for j in graph[now]:
            cost = distance[now] + j[1]
            if cost <= distance[j[0]]:
                distance[j[0]] = cost


dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n + 1):
    if distance[i] == INF:
        print('도달 할 수 없음')
    else:
        print(distance[i])
~~~


** 우선 순위 큐 (heap)**
우선순위가 가장 높은 데이터를 가장 먼저 삭제하는 자료구조


개선된 다익스트라 알고리즘
~~~python

import heapq

INF = int(1e9)

# 노드 수
n = 10

visited = [False] * (n + 1)

distance = [INF] * (n + 1)

graph = [
    [],  # 0번 노드 (사용하지 않음)
    [(2, 3), (3, 5), (4, 9)],  # 1번 노드와 연결된 노드들 (노드 2로 가는 비용 3, 노드 3으로 가는 비용 5, 노드 4로 가는 비용 9)
    [(1, 3), (4, 2)],  # 2번 노드와 연결된 노드들
    [(1, 5), (4, 1), (5, 7)],  # 3번 노드와 연결된 노드들
    [(1, 9), (2, 2), (3, 1), (5, 3)],  # 4번 노드와 연결된 노드들
    [(3, 7), (4, 3), (6, 4)],  # 5번 노드와 연결된 노드들
    [(5, 4), (7, 6)],  # 6번 노드와 연결된 노드들
    [(6, 6), (8, 8)],  # 7번 노드와 연결된 노드들
    [(7, 8), (9, 5)],  # 8번 노드와 연결된 노드들
    [(8, 5), (10, 2)],  # 9번 노드와 연결된 노드들
    [(9, 2)]  # 10번 노드와 연결된 노드들
]

start = 1


def dijkstra(start):
    q = []
    heapq.heappush(q, (0, start))
    distance[start] = 0

    while q:
        dist, now = heapq.heappop(q)

        if distance[now] < dist:
            continue

        for i in graph[now]:
            cost = dist + i[1]

            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q, (cost, i[0]))


dijkstra(start)

# 모든 노드로 가기 위한 최단 거리 출력
for i in range(1, n + 1):
    if distance[i] == INF:
        print('도달 할 수 없음')
    else:
        print(distance[i])

~~~


* 플로이드-워셜 알고리즘 (Floyd-Warshall Algorithm)
플로이드-워셜 알고리즘은 모든 노드 간의 최단 경로를 찾는 알고리즘으로, 그래프에 음수 가중치가 있어도 동작합니다. 그러나 음수 사이클은 처리하지 못합니다.

  * 작동 원리:
    * 모든 노드 간의 초기 거리를 무한대로 설정하되, 자기 자신으로의 거리는 0으로 설정합니다.
    * 각 노드를 중간 노드로 고려하면서, 경로를 갱신합니다.

  




* 벨만-포드 알고리즘 (Bellman-Ford Algorithm)
벨만-포드 알고리즘은 음수 가중치가 포함된 그래프에서도 동작하며, 특정 노드에서 다른 모든 노드까지의 최단 경로를 찾습니다. 음수 사이클이 존재하는 경우 이를 탐지할 수 있습니다.

  * 작동 원리:

    * 초기에는 모든 노드까지의 거리를 무한대로 설정하고, 시작 노드는 0으로 설정합니다.
    * 그래프의 모든 간선에 대해 (V-1)번 반복하여 최단 거리를 갱신합니다.
    * (V-1)번 반복 후에도 거리가 갱신된다면, 음수 사이클이 존재하는 것으로 간주합니다.




<hr>
<div>
    {% assign mydata = site.posts | where: "categories", "programming" %}
    {% assign has_content = "False" %}
      <h2>
    <span class = "jjw_h2_style"> 프로그래머스 예시 </span> <br><br>
      </h2>
      {% for data in mydata %}
         {% if data.tags[0] contains "greedy" %}
            <a href="{{ site.baseurl}}{{ data.url }}">{{ data.title }}</a> <br>
         {% else %}
            {% assign has_content = "True" %}
      {% endif %}
    {% endfor %}
</div>
<div>
{% if has_content == "True" %}
  <b>To be continue....</b> 
{% endif %}
</div>

















