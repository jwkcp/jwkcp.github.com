---
layout: post
title: 케글(Kaggle)할 때 판다스(Pandas)로 csv 읽어들일 때 FileNotFoundError 발생하는 경우
tags: python
comments: true
---

사실 이 에러는 케글(Kaggle)과 딱히 상관이 없다. 다만 케글을 처음하려고 할 때 제일 처음하는 것이 pandas로 csv를 읽는 작업인데 여기서 FileNotFoundError를 자주 만나기 때문이다. 이 에러는 상위 폴더 하위에 다른 폴더를 만든 사람들이 대부분 겪는다. 예를 들어 아래와 같이 폴더 구조를 만들었다면 초심자는 대부분 이 에러를 만난다.

```
mydirectory
    L kaggle
        L project
            L data
                L train.csv
            L source.py
```

그리고 아래와 같이 읽어들인다면 FileNotFoundError를 볼 가능성이 높다.
```
import pandas as pd

train_data = pd.read_csv('./data/train.csv', sep=',')
```

왜냐하면 파이썬이 현재 인식하고 있는 디렉토리를 source.py 위치라고 생각하기 때문이다. 하지만 아래 코드로 현재 위치를 확인해보면 mydirectory임을 알 수 있다.
```
import os

print(os.getcwd())
```

따라서 아래와 같이 경로를 지정해야 한다.
```
# No
train_data = pd.read_csv('./data/train.csv', sep=',')

# YES
train_data = pd.read_csv('./kaggle/project/data/train.csv', sep=',')
```
