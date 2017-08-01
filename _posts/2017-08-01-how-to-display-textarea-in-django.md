---
layout: post
title: 장고(Django)에서 모델 폼 Textarea(여러 줄 위젯) 사용하는 방법
tags: django, how-to
comments: true
---

## 1.모델 정의

아래와 같이 모델 정의 시 CharField가 아닌 TextField로 정의해준다.

{% highlight python %}
class TestModel(models.Model):
    short = models.CharField(max_length=30)
    longnwide = models.TextField(max_length=300)
{% endhighlight %}

## 2.폼 설정

아래에 보이는 class, placeholder는 더 예쁘게 보이기 위해 부트스트랩의 다른 속성을 적용한 것이다. 아래와 같이 본문 초기화 코드에 사용했으며 self.fields['lognnwide'].widget.attrs = {'rows': 10} 속성 설정을 통해 Textarea의 높이를 설정한다.

{% highlight python %}
class TestForm(models.ModelForm):
    class Meta:
        model = TestModel
        fields = '__all__'

    def __init__(self, *args, **kwargs):
        super(TestForm, self).__init__(*args, **kwargs)

        self.fields['longnwide'].widget.attrs = {
            'class': 'form-control',
            'placeholder': "This is a placeholder test",
            'rows': 10
        }
{% endhighlight %}



## 3.각 필드의 특성

### TextField
{% highlight python %}
class TextField(**options)[source]
{% endhighlight %}

*A large text field. The default form widget for this field is a Textarea.*   
**많은 양의 글자를 담는 필드입니다. 기본 위젯으로 Textarea가 적용됩니다.**

*If you specify a max_length attribute, it will be reflected in the Textarea widget of the auto-generated form field. However it is not enforced at the model or database level. Use a CharField for that.*      
**max_length 속성을 지정하면 자동으로 폼필드의 Textarea 위젯에 반영됩니다. 모델이나 데이터베이스에는 영향이 없고 CharField를 사용한 것과 같습니다.**

---

### CharField

{% highlight python %}
class CharField(max_length=None, **options)[source]
{% endhighlight %}

*A string field, for small- to large-sized strings.*   
**짧은 문자열부터 큰 문자열까지 담을 수 있는 문자열 필드입니다.**

*For large amounts of text, use TextField.*   
**많은 양의 텍스트에는 TextField가 더 적합하니 TextField를 사용하세요.**

*The default form widget for this field is a TextInput.*   
**위젯은 기본적으로 TextInput가 적용됩니다.**

*CharField has one extra required argument: CharField.max_length*   
**CharField를 사용할때는 max_length라는 매개변수가 필요합니다.**

*The maximum length (in characters) of the field. The max_length is enforced at the database level and in Django’s validation.*   
**필드의 최대 길이를 의미하며 장고(Django)가 데이터베이스 필드를 만들고, 입력된 값이 적절한지 유효성 검사를 할 때 사용됩니다.**

*[Note] If you are writing an application that must be portable to multiple database backends, you should be aware that there are restrictions on max_length for some backends. Refer to the database backend notes for details.*   
**[주의] 데이터베이스에 따라 지정한 대로 max_length가 동작하지 않는 등 max_length 사용에 제약이 있을 수 있습니다. 자세한 사항은 해당 데이터베이스 사양 설명 부분을 참고하세요.**
