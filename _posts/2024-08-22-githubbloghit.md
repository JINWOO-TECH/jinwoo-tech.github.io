---
layout: post
title: Github 블로그 만들기 (방문자 수 추가 )
description: >
  테마 적용
categories: github_blog
tags: github_블로그_만들기_공통
---
**hit**이란?
* 특정 URL이 얼마나 많이 새로고침 되어있는지를 알려주는 API

<br>

<h2>
    <span class = "jjw_h2_style">step 1. 옵션 설정 후 코드 복사 </span>
</h2>
<br>
<input type="checkbox">  [Hits](https://hits.seeyoufarm.com/)에 접속
<br>
<input type="checkbox">  옵션 채우기

![Xixia](/assets/images/github_blog/20240823hits1.png)

<table id="rwd-table">
  <tbody>
    <tr>
        <td data-label="TARGET URL"> 측정할 사이트 URL</td>
        <td data-label="ADD ICON"> hits 좌측에 띄울 아이콘 설정


</td>
        <td data-label="BORDER"> 둥근 모서리, 각진 모서리 설정 </td>
        <td data-label="TITLE BG COLOR"> hits가 쓰인 공간의 배경색 설정</td>
        <td data-label="ICON COLOR">ADD ICON에서 추가한 icon의 색상 설정</td>
        <td data-label="TITLE"> hits 대신 들어갈 문구 설정</td>
        <td data-label="COUNT BG COLOR"> 0/0 에 해당하는 구역의 배경색 설정 </td>
    </tr>
  </tbody>
</table>

<h2>
    <span class = "jjw_h2_style">step 2. 코드 복사 후 원하는 페이지에 붙이기 </span>
</h2>
<br>
<input type="checkbox">  코드 복사
![Xixia](/assets/images/github_blog/20240823hits2.png)

<br><br>
저는 **'Top Posts' 자리와 게시물**마다 방문자 수를 나타내겠습니다

<input type="checkbox">  'Top Posts' 위치에 코드 수정 및 방문자 코드 삽입 <br>

Top Posts 수정은 `_includes/sidebar.html`에서 할 수 있었습니다
![Xixia](/assets/images/github_blog/20240823hits3.png)
![Xixia](/assets/images/github_blog/20240823hits4.png)
![Xixia](/assets/images/github_blog/20240823hits5.png)

<br><br>
<input type="checkbox">  게시물마다 방문자 수를 나타내기<br>
`_layous/post.html`에서 할 수 있었습니다. 게시물마다 count할 수 있도록 코드를 수정했습니다. 해당 코드를 post.html의 원하는 자리에 붙여넣으시면 됩니다

~~~html
<a href="https://jinwoo-tech.github.io/"target="_blank">
<img
    src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23293786&title_bg=%23555555&icon=micro-dot-blog.svg&icon_color=%23E7E7E7&title=Visitors&edge_flat=false"/>
</a>
~~~

![Xixia](/assets/images/github_blog/20240823hits6.png)
![Xixia](/assets/images/github_blog/20240823hits7.png)


