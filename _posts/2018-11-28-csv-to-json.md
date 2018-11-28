---
layout: post
title: 자바스크립트에서 csv를 json으로 바꾸는 방법
tags: javascript
comments: true
---

파이썬에서 쉽게 했던 csv를 읽어 json으로 바꾸는 작업이 자바스크립트(javascript)에서는 좀처럼 쉽게 되지 않았는데 npm 라이브러리 중 csvtojson이 유용한 것 같아 소개한다.

## 설치

```
npm i -g csvtojson
```

아시다시피 i는 install -g는 global을 뜻한다. 터미널에서도 변환을 수행할 수 있기 때문에 -g 옵션을 주어 global로 설치하는 것을 추천한다.

## 터미널에서 변환하기

설치했으면 아래와 같은 커맨드로 변환이 가능하다.

```
$ csvtojson source.csv > converted.json
```

## 더 보기

[https://www.npmjs.com/package/csvtojson](https://www.npmjs.com/package/csvtojson)
