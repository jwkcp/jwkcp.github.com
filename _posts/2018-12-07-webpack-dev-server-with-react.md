---
layout: post
title: 웹팩 개발 서버(webpack-dev-server)로 리액트 개발 편하게 하기
tags: react webpack
comments: true
---

리액트(React) 개발 과정에는 여러 선택사항이 있지만 한가지 절차를 언급해 설명한다면 이렇습니다. **ES6 문법을 사용해 코드를 쓰고 필요한 모듈을 불러쓰기 위해 웹팩(Webpack)으로 의존성 있는 모듈을 번들링하여 배포.**

이 과정에서 브라우저에 작성한 소스를 보여주기 위한 웹서버와 ES6 문법을 ES5 등 구형 브라우저에서 해석할 수 있도록 하기 위한 바벨(Babel) 코드 변환(Transpiling)이 필요합니다. _(그래서 바벨을 자바스크립트를 위한 트랜스파일러 Transpiler 또는 컴파일러 Compiler 라고 합니다.)_

우선 **웹서버**가 필요한데 apache나 nginx같은 웹서버를 쓰기에는 개발용으로 너무 부담스럽습니다. 그래서 [live-server](https://www.npmjs.com/package/live-server)와 같은 라이브러리를 사용합니다. live-server는 'live-server 서비스할 폴더명'과 같이 설정해주면 간단한 웹서버가 뜹니다.

**트랜스파일러** 바벨은 babel-core와 babel-loader 또는 터미널에서 사용하기 위한 babel-cli를 설치하고 babel-preset-env, babel-preset-react와 같이 어떤 프리셋을 쓸지 설치해주고 이를 변환해줘야 합니다. 이 변환된 파일을 웹서버가 서비스하는 것이지요. 그런데 소스를 수정할 때 마다 매번 수동으로 변환해주는 것은 귀찮습니다. 그래서 --watch와 같은 옵션을 주어 계속 소스를 감시하고 있다가 소스가 변경되면 자동으로 변환하는 방법을 쓰기도 합니다. 그런데 이렇게 하더라도 1.웹서버를 띄우고 2.바벨 변환하고 하는 2가지를 해줘야 하지요. 귀찮습니다.

[webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server)는 이 번거로움에서 벗어나게 해줍니다. 아래와 같이 설치하고

```
yarn add webpack-dev-server
```

웹팩 설정파일(webpack.config.js)에서 아래와 같이 어디를 서비스할지만 지정해주면 됩니다.

```
const path = require("path");

module.exports = {
  ... 중략

  devServer: {
    contentBase: path.join(__dirname, "public")
  }

  ... 중략
};
```

그러면 터미널에서 yarn webpack-dev-server라고 입력하면 빌드부터 웹서버의 역할까지 한꺼번에 해줍니다. yarn webpack-dev-server라고 타이핑하는 게 귀찮다면 package.json의 scripts 항목에 원하는 이름을 지정하면 타이핑을 적게 하고 편하게 쓸 수 있습니다.

```
"scripts": {
  ... 중략

  "ds": "webpack-dev-server"

  ... 중략
}
```
