---
layout: post
title: 리액트 웹팩 사용 시 문제가 되는 소스를 찾기 쉽게 개발 설정 하는 방법
tags: react webpack
comments: true
---

리액트(React)에서 웹팩(Webpack)을 쓰다보면 번들된 결과물(bundle.js)로 서비스하기 때문에 개발 과정에서 문제가 되는 소스를 정확히 찾을 수 없어 난감할 때가 있다. 아래와 같이 웹팩 설정을 하면 문제가 되는 부분을 바로 보여주기 때문에 개발을 좀 더 편하게 할 수 있다. 소스맵(source map)을 추가히기 때문에 bundle.js 파일의 크기는 좀 더 늘어난다. 개발할 때만 사용하고 배포시에는 해당 옵션을 없애자. [여기](https://webpack.js.org/configuration/devtool/)에 설정 가능한 다른 옵션이 참고.

**webpack.config.js**

```
module.exports = {
  ... 중략

  devtool: "cheap-module-eval-source-map",

  ... 중략
};
```
