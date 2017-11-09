---
layout: post
title: 장고(Django) urls.py 에서 모든 패턴 매칭하는 정규표현식
tags: django, regex
comments: true
---

장고에서는 사용자가 요청하는 url을 패턴을 정규표현식으로 설정할 수 있는 기능을 제공하고 있다.   
예를 들어, 어떤 문서를 요청하는 url이 test.com/article/1 이라면 이를 {% highlight python %}url(r'^article/(?P<id>\d+)/$'){% endhighlight %} 처럼 표기할 수 있다. 이렇게 하면 사용자가 article/ 다음에 숫자를 넣고 요청하면 이 url 패턴과 일치되어 그 다음 동작을 수행한다.

그럼 모든 문자열을 받으려면 어떻게 해야 할까.
아래와 같이 .* 를 이용하면 된다.

{% highlight python %}
url(r'^article/(?P<id>.*)/$')
{% endhighlight %}
