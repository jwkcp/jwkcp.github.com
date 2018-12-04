---
layout: post
title: yarn run [script]에서 run을 꼭 써야할까?
tags: react webpack babel
comments: true
---

yarn을 사용할 때 run을 써야 하나 빼야하나 싶을 때가 있다. 어떤 문서에서는 yarn run build라고 설명하고 어떤 문서에서는 yarn build라고 되어 있고 말이다.

결론은 빼도 상관 없다. [yarn의 공식 문서를 보면 아래와 같이 나와 있다.](https://yarnpkg.com/lang/en/docs/cli/run/)

> It’s also possible to leave out the run in this command, each script can be executed with its name:

이렇게 할 수 있고,

```
yarn run test -o --watch
```

이렇게도 할 수 있다.

```
yarn test -o --watch
```
