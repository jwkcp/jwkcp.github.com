---
layout: post
title: vscode에서 jsx 확장자 emmet 활성화하기
tags: vscode emmet jsx
comments: true
---

## 문제
vscode에는 기본적으로 [emmet 기능](https://code.visualstudio.com/docs/editor/emmet)이 활성화되어 있다. 확장자가 html인 파일을 열고 ul>li 와 같이 타이핑해보면 제안이 툴팁으로 뜨면서 바로 사용 가능함을 확인할 수 있다. 하지만 jsx 확장자 파일을 편집할 경우 이 기능이 동작하지 않는다.
  
## 해결방법
아래와 같이 vscode 설정(cmd + ,)을 열어 항목을 추가하면 된다. (설정만 저장(cmd + s)하고 편집기를 재시작하지 않아도 바로 적용됨.)
  
~~~
"files.associations": {
    "*.js": "javascriptreact"
}
~~~
  
