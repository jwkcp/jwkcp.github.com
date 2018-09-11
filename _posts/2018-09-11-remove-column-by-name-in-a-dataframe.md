---
layout: post
title: Dataframe 자료형에서 이름명으로 컬럼 삭제 방법
tags: dataframe pandas
comments: true
---
      
캐글(Kaggle)과 같이 Pandas의 DataFrame 자료형을 다루다보면 열을 제거해야 하는 경우가 생긴다. 아래와 같은 코드로 이름명으로 컬럼을 제거할 수 있다.
    
~~~
your_dataframe_object.drop("여기에 컬럼명", axis=1, inplace=True)
~~~
     
axis 0은 row, 1은 column을 뜻한다. inplace를 True로 하면 새로운 Dataframe 자료형을 만들어 리턴하지 않고, 덮어 쓰라는 이야기다.
