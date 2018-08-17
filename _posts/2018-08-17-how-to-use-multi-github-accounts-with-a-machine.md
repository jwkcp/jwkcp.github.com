---
layout: post
title: 한 대의 맥에서 여러 개의 깃허브(github) 계정 사용하는 방법
tags: bootstrap
comments: true
---

# 개요
깃허브 계정이 여러 개 있고 이것을 터미널에서 사용하다보면 Permission error, User Denied 등 많은 에러를 만나게 된다. 여기서는 맥을 기준으로 ssh key를 이용해 여러 개의 깃허브 계정을 사용하는 방법을 알아본다.
      
# 문제
github에 pull, push를 처음하면 해당 계정 권한 확인을 하는 프롬프트가 뜨고, 입력된 계정이 맥 키체인에 보관되면서 다음에 다시 입력하지 않아도 바로 사용할 수 있는 상태가 된다. 그런데 깃허브 계정을 하나 더 만들고 저장소 동기화를 하려고 하면 이전에 기억된 계정정보 때문에 계속 오류가 발생한다. 이때 ssh key를 사용하여 각 저장소를 관리하면 편리하게 깃허브를 사용할 수 있다.
      
# 자주 사용하는 명령

**현재 원격 저장소 주소 보기**  
~~~
git remote -v
~~~
    
**현재 저장소 설정 전체 조회**  
(저장소 루트 폴더에서 .git/config 파일에 관련 설정이 있다.)
~~~
git config --list
~~~
     
# 기존에 입력된 키체인 정보 삭제
맥에 시스템 도구인 '키체인 접근'에서 github로 검색 후 삭제하면 터미널에 기억된 키체인 정보를 지울 수 있다. 우리는 ssh key를 이용할 것이므로 기존에 입력해 사용하던 키체인 정보를 '키체인 접근'에 가서 삭제해주자.
     
# SSH key 생성 및 설정
키 생성 방법은 [깃허브 공식 문서](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)에서 아주 자세히 단계별로 잘 설명하고 있다. 맥은 ~/.ssh 폴더에서 키를 관리하므로 여기로 이동 후 위 설명대로 키를 생성해주자. 참고로 깃허브 계정 하나 당 하나의 키만 등록이 가능하다. 동일한 키를 서로 다른 깃허브 계정에 등록할 수 없다는 말이다. 그러니 2개 깃허브 계정을 사용할거라면 키도 서로 다른 이메일과 이름으로 2개 생성하면 된다.
      
예를 들어, foo와 bar 계정 두 개를 위해 키를 생성한다고 하면 아래와 같고 foo 부분을 bar로 바꿔 반복하면 키를 2개 만들 수 있습니다.

### [키 생성하기](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
1단계: 키 생성
~~~
ssh-keygen -t rsa -b 4096 -C "foo@gmail.com"
~~~

2단계: 키생성    
~~~
Generating public/private rsa key pair
(공개키와 비밀키 쌍을 생성하고 있습니다.)
~~~
   
3단계: 원하는 키 이름 입력    
~~~
Enter a file in which to save the key (/Users/컴터계정명/.ssh/id_rsa): id_rsa_foo
(저장하려는 키 파일명을 입력하세요. 그냥 엔터를 치면 /Users/컴터계정명/.ssh/id_rsa에 생성함.)
~~~
   
4단계: 비밀번호 입력 (비밀번호 걸고 싶은 사람만 입력하시면 됩니다.)    
~~~
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
(걸고 싶은 비밀번호를 입력하세요. 그냥 엔터를 치면 비밀번호가 없는 상태로 설정됩니다.)
~~~

---    

### [생성된 키를 맥에 등록하기](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
1단계: ssh 에이전트 데몬으로 동작시키기
~~~
eval "$(ssh-agent -s)"
~~~
     
2단계: ssh 키 추가하기
~~~
ssh-add -K ~/.ssh/id_rsa_foo
~~~

---

여기까지 했다면 .ssh 폴더 안에 아래와 같은 파일이 생성되었을 것입니다.   
- id_rsa
- id_rsa_foo.pub
- id_rsa
- id_rsa_bar.pub
     
여기서 .pub 파일이 공개키이고 다른 하나(이름이 같지만 확장자가 없는)는 비밀키입니다. 공개키는 다른 사람에게 보여줘도 되는 키로 깃허브 계정에 이 키를 복사해서 등록합니다. 비밀키는 나만 알아야 하는 키입니다. 다른 사람에게 알려주거나 보여주거나 복사해주면 안됩니다.   
       
