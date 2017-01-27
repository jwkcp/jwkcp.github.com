---
layout: post
title: Apache 웹 서버에 대한 파일 권한 설정 방법
tags: apache how-to
comments: true
---
[![LSBAWS_HTTP_request_response.png](https://s26.postimg.org/7iiuc7bll/LSBAWS_HTTP_request_response.png)](https://postimg.org/image/9zuljgvhx/)

1. 다음 명령을 사용하여 www 그룹을 EC2 인스턴스에 추가합니다.
> [ec2-user ~]$ sudo groupadd www

2. ec2-user 사용자를 www 그룹에 추가합니다.
> [ec2-user ~]$ sudo usermod -a -G www ec2-user

3. 권한을 새로 고치고 새 www 그룹을 포함하려면 로그아웃합니다.
> [ec2-user ~]$ exit

4. 다시 로그인한 다음, groups 명령을 사용하여 www 그룹이 있는지 확인합니다.
> [ec2-user ~]$ groups
    ec2-user wheel www

5. /var/www 디렉터리 및 해당 콘텐츠의 그룹 소유권을 www 그룹으로 변경합니다.
> [ec2-user ~]$ sudo chown -R root:www /var/www

6. /var/www 및 그 하위 디렉터리의 디렉터리 권한을 변경해서 그룹 쓰기 권한을 추가하고 나중에 생성될 하위 디렉터리에서 그룹 ID를 설정합니다.
> [ec2-user ~]$ sudo chmod 2775 /var/www
> [ec2-user ~]$ find /var/www -type d -exec sudo chmod 2775 {} +
 
7. /var/www 및 하위 디렉터리의 파일 권한을 계속 변경해서 그룹 쓰기 권한을 추가합니다.
> [ec2-user ~]$ find /var/www -type f -exec sudo chmod 0664 {} +