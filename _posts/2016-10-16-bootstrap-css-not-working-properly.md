---
layout: post
title: 부트스트랩을 사용할 때 커스텀 CSS 레이아웃이 잘 적용되지 않는다면
tags: django bootstrap css how-to
comments: true
---
`주의! 아래 글 중 중괄호와 퍼센트 사인 사이의 공백은 실제 코드에서 제거되어야 합니다. jekyll 문법에서 인식 오류가 발생해 공백이 삽입되어 있습니다`

부트스트랩 사이트에서 제공하는 기본 레이아웃에, 장고(Django)의 html 상속과 css 중첩 구조를 사용할 때 실수하기 쉬운 부분을 설명하기 위해 몇 줄을 추가했다.

**1. css는 소스에 기재된 순서대로 덮어쓰기 되어 적용된다.**   
부트스트랩(bootstrap) css를 마음대로 바꾸려고 했는데 적용이 되지 않는다면 이 순서가 잘못되어 있을 가능성이 크다. 아래 bootstrap.min.css를 base.css나 { % block extrastyle % }보다 나중에 쓰면 bootstrap.min.css가 이들 css를 모두 덮어쓰기 때문에 어떤 css 변경도 적용되지 않는다.

**2. 자바스크립트(javascript)도 순서가 중요하다.**   
자바스크립트(javascript)도 마찬가지인데 제이쿼리(jquery)보다 부트스트랩을 먼저 써주면 부트스트랩의 자바스크립트 기능이 동작하지 않는다.

CSS와 자바스크립트 모두 기재되는 **순서가 중요**하다.

{% highlight html %}
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 위 3개의 메타 태그는 *반드시* head 태그의 처음에 와야합니다; 어떤 다른 콘텐츠들은 반드시 이 태그들 *다음에* 와야 합니다 -->
    <title>부트스트랩 101 템플릿</title>

    <!-- 부트스트랩 -->
    { % load static % }
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">
    <link rel="stylesheet" type="text/css" href="{ % static "css/base.css" % }">
    <link rel="stylesheet" type="text/css" href="{ % block extrastyle % }{ % endblock % }">

    <!-- IE8 에서 HTML5 요소와 미디어 쿼리를 위한 HTML5 shim 와 Respond.js -->
    <!-- WARNING: Respond.js 는 당신이 file:// 을 통해 페이지를 볼 때는 동작하지 않습니다. -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- jQuery (부트스트랩의 자바스크립트 플러그인을 위해 필요합니다) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js"></script>
    <!-- 모든 컴파일된 플러그인을 포함합니다 (아래), 원하지 않는다면 필요한 각각의 파일을 포함하세요 -->
    <script src="js/bootstrap.min.js"></script>
  </body>
</html>
{% endhighlight %}

---

### 장고 사용자를 위한 추가 팁
    
1. django template 문법인 { % static “경로” % }를 사용하려면 static 태그를 사용할 수 있도록 { % load staticfiles % }를 써줘야 한다. 그렇지 않으면 경로를 찾지 못하는 문제가 생긴다.   

2. html 상속 구조에서 하위 html에 적용될 css를 구현하는 사람이 자유롭게 변경하도록 하고 싶다면 14번째 줄을 눈여겨보자. 이렇게 구현하면 아래와 같이 상속 구조 하위에 위치한 html에서 자신의 css를 쉽게 불러 쓸 수 있다.

{% highlight python %}
{ % load staticfiles % }
{ % block extrastyle % }
    { % static "css/todo_list.css" % }
{ % endblock % }
{% endhighlight %}