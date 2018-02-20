---
layout: post
title: 파이썬에서 반복문 돌 때 인덱스 얻는 방법
tags: python, how-to
comments: true
---
  
파이썬에서 루프를 돌 때 인덱스가 필요한 경우가 있다. 아래와 같이 range를 쓰는 경우가 있는데 
  
~~~
for a in range(len(my_list)):
~~~
  
그러지 말고 enumerate를 쓰면 좋다. 쉽고 간편하며 권장되는 방식.
  
~~~
for idx, a in enumerate(my_list):
~~~
  