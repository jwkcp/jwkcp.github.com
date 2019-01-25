---
layout: post
title: 파이썬으로 딥러닝 코드 작성 시 알아두면 요긴한 코드 블럭 지정법
tags: python
comments: true
---

파이썬 코드 작성 시 아래와 같이 코드 블럭을 지정해두면 vscode 같은 텍스트 에디터나 pycharm 같은 IDE에서 코드 블럭을 구분할 수 있을 뿐만 아니라, 각 블럭 별 실행도 가능하게 된다. (짱편함)

특히, 케라스로 데이터셋 불러오는 등 오래걸리고 무거운 작업이 포함된 코드가 포함되어 있을 때 이 방법으로 블럭을 구분, 지정해주면 해당 부분을 제외하고 원하는 부분만 실행할 수 있어 시간도 크게 절약할 수 있다.

**사용법**

```
#%% code block 1 here
print("Hi! I'm code line 1)

#%% code block 2 here
print("Hi! I'm code line 2)
```
