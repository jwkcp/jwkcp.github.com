---
layout: post
title: Numpy의 corrcoef 함수 간단히 살펴보기
tags: python, numpy, ml
comments: true    
---
  
numpy를 이용해 데이터 공부를 하다보면 corrcoef라는 함수를 만나게 되는데 [피어슨 상관계수(Pearson correlation coefficient)](https://ko.wikipedia.org/wiki/%EC%83%81%EA%B4%80%EB%B6%84%EC%84%9D#%ED%94%BC%EC%96%B4%EC%8A%A8_%EC%83%81%EA%B4%80%EA%B3%84%EC%88%98)를 구하는 함수다. 
  
### 형태  
예를 들어, 아래와 같이 numpy로 할당한 두 배열이 있다고 했을 때 상관계수는 이런 식으로 표시된다.
   
**대상 배열**  
x = np.array([4, 6, 6, 5, 6, 5, 6, 7])  
y = np.array([5, 6, 1, 0, 0, 9, 6, 9])  
*(datacamp.com의 예제를 사용)*  
   
**corrcoef로 구한 피어슨 상관계수 형태**  
  
| 1 |     x      |      y     |
| --- | --- | --- |
| x | 1.         | 0.14586499 |
| y | 0.14586499 | 1.         |
333
| \  | x  | y  |
| --- | --- | --- |
| x  | 1. | 0.14586499 |
| y  | 0.14586499 | 1. |

123  




| Fruit         | Price         | Advantages         |
| ------------- | ------------- | ------------------ |
| Bananas       | $1.34         | {::nomarkdown}<ul><li>built-in wrapper</li><li>bright color</li></ul>{:/} |
| Oranges       | $2.10         | {::nomarkdown}<ul><li>cures scurvy</li><li>tasty</li></ul>{:/} |

  
[0,0]과 [1,1]의 값이 1이다. 음의 기울기( \ )를 가지는 직선 그래프처럼 행렬의 대각선이 쭉 1이다. 이는 가로축, 세로축에 x, y를 두었을 때 자기 자신하고 비교하면 무조건 상관계수가 1이 나오는 특성 때문이다. 

### 간단한 설명
위 예의 경우, 두 배열 간에 상관 관계를 알고 싶을 때 사용한다. 안경을 낀 것(x)과 성적(y)이 어느 정도 상관관계가 있는지 알아보고 싶을 때 상관계수를 구해보면 된다. 상관계수는 -1부터 1까지로 결과 값이 리턴되며 -1이나 1에 가까울수록 상관관계가 높은 것이다. 1은 그래프에서 / 과 같이 양의 기울기를 가지므로 안경을 낄수록 성적이 높아진다고 볼 수 있고, -1의 상관계수를 가진다면 안경을 낄수록 성적이 낮아진다고 해석할 수 있다.
  
한가지 해석에 있어 주의할 점은 상관계수가 높다는 것이 **인과관계**와는 상관이 없다는 것이다. 위 예의 경우 상관계수가 1일때 안경과 성적은 높은 상관이 있지만, 안경을 끼었기 때문에 성적이 높다고 말할 수는 없는 것이다.
  
### 참고
numpy의 corrcoef함수와 관련한 공식 설명은 [여기](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.corrcoef.html)를 참고하세요.
  

