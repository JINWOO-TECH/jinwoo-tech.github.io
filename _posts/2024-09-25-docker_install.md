---
layout: post
title:  "AWS EC2에 Docker,docker-compose 설치 & 명령어 "
categories: data_engineering
tags: Docker
description: AWS EC2에 Docker,docker-compose 설치 & 명령어
---
<h4>전제조건</h4>
> AWS EC2서버

[AWS EC2서버 생성하는 법 보러가기](/aws/2024/09/25/aws_create_ec2.html)

<h2>
    <span class = "jjw_h2_style">AWS EC2에 Docker, Docker-compose 설치하기</span>
</h2>
1-1. docker install

~~~bash
# docker 설치
sudo dnf install docker

# 기존 아래 명령어로 docker를 설치했는데 AL2023나오면서 기존 지원한
# amazon-linux-extras가 실행 안됨 
# sudo amazon-linux-extras install docker

# docker 설치 확인
docker -v 

# docker 권한 부여
sudo usermod -a -G docker ec2-user
# docker 실행
sudo service docker start
# auto-start 
sudo chkconfig docker on
~~~

![Xixia](/assets/images/aws/20240925dockerinstall1.png)


<br> 

1-2. docker-compose install

~~~bash
# docker-compose install
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

# docker-compose 권한
sudo chmod +x /usr/local/bin/docker-compose
~~~

![Xixia](/assets/images/aws/20240925docker-composeinstall1.png)

<br>
<br> 

<h2>
    <span class = "jjw_h2_style">Docker 명령어 </span>
</h2>

~~~bash
# 도커 컨테이너 확인
docker ps

# 도커 이미지 확인
docker images

# 도커 컨테이너 삭제
docker rm {container-id}

# 도커 이미지 삭제
docker rmi {image-id}
~~~



