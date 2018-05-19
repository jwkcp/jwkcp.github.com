---
layout: post
title: Django(장고) 배포 가이드(번역) - 1. 배포 체크리스트
tags: django deployment
comments: true
---
  
인터넷은 긴장을 늦출 수 없는 공간입니다. 장고 프로젝트를 인터넷에 공개하려 한다면 그전에 보안, 성능, 운영 등을 고려한 설정을 점검하는 시간을 반드시 가질 필요가 있습니다.  
  
장고는 많은 [보안 옵션](https://docs.djangoproject.com/en/2.0/topics/security/)을 내장하고 있습니다. 어떤 것들은 기본적으로 활성화되어 있고, 또 어떤 것들은 그렇지 않습니다. 모든 환경에 적합하지 않거나 개발할 때 불편하기 때문이죠. 예를 들어, 모든 웹사이트에 HTTPS를 강제로 사용하도록 하는 것은 좋지 않습니다. 그리고 로컬 개발 환경에서 그러는 것은 비현실적이죠.  
   
성능을 최적화하는 것 또한 편의성과 맞바꿈 *Trade-off* 해야 하는 것 중 하나입니다. 얻는 게 있으면 잃는 게 있는 것이죠. 운영 환경에서 캐싱 기능은 유용하지만 로컬 개발 환경에서는 그렇지 않습니다. 에러 리포팅도 같은 맥락입니다.   
  
아래 체크리스트는 이런 사항들을 포함하고 있습니다.  
  
- 최소한의 보안 수준을 맞추기 위해 필요한 적절한 장고 설정  
- 각기 다른 환경을 위한 설정
- 선택적 보안 옵션을 활성화 하는 방법
- 성능 최적화 방법
- 에러 리포팅 제공 방법
  
이런 설정들 중 꽤 많은 사항들이 민감하며 외부에 노출되지 않도록 해야 합니다. 일반적으로 배포를 할 때는 개발에 맞는 설정과 운영 시 개별적으로 사용되어야 하는 설정을 고려하여 그에 맞도록 다뤄져야 합니다.  
  
# Run manage.py check --deploy  
아래 기재된 체크 목록 중 일부는 check --deploy 명령을 사용하면 자동으로 검사가 가능합니다. 운영 설정을 대상으로 실행하는지 꼭 확인하세요!  
  
---
  
## 매우 중요한 설정 *Critical settings*  
  
### SECRET_KEY
비밀키 *secret key* 는 커다란 무작위 값으로 구성되어야 하고 다른 어떤 누구에게도 공개해서는 안됩니다.  
  
운영 환경에 사용되는 비밀키 *secret key* 가 외부에 노출되지 않도록 항상 신경써야 하며 Git이나 SVN과 같은 소스 버전 관리 시스템에 업로드 되지 않도록 하세요. 이런 노력은 외부 공격자가 키를 탈취하는 경우의 수를 줄여줍니다.  
  
비밀키 *secret key* 를 소스코드에 하드코딩하지 말고 아래와 같이 시스템 환경 변수를 이용해 사용하는 방법을 이용하세요.  
  
~~~
import os
SECRET_KEY = os.environ['SECRET_KEY']
~~~
  
또는 이렇게 파일에서 읽어들이는 방법을 쓸 수도 있습니다.  
  
~~~
with open('/etc/secret_key.txt') as f:
    SECRET_KEY = f.read().strip()
~~~
  
---
  
### 디버그 *DEBUG*
운영 환경에서는 절대! 절대! 절대로 DEBUG 설정을 True로 하면 안됩니다. 반드시 기억하세요. 절대 안됩니다.  
  
DEBUG = True 설정은 문제가 생겼을 때 브라우저에서 바로 자세한 스택 추적 정보 *tracebacks* 를 보여주기 때문에 아마 개발할 때 다들 활성화하고 쓰실 텐데요. 운영 환경에서 이렇게 하는 건 진짜 최악입니다. 일부 소스 코드나 로컬 변수, 설정, 사용하는 라이브러리 등의 정보가 불특정 다수에게 유출될 수 있습니다.  
  
---
  
## 환경에 따라 달리하는 설정 *Environment-specific settings*
  
### ALLOWED_HOSTS
DEBUG = False 로 설정한 다음에는 ALLOWED_HOSTS에 접근 가능한 도메인 또는 IP를 넣어줘야 서비스가 가능합니다.  
   
이 설정은 [CSRF(Cross-site request forgery, 사이트 간 요청 위조) 공격](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)을 막아줍니다. 와일드카드( * )를 사용한다면 자체적인 HTTP 헤더 검증 로직이나 이런 종류의 공격에 취약하지 않도록 조치하는 걸 잊으면 안됩니다.  
  
또 웹 서버 설정에서 유효한 호스트를 설정하는 것도 중요합니다. 허용하지 않은 이상한 요청은 장고 *Django* 로 보내지 말고 정적 에러 페이지를 보여주거나 아예 무시하도록 처리하는게 좋습니다. 이런 조치는 쓸데없는 에러가 발생하는 성가심으로 부터 (이메일 알림을 설정했다면 더욱 더)여러분을 해방시켜 줄 것입니다. 예를 들어, nginx(웹 서버의 종류 중 하나)에서 유효하지 않은 호스트에게 '444 응답 없음'을 리턴하도록 설정할 수 있습니다.  
  
~~~
server {
    listen 80 default_server;
    return 444;
}
~~~
  
---
  
### 캐시 *CACHES*
캐시 기능을 사용한다면 연결 문자열 *connection parameters* 는 개발과 운영 환경 각각 다를 것입니다. 장고는 기본적으로 (운영 환경에서)그다지 바람직하지 않은 프로세스 별 로컬 메모리 캐싱을 수행합니다.  
  
캐시 서버는 대체로 인증에 취약한 편입니다. 캐시 서버가 오직 여러분의 어플리케이션 서버에서만 접속할 수 있도록 설정되어 있는지 다시 한 번 확인하세요.  
  
만약 맴캐시 *Memcached* 를 사용하고 있다면 성능 향상을 위해 [cached sessions](https://docs.djangoproject.com/en/2.0/topics/http/sessions/#cached-sessions-backend)를 사용하는 것을 고려해보세요.  
  
---
  
### 데이터베이스 *DATABASES*  
데이터베이스 연결 파라메터는 개발과 운영 환경이 다를 것입니다.  
  
데이터베이스 비밀번호는 매우 주의깊게 다뤄야 합니다. 앞서 설명한 SECRET_KEY를 다루는 방법(환경변수 또는 파일에서 읽어들임)과 같은 방법으로 데이터베이스 비밀번호를 보호할 수 있습니다.  
  
높은 보안 수준을 위해 데이터베이스 서버는 어플리케이션 서버에서만 접근할 수 있도록 해야합니다.  
  
만약 백업 설정을 하지 않았다면 하세요. 지금 바로 당장 하세요! ^^  
  
---   
  
### EMAIL_BACKEND와 관련된 설정
만약 이메일을 보내는 기능이 있다면 이 값을 정확히 설정해야 합니다.  
  
장고 *Django* 는 기본적으로 webmaster@localhost와 root@localhost를 발신자로 사용합니다. 이렇게 사용할 경우 몇몇 이메일 서비스들은 이 주소로 발송된 이메일을 차단합니다. 이 주소를 바꾸고 싶다면 DEFAULT_FROM_EMAIL과 SERVER_EMAIL 값을 바꾸면 됩니다.  
  
---
  
### STATIC_ROOT와 STATIC_URL
개발 서버는 정적 파일을 자동으로 서비스하는 기능이 있습니다. 그러나 운영 환경에서는 그렇지 않습니다. collectstatic 명령으로 정적 파일이 복사될 수 있는 폴더 이름을 STATIC_ROOT값에 설정해야 서비스가 올바르게 동작합니다.  
  
---
  
### MEDIA_ROOT와 MEDIA_URL
미디어 파일은 사용자가 업로드한 파일입니다. 어떤 파일을 올릴지 알 수 없기 때문에 조심해야 합니다. 여러분의 웹 서버가 이 파일들을 가지고 뭔가 해석하거나 시스템에 영향을 주는 처리를 하지 않도록 하는 것은 매우 중요합니다. 예를 들어, 어떤 사용자가 .php 파일을 업로드할 경우 이를 절대 실행하도록 두어서는 안됩니다.  
  
그리고 이런 파일들을 어떻게 백업하면 좋을지 생각해보세요.  
  
---
  
## HTTPS
사용자가 로그인하는 기능이 있는 웹사이트는 비밀번호와 같은 엑세스 토큰이 탈취되지 않도록 사이트 전체에 걸쳐 HTTPS 를 적용해야 합니다. 장고에서는 로그인/비밀번호, 엑세스 쿠키, 비밀번호 재설정 토큰과 같은 것이 이에 해당합니다. (이메일로 비밀번호 재설정 토큰을 발송하는 것을 보호해줄 순 없다는 것은 알고 계시죠?)  
  
사용자 계정이나 관리자와 같이 보안에 민감한 부분에만 HTTPS를 적용하는 것은 충분치 않습니다. 동일한 세션 쿠키가 HTTP나 HTTPS에서 사용되고 있기 때문이죠. 웹 서버는 HTTP 요청을 HTTPS로 리다이렉트해야 합니다. 그리고 장고 서버는 HTTPS만 받아서 처리하도록 해야 합니다.  
  
HTTPS를 설정할 땐 아래 설정을 활성화해줘야 합니다.  
  
### CSRF_COOKIE_SECURE
실수나 사고로 CSRF 쿠키가 HTTP를 통해 전송되지 않도록 하려면 이 값을 True로 설정하세요.  
  
---
  
### SESSION_COOKIE_SECURE
실수나 사고로 세션 쿠키가 HTTP를 통해 전송되지 않도록 하려면 이 값을 True로 설정하세요.  
  
---
  
## 성능 최적화
DEBUG값을 False로 설정하는 것만으로도 개발 환경에서만 유용한 기능을 비활성화하여 성능에 도움을 줄 수 있습니다. 추가로 아래 항목들을 설정하시면 성능 개선에 도움이 됩니다.  
  
### CONN_MAX_AGE
[계속 데이터베이스에 연결 *persistent database connections*](https://docs.djangoproject.com/en/2.0/ref/databases/#persistent-database-connections) 옵션을 활성화하면 데이터베이스 요청 처리 과정에서 상당한 시간을 차지하는 연결 과정 속도를 개선해줍니다.  
  
이 옵션은 제한된 네트워크 성능을 가진 가상 호스팅에서 큰 위력을 발휘합니다.  
  
---
  
### TEMPLATES
캐시된 템플릿 로더 옵션을 활성화하면 매 요청마다 각 템플릿을 컴파일하여 렌더링하지 않아도 되므로 성능을 극적으로 향상시키는 경우가 많습니다. 템플릿 로더와 관련된 자세한 문서는 [여기](https://docs.djangoproject.com/en/2.0/ref/templates/api/#template-loaders)를 참고하세요.  
  
---
  
## 에러 리포팅
운영 환경에 최종 코드를 배포할 때 까지만 해도 모든 게 완벽하다고 생각하겠지만 예측할 수 없는 에러까지 어떻게 할 수는 없지요. 고맙게도 장고(Django)는 이런 에러는 잡아내고 적절히 알려주는 기능을 갖추고 있습니다.  
  
### 로깅
운영 환경에 코드를 배포하기 전에 로그 기록 설정이 적절하게 되어 있는지, 생각한 대로 잘 동작하는지 점검해야 합니다.  
  
[여기](https://docs.djangoproject.com/en/2.0/topics/logging/)에서 관련된 자세한 정보를 찾아볼 수 있습니다.  
  
---
  
### 어드민과 매니저 *ADMINS and MANAGERS*
[ADMINS](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-ADMINS)는 500에러를 메일로 알려줍니다.  
[MANAGERS](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-MANAGERS)는 404에러를 알려줍니다. 겉으로 보기엔 그럴싸하지만 사실은 쓸데없는 에러 리포트를 걸러 내려면 [IGNORABLE_404_URLS](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-IGNORABLE_404_URLS)를 이용하면 좋습니다.  
  
이메일로 에러 리포트를 보내는 방법은 [여기](https://docs.djangoproject.com/en/2.0/howto/error-reporting/)에서 자세한 정보를 찾아볼 수 있습니다.  
  
> 대용량 에러 리포트를 다룬다면 이메일은 좋은 방법이 아닙니다.  
> 이메일로 에러 리포트를 받도록 설정해두면 받은 편지함이 순식간에 가득 차버리는 일이 생길 수도 있습니다. [센트리 *Sentry*](https://docs.sentry.io/)같은 에러 모니터링 시스템을 사용하는 것을 고려해보세요. 센트리 *Sentry* 는 수집된 에러를 집계하는 유용한 기능도 있답니다.  
  
---
  
### 기본 에러 페이지 커스터마이징하기
장고(Django)는 여러 HTTP 코드에 대한 기본적인 에러 페이지와 템플릿을 이미 포함하고 있습니다. 404.html, 500.html, 403.html 그리고 400.html 템플릿을 루트 템플릿 폴더에 생성하는 방법을 통해 기본 에러 템플릿을 오버라이드 *override* 할 수 있습니다. 대부분 장고(Django)에서 기본 제공하는 에러 페이지 정도면 충분하지만 한편으론 멋진 에러 페이지를 만들고 싶은 마음도 있을텐데요. 아래 링크를 누르면 각 페이지 별 자세한 정보를 포함한 구현 방법을 알 수 있습니다.  
  
- [404(페이지 찾을 수 없음) 페이지](https://docs.djangoproject.com/en/2.0/ref/views/#http-not-found-view)
- [500(서버 에러) 페이지](https://docs.djangoproject.com/en/2.0/ref/views/#http-internal-server-error-view)
- [403(권한 없음) 페이지](https://docs.djangoproject.com/en/2.0/ref/views/#http-forbidden-view)
- [400(잘못된 요청) 페이지](https://docs.djangoproject.com/en/2.0/ref/views/#http-bad-request-view)
   
