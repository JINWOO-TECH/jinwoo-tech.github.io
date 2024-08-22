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

![Xixia](/assets/images/github_blog/20240819newmenusubmenu.png)


<br><br>

<h2>
    <span class = "jjw_h2_style">Step 1. header.html 파일 수정 </span>
</h2>

<input type="checkbox"> _includes 폴더 하위 header.html 코드의 아래 부분을 수정하거나 추가해 줍니다

```
# 추가 할 코드
<li class="active"><a href="{{ site.baseurl }}/{원하는 메뉴명}.html">{원하는 메뉴명}</a></li>

# 위에 추가 할 코드 넣는 위치
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
    <ul class="nav navbar-nav">
        {% assign trans = site.data.values[site.lan] %}
        <li class="active"><a href="{{ site.baseurl }}/">{{ trans.home }}</a></li>
        <li class="active"><a href="{{ site.baseurl }}/github_blog.html">{{ trans.github_blog }}</a></li>
        <li class="active"><a href="{{ site.baseurl }}/archive.html">{{ trans.archive }}</a></li>
        <li class="active"><a href="{{ site.baseurl }}/categories.html">{{ trans.categories }}</a></li>
        <li class="active"><a href="{{ site.baseurl }}/tags.html">{{ trans.tags }}</a></li>
        <li class="active"><a href="{{ site.baseurl }}/about.html">{{ trans.about }}</a></li>
    </ul>
</div>
```
<br>
코드를 수정했으면 상단 메뉴에 추가한 메뉴가 생성이 됩니다!

<br><br>

<h2>
    <span class = "jjw_h2_style">Step 2. 원하는 메뉴.html 파일 생성 및 내용추가  </span>
</h2>

"원하는 메뉴.html"은 메뉴를 클릭하여 페이지로 이동 하였을 때 나오는 화면입니다.
<br><br>
<input type="checkbox"> username.github.io 폴더 하위에 {원하는 메뉴명}.html 파일 생성해 줍니다 <br>
(위치 : username.github.io/{원하는 메뉴명}.html)
<br><br>

<input type="checkbox"> {원하는 메뉴명}.html 수정<br><br>
저는 일단 기존의 categories, tags 메뉴처럼 나오길 만들기를 원했습니다. 
그래서 _post 하위에 있는 페이지들이 어떻게 사용되는지 확인했습니다. 
아래 처럼 tags: {tag명} , categories: {categories 명}으로 되어 있었습니다.

```
---
layout: post
title:  "Readme of Jekyll!"
date:   2017-09-05 14:10:51 +0800
categories: Jekyll
tags: Jekyll
description: The read me page of Jekyll.
---
```

<br> 그래서 당연히 아래 처럼 github_blog: 태그명 이런식으로 사용하면 되겠구나 했습니다. 그리고 "원하는 메뉴.html"에서 github_blog로 받으면 되겠구나 했습니다. <br>

하지만 바로 에러가 나왔습니다.. "Liquid Exception: Liquid error (line 2): Cannot sort a null object. in github_blog.html" 이런 에러가 나왔습니다. <br>

site.github_blog식으로 사용하려면 변수가 정의가 되어 있어야 한다는데 여러가지 방법 시도하다가 변수를 정의하는 것은 실패하고 다른 방법으로 분류하기로 했습니다. 

```
# _post 하위에 작성한 페이지 (yyyy-mm-dd-title.md)
---
layout: post
title:  "Readme of Jekyll!"
date:   2017-09-05 14:10:51 +0800
categories: Jekyll
tags: Jekyll
github_blog: first_github
description: The read me page of Jekyll.
---

# "원하는 메뉴.html" 에러난 부분 
{% raw %}
{% assign sortedgithubbloges = site.github_blog | sort %}
{% endraw %}

```
<br>

<div>
    <span style="background-color: yellow;">나만의 메뉴 분류 rule</span>
</div>


> 1. categories는 메뉴에 따라 분류할 수 있도록 작성
> 2. tags는 메뉴에 따라 분류 후 그 안에 소제목

![Xixia](/assets/images/github_blog/20240820menurole.png)

<br><br>
이런 룰을 바탕으로 원하는 메뉴.html을 만들었습니다.<br>
아래 코드에서 {categories 메뉴}만 알맞게 수정하면 됩니다.

```
<div class="well article">
{% raw %}
  {% assign github_blog_posts = site.posts | where: "categories", "{categories 메뉴}" %}
  {% assign tags = "" | split: "" %}

  {% for post in github_blog_posts %}
    {% assign post_tags = post.tags %}
    {% for tag in post_tags %}
      {% unless tags contains tag %}
        {% assign tags = tags | push: tag %}
      {% endunless %}
    {% endfor %}
  {% endfor %}

  {% assign sorted_tags = tags | sort %}

  {% for tag in sorted_tags %}
    <h2 id="tag-{{ tag | slugify }}">{{ tag | capitalize }}</h2>
    <ul>
      {% for post in github_blog_posts %}
        {% if post.tags contains tag %}
          <li>
            <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
            <a href="{{ site.baseurl}}{{ post.url }}">{{ post.title }}</a>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  {% endfor %}
 {% endraw %}
</div>
```


<br><br>
<a href="{{ site.baseurl }}/github_blog/2024/08/19/githubblogmarkdown.html">
<span style="font-weight: bold; color: #007bff; font-size: 30px;"># Next &gt;&gt;&gt;&gt;&nbsp;&nbsp;  </span>
</a>
<br>
다음번에는 페이지를 생성할 때 자주 쓰는 markdown 문법들을 소개하겠습니다. 

