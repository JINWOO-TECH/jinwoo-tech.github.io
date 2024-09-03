---
layout: post
title: 5. 코딩테스트 기초(python) - 정렬
description: >
  코테 기초 정렬 알고리즘
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> 정렬 알고리즘 </span>
</h2>
정렬 알고리즘은 데이터를 특정 순서(예: 오름차순, 내림차순)로 정렬하는 방법을 제공하는 알고리즘입니다.

* 선택 정렬 (Selection Sort)
  * 원리: 리스트에서 가장 작은(혹은 큰) 요소를 찾아 첫 번째 위치로 옮기고, 그 다음 작은 요소를 찾아 두 번째 위치로 옮기는 식으로 진행합니다.
  * 시간 복잡도: 최악, 평균, 최선 모두 O(n^2)
  * 특징: 단순하지만 비효율적입니다. 버블 정렬보다 비교 횟수는 적지만 여전히 느립니다.

~~~python
array = [7,5,9,0,3,1,6,2,4,8]

for i in range(len(array)):
    min_index = i
    for j in range(i + 1, len(array)):
        if array[min_index] > array[j]:
            min_index = j
    array[i], array[min_index] = array[min_index] , array[i]

print(array)
~~~

* 삽입 정렬 (Insertion Sort)
  * 원리: 두 번째 요소부터 시작해 현재 요소가 그 이전의 요소들과 비교하여 올바른 위치에 삽입하는 방식입니다. 이미 정렬된 부분에 새 요소를 삽입하는 과정을 반복합니다.
  * 시간 복잡도: 최악, 평균은 O(n^2), 최선은 O(n) (데이터가 거의 정렬되어 있을 때)
  * 특징: 데이터가 거의 정렬되어 있을 때 매우 효율적입니다. 작은 배열이나 리스트에 적합합니다.

~~~python
array = [7,5,9,0,3,1,6,2,4,8]

for i in range(1, len(array)):
    for j in range(i,0,-1):
        if array[j] < array[j-1]:
            array[j], array[j-1] = array[j-1] ,array[j]
        else:
            break

print(array)
~~~

* 퀵 정렬 (Quick Sort)
  * 원리: 리스트에서 기준점(pivot)을 선택해 그보다 작은 요소는 왼쪽에, 큰 요소는 오른쪽에 배치한 후, 이 과정을 재귀적으로 반복합니다.
  * 시간 복잡도: 최악은 O(n^2) (최악의 경우는 pivot이 계속 최솟값이나 최댓값으로 선택될 때), 평균은 O(n log n)
  * 특징: 평균적으로 매우 빠르며, 대부분의 경우 가장 많이 사용되는 정렬 알고리즘 중 하나입니다. 다만, 최악의 경우 성능 저하가 있을 수 있습니다.

~~~python
array = [7,5,9,0,3,1,6,2,4,8]

def quick_sort(array,start,end):
    # 원소가 1개인 경우 종료
    if start >= end:
        return

    pivot = start
    left = start + 1
    right = end

    while(left <= right):
        # 피벗보다 큰 데이터를 찾을 때까지 반복
        while (left <= end and array[left] <= array[pivot]):
            left += 1
        # 피벗보다 작은 데이터를 찾을 때까지 반복
        while (right > start and array[right] >= array[pivot]):
            right -= 1

        if(left > right):
            array[right], array[pivot] =  array[pivot], array[right]
        else:
            array[left], array[right] =  array[right], array[left]

    quick_sort(array, start, right - 1)
    quick_sort(array, right + 1, end)


quick_sort(array,0,len(array) -1)
print(array)
~~~

* 기수 정렬 (Radix Sort)
  * 원리: 자릿수별로 데이터를 정렬하여 최종적으로 전체를 정렬합니다. 숫자나 문자열과 같은 특정 데이터에 사용됩니다.
  * 시간 복잡도: O(d(n + k)) (d: 자릿수, n: 데이터 개수, k: 자릿수 내 최대 값)
  * 특징: 특정 조건에서 매우 빠르며, 비교 기반이 아닌 정렬 방법입니다. 


* 버블 정렬 (Bubble Sort)
  * 원리: 인접한 두 요소를 비교해 교환하며 정렬을 반복합니다. 가장 큰(혹은 작은) 요소가 "거품"처럼 리스트의 끝으로 이동하는 것이 특징입니다.
  * 시간 복잡도: 최악, 평균, 최선 모두 O(n^2)
  * 특징: 간단하지만, 효율성이 낮아 작은 데이터셋에서만 유용합니다. 


* 병합 정렬 (Merge Sort)
  * 원리: 리스트를 반으로 나누고, 각각을 재귀적으로 정렬한 후 합쳐서 전체 리스트를 정렬합니다.
  * 시간 복잡도: 최악, 평균, 최선 모두 O(n log n)
  * 특징: 안정적인 정렬 알고리즘으로, 항상 일정한 시간 복잡도를 보장합니다. 추가 메모리 공간을 사용해야 한다는 단점이 있습니다.

* 힙 정렬 (Heap Sort)
  * 원리: 힙(이진 트리 구조)를 사용해 최대값(혹은 최소값)을 차례로 추출하여 정렬합니다.
  * 시간 복잡도: 최악, 평균, 최선 모두 O(n log n)
  * 특징: 퀵 정렬과 비슷한 성능을 보이지만, 추가적인 메모리 사용이 적습니다.

<h2>
    <span class = "jjw_h2_style"> 정렬 예시 </span>
</h2>

문제 : 두 배열의 원소 교체
> 두 배열 A,B 가 존재하고 두 배열은 N개의 원소로 구성되어 있다.
> 최대 K번의 바꿔치기 연산을 수행할 수 있는데, 바꿔치기 연산이란 배열 A에 있는 원소 하나와 배열 B에 있는 원소 하나를 골라서 두 원소를 서로 바꾸는 것을 의미한다.
> 최종 목표는 배열 A의 모든 원소의 합이 최대가 되도록 하는 것이다.
> 배열 A,B 와 N,K 가 주어졌을 때, 최대 K번의 바꿔치기 연산을 수행하여 만들 수 있는 배열 A의 모든 원소의 합의 최댓값을 출력하는 프로그램을 작성하시오.

~~~python
a_list = [1,2,5,4,3]
b_list = [5,5,6,6,5]
N = 5
K = 3

sorted_a = sorted(a_list)
sorted_b = sorted(b_list,reverse = True)

for i in range(K):
    if sorted_a[i] < sorted_b[i]:
        sorted_a[i], sorted_b[i] = sorted_b[i] ,sorted_a[i]
    else:
        break

print(sum(sorted_a))

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

















