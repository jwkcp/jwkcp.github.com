---
layout: post
title: 스위프트에서 JSON 파싱하기
tags: swift json how-to snippet
comments: true
---
자주 쓸 것 같아 책보고 처음 작성해보고 저장용으로 기록해 둠. (Swift 3.x, Xcode 8.x)

{% highlight swift %}
if let url = URL(string: "http://ip.jsontest.com/") {
            
    let urlSession = URLSession.shared
    let task = urlSession.dataTask(with: url, completionHandler: {
        (data, response, error) in
                
        if let nsstr = NSString(data: data!, encoding: String.Encoding.utf8.rawValue) {
            let str = String(nsstr)
            // Print raw string
            print(str)
                    
            do {
                let dic = try JSONSerialization.jsonObject(
                    with: str.data(using: String.Encoding.utf8)!,
                    options:JSONSerialization.ReadingOptions.mutableContainers)
                as! NSDictionary
                        
                // Print dictionary
                print(dic)
                        
                if let ipAddress = dic["ip"] {        
                    // Print value
                    print(ipAddress)
                }
            } catch {
                print("error occured")
            }
        }
    })
            
    task.resume()
}
{% endhighlight %}

- [JSONlint](http://jsonlint.com/ "JSONlint") – JSON 문법 검사기
- [JSONTest](http://www.jsontest.com/ "JSONTest") – JSON 테스트 문자열 서비스
