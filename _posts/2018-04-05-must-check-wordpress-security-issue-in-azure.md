---
layout: post
title: 애저(Azure)에서 워드프레스 생성 후 반드시 확인해야 할 DB보안 이슈
tags: wordpress
comments: true
---
  
--- 
  
## 확인해야 하는 부분
애저(Azure)에서 워드프레스를 만들 때 mysql DB도 함께 생성했다면 '모든 리소스 > mysql 서비스 > 연결 보안'에 '방화벽 규칙'을 반드시 확인하자. 처음 워드프레스를 설치한 다음에는 다음과 같이 설정되어 있을 것이다.   
~~~
AllowAll    0.0.0.0    255.255.255.255
~~~
이렇게 되어 있으면 어디서나 DB에 접속할 수 있게 된다. 이 설정은 지운다.  
  
그리고 아래와 같이 자기가 있는 IP만 등록해준다.  
~~~
(명칭)    (자기 아이피)    (자기아이피)

# 예를 들어,
MyPlace    10.21.10.211    10.21.10.211
~~~
  
---
  
## 주의해야 하는 부분
여기서 끝내면 안된다. 바로 위 설정에 '엑세스 설정'을 보면 'Azure 서비스 방문 허용'을 설정하거나 해제할 수 있는 부분이 있다. **반드시 설정**으로 해야 워드프레스가 정상적으로 동작한다. 그렇지 않으면 아래와 같은 에러를 만나게 된다. 워드프레스 서버가 DB서버에 접근할 권한이 없기 때문이다.  
   
~~~
Error establishing a database connection

This either means that the username and password information in your wp-config.php file is incorrect or we can’t contact the database server at yeonsys-mysqldbserver.mysql.database.azure.com. This could mean your host’s database server is down.

Are you sure you have the correct username and password?
Are you sure that you have typed the correct hostname?
Are you sure that the database server is running?
If you’re unsure what these terms mean you should probably contact your host. If you still need help you can always visit the WordPress Support Forums.
~~~