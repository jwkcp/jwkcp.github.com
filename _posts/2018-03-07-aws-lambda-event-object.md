---
layout: post
title: AWS lambda(람다) 함수 핸들러 event 변수 살펴보기(Python, POST)
tags: aws, lambda, python, telegram, bot
comments: true
---
  
파이썬으로 AWS lambda(람다) 함수를 구현하려면 핸들러의 매개변수로 event와 context를 받아야 한다. 여기서 event는 사용자 요청에 대한 정보를 담고 있는데 그 구조를 살펴보자.  
아래는 텔레그램 봇을 만들 때 POST 메시지로 보낸 요청을 lambda 함수가 받은 것이다. type을 찍어보면 딕셔너리인 것을 알 수 있다.
  
~~~
{
	'update_id': 439837311,
	'message': {
		'message_id': 13,
		'from': {
			'id': 93827364,
			'is_bot': False,
			'first_name': '홍길동',
			'username': 'honggildong',
			'language_code': 'ko-KR'
		},
		'chat': {
			'id': 93827364,
			'first_name': '홍길동',
			'username': 'honggildong',
			'type': 'private'
		},
		'date': 1520404199,
		'text': 'test message'
	}
}
~~~