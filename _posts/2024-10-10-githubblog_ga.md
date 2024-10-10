---
layout: post
title: Github 블로그 만들기 (Google Analytics)
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---
**Google Analytics**란?
웹사이트 소유자나 블로그 운영자가 방문자의 행동을 분석하고, 트래픽의 출처와 흐름을 이해할 수 있도록 도와주는 Google의 웹 분석 도구입니다.
Analytics를 사용하면 방문자 수, 페이지 뷰, 이탈률, 유입 경로 등 다양한 데이터를 추적할 수 있으며, 이를 통해 웹사이트의 성과를 개선하고, 마케팅 전략을 최적화할 수 있습니다.
또한, Google Analytics는 광고 캠페인의 성과를 측정하고 방문자의 행동 패턴을 이해하여 사용자 경험을 개선하는 데 중요한 역할을 합니다. (by chatgpt)

<br>

<h2>
    <span class = "jjw_h2_style">step 1. Google Analytics 시작 및 설정 </span>
</h2>
<br>
<input type="checkbox">  [Google Analytics](https://marketingplatform.google.com/about/analytics/)에 접속

<br>

<input type="checkbox">  지금 시작하기 클릭 후 계정 생성 내용 채우기  

![Xixia](/assets/images/github_blog/20241010ga1.png)

<br>
<input type="checkbox">  속성 만들기 내용 채우기  

![Xixia](/assets/images/github_blog/20241010ga2.png)

> `속성 이름` : 블로그 주소 <br>
> `보고 시간대` : 대한민국 <br>
> `통화` : 대한민국 원
> 

<br>
<input type="checkbox">비지니스 세부정보 설정  
![Xixia](/assets/images/github_blog/20241010ga3.png)

<br>
<input type="checkbox">비지니스 목표 설정  
![Xixia](/assets/images/github_blog/20241010ga4.png)

<br>
<input type="checkbox">데이터 수집 설정  
![Xixia](/assets/images/github_blog/20241010ga5.png)
<br>
![Xixia](/assets/images/github_blog/20241010ga6.png)


<br>

<h2>
    <span class = "jjw_h2_style">step 2. head에 script 붙여 넣기 </span>
</h2>
<br>
Google Analytics를 다 설정하고나면, 위의 그림과 같이 Google 태그 설정이 보여집니다. 태그를 복사해서 `_ includes/header.html` 
파일의 `<head>` 태그 안에 추가 해줍니다. 