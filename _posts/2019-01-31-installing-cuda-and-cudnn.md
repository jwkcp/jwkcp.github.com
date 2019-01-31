---
layout: post
title: 딥러닝 환경 구축하기
tags: ai ubuntu
comments: true
---

# 기본
1. 이 문서는 엔비디아(nvidia)의 GPU를 장착한 머신에서 tensorflow-gpu를 사용하기 위해 준비하는 과정을 다룹니다.
2. 2018년 1월 31일 현재 [tensorflow는 아래 환경](https://www.tensorflow.org/install/gpu)을 지원합니다.
   - GPU: 384.x 또는 그 이후 버전
   - CUDA: 9.0
   - cuDNN SDK: >= 7.2
3. [CUDA 9.0을 설치](https://developer.nvidia.com/cuda-90-download-archive)하려면 그에 맞는 운영체제를 설치해야 하는데 우분투(Ubuntu)의 경우 17.04와 16.04를 지원합니다.

4. [cuDNN](https://developer.nvidia.com/cudnn)을 설치할 때는 엔비디아 회원가입이 필요하다. 가입 후 설치 목록에 보면 .deb 패키지를 제공하는 버전이 16.04가 가장 최신이다. 다른 버전은 cuDNN Library for Linux를 설치하고 환경설정 등을 직접 해줘야 합니다.

**만약 자신이 딥러닝이 목표이고 우분투 최신 버전(18.04)를 꼭 사용안해도 되며 환경설정에 시간을 쓰고 싶지 않다면 아래 버전으로 설치하길 권한다.**

- **운영체제**: 우분투(Ubuntu) 16.04 LTS
- **Nvidia GPU 드라이버**: 우분투(Ubuntu) 시스템 설정 > 소프트웨어 & 업데이트 > 추가 드라이버 탭에서 nvidia... 로 시작하는 라디오 버튼 선택 후 적용
- **CUDA**: v9.0 .deb 패키지
- **cuDNN**: v7.4.2 .deb 패키지
- **파이썬**: v3.6

# CUDA 설치 과정 풀어보기

[nvidia 공식 사이트](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal)에 가보면 아래와 같이 설치 순서가 기재되어 있다. 여기서는 cuda 9.0 설치를 예로 들지만 다른 버전도 기본 절차와 의미는 동일하다.

## 프롤로그
.deb 확장자 프로그램을 처음 설치 해본 건 크롬, 비주얼 스튜디오 코드 등 이었는데 이 .deb 파일은 더블 클릭하면 우분투 소프트웨어(Ubuntu software) 로 연결이 되고 설치 후 설치된 목록에도 잘 나타났다. .deb 파일 설치도 .exe와 비슷하구나 싶었는데 cuda 설치를 위해 nvidia 사이트에서 제공하는 .deb를 더블 클릭했더니 위에 크롬 설치시와 동일하게 우분투 소프트웨어(Ubuntu software)로 연결이 되고 설치 버튼이 나왔고 설치 후에 설치 프로그램 목록에 보였다. 여기까지는 이전 설치 경험과 다를게 없었다. 하지만 프로그램이 제대로 설치된 것인지 아닌지 확인하기 위해 nvcc 명령을 터미널에 입력했을 때 해당 명령을 인식하지 못했고, 우분투 Super키를 누른 후에 별다른 프로그램 아이콘이 보이지 않아 뭔가 잘못되었음을 알았다. 다시 nvidia 공식 사이트에 가보니 아래와 같이 별도의 설치 과정이 있었다. .deb 파일을 더블 클릭하는 것만으로 끝난게 아니란 말이다. 그래서 설치 순서를 들여다 보기 시작했고 아래와 같은 의문이 생겼다.

## 설치 순서
> 1. `sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb`
> 2. `sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub`
> 3. `sudo apt-get update`
> 4. `sudo apt-get install cuda`

## 의문
위 과정에서 왜 dpkg와 apt-get을 함께 썼을까하는 의문이 들었다. 구글링을 해봐도 딱히 원하는 답이 나오지 않고 원론적인 답변, 이를테면 dpkg지는 데비안 계열의 패키지 관리자로 설치 등을 담당하지만 의존성 관리 등의 고급 기능은 제공하지 않고, apt-get은 의존성 관리와 업데이트 등의 고급 기능도 가능하다는 것. 그리고 apt-get 내부적으로 dpkg를 사용하고 있다는 것. 그럼 왜 dpkg 후에 apt-get으로 cuda를 재설치(?)하는 것일까?

## dpkg -i로 설치를 왜 하나?
dkpg -i로 설치해도 cuda가 설치되지만 패키지 간 의존성을 보지 않고 설치가 된다. apt-get을 통해 지속적 관리가 어려워지기 때문에 업데이트나 패키지 간 의존성이 깨지면 문제가 복잡해진다. 그럼 dpkg -i 명령은 왜 설치 순서에 포함되어 있을까. 두 가지 먼저 살펴볼 것이 있다.
    
첫째, dpkg -i를 통해 패키지 설치하면 설치 도중 키가 키가 없다면서 설치가 마무리되지 않고 중단된다. 그러면서 apt-key 명령을 통해 키를 추가할 수 있다는 안내가 나온다. 이 키의 위치가 /var 아래 cuda로 시작하는 디렉토리다. dpkg -i 명령을 실행하기 전에 /var 에 들어가보면 cuda로 시작하는 디렉토리가 없다. dpkg -i를 실행 후 들어가보면 cuda로 시작하는 디렉토리가 생겼고 그 안에 공개키(.pub)를 포함 수 많은 .deb 파일이 생성되어 있음을 볼 수 있다.

둘째, 위에 1, 2, 3을 무시하고 곧바로 sudo apt-get install cuda를 해보자. 설치가 안된다. 다운받을 위치를 apt-get 패키지 관리자가 모르는거다. apt-get은 /etc/apt 디렉토리에 있는 sources.list 파일과 sources.list.d 디렉토리를 참조한다. sources.list 파일을 열어보면 우분투에서 인정(?)한 공식 저장소 주소들을 볼 수 있다. sources.list.d에는 사용자가 추가한 저장소가 계속 추가된다. 

그렇다. dpkg -i 명령은 /etc/apt/sources.list.d에 cuda 저장소의 위치를 추가하고, /var 디렉토리 아래 cuda 관련 파일들(.pub, .deb 등)을 풀어놓는다. 그래서 apt-get install cuda 명령이 동작하도록 하는 것이다.

## 그럼 apt-key는 뭔가?
apt-key는 쉽게 말해 저장소가 유효한지 인증해주는 절차다. apt-key를 통해 cuda 설치를 위한 nvidia의 공개키를 등록함으로써 /etc/apt/sources.list.d에 추가된 저장소에서 cuda를 내려받을 수 있게 인증하는 절차다. dpkg -i를 통해 /var 디렉토리 아래 cuda로 시작하는 디렉토리 하위에 .pub 키가 존재하는 이유다.

## 마지막 명령 apt-get install cuda는?
이제 apt-get install cuda를 하면 cuda 패키지가 설치되는 걸 볼 수 있다. dpkg -i 과정을 통해 1.cuda 저장소가 추가되었고 2.관련 파일이 설치되며 .pub 키가 생겼고 3.이를 등록했다. 4.apt-get update로 필요한 파일도 업데이트 되었다. 마지막으로 apt-get install cuda를 통해 cuda도 apt-get 패키지 매니저가 관리할 수 있게 된 것이다. 

--
리눅스를 메인으로 처음 사용하다보니 .deb 파일과 dpkg, apt-get, apt-key 그리고 관련 메커니즘에 대해 잘 몰랐는데 이번 기회에 많이 배웠다. 나는 조금 귀찮더라도 그대로 명령을 따라 치는 것이 아니라 왜 그런지 알아두려고 노력하는 편이다. 그러면 괴로움은 두고 두고 즐거움이 되며 실력으로 쌓이기 때문이다.

