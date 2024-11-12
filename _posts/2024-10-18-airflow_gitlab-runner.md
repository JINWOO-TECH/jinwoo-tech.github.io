---
layout: post
title:  "(6) Airflow gitlab-runner 연동"
categories: data_engineering
tags: Airflow
description: Airflow gitlab-runner 연동해서 형상관리 가능하게 하기
---
> 여러 사람이 Airflow 코드를 사용할 때, MWAA처럼 코드를 S3에 직접 업로드해야 하거나, EC2에 Docker 등을 사용해 직접 운영하는 경우 코드 형상 관리에 어려움을 겪었을 것입니다. <br>
> 만약 Gitlab을 사용하신다면 gitlab-runner을 통해 이 문제를 해결할 수 있습니다.<br>
> 이번 포스팅에서는 Docker로 Airflow를 띄운 서버를 기준으로 말씀 드리겠습니다. <br>
> 방법을 간단히 설명드리면 gitlab에 push가 일어나면 gitlab-runner가 .gitlab-ci.yml 에 작성된 내용을 수행행하는 방식입니다.<br>
> 



<h2>
    <span class = "jjw_h2_style">1.Gitlab Runner란? </span>
</h2>
<br>

GitLab Runner는 GitLab에서 제공하는 오픈 소스 CI/CD 툴로, 특정 명령어를 자동으로 실행하고 빌드, 테스트, 배포 파이프라인을 자동화하는 역할을 합니다. 이를 통해 개발자들은 코드 변경 사항이 있을 때마다 자동으로 업데이트와 배포를 실행할 수 있어 코드 형상 관리가 훨씬 수월해집니다.

GitLab Runner는 GitLab 서버와 통신하며, GitLab에서 설정된 CI/CD 파이프라인에 따라 다양한 작업을 실행할 수 있습니다. 이때 Runner는 프로젝트나 팀에 맞춰 별도로 설치하고 구성할 수 있고, Docker, Kubernetes, Shell 등 다양한 환경에서 작동하므로, EC2 또는 Docker 환경에서도 유연하게 사용할 수 있습니다.

<br><br>

<h2>
    <span class = "jjw_h2_style">2.runner 생성 및 토큰정보, url 확인하기 </span>
</h2>

1) Settings → CI / CD → Runners -> New project runner 에서 runner를 만들 수 있습니다

![Xixia](/assets/images/dataengineer/20241018gitlabrunner1.png)
<br><br>

2) tag이름 설정 후 runner 생성

![Xixia](/assets/images/dataengineer/20241018gitlabrunner2.png)
<br><br>

3) 토큰정보, url 확인

![Xixia](/assets/images/dataengineer/20241018gitlabrunner3.png)

<br><br>
4) runner 생성확인

![Xixia](/assets/images/dataengineer/20241018gitlabrunner4.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">3.EC2서버에 gitlab-runner install 및 권한 설정</span>
</h2>

1) gitlab-runner install (amazon linux)
~~~bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
sudo yum install gitlab-runner
~~~
<br>

2) register gitlab-runner
~~~bash
sudo gitlab-runner register --url $URL --registration-token $REGISTRATION_TOKEN
~~~
`만약 URL이 http로 시작한다면 https로 바꿔주세요`

![Xixia](/assets/images/dataengineer/20241018gitlabrunner5.png)
> url과 token 위 명령어를 통해 입력해서 저는 `enter an excutor`에 `shell`로만 입력했습니다

<br>


3) runner 실행
~~~bash
sudo gitlab-runner run
~~~
<br>

runner 실행 확인
![Xixia](/assets/images/dataengineer/20241018gitlabrunner6.png)
<br><br>

---------------------------------
**troubleshooting** <br>
`runner has never contacted this instance` 상태일 때

~~~bash
# gitlab-runner의 config.toml에 저장된 모든 러너들을 한번 체킹하면서 프로젝트 인스턴스와 연결을 수행
gitlab-runner -debug run
~~~

<br><br>

<h2>
    <span class = "jjw_h2_style">4. .gitlab-ci.yml 파일 수정</span>
</h2>

> 일단 저는 gitlab에 push를 했을 경우, 서버에서 git pull을 한 후, dags 파일을 전체를 교체해주는 방법을 사용했습니다.<br>
> 만약 S3에 코드를 사용하는 MWAA인 경우도 먼저 git pull을 한 이후, S3의 dags파일을 교체해주는 방법을 사용하면 될 것 같습니다!

CI/CD -> Editor
~~~bash
stages:
 - build
airfow-code-ci:
 stage: build
 only:
 - main
 - merge_requests
 script:
 - echo "Start......."
 - cd /svc/gitlab/airflow
 - sudo git pull
 - sudo rm -rf /svc/airflow_docker/dags/*
 - sudo cp -r /svc/gitlab/airflow/dags/* /svc/airflow_docker/dags
~~~


