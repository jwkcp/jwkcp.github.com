---
layout: post
title: 로컬에서 테스트용 초간단 웹서버 띄우기
tags: html, webserver
comments: true
---

로컬에서 웹서버를 띄워 html, js 등을 테스트하고 싶은데 nginx나 apache를 깔고 설정하고 하는 것이 번거롭다면 아래의 라이브러리를 이용하면 수 초만에 웹서버를 띄우고 작성한 코드를 테스트해볼 수 있다.

'''
npm install -g live-server
'''

yarn을 쓰는 사람은 아래 커맨드로 설치할 수 있다.  
'''
yarn global install live-server
'''

> npm을 설치할 때 -g 인자나 yarn으로 설치할 때 global을 쓰는 것은 이 프로젝트 말고 다른 프로젝트에서도 live-server 기능을 쓰기 위한 것이다. 딱 이 프로젝트에서만 쓰고 싶은 사람은 -g나 global을 빼고 명령어를 설치하면 된다.

실행은 아래의 커맨드로 할 수 있다.  
'''
live-server 폴더명
'''

예를 들어, project/public 이란 구조로 되어 있으면 아래처럼 하면 된다.  
'''
live-server public
'''
