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

>손손 님의 댓글 (2016-12-17 05:16 오전):
>그냥

{% highlight python %}
{^ bootstrap_form comment_form ^}  # 꺽쇠(^)를 %로 바꿀 것. jekyll에서 {와 %를 연속으로 쓸 수 없어 {^로 표현함.
{% endhighlight %}

>하시면 되요