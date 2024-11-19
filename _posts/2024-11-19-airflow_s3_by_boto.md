---
layout: post
title:  "(11) Airflow s3로 파일 업로드 및 다운 (Airflow S3 boto)"
categories: data_engineering
tags: Airflow
description: Airflow s3로 파일 업로드 및 다운 (Airflow S3 boto) for Taskflow API
---
> 방법1. <a href="{{ site.baseurl }}/data_engineering/2024/11/15/airflow_s3_by_hook.html">Airflow S3 `Hook` 사용하기 </a> <br>
> 방법2. <a href="{{ site.baseurl }}/data_engineering/2024/11/15/airflow_s3_by_boto.html">Airflow S3 `boto` 사용하기 </a> <br>

<hr>

<h2>
    <span class = "jjw_h2_style">0. AWS CLI에 엑세스 키 등록 </span>
</h2>
<br>

~~~bash
aws configure
~~~


> AWS Access Key ID : {Access Key ID} <br>
> AWS Secret Access Key : {Secret Access Key}

<br>

![Xixia](/assets/images/dataengineer/20241119airflows3boto1.png)

<br>

<h2>
    <span class = "jjw_h2_style">1. s3 업로드, 읽기, 삭제 예시 코드 </span>
</h2>
<br>

~~~python
from airflow.decorators import dag, task, task_group
from datetime import datetime
import boto3

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
    file_path = f'/home/ec2-user/test.txt'
    bucket_name = 'jjw-raw-bucket'
    key = f'data/jjw_test.txt'
    
    # 파일 업로드
    @task
    def upload_to_s3(**context):
        print('start....')
        s3_client = boto3.client('s3')
        s3_client.upload_file(file_path, bucket_name, key)
        print(f"File uploaded to {bucket_name}/{key}")

    # 파일 읽기
    @task
    def read_from_s3(**context):
        print('start....')
        s3_client = boto3.client('s3')
        response = s3_client.get_object(Bucket=bucket_name, Key=key)
        file_content = response['Body'].read().decode('utf-8')
        print(f"File content: \n{file_content}")

    # 파일 삭제
    @task
    def delete_from_s3(**context):
        print('start....')
        s3_client = boto3.client('s3')
        s3_client.delete_object(Bucket=bucket_name, Key=key)
        print(f"File deleted from {bucket_name}/{key}")

    upload_to_s3() >> read_from_s3() >> delete_from_s3()

dag = test()
~~~
<br>

<br>

<h2>
    <span class = "jjw_h2_style">2. boto3 S3 Client 주요 함수들 </span>
</h2>
<br>

<table class="jjw_table">
  <thead>
    <tr>
      <th>함수명</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>list_buckets()</td>
      <td>현재 계정에서 접근 가능한 S3 버킷들의 목록을 반환합니다.</td>
    </tr>
    <tr>
      <td>create_bucket(Bucket)</td>
      <td>새로운 S3 버킷을 생성합니다.</td>
    </tr>
    <tr>
      <td>delete_bucket(Bucket)</td>
      <td>지정된 S3 버킷을 삭제합니다.</td>
    </tr>
    <tr>
      <td>list_objects_v2(Bucket, Prefix)</td>
      <td>특정 버킷의 객체 목록을 반환합니다. (Prefix를 사용하여 폴더처럼 특정 경로만 필터링 가능)</td>
    </tr>
    <tr>
      <td>upload_file(Filename, Bucket, Key)</td>
      <td>로컬 파일을 S3 버킷의 지정된 키(Key)로 업로드합니다.</td>
    </tr>
    <tr>
      <td>download_file(Bucket, Key, Filename)</td>
      <td>S3 버킷에서 특정 키(Key)의 파일을 로컬로 다운로드합니다.</td>
    </tr>
    <tr>
      <td>delete_object(Bucket, Key)</td>
      <td>특정 버킷에서 지정된 객체(Key)를 삭제합니다.</td>
    </tr>
    <tr>
      <td>put_object(Bucket, Key, Body)</td>
      <td>지정된 객체(Key)로 데이터를 업로드합니다. (파일 대신 문자열이나 바이너리 데이터 업로드 가능)</td>
    </tr>
    <tr>
      <td>get_object(Bucket, Key)</td>
      <td>S3 객체의 데이터를 반환합니다. (Body를 사용하여 파일 내용을 읽을 수 있음)</td>
    </tr>
    <tr>
      <td>copy(CopySource, Bucket, Key)</td>
      <td>S3 내에서 객체를 복사합니다.</td>
    </tr>
    <tr>
      <td>head_object(Bucket, Key)</td>
      <td>S3 객체의 메타데이터 정보를 반환합니다.</td>
    </tr>
  </tbody>
</table>

