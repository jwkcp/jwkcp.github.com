--- 
layout: post
title: Django 폼(Form)에서 부트스트랩 스타일 적용하기
tags: django form bootstrap
comments: true
---
Django의 폼 구현 방식은 유효성 검사라든지 여러 가지 측면에서 참 편리함을 준다. 하지만 가장 기본적인 HTML 컨트롤 그대로의 모습이라 모양이 세련되지 못하다. 그래서 부트스트랩을 적용해보려 했으나 쉽지 않았고, 그 과정에서 아래와 같이 각 필드의 속성을 업데이트하는 방법을 찾고 테스트해 보며 알게 된 방법을 기록해둔다.

---

## 스택오버플로우의 답변

아래와 같이 폼 클래스의 필드 위젯에 속성을 업데이트하는 방법을 스택오버플로우 답변에서 찾았다.
{% highlight python %}
class BootstrapModelForm(ModelForm):
    def __init__(self, *args, **kwargs):
        super(BootstrapModelForm, self).__init__(*args, **kwargs)
        for field in iter(self.fields):
            self.fields[field].widget.attrs.update({
                'class': 'form-control'
            })
-----
class MyForm(BootstrapModelForm):
    def __init__(self, *args, **kwargs):
        super(MyForm, self).__init__(*args, **kwargs)
        self.fields['name'].widget.attrs.update({'placeholder': 'Enter a name'})
{% endhighlight %}

---

## 개인적으로 테스트하여 사용한 방법

나는 아래와 같이 구현해봤는데 정상적으로 부트스트랩 스타일이 적용되었다.

{% highlight python %}
class ArticleForm(ModelForm):
    class Meta:
        model = Article
        fields = ('title', 'contents', 'author')
        widgets = {
            'title': TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter a title'}),
            'contents': Textarea(attrs={'class': 'form-control'}),
            'author': Select(attrs={'class': 'form-control'}),
        }
{% endhighlight %}

이렇게 작성된 폼을 뷰에서 아래와 같이 불러와 사용한다.

{% highlight python %}
iews.pyPython

class ArticleCreateView(FormView):
    template_name = 'post/article_form.html'
    form_class = ArticleForm
    success_url = reverse_lazy('post:index')

    def form_valid(self, form):
        form.instance.owner = self.request.user
        form.save()
        return super(ArticleCreateView, self).form_valid(form)
{% endhighlight %}

---

## Django-bootstrap3 라이브러리를 이용하는 방법

django의 폼(form)을 쓰는 다른 사람들도 이런 문제때문에 골치가 아팠던 것 같다. [django-bootstrap3](https://github.com/dyve/django-bootstrap3 "django-bootstrap3")라는 깃허브 저장소에 가면 위와 같이 widget에 일일이 부트스트랩 클래스를 타이핑하지 않고도 쉽게 사용할 수 있도록 라이브러리로 만들어 둔 라이브러리가 있었다. 예를 들어, django에서 User모델의 사용자 이름 폼 필드를 부트스트랩 스타일로 만들려면 아래와 같이 하면 된다.

`jekyll 문법 상 중괄호({)와 퍼센트(%)를 연속해서 쓸 수 없어 아래 코드에서는 중괄호({})와 퍼센트(%) 사이에 공백을 넣었다. 실제로는 사이의 공백을 두지 말아야 한다.`

{% highlight python %}
{ % load bootstrap3 % }
{ % bootstrap_css % }
{ % bootstrap_javascript % }
{ % bootstrap_field form.username name="txt_username" placeholder="사용자 이름" show_label=False size='large' % }
{% endhighlight %}

[여기](http://django-bootstrap3.readthedocs.io/ "여기")를 누르면 자세한 사용법을 볼 수 있다.

