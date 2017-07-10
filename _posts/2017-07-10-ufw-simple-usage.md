---
layout: post
title: ufw 간단한 설치, 사용법
tags: how-to, raspberrypi
comments: true
---

#### **1. 설치**
sudo apt-get install ufw
#### **2. 활성화**
sudo ufw enable
#### **3. 상태보기**
sudo ufw status
#### **4. 규칙 추가**
sudo ufw allow from 192.168.0.10 to any port 22 proto tcp
#### **5. 규칙 삭제**
sudo ufw delete allow from 192.168.0.10 to any port 22 proto tcp

---
[공식 문서를 통해 ufw에 대한 더 많은 정보 보기](https://help.ubuntu.com/community/UFW)   
