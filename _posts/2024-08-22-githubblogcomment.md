---
layout: post
title: Github 블로그 만들기 (댓글 기능 추가 - utterances )
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---
**utterances**의 장점
* 오픈소스 -> 무료, 광고 없음
* 마크다운으로도 댓글 작성 가능

<br>

<h2>
    <span class = "jjw_h2_style">step 1. 댓글을 저장할  github repository 생성</span>
</h2>
<br>

<input type="checkbox"> github repository를 public으로 생성

![Xixia](/assets/images/github_blog/20240822blogcommentcreate.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">step 2. utterances install </span>
</h2>
<br>

<input type="checkbox"> [utterances](https://github.com/apps/utterances)에 접속하여 설치

![Xixia](/assets/images/github_blog/20240822utterancesfirstpage.png)
![Xixia](/assets/images/github_blog/20240822utterancespage2.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">step 3. utterances 홈페이지 접속 후 설정 </span>
</h2>
<br>

<input type="checkbox"> [utterances](https://utteranc.es/)에 접속 <br>

<input type="checkbox"> 댓글용 Repository 주소, {프로필이름/레포지토리이름}를 입력

![Xixia](/assets/images/github_blog/20240822utterancesseting.png)

<br>

<input type="checkbox"> Blog Post ↔️ Issue Mapping 설정

![Xixia](/assets/images/github_blog/20240822utterancesseting2.png)

댓글이 달린 것을 블로그 페이지의 어떤 부분과 매핑 시킬지 정하는 것
<table id="rwd-table">
  <tbody>
    <tr>
        <td data-label="Issue title contains page pathname"> 👉 page pathname를 경로를 매핑</td>
        <td data-label="Issue title contains page URL"> 👉 url 경로를 매핑</td>
        <td data-label="Issue title contains page title"> 👉 title로 경로를 매핑</td>
        <td data-label="Issue title contains page og:title"> 👉 og태그로 경로를 매핑</td>
        <td data-label="Specific issue number"> 👉 issue번호로 경로를 매핑</td>
        <td data-label="Issue title contains specific term"> 👉 특정 키워드로 경로를 매핑</td>
    </tr>
  </tbody>
</table>
저는 고유하고 수정안 할 것 같은 첫번째로 선택했습니다.

<br>

<input type="checkbox"> Issue Label 설정 (선택)
글 옆에 어떤 라벨을 붙일 것인지 작성하는 옵션

![Xixia](/assets/images/github_blog/20240822utterancesseting3.png)

<br>

<input type="checkbox"> 원하는 테마 theme 선택

![Xixia](/assets/images/github_blog/20240822utterancesseting4.png)

<br>
<br>

<h2>
    <span class = "jjw_h2_style">step 4. 설정 후 만들어진 코드 적용 </span>
</h2>

<br>
<input type="checkbox"> 코드 복사
![Xixia](/assets/images/github_blog/20240822utterancesseting5.png)

<br>


<input type="checkbox"> 댓글 구현을 원하는 html 파일에 복사한 코드를 추가<br>

댓글이 달리 원하는 html파일을 찾아서 수정해야 하는데요, 일단 저는 `F12` 개발자 도구를 사용해서 html 코드가 어디에 작성되어 있는지 찾았습니다.<br>
class 이름으로 검색했습니다. 저는 _layouts/post.html 파일을 수정해야 하는 군요! _layouts/post.html 파일 적당한 부분에 복사한 코드를 추가합니다.

![Xixia](/assets/images/github_blog/20240822utterancesfindhtml.png)

![Xixia](/assets/images/github_blog/20240822utterancesend.png)