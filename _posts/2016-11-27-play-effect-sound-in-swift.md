---
layout: post
title: 스위프트(Swift)로 초간단 효과음 재생하기
tags: swift snippet
comments: true
---
스위프트(Swift)를 처음 시작하는 분들 중 연습삼아 간단한 효과음을 재생하고 싶어하는 분들에게 도움이 될만한 코드 조각(Code snippet)과 무료 효과음 사이트를 소개합니다.

*테스트 환경: Swift 3.x, Xcode 8.x*

---

## 코드 (1/3)
* 오디오 재생을 위해 관련 프레임워크를 임포트합니다.
{% highlight swift %}
import AVFoundation
{% endhighlight %}

* 오디오 플레이어를 선언하고,
{% highlight swift %}
var soundIntroPlayer = AVAudioPlayer()
{% endhighlight %}

* 경로를 문자열로 설정 (기존 NSBundle이 Bundle 클래스로 바뀜)
{% highlight swift %}
let soundintro = Bundle.main.path(forResource: "Ring", ofType: "mp3")
{% endhighlight %}

* 오디오 플레이어 객체 생성과 오디오 세션 준비
{% highlight swift %}
do {
        soundIntroPlayer = try AVAudioPlayer(contentsOf: URL(fileURLWithPath: soundintro! ))
            
        try AVAudioSession.sharedInstance().setCategory(AVAudioSessionCategoryAmbient)
        try AVAudioSession.sharedInstance().setActive(true)
    }
    catch{
         print(error)
    }
{% endhighlight %}

* 재생
{% highlight swift %}
soundIntroPlayer.play()
{% endhighlight %}

---

## 무료 효과음 사이트 (2/3)

[Freesoundeffects.com](https://www.freesoundeffects.com/free-sounds/drum-loops-10031/ "Freesoundeffects.com")

## 참고자료 (3/3)

[stackoverflow.com](http://stackoverflow.com/questions/32036146/how-to-play-a-sound-in-swift-2-and-3 "stackoverflow.com")