---
layout: post
title: jest를 이용해 스냅샷을 만들면 자꾸 ContextConsumer가 나오는 문제
tags: react jest
comments: true
---

# 문제

enzyme의 shallow를 이용해 컴포넌트의 테스트 스냅샷을 만들면 정상적으로 엘리먼트가 보이지 않고 자꾸 아래와 같이 ContextConsumer가 나온다.

```
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`페이지 구성 요소를 검사합니다. 1`] = `
<ContextConsumer>
  <Component />
</ContextConsumer>
`;
```

# 원인

테스트하려는 컴포넌트가 디폴트(Default) 컴포넌트로 로드되면서 리덕스(Redux)에 연결되어 있어서 그렇다.

# 해결방법

아래와 같이 디폴트(Default)가 아닌 네임드(Named)로 불러와 쓴다. 당연히 컴포넌트 정의 부분에서는 export default와 별로도 export를 붙여줘서 호출 가능한 상태로 만들어 둔 상태여야 한다.

```
// 이렇게 하면 Jest가 리덕스에 연결된다.
import AddPage from 'MyComponent'

// 이렇게 해야 리덕스와 연결을 끊은 상태로 로드된다. 필요한 항목은 테스트에서 props로 전달해둔다.
import { AddPage } from 'MyComponent'
```
