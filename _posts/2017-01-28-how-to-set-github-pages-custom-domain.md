---
layout: post
title: 지킬(Jekyll)을 이용한 깃허브(Github) 페이지 커스텀 도메인 설정하기
tags: jekyll github domain
comments: true
---
지킬(Jekyll)을 이용해 깃허브 페이지로 블로그 생성하게 되면 '사용자아이디.github.io'이 기본 도메인이 된다. 만약 'tech.개인도메인'처럼 설정하고 싶다면 아래와 같이 2가지만 설정하면 된다. 여기서는 mydomain.com을 가지고 있고 abc.mydomain.com을 깃허브 페이지에 연결한다고 가정하고 예를 들겠다.
 
1. 도메인을 구매한 곳에서 CNAME 설정   
> 예) CNAME Name에 abc 입력, Value에 '사용자아이디.github.io' 입력

2. 깃허브 사이트에서 해당 저장소로 이동 후 Settings 탭 클릭, Custom Domain 항목 설정
> 예) Custom domain값을 abc.mydomain.com 입력

(Optional)3. 구글 검색이 잘 되도록 등록하려면 Google Search Console에서 sitemap.xml을 제출해야 한다. [여기](http://dveamer.github.io/homepage/Sitemap.html "여기")를 참고해 sitemap.xml을 추출하는 코드를 작성한 후 Google Search Console의 'SITEMAP 추가/테스트'를 눌러 Sitemap을 제출하면 된다. 이때 _config.yml에서 설정된 url이 위에 1, 2번 과정에서 바꾼 url과 같은지 확인하고 같지 않다면 _config.yml의 값을 바꿔준다. 제대로 바뀌었다면 Google Search Console의 Sitemap 테스트가 문제없이 통과될 것이다.

여기까지 마치면 abc.mydomain.com을 입력하면 깃허브 페이지로 연결되는 것을 확인할 수 있다.
 
 