---
layout: post
title: 1. 코딩테스트 기초(python) - 자주 사용되는 라이브러리
description: >
  자주 사용되는 라이브러리
categories: programming
tags: 코딩테스트_기초_python
---


<h2>
    <span class = "jjw_h2_style">step 1. sum, min, max, eval 함수 </span>
</h2>
<br>

~~~python
a_list = [1,2,3,4,5,6]

# sum()
sum_result = sum(a_list)
print(sum_result) # 21

# min()
min_result = min(a_list)
print(min_result) # 1


# max()
max_result = max(a_list)
print(max_result) # 6

# eval()
eval_result = eval("(1+2)*5")
print(eval_result) # 15
~~~
<br><br>

<h2>
    <span class = "jjw_h2_style">step 2. sorted 함수 </span>
</h2>
<br>
Python의 내장 sorted() 함수는 Timsort 알고리즘을 사용합니다. <br> 
Timsort는 Merge Sort와 Insertion Sort의 아이디어를 결합한 하이브리드 정렬 알고리즘입니다. <br>
Merge Sort: 리스트를 절반으로 나누어 각각 정렬한 후, 다시 병합하는 방식입니다. 병합하는 과정에서 비교가 이루어지며, 전체 시간 복잡도는 **O(n log n)**입니다. <br>
Insertion Sort: 작은 배열의 경우, 삽입 정렬이 더 빠르게 작동합니다. 삽입 정렬은 리스트가 거의 정렬된 경우에 매우 효율적이며 **O(n)**의 시간 복잡도를 가집니다. <br>
따라서 시간복잡도는 최선의 경우는 **O(n)**, 평균 및 최악의 경우인 경우는 **O(n log n)** 입니다

~~~python
result = [3,1,2,4,7,5,6]

# 오름차순 정렬
print(sorted(result)) # [1,2,3,4,5,6,7]

# 내림차순 정렬 
print(sorted(result, reverse=True)) # [1,2,3,4,5,6,7]


# sorted() with key
result = [('a',10),('b',5),('c',20),('d',15),('e',20)]

# 숫자로 내림차수 정렬
print(sorted(result, key= lambda x: x[1],reverse=True)) # [('c', 20), ('e', 20), ('d', 15), ('a', 10), ('b', 5)]


# dictionary 정렬

result_dict = {'a' : 10, 'b' : 5 , 'c' : 3, 'd' : 10, 'e' : 2}

# key값 오름 value값 오름
print(dict(sorted(result_dict.items(), key=lambda x: (x[1], x[0]))))
# {'e': 2, 'c': 3, 'b': 5, 'a': 10, 'd': 10}

# key값 오름 value값 내림
print(dict(sorted(result_dict.items(), key=lambda x: (-x[1], x[0]), reverse=True)))
# {'e': 2, 'c': 3, 'b': 5, 'd': 10, 'a': 10}

# key값 내림 value값 내림
print(dict(sorted(result_dict.items(), key=lambda x: (x[1], x[0]), reverse=True)))
# {'d': 10, 'a': 10, 'b': 5, 'c': 3, 'e': 2}

# key값 내림차순 value값 오름
print(dict(sorted(result_dict.items(), key=lambda x: (-x[1], x[0]))))
# {'a': 10, 'd': 10, 'b': 5, 'c': 3, 'e': 2}

~~~

<br><br>

<h2>
    <span class = "jjw_h2_style">step 3. 순열 조합 </span>
</h2>
<br>

**순열** : 서로 다른 n개에서 서로 다른 r개를 선택하여 일렬로 나열하는 것 -> ** 순서 중요 O ** <br>

**조합** : 서로 다른 n개에서 **순서에 상관 없이** 서로 다른 r개를 선택하는 것 -> ** 순서 중요 X **

~~~python
data = ['A','B','C']

# 순열 (3개선택)
from itertools import permutations

per_result = list(permutations(data,3))
print(per_result)
# [('A', 'B', 'C'), ('A', 'C', 'B'), ('B', 'A', 'C'), ('B', 'C', 'A'), ('C', 'A', 'B'), ('C', 'B', 'A')]

# 조합 (3개선택)
from itertools import combinations

com_result = list(combinations(data,3))
print(com_result)
# [('A', 'B', 'C')]

~~~

<br><br>

<h2>
    <span class = "jjw_h2_style">step 4. Counter 라이브러리 </span>
</h2>
<br>

~~~python
from collections import Counter

a_list = ['a','b','c','b','d','a','a','c','d','e','a','a']

counter = Counter(a_list)

print(counter)
# Counter({'a': 5, 'b': 2, 'c': 2, 'd': 2, 'e': 1})

# a의 갯수
print(counter['a'])
# 5

# dictionary로 반환
print(dict(counter))
# {'a': 5, 'b': 2, 'c': 2, 'd': 2, 'e': 1}

~~~

<br><br>

<h2>
    <span class = "jjw_h2_style">step 5. 최대 공약수, 최소 공배수 </span>
</h2>
<br>

~~~python
import math

# 최대 공약수
result = math.gcd(12,6)
print(result)


# 최소 공배수
result = math.lcm(12,6)
print(result)
~~~


<h2>
    <span class = "jjw_h2_style">step 6. 이진 탐색 라이브러리 </span>
</h2>
<br>
bisect_left(a,x) : 정렬된 순서를 유지하면서 배열 a에 x를 삽입할 가장 왼쪽 인덱스를 반환
bisect_right(a,x) : 정렬된 순서를 유지하면서 배열 a에 x를 삽입할 가장 오른쪽 인덱스를 반환

~~~python
from bisect import bisect_left, bisect_right

a = [1, 2, 4, 4, 8]
x = 4

print(bisect_left(a, x)) # 2
print(bisect_right(a, x)) # 4 

~~~












