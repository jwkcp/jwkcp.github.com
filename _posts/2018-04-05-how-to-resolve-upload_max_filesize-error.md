---
layout: post
title: 워드프레스 테마 업로드 시 upload_max_filesize 에러가 발생할 경우
tags: wordpress
comments: true
---
  
## 증상
워드프레스 테마 업로드 시 아래와 같은 에러가 발생한다.
~~~
업로드한 파일은 php.ini의 upload_max_filesize에 지정한 크기를 초과하였습니다.
~~~
  
## 해결방법
php.ini 의 upload_max_filesize를 변경하면 된다.
~~~
예) upload_max_filesize=60M
~~~
  
## 참고
php.ini 파일은 보통 /usr/local/php/etc 아래에 있다. 파일이 없거나 내 파일의 설정을 확인하고 싶으면 아래와 같이 test.php를 워드프레스 서비스 루트 디렉토리에 만들고 http://{워드프레스주소}/test.php로 접속해보면 설치된 php에 관한 정보가 나오고 Configuration File (php.ini) Path 항목에서 경로를 확인할 수 있다.   
~~~
<?php phpinfo(); ?>
~~~