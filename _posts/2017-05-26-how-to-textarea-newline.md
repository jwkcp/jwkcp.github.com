---
layout: post
title: Django Textarea에서 개행문자 적용하기
tags: django, how-to
comments: true
---

Django를 이용해서 Textarea 폼에서 개행을 포함한 값을 입력받은 후, 이 데이터를 템플릿(Template)에서 불러 HTML로 출력하려고 하면 개행이 적용되지 않고 표시되는 것을 볼 수 있다. 왜 그럴까? 바로 Textarea에서 입력받을 때는 \n\r로 저장된 후 이 값을 그대로 뿌리기 때문이다. 반면 HTML에서 개행은 \n\r이 아니라 (br)이다.(에디터 문제로 홑화살괄호를 소괄호로 표시함) 이 값을 치환하는 방법은 여러 가지가 있지만 Django에서는 매우 간편하고 훌륭한 방법을 제공한다.
    
### Django의 내장 템플릿 태그(Built-in template tags) 사용하기
{% highlight python %}
{* value|linebreaksbr *}
{% endhighlight %}

*에디터 문제로 중괄호를 *로 표시함*

---

### Django 공식 사이트에 표기된 사용 방법
[![linebreaksbr.png](https://s26.postimg.org/8f4bg0dx5/linebreaksbr.png)](https://postimg.org/image/rwyyvyaut/)



---

Django의 내장 템플릿 태그에 관한 더 많은 정보는 [여기]("https://docs.djangoproject.com/en/1.11/ref/templates/builtins/")를 참고하세요.