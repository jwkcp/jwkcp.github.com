---
layout: post
title: 깃허브 저장소 최초 사용, 설정 방법
tags: git
comments: true
---
  
---
  
아래는 깃허브 저장소를 처음 초기화하면 나오는 안내이다. 다음에 사용할 때 참고하기 위해 옮겨둔다.  
  
## 커맨드라인에서 새로운 저장소 생성할 때
~~~
cho "# deeplearning" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/jwkcp/deeplearning.git
git push -u origin master
~~~
  
## 커맨드라인에서 기존 저장소에 푸시할 때
~~~
git remote add origin https://github.com/jwkcp/deeplearning.git
git push -u origin master
~~~
  