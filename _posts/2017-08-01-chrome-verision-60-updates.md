---
layout: post
title: 이제 간단한 모바일 테스트는 크롬 하나면 OK! 
tags: chrome, useful
comments: true
---

## 눈에 띄는 업데이트 3가지   
1. 모바일 테스트 기능   
2. 웹앱 품질 개선 기능(Lighthouse)   
3. 서드 파티 배지(badge) 표시 기능   
   
----
   
## 3가지 기능 소개   
    
#### 1. 모바일 테스트 기능  
기존에는 스마트폰이나 테블릿 등 화면 테스트를 위해서는 별도의 프로그램이나 웹서비스를 사용해서 확인을 해야 했는데 이제는 그럴 필요가 없어졌다. 크롬 브라우저가 버전 60으로 업데이트 되면서 이 기능을 내장했기 때문이다. 간단하게 개발자 모드(Cmd+Option+i)를 켜기만 하면 아래와 같은 화면이 나온다. 4K(UHD) 화면부터 작은 사이즈의 스마트폰, 특정 스마트폰 기종에 따른 크기를 아주 손쉽게 바꿔가며 테스트 해볼 수 있다. 아래 스크린샷의 **상단부에 블럭(?)처럼 보이는 버튼을 누르면 화면 사이즈가 즉시 변경**되기 때문에 직관적으로 사용할 수 있다.
    
[![chrome_4k.png](https://s26.postimg.org/emj3zwa9l/chrome_4k.png)](https://postimg.org/image/6h121qm0l/)    
    
[![chrome_laptop.png](https://s26.postimg.org/sqez8ahh5/chrome_laptop.png)](https://postimg.org/image/tsp5qu0ad/)    
    
[![chrome_mobile.png](https://s26.postimg.org/xqcfg8n3t/chrome_mobile.png)](https://postimg.org/image/3lnyuvi0l/)    
    
[![chrome_responsive.png](https://s26.postimg.org/60b91k8vt/chrome_responsive.png)](https://postimg.org/image/wldrx4b91/)    
    
----
    

#### 2. 웹앱 품질 개선 기능 (Lighthouse)
Lighthouse라는 웹앱 품질 개선 기능이 기본으로 탑재됐다. 개발자 모드 실행 후 Elements, Console, Source 등의 탭이 있는 곳을 보면 **Audits**가 새로 생긴 것을 볼 수 있다. 보다 손쉬운 테스트를 위해 [크롬 확장프로그램](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk)도 제공하고 있다. 이 기능에 대한 더 자세한 정보는 [여기](https://developers.google.com/web/tools/lighthouse/)를 참고하면 된다.    
    
[![chrome_audit_screen.png](https://s26.postimg.org/ccqebeby1/chrome_audit_screen.png)](https://postimg.org/image/p44khwlpx/)    
    
[![chrome_audit_check.png](https://s26.postimg.org/mya9nei9l/chrome_audit_check.png)](https://postimg.org/image/d0z8ucanp/)    
    
[![chrome_audits_graph.png](https://s26.postimg.org/6kuvhzri1/chrome_audits_graph.png)](https://postimg.org/image/e0u53sf79/)    
    
[![chrome_audits_ex.png](https://s26.postimg.org/tovcakut5/chrome_audits_ex.png)](https://postimg.org/image/5xvysgulx/)    
    
----    
    
#### 3. 서드 파티 배지(badge) 표시 기능
크롬 개발자 도구에 Network 탭을 눌렀을 때 외부 서드파티의 요청에는 아래와 같이 배지(badge)가 눈에 띄게 표시된다. 이 기능에 대한 더 자세한 정보는 [여기](https://developers.google.com/web/updates/2017/05/devtools-release-notes#badges)를 참고.

[![chrome_3rd_badge.png](https://s26.postimg.org/uyt5pmol5/chrome_3rd_badge.png)](https://postimg.org/image/ent1tbc39/)    
    
