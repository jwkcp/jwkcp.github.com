---
layout: post
title: 파이썬 모듈 pypi에 배포하는 방법
tags: python, how-to
comments: true
---
  
PyPI에서 라이브러를 사용만해보다 실제 올려보려면 조금 막막하다. 아래의 순서와 설명에 언급된 링크를 참조하면 쉽고 부담없이 자신의 모듈을 올려볼 수 있다.
  
## 순서
1. [pypi.org](https://pypi.org) 그리고 [test.pypi.org](https://test.pypi.org) 가입하기
2. 배포에 필요한 파일(setup.py, setup.cfg, MENIFEST.in) 만들기
   - setup.py: 배포 패키지를 만들 때 설정 파일 역할
   - setup.cfg: 몇 가지 부수적 설정 담당
   - MENIFEST.in: 파이썬 파일 외 함께 포함시킬 파일 기재
3. wheel을 이용해 배포 파일 생성
4. PyPI에 배포
  
---
  
## pypi 사이트 가입
PyPI에 라이브러리를 올리려면 당연히 계정이 있어야 한다. test.pypi.org는 pypi에 올리기 전에 테스트 해볼 수 있는 테스트 저장소로 사이트를 방문해 별도로 가입해야 한다. 필수 사항은 아니지만 실제 올려보기 전에 테스트 저장소에 체크해볼 수 있으므로 권장. 참, 사이트에서 회원 가입을 하면 이메일 인증을 해야 최종 완료된다. 이메일 인증을 하지 않은 계정으로는 라이브러리를 업로드 할 수 없으므로 이메일 인증을 꼭 하도록 하자.
  
## setup 관련 파일 만들기
크게 3가지로 보면 되겠다. setup.py / setup.cfg / MENIFEST.in  
가장 중요한 파일은 setup.py이다. 아래에 간단한 예시가 있다. 프로젝트 최상위 디렉토리 아래에 두면 된다. [이 문서](https://code.tutsplus.com/ko/tutorials/how-to-write-your-own-python-packages--cms-26076)가 큰 도움을 주었다.

### setup.py
~~~

from setuptools import setup, find_packages


setup(
    name             = '프로젝틈명',
    version          = '1.0',
    description      = '간단한 설명',
    long_description = open('README.md').read(),
    author           = '작성자명',
    author_email     = '작성자 이메일',
    url              = 'https://...',
    download_url     = 'https://...',
    install_requires = ['requests'],
    packages         = find_packages(exclude = ['docs', 'example']),
    keywords         = ['bot', 'study', 'api'],
    python_requires  = '>=3',
    zip_safe=False,
    classifiers      = [
        'Programming Language :: Python :: 3',
        'Programming Language :: Python :: 3.2',
        'Programming Language :: Python :: 3.3',
        'Programming Language :: Python :: 3.4',
        'Programming Language :: Python :: 3.5',
        'Programming Language :: Python :: 3.6'
    ]
)
~~~
  
### setup.cfg
아래 예시는 라이브러리를 설명하는 파일로 미리 작성해둔 마크다운형식(.md)을 연결해 주었다.
~~~
[metadata]
description-file = README.md
~~~
  
### MENIFEST.in
필수는 아니지만 이런 식으로 관련 파일을 포함할 수 있다.
~~~
include LICENSE

include README.md
~~~
  
## wheel로 배포 파일 만들기

~~~
pip install wheel
python setup.py bdist_wheel
~~~


## pypi에 배포하기
이제 pypi에 내가 만들 라이브러리를 올려 많은 사람들이 사용해볼 수 있도록 하자. 검색해보면 배포할 정보를 담은 .pypirc 파일을 만드는 것을 많이 볼 수 있을텐데 굳이 안만들어도 된다.  
  
> 자주 나오는 에러
> Q1. pypi.python.org 또는 testpypi.python.org 관련 에러
> A1. 배포 대상 주소로 xxx.python.org를 사용해서 그렇다. 각각 pypi.org와 test.pypi.org를 이용하도록 한다.
>   
> Q2. email verification(이메일 인증) 관련 에러
> A2. pypi 사이트에 회원 가입을 하고 입력한 이메일을 인증하지 않아서 그렇다. 입력한 이메일로 발송된 인증 링크를 클릭한 후 다시 시도한다.
   
#### TestPyPI에 테스트로 올려보기
~~~
pip install twine
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
~~~

#### 실제 PyPI에 올려기
~~~
pip install twine
twine upload dist/*
~~~
  
위 명령을 이용하면 PyPI의 아이디, 비밀번호를 묻는 프롬프트가 나타난다.

---

## 더 읽어보면 도움이 되는 자료
- [TestPyPI 사용법](https://packaging.python.org/guides/using-testpypi/#using-test-pypi)
- [PyPI 튜토리얼](https://packaging.python.org/tutorials/)
- [PyPI 가이드](https://packaging.python.org/guides)
- [나만의 파이썬 패키지를 작성하는 법](https://code.tutsplus.com/ko/tutorials/how-to-write-your-own-python-packages--cms-26076)
- [파이썬 패키지를 공유하는 법](https://code.tutsplus.com/ko/tutorials/how-to-share-your-python-packages--cms-26114)