---
layout: post
title:  "(12) Airflow로 Glue Job 트리거 하기 "
categories: data_engineering
tags: Apache_Airflow
description: Airflow로 Glue Job 트리거 하기 for Taskflow API
---
### 전제 조건

1. **AWS 계정**과 AWS Glue Job 생성
2. <a href="{{ site.baseurl }}/data_engineering/2024/11/19/airflow_s3_by_boto.html">AWS CLI 엑세스키 등록 </a>
3. <a href="{{ site.baseurl }}/data_engineering/2024/12/03/airflow_triggerglue.html">IAM 권한 추가 </a>

<hr>

<h2>
    <span class = "jjw_h2_style">1. 예시 코드  </span>
</h2>

<br>


> 1. `crawler_name`(크롤러 이름)으로 크롤러를 실행시킵니다.<br>
> 2. `crawler_name`(크롤러 이름)으로 크롤러가 완료될 때까지 상태를 주기적으로 확인하며, 이때 `time.sleep(60)`을 사용하여 `60초` 간격으로 상태를 체크할 수 있습니다.(적절한 시간으로 사용해주세요) <br>
> 3. 만약 Crawler 상태가 `FAILED`나 `STOPPED`일 경우, 오류를 발생시켜 문제를 알립니다.

~~~python
from airflow.decorators import dag, task
from datetime import datetime
import boto3
import time

default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2024, 12, 3),  # 시작 날짜
    'provide_context': False,
}

def tableList_trigger_glue_job(crawler_name):
    
    glue_client = boto3.client('glue', region_name='ap-northeast-2')
    response = glue_client.start_crawler(Name=crawler_name)
    
    # Crawler 실행 완료를 기다림
    while True:
        response = glue_client.get_crawler(Name=crawler_name)
        crawler_state = response['Crawler']['State']
        print(crawler_state)
        if crawler_state == 'READY':
            print(f'Glue Crawler {crawler_name} executed successfully!')
            break
        elif crawler_state in ['RUNNING', 'STOPPING']:
            print(f'Glue Crawler {crawler_name} is still running. Current state: {crawler_state}')
            time.sleep(60)
        elif crawler_state in ['FAILED', 'STOPPED']:
            print(f'Glue Crawler {crawler_name} execution FAILED or STOPPED.')
            raise

@dag(
    schedule_interval=None,
    default_args=default_args,
    max_active_runs=1,
    catchup=False,
    tags=['test']
)
def hello_dag():
    @task
    def trigger_glue_job():
        crawler_name = 'glue_crawler'
        tableList_trigger_glue_job(crawler_name)

    trigger_glue_job()


dag = hello_dag()

~~~

