---
layout: post
title: 리액트 JSX에서 이미지가 엑박으로 나오는 경우
tags: react
comments: true
---

## 문제

리액트에서 이미지를 표시하기 위해 아래와 같이 코딩하면 이미지가 로드되지 않고 엑박으로 나온다.

```
<img src='./images/mypic.png alt='mypic' />
```

## 해결방법

아래와 같이 하면 경로 문제를 리액트가 알아서 처리해준다.

```
import mypic from './images/mypic.png'

.. (중략)

<img src={mypic} alt='mypic' />
```
