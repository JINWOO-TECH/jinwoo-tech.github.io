---
layout: post
title: 2. 코딩테스트 기초(python) - stack
description: >
  자주 사용되는 라이브러리
categories: programming
tags: 코딩테스트_기초_python
---

<h2>
    <span class = "jjw_h2_style"> Stack 이란? </span>
</h2>
<br>
후입선출 (LIFO, Last In First Out) 구조를 가지는 선형 자료구조입니다. 이는 마지막에 삽입된 데이터가 가장 먼저 제거되는 방식입니다. <br>

흔히 접시를 쌓는 것을 비유로 많이 사용합니다. 새 접시를 쌓을 때는 맨 위에 놓고, 사용할 때도 맨 위에 있는 접시부터 꺼내는 것과 유사합니다.
![Xixia](/assets/images/programming/20240827stack1.png)
<br>

* push : 스택의 top에 데이터 삽입
* pop : 스택의 top에 위치한 데이터 삭제

<br><br>

<h2>
    <span class = "jjw_h2_style"> Stack 코딩테스트 예시 </span>
</h2>

문제 : 문자열 편집기
> 주어진 문자열에서 다음과 같은 특수 기호들을 처리하여 최종 결과를 만드는 프로그램을 작성하세요: <br>
> '<' : 커서를 왼쪽으로 한 칸 이동합니다.  <br>
> ':' 커서를 오른쪽으로 한 칸 이동합니다. <br>
> '~' : 커서 바로 앞에 있는 문자를 삭제합니다. <br>
> 그 외의 문자들은 커서가 위치한 곳에 삽입됩니다. <br>

~~~
> 입력된 문자열: "b<A>c2de<<~>>" 
> 출력된 문자열: "Abcde" <br>
~~~

1. 로직 생각하기 <br>
문자열을 순차적으로 처리하고, 커서의 좌우를 stack 두개를 사용하여 관리 

2. 두 개 스택 준비
   3. left_stack : 커서 왼쪽에 있는 문자를 저장
   4. right_stack : 커서 오른쪽에 있는 문자들을 저장

3. 로직 구현
   6. '<' 일 경우 left_stack이 비어 있지 않다면, left_stack의 마지막 문자를 right_stack으로 이동
   7. '>' 일 경우 right_stack 비어 있지 않다면, right_stack 마지막 문자를 left_stack으로 이동
   8. '~' 일 경우 left_stack이 비어 있지 않다면, left_stack에서 마지막 문자를 삭제합니다.
   9. 나머지 경우 일때는 해당 문자를 left_stack에 추가
   10. left_stack과 right_stack을 결합하여 최종 문자열을 만듭니다. 이때, right_stack은 역순으로 결합 

<br>

~~~python
def process_string(input_string):
    left_stack = []
    right_stack = []
    
    for char in input_string:
        if char == '<':
            if left_stack:
                right_stack.append(left_stack.pop())
        elif char == '>':
            if right_stack:
                left_stack.append(right_stack.pop())
        elif char == '~':
            if left_stack:
                left_stack.pop()
        else:
            left_stack.append(char)
    
    return ''.join(left_stack + right_stack[::-1])

# 테스트
input_string = 'b<A>c2de<<~>>'
result = process_string(input_string)
print(result)  # Abcde
~~~  

<hr>
<div>
    {% assign mydata = site.posts | where: "categories", "programming" %}
    {% assign has_content = "False" %}
      <h2>
    <span class = "jjw_h2_style"> 프로그래머스 예시 </span> <br><br>
      </h2>
      {% for data in mydata %}
         {% if data.tags[0] contains "stack" %}
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

















