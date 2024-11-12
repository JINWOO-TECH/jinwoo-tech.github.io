---
layout: post
title:  "(4)Airflow standalone 실행"
categories: data_engineering
tags: Airflow
description: Airflow standalone 실행
---

<h2>
    <span class = "jjw_h2_style">STEP 1.필수 패키지 설치 </span>
</h2>
<br>
~~~bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-pip python3-venv
~~~

<br>

<h2>
    <span class = "jjw_h2_style">STEP 2. 가상 환경 생성 </span>
</h2>
<br>
~~~bash
python3 -m venv airflow_env
source airflow_env/bin/activate
~~~
<br>

<h2>
    <span class = "jjw_h2_style">STEP 3. Airflow 설치 </span>
</h2>

<br>

~~~bash
# 환경 변수를 설정해야 합니다 (여기서 버전은 원하는 대로 변경 가능)
export AIRFLOW_VERSION=2.7.2
export CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-3.8.txt"

# Airflow 설치
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}"
~~~

<br>

<h2>
    <span class = "jjw_h2_style">STEP 4. Airflow 초기화 및 웹서버 실행 </span>
</h2>

<br>

~~~bash
# Airflow 환경변수 설정
export AIRFLOW_HOME=~/airflow

# DB 초기화
airflow db init

# 관리자 계정 생성
airflow users create \
    --username admin \
    --password admin \
    --firstname YourName \
    --lastname LastName \
    --role Admin \
    --email your.email@example.com

# ----------- Airflow Example file 제거 --------- #
# 실행전 airflow dag 예시들이 필요하지 않다면, `airflow.cfg`파일에서 `load_example`을 찾아 `False`로 수정하시면 됩니다.
# ----------- Airflow Example file 제거 --------- #

# 웹 서버 실행 (포트 8080)
airflow webserver --port 8080

# 웹 서버 실행 (백그라운드 실행)
airflow webserver --port 8080 --daemon
~~~

<br>


<h2>
    <span class = "jjw_h2_style">STEP 5. Airflow 스케줄러 실행 </span>
</h2>
<br>
~~~bash
airflow scheduler --daemon
~~~

<br>

<h2>
    <span class = "jjw_h2_style">STEP 6. Airflow UI 보기 </span>
</h2>
<br>

6-1. mobaxterm의 Tunneling > New SSH tunnel 클릭
![Xixia](/assets/images/dataengineer/20241010mobaxtermtunneling1.png)

<br>
6-2. 정보 입력 

![Xixia](/assets/images/dataengineer/20241010mobaxtermtunneling2.png)

1번 영역
> `Remote server`: ec2-XX-XXX-XXX-XXX.compute-1.amazonaws.com (EC2 퍼블릭 IP 또는 퍼블릭 DNS) <br>
> `Remote port`: 8080 (Airflow 웹 서버 포트)

2번 영역

> `SSH server`: ec2-XX-XXX-XXX-XXX.compute-1.amazonaws.com (EC2 퍼블릭 IP 또는 퍼블릭 DNS) <br>
> `SSH login`: ec2-user (Amazon Linux의 경우) <br>
> `SSH port`: 22 (기본 SSH 포트) 

3번 영역
> `Forwarded port`: 18080 (로컬 머신에서 사용할 포트)

<br>
6-3. 키페어 등록 후 실행 버튼 클릭

![Xixia](/assets/images/dataengineer/20241010mobaxtermtunneling3.png)

<br>
6-4. https://localhost:18080 접속 후 로그인

![Xixia](/assets/images/dataengineer/20241010mobaxtermtunneling4.png)

> `Username`: 위에 설정한 airflow db username <br>
> `Password`: 위에 설정한 airflow db password 


<br>

<h2>
    <span class = "jjw_h2_style">STEP 6. dag 생성 </span>
</h2>

> `$AIRFLOW_HOME/airflow/dags` 하위에 `{dag이름}.py` 로 파일 생성

저는 `/home/ec2-user/airflow/dags/hello_dag.py` 에 dag을 생성했습니다. <br><br>

hello_dag.py
~~~python
from airflow.decorators import dag, task
from datetime import datetime

default_args = {
    'owner': 'jinwoo',
    'start_date': datetime(2024, 10, 2),  # 시작 날짜
    # 'end_date' : datetime(2022, 9, 23, tzinfo=local_tz),
    'provide_context': False,
    # 'retries': 1,
    # "on_failure_callback": alter.slack_fail_alert
}


@dag(
    schedule_interval=None,
    # schedule_interval="0 2 * * *",
    default_args=default_args,
    max_active_runs=1,
    # catchup=True, # 반복시 설정
    catchup=False,
    tags=['test']
)
def hello_dag():
    @task
    def print_hello():
        print('hello')

    print_hello()


dag = hello_dag()
~~~

<br>
![Xixia](/assets/images/dataengineer/20241010airflowstandalonehellodag1.png)

![Xixia](/assets/images/dataengineer/20241010airflowstandalonehellodag2.png)










