---
layout: post
title: AWS EB(Elastic Beanstalk)를 사용할 때 알아둘 점 몇 가지
tags: python, aws, django
comments: true
---
  
aws의 eb자습서에 봐도 정확하게 그런지 아닌지 알기 어려운 점들이 있다. 그래서 직접 배포하고 ec2에 접속해서 확인해 본 점들을 나열해본다. 지속적으로 업데이트 할 예정  
  
***
  
**1. eb deploy를 통해 배포에서 제외할 파일을 설정할 때**  
   git init으로 git이 활성화 되어 있어야 .gitignore 파일이 적용된다. 그렇지 않으면 .ebignore 사용할 것
  
**2. 가상환경 디렉토리까지 eb deploy 대상에 포함시키지 마라.**  
   pip freeze로 생성된 requirements.txt로 eb가 알아서 ec2 인스턴스 /opt/python/run/venv 경로에 가상환경을 만든다.
  
**3. eb로 배포한 소스는 어느 경로에 있나?**  
   opt/python 경로에 bundle과 current 디렉토리가 있다. bundle은 그동안 배포한 것, current는 현재 버전이다. current는 bundle의 최신 버전으로 심볼링 링크가 걸려있다.

**4. eb에서 사용하는 .elasticbeanstalk 디렉토리는 배포 대상이 아니다.**  
   zip으로 압축해서 웹으로 통해 업로드할 때 이 디렉토리도 함께 압축해서 올려야 하는지 궁금했는데 명확하게 딱 표시된 곳이 없었다. 나중에 git init을 한 후 .gitignore파일을 만들었더니 거기에 .elasticbeanstalk를 제외하는 부분이 있더라. 그러니 이 디렉토리는 제외하면 된다. **.elasticbeanstalk** 디렉토리를 열어보면 eb 배포 환경에 관한 파일이 config.yml이 들어 있다. **.elasticbeanstalk**와 비슷하게 생긴 **.ebextensions** 디렉토리는 django.config와 같이 .config 확장자 파일로 배포 시 수행해야 하는 명령이나 WSGIPath 등 옵션을 설정할 수 있는 파일이 들어 있다. 이 디렉토리는 zip으로 압축할 때 같이 압축해주면 된다.

**5. 'Your WSGIPath refers to a file that does not exist' 라는 에러가 나올 때**  
   WSGIPath가 잘못되어 있는 것이다. 아니, 그럴리 없다는 생각이 든다면 아래 내용을 좀 더 읽어보고 자신의 경우와 비교해보자.   
   eb cli로 배포를 했다면 option_settings에 WSGIPath 설정을 해줬을 것이다. 잘 설정했다면 option이 eb에 적용이 안된거다. 종종 이런 경우가 있는 것 같다. eb 애플리케이션과 환경을 수 십번 생성했다 삭제하면서 확인해봤지만 정확히 어떤 시나리오에서 그런지 문제 재연 시나리오를 찾지 못했다. 심지어 정상적으로 배포되어 서비스 되는 경우에도 갑자기 위 에러가 나와서 확인해보면 WSGIPath가 application.py로 바뀌어 있는 것도 테스트 중 몇 번 경험했다. 이 문제 원인을 찾으려고 eb 생성, 삭제만 거짓말 좀 보태서 100번은 해본 것 같다. 갑지기 위 에러가 나는 애플리케이션/환경의 소스를 그대로 가져다가 새로운 애플리케이션/환경을 생성해 배포해보면 "Your WSGIPath..." 에러 없이 잘 구동되는 것도 확인했다. 소스나 소스에 포함된 설정 문제가 아니란 얘기다. 소스, git, eb config, command/option settings와 그에 따른 순서, 네이밍 등 수 많은 변수를 테스트해봤지만 논리적 오류를 찾지 못했다. 현재 상황에서는 aws eb 버그라고 밖에 생각이 안된다. (이 글을 쓰고 있는 시점은 2018년 2월 22일이다.)  
   eb가 정상적으로 WSGIPath를 찾게 해주는 해결 방법은 간단하다. eb 웹페이지에서 '구성 > 소프트웨어'에서 WSGIPath가 application.py로 되어 있을텐데 이걸 자신에게 맞게 '디렉토리이름/wsgi.py'로 바꿔주면 된다. 
    
  
