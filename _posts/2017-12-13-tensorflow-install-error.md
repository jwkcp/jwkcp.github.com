---
layout: post
title: 텐서플로우(Tensorflow) 설치 시 오류가 발생하는 경우 제일 먼저 확인해야 할 사항
tags: tensorflow, how-to 
comments: true    
---

텐서플로우를 처음 설치하다 보면 아래와 같이 에러 메시지가 나오면서 설치가 안되는 경우가 있다.
    
### 에러메시지  
"Could not find a version that satisfies the requirement tensorflow (from versions: ) No matching distribution found for tensorflow"  

### 해결방법  
현재 설치된 파이썬 버전이 텐서플로우에서 요구하는 것과 같은지 확인해야 한다.  
구글이 기본적으로 제공하는 텐서플로우 설치 파일은 64bit(x64)이기 때문에 32bit(x86) 파이썬을 설치했을 경우에 이 에러메시지를 만날 것이다. 
"어? 나는 32bit를 설치한 적이 없는데.."라는 생각이 든다면 무심코 [파이썬 사이트](https://www.python.org)에서 기본으로 제공하는 대표 다운로드 링크를 눌렀을 것이다. 이 경우 32bit 버전이 설치된다. 64bit 버전의 파이썬을 설치하려면 Downloads 페이지에서 원하는 버전의 파이썬 중 '**64**' 라는 문구가 포함되어 있는 64bit 버전을 다운로드하여 설치하면 된다.
