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

<hr>

<h2>
    <span class = "jjw_h2_style">1. IAM 권한 추가 </span>
</h2>
<br>

IAM > 원하는 사용자 OR 그룹 > 권한 추가 > `AWSGlueServiceRole` 추가

![Xixia](/assets/images/dataengineer/20241202awsglueiamrole.png)


<br>

<h2>
    <span class = "jjw_h2_style">2. 예시 코드  </span>
</h2>

<br>


> 1. `job_arguments`로 airflow에서 glue로 파라미터를 전달할 수 있습니다. <br>
> 2. Glue Job 실행 시, 실행 상태를 추적하기 위해 반환된 `job_run_id`를 사용합니다. <br>
> 3. Glue Job이 완료될 때까지 상태를 주기적으로 확인하며, 이때 `time.sleep(60)`을 사용하여 `60초` 간격으로 상태를 체크할 수 있습니다.(적절한 시간으로 사용해주세요)

<br>

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

def tableList_trigger_glue_job(job_name):
    
    glue_client = boto3.client('glue', region_name='ap-northeast-2')
    
    test_str = 'this is airflow'
    
    # Glue Job에 전달할 파라미터 설정
    job_arguments = {
        'test': test_str
    }

    # Glue Job 실행 요청 및 job_run_id 받기
    response = glue_client.start_job_run(
        JobName=job_name,
        Arguments=job_arguments
    )
    job_run_id = response['JobRunId']
    
    # Glue Job 완료를 기다림
    while True:
        response = glue_client.get_job_run(JobName=job_name, RunId=job_run_id)
        job_status = response['JobRun']['JobRunState']

        if job_status == 'SUCCEEDED':
            print(f'Glue Job {job_name} executed successfully!')
            break
        elif job_status in ['FAILED', 'STOPPED']:
            print(f'Glue Job {job_name} execution failed!')
            raise
        else:
            print(f'Glue Job {job_name} is still running. Current status: {job_status}')
            time.sleep(60)  # 60초마다 상태 확인

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
        job_name = 'glue_job'
        tableList_trigger_glue_job(job_name)

    trigger_glue_job()


dag = hello_dag()

~~~

