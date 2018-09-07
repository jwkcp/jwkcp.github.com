---
layout: post
title: 캐글(Kaggle)에서 정답 제출하는 방법 - 완전 초보자용
tags: how-to kaggle
comments: true
---

# 개요
캐글(Kaggle)에 처음 접속해서 정답을 제출해보는 것까지 한번 해보려고 하는데 어떻게 해야 하는지 잘 모르겠는 경우 아래와 같이 하면 된다. 

---
   
# 캐글에 제출하는 2가지 방법
크게 train.csv와 test.csv가 있는데 이걸 내려받아서 로컬 컴퓨터에서 해봐도 되고, 캐글의 커널(Kernels) 탭을 통해 해봐도 된다. 명령이 즉시 즉시 실행되는 스크립트(Script) 방식과 주피터 노트북같이 인터렉티브한 환경의 노트북(Notebook) 방식을 제공한다. 
    
**캐글 하는 방법**
1. 로컬 컴퓨터에서 작성, 실행하여 정답 csv를 만든 후 업로드하여 제출하는 방법
2. 커널(Kernels) 탭에서 작성, 실행하여 온라인에서 바로 제출하는 방법

1번은 다들 아실테니 2번으로 제출하는 방법에 대해 간단히 설명합니다.

---
 
# 커널을 통해 온라인으로 간편하게 제출하는 방법
코드를 쭉 작성하고 제일 마지막에 csv로 내보내는 코드를 넣는다. 예를 들면, 아래와 같다. DataFrame을 새로 만들어도 되고 그냥해도 되는데 핵심은 to_csv 메서드다. 이 코드를 실행하고 나면 *Output* 이란 탭이 새로 생긴다. 이 탭을 눌러보면 화면 중간 오른쪽에 *Submit to Competition*이라는 파란 버튼이 있고, 이 버튼을 누르면 바로 제출되며 그 결과를 볼 수 있다. 아래는 *Output* 탭이 생기기 전과 후를 캡춰한 화면이다.
      
~~~
my_first_submission = pd.DataFrame({"PassengerId": test_one.PassengerId, "Survived": test_one.Survived})
my_first_submission.to_csv("my_first_submission.csv", index=False)
~~~
      
** Output 탭이 생기기 전 **
[![kaggle_before_commit.png](https://s26.postimg.cc/f7i1swubd/kaggle_before_commit.png)](https://postimg.cc/image/8h1kjh75h/)
    
** Output 탭 생긴 후 **
[![kaggle_with_output.png](https://s26.postimg.cc/fx0u59n55/kaggle_with_output.png)](https://postimg.cc/image/o2iw3fbdx/)
    
---

** 참고: [Submitting From A Kernel, written by DanB](https://www.kaggle.com/dansbecker/submitting-from-a-kernel)
    