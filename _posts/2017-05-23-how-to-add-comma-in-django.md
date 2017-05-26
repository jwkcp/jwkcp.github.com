---
layout: post
title: Django 템플릿에서 세 자리마다 숫자에 콤마 찍기
tags: django, humanize, how-to
comments: true
---

웹페이지에서 금액을 표시할 때 세 자리마다 콤마를 찍고 싶은 경우가 있다. 아래 방법을 사용하면 Django에서 자바스크립트를 쓰지 않고 세 자리마다 콤마를 찍을 수 있다.
    
### settings.py 에 앱 추가하기 (1/3)
{% highlight python %}
INSTALLED_APPS = [
    ...
    'django.contrib.humanize',
    ...
]
{% endhighlight %}

### html 파일에 로드하기 (2/3)
{% highlight python %}
{* load humanize *}
{% endhighlight %}

위에 중괄호 다음의 *표시를 %로 바꾸세요.

### intcomma 태그 사용 (3/3)
{% highlight python %}
{* object.name|intcomma *} 
{% endhighlight %}

위 중괄호 다음의 *표시를 %로 바꾸세요.

---

이 외에도 1을 one으로 바꾼다던지, 1000000을 1.0 million으로 바꾸는 등 다양한 기능이 humanize에 포함되어 있다. 더 많은 정보는 [여기]("https://docs.djangoproject.com/en/1.11/ref/contrib/humanize/")를 참고하세요.