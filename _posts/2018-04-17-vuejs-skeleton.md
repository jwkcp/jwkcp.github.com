---
layout: post
title: vue.js 뼈대 및 순서 (초간단 예제)
tags: deeplearning
comments: true
---
  
---
  
아래는 vue.js의 뼈대다. 처음 vue.js를 돌려보고 싶은 사람은 당연한 걸 수도 있지만 3가지를 알고 있자.
  
1. 제일 아래 3번 script에 type="javascript"를 쓰면 동작하지 않는다.
2. 2번과 3번 블럭의 순서를 바꿔쓰면 동작하지 않는다.
3. 1번의 주소는 CDN(https://unpkg.com/vue) 주소로 대신 사용할 수 있다.

---

~~~
<!-- 1번 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<!-- 2번 -->
<div id="app">
    <p>{{ message }}</p>
</div>

<!-- 3번 -->
<script>
    new Vue({
        el: '#app',
        data: {
        message: 'Hello Vue.js!'
    }
})
</script>
~~~

위 소스는 [vue.js 공식 사이트](https://kr.vuejs.org/)에서 가져온 것으로 [vue.js 공식 사이트](https://kr.vuejs.org/)에서 더 많은 예제와 쉬운 설명을 찾아 볼 수 있다.