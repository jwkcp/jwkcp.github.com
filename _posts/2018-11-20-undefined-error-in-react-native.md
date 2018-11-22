---
layout: post
title: 리액트 네이티브(react-native)에서 undefined ... 에러가 발생할 경우
tags: react-native
comments: true
---

아래와 같은 에러가 발생하면다면 컴포넌트(component)에 this 키워드를 써서 발생하는 경우가 많으니 제일 먼저 확인해보세요.  


> TypeError: undefined is not an object

아시는 것처럼 컴포넌트는 클래스를 사용하지 않고 내부적으로 상태(state)를 다루지 않습니다. 그래서 클래스 내부에서 사용하는 this도 사용할 수 없지요. 그런데 이런 컴포넌트에서 props를 받을 때 실수로 this.props 라고 쓰는 경우가 종종 있습니다. 이 때 이런 에러가 발생할 수 있습니다.  
  
리액트 네이티브는 에러 메시지가 구체적이지 않아 오타 하나에도 시간을 많이 잡아먹게 되는군요..
