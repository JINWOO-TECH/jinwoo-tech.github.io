---
layout: post
title: Github 블로그 만들기 (seo 적용)
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---

<h2>
    <span class = "jjw_h2_style">step 1. sitemap.xml 생성하기</span>
</h2>

검색엔진에서 Github 블로그의 글들을 긁어올 수 있도록, 사이트의 모든 페이지 정보를 모아두는 `sitemap.xml`이 필요합니다. 
새로운 페이지를 생성할 때마다 매번 추가하는 건 어려운 일이니 자동으로 추가가 되도록 설정해 줍니다 <br>

<input type="checkbox"> {username}.github.io 하위 폴더에 sitemap.xml을 생성한 후 
[sitemap.xml 링크](https://github.com/JINWOO-TECH/jinwoo-tech.github.io/blob/main/sitemap.xml)를 복사 후 git에 push 합니다


<input type="checkbox">  push 완료후 아래와 같이 url에 {username}.github.io/sitemap.xml을 확인합니다.

![Xixia](/assets/images/github_blog/20240822sitemap.png)
<br><br>

<h2>
    <span class = "jjw_h2_style">step 2. robot.txt 생성하기</span>
</h2>

사이트를 검색엔진에 등록하면 검색엔진의 크롤러가 사이트를 크롤링합니다. robot.txt는 크롤러가 접근 할 수 있는 URL을 알려줍니다.

<input type="checkbox"> {username}.github.io 하위 폴더에 robot.txt을 생성한 후 아래 코드를 붙여넣습니다. <br>
(sitemap의 주소는 자신의 블로그 URL로 수정)

~~~
User-agent: *
Allow: /
Sitemap: https://{username}.github.io/sitemap.xml
~~~
<br><br>

<h2>
    <span class = "jjw_h2_style">step 3. feed.xml 생성하기</span>
</h2>

feed.xml는 웹사이트의 RSS 또는 Atom 피드를 제공하는 XML 파일입니다. 이 파일은 사이트의 최신 콘텐츠를 구독할 수 있도록 도와주는 기능을 합니다.

<input type="checkbox"> {username}.github.io 하위 폴더에 feed.xml을 생성한 후 
[feed.xml 링크](https://github.com/JINWOO-TECH/jinwoo-tech.github.io/blob/main/feed.xml)를 복사 후 git에 push 합니다.

<br><br>

<h2>
    <span class = "jjw_h2_style">step 4. 검색엔진 등록하기</span>
</h2>

### 4.1 구글(Google)
<input type="checkbox"> [Google Search Console](https://search.google.com/search-console/welcome) 접속 후 URL 접두어에 `https://{username}.github.io` 입력
<br>
![Xixia](/assets/images/github_blog/20240822googleseopage.png)

<br><br>

<input type="checkbox"> 해당 HTML 파일 {username}.github.io 하위 폴더에 저장 (이름 안바꾸고 그대로 넣으시면 됩니다) 후 git push
<br>
![Xixia](/assets/images/github_blog/20240822googleseopageconfirm.png)

<br><br>

<input type="checkbox"> 소유권 확인 후 `속성으로 이동` 클릭 후 <br>
<input type="checkbox"> 좌측메뉴 sitemaps > 새 사이트맵 추가에 `https://{username}.github.io/sitemap.xml` 입력 후 제출 <br>
<input type="checkbox"> 제출된 사이트맵 상태 `성공` 확인
<br>
![Xixia](/assets/images/github_blog/20240822googleseopagecheck.png)

![Xixia](/assets/images/github_blog/20240822googlesitemapadd.png)

<br><br>

### 4.1 네이버(Naver)
<input type="checkbox"> [네이버 서치어드바이저](https://searchadvisor.naver.com/) 접속 <br>

<input type="checkbox"> 웹마스터 도구 > 사이트 관리 > 사이트 등록 페이지에서 `https://{username}.github.io` 입력 <br>

<input type="checkbox"> 해당 HTML 파일 {username}.github.io 하위 폴더에 저장 (이름 안바꾸고 그대로 넣으시면 됩니다) 후 git push 

![Xixia](/assets/images/github_blog/20240822naverpageconfirm.png)

<br>

<input type="checkbox"> 등록된 사이트 링크 클릭하여 사이트 관리 페이지 접속 <br>

<input type="checkbox"> 좌측 요청 > 사이트맵 제출탭에서 `https://{username}.github.io/sitemap.xml` 입력 후 제출  <br>

<input type="checkbox"> 좌측 요청 > RSS 제출탭에서 `https://{username}.github.io/feed.xml` 입력 후 제출  <br>

![Xixia](/assets/images/github_blog/20240822naversitemapconfirm.png)

<br><br>

### 4.1 다음(Daum)
<input type="checkbox"> [다음 검색등록](https://register.search.daum.net/index.daum) 접속 <br>
<input type="checkbox"> `신규등록하기` 클릭 후 `https://{username}.github.io` 입력  <br>

<br><br>

<a href="/github_blog/#">
<span style="font-weight: bold; color: #007bff; font-size: 30px;"># Next &gt;&gt;&gt;&gt;&nbsp;&nbsp;  </span>
</a>
<br>
seo 최적화 방법에 대해 알아보겠습니다!