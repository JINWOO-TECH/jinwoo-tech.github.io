---
layout: post
title:  "(3) AWS 프리티어 사용하기 - S3 버킷 만들기"
categories: aws
tags: AWS_프리티어
description: AWS 프리티어 사용하기 - S3 버킷 만들기
---

S3의 프리티어 조건은 아래와 같습니다

* 표준 스토리지 5GB까지 무료
* GET 요청 - 20,000건 (읽기 제한), PUT 요청 각 2,000건

<h2>
    <span class = "jjw_h2_style">AWS S3 버킷 생성</span>
</h2>
[aws ec2 콘솔 가기](https://ap-northeast-2.console.aws.amazon.com/s3/get-started?region=ap-northeast-2)
<br>


> `create bucket` 클릭

![Xixia](/assets/images/aws/20241115s3create.png)
<br>

> bucket 설정

![Xixia](/assets/images/aws/20241115s3create2.png)
<br>

> bucket 생성 확인

![Xixia](/assets/images/aws/20241115s3create3.png)
<br><br>

<h2>
    <span class = "jjw_h2_style">AWS Access Key 생성</span>
</h2>
[aws ec2 콘솔 가기](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-northeast-2#/users)
<br><br>
AWS 의 IAM을 통해 리소스의 접근 권한을 설정할 수 있습니다.<br>
Access Key 를 발급하기 위해서는 우선 사용자가 있어야 하는데 없다면 `create user` 버튼을 클릭하여 사용자를 생성해야 합니다.
![Xixia](/assets/images/aws/20241115accesskey1.png)

<br>

> 사용자 이름을 누르면 정보가 나옵니다. `Security credentials`탭의 `Access keys`에서 `create access key`를 클릭하여 access key를 생성

![Xixia](/assets/images/aws/20241115accesskey4.png)

<br>

> `Application running on an aws compute service`를 체크하고 `Next` 버튼을 클릭

![Xixia](/assets/images/aws/20241115accesskey2.png)

<br>

> 생성된 Accesskey를 메모장에 보관

![Xixia](/assets/images/aws/20241115accesskey3.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">사용자 권한 설정</span>
</h2>

> `IAM` > `Users` 에서 s3에 업로드 할 User 선택

![Xixia](/assets/images/aws/20241115s3create4.png)

<br> 

> `Permissions` > `Add permisions` 버튼 클릭

![Xixia](/assets/images/aws/20241115s3create5.png)

<br> 

>  `Attach policies directly` > `AmazonS3FullAccess` 체크 > `Next` > `Add permisions`

![Xixia](/assets/images/aws/20241115s3create6.png)
