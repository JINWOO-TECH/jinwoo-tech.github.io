---
layout: post
title: github 블로그 만들기 (markdown 문법)
description: >
  이미지 추가, 코드 박스, 글 크기 등등
categories: github_blog
tags: markdown_문법
---

> 이번에는 이미지 추가, 코드 박스, 글 크기 등등 페이지를 만들 때 사용하는 markdown 문법들에 대해 소개하겠습니다.

<h2>Step 1. header </h2>
사이드바 메뉴는 _featured_tags 폴더 하위에 위치하고 있습니다. _featured_tags 폴더 아래 *.md 파일로 작성하면 됩니다. <br>
가장 중요한 것은 menu 값을 true 설정하는 것 입니다. true로 설정해야 사이드 메뉴에 나옵니다 
<br><br>

*.md 예시)
```
--- 
layout: list
title: Github 블로그 만들기 for window
slug: Github_blog_window
menu: true
order: 2
description: >
  나만의 Github 블로그 만들기 (Hydejack 테마 사용)
---
```
> layout : 사이드바 메뉴인 경우 주로 list 사용 <br>
> title : 사이드바 메뉴 이름 <br>
> slug :  하위 페이지 묶을 tag명<br>
> menu : 사이드바 메뉴 설정 여부, true로 설정 <br>
> order : 사이드바 메뉴 위치 1인 경우가 제일 상위에 위치함 <br>
> description : 사이드바 메뉴 설명 

<br>
<img src = '/assets/img/20240819featuredtags.png' style="height:500px;">
<img src = '/assets/img/20240816sidebarmenudescription.png'>

<h2>Step 2. 페이지 추가</h2>
페이지 추가는 _posts 하위 폴더에 yyyy-mm-dd-pagename.md 형태로 파일을 생성합니다.

yyyy-mm-dd-pagename.md 예시)
```
---
layout: post
title: github 블로그 만들기 - [1]
description: >
  github repository 생성 및 USERNAME.github.io에서 블로그 확인하기
tags: [Github_blog_window]
---
```
> layout : 주로 post 사용 <br>
> title : 페이지 이름 <br>
> description : 페이지 설명 <br>
> tags : 상위 페이지에 해당하는 slug 명 <br>
> -> 중복 사용 가능 <br>
> 예) [Github_blog_window, data_engineer]

<br>
<img src = '/assets/img/20240819postfolder.png' style="height:500px;">
<img src = '/assets/img/20240819postpagedesciption.png'>



<h1> Next >></h1>
다음번에는 페이지를 생성할 때 자주 쓰는 markdown 문법들을 소개하겠습니다. 