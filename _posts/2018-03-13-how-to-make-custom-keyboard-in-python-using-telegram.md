---
layout: post
title: 텔레그램 봇 구현 시 커스텀 키보드 만드는 초간단 방법
tags: how-to, python, telegram, bot
comments: true
---
  
## 어떻게 동작하나
아래와 같이 텔레그램 봇은 채팅방에 메시지를 보낼 수 있는데 여기서 주목해야 할 부분은 **reply_markup**다.   
https://api.telegram.org/bot{TOKEN}/sendmessage?text={MESSAGE}&chat_id={CHATID}&reply_markup={KEYBOARD}
  
## 만드는 방법은
여기(KEYBOARD)에 만들고 싶은 키보드 형태를 아래와 같이 넣어준다.  
~~~
{"keyboard": [["Done","Done 3"], ["Update"], ["Log Time"]]}  
~~~
  
한 줄로 쓰니 좀 복잡해보인다면 아래처럼 풀어서 보면 어떤 중첩 구조를 가지는지 쉽게 알 수 있다.  
~~~
{
    "keyboard": 
    [
        ["A","B"], 
        ["C","D"]
    ]
}
~~~
  
## 결과
그러면 아래와 같이 나온다.  
![Keyboard Example](https://core.telegram.org/file/811140733/2/KoysqJKQ_kI/a1ee46a377796c3961)
  
## 참고 자료
[https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating](https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating)