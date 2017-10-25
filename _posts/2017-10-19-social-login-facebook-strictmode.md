---
layout: post
title: 페이스북 소셜 로그인 사용 시 오류가 발생하는 경우
tags: facebook, social_login
comments: true
---

페이스북 소셜 로그인 앱을 생성 후 소셜 로그인을 시도하면 아래와 같은 메시지가 나오며 오류가 발생할 때가 있다.

### 증상

> URL을 읽어들일 수 없음: 앱 도메인에 포함되어 있지 않은 URL입니다. 이 URL을 읽어들이려면 앱 설정에서 앱 도메인 필드에 앱의 모든 도메인과 서브 도메인을 추가하세요.

### 원인   

분명 예전에 문제없이 동작했었는데 뭔가 이상하다 싶다면 페이스북 소셜 로그인에 새로 생긴 설정 'Strict 모드'를 살펴봐야 한다.
좌측 메뉴 중 '제품 > Facebook 로그인 > 설정'에 들어가면 **리디렉션 URI에 Strict 모드 사용** 이란 옵션이 있다. 이 설정 활성화 여부에 따라 **유효한 OAuth 리디렉션 URI** 에 넣는 주소가 변하지 않더라도 오류가 발생했다가 안했다가 하기 때문이다.

### 해결 방법
예를 들어, 로그인 주소가 http://localhost:8000/accounts/login 라고 가정했을 때, 'Strict 모드'사용 여부에 따른 '유효한 OAuth 리디렉션 URI 값'을 설정은 아래와 같다.

1.리디렉션 URI에 Strict 모드 사용: **활성화**

(오류) http://localhost:8000   
(성공) http://localhost:8000/accounts/login

2.리디렉션 URI에 Strict 모드 사용: **비활성화**

(성공) http://localhost:8000   
(성공) http://localhost:8000/accounts/login

### 결론   
'Strict 모드'를 비활성화하고 2번 방벙을 사용하거나 'Strict 모드'를 활성화하고 1번 방법을 사용한다.   
페이스북에서는 1번 방법을 권장하고 있다.

--

*'Strict 모드'에 관한 페이스북 문서는 [여기](https://developers.facebook.com/docs/facebook-login/security/#surfacearea) 참고*
