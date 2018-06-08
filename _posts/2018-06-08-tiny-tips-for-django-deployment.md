---
layout: post
title: 우분투에서 장고(Django) 배포 경험담
tags: ubuntu django
comments: true
---

# 장고(Django), 배포 그리고 환경변수
파이썬과 장고를 이용해 만든 프로젝트를 깃허브(github)에 올리고 실서버에 배포를 하는 경우를 생각해보자. 이 과정에서 SECRET_KEY 라든지, 각종 API키와 DB접속정보 등을 소스코드와 분리해야 한다는 조언을 많이 보았을 것이다. 무심코 SECRET_KEY와 DB 접속정보를 settings.py에 그대로 담은 채 깃허브에 푸시(Push)하면 내가 만든 서비스에서 장고가 제공하는 보안 기능이 무력화되고 DB 서버가 외부 위험에 그대로 노출된다. VPS업체에서 제공하는 인바운드 방화벽 규칙이나 리눅스의 ufw도 있긴 하지만 어쨌든 이건 절대적으로 피해야 하는 실수다.  
  
이런 환경변수를 소스 코드와 분리하여 별도의 공간에서 불러다 쓰기 위해서는 별도의 파일에 저장하여 .gitignore에 기록 후 버전 관리 대상에서 제외하거나, 실서버의 환경변수에 이 값을 설정하는 방법이 많이 쓰인다. 이 중에서도 이 글에서 이야기할 팁은 실서버의 환경변수에 이 키-값들을 설정하고 불러다 쓰는 과정에서 겪을 수 있는 문제에 대한 이야기다.   
  
아주 아주 옛날에 윈도우 98을 설치하는 과정에서 속담처럼 하던 얘기가 98번은 설치해봐야 제대로 설치할 줄 알게 된다고 해서 윈도우 98이라는 풍문이 있었다. 처음 장고 서비스를 배포해보면서 이때의 기억을 떠올리며 실서버에 장고를 올리는 작업을 적어도 16번은 해보겠다고 생각했다. 요즘 우분투는 16.04를 많이 쓰니까.
  
---
  
# 산 넘어 산  
## runserver는 안된다고?
로컬 머신에서 개발할 때는 python manage.py runserver 0.0.0.0:8000과 같이 내부 서버를 띄워 테스트를 했었다. 그런데 이 서버는 말 그대로 개발용으로 개발된 것이라 실제 운영 환경에 적용하기엔 무리가 있다. 그래서 uwsgi나 gunicorn같은 어플리케이션이 중간에서 장고를 돌리고 앞단에는 부하와 정적 파일 서빙을 담당하는 apache나 nginx 같은 전용 웹서버가 놓인다. gunicorn과 nginx를 쓴다고 했을 때 여기까지 순서를 보면 -  
  
> nginx - gunicorn - django

대충 이렇게 표시할 수 있다. 이렇게 구성해서 서비스를 띄우면 될 텐데 중간에 몇 가지 더 추가되는 것이 있다.  
  
- 파이썬과 관련 패키지를 독립적으로 운영하기 위한 **virtualenv**
- 서비스가 죽거나 서버가 재시작되어도 프로세스를 계속 살려내고 시작하며 관리해주는 **supervisor**
- 파이썬 패키지 관리자 **pip**
- 웹서버 **nginx**
- 데이터베이스 **postgresql**
- 로케일 **locale**
- 등등 ...
   
