---
layout: post
title: 리덕스 todos 예제 구조 한눈에 보기
tags: redux
comments: true
---
           
[리덕스 공식 사이트의 예제 페이지](https://redux.js.org/introduction/examples)에 보면 Todos라는 예제가 있다. 단순 리덕스를 사용하는 것을 넘어 react-redux와 함께 컨테이너, 컴포넌트와 함께 사용하는 첫 예제인데 처음 보면 그 구조가 잘 그려지지 않는다. 잘게 나누어진 폴더와 파일들이 머릿속을 어지럽히기 때문이다. Todos 예제의 이해하기 쉽게 그림으로 그려본 것이다. 소스 구조는 [여기](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/todos)를 누르면 바로 볼 수 있다.
    
## 그림으로 보는 프로젝트 구조    
[![redux-todos-image.png](https://i.postimg.cc/TwdkQ6Cq/redux-todos-image.png)](https://postimg.cc/4K0bNrCm)

---

진한 보라색은 store를 Provider로 공급하는 index.js이다. 가장 밖의 껍질인 셈이다. App.js는 리액트각 컴포넌트의 레이아웃을 짜는 역할을 한다. 회색으로 표시된 것은 컴포넌트(component)다. 그 안에 초록색은 컨테이너(container)다. 컨테이너 안에 다시 컴포넌트가 싸여 있는 모습을 보이는데 이는 mapStateToProps와 mapDispatchToProps가 connect를 통해 사용할 수 있게 역할 분담을 하며 나뉘어져 있는 것이다.  
     
---

## 프로젝트 구조
[![2018-10-29-5-34-50.png](https://i.postimg.cc/fR55LFwV/2018-10-29-5-34-50.png)](https://postimg.cc/qNh2D16r)

---

## 결과 화면
[![2018-10-29-5-36-01.png](https://i.postimg.cc/TP49jD5G/2018-10-29-5-36-01.png)](https://postimg.cc/LqjL2hV0)

