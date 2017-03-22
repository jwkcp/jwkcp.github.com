---
layout: post
title: Swift에서 http(non-ssl) 통신 허용 설정
tags: swift, ios, code-snippet
comments: true
---

#### info.plist에 아래의 설정을 붙여넣기 한다.
  
1. 붙여넣기 좋도록 편하도록 info.plist에서 마우스 오른쪽 버튼을 누르면 'Open As' 메뉴가 나오고 Source Code를 선택한다.
2. 마지막 </dict> 바로 앞에 붙여 넣는다.
  
{% highlight swift %}
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key><true/>
</dict>
{% endhighlight %}