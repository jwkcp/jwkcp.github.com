---
layout: post
title: Django(장고)에서 배포 전 SECRET_KEY 감추기
tags: django
comments: true
---
  
---

장고 배포 전 반드시 확인해야 하는 [체크리스트](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/)가 있다. 아래는 그 중에서도 SECRET_KEY를 감추는 2가지 방법이다. 자신이 편한 방법으로 사용하면 된다. 두 번째 '별도 파일로 보관하는 방법'을 사용할 경우 git과 같은 소브 버전 관리 시스템에 해당 파일이 업로드 되지 않도록 .gitignore 파일에 추가해주는 것을 잊지 말자.

~~~
# 서버 환경 설정에서 불러오는 방법
import os
SECRET_KEY = os.environ['SECRET_KEY']

또는

# 별도 파일로 보관하는 방법
with open('etc/secret_key.txt') as f:
    SECRET_KEY = f.read().strip()
~~~
