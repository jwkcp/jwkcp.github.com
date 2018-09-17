---
layout: post
title: 판다스(Pandas)의 Dataframe Series 자료형에서 lambda식 사용하기
tags: pandas
comments: true
---
      
아래와 같이 사용할 수 있다.

---

~~~
series.apply(lambda x: x**2)
~~~
     
~~~
series.apply(lambda x: 'old' if x >= 20 else 'new')
~~~
     