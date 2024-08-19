---
layout: post
title: Github 블로그 만들기 (xixia 테마) - [1]
description: >
  테마 적용
categories: github_blog
tags: Github_블로그_만들기_(xixia테마)
---
github blog를 만들 때, 처음에 hydejack 테마를 사용하였는데 사이드바 메뉴 하위에 서브메뉴들이 필요할 것 같더라고요,, <br><br>
일단 찾아보다가 hydejack 테마에 서브메뉴를 추가하는게 어려울 것 같아서 테마를 바꾸기로 했습니다! <br><br>
바꾼 테마는  <a href="http://jekyllthemes.org/themes/xixia/">Xixia</a> 테마입니다! <br><br>
대충 둘러보니깐 충분히 서브메뉴를 만들 수 있을 것 같아서 삽질을 시작했습니다! <br><br>
이번 포스팅에서는 <b>메뉴바 생성 및 서브메뉴 생성</b>에 대해서만 알아보겠습니다. <br><br>
github repository 생성, 테마 입혀서 local과 github.io에 띄우는 건 hydejack 테마와 동일 하니깐 이전 포스팅을 참고해 주세요.
<br>
<a href="{{ site.baseurl }}/github_blog/2024/08/13/githubblog1.html">
<span style="font-weight: bold; color: #007bff; font-size: 15px;">github repository 생성  </span>
</a>
<br>
<a href="{{ site.baseurl }}/github_blog/2024/08/16/githubblog2.html">
<span style="font-weight: bold; color: #007bff; font-size: 15px;">테마 입혀서 local과 github.io에 띄우기  </span>
</a>

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240819newmenusubmenu.png)


<br><br>

## Step 1. _config.yml 설정

<input type="checkbox"> http://jekyllthemes.org/ 에서 테마 선택 후 다운, local에서 띄우기

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816hydejackdownload.png)

<br><br>
<input type="checkbox"> 다운 받은 파일 전체 복붙 

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816hydejackpaste.png)
<br><br>

## Step 2. Ruby 설치 및 인코딩 설정

Ruby는 프로그래밍 언어라고 합니다. jekyll은 ruby로 만들어져 있어 jekyll설치 전 먼저 설치하겠습니다.

저는 3.3.1-1 버전을 설치하였습니다!

<input type="checkbox"> https://rubyinstaller.org/downloads/ 사이트 접속 후 Ruby 다운로드 & 설치

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816rubydown.png)
<br><br>

<input type="checkbox"> ruby 명령어창 실행 및 인코딩 설정
본인의 폴더로 이동 및 인코딩

```
cd <폴더>
chcp 65001
```
![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816rubycmdstart.png)
<br><br>

chcp 65001 명령어 입력 후 아래 사진 나오면 인코딩 설정 완료
![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816rubyencoding.png)
<br><br>

## Step 3. jekyll 설치 및 사이트 생성 
<input type="checkbox"> jekyll 및 bundle 설치 (ruby 명령어창)

```
gem install jekyll bundler
gem install webrick
```
<br><br>

<input type="checkbox"> jekyll생성 및 서버 동작 (ruby 명령어창)

```
# jekyll생성
jekyll new ./
 
#서버 동작
bundle install
bundle exec jekyll serve
```
<br><br>

<input type="checkbox"> local에서 블로그 확인 (http://127.0.0.1:4000/ 또는 http://localhost:4000/)

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816localhoststart.png)

<br><br>

## Step 4. _config,yml 파일 수정

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

## Step 5. push후 확인
git에 push를 해줍니다. push가 완료되어도 바로 적용이 안되서 살짝 기다려야 됩니다! (이래서 local에서 실시간으로 볼 수 있게 jekyll 사용하는 듯)
<br>
<input type="checkbox"> https://username.github.io/ 에 접속

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816firststatepage.png)

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
![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816jekyllnewerror.png)
<br><br>

해결 방법
> Gemfile.lock 파일 삭제 <br>
> jekyll new ./ --force 실행

![Xixia]({{ site.baseurl }}//assets/images/github_blog/20240816jekyllnewerror2.png)
