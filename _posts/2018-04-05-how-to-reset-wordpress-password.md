---
layout: post
title: 워드프레스 로그인 비밀번호 DB에서 초기화(변경)하는 방법
tags: wordpress
comments: true
---
  
워드프레스의 사용자 테이블은 (별도로 접두어 변경을 하지 않았다면) wp_users 이다. 여기서 user_pass 컬럼을 변경하면 되는데 비밀번호가 암호화되어 저장되기 때문에 MD5('비밀번호') 함수를 이용한다.   
  
~~~
UPDATE WP_USERS SET USER_PASS=MD5('새로운비밀번호') WHERE USER_LOGIN = '변경대상아이디'
~~~