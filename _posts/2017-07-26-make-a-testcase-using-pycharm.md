---
layout: post
title: 파이참(Pycharm)에서 테스트 케이스 만들기와 No tests were found 문제 해결하기
tags: pycharm 
comments: true
---

###기본적으로 아래와 같이 테스트 케이스를 만든다.
1. tests.py 파일에 (파일명은 실행 여부에 영향을 미치지 않는다.)
2. 아래와 같이 TestCase 클래스를 상속받는 테스트 클래스와 함수를 만든다.
{% highlight python %}
from django.test import TestCase


class MyClass(TestCase):
    def test_my_func(self):
        self.assertTrue(False)
{% endhighlight %}
3. 테스트 케이스를 실행한다.
python manage.py test 'app_name' 또는 파이참 컨텍스트 메뉴에서 Run 'Test: ...' 실행

###파이참이 테스트 케이스를 찾지 못하고 No tests were found 메시지가 나온다면
테스트 클래스 내 함수의 이름이 test로 시작하는지 확인해보자. test로 시작하지 않으면 테스트 케이스로 인식하지 못한다.
