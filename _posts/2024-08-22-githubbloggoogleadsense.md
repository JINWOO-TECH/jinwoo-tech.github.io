---
layout: post
title: Github 블로그 만들기 (Google AdSense 등록)
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---
**Google AdSense**이란?
Google AdSense는 웹사이트 소유자나 블로그 운영자가 자신의 사이트에 광고를 게재하여 수익을 창출할 수 있도록 도와주는 Google의 광고 프로그램입니다. 
AdSense를 통해 사이트 소유자는 사이트에 맞는 광고를 자동으로 게재할 수 있으며, 방문자가 해당 광고를 클릭하거나 광고와 상호작용할 때마다 수익을 얻습니다. (by chatgpt)

<br>

<h2>
    <span class = "jjw_h2_style">step 1. Google AdSense 시작하기 </span>
</h2>
<br>
<input type="checkbox">  [Google AdSense](https://adsense.google.com/intl/ko_kr/start/)에 접속
<br>
<input type="checkbox">  시작하기 클릭 후 내용 채우기  

![Xixia](/assets/images/github_blog/20240823adsense.png)

<br>
<input type="checkbox">  정보 입력 및 광고 설정하기 
![Xixia](/assets/images/github_blog/20240823adsense2.png)

<br>
<input type="checkbox">  스크립트 삽입하기 <br>
사이트 소유권을 확인해야한다.
이번에는 `<head></head>` 태그 사이에 스크립트를 심어야 돼서 `_includes/header.html` 에 스크립트를 추가하면된다
<br>

![Xixia](/assets/images/github_blog/20240823adsense3.png)
![Xixia](/assets/images/github_blog/20240823adsense4.png)

<br><br>
<input type="checkbox">  검토 기다리기...
![Xixia](/assets/images/github_blog/20240823adsense5.png)

<br>
약 2주뒤!!<br>
![Xixia](/assets/images/github_blog/20240904adsenseaprove.png)


<input type="checkbox">  ads.txt 파일 등록하기
승인 후 google adsense에 들어가면 아래처럼 ads.txt파일 문제를 해결하라고 나옵니다
![Xixia](/assets/images/github_blog/20240904adsenseafirstpage.png)

[ads.txt 파일 추가하기](https://support.google.com/adsense/answer/12171244)
루드 디렉토리에 **ads.txt** 파일을 만들어주고 아래의 내용을 붙여넣습니다 
> google.com, pub-0000000000000000, DIRECT, xxxxxxxxxxxxxxxxx <br>

여기서 pub-0000000000000000은 게시자 ID입니다.<br>
xxxxxxxxxxxxxxxxx 이 부분은 'ads.txt 파일 추가하기' 링크 클릭후 확인해 주세요.

