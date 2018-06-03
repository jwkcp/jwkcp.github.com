---
layout: post
title: pycrypto 설치 시 에러가 발생하는 경우 해결 방법
tags: django pycrypto
comments: true
---
  
## 증상
pycrypto 설치 시 아래의 문구가 포함된 에러가 발생한다.  
~~~
Running setup.py bdist_wheel for pycrypto ... error

...

src/MD2.c:31:20: fatal error: Python.h: No such file or directory
  compilation terminated.
  error: command 'x86_64-linux-gnu-gcc' failed with exit status 1

  ----------------------------------------
  Failed building wheel for pycrypto
  
...

src/MD2.c:31:20: fatal error: Python.h: No such file or directory
    compilation terminated.
    error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
~~~

---

## 해결 방법
만약 파이썬3를 사용하고 있다면 이 해결 방법이 통할 가능성이 높다. 관련 도구를 설치할 때 아래와 같이 했을 것이다.

~~~
sudo apt-get install python-dev
~~~
  
하지만 파이썬3의 경우 아래 패키지를 설치한다.
  

~~~
sudo apt-get install python3-dev
~~~
