---
layout: post
title: Django(장고) 배포 가이드(번역) - 4. 아파치 서버와 mod_wsgi 사용법
tags: django deployment
comments: true
---
  
> 이 글은 장고(Django) 공식 페이지의 글을 번역한 것입니다. 원문은 [여기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/)를 참고하세요.  
  
# 아파치 서버와 mod_wsgi 사용법
    
[아파치 서버](https://httpd.apache.org/)와 [mod_wsgi](http://www.modwsgi.org/)를 이용하여 장고를 배포하는 방법은 오랜 시간동안 검증된 좋은 선택입니다.  
  
mod_wsgi는 장고를 포함한 어떤 파이썬 [WSGI](http://www.wsgi.org/) 어플리케이션도 호스팅할 수 있는 아파치 모듈입니다. 장고는 mod_wsgi를 지원하는 어떤 버전의 아파치 서버와도 훌륭하게 잘 동작합니다.  
  
[mod_wsgi 공식 문서](https://modwsgi.readthedocs.io/)에는 여러분들이 만든 서비스에 mod_wsgi를 사용하는 자세한 방법이 기재되어 있습니다. 처음이시라면 [mod_wsgi 설치 및 설정 방법](https://modwsgi.readthedocs.io/en/develop/installation.html)부터 살펴보면 좋습니다.  
  
## 기본 설정
mod_wsgi를 설치하고 활성화했다면 가장 먼저 할 일은 아파치 서버의 [httpd.conf](https://wiki.apache.org/httpd/DistrosDefaultLayout) 파일을 수정하는 것입니다. 만약 2.4보다 오래된 버전의 아파치 서버를 사용한다면 **Require all granted** 를 **Allow from all** 로 바꾼 후 **Order deny,allow** 라인을 위에 추가하세요.  
   
~~~
WSGIScriptAlias / /path/to/mysite.com/mysite/wsgi.py
WSGIPythonHome /path/to/venv
WSGIPythonPath /path/to/mysite.com

<Directory /path/to/mysite.com/mysite>
<Files wsgi.py>
Require all granted
</Files>
</Directory>
~~~
  
**WSGIScriptAlias** 의 첫 번째 부분은 서비스하려 어플리케이션의 기본 URL이고(**/** 는 루트 url을 나타냄), 두 번째 부분은 보통은 프로젝트 패키지(여기에서는 **mysite**) 내부에 있는 "WSGI 파일"의 위치를 나타냅니다. 이렇게 해두면 아파치 서버가 WSGI 어플리케이션 사용한다고 정의된 URL을 포함하여 하위 URL에 대한 모든 요청에 응답하게 됩니다.  
  
만약 여러분의 프로젝트에 필요한 라이브러리를 [virtualenv](https://virtualenv.pypa.io/)에 설치해서 쓰고 있다면 **WSGIPythonHome** 값에 virtualenv 경로를 추가해주세요. 자세한 정보는 [mod_wsgi virtualenv guide](https://modwsgi.readthedocs.io/en/develop/user-guides/virtual-environments.html)에서 찾아볼 수 있습니다.  
  
**WSGIPythonPath** 부분은 파이썬 경로에서 여러분의 프로젝트 패키지가 사용 가능하도록 만들어 줍니다. 다시 말해 **import mysite** 이렇게 쓸 수 있게 해준다는 거지요.  
  
**<Directory>** 부분은 아파치가 wsgi.py 파일에 접근할 수 있도록 설정하는 부분인데요. 다음 줄에서 WSGI 어플리케이션 객체가 있는 **wsgi.py** 파일이 있는지 확인하는 절차를 거칩니다. 장고(Django) 1.4 버전부터 **[startproject](https://docs.djangoproject.com/en/2.0/ref/django-admin/#django-admin-startproject)** 명령을 사용하면 이 부분이 생기게 되지만 그렇지 않다면 수동으로 만들어 주시면 됩니다. [WSGI 살펴보기](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/)에서 이 파일에 넣어야 하는 필수 또는 기타 항목에 대한 자세한 정보를 얻을 수 있습니다.  

> 경고  
>
> 하나의 mod_wsgi 프로세스에서 여러 장고 사이트를 운영하면 모든 사이트에서 가장 먼저 실행되는 설정 파일을 사용하도록 자동 자정 됩니다. 이 문제는 아래와 같이 wsgi.py 파일 변경을 통해 해결할 수 있습니다.

이 파일을:    
~~~
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "{{ project_name }}.settings")  
~~~
    
이렇게 바꿉니다:
~~~
os.environ["DJANGO_SETTINGS_MODULE"] = "{{ project_name }}.settings"
~~~
   
또는 [mod_wsgi 데몬 모드](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/modwsgi/#daemon-mode)를 사용하여 각각의 사이트가 독립된 데몬 프로세스 상에서 실행될 수 있도록 할 수 있습니다.  
  
> 파일 업로드 시 **UnicodeEncodeError** 문제 수정하기
>
> 아스키(ASCII) 문자가 아닌 문자가 포함된 파일명을 가진 파일을 업로드할 때 UnicodeEncodeError가 발생한다면 아파치가 아스키(ASCII) 문자 외의 문자도 이름으로 사용할 수 있도록 설정되어 있는지 확인하세요:  
  
~~~
export LANG='en_US.UTF-8'
export LC_ALL='en_US.UTF-8'
~~~    

이 설정은 보통 **/etc/apache2/envvars** 에 위치합니다. 더 자세한 정보는 [유니코드 참고 가이드에 포함된 파일(Files) 섹션](https://docs.djangoproject.com/en/2.0/ref/unicode/#unicode-files)을 읽어보세요.  
  
---
  
## mod_swgi 데몬 모드로 사용하기
"데몬 모드"는 (비 윈도우 계열 플랫폼에서)mod_wsgi를 운영할 때 권장되는 모드입니다. 필요한 데몬 프로세스 그룹을 생성하고 장고(Django) 인스턴스에 이를 위임하여 실행하려면 **WSGIDaemonProcess** 과 **WSGIProcessGroup** 항목에 적절한 값을 넣어줘야 합니다. 또, **WSGIPythonPath** 를 사용할 수 없고 **WSGIDaemonProcess** 에 **python-path** 옵션을 이용하는 것으로 변경해야 합니다.  
  
예)  
~~~
WSGIDaemonProcess example.com python-home=/path/to/venv python-path=/path/to/mysite.com
WSGIProcessGroup example.com
~~~
  
만약에 서브 디렉토리(이 예제를 예로 들면 **https://example.com/mysite**)에서 서비스를 운영하고 싶다면 **WSGIScriptAlias** 항목에 별도의 값을 추가해야 합니다.  
  
~~~
WSGIScriptAlias /mysite /path/to/mysite.com/mysite/wsgi.py process-group=example.com
~~~
  
더 자세한 정보는 [데몬 모드 설정하기](https://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html#delegation-to-daemon-process)라는 문서를 참고하세요.  
   
---  
  
## 파일 서빙하기
장고(Django)는 직접 파일을 서빙하지 않습니다. 대신 여러분이 선택한 웹서버에게 그 일을 맡기는 방식을 취합니다.  
  
미디어 파일을 서빙할 때도 마찬가지입니다. 그럼 어떻게 하냐구요? 아래에 방법을 권장합니다.  
  
- [Nginx](https://nginx.org/en/)
- [Stripped-down 버전의 아파치](https://httpd.apache.org/)
  
그러나 어쩔 수 없이 장고(Django)와 함께 동일한 아파치 **가상 호스트(VirtualHost)** 에서 미디어 피일을 서빙해야 한다면 아파치 서버를 그에 맞게 설정한 다음, 장고와 통신 가능한 mod_wsgi 인터페이스를 사용할 수도 있습니다.  
  
아래는 장고(Django)를 사이트 루트로 사용하여 **robots.txt**, **favicon.ico**, **/static/** 폴더와 **/media/** 폴더의 정적 파일을 서빙하도록 하고, 그 이외에 기타 URL은 mod_wsgi에서 서빙하도록 하는 예제입니다.  
  
~~~
Alias /robots.txt /path/to/mysite.com/static/robots.txt
Alias /favicon.ico /path/to/mysite.com/static/favicon.ico

Alias /media/ /path/to/mysite.com/media/
Alias /static/ /path/to/mysite.com/static/

<Directory /path/to/mysite.com/static>
Require all granted
</Directory>

<Directory /path/to/mysite.com/media>
Require all granted
</Directory>

WSGIScriptAlias / /path/to/mysite.com/mysite/wsgi.py

<Directory /path/to/mysite.com/mysite>
<Files wsgi.py>
Require all granted
</Files>
</Directory>
~~~

만약 2.4보다 오래된 버전의 아파치 서버를 사용한다면 **Require all granted** 를 **Allow from all** 로 바꾼 후 **Order deny,allow** 라인을 위에 추가하세요.  
  
---
  
## 어드민(admin) 파일 서빙하기
장고(Django) 설정의 [INSTALLED_APPS](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-INSTALLED_APPS)에  [django.contrib.staticfiles](https://docs.djangoproject.com/en/2.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles)이 있으면 장고의 개발 서버는 자동으로 어드민 앱(그리고 다른 설치된 앱)의 정적 파일을 자동으로 서빙하도록 동작합니다. 그러나 운영 등에서 쓰이는 다른 서버들을 사용할 때는 그렇지 않죠. 아파치 또는 직접 사용하기로 정한 웹 서버 설정을 직접 만져서 어드민 파일들을 서빙하도록 수정해 줄 필요가 있습니다.  
  
장고 배포판에서는 **django/contrib/admin/static/admin** 에 어드민 파일이 있습니다.    
  
특별한 경우가 아니라면  [django.contrib.staticfiles](https://docs.djangoproject.com/en/2.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles)를 통해 어드민 파일들을 다루도록 하는 방법을 강력히 권장합니다. (이전 섹션에서 언급한 웹 서버가  [collectstatic](https://docs.djangoproject.com/en/2.0/ref/contrib/staticfiles/#django-admin-collectstatic) 명령으로 [STATIC_ROOT](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_ROOT)로 부터 정적 파일을 수집하여 한 곳에 모은 후    [STATIC_URL](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_URL)이 가리키는 [STATIC_ROOT](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_ROOT)를 서빙하도록 할 수 있습니다.) 그 외 아래 3가지 방법 중 하나를 선택해 사용할 수도 있습니다.   
  
1. 문서(document) 루트 안에서 어드민 정적 파일의 심볼릭 링크 생성하기 (이 방법을 사용하려면 아파치 설정에서 **+FollowSymLinks** 이 필요할 수 있습니다.)
2. 위에서 본 것과 같이 **Alias** 지시어를 사용하여 실제 위치의 어드민 파일에 적절한 URL에 별칭 부여 (대부분 **[STATIC_URL](https://docs.djangoproject.com/en/2.0/ref/settings/#std:setting-STATIC_URL) + admin/**)
3. 아파치 문서(document) 루트에 어드민 정적 파일 복사해 넣기

---
  
## 아파치에서 장고(Django) 사용자 데이터베이스 인증하기
장고(Django)는 아파치가 장고의 인증 백엔드 *(인증을 위해 장고는 여러 방법을 제공하고 있는데 그것을 제공하는 방법의 명칭을 백엔드(backend)라고 함)* 를 이용해 직접 사용자 인증을 처리할 수 있도록 핸들러를 제공하고 있습니다.[mod_wsgi 인증 문서](https://docs.djangoproject.com/en/2.0/howto/deployment/wsgi/apache-auth/)를 살펴보세요.
   
