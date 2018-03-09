---
layout: post
title: 깃(git) pull 명령 후 non-fast-forward 문제 해결방법
tags: how-to, git
comments: true
---
  
## 현상
github에서 저장소 생성 후 저장소 주소를 remote에 입력(git remote add origin https://github.....)했고, 로컬에서도 정상적으로 초기화(git init)했는데도 git pull 또는 git merge 명령이 동작하지 않고 git push origin master시 **[rejected]        master -> master (non-fast-forward)**이런 에러가 발생하는 경우

## 원인
깃허브에 생성된 원격 저장소와 로컬에 생성된 저장소 간 공통분모가 없는 상태에서 병합하려는 시도로 인해 발생. 기본적으로 관련 없는 두 저장소를 병합하는 것은 안되도록 설정되어 있음.

## 해결방법
아래와 같이 git pull 시에 **--allow-unrelated-histories** 옵션 추가하여 관련 없었던 두 저장소를 병합하도록 허용
~~~
git pull origin master --allow-unrelated-histories
~~~