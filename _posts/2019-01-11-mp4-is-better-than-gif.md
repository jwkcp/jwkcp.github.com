---
layout: post
title: gif보다는 mp4를 쓰세요.
tags: web
comments: true
---

html5의 video 태그를 이용해 mp4를 로드하는 것이 gif를 쓰는 것보다 5~20배 빠르다고 한다.([chrome dev summit 2018](https://youtu.be/reztLS3vomE)) 단순하게 생각했을 때 사진(gif)이 동영상(mp4)보다 가벼울 것 같고 동영상은 뭔가 웹사이트에 올리기 부적합한 물건 같은 선입견이 드는데 실제는 그렇지 않다는 말이다. gif는 여러 장의 사진이 뭉쳐있고 ffmpeg으로 압축한 mp4 파일은 높은 효율로 압축한 것이니 다시 생각해보면 이해가 되기는 한다. 트위터의 gif도 표시만 gif로 하고 있지 사실은 동영상이라고 한다. 이제 img 태그의 gif 대신 mp4의 video 태그를 사용하도록 하자.

# Gif 대신 mp4로

```
# BAD! 이렇게 말고
<img src="dog.gif">

# GOOD! 이렇게
<video>
   <source src="dog.mp4" type="video/mp4">
</video>
```

# mp4는 어디서 구하나

ffmpeg으로 gif에서 변환할 수 있다. 맥은 아래와 같이 설치하고 변환한다.

```
# 설치
brew install ffmpeg

# 변환
ffmpeg -i dog.gif dog.mp4
```
