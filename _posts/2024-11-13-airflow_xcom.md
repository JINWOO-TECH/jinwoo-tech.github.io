---
layout: post
title:  "(9) Airflow Task간 데이터 공유 - Xcom"
categories: data_engineering
tags: Airflow
description: Airflow Task간 데이터 공유(Xcom) for Taskflow API
---

<h2>
    <span class = "jjw_h2_style">1. Xcom 이란? </span>
</h2>
<br>
Airflow의 XCom은 task 간 데이터를 공유하기 위한 기능입니다. XCom(“Cross-communication”)을 통해 각 task가 서로 필요한 정보를 주고받으며 작업을 이어갈 수 있습니다
<br>

<h2>
    <span class = "jjw_h2_style">2. XCom 개념 </span>
</h2>
<br>

* `XCom 기능` : XCom은 task 간 데이터를 저장하고 검색하기 위한 메시지 큐 같은 역할을 합니다. task에서 반환한 값이나 필요한 데이터를 다른 task가 참조할 수 있습니다.
* `Key-Value 기반 저장`: XCom은 key-value 방식으로 데이터를 저장합니다. 따라서 동일한 task에서 여러 값을 저장할 수도 있습니다.
* `한정된 데이터 용량`: XCom은 주로 작은 데이터나 메타데이터 전달을 위해 설계되었습니다. 대용량 데이터나 파일을 전송할 경우엔 XCom 대신 외부 저장소(S3, GCS 등)를 사용하도록 권장됩니다.


<br>


<h2>
    <span class = "jjw_h2_style">3. 샘플 코드 </span>
</h2>
<br>

CASE 1. Taskflow API `return` 활용하기 

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
def test():

    @task
    def task1(**context):
        print('task1')
        s = 'this is from task1'
        return s
        
    @task
    def task2(data):
        print(data)


    data = task1()
    task2(data)

dag = test()
~~~
<br>


<br>

CASE 2. XCom Push 및 Pull을 사용

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
def test():

    @task
    def task1(**context):
        print('task1')
        s = 'this is from task1'
        context['ti'].xcom_push(key='xcom_test', value=s)
        
    @task
    def task2(**context):
        s = context['ti'].xcom_pull(key='xcom_test')
        print(s)


    task1() >> task2()

dag = test()
~~~
<br>

![Xixia](/assets/images/dataengineer/20241112airflowxcom1.png)

<br>


<h2>
    <span class = "jjw_h2_style">3. Airflow UI에서 xcom 데이터 확인하기 </span>
</h2>
<br>

`Admins > XComs` 에서 확인 가능합니다 
<br>
![Xixia](/assets/images/dataengineer/20241112airflowxcom2.png)
