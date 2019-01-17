---
layout: post
title: 리액트(React)에서 부트스트랩(Bootstrap) 사용하는 방법
tags: react bootstrap
comments: true
---

리액트에서 부트스트랩을 쓰려면 어떤 방법이 좋을까?

## bootstrap 라이브러리 설치 (1/3)

```
yarn add bootstrap

// npm을 쓴다면
npm install --save bootstrap
```

## import (2/3)

src/index.js와 같이 최상위에서 컴포넌트 호출하는 소스에 넣어주면 한 번만 입력해도 모든 컴포넌트에 적용됩니다.

```
import 'bootstrap/dist/css/bootstrap.css'
```

## 사용하기 (3/3)

그냥 하던대로 쓰면 끝.

```
<div className="container">
  <button className="btn btn-default">Click Me</button>
</div>
```

_참고링크: [https://facebook.github.io/create-react-app/docs/adding-bootstrap](https://facebook.github.io/create-react-app/docs/adding-bootstrap)_
