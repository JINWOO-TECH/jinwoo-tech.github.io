---
layout: post
title:  "(7) Airflow Task간 의존성"
categories: data_engineering
tags: Apache_Airflow
description: Airflow Task Dependency For Taskflow API
---

<h2>
    <span class = "jjw_h2_style">1. 의존성 이란? </span>
</h2>
<br>

Airflow에서 의존성(dependencies)은 각 작업(task)들이 실행되는 순서와 관계를 정의하는 핵심 요소입니다. 데이터 파이프라인에서 서로 의존하는 task들이 올바른 순서로 수행되도록 제어함으로써, 복잡한 작업 흐름을 안전하고 일관성 있게 관리할 수 있습니다.
<br>

Airflow는 DAG(Directed Acyclic Graph, 방향성 비순환 그래프) 구조를 사용하여 여러 작업을 연결하고, 각 작업이 특정 조건에 따라 실행되도록 의존성을 정의합니다. 의존성 설정은 각 task가 언제, 어떤 조건에서 실행될지 결정하며, 파이프라인이 효율적이고 신뢰성 있게 작동하는 데 중요한 역할을 합니다.

<br>

<h2>
    <span class = "jjw_h2_style">2. 의존성의 주요 역할 </span>
</h2>
<br>

1. `순서 보장`:
데이터 파이프라인의 작업 순서를 정의하고, 의도한 대로 순차적으로 실행됩니다. 예를 들어 데이터 전처리 작업 후에 모델 학습을 수행하도록 순서를 지정할 수 있습니다.

2. `조건에 따른 실행`:
특정 task가 성공하거나 실패했을 때, 혹은 이전 task의 일부 조건을 만족할 때 다음 task가 실행되도록 설정할 수 있습니다. 이를 통해 예외 상황 처리, 데이터 유효성 검증 등을 효율적으로 관리할 수 있습니다.

3. `외부 데이터 및 타 DAG과의 연계`:
Airflow는 하나의 DAG 내에서만 의존성을 설정하는 것이 아니라, 다른 DAG의 결과에 따라 작업을 수행하도록 연결할 수 있습니다. 이를 통해 여러 데이터 파이프라인이 긴밀히 연계된 복잡한 작업 흐름을 관리할 수 있습니다.

<br>

<h2>
    <span class = "jjw_h2_style">3. 의존성 설정 방법 </span>
</h2>
<br>

Airflow에서는 각 작업을 `>>`, `<<` 연산자 또는 `set_downstream`, `set_upstream` 메서드를 통해 간단히 연결할 수 있습니다.
조건부 실행, 외부 DAG과의 의존성 설정, 트리거 규칙, 조건 분기 등 다양한 옵션이 있어 복잡한 요구사항에 맞는 유연한 의존성 구성이 가능합니다.


<br>

<h2>
    <span class = "jjw_h2_style">4. 샘플 코드 </span>
</h2>

<br>

* 단순 병렬 실행 <br><br>
  ![Xixia](/assets/images/dataengineer/20241112airflowtaskdependency4.png)
  <br><br>
   ~~~python
   from airflow.decorators import dag, task, task_group
   from datetime import datetime
   
   default_args = {
       'owner': 'jinwoo',
       'start_date': datetime(2024, 11, 10),  # 시작 날짜
       'end_date': datetime(2024, 11, 11),  # 시작 날짜
   }
   @dag(
       # schedule_interval="0 4 * * *",  # UTC
       schedule_interval=None,
       default_args=default_args,
       tags=['task_test']
   )
   def dependency_test():
       @task
       def task1(**context):
           print('task1')
       
       @task
       def task2(**context):
           print('task2')
       @task
       def task3(**context):
           print('task3')
   
       task1(), task2(), task3()
   
   dag = dependency_test()
   ~~~
<br>

* 선형 의존성 <br><br>
  ![Xixia](/assets/images/dataengineer/20241112airflowtaskdependency1.png)
  <br><br>
   ~~~python
   from airflow.decorators import dag, task, task_group
   from datetime import datetime
   
   default_args = {
       'owner': 'jinwoo',
       'start_date': datetime(2024, 11, 10),  # 시작 날짜
       'end_date': datetime(2024, 11, 11),  # 시작 날짜
   }
   @dag(
       # schedule_interval="0 4 * * *",  # UTC
       schedule_interval=None,
       default_args=default_args,
       tags=['task_test']
   )
   def dependency_test():
       @task
       def task1(**context):
           print('task1')
       
       @task
       def task2(**context):
           print('task2')
   
       task1() >> task2()
   
   dag = dependency_test()
   ~~~
<br>

* 팬아웃(일 대 다) 의존성 <br><br>
  ![Xixia](/assets/images/dataengineer/20241112airflowtaskdependency2.png) <br><br>
  ~~~python
  from airflow.decorators import dag, task, task_group
  from datetime import datetime
  
  default_args = {
      'owner': 'jinwoo',
      'start_date': datetime(2024, 11, 10),  # 시작 날짜
      'end_date': datetime(2024, 11, 11),  # 시작 날짜
  }
  @dag(
      # schedule_interval="0 4 * * *",  # UTC
      schedule_interval=None,
      default_args=default_args,
      tags=['task_test']
  )
  def dependency_test():
      @task
      def task1(**context):
          print('task1')
      
      @task
      def task2(**context):
          print('task2')

      @task
      def task3(**context):
          print('task3')
      
      task1() >> [task2(), task3()]
  
  dag = dependency_test()
  ~~~
<br>

* 팬인(다 대일) 의존성 <br><br>
  ![Xixia](/assets/images/dataengineer/20241112airflowtaskdependency3.png)<br><br>
  ~~~python
  from airflow.decorators import dag, task, task_group
  from datetime import datetime
  
  default_args = {
      'owner': 'jinwoo',
      'start_date': datetime(2024, 11, 10),  # 시작 날짜
      'end_date': datetime(2024, 11, 11),  # 시작 날짜
  }
  @dag(
      # schedule_interval="0 4 * * *",  # UTC
      schedule_interval=None,
      default_args=default_args,
      tags=['task_test']
  )
  def dependency_test():
      @task
      def task1(**context):
          print('task1')
      
      @task
      def task2(**context):
          print('task2')
  
      @task
      def task3(**context):
          print('task3')
      [task1(), task2()] >> task3()
  
  dag = dependency_test()
  ~~~