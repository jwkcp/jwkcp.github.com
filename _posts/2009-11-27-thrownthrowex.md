---
layout: post
title: C# “throw”와 “throw ex” !! 도대체 뭐가 다르냐구!!
tags: c#
comments: true
---
[![이미지 출처: Umedy](https://s26.postimg.org/fymp5uc1l/88750_c444_7.jpg)](https://postimg.org/image/ye7638q5x/)

`이 글은 'Difference between "throw" and "throw ex" in .NET'라는 글을 번역해 2009년에 쓰여졌습니다. 번역 상의 오류가 있을 수 있습니다. 아래 링크를 클릭하시면 원문 기사를 보실 수 있습니다.`

원문 기사: [Difference between "throw" and "throw ex" in .NET](http://scottdorman.github.io/2007/08/20/Difference-between-quotthrowquot-and-quotthrow-exquot-in-.NET/ "Difference between "throw" and "throw ex" in .NET")

---

예외처리는 .NET개발자들.. 특히, 초보 프로그래머들이 공통적으로 겪는 문제입니다. 에러가 날 것 같은 부분을 처리하기 원할 때 try/catch로 묶는 것은 다들 알고 있지요?

여기서는 이런 예외처리 시 준수해야 하는 규칙이나 가이드라인에 대해 이야기하려는 게 아니고, 예외가 발생한 곳부터 최종 호출지점 혹은, 예외를 처리하기 원하는 지점까지 버블링 – 뽀글뽀글 거품처럼 예외를 전달하는 – 을 하는 방법에 대해 이야기하려 합니다.

예외를 버블링(계속 전달하는..)한다는 건, 어떤 코드에서 예외를 잡고 이를 처리하려 할 때 호출한 녀석에게도 이를 처리할 기회를 준다는 거라고 보면 됩니다. 굉장히 일반적인 방법이지만 디버깅 시 중대한 문제를 만들어 낼 잠재적 가능성이 있으니 아래의 글을 잘 읽어주세요.

대부분의 코드를 아래와 비슷하게 작성 할 겁니다.

{% highlight c# %}
try
{
    // do some operation that can fail
}
catch (Exception ex)
{
    // do some local cleanup
    throw ex;
}
{% endhighlight %}

멋진 코드입니다. 모든 구문들이 면밀히 자기의 역할을 수행하고 있어요.

적절하게 예외도 잡아내고 있고, 이를 잘 처리해 주고 있습니다. 예외를 버블링 하기 위한 코도 있군요!! (이 것은 이야기의 논지를 흐트러트리지 않기 위해 모든 예외를 잡고 있지만 실제 코드에서는 처리하려는 예외를 지정해서 잡아내도록 하세요. 꼭 그렇게 하세요!)

계속 봅시다. 아래의 코드도 많이 봤을 겁니다.

{% highlight c# %}
try
{
    // do some operation that can fail
}
catch (Exception ex)
{
    // do some local cleanup
    throw;
}
{% endhighlight %}

이전 예제랑 별반 차이 없어 보입니다. 근데 뭔가 더 있습니다.
이제부터가 핵심입니다. 잘 보세요.

잘 살펴보지 않으면 찾아 낼 수 없는 아주 작은 차이가 보이시나요?

바로, 던져진 예외 안에 스택 추적 정보(stack trace information)가 있는지의 여부입니다.
무슨 말인지 모르시겠다고요?

당연합니다!
아직 이야기를 시작하지도 않았거든요. 더 보세요. ▼

첫 번째 예제의 경우 메서드가 실패한 이후에 스택 추적(stack trace)이 속된말로 ‘쫑’ 납니다. 무슨 말이냐 하면, 뭔가 문제가 생겼거나 어떻게 호출되어왔는지 알아보기 위해 스택 추적 정보(stack trace information)를 들여다 보면 이 예외가 ‘내 코드에서 최초 발생한 것처럼 보인다.’ 이 말입니다. 근데 CLR에서 만들어낸 SqlException 같은 경우에는 항상 이렇지는 않습니다. (눈치 빠르신 분들은 더 이상 안보셔도 됩니다. 근데 뒤에 더 중요한 것이 나옵니다.)

이 문제는 더 이상 완벽한 스택 추적 정보(stack trace information)를 얻을 수 없는 “망가진 스택(breaking the stack)” 문제로 알려져 있습니다. 이 문제는 새로운 예외를 만들어서 던진 코드 때문에 발생합니다.

그러면 “throw”를 사용하면 어떻게 됩니까?
어떻게 될까요?

스택추적정보(stack trace information)가 수천 년의 세월에도 불구하고 그대로 보존된 미이라처럼 한치의 손상 없이 잘 보존됩니다. 못 믿으시겠으면 두 코드 블럭이 생성한 CIL코드를 잘 살펴보세요. 첫 번째 코드는 “throw”를, 두 번째 코드는 “rethrow”를 호출하는 모습이 보입니다.

잠깐!!

아직 모든 코드를 “rethrow”를 호출하는 코드로 바꾸지 말아주세요. 중요한 할 말이 있습니다. “throw ex”를 호출하는 것이 맞는 부분도 있거든요. 어떤 처리를 하기 위해 특정 예외를 잡아냈습니다. 그리고 여기에 뭔가 ‘딱’ 알아 볼 만한 나만의 정보를 추가하거나, 처리를 하고 싶던 때가 있었을 거라고 생각합니다. 이런 경우는 새로운 예외가 생성되어 던져져야 합니다.

자, 집중하세요. 이런 경우 또 두 가지 방법이 있습니다. 아래를 보시죠.

{% highlight c# %}
try
{
    // do some operation that can fail
}
catch (Exception ex)
{
    // do some local cleanup
    throw new ApplicationException(“operation failed!”);
}
{% endhighlight %}

어떤가요? 내 코드랑 비슷한가요? 이 코드 역시 우리가 지금까지 살펴보았던 “망가진 스택(breaking the stack)” 문제가 있습니다. 진짜로 하려고 했던 건 이런 모습일겁니다.

{% highlight c# %}
try
{
    // do some operation that can fail
}
catch (Exception ex)
{
    // do some local cleanup
    throw new ApplicationException(“operation failed!”, ex);
}
{% endhighlight %}

원래의 예외를 새로운 예외의 inner exception으로 전달합니다. 즉, original exception를 ApplicationException으로 전달하면서 original exception와 스택 추적 정보(stack trace information)를 ApplicationException의 inner exception으로 잘 보존하고 있습니다.

마무리 하겠습니다

1. 꼭 필요한 경우에만 예외를 잡자.   
2. 예외를 연속적으로 던져버리고 싶다면(스택추적정보를 깨뜨리지 않고) throw를 사용하자.   
3. 잡은 예외에 뭔가 더해서 던지고 싶다면 항상 inner exception으로 original exception을 전달하자.   

---

`이 글은 'Difference between "throw" and "throw ex" in .NET'라는 글을 번역해 2009년에 쓰여졌습니다. 번역 상의 오류가 있을 수 있습니다. 아래 링크를 클릭하시면 원문 기사를 보실 수 있습니다.`

원문 기사: [Difference between "throw" and "throw ex" in .NET](http://scottdorman.github.io/2007/08/20/Difference-between-quotthrowquot-and-quotthrow-exquot-in-.NET/ "Difference between "throw" and "throw ex" in .NET")