---
layout: post
title: zsh의 path 설정은 어디서 해야 하나?
tags: zsh
comments: true
---
  
sh 쉘은 .bashrc 나 bash_profile을 통해서 path를 설정할 것이다. 하지만 on-my-zsh를 사용하기 위해 zsh 쉘을 사용하는 사람도 많을 것이다. zsh 쉘은 다음 경로에서 path 설정을 할 수 있다.   
  
~~~
~/.zshrc
~~~
  
바로 적용시키기 위해서는  
~~~
source .zshrc
~~~
  
