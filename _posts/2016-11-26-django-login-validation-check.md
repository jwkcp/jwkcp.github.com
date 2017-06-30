---
layout: post
title: Django에서 사용자 인증 여부를 확인하는 2가지 방법
tags: django authentication how-to
comments: true
---
Django에서 각 뷰에 접근 시 로그인 여부를 검사하여 접근을 통제할 수 있다. 크게 아래와 같이 2가지로 나눠 볼 수 있다.

* 데코레이터(Decorator)를 이용한 구현 방법: 함수형 뷰에서 사용
* 믹스인(Mixin)을 이용한 구현 방법: 클래스형 뷰에서 사용

---

## 데코레이터(Decorator)를 이용한 구현 방법

함수형뷰를 사용한다면 아래와 같이 간편하게 @login_required를 써주는 것 만으로도 모든 작업이 끝난다. 하지만 클래스형으로 뷰를 구현한다면 이 방법은 사용할 수 없다.

{% highlight python %}
from django.contrib.auth.decorators import login_required
 
@login_required
def my_view(request):    
{% endhighlight %}
---

## 믹스인(Mixin)을 이용한 구현 방법

### 1. 직접 구현하는 방법

아래와 같이 LoginRequiredMixin 클래스를 직접 구현해 클래스형 뷰에서 상속받아 사용할 수 있다. 폼입력과 로그인 통제가 필요한 곳이면 매번 쓰는 기능인데 여간 귀찮고 번거로운게 아니다.

{% highlight python %}
from django.contrib.auth.decorators import login_required
 
# 믹스인 구현
class LoginRequiredMixin(object):
    @classmethod
    def as_view(cls, **initkwargs):
        view = super(LoginRequiredMixin, cls).as_view(**initkwargs)
        return login_required(view)
 
# 구현한 믹스인 가져다 쓰기
class PhotoCreateView(LoginRequiredMixin, CreateView):
    model = Photo
    fields = ['album', 'title', 'image', 'description']
    success_url = reverse_lazy('photo:index')
{% endhighlight %}

### 2. 만들어진 믹스인(Mixin) 가져다 쓰기

짝짝짝! Django 1.9부터 사용할 수 있다. 아래와 같이 @login_required 데코레이터를 쓰는 것 만큼이나 무척 간편해졌다.  제네릭뷰(Generic View) 때문에 어쩔 수 없이 클래스형 뷰를 사용하는 사람에게 큰 편리함을 준다.

{% highlight python %}
from django.contrib.auth.mixins import LoginRequiredMixin
 
# 바로 위 코드와 비교해보자. 아주 간단해졌다.
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
{% endhighlight %}