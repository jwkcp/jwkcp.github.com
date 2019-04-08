---
layout: post
title: 리액트(React) 개발할 때 어떤 디렉토리 구조가 제일 좋은가요?
tags: react advice
comments: true
redirect_to: https://devlog.jwgo.kr{{ page.url }}
---

오랜 기간 개발해 온 언어 또는 프로젝트의 경우 자신만의 디렉토리 구조가 있거나 팀에서 사용하는 구조가 있을 경우 그 방법을 따르면 된다. 하지만 새로 배워 시작하려는 경우 어떤 디렉토리 구조를 가지는 것이 최선인가는 늘 고민거리다. 외부 라이브러리부터, 이미지, JSON파일과 같은 데이터, js, css, 기타 설정 파일 등을 어떻게 배치하고 구성하면 좋을까.

이런 고민에 대해 리액트 페이지의 [너무 깊이 생각하지 마세요(Don't overthink it)](https://reactjs.org/docs/faq-structure.html#dont-overthink-it) 글은 큰 깨달음을 준다. 리액트 페이지의 글이지만 다른 어떤 언어라도 적용되는 조언이다. 새로운 언어를 시작할 때마다 '베스트 프렉티스'를 찾아해매는 분들이 계시다면 저와 함께 이 글을 읽고 마음의 짐을 벗을 수 있다면 참 좋겠다.

---

## 너무 깊이 생각하지 마세요.

지금 어떤 프로젝트를 막 시작했다면 파일 구조를 선택하는데 5분 이상을 투자하지 마세요. '기능이나 라우팅' 또는 '파일 타입'에 따라 나눠보거나, 혹은 자신만의 방법을 선택해 그냥 코딩을 시작하세요. 어떤 방법을 택하든지 나중에 본격적인 코딩을 하게 되면 십중팔구 뭔가 더 좋은 방법을 찾으려는 자신을 발견할 수 있을겁니다.

그것조차도 뭔가 결정하기 고민된다면 그냥 모든 파일을 한 폴더에 다 집어넣고 시작하세요. 하다보면 디렉토리가 점점 비대해지고 뭔가 조치가 필요하단 생각을 하게 될겁니다. 이때쯤 되면 주로 어떤 파일이 같이 수정되는지 눈에 들어오게 되지요. 일반적으로 함께 수정되는 파일들을 같이 두는 것은 훌륭한 접근 방법입니다. 이걸 우리는 '코로케이션(colocation)'이라고 부릅니다.

실전에서 프로젝트가 점점 커지게되면 여러 파일 구조를 섞어 사용하는 경우를 흔히 볼 수 있습니다. 그러니 입문 과정에 어떤 파일 구조를 선택하는가 하는 문제는 정말 별로 중요하지 않습니다. 그냥 좋아 보이는 파일 구조를 선택하고 코딩을 시작하세요.

_아래는 원문입니다._

> ## Don’t overthink it
>
> If you’re just starting a project, don’t spend more than five minutes on choosing a file structure. Pick any of the above approaches (or come up with your own) and start writing code! You’ll likely want to rethink it anyway after you’ve written some real code.
>
> If you feel completely stuck, start by keeping all files in a single folder. Eventually it will grow large enough that you will want to separate some files from the rest. By that time you’ll have enough knowledge to tell which files you edit together most often. In general, it is a good idea to keep files that often change together close to each other. This principle is called “colocation”.
>
> As projects grow larger, they often use a mix of both of the above approaches in practice. So choosing the “right” one in the beginning isn’t very important.
