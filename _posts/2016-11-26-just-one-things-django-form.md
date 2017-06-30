---
layout: post
title: Django 폼을 사용하기 전에 알아야 할 단 한가지
tags: django form
comments: true
---
Django에서는 사용자 입력을 받기 위해 사용되는 텍스트박스, 드롭다운박스와 같은 폼(Form)을 html에 렌더링하기 위해 훌륭한 방법을 제공하고 있습니다. 이 방법을 사용하면 사전 검사를 통해 사용자의 입력 중 유효한 값만을 사용할 수 있는 등 반복적으로 해야하는 귀찮은 일들을 크게 줄여 줍니다.

* Django에서 폼(Form)을 사용하는 2가지 방법

1. forms.Form 클래스를 상속받는 일반 폼(Form)
2. forms.ModelForm 클래스를 상속받는 모델 폼(Form)

* 어떤 폼 구현 방식이 나에게 어울리나?

사용하려는 폼의 성격이 **모델(Model)과 얼마나 관련**이 있고, **스타일 등의 커스터마이징을 원하나** 생각해보는 것이 폼 구현 방법 결정에 큰 도움을 줍니다.

---

## 일반 폼(Form)
   
forms.Form 클래스를 상속받아 사용합니다. 일일이 모델에 정의한 필드를 다 맵핑해줘야 해 번거롭고 힘든면이 있습니다. 모델(model)에 정의한 값을 html로 렌더링하는 경우라면 Django는 ‘모델 폼’ 이라는 더 좋은 방법을 제공합니다. 그렇다고 ‘일반 폼’이 쓸모없는 건 아닙니다. 모델(model)에서 정의한 필드 외 값을 다루어야 하는 경우 일반 폼을 사용해야 합니다.

이렇게 폼을 정의하고,

{% highlight python %}
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)
{% endhighlight %}

이렇게 뷰에서 불러다 씁니다.

{% highlight python %}
from django.shortcuts import render
from django.http import HttpResponseRedirect

from .forms import NameForm

def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == 'POST':
        # create a form instance and populate it with data from the request:
        form = NameForm(request.POST)
        # check whether it's valid:
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL:
            return HttpResponseRedirect('/thanks/')

    # if a GET (or any other method) we'll create a blank form
    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
{% endhighlight %}

---

## 모델 폼(ModelForm)

forms.ModelForm을 상속받아 사용합니다. 모델(model)에 정의한 필드만 html로 렌더링한다면 이 방법을 쓰는게 좋습니다. 아래와 같이 폼 클래스를 정의할 때 메타(Meta) 클래스의 속성만 지정해주면 나머진 Django가 다 알아서 해줍니다. fields에 html 렌더링을 원하는 항목을 표기할 수도 있습니다. 더 자세한 ‘모델 폼’에 대한 정보는 [여기](https://docs.djangoproject.com/en/1.10/topics/forms/modelforms/#modelform "여기")를 참고하세요.

{% highlight python %}
from django.forms import ModelForm

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = '__all__'
{% endhighlight %}

### 모델 폼을 더 쉽게 만들자. 팩토리 함수!

Django에서는 이런 모델 폼을 더 쉽게 생성할 수 있도록 팩토리(Factory)함수를 제공합니다. GoF(Gang of Four)의 디자인패턴(Design pattern)에서 등장하는 그것 맞습니다. : ) 아래와 같이 modelform_factory 함수 한줄만 호출하면 ‘모델 폼’이 만들어집니다.

{% highlight python %}
from django.forms import modelform_factory
from myapp.models import Book

BookForm = modelform_factory(Book, fields=("author", "title"))
{% endhighlight %}

이렇게 widgets 속성을 이용해 상세 설정을 해줄 수도 있습니다.

{% highlight python %}
from django.forms import Textarea
Form = modelform_factory(Book, form=BookForm,
                         widgets={"title": Textarea()})
{% endhighlight %}

### 폼(Form) 사용의 끝판왕, 제네릭 뷰(Generic view)

지금까지 폼을 사용할 때 모델과 연관이 있는지, 없는지에 따라 ‘일반 폼’과 ‘모델 폼’을 사용으로 나눠봤고, 팩토리 함수를 통해 더 쉽게 ‘모델 폼’을 만드는 것도 알아봤습니다. Django는 한 발 더 나아가 제네릭 뷰를 이용해 폼을 다룰 수 있게 해두었습니다. 가만히 보니 사람들이 모델(Model)과 관련된 폼(Form)을 뷰(View)를 통해 html 렌더링하는 경우가 많더라는 거죠. 제네릭 뷰를 이용한 폼은 이런 경우에 선택할 수 있는 최고의 방법입니다. : )

아래 예제를 잘 보면 CreateView, UpdateView, DeleteView를 임포트하여 상속 후, 별도의 폼(Form) 클래스를 만들지 않고 뷰(View)에서 바로 필요한 속성만 딱 지정해준 것을 볼 수 있습니다.

{% highlight python %}
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy
from myapp.models import Author

class AuthorCreate(CreateView):
    model = Author
    fields = ['name']

class AuthorUpdate(UpdateView):
    model = Author
    fields = ['name']

class AuthorDelete(DeleteView):
    model = Author
    success_url = reverse_lazy('author-list')
{% endhighlight %}