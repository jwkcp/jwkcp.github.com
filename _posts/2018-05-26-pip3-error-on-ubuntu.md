---
layout: post
title: 우분투에서 pip3가 동작하지 않을 때
tags: ubuntu
comments: true
---
  
## 증상
sudo apt-get install python3-pip 로 pip3를 설치했으나 아래와 비슷한 에러가 발생할 때  
~~~
Traceback (most recent call last):
  File "/usr/bin/pip3", line 9, in <module>
    from pip import main
ImportError: cannot import name 'main'
~~~

---

## 해결방법
관련 해결방법을 [스택오버플로우](https://stackoverflow.com/questions/49836676/python-pip3-cannot-import-name-main-error-after-upgrading-pip)에서 찾을 수 있었다. 아래는 그 답변을 간략히 번역하고 해결방법을 옮겨놓은 것이다.  
  
*sudo pip install pip --upgrade 같은 명령으로 무심코 시스템 pip를 업그레이드 한 것이 원인입니다. pip 10.x은 내부적으로 위치해야 할 곳을 정하게 되어 있으며, pip3 명령은 pip에 의해 관리되는 파일이 아니라 패키지 관리자에 의해 제공되는 파일입니다. 더 자세한 정보는 [여기](https://github.com/pypa/pip/issues/5221)를 참고하세요.   
   
시스템 pip를 업그레이드 하지 않고 virtualenv를 사용하고 싶다면 아래의 명령으로 pip3를 복구해야 합니다.*  
  
~~~
sudo python3 -m pip uninstall pip && sudo apt install python3-pip --reinstall
~~~
  
  
