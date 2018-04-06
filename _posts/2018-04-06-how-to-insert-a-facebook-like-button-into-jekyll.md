---
layout: post
title: Jekyll에 페이스북 좋아요 버튼 만들어 넣기
tags: jekyll facebook
comments: true
---
  
---
  
## 페이스북 좋아요 버튼 만들기
1. 페이스북 [개발자센터](https://developers.facebook.com)에서 앱 생성
2. [이 페이지](https://developers.facebook.com/docs/plugins/like-button/)를 방문해 좋아요 버튼 생성 코드 획득. (아래와 같은 코드를 직접 넣어도 됨.)

**1번 코드**
~~~
# 이 코드는 body 태그의 끝 부분에 삽입
<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/ko_KR/sdk.js#xfbml=1&version=v2.12&appId=123456789098765&autoLogAppEvents=1';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>
~~~
  
**2번 코드**
~~~
# 이 코드는 좋아요 버튼을 두고 싶은 자리에 삽입
<div class="fb-like" data-href="{글주소}" data-layout="button_count" data-action="like" data-size="small" data-show-faces="true" data-share="false"></div>
~~~
  
3. 끝.

---
  
## jekyll의 모든 포스팅에 자동으로 붙게 만들기
1. 위에 **1번** 스크립트 코드에 (만약 default.html을 상속받아 사용하는 사람이라면 default.html에) 넣으면 편하다.
2. post.html 같은 포스팅 레이아웃 템플릿 파일에 **2번** 코드를 넣을 때 {글주소} 부분을 사용자가 읽고 있는 포스팅에 따라 동적으로 바뀌게 해줘야 한다. 아래와 같이 하면 된다.
(< 와 >는 { 과 } 으로 바꿔서 쓰세요.)  
~~~
data-href="<< site.url >><< page.url >>"
~~~
  
**site.url** 은 _config.yml에서 지정한 값으로 가지고 온다. (e.g. https://mydomain.com)  
**page.url** 은 현재 읽고 있는 포스팅 상대주소 값이다. (e.g. /2018/01/01/myposting.html)
  
이걸 합치면 https://mydomain.com/2018/01/01/myposting.html 이 되고 페이스북 좋아요 버튼 코드에 이 주소가 들어가게 된다.
