---
layout: post
title: RStudio 설치 및 오류 해결 방법
tags: pandas
comments: true
---
      
## 설치 순서
RStudio를 설치해서 쓰기 위해서는 R을 먼저 설치해야 한다.    
       
1. R 설치 링크: [R 공식사이트](https://www.r-project.org/), [전세계 다운로드 페이지](http://cran.r-project.org/mirrors.html), [한국 다운로드 페이지](https://cran.seoul.go.kr/bin/macosx/), [즉시 다운로드 링크](https://cran.seoul.go.kr/bin/macosx/R-3.5.1.pkg)
2. RStudio 설치 링크: [RStudio 공식사이트](https://www.rstudio.com), [다운로드 페이지](https://www.rstudio.com/products/rstudio/download/#download)
   
---

## 오류 해결 방법
**증상**     
> R Not Found
> Unable to locate R binary by scanning standard locations
      
**원인**     
R이 설치되어 않은 상태에서 RStudio를 섪치한다음 실행하는 경우 발생한다.
      
**해결 방법**     
위 링크를 통해 R을 설치한다.
        
---
    
**증상**     
> WARNING: You're using a non-UTF8 locale, therefore only ASCII characters will work.
> Please read R for Mac OS X FAQ (see Help) section 9 and adjust your system preferences accordingly.
    
**원인**     
R이 구동되는 로케일 설정에 문제가 있다.
    
**해결 방법**     
터미널에서 아래 명령을 실행 후 R을 재시작한다.
   
~~~
defaults write org.R-project.R force.LANG en_US.UTF-8
~~~