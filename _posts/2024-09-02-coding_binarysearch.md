---
layout: post
title: 6. 코딩테스트 기초(python) - 이진 탐색
description: >
  코테 기초 이진 탐색
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> 이진 탐색 </span>
</h2>
이진 탐색(Binary Search)은 정렬된 배열에서 특정 값을 효율적으로 찾는 알고리즘입니다. 탐색 범위를 반씩 줄여가면서 원하는 값을 찾아내므로, 시간 복잡도가 매우 효율적입니다.

* 전제 조건
  * 이진 탐색을 사용하기 위해서는 데이터가 정렬되어 있어야 합니다. 오름차순 또는 내림차순으로 정렬된 상태여야만 올바르게 동작합니다.

* 탐색 과정:
  * 데이터의 중간값을 선택합니다.
  * 중간값과 찾고자 하는 값을 비교합니다.
  * 찾는 값이 중간값보다 크면, 중간값의 오른쪽 부분을 대상으로 탐색을 계속합니다.
  * 찾는 값이 중간값보다 작으면, 중간값의 왼쪽 부분을 대상으로 탐색을 계속합니다.
  * 위 과정을 찾는 값을 발견하거나 탐색 범위가 없어질 때까지 반복합니다.

* 특징:
  * 매 단계마다 탐색 범위가 절반으로 줄어들기 때문에 매우 빠르게 값을 찾을 수 있습니다.
  * 정렬된 큰 데이터셋에서 효율적으로 사용됩니다.

~~~python
array = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

target = 7

def binary_search(array, target, start, end):
    if start > end:
        return None

    mid = (start + end) // 2

    if array[mid] == target:
        return mid
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    else:
        return binary_search(array, target, mid + 1, end)

print(binary_search(array, target, 0, len(array) - 1))
~~~

<h2>
    <span class = "jjw_h2_style"> 이진 탐색 예시 </span>
</h2>

문제 : 떡볶이 떡 만들기
> 한 봉지안에 들어가 있는 떡뽁이 떡의 길이는 일정하지 않습니다. <br>
> 절단기에 높이 H,를 지정하면 높이가 H보다 긴 떡은 짤리고, 낮을 떡은 잘리지 않습니다.
> 적어도 M만큼의 짤린 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최대값을 구하는 프로그램을 작성하시오

~~~python
N = 4

# 가져갈 떡의 크기
M = 6

duck = [19, 15, 10, 17]

start = 0
end = max(duck)

result = 0
while(start <= end):
    total = 0
    mid = (start + end) // 2

    for x in duck:
        if x > mid:
            total += x - mid

    if total < M :
        end = mid - 1

    else:
        result = mid
        start = mid + 1

print(result)
~~~


문제 : 정렬된 배열에서 특정 수의 개수 구하기
> N개의 원소를 포함하고 있는 수열이 오름차순으로 정렬되어 있습니다.
> 이 때, 수얄에서 x가 등장하는 횟수를 계산하세요.

~~~python
from bisect import bisect_right, bisect_left

a_list = [1, 1, 2, 2, 2, 2, 3]
x = 2

first_index = bisect_left(a_list,x)

last_index = bisect_right(a_list,x)


print(last_index - first_index)
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

















