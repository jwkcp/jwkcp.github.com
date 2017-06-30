---
layout: post
tags: ssl
comments: true
---
https를 위한 무료 SSL 추천
letsencrypt

https://letsencrypt.org/

인증서 만들기
$sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
또는 sudo apt-get install letsencrypt
$sudo service nginx stop (서버 인증시 필요) –> webroot를 쓰면 필요없는듯
방화벽 오픈 여부 체크(80, 433)
letsencrypt 디렉토리 이동 후
$sudo ./letsencrypt-auto certonly –standalone 설치
약관 동의 후 ssl 사용 도메인 입력 (* 와일드카드 사용 불가)
/etc/letsencrypt/live/도메인에 인증서와 키 생성
nginx 설정 수정 (수정 시 필요하면 이 사이트 참조: https://mozilla.github.io/server-side-tls/ssl-config-generator/)
nginx 재시작
인증서 갱신하기
letsencrypt renew –dry-run –agree-tos 로 테스트
letsencrypt renew 로 crontab. crontab은 sudo로 편집해 root로 실행될 수 있도록
nginx에서 강제 리다이렉트하기
server {
listen 80;
server_name post.helpus.kr;
return 301 https://$server_name$request_uri;
}