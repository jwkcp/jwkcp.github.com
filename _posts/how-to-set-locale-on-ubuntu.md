---
layout: post
title: 우분투에서 locale(이하 '로케일') 설정하기
tags: ubuntu setting
comments: true
---
  
# 문제
sudo apt-get -y install supervisor 와 같이 어떤 패키지를 설치할 때 로케일 에러가 발생하는 경우가 있다. 이 때 아래 내용을 참고하여 자신에게 맞는 로케일 환경을 설정해주면 해결 가능하다.  
    
# 방법
## 로케일 생성과 기본 로케일 설정
아래의 방법으로 로케일을 생성하고 시스템에서 사용할 기본 로케일을 설정할 수 있다.

**로케일 생성**  
~~~
sudo dpkg-reconfigure locales 명령 후 첫 번째 UI에서 선택
 
또는
 
sudo vi /etc/locale.gen 에서 사용할 로케일 주석 제거
sudo locale-gen 입력

또는

sudo locale-gen ko_KR.UTF-8 이렇게 개별적으로 설치할 수도 있다.
~~~
  
**기본 로케일 설정**
~~~
sudo dpkg-reconfigure locales 명령 후 두 번째 UI에서 선택

또는

sudo vi /etc/default/locale 에서 사용할 로케일 직접 입력
~~~

---
  
## 로케일 확인
~~~
locale

또는

locale -a (사용 가능한 로케일만 보기)
~~~
