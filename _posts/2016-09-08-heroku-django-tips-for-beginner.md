---
layout: post
title: 초보자가 헤로쿠(Heroku)에 장고(Django) 앱 배포 시 알아두면 좋은 정보
tags: heroku django deployment
comments: true
---
[![featured.jpg](https://s26.postimg.org/e2qq378ft/featured.jpg)](https://postimg.org/image/by6d246t1/)

헤로쿠(Heroku)에 앱 배포시 아래와 같은 커맨드를 쓴다.

{% highlight bash shell scripts %}
$ heroku create
Creating lit-bastion-5032 in organization heroku... done, stack is cedar-14
http://lit-bastion-5032.herokuapp.com/ | https://git.heroku.com/lit-bastion-5032.git
Git remote heroku added
{% endhighlight %}

그런데 장고(Django) 앱 배포 시 아래와 같이 정상적으로 진행되지 않고 여러가지 오류가 날 때가 있다.
배포가 거절되었다(Reject Deploying)는 등등…

{% highlight bash shell scripts %}
$ git push heroku master
Counting objects: 232, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (217/217), done.
Writing objects: 100% (232/232), 29.64 KiB | 0 bytes/s, done.
Total 232 (delta 118), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote:
remote: -----> Python app detected
remote: -----> Installing python-2.7.11
remote:      $ pip install -r requirements.txt
remote:        Collecting dj-database-url==0.4.0 (from -r requirements.txt (line 1))
remote:          Downloading dj-database-url-0.4.0.tar.gz
remote:        Collecting Django==1.9.2 (from -r requirements.txt (line 2))
remote:          Downloading Django-1.9.2-py2.py3-none-any.whl (6.6MB)
remote:        Collecting gunicorn==19.4.5 (from -r requirements.txt (line 3))
remote:          Downloading gunicorn-19.4.5-py2.py3-none-any.whl (112kB)
remote:        Collecting psycopg2==2.6.1 (from -r requirements.txt (line 4))
remote:          Downloading psycopg2-2.6.1.tar.gz (371kB)
remote:        Collecting whitenoise==2.0.6 (from -r requirements.txt (line 5))
remote:          Downloading whitenoise-2.0.6-py2.py3-none-any.whl
remote:        Installing collected packages: dj-database-url, Django, gunicorn, psycopg2, whitenoise
remote:          Running setup.py install for dj-database-url: started
remote:            Running setup.py install for dj-database-url: finished with status 'done'
remote:          Running setup.py install for psycopg2: started
remote:            Running setup.py install for psycopg2: finished with status 'done'
remote:        Successfully installed Django-1.9.2 dj-database-url-0.4.0 gunicorn-19.4.5 psycopg2-2.6.1 whitenoise-2.0.6
remote:
remote:      $ python manage.py collectstatic --noinput
remote:        58 static files copied to '/app/gettingstarted/staticfiles', 58 post-processed.
remote:
remote: -----> Discovering process types
remote:        Procfile declares types -> web
remote:
remote: -----> Compressing...
remote:        Done: 39.3M
remote: -----> Launching...
remote:        Released v4
remote:        http://lit-bastion-5032.herokuapp.com/ deployed to Heroku
remote:
remote: Verifying deploy... done.
To git@heroku.com:lit-bastion-5032.git
 * [new branch]      master -> master
{% endhighlight %}

---

그럴 땐 아래 3가지 항목이 정상적으로 잘 설정되어 있는지 확인 해보면 좋다.

**1. Procfile 파일의 생성**   
Procfile을 프로젝트 최상위 폴더에 생성 후 아래와 같이 WSGI에 대한 설정 부분을 입력한다.
web: gunicorn <자기 프로젝트 이름>.wsgi --log-file -  (입력 시 꺽쇠기호는 빼고 입력)

**2. gunicorn 패키지 설치 여부**   
헤로쿠에 앱 배포 시 WSGI를 사용하는데 <1번>에서 gunicorn을 사용하기로 했다면, gunicorn 패키지가 로컬에 설치되어 아래 <2번>requirements.txt를 만들 때 항목에 기록되어야 헤로쿠 앱 배포 시 자동으로 설치가 되며 에러가 발생하지 않는다.

**3. requirements.txt 존재 여부**   
‘pip freeze > requirements.txt’ 명령으로 현재 프로젝트의 패키지 설치 환경이 담긴 파일을 만들 수 있다. 이 파일이 헤로쿠에 배포하고자 하는 프로젝트 최상위 폴더에 위치해야 한다.
초보자가 사소하지만 자주하는 실수는 ‘requirements.txt’ 파일명을 입력할 때 ‘s’ 문자를 빼고 생성하는 것이다.

**4. collectstatic 명령**   
헤로쿠 배포 스크립트가 collectstatic 명령을 실행할 때 에러가 나는 경우는 STATIC_ROOT 항목이 settings.py에 설정되지 않아서이다. 개발버전이 아닌 배포를 할 때는 장고 프로젝트 내에 여러군데 흩어져있는 정적 파일들(html, css 등등)을 한 곳에 모아 주어야 WSGI가 실행될 때 해당 경로를 참고 할 수 있다.
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles') 와 같이 settings.py에 설정하면 된다.

---

필수적이진 않지만 잘 설명되어 있지 않는 부분 중 하나가 실행되는 파이썬 버전을 지정하는 방법이다.
기본적으로 헤로쿠 문서의 Getting Start를 따라 실행하면 로컬 개발 환경에서 파이썬 3를 이용해 개발하고 있더라도 헤로쿠에 배포 시 배포 스크립트는 파이썬 2.x 버전으로 설치한다. 원하는 버전으로 설치하기 위해선 아래와 같이 프로젝트의 최상위 폴더에 runtime.txt 파일을 만들고 실행되길 원하는 파이썬 버전명을 지정하면 해당 버전으로 헤로쿠 앱 배포 시 설치된다.

> python-3.5.2