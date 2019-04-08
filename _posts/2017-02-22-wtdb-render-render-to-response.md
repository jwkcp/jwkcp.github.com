---
layout: post
title: Django의 render()와 render_to_response() 차이
tags: django
comments: true
---
Django에서 뷰를 통해 템플릿 렌더링(rendering)을 하려면 크게 2개지 함수가 눈에 띈다. render()와 render_to_response()가 그것. 대충 보면 렌더링할 템플릿명과 값을 딕셔너리 형태로 넘겨주는 것이 똑같기 때문에 큰 차이가 없어 보인다. 그럼 어떤게 최신인가? 어떤 방법이 더 권장되나? 하는 궁금증이 생긴다. 아래에 그 차이와 예제를 간단히 써봤다. 상황에 따라 골라쓰면 되지만 더 권장되는 방법도 있으니 눈여겨 보자.
   
***
   
## **비교1**: render()
### 함수 선언(Function declaration)
{% highlight python %}
{% raw %}
render(request, template_name, context=None, content_type=None, status=None, using=None)
{% endraw %}
{% endhighlight %}

### 사용법
{% highlight python %}
{% raw %}
from django.shortcuts import render

def my_view(request):
    # View code here...
    return render(request, 'myapp/index.html', {'foo': 'bar'})
{% endraw %}
{% endhighlight %}

--

## **비교2**: render_to_response()
### 함수 선언(Function declaration)
{% highlight python %}
{% raw %}
render_to_response(template_name, context=None, content_type=None, status=None, using=None)
{% endraw %}
{% endhighlight %}

### 사용법
{% highlight python %}
{% raw %}
from django.shortcuts import render_to_response

def my_view(request):
    # View code here...
    return render_to_response('myapp/index.html', {'foo': 'bar'})
{% endraw %}
{% endhighlight %}

***
   
## 해설
위 '사용법'의 마지막 줄을 보면 render_to_response() 함수에서는 안보이는 request 매개변수가 render() 함수에 보인다. request 변수가 없으면 렌더링된 템플릿에서 user, session, site 등 [유용하고 중요한 값](https://docs.djangoproject.com/en/1.10/ref/request-response/)을 사용하기 힘들어진다. 물론 굳이 render_to_response() 함수를 써서 하고 싶다면 가능한 방법은 있다. 아래처럼 context_instance를 전달해주는 것이다.

{% highlight python %}
{% raw %}
from django.shortcuts import render_to_response

def my_view(request):
    # View code here...
    return render_to_response('myapp/index.html', {'foo': 'bar'}, context_instance=RequestContext(request))
{% endraw %}
{% endhighlight %}

그러나 render() 함수를 써서 request를 첫번째 인자로 전달만 하면 되는데 굳이 불편하고 긴 코드를 쓸 필요는 없다. Django 공식 튜토리얼에서도 render_to_response() 함수의 사용을 권장하지 않고 있으며 향후 사라지게(deprecated) 될 것이라고 표기하고 있으니 고민하지 말고 render() 함수 쓰자.

> render_to_response()
> This function preceded the introduction of render() and works similarly except that it doesn’t make the request available in the response. It’s not recommended and is likely to be deprecated in the future.

***
   
## 한줄 요약
  
render() 함수 쓰세요.
      
***   
   
> 참고링크
- [Django 공식 튜토리얼: render(), render_to_response()](https://docs.djangoproject.com/en/1.10/topics/http/shortcuts/)
- [Django 공식 튜토리얼: RequestContext](https://docs.djangoproject.com/en/1.10/ref/templates/api/#django.template.RequestContext)
