---
layout: post
title:  "(10) Airflow s3로 파일 업로드 및 다운 (Airflow S3 Hook)"
categories: data_engineering
tags: Apache_Airflow
description: Airflow s3로 파일 업로드 및 다운 (Airflow S3 Hook) for Taskflow API
---
> 방법1. <a href="{{ site.baseurl }}/data_engineering/2024/11/15/airflow_s3_by_hook.html">Airflow S3 `Hook` 사용하기 </a> <br>
> 방법2. <a href="{{ site.baseurl }}/data_engineering/2024/11/15/airflow_s3_by_boto.html">Airflow S3 `boto` 사용하기 </a> <br>

<hr>

<h2>
    <span class = "jjw_h2_style">0. Airflow S3 Hook 사용하기 </span>
</h2>
<br>

>  전제조건 <br>
> <a href="{{ site.baseurl }}/github_blog/2024/11/15/aws_create_s3.html">s3 생성 및 Access key 생성 </a>


<br>

<h2>
    <span class = "jjw_h2_style">1. 라이브러리 설치 </span>
</h2>
<br>

~~~bash
pip3 install apache-airflow[amazon]
~~~
<br>

<h2>
    <span class = "jjw_h2_style">2. Airflow Connection 등록 </span>
</h2>

<br>
Airflow UI에서 `Admin` -> `connection` 탭에 들어가 `+` 버튼을 클릭하여 새 연결을 설정해 줍니다.

![Xixia](/assets/images/aws/20241115accesskeyconnection1.png)
<br>
<br>

> Connection Id : `사용할 ID`  <br>
> Connection Type : `Amazon Web Services` <br>
> AWS Access Key ID , AWS Secret Access Key <br>
> Extra : `{"region_name : "ap-northeast-2"}`

![Xixia](/assets/images/dataengineer/20241115airflows30.png)
<br>
<br>

<h2>
    <span class = "jjw_h2_style">3. s3 업로드 예시 코드 </span>
</h2>

~~~python
from airflow.decorators import dag, task, task_group
from datetime import datetime
from airflow.providers.amazon.aws.hooks.s3 import S3Hook
import os

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
    def upload_to_s3(**context):
        print('start')
        filename = f'/home/ec2-user/test.txt'
        key = f'data/jjw_test.txt'
        bucket_name = 'jjw-raw-bucket'

        # S3에 파일 업로드
        hook = S3Hook('aws_access_key')  # connection ID
        if os.path.exists(filename):
            hook.load_file(filename=filename, key=key, bucket_name=bucket_name, replace=True)
            print(f"File {filename} uploaded to S3 bucket {bucket_name} with key {key}")
        else:
            print(f"File {filename} not found.")


    upload_to_s3()

dag = test()
~~~
<br>

> task 확인

![Xixia](/assets/images/dataengineer/20241115airflows31.png)

<br>

> s3 확인

![Xixia](/assets/images/dataengineer/20241115airflows32.png)

<br>
<br>

<h2>
    <span class = "jjw_h2_style">3. s3 파일 읽기 예시 코드 </span>
</h2>

~~~python
from airflow.decorators import dag, task, task_group
from datetime import datetime
from airflow.providers.amazon.aws.hooks.s3 import S3Hook
import os

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
    def read_file_from_s3(**context):
        bucket_name = 'jjw-raw-bucket'
        key = f'data/jjw_test.txt'
        
        hook = S3Hook('aws_access_key')
        
        # S3에서 파일 읽기
        file_obj = hook.get_key(key, bucket_name=bucket_name)
        if file_obj:
            file_content = file_obj.get()['Body'].read().decode('utf-8')
            print(f"Read content from {key}:\n{file_content}")
            return file_content  # 파일 내용을 다음 태스크에 사용할 수 있도록 반환
        else:
            print(f"File {key} not found in bucket {bucket_name}.")
            return None


    read_file_from_s3()

dag = test()
~~~

<br>
<br>

<h2>
    <span class = "jjw_h2_style">3. s3 파일 삭제 예시 코드 </span>
</h2>

~~~python
from airflow.decorators import dag, task, task_group
from datetime import datetime
from airflow.providers.amazon.aws.hooks.s3 import S3Hook
import os

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
    def delete_file_from_s3(**context):
        bucket_name = 'khuda-de-project'
        key = f'data/jjw_test.txt'
        
        hook = S3Hook('de_velog_aws')
        
        # S3에서 파일 삭제
        hook.delete_objects(bucket=bucket_name, keys=key)
        print(f"File {key} deleted from S3 bucket {bucket_name}")


    delete_file_from_s3()

dag = test()
~~~


<br>