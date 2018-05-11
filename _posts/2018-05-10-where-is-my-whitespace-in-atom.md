---
layout: post
title: 아콤(Atom) 에디터에서 자꾸 공백이 사라질 때 해결 방법
tags: editor
comments: true
---
  
---
  
아톰 에디터를 쓰다보면 마크다운의 개행을 위해 빈줄에 공백 문자 2개를 넣을 때가 있는데 다시 보면 저절로 삭제되어 있는 경우를 만날 수 있다. 이것은 아톰 에디터의 기본 설정 때문이다. 아래 경로에서 'Ignore whitespace only lines'에 체크해주면 빈줄의 공백이 자동으로 삭제되지 않는다.
  
> Settings(Cmd + ,) -> Packages -> Core Packages
  
설정 거의 마지막에 있으니 참고!
  
