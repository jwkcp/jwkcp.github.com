---
layout: post
title: Swift에서 웹뷰(webview) 탐색 코드 조각
tags: swift, ios, code-snippet
comments: true
---

#### 웹뷰로 페이지 탐색하기 초간단 예제
  
아래 예제 코드와 같이 https가 아닌 http 접속도 허용하려면 [여기](https://jwkcp.github.io/2017/03/22/ios-swift-http-allow/)를 참고하여 설정을 추가해주어야 한다.
  
{% highlight swift %}
wvBrowser.loadRequest(URLRequest.init(url: URL(string: "http://www.daum.net")!))
{% endhighlight %}
