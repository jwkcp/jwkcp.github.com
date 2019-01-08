---
layout: post
title: 엔자임(enzyme)에서 simulate 함수 사용 시 에러가 발생하는 경우
tags: react enzyme
comments: true
---

## 문제

리액트에서 앤자임(enzyme)을 사용해 버튼 클릭 테스트를 할 때 simulate 함수 부분에서 아래와 같은 에러가 발생하는 경우가 있다.

**에러 메시지**

```
Method “simulate” is meant to be run on 1 node. 0 found instead
```

---

## 해결방법

**snapshot**의 파일을 열어보면 simulate로 테스트하려는 button 등이 안보이고 다른 컴포넌트가 랜더링되어 있는 것을 확인 할 수 있을 것이다. 그래서 simulate 함수가 실행할 노드가 없다는 에러를 뱉은 것이었다. 아마 테스트하려는 대상 예를 들어, LoginPage.js가 리덕스를 사용하고 있고 리덕스까지 테스트하면 안되기 때문에 connect 부분을 **export default connect ...** 와 같이 디폴트로 설정하고 LoginPage는 **export LoginPage ...** 와 같이 코드를 작성했을 것이다. default 키워드 없는 컴포넌트는 호출할 때 네임드 함수로 호출해야 한다. 아래처럼.

```
import { LoginPage } from 'LoginPage'

... 중략 ...
```

아마 이 에러(Method “simulate” is meant to be run on 1 node. 0 found instead)가 나는 경우는 중괄호 없이 컴포넌트를 호출해서 그럴 것이다.
