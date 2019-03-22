---
layout: post
title: 몽구스(mongoose)에서 populate vs execPopulate 차이점
tags: mongoose
comments: true
---

## 요약
populate()는 인자로 콜백함수를 줘야지만 실행된다. execPopulate()는 콜백함수를 지정하면 콜백으로, 콜백을 지정하지 않으면 프라미스(promise)를 리턴한다.
    
---

## populate()
[populate](https://mongoosejs.com/docs/api.html#document_Document-populate)는 완료가 되었을 때 콜백을 실행한다. 콜백이 전달되지 않으면 실행되지 않는다. 아래는 공식 사이트의 예제다.

```
doc
.populate('company')
.populate({
  path: 'notes',
  match: /airline/,
  select: 'text',
  model: 'modelName'
  options: opts
}, function (err, user) {
  assert(doc._id === user._id) // the document itself is passed
})

// summary
doc.populate(path)                   // 실행되지 않음
doc.populate(options);               // 실행되지 않음
doc.populate(path, callback)         // 실행됨
doc.populate(options, callback);     // 실행됨
doc.populate(callback);              // 실행됨
doc.populate(options).execPopulate() // 실행됨, 프라미스를 리턴함.
```
      
## execPopulate()
[execPopulate](https://mongoosejs.com/docs/api.html#document_Document-execPopulate)는 콜백이 옵션이다. 콜백함수를 인자로 전달하면 프라미스(promise)를 리턴하지 않고 콜백 함수를 전달한다. 인자로 아무것도 전달하지 않으면 프라미스를 리턴한다. 아래는 공식 사이트의 예제.

```
var promise = doc.
  populate('company').
  populate({
    path: 'notes',
    match: /airline/,
    select: 'text',
    model: 'modelName'
    options: opts
  }).
  execPopulate();

// summary
doc.execPopulate().then(resolve, reject);
```