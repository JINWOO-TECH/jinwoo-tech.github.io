---
layout: post
title:  "Airflow AWS EC2에 Docker로 설치하기"
categories: data_engineering
tags: Airflow
description: Airflow AWS EC2에 Docker로 설치
---

<h4>전제조건</h4>
> 서버에 docker 및 docker-compose 설치 완료
> 서버 (최소사양 cpu : 2 vCPUs, 메모리 : 4GB RAM)

[docker 및 docker-compose 설치하는 법 보러가기](/data_engineering/2024/09/25/docker_install.html)


<h2>
    <span class = "jjw_h2_style">STEP 1.docker-compose.yaml 파일 다운 </span>
</h2>

~~~bash
# {airflow-version}에 원하는 버전 입력
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/{airflow-version}/docker-compose.yaml'

# 예 
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.3.3/docker-compose.yaml'
~~~

![Xixia](/assets/images/dataengineer/20241008airflowymlfiledown.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">STEP 2. .env 설정 </span>
</h2>

~~~bash
# UID 확인
echo $(id -u)

# .env 파일 UID 설정
vi .env
######################
AIRFLOW_UID= {UID}
######################

# 만약 echo $(id -u) 의 값이 1000이라면 .env 파일에는 AIRFLOW_UID= 1000 으로 설정

~~~

<h2>
    <span class = "jjw_h2_style">STEP 3. 파이썬 종속성 설정  </span>
</h2>
파이썬 종속성을 설정하기 위해 Dockerfile과 requirements.txt 파일을 docker-compose.yaml파일과 동일한 위치에 만들어야 합니다.

![Xixia](/assets/images/dataengineer/20241008airflowfiletree.png)

Dockerfile
~~~bash
FROM apache/airflow:2.3.0
COPY requirements.txt .
RUN pip3 install --upgrade pip
RUN pip3 install awscli
RUN pip3 install -r requirements.txt
~~~
<br>

requirements.txt
~~~bash
--constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.3.0/constraints-3.7.txt"
# 파이썬 종속성 추가 
boto3==1.22.0
fsspec==2022.3.0
s3fs==0.4.2
~~~

<br>


<h2>
    <span class = "jjw_h2_style">STEP 4. docker-compose.yaml파일 수정  </span>
</h2>

일단 Dockerfile을 사용하도록 수정해주고, 기타 설정을 해줍니다

> image 부분을 지우고 bulid: . 의 주석을 지웁니다. <br>
> enviroment 부분을 수정합니다 <br>
> 일단 저는 예시 dag이랑 default timezone을 설정했습니다.

![Xixia](/assets/images/dataengineer/20241008airflowyamlfile.png)


<h2>
    <span class = "jjw_h2_style">STEP 5. docker-compose 실행 </span>
</h2>

~~~bash
# 초기 실행 때는 에러 로그를 확인하기 위해 
docker-compose up

# 백그라운드 
docker-compose up -d 

~~~

저는 AWS 프리티어를 사용해서 Airflow를 Docker로 띄우려고 시도했는데, CPU 사용률이 100%까지 치솟아서 제대로 작동하지 않았어요. 😓 <br>
<span class="jjw_line">(참고: Airflow 최소사양은 CPU: 2 vCPUs, 메모리: 4GB RAM입니다.)</span>

만약 프리티어가 아닌 환경에서 최소 사양을 만족한다면, 위의 방법으로 충분히 Airflow를 띄울 수 있을 거예요. <br>

그래서 이번에는 프리티어에서 사용할 수 있는 Airflow를 구동하는 방법을 소개해드리려고 합니다! <br>

<br>

[airflow standalone 실행](/data_engineering/2024/10/08/airflow_standalone_install.html)