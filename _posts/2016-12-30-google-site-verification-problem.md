---
layout: post
title: 워드프레스 – 구글 검색 콘솔 연결 시, 사용자 삭제가 안될 경우 해결책(google-site-verification 문제)
tags: wordpress how-to
comments: true
---
## 증상 및 원인
워드프레스에서 애드센스(Adsense)와 같은 구글 플러그인을 사용하다 실수로 다른 구글 계정이 사이트 소유자로 등록되어 버리는 경우가 있다. 이렇게 되면 구글 검색 콘솔(Google Search Console)에서 사이트 소유자가 2개로 표시된다. 이때 잘못 등록된 사이트 소유자를 삭제하려고 하면 다음의 메시지가 나타난다.

>“xxx@gmail.com”에 대한 확인을 취소하시겠습니까?
메타 태그, 확인 파일 또는 DNS 레코드가 연결된 이메일 주소입니다. 이 관리자에 대한 확인을 취소하려면 사이트에서 다음을 삭제해야 합니다.

{% highlight html %}
<meta name=”google-site-verification” content=’YOUR-USER-KEY”/>
{% endhighlight %}

취소 버튼이 2개 있는 것도 이상하지만 어느 취소 버튼을 눌러도 사용자 삭제가 안된다. 현재 사이트에 사이트 소유자 인증 시 사용했던 메타 태그가 입력되어 있기 때문이라는데 아무리 찾아봐도 이 메타 태그가 입력했던 곳도 없고, 수정할 수 있는 곳도 없다. 헤더에 메타 태그를 수정할 수 있는 여러 기능(워드프레스 기본, 젯팩, 헤더 삽입 플러그인 등)을 모두 다 살펴봐도 새로 입력하는 것만 가능하고 기존 입력된 메타 태그를 수정할 수 있는 방법이 없었다.
[![user_delete_on_google_search_console_snapshot.png](https://s26.postimg.org/sw0a3axdl/user_delete_on_google_search_console_snapshot.png)](https://postimg.org/image/5hsardfg5/)

[![user_delete_on_google_search_console_snapshot3.png](https://s26.postimg.org/tmt092zqx/user_delete_on_google_search_console_snapshot3.png)](https://postimg.org/image/iagerar1x/)

‘메타 태그’때문에 위와 같은 메시지가 나타나며 사용자 삭제가 되지 않는다. 

## 해결 방법
그래서 SSH로 접속한 다음 워드프레스 설치 파일에서 메타 태그 생성 부분을 살펴보다가 갑자기 이런 생각이 떠올랐다. “만약 의심가는 플러그인(여기서는 애드센스)을 비활성화(삭제 말고) 한 다음 해보면 어떨까? 결론은 성공.

1. 워드프레스 접속
2. 설정 > 플러그인 > 설치된 플러그인
3. 애드센스 선택 > 비활성화
4. 구글 검색 콘솔(Google Search Console) 접속 > 사용자 삭제
5. 끝.

삭제가 된다. 이후 다시 애드센스 플러그인을 활성화하니 기존 설정된 광고 위치 등에 영향없이 정상적으로 활성화되었다. 이렇게 쉬운 방법을 두고 강제로 구글에서 삽입한 메타태그를 지우려고 했다니. 같은 문제를 호소하는 글이 꽤 있었지만 대부분 외국 사이트의 경험이기도 하거나와, 궁금해하는 것과 좀 동떨어진 케이스. 그나마 [이 글](https://generatepress.com/forums/topic/no-google-meta-tags-in-header-php-help/ "이 글")이 내가 겪었던 문제와 가장 흡사했다. 구글에서도 [사용자, 소유자 등의 삭제 안내 페이지](https://support.google.com/webmasters/answer/2453966?hl=en&vid=0-528448905133-1483064785120&visit_id=1-636186722008773488-373412854&rd=1 "사용자, 소유자 등의 삭제 안내 페이지")를 제공했지만 내 케이스의 경우 전혀 도움이 안됐다.

비슷한 문제로 어려움을 겪는 분이 있다면 이 글은 큰 도움이 될거라 생각한다.