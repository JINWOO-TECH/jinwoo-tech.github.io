---
layout: post
title: github 블로그 만들기 - [1]
description: >
  github repository 생성 및 USERNAME.github.io에서 블로그 확인하기
tags: [Github_blog_window]
---

<h2>Step 1. github Repository 생성</h2>

<input type="checkbox"> 개인 git repository 접속 및 repository 생성 <br>(Repository name : username.github.io 의 형식) 
<br><br>

<div>
    <img src = '/assets/img/20240813creategitrepository.png' style="display:inline; width:45%; height:120px;">
    <img src = '/assets/img/20240813gitrepositoryname.png' style="display:inline; width:45%; height:120px;">
</div>


<h2>Step 2. github clone 하기</h2>

Github 홈페이지에서 직접 코드를 추가하고 수정하는 것도 가능하지만 본인 PC의 VSCode, 파이참, 인텔리제이 등 과 같은 통합 개발 환경에서 
작업하는게 더 편리하기 때문에 원격 저장소(Github)와 로컬 PC의 통합 개발 환경을 연결하는 과정이 필요하는데 이를 clone이라고 함.

<input type="checkbox"> code버튼 클릭 후 URL 복사  
<br><br>

<img src = '/assets/img/20240814gitrepositoryclone.png' > 
<br><br>

<input type="checkbox"> 원하는 폴더 위치에서 git clone

```
cd <원하는 폴더>
git clone <url>
```

<h2>Step 3. 임시 HTML 파일 생성</h2>
<input type="checkbox"> index.html 파일 생성 및 원하는 텍스트 삽입,  github에 push

```
git add .
git commit -m "<커밋 메시지>"
git push
```
<img src = '/assets/img/20240816hellogithub.png' > 


<h2>Step 4. 페이지 확인 </h2>
<input type="checkbox"> "https://username.github.io/" 접속
<img src = '/assets/img/20240816hellopage.png' > 

<h1> Next >></h1>
블로그에 적용할 테마를 찾고 적용하는 방법을 알아보겠습니다!




