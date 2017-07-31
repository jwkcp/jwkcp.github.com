---
layout: post
title: 부트스트랩(Bootstrap) 드롭다운(Dropdown)에서 텍스트 및 값(value) 가져오기 
tags: bootstrap, how-to, dropdown 
comments: true
---

### 아래와 같은 코드로 부트스트랩을 이용해 드롭다운 컨트롤을 만들었다면
{% highlight html %}
<div class="dropdown">
    <button class="btn btn-default dropdown-toggle" id="mystatus" value="title" type="button" data-toggle="dropdown" aria-expanded="true">
    제목
    <span class="caret"></span>
    </button>
    <ul id="mytype" class="dropdown-menu" role="menu" aria-labelledby="searchType">
        <li role="presentation">
            <a role="menuitem" tabindex="-1" href="#" value="a">A</a>
        </li>
        <li role="presentation">
            <a role="menuitem" tabindex="-1" href="#" value="b">B</a>
        </li>
        <li role="presentation">
            <a role="menuitem" tabindex="-1" href="#" value="c">C</a>
        </li>
    </ul>
</div>
{% endhighlight %} 
  
### 다음과 같은 코드로 텍스트(A)와 값(a)를 가져올 수 있다.
{% highlight javascript %}
$('#mytype li > a').on('click', function() {
    // 버튼에 선택된 항목 텍스트 넣기 
    $('#mystatus').text($(this).text());
        
    // 선택된 항목 값(value) 얻기
    alert($(this).attr('value'));
});
{% endhighlight %}
