---
layout: post
title: "자주 쓰지만 계속 까먹는 Object 메소드 정리"
description: "까먹지 말자 쫌"
date: 2019-01-07
tags: [ES6, Object, keys, hasOwnProperty, for..in, TIL]
comments: true
share: true
---

자주 써야하고 실제로 자주 쓰지만 쓰려고 할 때마다 까먹는 Array 메소드를 정리해보고자 한다. 모든 내용은 MDN + 구글링을 참고했고, 노트 정리처럼 공부용 + 개인 위키용으로 작성했다. (혹시 공부한 내용 중 틀린 부분이 있다면 댓글 부탁드립니다.)

### key가 필요할 때

#### 0. keys
- 메소드 이름 그대로 해당 Object의 key를 반환해준다, for..in으로도 key를 활용할 수 있긴 하지만 key를 배열로 반환해준다는 것 자체가 큰 차이다.
- **params**: 해당 Object
- **return**: [key, ...]

```javascript
let time = {
    indicator: ['am', 'pm'],
    hour: null,
    min: null,
    sec: null
}

Object.keys(time);
// output
// ["indicator", "hour", "min", "secs"]
// Object.keys(time).forEach(el => el...)
// 이런 식으로 key를 바로 활용할 수 있다!
```