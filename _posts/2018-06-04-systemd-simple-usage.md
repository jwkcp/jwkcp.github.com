---
layout: post
title: 우분트에서 systemd 간단 사용법
tags: ubuntu systemd
comments: true
---
  
## 모든 서비스 조회하기
~~~
sudo systemctl list-units

# 또는 상태별 조회하기
# 서비스 상태(failed, running, exited, plugged)
  
sudo systemctl list-units --state=failed
~~~

---

## 활성화 및 시작
~~~
sudo systemctl enable 내서비스이름
sudo systemctl start 내서비스이름
~~~

---

## 종료 및 비활성화
~~~
sudo systemctl stop 내서비스이름
sudo systemctl disable 내서비스이름

---

## 불필요한 서비스 삭제
~~~
sudo systemctl reset-failed
~~~