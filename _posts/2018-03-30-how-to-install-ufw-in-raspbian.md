---
layout: post
title: 라즈베리파이에서 ufw와 같은 패키지 설치가 실패한다면?
tags: how-to, raspberrypi
comments: true
---
  
아래 명령으로 패키지를 검색하고 내려받을 소스가 어떻게 되어 있는지 확인해보자.  
~~~
cat /etc/apt/sources.list
~~~
  
주석 처리되어 있는 "deb-src ..." 부분이 있을텐데 주석을 해제해준다. 그리고 ufw를 설치해보면 정상적으로 설치되는 걸 확인할 수 있다. sudo apt-get update도 이 주석을 해제한 후 사용이 가능하다.

  