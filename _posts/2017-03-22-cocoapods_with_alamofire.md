---
layout: post
title: 코코아포드(cocoapods) 초간단 설치 및 사용법
tags: ios, swift, cocoapods, alamofire
comments: true
---
## 소개
코코아포드(cocoapods)는 스위프트(swift)와 오브젝티브씨(Objective-c) 개발에 사용되는 의존성 관리자다. 앱 개발을 하다보면 오픈소스 라이브러리의 업데이트에 따른 관리를 지속적으로 해줘야 하는데 사용하는 라이브러리가 늘어나게 되면 관리에 시간이 많이 낭비된다. 코코아포드(cocoapods)는 이런 귀찮은 일을 자동화하고 시간을 절약하게 해준다. 여러 의존성 관리자가 있지만 코코아포드(cocoapods)는 가장 널리 쓰이는 도구 중 하나이므로 처음 의존성 관리자 도구를 알아보고 있다면 믿고 사용해도 좋다. 아래는 [코코아포드(cocoapods)](https://cocoapods.org/) 사이트의 설명이다.

>CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 29 thousand libraries and is used in over 1.8 million apps. CocoaPods can help you scale your projects elegantly.
   
## 설치 및 사용 방법
코코아포드(cocoapods)가 사용된지 몇 년이 지났고 그동안 많은 업데이트가 이뤄져 왔다. 과거 수동으로 작업해야 했던 부분도 명령어 하나로 가능하도록 바뀐 부분이 있으니 인터넷에서 관련 자료를 찾고 있다면 포스팅된 날짜를 보고 너무 오래됐으면 더 간단하고 좋은 방법이 있는지 찾아보는 것도 좋겠다. 아래는 간단히 나열한 설치 방법이다.

#### 1. 설치하기
{% highlight shell %}
sudo gem install cocoapods
{% endhighlight %}
  
#### 2. 필요한 파일 다운로드 및 준비
{% highlight shell %}
pod setup
{% endhighlight %}
  
#### 3. 의존성 관리를 할 Xcode 프로젝트 폴더에서 아래 명령 실행  
*(이 명령을 실행하면 Podfile이 생성됩니다.)*
{% highlight shell %}
pod init
{% endhighlight %}
  
#### 4. Podfile 편집
{% highlight shell %}
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'cocoapods_test2' do
  # 스위프트를 사용하지 않고 동적 라이브러리를 이용하지 않는다면 아래 구문을 주석처리 합니다
  use_frameworks!

  # 여기에 설치할 라이브러리를 나열합니다.
  pod 'Alamofire'

end
{% endhighlight %}
   
#### 5. Podfile에서 나열한 라이브러리 설치하기
{% highlight shell %}
pod install
{% endhighlight %}
  
#### 6. 여기까지 잘 따라왔다면 Xcode 프로젝트 폴더에 '프로젝트명.xcworkspace'라는 파일이 보일겁니다.
  
#### 7. 이 '프로젝트명.xcworkspace' 파일을 더블 클릭하여 실행하면 라이브러리를 사용할 준비가 완료됩니다

---
  
## 뭔가 잘 안된다면...
  
#### 1. 프로젝트 하위 파일이 안보입니다.  
'프로젝트명.xcworkspace' 파일을 더블 클릭하여 실행했을 때 아래 캡춰처럼 파일이 보이지 않는다면 Xcode의 다른 창에서 해당 프로젝트가 열려 있을 수 있습니다. 그 창을 닫고 다시 '프로젝트명.xcworkspace'를 실행해보세요.

[![cocoapods_nofiles.png](https://s26.postimg.org/76qwwwqk9/cocoapods_nofiles.png)](https://postimg.org/image/e9yscivzp/)
  
#### 2. import Alamofire 했을 때 에러가 뜬다면  
cannot load underlying module for 'alamofire'라는 에러가 발생할 수 있습니다. Xcode에서 'Project' > 'Clean'을 실행한다음 Xcode를 원전히 종료하고 다시 실행해보세요. (여러가지 원인이 있을 수 있어 이렇게 해도 해결되지 않는 경우가 있을 수 있습니다.)
  
[![cocoapods_import_error.png](https://s26.postimg.org/7ks8wiant/cocoapods_import_error.png)](https://postimg.org/image/voj0kst4l/)
  
---
  
## 참고자료
  
1. [코코아포드(cocoapods)](https://cocoapods.org/)
2. [Alamofire Github](https://github.com/Alamofire/Alamofire)
3. [SwiftJSON Github](https://github.com/SwiftyJSON/SwiftyJSON)
  