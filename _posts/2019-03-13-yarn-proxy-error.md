---
layout: post
title: yarn으로 패키지 추가 시 proxy 관련 에러가 발생하는 경우 해결 방법
tags: yarn javascript
comments: true
---

## 문제
yarn으로 패키지 설치를 하려고 할 때 갑자기 아래와 같은 에러가 발생하면서 안되는 경우가 있다.   
    
```
yarn add v1.x.x
info No lockfile found.
[1/4] Resolving packages...
info There appears to be trouble with your network connection. Retrying...
info There appears to be trouble with your network connection. Retrying...
info There appears to be trouble with your network connection. Retrying...
```
   
## 원인
yarn이 저장소를 찾을(lookup) 때 뭔가 문제가 생겼다는 말이다.    
(yarn의 timeout 시간을 늘리라는 답변도 많은데 나의 경우에는 해당되지 않았다. 이 경우는 진짜 네트웍 속도가 엄청 느린 환경의 사용자일 가능성이 높다고 생각한다. 한국에 있는 대부분의 사용자는 해당 사항이 없지 않을까.)
     
간단히 wget이나 curl 등으로 아래와 같이 yarn 저장소의 키를 가져오는 테스트를 해보자.   

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg
```

그리고 ```curl: (7) Failed to connect to 127.0.0.1 port 8888: 연결이 거부됨``` 에러가 발생한다면 중간에 127.0.0.1:8888로 연결 요청을 돌리는 프록시 설정이 되어 있을 가능성이 높다.

---

```env | grep proxy``` 명령으로 지금 환경에 설정된 프록시 값을 볼 수 있다.


## 해결방법
1. ~/.bashrc에서 proxy 설정을 찾아 삭제한다.(이게 왜 여기 들어가게 된지 모르겠지만 나의 경우에는 도커를 띄우다 설정된 것 같다.) 
2. ```sh ~/.bashrc```로 환경을 갱신 후 터미널을 닫고 다시 yarn 을 실행해본다. (그냥 컴퓨터를 재시작해도 된다.)
3. 끝.

