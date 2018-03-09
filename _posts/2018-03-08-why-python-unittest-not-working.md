---
layout: post
title: 파이썬 유닛테스트를 실행했을 때 아무 결과가 나오지 않는다면
tags: python
comments: true
---
  
파이썬 유닛(단위)테스트 실행 시 정상적으로 unittest 모듈도 임포트하고 TestCase 클래스도 상속받아서 잘 구현했는데 아무런 결과가 나오지 않을 때가 있다. 
  
---
  
## 원인
unittest 모듈을 실행해주는 코드 또는 명령이 없기 때문이다.
  
## 해결방법
아래 2가지 경우에 해당하는지 확인해보자. 둘 중 한 가지만 만족해도 테스트 결과가 나온다.
  
### 선택-1
테스트 소스 제일 아래 아래와 같은 코드를 넣어주었는가? 터미널에서 이 함수를 바로 불러 실행하려면 unittest.main()을 실행시켜 줘야 한다.
~~~
if __name__ == '__main__':
    unittest.main()
~~~
  
### 선택-2
**선택1**과 같은 코드를 넣지 않아도 터미널 명령으로 테스트 코드를 실행할 수 있다.  
-m 옵션을 python 실행 시 주면 뒤에 나오는 모듈을 임포트 한 후 스크립트를 실행한다.  
~~~
python -m unittest [option] [test.py]
~~~
  
test.py에 테스트 코드가 있는 파일명을 넣어주면 된다. option 부분에 -v 를 넣으면 더 자세하게 나온다. 더 많은 옵션은 아래를 참고하자.
  
## uniitest 모듈 사용법과 옵션
~~~
usage: python -m unittest [-h] [-v] [-q] [--locals] [-f] [-c] [-b]
                          [tests [tests ...]]

positional arguments:
  tests           a list of any number of test modules, classes and test
                  methods.

optional arguments:
  -h, --help      show this help message and exit
  -v, --verbose   Verbose output
  -q, --quiet     Quiet output
  --locals        Show local variables in tracebacks
  -f, --failfast  Stop on first fail or error
  -c, --catch     Catch Ctrl-C and display results so far
  -b, --buffer    Buffer stdout and stderr during tests

Examples:
  python -m unittest test_module               - run tests from test_module
  python -m unittest module.TestClass          - run tests from module.TestClass
  python -m unittest module.Class.test_method  - run specified test method
  python -m unittest path/to/test_file.py      - run tests from test_file.py

usage: python -m unittest discover [-h] [-v] [-q] [--locals] [-f] [-c] [-b]
                                   [-s START] [-p PATTERN] [-t TOP]

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Verbose output
  -q, --quiet           Quiet output
  --locals              Show local variables in tracebacks
  -f, --failfast        Stop on first fail or error
  -c, --catch           Catch Ctrl-C and display results so far
  -b, --buffer          Buffer stdout and stderr during tests
  -s START, --start-directory START
                        Directory to start discovery ('.' default)
  -p PATTERN, --pattern PATTERN
                        Pattern to match tests ('test*.py' default)
  -t TOP, --top-level-directory TOP
                        Top level directory of project (defaults to start
                        directory)

For test discovery all test modules must be importable from the top level
directory of the project.
~~~