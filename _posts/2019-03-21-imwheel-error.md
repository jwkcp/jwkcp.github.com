---
layout: post
title: imwheel사용 시, 우분투(Ubuntu)에서 휠 스크롤이 갑자기 동작하지 않을 때
tags: ubuntu
comments: true
---

## 문제
우분투에서 휠 스크롤이 갑자기 동작하지 않거나, 휠을 누르고 스크롤 할 때만 조금씩 동작한다.
    
## 해결방법
imwheel을 재실행한다.

1. ```ps -ef | grep imwheel```로 프로세스가 떠 있는지 확인하고 (선택사항)
2. ```imwheel --kill```로 imwheel 프로세스를 죽인다.
3. ```imwheel```을 입력해 재실행한다.
     
이렇게 하면 휠이 다시 정상적으로 동작하는 것을 볼 수 있다.
    