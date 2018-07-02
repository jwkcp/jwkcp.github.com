---
layout: post
title: Django 로깅 사용법 및 템플릿
tags: django
comments: true
---

logging은 포맷터, 핸들러 등을 코드에서 지정해줘도 되지만 그러면 소스 코드가 지저분해지며 관리하기가 복잡하고 비효율적이 된다. 이런 이유에서 settings.py에 LOGGING 값을 지정해두고 사용하면 편하다. 
    
---

## 소스에서 (예를 들어, views.py)
~~~
import logging

# 아래 'default'는 settings.py의 LOGGING 값에서 임의로 정하는 값이다. 설정을 불러오는 키(key)이며 'abc'와 같은 이름으로 지정해도 상관없다. 
logging = logging.getLogger('default')
~~~
  
## 설정에서 (예를 들어, settings.py)
~~~
LOGGING = {
    'version': 1,
    'diable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': '%(asctime)s [%(levelname)8s] %(message)s'
        },
    },
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'standard'
        },
        'logfile': {
            'level': 'INFO',
            'class': 'logging.handlers.RotatingFileHandler',
            'maxBytes': 1024 * 1024 * 10,   # 로그 파일 당 10M 까지
            'backupCount': 10,              # 로그 파일을 최대 10개까지 유지
            # 'class': 'logging.FileHandler',
            'filename': 'logs/logfile.log',
            'formatter': 'standard'
        },
    },
    'loggers': {
        'default': {
            'level': 'DEBUG',               # 로거의 기본 레벨. 이 레벨이 우선시 된다.
            'handlers': ['console', 'logfile']
        },
    },
}
~~~