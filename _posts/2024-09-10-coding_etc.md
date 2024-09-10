---
layout: post
title: 9. 코딩테스트 기초(python) - 기타 알고리즘
description: >
  코딩테스트에 자주 출제되는 기타 알고리즘
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> 소수 판별 </span>
</h2>
1보다 큰 자연수 중에서 1과 자기 자신을 제외한 자연수로는 나누어떨어지지 않는 자연수입니다 <br>

소수를 판별하기 위해 2 ~ 판별하는 수 까지 반복문을 돌리는 것보다 약수의 성질을 활용하는 것이 바람직합니다.<br>
약수의 성질이랑 모든 약수가 가운데 약수를 기준으로 곱셈 연산에 대해 대칭을 이루는 것을 말합니다. <br>
예를 들어 16의 약수는 1,2,4,8,16입니다 이때 2 * 8 은 8 * 2 와 대칭입니다. <br>
따라서 우리는 특정한 자연수의 약수를 찾을 때 가운데 약수까지만 확인 하면 됩니다.


~~~python
import math

def is_prime(number):
    if number == 1 or number == 2:
        return False
    half_number = number // 2
    for i in range(2, int(math.sqrt(number)+1)):
        if number % i == 0:
            return False # 소수가 아님
    return True # 소수임

number = 21
if  is_prime(number):
    print('소수')
else:
    print('소수 아님')
~~~

<h2>
    <span class = "jjw_h2_style"> 에라토스테네스의 체 </span>
</h2>
특정한 수의 범위 안에 존재하는 모든 소수를 찾아야 할 때 에라토스테네스의 체 알고리즘을 사용할 수 있습니다 <br>

<span style="background-color: yellow;">에라토스테네스의 체 알고리즘 구체적 동작 과정</span>
1. 2부터 N까지의 모든 자연수를 나열한다
2. 남은 수 중에서 아직 처리하지 않은 가장 작은 수 i를 찾는다.
3. 남은 수 중에서 i의 배수를 모두 제거한다 (i는 제거하지 않는다)
4. 더 이상 반복할 수 없을 때까지 2번과 3번의 과정을 반복한다.


~~~python
import math

# 2부터 1000까지 소수 판별
n = 1000

array = [True for _ in range(n+1)]

for i in range(2,int(math.sqrt(n)) + 1):
    if array[i] == True:
        # 모든 배수 지우기
        j =2
        while i * j <= n:
            array[i * j] = False
            j += 1

for i in range(2, n+1):
    if array[i] :
        print(i , end =' ')
~~~

<h2>
    <span class = "jjw_h2_style"> 투 포인터 </span>
</h2>
투 포인터 알고리즘은 리스트에 순차적으로 접근해야 할 때 두 개의 점의 위치를 기록하며 처리하는 알고리즘을 의미합니다 <br>
리스트에 담긴 데이터에 순차적으로 접근해야 할 때는 **시작점**과 **끝점** 2개의 점으로접근할 데이터의 범위를 표현할 수 있습니다.

문제 예시)
> N개의 자연수로 구성된 수열이 있다 <br>
> 합이 M인 부분 연속 수열의 개수를 구하시오

문제 아이디어)
1. 시작점(start)과 끝점(end)이 첫 번째 요소의 인덱스(0)을 가리키도록 한다.
2. 현재 부분 합이 M과 같다면, 카운트
3. 현재 부분 합이 M 작다면, end를 1증가
4. 현재 부분 합이 M보다 크거나 같다면, start를 1증가
5. 모든 경우를 확인할 때까지 2~4번 반복


~~~python
N = [1, 2, 3, 2, 5]
M = 5

start, end = 0, 0

total = 0
cnt = 0

while True:
    # 수열 길이보다 커지면 끝
    print(f"start : {start}, end : {end}")
    if start == len(N):
        break

    # end 값이 안커지게
    if end > len(N):
        end = len(N)

    # start값이 end값보다 커지면 end 값 start 값으로 초기화
    if start > end:
        end = start

    if total < 5:
        total += N[end]
        end +=1
    elif total > 5:
        total -= N[start]
        start += 1

    else:
        total -= N[start]
        cnt += 1
        start += 1

print(cnt)
~~~

<h2>
    <span class = "jjw_h2_style"> 구간 합  </span>
</h2>
연속적으로 나열된 N개의 수가 있을 때 특정 구간의 모든 수를 합한 값을 계산하는 문제를 구간 합 문제라고 합니다.

문제 예시)
> N개의 정수로 구성된 수열이 있습니다. <br>
> M개의 쿼리 정보가 주어집니다 <br>
> 각 쿼리는 LEFT 와 Right로 구성됩니다. [LEFT,Right] 구간에 포함된 데이터들의 합을 출력해야 합니다

문제 아이디어)
1. 접두사 합 : 배열의 맨 앞부터 특정 위치까지의 합을 미리 구해 놓는 것
2. N 개의 수 위치 각각에 대하여 접두사 합을 계산하여 P에 저장합니다
3. 매 M개의 쿼리 정보를 확일할 때 구간 합은 P[Right] - p[LEFT-1] 입니다

~~~python
N = [10, 20, 30, 40, 50]

P = [0]
sum_value = 0
for data in N:
    sum_value += data
    P.append(sum_value)

left = 3
right = 4

print(P[right] - P[left - 1])
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

















