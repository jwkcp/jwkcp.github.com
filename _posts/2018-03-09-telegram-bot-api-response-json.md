---
layout: post
title: 텔레그램(telegram) 봇 API 응답 값 살펴보기
tags: telegram, bot
comments: true
---
  
## 내 정보 요청 (getMe)
> https://api.telegram.org/bot토큰/getMe   
~~~
{
	"ok": true,
	"result": {
		"id": 837463521,
		"is_bot": true,
		"first_name": "my_bot",
		"username": "my_bot"
	}
}
~~~
  
## 메시지 보낼 때 (sendmessage)
> https://api.telegram.org/bot토큰/sendmessage?text=MSG&chat_id=098374623  
~~~
{
	"ok": true,
	"result": {
		"message_id": 242,
		"from": {
			"id": 987364523,
			"is_bot": true,
			"first_name": "내 봇 이름",
			"username": "my_bot"
		},
		"chat": {
			"id": 098374623,
			"first_name": "대화상대자 성명",
			"username": "대화상대자 아이디",
			"type": "private" 
		},
		"date": 1520578752,
		"text": "MSG"
	}
}
~~~
  
## 메시지 받을 때 (getUpdates)
> https://api.telegram.org/bot토큰/getUpdates   
~~~
{
	"ok": true,
	"result": [{
		"update_id": 234324341,
		"message": {
			"message_id": 237,
			"from": {
				"id": 73645321,
				"is_bot": false,
				"first_name": 성명,
				"username": 아이디,
				"language_code": "ko-KR"
			},
			"chat": {
				"id": 73645321,
				"first_name": 성명,
				"username": 아이디,
				"type": "private"
			},
			"date": 1520561516,
			"text": "d"
		}
	}, {
		"update_id": 234324342,
		"message": {
			"message_id": 238,
			"from": {
				"id": 73645321,
				"is_bot": false,
				"first_name": 성명,
				"username": 아이디,
				"language_code": "ko-KR"
			},
			"chat": {
				"id": 58506478,
				"first_name": 성명,
				"username": 아이디,
				"type": "private"
			},
			"date": 1520561804,
			"text": "s"
		}
	}]
}
~~~
  
## 웹훅 정보 설정 (setWebhook)  
> https://api.telegram.org/bot토큰/setWebhook?url=웹훅URL
~~~
{
	"ok": true,
	"result": true,
	"description": "Webhook was set"
}
~~~   
  
** url 파라메터 없이 요청을 보내면 웹훅이 삭제되므로 참고하자.  
  
## 설정된 웹훅 정보 요청 (getWebhookInfo) 
> https://api.telegram.org/bot토큰/getWebhookInfo   
~~~
{
	"ok": true,
	"result": {
		"url": 메시지를 봇이 받으면 텔레그램이 이 URL로 보내준다. 메시지가 왔다는 알림을 받을 URL,
		"has_custom_certificate": false,
		"pending_update_count": 5,
		"last_error_date": 1520577548,
		"last_error_message": "Wrong response from the webhook: 500 Internal Server Error",
		"max_connections": 40
	}
}
~~~
  
## 설정된 웹훅 정보 삭제 (deleteWebhook)
> https://api.telegram.org/bot토큰/deleteWebhook   
~~~
{
	"ok": true,
	"result": true,
	"description": "Webhook was deleted"
}
~~~
  
---
  
더 자세한 정보는 [텔레그램 공식 문서](https://core.telegram.org/bots/api)를 참고하고 리턴된 JSON을 예쁘게 만들어서 보고 싶다면 [JSONLINT](https://jsonlint.com/)를 이용하자.
  