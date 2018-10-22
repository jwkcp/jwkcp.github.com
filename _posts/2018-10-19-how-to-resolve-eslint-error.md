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
       
이미 create-react-app에 eslint가 포함되어 있는데 수동으로 따로 또 설치하면서 의존성 모듈 설치까지 덮어쓰면서 문제가 생긴 것이다. 이 문제를 피하려면 1.표시된 에러 메시지를 따르거나, 2.*create-react-app* 로 앱 설치 후 별도로 eslint를 설치하지 않고 기본적으로 포함된 모듈을 사용한다. 
    
그리고 *eslint --init* 시 별도의 의존성 라이브러리를 설치하지 않고 *create-react-app*에 포함된 것을 쓴다. 그러면 *yarn start*가 정상 실행되는 것을 볼 수 있다.
      
---
     
예를 들어, eslint --init을 통해 airbnb 설정을 사용하겠다고 결정하면 아래와 같은 모듈이 필요하다. (2018년 10월 기준)
~~~
eslint@^4.19.1 || ^5.3.0 
eslint-plugin-import@^2.14.0 
eslint-plugin-jsx-a11y@^6.1.1 
eslint-plugin-react@^7.11.0

eslint-config-airbnb@latest 
~~~
     
이 중에서 eslint-config-airbnb를 제외하고 나머지는 create-react-app에 포함되어 있다. (버전이 다르다는 경고가 출력될 수 있으니 참고) 따라서 eslint-config-airbnb만 따로 설치해주면 된다.
~~~
yarn add eslint-config-airbnb
~~~
       
airbnb가 아닌 standard를 선택하면 아래 모듈을 설치할 것지 물어본다. 선택에 따라 필요한 모듈이 다르니 참고.
~~~
eslint@>=5.0.0 
eslint-plugin-import@>=2.13.0 
eslint-plugin-node@>=7.0.0 
eslint-plugin-promise@>=4.0.0 
eslint-plugin-standard@>=4.0.0

eslint-config-standard@latest 
~~~


   