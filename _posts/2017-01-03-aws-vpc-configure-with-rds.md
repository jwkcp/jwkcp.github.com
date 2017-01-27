---
layout: post
title: Amazon RDS DB 인스턴스에 사용할 Amazon VPC 생성
tags: aws rds vpc
comments: true
---
[![con-VPC-sec-grp.png](https://s26.postimg.org/ezs1rf14p/con_VPC_sec_grp.png)](https://postimg.org/image/406uftaph/)

AWS 인프라 구성을 할 때, 아니 굳이 AWS를 사용하지 않더라도 3-tier 구성을 하려면 네트워크 구성을 웹서버 등이 놓이는 DMZ 공용 영역과 데이터베이스가 위치하여 외부 접근을 차단한 사설 영역으로 나눈다. 뭐 이렇게 구성하지 않고 한 서버에 다 몰아넣고 서비스하는 회사도 꽤 많지만 좋은 방법은 아니다. (그러면 안된다.)

AWS 문서를 보던 도중, 참고가 될 만한 문서가 있어 스크랩해둔다.

> “공용 인터넷이 아닌 웹 서버에서만 Amazon RDS DB 인스턴스를 사용할 수 있어야 하므로 퍼블릭 및 프라이빗 서브넷이 모두 있는 VPC를 생성합니다. 퍼블릭 서브넷에서 웹 서버를 호스팅하므로 웹 서버에서 퍼블릭 인터넷에 액세스할 수 있습니다. Amazon RDS DB 인스턴스는 프라이빗 서브넷에서 호스팅됩니다. 동일한 VPC에서 호스팅되므로 웹 서버에서는 Amazon RDS DB 인스턴스에 연결할 수 있지만, 퍼블릭 인터넷에서는 Amazon RDS DB 인스턴스에 액세스할 수 없어 보다 강화된 보안이 가능합니다.
>
> 다음은 퍼블릭 서브넷과 프라이빗 서브넷을 모두 포함하는 VPC와, 해당 보안 그룹을 생성하는 절차입니다.”
>
>– 본문 중 –

[여기](http://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html "여기")를 클릭하여 내용 더 보기