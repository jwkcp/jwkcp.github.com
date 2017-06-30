---
layout: post
title: 비트나미(bitnami)로 워드프레스 설치 후 초기 비밀번호 찾기
tags: wordpress, bitnami, how-to
comments: true
---

아마존 라이트 세일이나 기타 호스팅 서비스를 통해 워드프레스를 설치하다 보면 [비트나미(bitnami)]("https://bitnami.com/")를 통해 진행하는 경우가 종종 있다. 이때 /admin으로 접속할 초기 아이디, 비밀번호를 잘 몰라 헤매는 경우가 있는데 아래의 위치에서 초기 비밀번호를 찾을 수 있다. (예전에는 user / bitnami 로 접속할 수 있었지만 보안 상으로 이유로 변경된 것 같다.)
    
### SSH로 접속
아래 화살표로 표시된 아이콘을 누르면 팝업창이 뜨면서 검정 바탕화면에 하얀색 글씨가 있는 화면이 나온다. (혹은, Connect using SSH라고 써진 주황색 버튼을 눌러도 된다.)

[![bitnami-lightsail.png](https://s26.postimg.org/5099xec8p/bitnami-lightsail.png)](https://postimg.org/image/n32com839/)
---    
![ls-tutorial-image210-7749d305.png](https://docs.bitnami.com/images/img/platforms/aws/ls-tutorial-image210-7749d305.png)
*[이미지 출처: Bitnami webpage](https://docs.bitnami.com/aws/get-started-lightsail/)*

---

### 비밀번호 보기
아래의 이미지와 같이 cat bitnami application password 라고 치면 랜덤하게 생성된 비밀번호가 나온다. 이 패스워드를 /admin 으로 접속 후 아이디 user와 함께 입력하면 로그인이 된다.

![ls-tutorial-image212-5285f948.png](https://docs.bitnami.com/images/img/platforms/aws/ls-tutorial-image212-5285f948.png)

---


비트나미(bitnami)의 공식 문서는 [여기](https://docs.bitnami.com/aws/get-started-lightsail/)를 참고하세요.