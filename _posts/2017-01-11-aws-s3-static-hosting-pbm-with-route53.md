---
layout: post
title: AWS S3로 정적 사이트 호스팅 시, Route53 CNAME으로 연결되지 않을 때
tags: aws s3 route53 how-to
comments: true
---

[![amazonaws-e1484111234834.jpg](https://s26.postimg.org/l88s799bt/amazonaws_e1484111234834.jpg)](https://postimg.org/image/hbvgb9oc5/)

### 케이스

S3에 버킷 생성 후 정적 호스팅을 하기 위해 Route53에 CNAME 연결을 하면 아래와 같은 메시지가 뜨면서 사이트가 정상적으로 열리지 않는다. 하지만 <버킷이름>.domain.com.s3-us-west-1.amazonaws.com과 같이 Endpoint로 직접 접속하면 정상적으로 콘텐츠가 표시된다.

> Code: NoSuchBucket

### 원인

S3 버킷 이름과 Route53에서 CNAME으로 입력한 명칭이 달라서 발생

---

### 해결책

S3 버킷 이름과 Route53의 CNAME 명칭을 동일하게 해준다. 예를 들어, domain.com이란 도메인을 보유하고 mytestbucket.domain.com으로 서비스하고 싶다면 –

- S3 버킷이름: mytestbucket.domain.com
- Route53 명칭: mytestbucket.domain.com (대부분 .domain.com은 고정으로 입력되어 있을테니 mytestbucket만 앞부분에 입력하면 됨)

이제 정상적으로 사이트가 로딩되는 것을 볼 수 있다.