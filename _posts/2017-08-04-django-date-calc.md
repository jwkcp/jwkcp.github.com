---
layout: post
title: 장고(Django) 템플릿 태그(Template tags)로 남은 날, 지난 날 계산하기
tags: django
comments: true
---

남거나 지난 일수를 사용자에게 알려주어야 할 때가 있다. 장고는 이런 경우를 대비해 템플릿 태그를 사용해 간편하게 계산할 수 있는 방법을 제공한다. 예를 들어, 2017-08-01 18:00 이란 값을 가진 날짜가 있고 오늘이 2017-08-02 12:00 라면 timesince 템플릿 태그는 **1일, 6시간** 이란 값 리턴하는 식이다.

*아래 <는 {로 바꿔주시길.*

---

### 지난 날 계산하기
{% highlight python %}
<< your_date_variable|timesince >>
{% endhighlight %}

이게 끝이다. 어떤 특정 날짜부터 계산하고 싶다면 아래와 같이 timesince뒤에 날짜를 지정하면 된다.
{% highlight python %}
<< your_date_variable|timesince:target_date >>
{% endhighlight %}

### 남은 날 계산하기
위에 timesince를 timeuntil로 바꿔주면 된다.

---

*템플릿 태그에 관한 더 많은 정보는 [여기](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/) 참고.*
