---
layout: post
title: 노멀라이즈(normalize.css) 적용 전후 비교와 사용 방법
tags: html
comments: true
---

브라우저마다 각기 다른 기본값을 통일하고 일관된 모양을 표현하기 위해 reset css나 normalize css를 통해 base css 작성하는 방법을 쓴다. 여기서는 normalize 코드 중 하나인 [normalize.css](http://necolas.github.io/normalize.css/)의 사용법과 간단히 버튼의 변화를 통한 전후 모양을 살펴본다. 세상에는 많은 종류의 normalize 코드가 존재하며 자신만의 코드를 만들어 사용하는 사람도 정말 많다.

---

## 사용법

### yarn 또는 npm을 통한 설치

```
yarn add normalize.css

또는

npm install normalize.css
```

위와 같이 설치 후 코드에서 -

```
import 'normalize.css'
```

### 파일 직접 다운로드

[https://necolas.github.io/normalize.css/8.0.1/normalize.css](https://necolas.github.io/normalize.css/8.0.1/normalize.css) 다운로드 후(같은 폴더라면)

```
import './normalize.css'
```

## 결과

[![2019-04-06-7-36-51.png](https://i.postimg.cc/bNPt4LrR/2019-04-06-7-36-51.png)](https://postimg.cc/hhCvdL9J)

부트스트랩도 normalize css 코드가 포함되어 있기 때문에 부트스트랩을 쓴다면 별도의 normalize css 코드를 안써도 될 수 있다.