## virtualenv
**virtualenv**없이 일단 기본만 해보자 싶었는데, virtualenv없이 그냥 장고 서비스를 돌려보니 로컬 머신에 패키지와 깔았다 지웠다 하면서 패키지 경로와 버전이 얽히고 도대체 지금 실행되고 있는 패키지가 어디에 것인지 which 를 써가며 계속 확인해보고 패키지 경로 문제로 알 수 없는 에러와 이를 해결하기 위해 며칠을 소진하고 나니 아무 쓸대없는 것 같았던 virtualenv가 소중하게 느껴졌다. 설치도 간단하고 배우는 것도 간단하고 이제는 virtualenv 없이는 못 살 것 같다는 생각이 들 때 좀 익숙해졌구나 하는 생각도 들었다. 그때 쯤 virtualenv를 설치하는 방법이 apt-get도 있고 pip도 있다는 걸 알게되고 pip는 python2에 맞는 버전과 python3에 맞는 pip3 가 있다는 사실도 알게 되었다. 가상환경을 python3로 생성하면 python3용 pip3는 더 이상 pip3로 호출하지 않고 pip로 호출하면 되었고, python3로 생성하지 않은 상태에서 pip3를 pip3 install --upgrade pip3를 했다가 pip와 뭔가 잘못되어 패키지 설치 경로가 엉망이 되는 경험도 있었다. 그러던 도중 python3에는 venv라는 명령으로 별도의 패키지 설치 없이 가상환경을 구성할 수 있는 기능이 추가되었다는 것을 알게 되었다.    
   
*python -m venv myvenv*  
  
---

