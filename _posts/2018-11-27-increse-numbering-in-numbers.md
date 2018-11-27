---
layout: post
title: 맥 넘버스(Numbers)에서 순차적으로 숫자 증가하며 채우기
tags: numbers tools
comments: true
---

엑셀에서 순차적으로 숫자를 채우려면 셀의 우측 하단 모서리를 잡고 쭉 드레그하면 된다. 그러면 같은 숫자로 채워지고 조그만 툴팁상자같은 것이 나타나면서 연속 숫자 채우기를 할 것인지 물어본다.

하지만 맥에서 사용하는 넘버스(Numbers)는 이게 안된다. 셀의 우측 하단 모서리를 잡고 아래로 드레그하면 셀만 선택이 되고, 셀의 하단 중앙을 잡고 아래로 드레그하면 같은 숫자로 채워진다.  
![넘버스 셀선택](https://support.apple.com/library/APPLE/APPLECARE_ALLGEOS/Product_Help/en_US/PUBLIC_USERS/134390/S3220_autofillHandle4.png)

### 넘버스에서 이 기능을 사용하려면

1. 최소 셀 2개에 값을 입력하고 그 셀을 선택한다.
2. 셀 하단 중앙에 노란점을 잡고 아래로 쭉 드레그한다.

예를 들어, 1, 2, 3, 4 이렇게 하고 싶으면, 첫번째 셀에 1을 입력하고 두번째 셀에 2를 입력하고 이 두 셀을 선택한 다음 선택된 셀의 하단 가운데 노란 점을 쭉 아래로 당기면 된다.

1, 3, 5, 7, 9 이렇게 2개씩 점프하는 값을 입력하고 싶으면 첫번재 셀이 1을 입력하고 두번째 셀이 3을 입력하고 이 두 셀을 선택한 다음 선택된 셀의 하단 가운데 노란 점을 쭉 아래로 당기면 된다.

넘버스(Numbers)에서는 최초 2개 셀의 패턴을 파악해 나머지 셀을 자동으로 채워주는 것이었다. 똑똑하다.

_참고: [https://support.apple.com/kb/PH26375?viewlocale=en_US&locale=fr_FR](https://support.apple.com/kb/PH26375?viewlocale=en_US&locale=fr_FR)_
