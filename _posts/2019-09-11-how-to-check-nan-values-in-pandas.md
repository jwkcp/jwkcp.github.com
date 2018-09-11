---
layout: post
title: Dataframe 자료형에서 NaN 값 찾아내기
tags: dataframe pandas
comments: true
---
      
캐글(Kaggle)과 같이 Pandas의 DataFrame 자료형을 다룰 때 가장 먼저하는 것이 NaN과 같은 값의 데이터를 처리하는 일이다. 아래 방법으로 어떤 항목이 NaN을 가지고 있는지 확인할 수 있다. 

~~~
DATAFRAME["COLUMN"].isnull().values.any()
~~~
     
대문자로 표기한 DATAFRAME이나 COLUMN은 자기가 다루는 데이터 변수명을 입력하면 된다. 그러면 결과가 True 또는 False로 나온다.

---

DATAFRAME["COLUMN"]은 타입을 확인해보면 class 'pandas.core.series.Series 이다.
이미 눈치챈 분도 계시겠지만 한 컬럼이 아니라 DATAFRAME 전체를 한번에 검사하려면 아래와 같이 하면 된다.

~~~
DATAFRAME.isnull().any()
~~~
