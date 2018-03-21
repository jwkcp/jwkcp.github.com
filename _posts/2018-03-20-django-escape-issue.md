---
layout: post
title: 장고(django)에서 넘겨받은 값에 html이 그대로 보인다면
tags: how-to, django
comments: true
---
  
## 문제
예를 들어, 장고 UserCreationForm 클래스의 맴버인 password1의 help_text를 출력해보면 {{ password1.help_text }} 아래와 같이 html 코드가 그대로 보이는 경우가 있다.  
~~~
<ul><li>다른 개인정보와 비슷한 비밀번호는 사용할 수 없습니다.</li><li>비밀번호는 최소 8자 이상이어야 합니다.</li><li>비밀번호는 일상적으로 사용되는 비밀번호일 수 없습니다.</li><li>비밀번호는 전부 숫자로 할 수 없습니다.</li></ul>
~~~

## 원인
장고의 이스케이프(escape) 기능이 기본으로 켜져있기 때문이다. 아래와 같이 처리하는 식이다. [이 사이트](https://www.freeformatter.com/html-escape.html)에서 html 태그가 어떤 식으로 이스케이프되는지 테스트해 볼 수 있다.
~~~
" is replaced with &quot;
& is replaced with &amp;
< is replaced with &lt;
> is replaced with &gt;
~~~

## 해결
장고는 기본적으로 이스케이프 기능이 활성화 되어 있다. 그래서 password1의 help_text가 브라우저에 렌더링되지 않고 그대로 보일 수 있었던 것이다. 아래와 같이 2가지 방법으로 html 코드 그대로가 아닌 렌더링된 모습을 보이도록 할 수 있다.   
  
#### 태그(Tag) 사용하기
> 아래 ( 기호를 { 으로 바꾸고 사용하세요.  
  
~~~
<!-- 이스케이프 켜기: html 코드가 그대로 보임 -->
(% autoescape on %){{ password1.help_text }}(% endautoescape %)
  
<!-- 이스케이프 끄기: html 코드가 예쁘게 렌더링되어 보임 -->
(% autoescape off %){{ password1.help_text }}(% endautoescape %)
~~~
  
#### 필터(filter) 사용하기
위와 같이 태그를 사용하지 않고 간단히 필터를 쓸 수도 있다.  
~~~
<!-- 이스케이프 켜기: html 코드가 그대로 보임 -->
{{ password1.help_text|escape }}
  
<!-- 이스케이프 끄기: html 코드가 예쁘게 렌더링되어 보임 -->
{{ password1.help_text|safe }}
~~~
  
---
  
> 이스케이프가 장고에서 기본으로 활성화되어 있는 이유는 보안 때문이기도 하다. 안전이 보장되지 않은 문자에 함부로 safe 필터를 적용하거나 autoescape off를 하게 되면 XSS(Cross-site Scripting)과 같은 공격에 취약해질 수 있으니 주의하자.