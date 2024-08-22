---
layout: post
title: Github 블로그 만들기 (seo 적용)
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---


ad - https://seungwubaek.github.io/blog/google_adsense/
SEO 소개 -  https://yenarue.github.io/tip/2020/04/30/Search-SEO/


## step 1. sitemap.xml 생성하기
검색엔진에서 Github 블로그의 글들을 긁어올 수 있도록, 사이트의 모든 페이지 정보를 모아두는 `sitemap.xml`이 필요합니다. 
새로운 페이지를 생성할 때마다 매번 추가하는 건 어려운 일이니 자동으로 추가가 되도록 설정해 줍니다 <br>

<input type="checkbox"> {username}.github.io 하위 폴더에 sitemap.xml을 생성한 후 
[sitemap.xml 링크](https://github.com/JINWOO-TECH/jinwoo-tech.github.io/blob/main/sitemap.xml)를 복사 후 git에 push 합니다


<input type="checkbox">  push 완료후 아래와 같이 url에 {username}.github.io/sitemap.xml을 확인합니다.

![Xixia](/assets/images/github_blog/20240822sitemap.png)
<br><br>

## step 2. robot.txt 생성하기
사이트를 검색엔진에 등록하면 검색엔진의 크롤러가 사이트를 크롤링합니다. robot.txt는 크롤러가 접근 할 수 있는 URL을 알려줍니다.

<input type="checkbox"> {username}.github.io 하위 폴더에 robot.txt을 생성한 후 아래 코드를 붙여넣습니다. <br>
(sitemap의 주소는 자신의 블로그 URL로 수정)

~~~
User-agent: *
Allow: /
Sitemap: https://{username}.github.io/sitemap.xml
~~~
<br><br>

## step 3. feed.xml 생성하기
feed.xml는 웹사이트의 RSS 또는 Atom 피드를 제공하는 XML 파일입니다. 이 파일은 사이트의 최신 콘텐츠를 구독할 수 있도록 도와주는 기능을 합니다.

<input type="checkbox"> {username}.github.io 하위 폴더에 feed.xml을 생성한 후 
[feed.xml 링크](https://github.com/JINWOO-TECH/jinwoo-tech.github.io/blob/main/feed.xml)를 복사 후 git에 push 합니다.

<br><br>

## step 4. 검색엔진 등록하기

### 4.1 구글(Google)
<input type="checkbox"> [Google Search Console](https://search.google.com/search-console/welcome) 접속 후 URL 접두어에 https://{username}.github.io 입력
<br>
![Xixia](/assets/images/github_blog/20240822googleseopage.png)
<br><br>
<input type="checkbox"> 해당 HTML 파일 {username}.github.io 하위 폴더에 저장 후 git push
<br>
![Xixia](/assets/images/github_blog/20240822googleseopageconfirm.png)