---
layout: post
title: 리덕스 차근 차근 이해하기
tags: redux
comments: true
---
           
리덕스(redux)의 개념은 크게 어려운 부분이 없다. 디자인 패턴을 이용해 객체지향 프로그래밍을 해본 사람이라면 이런 식을 구조를 구현해 사용해본적이 있을 것이다. 하지만 리덕스를 리액트를 배우는 사람들이 리액트 때문에 시작하는 경우가 많다보니 리덕스 자체의 원리보다 리액트와 합쳐진 형태를 먼저 보게 된다. 이 때 react-redux와 같은 외부 라이브러리까지 추가되면서 혼란을 가중시킨다. 
    
이런 분들을 위해 리덕스에 대한 간단한 설명을 하고 몇 가지 학습 자료를 소개한다.

---

# 리덕스의 개념 원리
리덕스는 크게 **저장소(store)**, **명령(dispatch)**, **구독(subscribe)** 3가지로 나눠볼 수 있다. **저장소(store)**에 뭔가 바꾸고 싶으면 **명령(dispatch)**을 이용해 바꾸고, 관심있는 사람(함수)가 값이 바뀔 때 마다 받아보는 것이다. 이 때 "나 관심있어요"라고 말해주는 행동이 **구독(subscribe)**이다. 
    
> 명령(dispatch)  -->  저장소(store)  <--  구독(subscribe)
    
이를 코드로 보면 아래와 같다. (출처: [redux github](https://github.com/reduxjs/redux/blob/master/examples/counter-vanilla/index.html))   
     
~~~
<!DOCTYPE html>
<html>
  <head>
    <title>Redux basic example</title>
    <script src="https://unpkg.com/redux@latest/dist/redux.min.js"></script>
  </head>
  <body>
    <div>
      <p>
        Clicked: <span id="value">0</span> times
        <button id="increment">+</button>
        <button id="decrement">-</button>
        <button id="incrementIfOdd">Increment if odd</button>
        <button id="incrementAsync">Increment async</button>
      </p>
    </div>
    <script>
      function counter(state, action) {
        if (typeof state === 'undefined') {
          return 0
        }
        switch (action.type) {
          case 'INCREMENT':
            return state + 1
          case 'DECREMENT':
            return state - 1
          default:
            return state
        }
      }
      var store = Redux.createStore(counter)
      var valueEl = document.getElementById('value')
      function render() {
        valueEl.innerHTML = store.getState().toString()
      }
      render()
      store.subscribe(render)
      document.getElementById('increment')
        .addEventListener('click', function () {
          store.dispatch({ type: 'INCREMENT' })
        })
      document.getElementById('decrement')
        .addEventListener('click', function () {
          store.dispatch({ type: 'DECREMENT' })
        })
      document.getElementById('incrementIfOdd')
        .addEventListener('click', function () {
          if (store.getState() % 2 !== 0) {
            store.dispatch({ type: 'INCREMENT' })
          }
        })
      document.getElementById('incrementAsync')
        .addEventListener('click', function () {
          setTimeout(function () {
            store.dispatch({ type: 'INCREMENT' })
          }, 1000)
        })
    </script>
  </body>
</html>
~~~

---
   
위 소스에서 html 부분, 그러니까 body 태그에서 script 부분을 뺀 부분이 UI를 만들어 주는 부분이다. 우리가 여기서 살펴볼 부분은 script 태그 안에 부분이다. 불필요한 부분을 좀 빼고 나눠보면 아래와 같다.   

## 스토어(store)
~~~
var store = Redux.createStore(counter)
~~~

## 구독(subscribe)
~~~
store.subscribe(render)
~~~

## 명령(dispatch)
~~~
document.getElementById('increment').addEventListener('click', function () {
  store.dispatch({ type: 'INCREMENT' })
})
document.getElementById('decrement').addEventListener('click', function () {
  store.dispatch({ type: 'DECREMENT' })
})
~~~
    
## 기타 
구독을 하고 나서 값이 바뀌었다는 신호를 받으면 뭔가 해야하지 않겠나. 아무것도 안할거라면 굳이 구독할 필요가 없으니 말이다. 그래서 아래 부분은 구독 후 값이 바뀌었다는 신호를 받으면 실행되는 부분이다. 아래 render() 함수는 최초 실행 시 값을 표시하기 위한 부분이니 여기서는 큰 의미를 두지 말자. 
~~~
function render() {
  document.getElementById('value').innerHTML = store.getState().toString()
}
render()
~~~
    
또, 맨 위에 counter라는 함수가 있다. 스토어를 생성할 때 인자로 넣어주는데 명령을 받으면 어떤 동작을 수행하지 정하는 곳이다. 리듀서(reducer)라고 부른다. 이름이 생소하여 좀 어렵게 느껴지지만 그냥 함수다.
~~~
function counter(state, action) {
  if (typeof state === 'undefined') {
    return 0
  }
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
~~~

# 연습하기
세 군데로 잘라보니 별로 어렵지 않다. 이제 손으로 좀 익힐 차례다. 리액트와 함께 쓰려면 리액트 구조에 맞게 구현도 해보고, 좀 더 편하게 구현할 수 있게 react-redux 같은 라이브러리도 써본다. [여기](https://redux.js.org/introduction/examples)에 가면 위와 같이 index.js 파일 하나로 구현한 리덕스 예제부터 다양한 응용이 들어간 코드까지 만나볼 수 있다. [이 소스](https://codesandbox.io/s/github/reactjs/redux/tree/master/examples/counter)는 위에서 살펴본 리덕스를 리액트에 가장 간단한 방법으로 붙여본 소스다. 이 소스를 보고 몇 번 연습해보면 감이 오리라 생각한다. 영어에 큰 거부감이 없는 분들은 리액트를 만든 Dan Abramov가 직접 알려주는 동영상 튜토리얼을 추천한다. [Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)