## supervisor
**supervisor**는 init, upstart와 같이 부팅 시 프로세스를 실행하고 죽으면 다시 살려주는 프로세스 관리자다. 과거에는 이 프로세스 관리자로 supervisor를 많이 사용했는데 우분투가 init에서 upstart를 거쳐 데비안 계열의 공식 프로세스 관리자인 **systemd**를 채택하면서 supervisor대신 systemd를 사용하기로 했다. [10분만에 익히는 supervisor 설치와 사용법](https://jwkcp.github.io/2016/11/07/how-to-use-supervisor-in-one-minute/)을 통해 supervisor에 대해 간단히 살펴볼 수 있다. systemd를 사용하려면 /etc/systemd/system/ 경로 아래 내서비스명.service 파일이 있어야 한다. [여기](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)에서 DigitalOcean에서 풀어서 설명하는 systemd의 설정 파일에 대한 설명을 볼 수 있고, [여기](https://jwkcp.github.io/2018/06/04/systemd-simple-usage/)를 누르면 systemd에서 사용하는 명령어와 각 항목들에 대한 간략한 설명을 볼 수 있다. 아래는 systemd에 대한 위키백과의 설명이다.  
  
> systemd는 일부 리눅스 배포판에서 유닉스 시스템 V나 BSD init 시스템 대신 사용자 공간을 부트스트래핑하고 최종적으로 모든 프로세스들을 관리하는 init 시스템이다. systemd라는 이름 뒤에 추가된 d는 유닉스에서의 데몬을 나타낸다.
  
[![2018-06-08_2.12.17.png](https://s26.postimg.cc/ww1z904ah/2018-06-08_2.12.17.png)](https://postimg.cc/image/vh0eka379/)
  
**참고: 환경변수가 적용되는 시점을 잘 생각하자.**  
systemd를 사용해 gunicorn을 구동하고 gunicorn 스크립트를 통해 virtualenv를 활성화함과 동시에 환경변수를 설정하도록 했다면 systemd가 환경변수를 참조하도록 하면 안된다. 그러면 빈 값을 참조할 것이고 systemd의 status 명령 결과는 늘 failed인 상태가 될 것이다. .bashrc나 .zshrc, .bash_profile 등에 환경변수를 넣었다면 해당 명령이 실행되는 시점, 예를 들어 사용자 로그인 시점이라든지, 쉘 실행 시점 등을 주의할 필요가 있다. 지금은 당연한 내용이라고 생각할 수 있지만 나중에 골탕먹기 쉽다.  
   
---
  
## pip
**pip**는 파이썬 패키지 관리자다. python3가 나오면서 pip도 pip3가 나왔다. 그래서 python3를 설치했다면 pip3를 써야하는데 *python3 -m venv myvemv* 와 같이 python3로 가상환경을 만들면 pip3가 아닌 pip로 쓰면 된다. 이 과정에서 [이런](https://jwkcp.github.io/2018/05/26/pip3-error-on-ubuntu/) 문제를 겪기도 했다. 초기에 관련 의존성 패키지를 설치를 할 때는 아래와 같은 것들을 설치했다.  
  
~~~
sudo apt-get -y install build-essential libpq-dev python3-dev postgresql postgresql-contrib, nginx
~~~
  
## nginx
웹서버. apache와 함께 가장 많이 쓰이는 웹서버다. gunicorn 서비스를 127.0.0.1:8000과 같이 바인딩하면 nginx가 sites-enabled의 설정 파일을 통해 관련 외부 요청을 연결시켜준다. 외부 사용자 요청을 nginx가 받아서 systemd로 띄운 gunicorn을 django를 통해 처리 후 되돌려 주는 것이다.   
 
## postgresql
mysql가 함께 많이 사용되는 데이터베이스. 레퍼런스와 트러블 슈팅 자료 등은 확실히 mysql이 많고, 특별한 이유가 아니면 굳이 mysql을 거부할 이유도 없지만 오라클에 인수 후 뭔가 지속가능성에 의구심이 들어 postgresql을 사용해봐야겠다는 생각을 해서 시도했다. 쿼리문 등 기본적으로 사용하는 것은 비슷하지만 다른 부분은 또 전혀 달라서 관련 명령어를 찾아봐야했다. 예를 들어 데이터베이스 목록을 보는 명령이 mysql은 *show databases;* 이지만 postgresql은 *\l;*이다. 처음 사용하면서 간단히 관련 명령을 [여기](https://jwkcp.github.io/2018/05/25/getting-start-postgres/)에 정리해두었다. mysql은 [여기](https://jwkcp.github.io/2018/05/23/prepare-to-use-mysql/)를 참고하면 된다.    
  
## 로케일(locate)
별것 아닌 것 같지만 꼭 필요하고 처음 접하면 은근 시간을 잡아먹는다. ubuntu에서 설정해야 하는 locale과 postgresql에서 설정해야 하는 locale이 있다. ubuntu에서 locale 설정은 [여기](https://jwkcp.github.io/2018/06/01/how-to-set-locale-on-ubuntu/)에 정리해두었다. postgresql의 locale은 /etc/postgresql/9.5/main/postgresql.conf 파일을 열어보면 중간 쯤에 lc_로 시작하는 부분이 있다.  
  
---

# 장고 공식 문서의 배포 가이드 번역
관련 내용을 살펴보고 처음 적용해보면서 어차피 보는 거 번역해두면 좋겠다 싶었다.  
  
- [Django(장고) 배포 가이드(번역) - 1. 시작하기](https://jwkcp.github.io/2018/05/20/deployment-start/)
- [Django(장고) 배포 가이드(번역) - 2. WSGI를 이용해 배포하기](https://jwkcp.github.io/2018/05/20/deployment-wsgi/)
- [Django(장고) 배포 가이드(번역) - 3. 배포 체크리스트](https://jwkcp.github.io/2018/05/11/deployment-checklist-for-django/)
- [Django(장고) 배포 가이드(번역) - 4. 아파치 서버와 mod_wsgi 사용법](https://jwkcp.github.io/2018/05/21/deployment-mod-wsgi/)
- [Django(장고) 배포 가이드(번역) - 6. Gunicorn 사용법](https://jwkcp.github.io/2018/05/23/deployment-gunicorn/)
- [Django(장고) 배포 가이드(번역) - 7. uWSGI 사용법](https://jwkcp.github.io/2018/05/23/deployment-uwsgi/)

---
  
# 마치며
간단하다고 생각했는데 생각보다 간단하지 않았다. 예를 들어, 설정 파일을 수정하려면 vi를 알아야 하고 vi는 알지만 좀 더 효율적으로 수정하기 위해선 잘 안쓰던 명령도 찾아봐야했다. 좀 귀찮은 과정이만 시간이 누적될수록 생산성이 향상되기 때문이다. 어쩌면 나무를 베기 전 도끼날을 날카롭게 가는 과정이 대단하고 거창한 무언가가 아니라 사소하고 작고 당장은 안하고 싶고 그런, 끊임없이 잊을만하면 나를 괴롭히는 그런 것들을 조금씩 없애며 배우고 익숙하게 만들어 궁극적으로는 자연스럽게 되고 생산성으로 이어지는 과정이 아닐까 생각했다. 생각해보면 [맞춤법](https://chrome.google.com/webstore/detail/%ED%95%9C%EA%B5%AD%EC%96%B4-%EB%A7%9E%EC%B6%A4%EB%B2%95-%EA%B2%80%EC%82%AC%EA%B8%B0/jglndaljomjcobaaoohfccecjcekcfgo?hl=ko)도 그렇고, 리눅스 명령어도 그렇고, AWS도 그렇고, 맥도 그렇고, VSCode나 Atom이나 Sublime텍스트도 그렇다. 긴가 민가 싶으면 찾아보아야 한다. 다음에 또 찾게 되더라도 찾아보아야 한다. 마치 고구마 줄기 꺼내듯 모르고 낮설고 어색한 것들이 줄줄이 달려 나오지만 귀찮아하지 않고 조금씩 배우다보면 어느 순간 다른 사람이 '너는 어떻게 그런 걸 알고 있니' 묻는 상황을 만나게 된다.   
  
이렇게 직접 배포를 하기 전 사실 많은 고민이 들었다. 이렇게 배포하는 게 맞는건가? 요즘은 어떻게 하나? 가상서버는 어디 것을 써야하나, EC2는 이게 제일 싼가. Lightsail은 더 싸던데, 뭔가 나중에 발견되어 문제를 만들 그런 제약 사항이 있는 건 아닌가. DB는 직접 VPS에 설치해야 하나, RDS를 써야 하나, 요금 폭탄 맞으면 어쩌지. 지금 사이즈에 알맞는 선택은 뭘까. 실서버에도 가상환경을 구성해야 하나. 로컬 소스를 어떻게 실서버에 옮기면 좋을까. Elastic Beanstalk라는 서비스는 소스만 압축해서 올리면 다 된다던데, 이러면 장애가 생겼을 때 내가 대응할 수 있을까. Amazon linux가 최적화가 잘 되어 있지 않을까, Ubuntu를 더 많이 쓰는 것 같긴 한데. AWS말고 GCP나 DigitalOcean, Linode, Cafe24같은 곳은 어떨까. 도커를 이용하면 좀 나을까. mysql은 mysql workbench가 있었는데 postgresql은 클라이언트 프로그램으로 뭘 쓰면 좋을까. AWS VPC와 Subnet을 처음부터 구성하지 않아도 될까. EBS와 CPU코어 수는 어떻게 해야 할까. GiB는 뭘까. EIP 릴리즈는 했나. 리전은 어디로 선택해야 할까. 다른 리전에 생성하고 종료하지 않은 서버가 남아있었네. 아이구.  
  
정말 꼬리에 꼬리를 무는 이런 질문과 궁금증은 사실 가급적 시행착오를 줄이고 실수하지 않고 최적화된 길로 가려는 생각에 기반하지 않았나 싶다. 그래서 선듯 뭘 해보지 못하고 검색하고 생각하고 고민하고 이런 과정을 반복하기도 했다. 그런데 돌아보면 생각은 많았으나 진전이 없었고, 실수를 줄이려 했으나 사실 시도하지 않음으로 해서 실수가 없었다. 따라서 배우는 것도 없었고 늘 고민으로 마음은 무거웠으며 A vs B, What is the best practice ... 와 같은 질문과 답변에 시간을 챗바퀴를 돌았다. 아직 나와 비슷한 고민을 했던 분들이 있다면 먼저 몸을 움직여 시도해보는 것이 낫다. 걱정하지 말자. 고장나지 않는다. 시도해보자. 
  
그래도 뭐부터 해야 할지 막막하다면 장고로 Hello, world를 표시하는 프로그램을 짜고, AWS에 프리티어를 가입한 다음, EC2 t2 micro를 만들고, 온라인에 내 서비스가 보여지게 하는것 부터 해보자. 그 다음에 DB도 붙고, Git도 붙고, Gunicorn도 붙고(또는 uWSGI), nginx도 붙고, systemd도 붙여보고, nano도 써봤다가 vi도 써보고 하면 된다. 나중에 남은 것들을 고민하지 말자. 대충 저 앞에 개울이 있고 점프를 하거나 헤엄치면 된다는 정도만 알아도 충분하다고 생각한다.  
  

