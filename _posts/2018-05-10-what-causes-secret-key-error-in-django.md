---
layout: post
title: ImproperlyConfigured: The SECRET_KEY settings must not be empty 에러가 발생하는 이유
tags: django
comments: true
---

---

Django(장고)는 DJANGO_SETTINGS_MODULE 변수를 통해 settings.py를 읽은 후 거기에 포함된 SECRET_KEY를 읽어 처리합니다. 'ImproperlyConfigured: The SECRET_KEY settings must not be empty' 이 에러가 발생하는 이유는 Django가 settings.py를 못찾고 있기 때문입니다.  

settings.py를 폴더 아래로 옮겼거나, 파일명을 바꿨거나 했다면 BASE_DIR이 폴더 깊이(depth)에 맞게 잘 설정되어 있는지(print로 찍어보고) 확인해보세요. settings.py의 파일명은 마음대로 바꿔써도 됩니다만 바뀐 파일명을 참조할 수 있도록 읽어들이는 부분을 손봐줘야 합니다.  

manage.py(개발환경)이나 wsgi.py(운영환경)의 DJANGO_SETTINGS_MODULE가 변경(했을 것으로 예상됩니다만..)한 설정 파일을 잘 참고하도록 되어 있는지 다시 확인하시기 바랍니다.   

이 에러는 경로든, 명칭이든 간에 settings.py가 잘못되었을 확률이 백프로라고 보시면 크게 틀리지 않습니다.   
