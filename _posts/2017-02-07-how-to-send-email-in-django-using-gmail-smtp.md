---
layout: post
title: Django에서 SMTP를 이용해 이메일 발송하는 방법
tags: django google smtp
comments: true
---
Django에서 비밀번호 리셋 기능은 사용자의 이메일로 리셋 링크를 전송하는 방법을 사용한다. 테스트용으로 콘솔에 이메일 발송을 찍어볼 수 있고, 실제 SMTP 서버를 이용해 발송을 해볼 수 있다. 여기서는 테스트 발송을 콘솔에 찍어보는 방법과 지메일 SMTP를 이용해 실제 발송을 하는 방법을 살펴보도록 한다.   
   
---

## 콘솔에 찍어 테스트 발송하는 방법
Django > settings.py에 아래 항목을 추가한다.

{% highlight python %}
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'  # development only
{% endhighlight %}

--

## 지메일 SMTP를 이용해 발송하는 방법
Django > settings.py에 아래 항목을 추가한다. 콘솔에 찍었던 코드는 주석처리하거나 삭제하도록 한다.

{% highlight python %}
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_HOST_USER = '내 이메일 아이디@gmail.com'
EMAIL_HOST_PASSWORD = '비밀번호'
EMAIL_USE_TLS = True
DEFAULT_FROM_EMAIL = '보내는 사람이름 <내 이메일 아이디@gmail.com>'
{% endhighlight %}
   
---

위와 같이 설정값을 넣고 발송을 해보면 지메일에서 보안이 취약한 앱에서 로그인 시도를 했다는 메시지를 이메일로 보내는 경우가 있다. SSL 연결이 없어서 그런 것인데 아래와 같이 **임시**로 설정을 바꾼 후 SSL없이 지메일에서 발송 테스트를 해볼 수 있다. 실제 서비스 시에는 꼭 SSL을 적용하고 지메일 보안 수준을 원래대로 돌려놓도록 하자. 

[![google_smtp_security_level.png](https://s26.postimg.org/cw9eijwxl/google_smtp_security_level.png)](https://postimg.org/image/hi5iqwigl/)