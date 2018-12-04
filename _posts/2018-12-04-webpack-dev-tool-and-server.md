---
layout: post
title: 웹팩으로 번들링한 파일 디버깅용 소스 맵 추가 방법과 웹팩 개발 서버 사용하기
tags: react webpack
comments: true
---

# 웹팩(Webpack) 번들링 파일의 디버깅 도우미

웹팩으로 번들링을 하면 bundle.js와 같은 파일에 소스가 모두 묶여 브라우저에서 오류가 발생했을 때 어떤 소스 파일에서 문제가 있는지 원인을 찾기 어렵다. 이 때 아래와 같이 웹팩 설정(webpack.config.js)에 항목을 넣으면 문제가 되는 소스 파일이 확인 가능해진다. [여기](https://webpack.js.org/configuration/devtool/)에 가면 devtool 항목에 지정할 수 있는 옵션이 여러 개 있다. 아래는 예시.

**webpack.config.js** 파일에 아래 내용을 추가

```
moudle.exports = {
  ... 중략

  devtool: "cheap-module-eval-source-map"

  ... 중략
}
```

---

# 웹팩(Webpack) 개발 서버 띄우기

create-react-app 같은 도구가 아니라 그냥 처음부터 스스로 셋업 해 리액트 개발을 하려고 하면 개발 서버가 필요하다. 그런데 개발 환경에서 apache나 ngnix같은 웹서버를 쓰려고 하면 좀 번거롭다. 그래서 yarn add live-server와 같이 개발용 웹서버 라이브러리를 설치해서 쓰곤 한다. 당연히 웹서버가 서비스할 소스들은 바벨(babel)이나 웹팩(webpack)으로 빌드해줘야 한다. 그래서 빌드와 더불어 소스가 변경되면 다시 자동으로 빌드해주는 것 하나, 이 빌드된 결과물을 서비스할 웹서버 하나가 필요하다. 하지만 웹팩 개발 서버를 설치하면 이게 손쉽게 해결된다.
  
**설치**

```
yarn add webpack-dev-server
```

**설정 (webpack.config.js)**

```
const path = require("path");

module.exports = {
  ...중략

  devServer: {
    contentBase: path.join(__dirname, "public")
  }

  ...중략
}
```

**설정 (package.json)**

```
{
  ...중략

  "scripts": {
    "dev-server": "webpack-dev-server"
  }

  ...중략
}
```
