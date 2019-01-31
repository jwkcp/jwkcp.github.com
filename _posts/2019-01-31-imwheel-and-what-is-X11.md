---
layout: post
title: 우분투(Ubuntu)에서 마우스 휠 속도 빠르게 하기. (feat. X11은 무엇인가)
tags: linux ubuntu
comments: true
---

마우스 휠 속도 빠르게 하기
===
우분투에서 마우스 휠 속도를 높이기 위해서는 imwheel이란 패키지를 설치하면 된다. 설정도 간단하다.

### 1. imwheel 설치
```
sudo apt-get install imwheel
```

### 2. 설정 파일 만들기 & 속도 입력
```
vi ~/.imwheelrc

```
그리고 아래 내용 붙여 넣기
```
".*"
None, Up,   Button4, 3 
None, Down, Button5, 3
```

### 3. 시작할 때 항상 자동으로 활성화되게 하기
```
sudo vi /etc/X11/imwheel/startup.conf
```
그리고 아래 값을 0에서 1로 바꿔주기
```
IMWHEEL_START=0
```


X11 디렉토리의 역할
===

위에 값을 설정하며 startup.conf 파일이 X11 디렉토리 하위에 있는데 이게 뭔지 궁금했다. X11이라니..
     
X11 디렉토리는 프로토콜과 GUI(Graphical User Interface) 기본값이 정의된 곳이다. 그래서 마우스 버튼과 관련 설정값을 다루는 imwheel이 여기에 설정 파일을 만들고 참조한 것이다. X는 리눅스에서 GUI를 일컫는 X Window에서 따왔고 11은 버전을 뜻한다. 그러니까 각종 시스템 설정파일이 포함된 디렉토리(/etc)아래 X11 디렉토리 아래 imwheel의 설정값이 있는게 이상해 보이지 않는다. 아니 당연한 것이다. 이렇게 알고 보니 위에 imwheel 설정 과정이 좀 친근하게 느껴지지 않는가?




