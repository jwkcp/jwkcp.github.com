---
layout: post
title: 우분투(Ubuntu)에 몽고디비(MongoDB) 설치하는 법과 오류 해결 방법
tags: mongodb
comments: true
---

기본적으로 몽고디비 설치는 [mongodb.com의 공식 매뉴얼](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition-using-deb-packages)을 따르면 된다. 간단히 해당 절차를 살펴보고 쉽게 만날 수 있는 에러 메시지를 어떻게 해결하는지 확인해본다.

## 설치방법
1. 패키지의 일관성과 훼손을 막기 위해 배포자가 서명한 GPG 키 임포트하기
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

2. 몽고디비 패키지 저장소 등록하기 
(아래는 우분투 16.04이므로 다른 버전의 위의 [mongodb.com의 공식 매뉴얼](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition-using-deb-packages)을 참고할 것)
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

3. 우분투 패키지 데이터베이스 업데이트
```
sudo apt-get update
```

4. 몽고디비 설치
```
sudo apt-get install -y mongodb-org
```

5. 몽고디비 실행
```
sudo service mongod start
```

6. 몽고디비 실행 여부 확인
    6-1. 아래 경로에 로그파일이 생성되어 있는지 체크
        /var/log/mongodb/mongod.log
    6-2. 몽고디비 프로세스 확인
        ps -ef | grep mongodb

## 발생할 수 있는 에러와 해결 방법
### sudo: serivce: command not found
**sudo serivce mongod start**명령을 하면 위와 같이 명령을 찾을 수 없다고 나온다.

### Failed to start mongod.service: Unit mongod.service not found
mongod 서비스를 실행하려고 하면 서비스 시작을 실패했다고 나온다.

---
이 에러를 찾아보면 mongodb로 해야 한다거나 unmask를 해야 한다거나 재설치해보라거나 하는 등 굉장히 많은 답변이 나오는데 다 소용없다. 우분투를 쓰고 있는데 이와 같은 에러를 만난다면 몽고디비 공식 사이트 설치 매뉴얼에 있는 <중요> 항목을 눈여겨 볼 필요가 있다. 우분투에 기본적으로 mongodb가 있는데 이건 MongoDB Inc.에서 유지보수하는게 아니며 이것 때문에 충돌이 발생할 수 있으니 삭제 후 설치 과정을 진행하라는 안내다.

> The mongodb-org package is officially maintained and supported by MongoDB Inc. and kept up-to-date with the most recent MongoDB releases. This installation procedure uses the mongodb-org package. The mongodb package provided by Ubuntu is not maintained by MongoDB Inc. and conflicts with the mongodb-org package. To check if Ubuntu’s mongodb package is installed on the system, run sudo apt list --installed | grep mongodb. You can use sudo apt remove mongodb and sudo apt purge mongodb to remove and purge the mongodb package before attempting this procedure.

> mongodb-org 패키지는 MongoDB Inc가 공식적으로 유지보수와 지원, 최신 업데이트를 제공합니다. 이 설치 과정은 mongodb-org 패키지를 다루고 있습니다. 우분투에 기본으로 제공된 mongodb는 MongoDB Inc에서 유지보수하지 않고 mongodb-org 패키지와 충돌합니다. 우분투에 mongodb 패키지가 설치되어 있는지 확인하려면 sudo apt list --installed | grep mongodb 를 입력해보세요. 설치된 (우분투에서 제공한)mongodb가 있다면 이 설치 과정을 따르기 전에 sudo apt remove mongodb와 sudo apt purge mongodb 명령으로 해당 패키지를 완전히 제거할 수 있습니다.

결론적으로 위 에러는 우분투의 mongodb와 MongoDB Inc에서 제공하는 mongodb가 충돌하여 발생하는 것으로 우분투에서 제공하는 mongodb 패키지를 완전히 제거한 후 시도하면 해당 에러가 발생하지 않는다.
