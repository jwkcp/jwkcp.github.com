---
layout: post
title: Git merge 파일 비교를 쉽게 하도록 도와주는 툴
tags: git
comments: true
---

## 개요
git merge 시 충돌 등으로 인해 두 파일을 비교해야 할 때가 있는데 이를 좀 더 보기 쉽게 도와주는 툴이다. 비교하는 툴은 여러 종류가 있고 아래 명령으로 그 목록을 확인할 수 있다. 여기서는 diffmerge라는 툴의 간단한 설치 사용법을 알아본다.
   
~~~
git mergetool --tool-help
~~~
         
## 사용법
1. diffmerge 설치 ([https://sourcegear.com/diffmerge/downloads.php](https://sourcegear.com/diffmerge/downloads.php))
2. 아래의 명령으로 사용
       
~~~
git mergetool --tool=diffmerge
~~~
   
