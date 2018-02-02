---
layout: post
title: 잘못 푸시(push)한 파일을 깃허브에서 삭제하는 방법
tags: how-to, git
comments: true
---

### 문제  
.gitignore 파일을 만들어 무시할 목록을 작성하기 전에, 혹은 실수로 잘못 푸시(push)된 파일을 삭제할 때 github에서 강제로 직접 삭제하지 말고 아래의 순서대로 하면 된다.
  
### 방법
~~~  
1. git rm -r --cached 삭제할 파일 또는 폴더명
(잘 반영되었는 확인하려면 git status를 입력해보자.)
2. git commit -m "커밋 메시지"
3. git push origin master
~~~
  

