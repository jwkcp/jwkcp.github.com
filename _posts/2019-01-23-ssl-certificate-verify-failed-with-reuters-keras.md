---
layout: post
title: 케라스(Keras)에서 reuters 데이터셋 받을 때 SSL:CERTIFICATE_VERIFY_FAILED 에러가 발생하는 경우 해결방법
tags: ai keras python
comments: true
---

## 문제

케라스로 로이터 데이터셋을 받을 때 아래와 같은 에러가 발생한다. 해결 방법을 맥(Mac) 기준으로 설명한다.

```
Using TensorFlow backend.
Downloading data from https://s3.amazonaws.com/text-datasets/reuters.npz
Traceback (most recent call last):
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/urllib/request.py", line 1318, in do_open
    encode_chunked=req.has_header('Transfer-encoding'))
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 1239, in request
    self._send_request(method, url, body, headers, encode_chunked)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 1285, in _send_request
    self.endheaders(body, encode_chunked=encode_chunked)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 1234, in endheaders
    self._send_output(message_body, encode_chunked=encode_chunked)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 1026, in _send_output
    self.send(msg)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 964, in send
    self.connect()
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/http/client.py", line 1400, in connect
    server_hostname=server_hostname)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ssl.py", line 407, in wrap_socket
    _context=self, _session=session)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ssl.py", line 814, in __init__
    self.do_handshake()
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ssl.py", line 1068, in do_handshake
    self._sslobj.do_handshake()
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/ssl.py", line 689, in do_handshake
    self._sslobj.do_handshake()
ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:777)

During handling of the above exception, another exception occurred:
```

## 원인

케라스 라이브러리가 문제 있는게 아닌가 생각했다면 SSL: CERTIFICATE_VERIFY_FAILED 메시지에 주목할 필요가 있다. 이 메시지는 케라스 라이브러리가 로이터(reuters) 데이터셋을 받으려고 했는데 SSL 인증에 실패했다는 말이다. 왠 SSL? 했다면 위에 다운로드 주소를 다시 보자.

> Downloading data from https://s3.amazonaws.com/text-datasets/reuters.npz

그렇다. 케라스는 아마존 S3에서 https를 통해 데이터를 받으려고 했는데 SSL 인증에 실패하면서 다운받지 못했던 것이다.

## 해결방법

이 내용은 맥에 설치된 파이썬 디렉토리의 문서(/Applications/Python 3.6/ReadMe.rtf)에도 존재한다. 아래 명령을 통해 인증서 설치를 해주면 문제가 해결된다.

```
source /Applications/Python\ 3.6/Install\ Certificates.command
```

위에 보면 역슬래시 문자가 두 개 보이는데 이는 공백을 공백으로 인식하기 위한 이스케이프 문자다. 쪼개보면 이런 구조다.
맨 마지막에 Install Certificates.command는 전체가 파일 하나의 이름이다. 앞에 Install 이게 무슨 명령어라고 생각하지 말자.

```
/Applications
    ㄴ Python 3.6
      ㄴ Install Certificates.command
```
