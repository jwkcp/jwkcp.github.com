---
layout: post
title: AWS RDS가 외부에서 접속이 안될 때
tags: how-to, aws, rds
comments: true
---
### 기본사항
포트가 정상적으로 열려 있는지는 아래와 같이 확인할 수 있다. (당연히 telnet으로 확인해도 된다.)  
   
nc -zv [RDS 엔드포인트 주소] [포트]

### RDS의 Publicly Accessible 설정 검토
이 설정은 기본적으로 No가 선택되어 있을 것이다. 이 설정을 Yes로 바꾼다.
잘 알고 있겠지만 DB를 외부에서 접속할 수 있도록 허용하는 것은 권장하지 않으며 매우 위험하다. 필요한 경우에만 설정하여 사용하는 것이 좋으며 일종의 방화벽 역할을 하는 AWS의 Security Group 설정을 통해 꼭 필요한 IP만 접근할 수 있도록 제한한 필요가 있다. Publicly Accessible 설정이 YES인 상태에서 RDS가 속한 Security Group의 Inbound 규칙에 허용 IP를 Any(0.0.0.0/0)으로 설정해두는 일이 없도록 주의하자

### RDS가 속한 Security Group의 Inbound에 규칙 추가
Security Group에 Workbench 등으로 접근하려는 IP나 IP 대역대와 포트를 추가한다. 자신의 아이피가 111.112.113.114라면 뒤에 서브넷마스크를 추가한 111.112.113.114/32를 입력하면 딱 해당 아이피만 접근하도록 설정된다.  

---

위 두 가지 설정이 잘 되어 있음에도 접근이 안된다면 내부에서 사용하는 방화벽 등에 의해 차단되진 않았는지 체크해보자.
