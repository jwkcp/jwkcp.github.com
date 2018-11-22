---
layout: post
title: json.dumps 메서드 사용 시 한글이 유니코드 형태로 저장될 때 해결방법
tags: python
comments: true
---

ensure_ascii의 값을 False로 설정해주면 됩니다.

```
json.dumps(data, ensure_ascii=False)
```
