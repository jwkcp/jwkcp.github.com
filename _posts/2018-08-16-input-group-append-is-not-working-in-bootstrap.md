---
layout: post
title: 부트스트랩(bootstrap)에서 input-group-append가 동작하지 않을 경우
tags: bootstrap
comments: true
---

## 문제
아마 [http://bootstrap4.kr](http://bootstrap4.kr/)에서 cdn 주소를 복사해와서 사용하고 있을 가능성이 크다. 이 사이트는 [https://getbootstrap.com](https://getbootstrap.com)을 번역해 둔 사이트인데 현재(2018.08.16) 최신 버전의 부트스트랩 cdn 링크가 걸려있지 않고 4.0.0-beta 버전의 cdn 링크가 기재되어 있다. 그리고 이 버전의 input-group-append는 오류가 있다.
     
** bootstrap4.kr에 표시된 cdn 경로. 잘 보면 4.0.0-beta 버전의 링크가 있다.
~~~
CSS only
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/css/bootstrap.min.css" integrity="sha384-/Y6pD6FV/Vv2HJnA6t+vslU6fwYXjCFtcEpHbNJ0lyAFsXTsjBbfaDjzALeQsN6M" crossorigin="anonymous">
JS, Popper.js, and jQuery
<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.3/umd/popper.min.js" integrity="sha384-vFJXuSJphROIrBnz7yo7oB41mKfc8JzQZiCq4NCceLEaO4IHwicKwpJf9c9IpFgh" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta/js/bootstrap.min.js" integrity="sha384-h0AbiXch4ZDo7tp9hKZ4TsHbi047NrKGLO3SEJAg45jXxnGIfYzk4Si90RDIqNm1" crossorigin="anonymous"></script>
~~~
         
## 해결방법
[https://getbootstrap.com](https://getbootstrap.com)에 있는 최신 버전의 안정 버전 링크를 사용하면 된다. 아래를 보면 4.1.3 버전의 cdn 주소가 기재되어 있다. 


~~~
CSS only
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
JS, Popper.js, and jQuery
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
~~~
    
---
    
## 참고
부트스트랩 3.x까지는 input-group-append가 input-group-btn이었다. v4.x로 오면서 바뀌었음.
     

