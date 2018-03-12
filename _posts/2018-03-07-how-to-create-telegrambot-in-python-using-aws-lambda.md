---
layout: post
title: 서버없이 텔레그램(Telegram) 챗봇 만드는 방법과 순서(Python, AWS lambda)
tags: python, bot, aws, lambda
comments: true
---
  
### 시작하기
텔레그램 챗봇을 구현해보기 전에 아래 사항을 확인해보자. 
  
**1. 봇 만들고 토큰(Token)받기**
@BotFather를 텔레그램에서 검색하여 /newbot 을 입력, 새로운 봇 이름과 아이디 등을 지정하고 **토큰(Token)**을 받는다. 이 때, 발급받은 토근은 일종의 비밀번호와 같이 '나'를 증명하는 역할을 하므로 아무한테도 알려주면 안된다. github 등에 실수로 올리지 않도록 주의한다.  

이제부터 표기되는 **TOKEN** 자리에는 토큰 앞에 bot 이란 문자열을 붙인 값으로 치환하여 사용하도록 하자.   
e.g. bot0912898821989823917jfio2jfudhausf
  
**2. 테스트 해보기**
봇이 잘 생성되었는지 정보 가져와보기
> https://api.telegram.org/TOKEN/getME

봇에게 메시지를 보내보자. 아래 주소를 호출하면 내가 보낸 메시지가 나온다.
> https://api.telegram.org/TOKEN/getUpdates

봇에게 메시지를 받아보자. chat_id는 위에 getUpdates를 호출했을 때 할 수 있다.  
text의 값에 보내고 싶은 메시지를 쓰면 된다.
> https://api.telegram.org/TOKEN/sendmessage?text=MyMessage&chat_id=98765432
  
여기까지 해봤다면 챗봇의 동작 원리에 대해선 대충 감이 잡힐 것이다. 잠깐 정리해보면 -
  
1. 봇파더(@BotFather)를 통해 만들 봇 정보를 주고 봇을 생성
2. 통신에 사용할 토큰(Token)을 받는다.
3. 이 토큰을 이용해 메시지를 보내면 텔레그램 서버가 메시지가 받고
4. 사용자는 getUpdates를 GET으로 보내 받은 메시지 목록을 받아볼 수 있다. (webhook이라는 방식도 있는데 그건 좀 있다가)
5. 메시지를 보낼 때는 getUpdates에서 확인한 chat_id를 이용해 보낸다.
6. 끝.
  
---
  
## 구현하기
여기까지 하고 나면 봇이 메시지를 보내고 받는 원리는 알았을 것이다. 이제 메시지를 '보내고 받는' 과정에서 더 해야할 일이 무엇인지 살펴보자.
  
> **더 해야 할 일**
> 1. getUpdates를 사람이 웹브라우저로 계속 호출할 수 없으니 '봇(컴퓨터)'에게 시켜야 한다. -> 반복문(loop) or 웹훅(webhook)
> 2. 프로그램 코드를 올릴 '서버'가 필요하다. -> EC2, 라즈베리파이 등
  
이 두 가지를 차근 차근 살펴본다.
  
