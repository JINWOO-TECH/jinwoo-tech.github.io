---
layout: post
title: (프로그래머스 / python3) - 가장 먼 노드

description: >
  (프로그래머스 / python3) - 가장 먼 노드

categories: programming
tags: 코딩테스트_문제_python_우선순위큐
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.
[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/49189)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
다익스트라 알고리즘 사용

1. 초기화

   * 주어진 노드의 수 n을 사용해 그래프와 거리 배열을 초기화합니다.
   * 무한대 값 INF를 이용해 초기 거리를 설정합니다. (예: INF = int(1e9))
   * distance 배열은 각 노드까지의 최소 거리를 저장하며, 초기 거리를 무한대로 설정합니다.
   * graph 리스트에 각 도로를 양방향으로 저장하여 인접 리스트를 구성합니다. 각 도로 정보는 (연결된 노드, 소요 거리) 형태로 저장됩니다.

2. 다익스트라 알고리즘 함수 정의

   * dijkstra() 함수를 정의하여 최단 거리를 계산합니다.
   * 우선순위 큐 heapq를 사용하여 효율적인 거리 계산을 수행합니다.
   * 시작 지점(1번 노드)을 큐에 추가하고, 시작 마을의 거리를 0으로 설정합니다.

3. 우선순위 큐를 이용한 최단 거리 갱신

   * 큐에서 현재 노드와 현재 위치를 꺼내고, 만약 저장된 거리보다 크다면 생략하여 중복 연산을 방지합니다.
   * 현재 노드에 연결된 다른 노드들을 탐색하며, 새로운 거리 cost가 현재 저장된 거리보다 작다면 distance 배열을 갱신하고 큐에 새로운 경로를 추가합니다.

4. 가장 먼 노드의 개수 계산

   * 다익스트라 알고리즘이 완료되면 distance 배열에 모든 노드에 대한 최단 거리가 저장됩니다.
   * Counter를 이용해 거리에 따른 마을 수를 구하고 무한대 값 INF을 제외한 큰 값을 가져와 반환합니다.
   
<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
import heapq
from collections import Counter


def solution(n, edge):
    answer = 0
    INF = int(1e9)
    
    # 그래프 만들기 (가는노드, 비용)
    graph = [[] for _ in range(n + 1)]
    for e in edge: 
        s,e = e
        graph[s].append((e,1))
        graph[e].append((s,1))
    
    
    distance = [INF] * (n + 1)
    
    # 다익스트라 알고리즘
    def dijkstra(start):
        q = []
        
        distance[start] = 0
        heapq.heappush(q, (start, 0))
        
        while q:
            now, dist = heapq.heappop(q)
            
            # 최단 경로가 아닐 때
            if distance[now] < dist :
                continue
            
            for info in graph[now]:
                # info[0] = 향하는 곳, info[1] = 비용                 
                cost = dist + info[1]
                
                if cost < distance[info[0]]:
                    distance[info[0]] = cost
                    heapq.heappush(q, (info[0], cost))
                    
    dijkstra(1)

    counter = dict(Counter(distance))
    max_val = sorted(counter.keys(),reverse = True)[1]
    
    return counter[max_val]



~~~









