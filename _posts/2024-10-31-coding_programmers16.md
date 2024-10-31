---
layout: post
title: (프로그래머스 / python3) - 배달

description: >
  (프로그래머스 / python3) - 배달

categories: programming
tags: 코딩테스트_문제_python_우선순위큐
---

<h2>
    <span class = "jjw_h2_style"> 문제 </span>
</h2>
N개의 마을로 이루어진 나라가 있습니다. 이 나라의 각 마을에는 1부터 N까지의 번호가 각각 하나씩 부여되어 있습니다. 각 마을은 양방향으로 통행할 수 있는 도로로 연결되어 있는데, 서로 다른 마을 간에 이동할 때는 이 도로를 지나야 합니다. 도로를 지날 때 걸리는 시간은 도로별로 다릅니다. 현재 1번 마을에 있는 음식점에서 각 마을로 음식 배달을 하려고 합니다. 각 마을로부터 음식 주문을 받으려고 하는데, N개의 마을 중에서 K 시간 이하로 배달이 가능한 마을에서만 주문을 받으려고 합니다. 다음은 N = 5, K = 3인 경우의 예시입니다.

1번 마을에 있는 음식점은 [1, 2, 4, 5] 번 마을까지는 3 이하의 시간에 배달할 수 있습니다. 그러나 3번 마을까지는 3시간 이내로 배달할 수 있는 경로가 없으므로 3번 마을에서는 주문을 받지 않습니다. 따라서 1번 마을에 있는 음식점이 배달 주문을 받을 수 있는 마을은 4개가 됩니다.
마을의 개수 N, 각 마을을 연결하는 도로의 정보 road, 음식 배달이 가능한 시간 K가 매개변수로 주어질 때, 음식 주문을 받을 수 있는 마을의 개수를 return 하도록 solution 함수를 완성해주세요.

[문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/12978)

<br>

<h2>
    <span class = "jjw_h2_style"> 풀이 </span>
</h2>
<br>
다익스트라 알고리즘 사용

1. 초기화

   * 주어진 마을의 개수 N과 각 도로의 정보를 사용해 그래프와 거리 배열을 초기화합니다.
   * 무한대 값 INF를 이용해 초기 거리를 설정합니다. (예: INF = int(1e9))
   * distance 배열은 각 마을까지의 최소 거리를 저장하며, 모든 마을의 초기 거리를 무한대로 설정합니다.
   * graph 리스트에 각 도로를 양방향으로 저장하여 인접 리스트를 구성합니다. 각 도로 정보는 (연결된 마을, 소요 시간) 형태로 저장됩니다.

2. 다익스트라 알고리즘 함수 정의

   * dijkstra(시작 마을) 함수를 정의하여 최단 거리를 계산합니다.
   * 우선순위 큐 heapq를 사용하여 효율적인 거리 계산을 수행합니다.
   * 시작 지점(1번 마을)을 큐에 추가하고, 시작 마을의 거리를 0으로 설정합니다.

3. 우선순위 큐를 이용한 최단 거리 갱신

   * 큐에서 현재 거리와 현재 위치를 꺼내고, 만약 저장된 거리보다 크다면 생략하여 중복 연산을 방지합니다.
   * 현재 마을에 연결된 다른 마을들을 탐색하며, 새로운 거리 cost가 현재 저장된 거리보다 작다면 distance 배열을 갱신하고 큐에 새로운 경로를 추가합니다.

4. K 시간 이하로 배달 가능한 마을 개수 계산

   * 다익스트라 알고리즘이 완료되면 모든 마을에 대해 1번 마을에서의 최단 거리가 저장됩니다.
   * distance 배열에서 K 이하의 거리 값을 갖는 마을 개수를 세어 반환합니다.
   
<br><br>

<h2>
    <span class = "jjw_h2_style"> 코드 </span>
</h2>

~~~python
import heapq

def solution(N, road, K):
    # 초기화
    answer = 0
    
    # 무한대로 설정하여 초기화할 값
    INF = int(1e9)
    
    # 각 마을까지의 최소 시간을 저장할 리스트 (모든 거리 초기값을 무한대로 설정)
    distance = [INF] * (N + 1)
    
    # 그래프 초기화 (마을의 개수만큼 빈 리스트 생성)
    graph = [[] for _ in range(N + 1)]
    
    # 도로 정보 저장 (양방향 그래프)
    for r in road:
        start, end, cost = r
        graph[start].append((end, cost)) # start 마을에서 end 마을까지의 소요 시간 cost
        graph[end].append((start, cost)) # end 마을에서 start 마을까지의 소요 시간 cost
    
    # 다익스트라 알고리즘 정의
    def dijkstra(start):
        # 우선순위 큐 초기화 및 시작 노드 설정
        q = []
        heapq.heappush(q, (0, start)) # (거리, 마을) 형태로 큐에 저장
        distance[start] = 0 # 시작 마을의 거리를 0으로 설정
        
        # 큐가 빌 때까지 반복
        while q:
            # 큐에서 가장 짧은 거리를 가진 마을 정보 꺼내기
            dist, now = heapq.heappop(q)
            
            # 이미 처리된 마을이면 생략
            if distance[now] < dist:
                continue
            
            # 현재 마을과 인접한 마을들 확인
            for info in graph[now]:
                cost = dist + info[1] # 현재 마을까지의 거리 + 인접 마을까지의 거리
                # 인접 마을까지의 새로운 거리가 기존 거리보다 짧으면 갱신
                if cost < distance[info[0]]:
                    distance[info[0]] = cost
                    heapq.heappush(q, (cost, info[0])) # (새로운 거리, 인접 마을) 형태로 큐에 저장
    
    # 1번 마을에서 시작하는 다익스트라 알고리즘 실행
    dijkstra(1)
    
    # K 이하의 시간으로 배달 가능한 마을 개수를 계산
    answer = len([d for d in distance if d <= K])
    
    return answer


~~~









