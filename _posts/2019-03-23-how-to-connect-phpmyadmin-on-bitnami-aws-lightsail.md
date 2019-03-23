---
layout: post
title: 비트나미(bitnami)로 워드프레스 설치 후 phpmyadmin 연결하는 방법
tags: wordpress
comments: true
---

비트나미(bitnami)로 워드프레스를 설치하면 보안 상 기본적으로 phpmyadmin을 localhost를 통해서만 접근하도록 제한하고 있다. 아래 명령을 터미널에서 실행하여 로컬 컴퓨터에서도 phpmyadmin을 사용할 수 있다.

** .pem 확장자의 키 파일 방식을 이용하는 경우**

```
ssh -N -L 8888:127.0.0.1:80 -i KEYFILE bitnami@SERVER-IP
```

** 비밀번호 방식을 이용하는 경우**

```
ssh -N -L 8888:127.0.0.1:80 bitnami@SERVER-IP
```

이후, 웹브라우저를 열고 localhost:8888/phpmyadmin에 접속하면 된다. 아이디 비밀번호 입력창이 나오면 아래와 같이 입력한다.

- Username: root
- Password: 비트나미 비밀번호 (모르시는 분은 서버 접속 후 /home/bitnami/bitnami_application_password를 열어보면 알 수 있습니다.)

---

> 비트나미(bitnami)란? AWS나 라이트셰일(lightsail) 또는 기타 많은 호스팅 서비스에서 비트나미로 워드프레스를 설치할 수 있는 이미지를 제공한다. (워드프레스를 사용하려면 nginx나 apache같은 웹서버, php 그리고 mysql 데이터베이스를 설치해야 하는데 비트나미 이미지를 사용하면 이 과정을 손쉽게 진행할 수 있다.)

참고링크: [https://docs.bitnami.com/virtual-machine/faq/get-started/access-phpmyadmin/#access-phpmyadmin-on-linux-and-mac-os-x](https://docs.bitnami.com/virtual-machine/faq/get-started/access-phpmyadmin/#access-phpmyadmin-on-linux-and-mac-os-x)
