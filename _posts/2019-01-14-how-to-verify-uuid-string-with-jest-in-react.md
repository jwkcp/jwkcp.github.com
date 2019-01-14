---
layout: post
title: 리액트에서 Jest를 이용해 테스트 코드를 작성할 때 UUID를 검증하는 방법
tags: react jest enzyme
comments: true
---

UUID 라이브러리는 쉽게 유일한 id를 생성할 수 있게 해주는 라이브러리다. 이 라이브러리를 이용해 id를 할당하는 컴포넌트를 테스트할 때 매번 바뀌는 값때문에 어떻게 테스트를 할까 고민이 되었다. UUID가 생성한 문자열을 하드코딩으로 입력해두고 테스트하거나, 정규식을 이용해 UUID가 생성하는 문자열을 검증해도 된다. 보다 손쉽게 그냥 문자열이 들어오는지 여부만 검증 대상으로 하고 싶다면 아래와 같이 쓰면 된다.

```
expect(action).toEquel({
  type: "ADD_TASK",
  task: {
    id: expect.any(String)
  }
})
```
