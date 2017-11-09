---
layout: post
title: 파이썬을 이용해 REST API 호출해보기
tags: python, restapi, how-to
comments: true
---

예를 들어, 카카오에서 블로그 검색을 하는 REST API를 호출한다고 가정해보자. 아래와 같은 Request를 보내야 한다. 앱키(app_key)는 **헤더** 에 담고 검색어 등은 파라메터로 보낸다. 이 작업을 파이썬에서는 어떻게 해야 할까.

{% highlight python %}   
GET /v2/search/blog HTTP/1.1
Host: dapi.kakao.com
Authorization: KakaoAK {app_key}
{% endhighlight %}

파라메터 키는 query, sort, page, size가 있다. (아래와 같이 curl을 통해서도 요청할 수 있다.)

{% highlight python %}
curl -v -X GET "https://dapi.kakao.com/v2/search/blog" \
--data-urlencode "query=https://brunch.co.kr/@tourism 집짓기" \
-H "Authorization: KakaoAK kkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkk"
{% endhighlight %}

---

우선, 헤더와 파라메터를 사전형으로 아래와 같이 만든다.

{% highlight python %}
headers = {
    'Content-Type': 'application/json; charset=utf-8',
    'Authorization': 'KakaoAK 자기가 생성한 앱의 앱키를 여기에 넣는다')
}
params = {
    'sort': 'accuracy',
    'size': 50,
    'query': keyword
}
{% endhighlight %}

그리고 아래와 같이 요청을 보내면 응답 값으로 JSON이 리턴된다.

{% highlight python %}
result = requests.get("https://dapi.kakao.com/v2/search/blog", headers=headers, params=params)
{% endhighlight %}
