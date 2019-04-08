---
layout: post
title: 로컬 장고(django) 앱을 가상사설서버(VPS)에 배포하는 법
tags: django vps deploy how-to
comments: true
---
`이 문서는 VPS 서비스를 제공하는 디지털오션(DigitalOcean)의 문서를 번역한 것입니다.
 오역이 있을 수도 있으므로 위의 원문 링크를 꼭 참고하세요.`
 
 원문 링크: [디지털오션(DigitalOcean)의 문서 바로가기](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-local-django-app-to-a-vps "디지털오션(DigitalOcean)의 문서")
 
 ---
 
## 전제조건
 
이 튜토리얼은 선택한 (이 문서에서는 데비안 7을 사용하고 있으나 우분투도 문제 없습니다.) 운영체제에 가상사설서버(이하 VPS)가 이미 설치되어 있다고 가정합니다. 만약 그렇지 않다면 이 [튜토리얼](https://www.digitalocean.com/community/articles/how-to-create-your-first-digitalocean-droplet-virtual-server "튜토리얼")을 참고하세요. 시작하기 전에 장고 애플리케이션을 올릴 수 있도록 적절히 설정된 클라우드 서버와 함께 사용될 데이터베이스 서버, 웹서버, 그리고 가상환경(virtualenv)이 이미 설치되어 있는지 확인하세요. 그렇지 않다면 [장고 서버 셋팅하기](https://www.digitalocean.com/community/articles/how-to-install-and-configure-django-with-postgres-nginx-and-gunicorn "장고 서버 셋팅하기")에 대한 1 – 6단계를 따라하십시오.
 
---
  
## 첫 번째: 패키지 업데이트 하기
 
뭔가를 하기 전에, 어떤 패키지 관리자를 사용하든 항상 패키지를 최신 상태로 유지하는 것은 좋은 습관입니다. SSH를 통해 VPS에 접속하여 아래의 명령을 실행할 수 있습니다.
 
> sudo apt-get update
> sudo apt-get upgrade
 
첫 번째 명령은 apt-get로 관리되는 패키지의 업데이트를 다운로드 합니다. 두 번째 명령은 다운로드한 업데이트를 설치합니다. 위 명령을 실행하면 설치할 업데이트가 있을 경우 설치 할지 말지에 대한 표시가 나타납니다. 이 표시가 나타나면 “y”를 누르고 “엔터”키를 입력하세요.

---

## 두 번째: 가상환경(Virtualenv) 구성하기

앞서 얘기한 전제조건을 이미 만족하도록 구성해두었다면 이 단계는 건너띄어도 됩니다.

우리는 프로젝트 파일과 파이썬 패키지가 위치할 곳에 가상환경의 구성이 필요합니다. 가상환경을 사용하지 않을 거라면, 간단히 장고 프로젝트가 위치할 곳에 디렉토리를 만들고 세 번째 단계로 이동하세요. 

가상환경을 만드는 명령어는 아래와 같습니다. 경로는 가상사설서버 내 원하는 위치로 변경하는 것을 잊지마세요:
> virtualenv /opt/myproject

이제 가상환경을 구성했으니, 이제 가상환경을 활성화하고 장고와 필요한 다른 파이썬 패키지를 pip를 통해 설치합니다. 아래의 예제는 어떻게 가상환경을 활성화하고 pip를 이용해 장고를 설치하는 예제입니다.
> source /opt/myproject/bin/activate   
> pip install django

자, 이제 이 프로젝트의 데이터베이스를 생성할 준비가 됐습니다.

---

## 세 번째: 데이터베이스 생성하기

이 튜토리얼 문서는 PostgreSQL 데이터베이스를 사용하는 것으로 가정합니다. 그렇지 않다면 원하는 데이터베이스 생성을 어떻게 하는지에 대한 문서를 확인하세요.

PostgreSQL 데이터베이스를 생성하기 위해선 아래 명령을 실행합니다.
> sudo su - postgres

이제 터미널 프롬프트에 “postgres@yourserver”라고 나타날 것입니다. 그렇다면, 원하는 이름의 데이터베이스를 아래 명령을 실행해 생성합니다.
> createdb mydb

아래의 명령으로 데이터베이스 사용자를 생성합니다.
> createuser -P

이제 6개의 질문이 나타난다. 첫 번째는 새로운 사용자의 이름(어떤 이름을 사용해도 좋다)이다. 두 번째 질문은 새로운 사용자의 비밀번호와 비밀번호 재확인이다. 마지막 3개 질문은 간단하게 “n”을 입력하고 엔터를 누르면 된다. 이 과정은 새로운 사용자가 접속할 수 있는 곳이 어딘지 결정합니다. 아래 명령으로 PostgresSQL 를 활성화합니다.
> psql

마지막으로, 새로운 사용자가 새로운 데이터베이스에 접근할 수 있게 아래의 명령으로 권한을 줍니다.
> GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;

이제 데이터베이스를 구성했고 사용자가 데이터베이스에 접근할 수 있게 했습니다. 다음은 정적 파일을 서비스하기 위한 웹 서버의 설정과 관련된 작업을 해봅니다.

---

## 네 번째: 가상서설서버 설정하기

사이트에서 사용할 새로운 환경 설정 파일을 생성해야 한다. 이 튜토리얼은 당신이 클라우드 서버로 NGINX를 사용한다는 것을 가정합니다. 여기에 해당하지 않는다면 이 단계를 완료하기 위해 선택한 웹서버의 문서를 확인할 필요가 있습니다.

NGINX에서는, 사이트 웹서버 설정 파일을 생성하고 수정하기 위해 아래 명령을 실행하고, “myproject”로 되어 있는 명령의 마지막 부분을 자신의 프로젝트 명으로 바꿨는지 확인합니다.
> sudo nano /etc/nginx/sites-available/myproject

이제 에디터를 열어 아래 코드를 입력합니다.

```
server {
    server_name yourdomainorip.com;

    access_log off;

    location /static/ {
        alias /opt/myenv/static/;
    }

    location / {
        proxy_pass http://127.0.0.1:8001;
        proxy_set_header X-Forwarded-Host $server_name;
        proxy_set_header X-Real-IP $remote_addr;
        add_header P3P 'CP="ALL DSP COR PSAa PSDa OUR NOR ONL UNI COM NAV"';
    }
}
```  

저장하고 빠져나갑니다.    
위 설정은 장고 프로젝트에서 구성한 정적 파일 디렉토리인 yourdomainorip.com/static/으로 요청되는 모든 요청에 응답하도록 되어 있습니다. yourdomainorip.com의 8001포트로 들어오는 모든 요청은 Gunicorn(혹은, 선택한)이 대신 처리합니다. 다른 줄은 Gunicorn이 통과시킬 호스트명과 IP주소에 대한 것입니다. 이렇게 하지 않으면 모든 요청의 IP주소는 127.0.0.1과 VPS 호스트명으로 설정됩니다.

자, 이제 이 설정 파일을 가리킬 /etc/nginx/sites-enabled 디렉토리의 심볼릭 링크를 구성해야 합니다. 이것을 통해 NGINX가 활성 사이트를 알아챕니다. /etc/nginx/sites-enabled로 디렉토리를 바꾸는 방법은 아래와 같습니다.
> cd /etc/nginx/sites-enabled

디렉토리를 바꿨으면 아래 명령을 실행합니다.
> sudo ln -s ../sites-available/myproject

이제 NGINX를 재시작 해봅시다.
> sudo service nginx restart

이런 에러 메시지를 보게 될 것입니다.
> server_names_hash, you should increase server_names_hash_bucket_size: 32

‘etc/nginx/nginx.conf’ 파일의 수정을 통해 이 문제를 해결할 수 있습니다. 파일을 열고 아래 문장의 주석을 해제하세요.
> server_names_hash_bucket_size 64;

이제 프로젝트 파일을 서버에 올리는 일만 남았습니다.

---

## 다섯 번째: 로컬 장고 프로젝트 올리기
   
프로젝트를 올릴 때 FTP, SFTP, SCP, Git, SVN, etc 등 여러 가지 옵션이 있습니다. 여기서는 Git을 이용해 VPS로 프로젝트 파일을 옮길 것입니다.

가상환경인 virtualenv가 어디 구성되었는지 혹은 프로젝트를 위치할 디렉토리를 찾습니다. 그리고 아래의 명령을 통해 이 디렉토리로 이동합니다.
> cd /opt/myproject

그 후, 아래 명령으로 프로젝트 파일이 위치한 곳에 새로운 디렉토리를 만듭니다.
> mkdir myproject

두 개의 같은 이름을 가진 디렉토리는 좀 불필요해 보입니다. 그래도 가상환경의 이름과 프로젝트의 이름이 같도록 하기 위해 그렇게 하겠습니다.

이제 아래 명령을 실행해 새로운 디렉토리로 이동하겠습니다.
> cd myproject

여러분의 프로젝트가 이미 Git 저장소에 포함되어 있다면 여러분의 코드가 모두 커밋되고 푸시되었는지 터미널(맥) 또는 커맨드 프롬프트(PC)에서 간단히 확인할 수 있습니다.
> git status

결과에 아무 파일도 볼 수 없다면 잘되고 있는 것입니다. 이제 아래의 명령을 통해 Git을 설치해보겠습니다.
> sudo apt-get install git

질문에 yes라고 대답하려면 “y”를 입력하고 엔터를 누릅니다. 깃이 설치되었다면 아래 명령을 통해 여러분의 프로젝트를 VPS의 프로젝트 디렉토리로 가져(pull) 올 수 있습니다.
> git clone https://webaddressforyourrepo.com/path/to/repo

여러분이 Git 호스팅에 Github나 Bitbucket를 사용하고 있다면 clone 버튼을 이용해 주소를 확인 할 수 있을 것입니다. “.”(마침표)를 빠트리지 않도록 주의합니다. 마침표를 빠트리고 입력한다면 원치하는 프로젝트 디렉토리 내의 이름으로 생성되니 주의하세요.

배포 수단으로 Git을 쓰지 않는다면 FTP나 기타 다른 방법으로 생성한 프로젝트 디렉토리에 프로젝트 파일을 이동할 수 있습니다.

이제 거의 끝나갑니다. 이제 앱 서버(app server)를 구성만 남았으니 힘내세요!

---

## 여섯 번째: 앱 서버 설치하고 설정하기

여러분이 앞서 이야기한 전제조건을 만족하고 있다면 이 단계는 건너 뛰어도 됩니다
 
이제 앱 서버를 설치하고 장고 앱이 8001포트를 이용해 요청에 응답할 수 있도록 하겠습니다. 이 예제에서는 Gunicorn을 이용하겠습니다. Gunicorn을 설치 전 먼저 가상환경(virtualenv)을 활성화하세요.
> source /opt/myproject/bin/activate

가상환경이 활성화되면 아래의 명령을 실행해 Gunicorn을 설치합니다.
> pip install gunicorn

Gunicorn이 설치됐습니다. 이제 여러분의 도메인이나 IP에 8001 포트로 접속합니다
> gunicorn_django --bind yourdomainorip.com:8001

원한다면 “ctrl + z”를 누르고 “bg”를 입력하여 백그라운드로 실행할 수도 있습니다. Gunicorn의 더 많은 설정과 구성에 대한 정보는 [이 튜토리얼](https://www.digitalocean.com/community/articles/how-to-install-and-configure-django-with-postgres-nginx-and-gunicorn "이 튜토리얼")에서 찾을 수 있습니다.

자, 이제 마지막 단계입니다.

---

## 일곱 번째: 앱 설정하기

마지막 과정은 운영 서버에서 여러분의 앱을 설정하는 과정입니다. 우리가 필요한 모든 설정은 장고 프로젝트의 “settings.py”에 있습니다. 아래의 명령으로 파일을 여세요.
> sudo nano /opt/myproject/myproject/settings.py

settings 파일의 경로는 여러분이 프로젝트를 어떻게 구성했는지에 따라 다르니 상황에 맞춰 적절히 수정합니다.

settings 파일이 열리면, DEBUG 항목을 False로 설정합니다.
> DEBUG = False

이렇게 설정하면 에러가 발생했을 때 사용자들에게 스택 추적을 포함한 디버그 정보를 노출하지 않고 404 혹은 500 에러 페이지를 보여주게 됩니다.

이제 데이터베이스 구성을 수정할 차례입니다. 아래에 보여지는 항목에서 데이터베이스명, 사용자명 그리고 비밀번호 부분을 여러분들의 정보에 맞게 입력하겠습니다.

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.psycopg2', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'NAME': 'mydb',                      # Or path to database file if using sqlite3.
        # The following settings are not used with sqlite3:
        'USER': 'myuser',
        'PASSWORD': 'password',
        'HOST': '',                      # Empty for localhost through domain sockets or '127.0.0.1' for localhost through TCP.
        'PORT': '',                      # Set to empty string for default.
    }
}
```

이제 정적 파일 설정을 수정할 차례입니다.

> STATIC_ROOT = '/opt/myproject/static/'
> STATIC_URL = '/static/'

파일을 저장하고 닫습니다. 이제 정적 파일을 모을 차례입니다. “manage.py” 스크립트가 있는 디렉토리로 이동 후 아래의 명령을 실행합니다.
> python manage.py collectstatic

이 명령은 위에서 구성한 settings.py 파일에 따라 정적 파일을 수집하여 디렉토리에 넣는 역할을 합니다.   
모두 끝났습니다! 이제 여러분의 앱은 운영 서버에 배포되어 사용할 준비가 되었습니다.

---

`이 문서는 VPS 서비스를 제공하는 디지털오션(DigitalOcean)의 문서를 번역한 것입니다. 오역이 있을 수도 있으므로 위의 원문 링크를 꼭 참고하세요.`   

원문 링크: [디지털오션(DigitalOcean)의 문서 바로가기](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-local-django-app-to-a-vps)
      
