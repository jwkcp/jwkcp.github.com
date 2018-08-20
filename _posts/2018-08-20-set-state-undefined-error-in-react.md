---
layout: post
title: 리액트에서 "TypeError Cannot read property 'setState' of undefined" 가 발생할 때
tags: react
comments: true
---

# 문제
this.setState 함수를 쓰는 도중 갑자기 "TypeError: Cannot read property 'setState' of undefined" 이런 에러가 발생할 때가 있다. 이상하다. 문제가 생길 만한 이유가 없는데.. 
      
# 원인
원인은 호출하는 함수가 bind되지 않아서 그렇다.

onChange에 연결할 함수로 this.handleChange를 지정했다면 this.handleChange 함수는 아래 두 가지 방법 중 한 가지 방법으로 구현되어 있어야 한다.

### 방법1 - 생성자에서 바인딩 해주기
~~~
this.handleChange = this.handleChange.bind(this);
~~~

### 방법2 - 화살표 함수로 구현
~~~
handleChange = (props) => {
  ...
}
~~~
      
---
     
화살표 함수로 구현하면 자동으로 바인딩이 되기 때문에 생성자에서 명시적으로 바인딩을 해주지 않아도 된다. 달리 말해 화살표 함수를 쓰지 않는다면 생성자에서 바인딩을 해줘야 한다. 별것 아니지만 리액트 문법에 익숙하지 않는 사람들이 흔히 겪는 실수다.
        