---
layout: post
title: 엔자임(enzyme)에서 DateRangePicker 테스트 시 에러가 발생하는 경우
tags: react
comments: true
---

## 문제

Airbnb에서 만든 리액트용 데이트 픽커(Date picker) 모듈을 enzyme으로 테스트 할 때 아래와 같은 오류 메시지가 나오면서 에러가 발생할 때가 있다.

```
wrapper.find("DateRangePicker").prop("onDatesChange")({

... 중략 ...
```

**에러 메시지**

```
Method “props” is meant to be run on 1 node. 0 found instead.
```

---

## 해결방법

**snapshot**의 파일을 열어보면 withStyles(DateRangePicker) 로 되어 있는 모습을 볼 수 있다.

```
wrapper.find("withStyles(DateRangePicker)").prop("onDatesChange")({

... 중략 ...
```
