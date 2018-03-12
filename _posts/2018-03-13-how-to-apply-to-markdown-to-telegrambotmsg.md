---
layout: post
title: 텔레그램 봇 메시지에 마크다운 적용하는 방법
tags: how-to, telegram
comments: true
---
  
마크다운이나 HTML 스타일로 텔레그램 봇 메시지를 보낼 수 있다. 

## 방법
URL 파라메터로 parse_mode를 지정하고 text의 값에 마크다운이나 HTML을 넣으면 된다. 현재(2018.03) markdown과 html 두 가지를 지원한다.
  
~~~
https://api.telegram.org/bot{TOKEN}/sendmessage?text={MSG}&chat_id=098765432&parse_mode="markdown"
~~~
  
## 적용 가능 범위
모든 구문이 가능한 것은 아니고 아래와 같은 간단한 구문만 적용 가능하다. 더 자세한 내용은 [공식 문서](https://core.telegram.org/bots/api#markdown-style)를 참고.
  
**마크다운(markdown) 스타일**
~~~
*bold text*
_italic text_
[inline URL](http://www.example.com/)
[inline mention of a user](tg://user?id=123456789)
`inline fixed-width code`
```block_language
pre-formatted fixed-width code block
```
~~~
  
**HTML 스타일**
~~~
<b>bold</b>, <strong>bold</strong>
<i>italic</i>, <em>italic</em>
<a href="http://www.example.com/">inline URL</a>
<a href="tg://user?id=123456789">inline mention of a user</a>
<code>inline fixed-width code</code>
<pre>pre-formatted fixed-width code block</pre>
~~~
  
## 주의사항
1. 아래와 같이 마크다운 링크 구문에 한글을 적용하려고 하니 잘 안되었다. (GET, POST 요청 툴이나 URL에 실어 보내면 정상적으로 적용됨) 유니코드 인코딩 때문인 듯
   ~~~
   예) [예제문서](http://example.com)
   ~~~
2. [공식 문서](https://core.telegram.org/bots/api#markdown-style)의 HTML관련 안내에 따르면 일부 태그만 지원하며 태그는 중첩되면 안되고 적절한 대체 표기를 사용해야 한다. 
  