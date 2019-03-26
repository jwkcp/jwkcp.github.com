---
layout: post
title: 리액트 네이티브(React Native) 개발 시 엑스포(Expo) 실행하면 file watchers 에러가 발생하는 경우
tags: react-native
comments: true
---

##문제
파일 감시 수가 제한치에 도달하면 아래와 같은 에러가 발생한다.
```
$ expo start
Starting project at /home/myproject
Expo DevTools is running at http://localhost:19002
Opening DevTools in the browser... (press shift-d to disable)
Starting Metro Bundler on port 19001.
internal/fs/watchers.js:173
    throw error;
    ^

Error: ENOSPC: System limit for number of file watchers reached, watch '/home/myproject/node_modules/babel-code-frame'
    at FSWatcher.start (internal/fs/watchers.js:165:26)
    at Object.watch (fs.js:1253:11)
    at NodeWatcher.watchdir (/home/myproject/node_modules/sane/src/node_watcher.js:175:20)
    at Walker.<anonymous> (/home/myproject/node_modules/sane/src/common.js:116:12)
    at Walker.emit (events.js:189:13)
    at /home/myproject/node_modules/walker/lib/walker.js:69:16
    at go$readdir$cb (/home/myproject/node_modules/graceful-fs/graceful-fs.js:162:14)
    at FSReqWrap.args [as oncomplete] (fs.js:140:20)
```
    
##해결방법
현재 file watchers 최대치가 몇으로 되어있는지 확인하려면 ```cat /proc/sys/fs/inotify/max_user_watches```를 터미털에 입력하면 된다. 최대 사이즈를 늘리려면 ```/etc/sysctl.conf```파일을 열어 제일 마지막에 ```fs.inotify.max_user_watches=524288```를 추가해준다. 다시 실행해보면 에러가 발생하지 않는다.
      
