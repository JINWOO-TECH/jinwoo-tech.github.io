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

* 벨만-포드 알고리즘 (Bellman-Ford Algorithm)
벨만-포드 알고리즘은 음수 가중치가 포함된 그래프에서도 동작하며, 특정 노드에서 다른 모든 노드까지의 최단 경로를 찾습니다. 음수 사이클이 존재하는 경우 이를 탐지할 수 있습니다.

  * 작동 원리:

    * 초기에는 모든 노드까지의 거리를 무한대로 설정하고, 시작 노드는 0으로 설정합니다.
    * 그래프의 모든 간선에 대해 (V-1)번 반복하여 최단 거리를 갱신합니다.
    * (V-1)번 반복 후에도 거리가 갱신된다면, 음수 사이클이 존재하는 것으로 간주합니다.


* 플로이드-워셜 알고리즘 (Floyd-Warshall Algorithm)
플로이드-워셜 알고리즘은 모든 노드 간의 최단 경로를 찾는 알고리즘으로, 그래프에 음수 가중치가 있어도 동작합니다. 그러나 음수 사이클은 처리하지 못합니다.

* 작동 원리:
  * 모든 노드 간의 초기 거리를 무한대로 설정하되, 자기 자신으로의 거리는 0으로 설정합니다.
  * 각 노드를 중간 노드로 고려하면서, 경로를 갱신합니다.


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

















