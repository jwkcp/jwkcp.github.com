---
layout: post
title: 우분투 사용자 생성, 그룹추가, 삭제 명령어
tags: ubuntu
comments: true
---
  
## 사용자 생성
~~~
sudo adduser 생성할사용자명    # 홈 디렉토리까지 만들어짐
  
또는 
  
sudo useradd 생성할사용자명    # 그냥 사용자만 생성됨
~~~

---

## 사용자를 그룹에 추가
~~~
sudo gpasswd -a 추가할사용자명 sudo(추가할대상그룹)
~~~

---

## 사용자 삭제
~~~
sudo deluser 삭제할사용자명    # 사용자만 삭제, 홈디렉토리는 남아있음

또는

sudo deluser --remove-home 삭제할사용자명    # 홈디렉토리도 삭제
~~~

---

**참고자료**  
[https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-16-04)