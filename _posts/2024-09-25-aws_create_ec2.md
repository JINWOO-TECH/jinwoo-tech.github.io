---
layout: post
title:  "(1) AWS 프리티어 사용하기 - EC2서버 생성 및 접속"
categories: aws
tags: AWS_프리티어
description: AWS 프리티어 사용하기 - EC2서버 생성 및 접속
---

<h2>
    <span class = "jjw_h2_style">AWS EC2 생성</span>
</h2>
[aws ec2 콘솔 가기](https://ap-northeast-2.console.aws.amazon.com/ec2/home?region=ap-northeast-2#Home)
<br>


<h3>인스턴스 시작 클릭</h3>
![Xixia](/assets/images/aws/20240925awsec2console.png)

<br>

<h3>인스턴스 설정</h3>
이름 및 태그 설정 -> 인스턴스 선택 -> 키 페어(로그인) -> 인스턴스 시작
<br> (네트워크, 스토리지, 고급설정은 기본값으로 설정)
<br>

프리티어 사용시 **프리 티어 사용 가능**한 인스턴스를 골라 사용 해야합니다.
![Xixia](/assets/images/aws/20240925awscrearte1.png)

<br>

키 페어는 원격 접속할 경우 필요합니다. 없는 경우 키 페어 생성을 눌러 생성해 주세요! <br>
생성 후 다운받은 `.pem` 파일은 잘 보관해주세요
![Xixia](/assets/images/aws/20240925awscrearte2.png)
![Xixia](/assets/images/aws/20240925awskeypair.png)


<h3>인스턴스 상태 확인</h3>
조금만 기다리면 `instance state`가 `Running`으로 바뀝니다
![Xixia](/assets/images/aws/20240925awsec2running.png)


<h2>
    <span class = "jjw_h2_style">AWS EC2 접속</span>
</h2>
EC2가 생성되었다면, MobaXterm으로 EC2에 접속해보겠습니다. <br>
만약 MobaXterm이 없을 경우 [MobaXterm 설치](https://mobaxterm.mobatek.net/download-home-edition.html)에 접속해 Installer edition을 다운 받은 후 설치해 주세요.

<br>
일단 퍼블릭 IPv4 주소를 알아야 되는데 AWS EC2 콘솔에서 원하는 인스턴스 클릭 후,<br>
Detail탭에 가면 퍼블릭 IPv4 주소를 복사할 수 있습니다.
![Xixia](/assets/images/aws/20240925awsec2publicip.png)
<br>
<br>
MobaXterm을 실행한 후, Session 탭을 클릭합니다. 탭이 열리면 아래 내용을 맞게 넣어줍니다. <br>
* Remote host : ec2-user@퍼블릭 IPv4 주소
* Use private key : 생성한 키 페어의 위치 

![Xixia](/assets/images/aws/20240925awsec2xterm.png)
<br>
<br>

접속 확인
![Xixia](/assets/images/aws/20240925awsec2connect.png)

<br>
<br>

<h2>
    <span class = "jjw_h2_style">AWS EC2 중지</span>
</h2>
일단 EC2 인스턴스가 돌아가면 비용이 발생하게 됩니다. <br>
프리티어지만 혹시나 돈이 빠져나가는 것을 최소화하고자, 사용을 안할때는 인스턴스를 중지했다가 사용할 경우 다시 시작을 하면됩니다.

AWS EC2 콘솔 > Instance State > 인스턴스 중지
![Xixia](/assets/images/aws/20240925awsec2stop.png)
