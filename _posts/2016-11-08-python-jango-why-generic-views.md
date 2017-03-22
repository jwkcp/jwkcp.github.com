---
layout: post
title: 파이썬 장고(Django), 클래스형 뷰를 사용하는 이유
tags: django
comments: true
---
이 글에서는 Django 튜토리얼 문서 중 index뷰를 가지고 3가지 각기 다른 타입으로 구현한 것을 비교해본다. 그 과정에서 클래스형 뷰가 원초적 구현 방법에 비해 얼마나 큰 편리함을 제공하는지 살펴볼 것이다. 먼저 3개의 코드 블럭을 눈으로 쭉 훓어보자.

### 1. HttpResponse를 이용한 함수형 뷰 
함수형뷰를 사용해 template을 읽어 HttpResponse로 응답하는 과정.. 번거롭고 복잡하다. 하지만 가장 기초적인 부분이이 알아두면 나중에 나오는 클래스뷰의 필요성과 원리를 이해할 때 도움이 된다.
{% highlight python %}
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    
    context = {
        'latest_question_list': latest_question_list,
    }
    
    return HttpResponse(template.render(context, request))
{% endhighlight %}

### 2. render를 이용한 함수형 뷰
아래는 위 함수형 index뷰를 shortcuts.render를 사용하여 보다 간결하게 표현한 모습이다. 
위 코드 중 3번째 줄의 template = … 로 시작하는 부분이 없어지고 제일 하단에 HttpResponse로 리턴하던 것이 render함수로 대체되었다. render함수의 패키지 이름이 왜 shortcuts인지 의문이 풀리는 순간이다.
{% highlight python %}
from django.shortcuts import render

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]

    context = {
        'lastest_question_list': latest_question_list,
    }

    return render(request, 'polls/index.html', context)
{% endhighlight %}

### 3. 제네릭 뷰 시스템(Generic views system)
위의 반복되는 공통 부분을 패턴화하여 사용하기 쉽게 추상화 해두었다. 아래와 같이 최소한 그 뷰가 어떤 모델을 사용할 것인지만 지정해주면 나머진 모두 상위 클래스(여기서는 List generic view)가 모든 걸 알아서 해준다.
{% highlight python %}
from django.views.generic import ListView

class IndexView(ListView):
    model = Question
{% endhighlight %}
필요에 따라 각 뷰마다 달라지는 값을 넣어주기만 하면 된다. 이게 상속을 사용할 수 있는 클래스뷰를 사용하는 힘이자 이유다. 코드를 쓰는 것 자체가 목적이 아닌 도구임을 생각해보면, Django를 왜 쓰는가에 대해 고민했던 사람이라면 제네릭뷰를 활용한 구현은 두 손들어 환영할만한 훌륭한 방법이다.

---

[Django 튜토리얼](https://docs.djangoproject.com/en/1.10/intro/tutorial04/)에서는 처음부터 클래스뷰 사용법을 알려주지 않고 함수형뷰의 초기 형태에서 리팩토링하는 방법을 의도적으로 구성하고 있는데 그 이유를 다음과 같이 적고 있다.

>“Why the code-shuffle?

“왜 자꾸 코드를 바꿔대나요?
>Generally, when writing a Django app, you’ll evaluate whether generic views are a good fit for your problem, and you’ll use them from the beginning, rather than refactoring your code halfway through. But this tutorial intentionally has focused on writing the views “the hard way” until now, to focus on core concepts.

일반적으로 우리가 장고 앱을 작성할 때는 먼저 제네릭 뷰가 문제를 해결할 수 있는지 생각한 후 바로 사용하게 될 겁니다. 힘들게 코드를 리팩토링해가는 대신 말이죠. 하지만 이 튜토리얼은 핵심 개념을 잡아주기 위해 의도적으로 순차적으로 리팩토링을 해가며 뷰를 작성했습니다.
>
>You should know basic math before you start using a calculator.”

계산기를 쓰기 전에 산수부터 익혀야 하니까요.

### 4. 그럼 클래스형 뷰가 함수형 뷰를 완전히 대체하는 건가요?
결론부터 이야기하면 그렇지 않다. 클래스형 뷰와 함수형 뷰는 상황에 따라 선택하여 쓰는 것이지 어느 한쪽을 더 진보된 형태로 말하기 힘들다. 훌륭한 Django 인사이트를 제공하는 [simpleisbetterthancomplex.com](https://simpleisbetterthancomplex.com)의 Vitor Freitas는 클래스형 뷰보다 함수형 뷰를 더 선호한다. 여러가지 이유가 있지만 읽기 쉽고 코드의 흐름이 명료하다는 점을 그는 큰 장점으로 꼽는다. 아래는 그가 그의 사이트에서 두 뷰의 장, 단점을 비교한 것을 번역한 것이다. 원문은 [여기](https://simpleisbetterthancomplex.com/article/2017/03/21/class-based-views-vs-function-based-views.html)를 누르면 볼 수 있다.
    
[![class_func_pros_and_cons.png](https://s26.postimg.org/4o57w84u1/class_func_pros_and_cons.png)](https://postimg.org/image/acbin496d/)

