---
layout: post
title: 워드프레스에서 비밀번호를 잊어버렸을 때 초기화 방법
tags: wordpress
comments: true
---

## 문제
워드프레스 설치 후 비밀번호를 잊어버렸는데 메일 서버 설정까지 안해뒀다면 비밀번호 찾기 메일이 안온다.    
    
## 해결방법
이럴 땐 phpmyadmin으로 접속해서 바꾸는게 제일 편하다. AWS Lightsail과 같이 로컬 호스트(127.0.0.1)에서만 접근 가능하도록 한 곳은 [여기](https://jwkcp.github.io/2019/03/23/how-to-connect-phpmyadmin-on-bitnami-aws-lightsail/)를 참고해 터미널 명령 한 줄만 쳐주면 접속이 가능하다.     
      
**순서**
1. phpmyadmin 접속
2. wordpress 설치 DB에서 wp_users 테이블에서 비밀번호 변경을 원하는 계정을 선택하고 '수정' 클릭
3. user_pass에 '함수' 항목을 **md5**로 선택하고, '값'에 원하는 비밀번호를 평문으로 그냥 입력
4. 하단에 '실행' 버튼 클릭
     
이렇게 하면 평문으로 입력한 비밀번호가 md5로 해시되어 입력되어 있을 것이다. 이제 입력한 평문 비밀번호로 로그인하면 끝.
     
      
