---
layout: post
title: 장고(Django) 모델 폼 input 태그 입력 시 *(별표) 표시하기
tags: react vue
comments: true
---
  
---
  
일반 html 태그에서는 아래와 같이 type을 password로 지정해서 이와 같은 작업을 할 수 있다.
  
~~~
<input type=password>
~~~
  
django를 이용할 경우에도 모델 폼이 아닌 그냥 폼의 경우 아래와 같이 하면 된다.
  
~~~
password = forms.PasswordInput()
~~~
  
그러나 모델 폼의 경우 위와 같이 하면 모델과 연동된 폼이 아닌 새로운 폼 인스턴스가 할당된다.
  
---
   
**그럴 때 아래와 같이 폼의 메타 클래스를 이용하면 된다.**
  
~~~
class MyForm(forms.ModelForm):
    class Meta:
        model = MyModel
        fields = ['name', 'password']
        widgets = {'password': forms.PasswordInput()}
~~~