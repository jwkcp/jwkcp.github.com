---
layout: post
title: Django, 똑똑한 테스트 케이스를 만드는 3가지 방법
tags: django tdd
comments: true
---
- 테스트 클래스(TestClass) 작성 시, 뷰와 모델을 분리해 작성해라.
{% highlight python %}
class QuestionViewTests(TestCase):
...
class QuestionMethodTests(TestCase):
{% endhighlight %}
- 테스트 조건 별로 테스트 메서드를 나눠라.

- 그 테스트의 성격을 알 수 있도록 테스트 메서드의 이름을 지어라.
{% highlight python %}
def test_index_view_with_future_question_and_past_question(self):
...
def test_index_view_with_two_past_questions(self):
...
def test_index_view_with_a_future_question(self):
{% endhighlight %}

---

>**As long as your tests are sensibly arranged, they won’t become unmanageable. Good rules-of-thumb include having:**
>
> - a separate TestClass for each model or view.
> - a separate test method for each set of conditions you want to test
> - test method names that describe their function

원문링크: [Django Tutorial](https://docs.djangoproject.com/en/1.10/intro/tutorial05/ "Django Tutorial")