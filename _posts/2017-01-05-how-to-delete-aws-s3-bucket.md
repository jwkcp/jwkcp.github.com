---
layout: post
title: AWS Elastic Beanstalk로 생성한 S3가 삭제되지 않을 때
tags: aws beanstalk s3
comments: true
---
AWS를 처음 쓰는 사람들 중 Elastic Beanstalk가 자동으로 생성한 S3 인스턴스를 삭제하려고 할 때 삭제가 되지 않아 당황하시는 분들이 있을 것입니다. 그건 S3의 삭제 권한 설정 때문인데요. 아래 순서대로 따라하시면 바로 S3 버킷을 삭제할 수 있습니다.

[![BucketPolicyEditor.png](https://s26.postimg.org/ohrsrgmt5/Bucket_Policy_Editor.png)](https://postimg.org/image/3xmysz71x/)

S3의 All buckets이 표시된 리스트에서 삭제를 원하는 항목을 누르고 우측 상단의 None, Properties, Transfers 중 Properties를 누릅니다. 하위에 표시되는 메뉴 중 Permissions을 누르면 나오는 설정 중 “Effect”: “Deny”라고 된 부분의 값을 “Allow”로 바꿔주고 저장합니다.

다시 삭제를 해보면 즉시 삭제가 되는 것을 볼 수 있습니다.