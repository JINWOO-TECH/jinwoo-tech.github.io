---
layout: post
title: 3. 코딩테스트 기초(python) - greedy
description: >
  코테 기초 greedy
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> 그리디 알고리즘 이란? </span>
</h2>
<br>
문제를 해결하는 과정에서 매 순간 가장 최적이라고 판단되는 선택을 하는 방법입니다. 그리디 알고리즘은 단기적으로 
최적의 선택을 계속해서 하는 방식이기 때문에, 이 방법으로 얻은 해답이 항상 최적의 해답이라는 보장은 없지만,
코딩 테스트에서의 대부분의 그리디 문제는 **그리디 알고리즘으로 얻은 해가 최적의 해가 되는 상황에서, 이를 추론**할 수 있어야 풀리도록 출제됩니다.

<br><br>

 
<h2>
    <span class = "jjw_h2_style"> 그리디 알고리즘  예시 </span>
</h2>
 
문제 : 거스름돈 문제
> 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리 동전이 무한히 존재 <br>
> 거슬러 주어야 할 돈이 N원일 때, 거슬러 주어야 할 동전의 최소 개수를 구하는문제  <br>
> 단, 거슬러 줘야 할 돈 N은 항상 10의 배수 

<br>

1. 아이디어 생각하기 <br>
가장 큰 화폐 단위부터 돈을 거술러 주면 됩니다.
N원을 거슬러 줘야 할 때, 가장 먼저 500원으로 것룰러 줄 수 있을 만큼 거슬러 줍니다 -> 큰 단위가 항상 작은 단위의 배수이므로


~~~python
def solution(money):
    # 거스름돈 list
    change_list = [500, 100, 50, 10]
    
    # 내림차순 정렬
    sorted_chang_list = sorted(change_list, reverse=True)
    
    cnt = 0
    for change_money in sorted_chang_list:
        cnt += (money // change_money)
        money = (money % change_money)
    return cnt

# 테스트 동전
money = 1260

result = solution(money)
print(result)
~~~


















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

