### [깃허브에 공개키 등록하기](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
오른쪽 위 깃허브 로그인 후 나오는 계정 이미지를 누르고 설정(settings)에 들어가면 좌측 사이드바 메뉴에 'SSH and GPG keys'라는 메뉴가 있습니다. 이걸 누릅니다. 그러면 'SSH keys'라는 항목이 오른편에 나오고 'New SSH key'라는 버튼이 보입니다. 이 버튼을 누르면 우리가 만든 공개키를 등록할 수 있는 화면이 나옵니다. 타이틀(Title)은 암거나 내가 알아보기 좋은 이름으로 지어 줍니다. 그 다음 아래 키(Key) 부분에 위에서 .pub 으로 끝나는 파일의 내용을 복사해서 넣어줍니다. 파일을 내용을 복사하는 방법은 텍스트 편집기로 열어서 복사해도 되고, 터미널에서 하고 싶다면 아래 명령을 사용해도 됩니다.    
    
~~~
pbcopy < ~/.ssh/id_rsa_foo.pub
~~~
      
저장 버튼을 누르면 내가 지정한 이름과 함께 키가 등록된 걸 볼 수 있습니다. 로컬 머신에 등록한 ssh키와 깃허브 저장소에 등록한 키가 일치하는지, 잘 등록이 되었는지 알고 싶다면 등록 페이지에 표시된 Fingerprint를 비교해보면 됩니다. 로컬 머신에서 등록된 키의 Fingerprint를 보는 명령은 아래와 같습니다.    
    
~~~
ssh-add -l
~~~

---

### [마지막으로 ssh 설정파일은 만듭니다.](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)
ssh의 설정 파일은 config 이라는 이름을 가진 파일입니다. ~/.ssh 폴더에 있습니다. 하지만 대부분 이 과정을 따라하시는 분들이라면 이 파일이 없으실 겁니다. 새로 생성해주시면 됩니다. foo와 bar 깃허브 계정, 그리고 각각의 ssh 키를 예로 들고 있었기 때문에 거기에 맞춰 만들어 보면 아래와 같습니다.    

~~~
Host github.com-foo
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_foo
  User foo

Host github.com-bar
  HostName github.com
  IdentityFile ~/.ssh/id_rsa_bar
  User bar
~~~
     
위에 Host 뒤의 github.com-foo나 github.com-bar는 원하는 문자열을 입력하시면 됩니다. (이 값은 저장소 주소 불러올 때 쓸 것입니다. 일종의 키인 셈이죠.) User는 깃허브 계정 사용자명을 입력하시면 됩니다.
    
이제 깃허브 저장소로 가서 'Clone or download'를 눌러 주소를 한 번 살펴보세요. https 주소도 있고, git 주소도 있습니다.
예를 들어, 이런 식이지요.   
    
- git 방식 주소: git@github.com:foo/foo_site.git
- https 방식 주소: https://github.com/foo/foo_site.git
    
ssh 키로 깃허브를 이용할 때는 git 방식 주소를 씁니다. 직전에 git의 config 파일 만든 거 생각 나시지요? 거기에 맞게 git 방식 주소를 바꾸면 이렇게 됩니다.
    
~~~
git@github.com-foo:foo/foo_site.git
~~~
   
@ 다음 부분이 github.com이 github.com-foo로 바뀌었습니다. 이렇게 하면 git의 config에서 HostName과 IdentityFile과 User를 찾아 자동으로 매핑해줍니다. 이 주소를 원격(remote) 저장소 주소로 쓰면 됩니다. 원격 저장소 주소는 어떻게 설정하냐구요? 위에서 잠깐 알아봤었는데요. .git/config 파일 생각나시죠? 여기에서 remote "origin" 이런 항목에 url을 바꿔주면 됩니다. 또 아래와 같은 명령으로도 저장소 주소를 바꿀 수도 있습니다.   

새로운 원격 저장소 주소 추가할 때    
~~~
git remote add origin git@github.com-foo:foo/foo_site.git
~~~
      
기존 원격 저장소 주소 바꿀 때    
~~~
git remote set-url origin git@github.com-foo:foo/foo_site.git
~~~

