---
layout: post
title: eslint 사용 시, exit code 127과 함께 yarn start 실행 시 오류가 발생하는 경우 해결방법
tags: react
comments: true
---
      
create-react-app으로 리액트 앱 생성 후 eslint를 사용하기 위해 *yarn add eslint* 로 설치, *yarn --init* 로 설정 후 *yarn start*를 하면 아래와 같은 오류가 발생한다. 
     
~~~
Command failed with exit code 127
~~~
       
이미 create-react-app에 eslint가 포함되어 있는데 수동으로 따로 또 설치하면서 의존성 모듈 설치까지 덮어쓰면서 문제가 생긴 것이다. 이 문제를 피하려면 *create-react-app* 로 앱 설치 후 별도로 eslint를 설치하지 않고 기본적으로 포함된 모듈을 사용한다. 그리고 *eslint --init* 시 별도의 의존성 라이브러리를 설치하지 않고 *create-react-app*에 포함된 것을 쓴다. 그러면 *yarn start*가 정상 실행되는 것을 볼 수 있다.

   