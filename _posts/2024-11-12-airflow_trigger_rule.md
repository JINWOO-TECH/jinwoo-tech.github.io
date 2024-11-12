---
layout: post
title:  "Airflow Task trigger rule"
categories: data_engineering
tags: Airflow
description: Airflow Task Trigger Rule For Taskflow API
---

<h2>
    <span class = "jjw_h2_style">1. trigger_rule 이란? </span>
</h2>
<br>

Airflow에서 `trigger_rule`은 특정 task가 언제 실행될지 결정하는 중요한 설정입니다. 일반적으로 task는 이전 task들이 성공했을 때만 실행되지만, 다양한 시나리오에서 유연하게 작업을 제어할 수 있도록 여러 `trigger_rule` 옵션을 제공합니다.
<br>

<h2>
    <span class = "jjw_h2_style">2. trigger_rule 옵션 </span>
</h2>
<br>

<table class="jjw_table">
  <thead>
    <tr>
      <th>Trigger Rule 옵션</th>
      <th>의미</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>all_success</td>
      <td>모든 upstream task가 성공한 경우 실행</td>
    </tr>
    <tr>
      <td>all_failed</td>
      <td>모든 upstream task가 실패한 경우 실행</td>
    </tr>
    <tr>
      <td>all_done</td>
      <td>모든 upstream task의 상태와 관계없이 실행 (성공, 실패, 스킵 등)</td>
    </tr>
    <tr>
      <td>one_success</td>
      <td>하나의 upstream task라도 성공한 경우 실행</td>
    </tr>
    <tr>
      <td>one_failed</td>
      <td>하나의 upstream task라도 실패한 경우 실행</td>
    </tr>
  </tbody>
</table>


<br>

<h2>
    <span class = "jjw_h2_style">2. 샘플코드 </span>
</h2>
<br>

![Xixia](/assets/images/dataengineer/20241112airflowtaskdependency5.png)

~~~python
from airflow.decorators import dag, task, task_group
from datetime import datetime

default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2024, 11, 10),  # 시작 날짜
 #   'end_date': datetime(2024, 11, 11),  # 시작 날짜
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

    @task(trigger_rule='all_failed')
    def task2(**context):
        print('task2')

    @task(trigger_rule='all_done')
    def task3(**context):
        print(task3)

    task1() >> [task2(),task3()]

dag = dependency_test()
~~~