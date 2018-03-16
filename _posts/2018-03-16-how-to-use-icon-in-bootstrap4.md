---
layout: post
title: 부트스트랩4(Bootstrap4)에서 아이콘 사용하기
tags: how-to, bootstrap
comments: true
---
  
## 개요
지금까지는 부트스트랩에서 아이콘을 불러 쓸 때 Glyphicons를 이용했을 것이다. [부트스트랩4 부터는 Glyphicons이 없어(Dropped)](https://getbootstrap.com/docs/4.0/migration/#components)졌다. 대신 [이용 가능한 다른 아이콘 라이브러리 목록](https://getbootstrap.com/docs/4.0/extend/icons/)을 알려준다. 2018년 3월 현재 Iconic과 Octicons는 부트스트랩에서 테스트를 했고 나머지는 테스트하진 않았지만 선택 가능한 옵션이라고 한다.
  
여기서는 [해당 페이지](https://getbootstrap.com/docs/4.0/extend/icons/)에는 기재되어 있지 않지만 호평이 많은 [fontawesome](https://fontawesome.com) 사용법에 대해 알아본다. 다른 라이브러리도 큰 맥락은 대부분 비슷할 것이다.
  
## fontawesome 사용법
크게 4가지로 나눠볼 수 있다.  
1. 자바스크립트를 이용한 SVG (SVG with JS)
2. CSS를 이용한 웹폰트 (Web Fonts with CSS)
3. 데스크탑에서 사용하기 (Desktop Use)
4. 기타 (Advanced)
  
여기서는 사용자 환경(예컨데 글꼴 등)에 따라 아이콘이 안보이는 등의 문제가 있는 웹폰트 방식 말고, SVG(Scalable Vector Graphics)를 이용한 방식을 써본다. 요즘은 많이 웹폰트에서 SVG로 이동하고 있는 추세다.  

---
  
### 1. 불러오기
[이 사이트](https://fontawesome.com/get-started/svg-with-js)에서 다운로드를 받거나 CDN을 쓰면 된다. 
- 다운로드 받아 쓸 사람은 정적 파일 서빙하는 위치에 옮긴 후 소스에서 fontawesome-all.js를 참조하면 된다.  
- CDN 방식으로 쓸 사람은 아래 주소를 참조하면 된다.  
~~~
<script defer src="https://use.fontawesome.com/releases/v5.0.8/js/solid.js" integrity="sha384-+Ga2s7YBbhOD6nie0DzrZpJes+b2K1xkpKxTFFcx59QmVPaSA8c7pycsNaFwUK6l" crossorigin="anonymous"></script>
<script defer src="https://use.fontawesome.com/releases/v5.0.8/js/fontawesome.js" integrity="sha384-7ox8Q2yzO/uWircfojVuCQOZl+ZZBg2D2J5nkpLqzH1HY0C1dHlTKIbpRz/LG23c" crossorigin="anonymous"></script>
~~~
  
### 2. 사용하기
웹 폰트를 사용할 때와 똑같다. span 태그를 써도 되고 i 태그를 써도 된다. 사이트 예제에서 i를 쓴 건 짧기 때문에 더 편해서라고 한다.
~~~
<span class="fas fa-user"></span>

또는

<i class="fas fa-user"></i>
~~~
  
### 3. 정리
아래는 공식 사이트의 예제이다.  
~~~
<head>
    <!--load everything-->
    <script defer src="/static/fontawesome/fontawesome-all.js"></script>
</head>
<body>
    <!--user icon in two different styles-->
    <i class="fas fa-user"></i>
    <i class="far fa-user"></i>
    <!--brand icon-->
    <i class="fab fa-github-square"></i>
</body>
~~~
