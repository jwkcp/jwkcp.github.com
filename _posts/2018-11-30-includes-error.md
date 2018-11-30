---
layout: post
title: 리액트 배포 시 인터넷 익스플로러에서만 includes 또는 promise 에러가 발생하는 경우
tags: react
comments: true
---

# includes 에러

### 에러 메시지

> 개체가 'includes' 속성이나 메서드를 지원하지 않습니다.

### 원인 및 해결방법

아마 소스코드 중에 Array 메서드로 includes를 사용한 것이 있을 것이다. 이 구문을 브라우저가 이해못해서 에러가 나는 것. 같은 기능을 할 수 있는 indexOf로 바꿔주면 된다.

---

# promise 에러

### 에러 메시지

> 'Promise'이(가) 정의되지 않았습니다.

### 원인 및 해결방법

해당 기능을 인터넷 익스플로러에서 지원하지 않아서 그렇다. 아래 스크립트 링크를 추가하여 polyfill을 사용할 수 있게 해주면 구형 익스플로러에서도 동작할 수 있게 코드를 바꿔준다.

```
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@7/dist/polyfill.min.js"></script>
```
