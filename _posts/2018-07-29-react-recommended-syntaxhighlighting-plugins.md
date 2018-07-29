---
layout: post
title: 리액트 공식 페이지에서 추천하는 문법 강조(Syntax highlighting) 플러그인 
tags: react 
comments: true
---

[리액트 공식 튜토리얼 페이지](https://reactjs.org/tutorial/tutorial.html)에서 추천하고 있는 에디터 별 문법 강조 플러그인을 소개한다. 

---
  
아래와 같이 리액트 튜토리얼 페이지에서 바벨(Babel)에서 소개하는 페이지를 링크 걸고 추천하고 있다.   
> "We recommend following [these instructions](http://babeljs.io/docs/en/editors/) to configure syntax highlighting for your editor."

---
## 각 에디터 별 문법 강조 플러그인 추천
### Atom
[language-babel](https://atom.io/packages/language-babel) 패키지 설치
   
### Sublime Text 3
[Package Control](https://packagecontrol.io/installation) 설치 후 패키지 컨트롤 메뉴를 통해 [Babel](https://packagecontrol.io/packages/Babel) 패키지 설치 

### Vim
[vim-javascript](https://github.com/pangloss/vim-javascript) 플러그인 설치. [yajs.vim](https://github.com/othree/yajs.vim)이나 [ex.next.syntax](https://github.com/othree/es.next.syntax.vim)를 사용하는 것도 방법.

### Visual Studio Code
[sublime-babel-vscode](https://marketplace.visualstudio.com/items?itemName=joshpeng.sublime-babel-vscode) 확장 프로그램 설치. 충돌 방지를 위해 기존에 다른 자바스크립트 문법 확장 프로그램(Latest TypeScript and JavaScript Grammar, Babel ES6/ES7 같은)이 설치되어 있다면 삭제하기를 권장.   
함께 사용할만한 컬러 테마로는 [One Dark](https://marketplace.visualstudio.com/items?itemName=joshpeng.theme-onedark-sublime), [Charcoal Oceanic Next](https://marketplace.visualstudio.com/items?itemName=joshpeng.theme-charcoal-oceanicnext) 추천.    
   
### WebStore
웹스톰은 해당 기능을 포함하고 있기 때문에 별도 설치는 필요없으나 설정에서 [활성화](https://blog.jetbrains.com/webstorm/2015/05/ecmascript-6-in-webstorm-transpiling/)시켜줄 필요가 있을 수도 있음.

> 원문링크: [http://babeljs.io/docs/en/editors/](http://babeljs.io/docs/en/editors/)
