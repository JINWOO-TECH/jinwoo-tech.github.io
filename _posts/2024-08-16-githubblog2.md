---
layout: post
title: Github 블로그 만들기 (Hydejack 테마) - [2]
description: >
  테마 적용
categories: github_blog
tags: Github_블로그_만들기_(Hydejack테마)
---
여러 테마들이 있지만 저는 Hydejack 테마 사용를 사용하겠습니다.
(테마별 디테일이 다르긴 하지만 큰 틀에서는 동일)

그 전 Jekyll(지킬)과 Ruby(루비)가 필수입니다.
저도 블로그 만들면서 처음 보았기 때문에 chatgpt한테 도움을 받았습니다!

![Xixia](/assets/images/github_blog/20240816jekyllruby.png)

요약하자면 github 블로그는 Jekyll이 Ruby로 정적 사이트 생성하고, 로컬에서 Jekyll 서버를 실행해서 개발하면 github에 push를
하지 않고 실시간으로 결과를 확인 할 수 있기 때문에 Jekyll(지킬)과 Ruby(루비)가 필수입니다.
<br><br>

<h2>
    <span class = "jjw_h2_style">Step 1. 테마 선택</span>
</h2>

<input type="checkbox"> http://jekyllthemes.org/ 에서 테마 선택 후 다운

![Xixia](/assets/images/github_blog/20240816hydejackdownload.png)

<br><br>
<input type="checkbox"> 다운 받은 파일 전체 복붙 

![Xixia](/assets/images/github_blog/20240816hydejackpaste.png)
<br><br>

<h2>
    <span class = "jjw_h2_style">Step 2. Ruby 설치 및 인코딩 설정</span>
</h2>

Ruby는 프로그래밍 언어라고 합니다. jekyll은 ruby로 만들어져 있어 jekyll설치 전 먼저 설치하겠습니다.

저는 3.3.1-1 버전을 설치하였습니다!

<input type="checkbox"> https://rubyinstaller.org/downloads/ 사이트 접속 후 Ruby 다운로드 & 설치

![Xixia](/assets/images/github_blog/20240816rubydown.png)
<br><br>

<input type="checkbox"> ruby 명령어창 실행 및 인코딩 설정
본인의 폴더로 이동 및 인코딩

```
cd <폴더>
chcp 65001
```
![Xixia](/assets/images/github_blog/20240816rubycmdstart.png)
<br><br>

chcp 65001 명령어 입력 후 아래 사진 나오면 인코딩 설정 완료
![Xixia](/assets/images/github_blog/20240816rubyencoding.png)
<br><br>

<h2>
    <span class = "jjw_h2_style">Step 3. jekyll 설치 및 사이트 생성 </span>
</h2>

<input type="checkbox"> jekyll 및 bundle 설치 (ruby 명령어창)

```
gem install jekyll bundler
gem install webrick
```
<br>

<input type="checkbox"> jekyll생성 및 서버 동작 (ruby 명령어창)

```
# jekyll생성
jekyll new ./
 
#서버 동작
bundle install
bundle exec jekyll serve
```
<br>

<input type="checkbox"> local에서 블로그 확인 (http://127.0.0.1:4000/ 또는 http://localhost:4000/)

![Xixia](/assets/images/github_blog/20240816localhoststart.png)

<br><br>

<h2>
    <span class = "jjw_h2_style">Step 4. _config,yml 파일 수정 </span>
</h2>

```
# url 설정 안할 시 테마 적용 안됨!
url: https://<username>.github.io/

# 사이드바 타이틀 
title: JINWOO-TECH

# 사이트바 타이틀 아래 설명
description:           >
  데이터엔지니어 
```
등등 설정을 해주시면 됩니다!
<br><br>

<h2>
    <span class = "jjw_h2_style">Step 5. push후 확인 </span>
</h2>

git에 push를 해줍니다. push가 완료되어도 바로 적용이 안되서 살짝 기다려야 됩니다! (이래서 local에서 실시간으로 볼 수 있게 jekyll 사용하는 듯) 
<br>

<input type="checkbox"> https://username.github.io/ 에 접속

![Xixia](/assets/images/github_blog/20240816firststatepage.png)

<br><br>
<a href="{{ site.baseurl }}/github_blog/2024/08/16/githubblog3.html">
<span style="font-weight: bold; color: #007bff; font-size: 30px;"># Next &gt;&gt;&gt;&gt;&nbsp;&nbsp;  </span>
</a>
<br>
테마 그대로 github.io에 띄우는 방법을 알아봤는데요, 테마 별 설정하는게 조금씩 다를 수 있으니 README.md 등등 파일을 꼭 읽어 주세요! 
<br> 다음은 blog 새로운 페이지 및 사이드바 메뉴 설정에 대해 알아보겠습니다.

<br><br>

<hr style="border-width:1px 0 0 0; border-color:#000;">
<hr style="border-width:1px 0 0 0; border-color:#000;">

# 에러 처리 
### jekyll new ./  명령어 작동시 에러 처리 
![Xixia](/assets/images/github_blog/20240816jekyllnewerror.png)
<br><br>

해결 방법
> Gemfile.lock 파일 삭제 <br>
> jekyll new ./ --force 실행

![Xixia](/assets/images/github_blog/20240816jekyllnewerror2.png)