#### 1. getUpdates를 사람이 웹브라우저로 계속 호출할 수 없으니 '봇(컴퓨터)'에게 시켜야 한다. -> 반복문(loop) or 웹훅(webhook)
메시지를 받는 일은 getUpdates가 한다고 앞서 말했다. 하지만 언제 메시지가 올지 모르니 getUpdates를 계속 실행해주어야 새로운 메시지가 도착하는지 알 수 있을 것이다. 이것을 폴링(polling)한다고 하는데 프로그래밍에서는 반복문이다. getUpdates를 특정 주기로 반복실행하면서 새로운 메시지가 도착했는지 확인하는 것이다. 이런 일련의 작업을 좀 편하게 할 수 있도록 유명한 라이브러리([teleport](https://github.com/gravitational/teleport), [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot)들이 있다. 이런 라이브러리에서 start_polling()과 같은 함수가 getUpdates를 실행하는 반복문 함수라고 보면 된다.  
  
그런데 또 이런 의문이 든다. 비효율적으로 어떻게 계속 루프를 돌고 있는가. 챗봇과 메시지를 주고 받을 때보다 안그럴 때가 더 많다면 자원 낭비이기도 하다. 그래서 텔레그램은 웹훅(webhook)이란 기능을 제공한다. 봇에게 누군가 톡을 보내면 미리 등록한 주소(url)로 알려주겠다는 것이다. 푸시 알림을 해주는 것이라고 보면 된다.   
  
이제 자신이 원하는 방법이 반복문인지, 웹훅인지 결정하도록 하자.
  
#### 2. 프로그램 코드를 올릴 '서버'가 필요하다. -> EC2, 라즈베리파이 등  
**무엇이든 서버로 쓸 수 있지만**  
텔레그램 서버와 메시지를 주고 받는 작업을 중개하고, 그 사이에 내가 원하는 기능을 수행하도록 할 서버가 필요하다. AWS EC2나 Digital Ocean과 같은 곳의 VPS, 또는 라즈베리파이도 가능하다. 간단히 테스트해 볼 목적이면 자신의 컴퓨터에서 임시 서버를 띄워 테스트해봐도 된다.
  
**AWS lambda(람다)를 이용해 서버없는 서비스로**  
그런데 간단한 챗봇 돌리려고 EC2 한대를 통채로 쓰는 건 좀 과하기도 하고 돈 낭비이기도 하다. 여기서는 AWS의 서버리스 서비스인 lambda(람다)를 이용하기로 한다. lambda는 서버없이 서비스를 구현하도록 만든 서비스다. C#, Node.js, python2, python3 등 다양한 언어를 지원한다. 여기서는 python3를 사용하기 한다.
  
**특별한게 없지만 강력하고 싼 AWS lambda**  
AWS lambda는 핸들러라고 부르는 함수에 동작 코드를 작성해서 넣는데 핸들러 함수는 처음 자동으로 완성되어 있는 뼈대를 그대로 사용해도 되고, def test(event, context)와 같이 자신이 만들어줘도 된다. event와 context를 매개변수로 받게 되어 있으니 잊지 말자. 더 많은 정보는 [여기](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/python-context-object.html)를 참고하고 event 객체에 어떤 항목들이 포함되어 있는지 보려면 [여기](https://jwkcp.github.io/2018/03/07/aws-lambda-event-object/)를 참고하면 도움이 된다. 
  
**lambda에 코드를 배포하는 3가지 방법**  
1. 인라인 편집기 (간단한 코드는 그냥 온라인에서 코딩하고 바로 실행)
2. zip 파일 업로드 (외부 라이브러리를 이용하거나 규모가 좀 클 경우)
3. S3 (zip과 마찬가지. 이게 더 편하다면 이 방식으로)

**lambda에서 외부 라이브러리를 이용하려면**  
lambda는 인라인 편집기를 지원한다. 다시 말해 온라인에서 간단히 코딩한다음 바로 실행시킬 수 있다. 하지만 인라인 편집기는 외부 라이브러리를 이용할 수 없다. 외부 라이브러리를 이용하려면 위에 3가지 방법 중 2, 3번을 이용해야 한다. zip 파일을 만들 때 주의사항이 있다. 설치한 외부 라이브러리를 압축할 때 잘못 압축하면 프로그램이 제대로 동작하지 않는다. 아래와 같이 디렉토리 구조와 레벨을 주의해서 압축하자.
  
~~~
RootDirectory
   - lambda_handler.py
   - 외부 라이브러리들1 (파일들)
   - 외부 라이브러리들2 (파일들)
   - ...
~~~
  
외부 라이브러리를 설치할 때 1.가상환경을 이용하는 사람 2.그냥 기본 파이썬 인터프리터를 이용하는 사람 은 파이썬 디렉토리에서 %YOUR_PATH%/lib/site-packages 안에 있는 내용물을 RootDirectory 하위에 복사하고 RootDirectory를 zip으로 만들어야 한다. 
  
가상환경이나 기본 환경을 안쓰고 그냥 프로젝트 폴더에 바로 설치하고 싶은 사람들은 아래와 같이 -t 옵션을 이용하자. 만약 터미널 디렉토리 위치가 RootDirectory 바로 아래에 있다면 아래 명령을 이용하면 된다. (requests를 설치한다고 가정할 경우) 뒤에 쩜(.)은 현재 폴더에 라이브러리를 설치하란 뜻이다. 배포 파일 만드는 법과 관련된 공식 문서는 [여기](https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html) 참고.
  
~~~
pip install requests -t .
~~~

**lambda 함수에는 일단 아무거나**  
당장 어떤 로직을 구현하지 말고 return "Hello, world"나 requests.get("https://api.telegram.org/TOKEN/sendmessage?text=MyMessage&chat_id=98765432")와 같은 코드만 넣어 두자. 우리는 지금 전체적인 텔레그램 챗봇이 lambda에서 돌아가는 구조를 이해하는게 목적이니 작성한 코드에서 에러가 발생하면 괜히 신경써야 할 문제만 늘어난다.
    
**lambda를 이용한다면 웹훅(webhook)을**  
lambda 함수에서 getUpdates를 반복문을 이용해 돌리거나 teleport나 python-telegram-bot과 같은 외부 라이브러리의 start_polling() 함수를 이용해 돌려보면 timeout과 관련된 에러 메시지를 보게 된다. 아마도 lambda에서는 이런 식의(3초 이상 도는) 무한 루프는 예외가 발생하도록 만들어 둔 것 같다. 따라서 웹훅을 이용해야 한다. 웹훅을 이용하면 무한 루프를 돌면서 getUpdates를 감시하지 않아도 메시지가 도착하면 텔레그램이 알려준다. 어떻게? 어떻게 알려주는가? 바로 웹훅이다.
  
아래 주소가 웹훅을 등록/해지하는 주소다.  
https://api.telegram.org/TOKEN/setWebhook  
  
GET으로 호출하면 아래와 같은 메시지를 리턴하면서 웹훅이 해지되어 있음을 알려준다.
~~~
{"ok":true,"result":true,"description":"Webhook is already deleted"}
~~~

POST로 url값에 메시지 도착 시 호출할 url 주소를 넣어주면 웹훅이 설정된다.
  
혹시 아래와 같은 메시지가 나오는 분이 있다면 응답받을, 그러니까 메시지가 도착했을 때 호출 될(웹훅 될) 주소가 https가 아닌 http여서 그렇다. 텔레그램 웹훅은 https에 대해서만 동작한다. 구매한 SSL인증서가 적용된 도메인 주소나 자신이 openssl 등으로 만든 SSL인증서가 적용된 도메인 주소여야 한다. lambda를 사용한다면 AWS API Gateway도 사용할 것이므로 그냥 넘어간다. AWS API Gateway는 좀 더 뒤에 언급한다.
  
~~~  
"ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed"
~~~
  
**lambda를 이용한다면 AWS API Gateway를**  
lambda는 그 자체만으로는 아무것도 할 수 없다. 무언가 lambda에 작성한 코드를 툭 건드려 실행시켜줄 '신호'가 필요하다. 그게 AWS API Gateway다. 사물인터넷 기기로부터 신호를 받는 IoT나, S3등의 변경으로부터 받는 신호 등 다양하게 있지만 우리는 'API 게이트웨이'를 선택한다. (AWS 콘솔 좌상단에 '서비스'를 눌러 API Gateway를 선택 후 미리 만들어 두어도 된다.) API 게이트웨이에는 GET, POST 등의 메서드를 선택해 원하는 lambda 함수와 연결할 수 있게 되어 있다. (lambda 함수를 만든 리전을 선택하고 하단의 lambda 함수 이름의 첫 알파벳을 치면 자동 완성됨.)  
  
**API 배포**  
이렇게 만든 API 게이트웨이의 메서드 목록 위쪽에 '작업' 드롭다운 버튼을 누르면 API 배포가 보인다. API 배포를 하면 이제 실제 외부에서 호출할 수 있는 상태가 된다. 이 주소가 https이니 이 주소를 텔레그램 웹훅 주소르 등록해주면 된다.
  
**관련 라이브러리**  
1. [teleport](https://github.com/nickoala/telepot)
2. [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot)
    - [매뉴얼](http://python-telegram-bot.readthedocs.io/en/stable/index.html)
    - [초간단 예제](https://python-telegram-bot.org/)
  
3. [lambdagram](https://github.com/jwkcp/lambdagram): 서버없는 백엔드 서비스인 AWS lambda에서 웹훅을 이용해 간단히 메시지를 주고 받을 수 있는 라이브러리  
    ~~~
    pip install lambdagram
    ~~~
     
    ~~~
    from lambdagram.bot import Bot


    TOKEN = "THE TOKEN YOU GOT FROM @BotFather"
    
    def lambda_handler(event, context): # Basic function signature on AWS lambda 
        
        bot = Bot(TOKEN)
        bot.send_message(event, "THE MESSAGE YOU WANT TO SEND")
    ~~~