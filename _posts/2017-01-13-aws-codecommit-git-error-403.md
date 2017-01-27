---
layout: post
title: AWS CodeCommit에서 Git Push 시 403 에러가 발생할 때 해결방법
tags: aws git how-to
comments: true
---
AWS의 CodeCommit을 통해 git push를 하려고 하면 아래와 같은 메시지가 나오면서 정상적으로 push되지 않는 경우가 있다.

> fetal: unable to access ‘http://git-codecommit.us-west-2.amazonaws.com/v2/repos/MyRepos/’: The requested URL returned error: 403

[![aws_codecommit_error.png](https://s26.postimg.org/d393s2em1/aws_codecommit_error.png)](https://postimg.org/image/pun9ykodx/)

---
  
### 해결방법

대부분 Mac 사용자에게서 발생하는 것으로 보이는데 이글에 첨부된 이미지와 같이 맥의 키체인 관리자의 키체인 목록에서 git-codecommit.us-west-2.amazonaws.com를 찾은 다음, 정보 보기 버튼( i 마크로 된)을 누르면 속성창이 하나 뜬다. 여기서 ‘접근제어’ 탭을 누르고 목록의 git-credential-osxkeychain를 삭제하고 창을 닫으면 된다.

이후 다시 git push를 하려고 하면 Max OS에서 키체인 허용 여부를 묻는데 이 때  ‘거부’를 클릭하면 정상적으로 푸시되는 것을 볼 수 있다.