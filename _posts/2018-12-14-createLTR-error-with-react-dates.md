---
layout: post
title: Cannot read property 'createLTR' of undefined 에러가 발생하는 경우 해결방법
tags: react
comments: true
---

[react-dates](https://github.com/airbnb/react-dates)를 사용할 때 대부분 아래와 같이 임포트 후 사용한다.

```
import { SingleDatePicker } from 'react-dates';
import 'react-dates/lib/css/_datepicker.css';
```

그런데 아래와 같은 에러가 발생한다면?

> Cannot read property 'createLTR' of undefined

[react-dates 버전이 올라가면서 v13.0.0부터 react-with-styles에 의존성](https://github.com/airbnb/react-dates#initialize)이 생겼다. 그래서 **import 'react-dates/initialize'** 임포트 구분을 react-dates 임포트 구문보다 앞서서 호출하면 위 에러가 사라진다.

```
import 'react-dates/initialize';

import { SingleDatePicker } from 'react-dates';
import 'react-dates/lib/css/_datepicker.css';
```
