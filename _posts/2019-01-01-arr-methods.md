---
layout: post
title: "자주 쓰지만 계속 까먹는 Array 메소드 정리"
description: "까먹지 말자 쫌"
date: 2019-01-01
tags: [Array, splice, slice, join, concat, forEach, map, reduce, filter, sort, indexOf, TIL]
comments: true
share: true
---

자주 써야하고 실제로 자주 쓰지만 쓰려고 할 때마다 까먹는 Array 메소드를 정리해보고자 한다. 모든 내용은 MDN + 구글링을 참고했고, 노트 정리처럼 공부용 + 개인 위키용으로 작성했다. (혹시 공부한 내용 중 틀린 부분이 있다면 댓글 부탁드립니다.)

### 순회가 목적일 때
배열을 순회해야 할 때가 정말 많은데, 배열 + 순회 키워드가 합쳐지기만 하면 생각도 안 하고 `for..of`를 바로 사용하는 나를 발견했다. 하지만 아래와 같은 좋은 메소드들도 있다.

#### 0. forEach
- 주어진 배열의 원소를 한 번씩 순회하면서 callback 함수에 정의된 대로 연산 가능
- **params**: el, idx, callback
- **return**: undefined

```javascript
let arr = [1, 2, 3],
    newArr = [];

arr.forEach((el, idx) => {
	el += 1;
	newArr.push(el);
})
```
```javascript
let arr = [1, 2, 3];

arr.forEach((el, idx) => {
	return arr[idx] = el += 1;
})
```
- **`forEach`는 새로운 배열을 반환하거나 기존 배열을 바꾸지도 않으므로 연산된 값을 새 배열에 `push`하거나 `arr[idx]`에 넣어주어야 한다**

#### 1. map
- forEach와 마찬가지로 배열의 원소를 순회한다. 새로운 배열을 반환하는 것이 큰 차이.
- **params**: el, idx, arr
- **return**: new Array

```javascript
let arr = ['example.md', 'test.md', 'README.md'];

let newArr = arr.map(el => {
	return el.split('.')[0];
})
```
- **`map`이 `return`해주는 새 `arr`을 잘 할당해주기만 하면 된다**


### 특정 원소가 존재하는지 확인하고 싶을 때
#### 0. indexOf
- 메소드의 이름 그대로 특정 원소의(of) 인덱스를 알려줌
- **params**: value
- **return**: 넘겨준 value를 가진 el의 idx (중복으로 있을 경우, 가장 먼저 찾은 idx)/ 없을 경우에는 -1

```javascript
let beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];
let bisons = [];

beasts.forEach((e, idx) => {
	if(e === 'bison') bisons.push(idx);
});
// bisons = [1, 4];
```


### 필터링이 필요할 때
#### 0. filter
- 메소드의 이름 그대로 callback 함수의 조건에 일치하는(true인) 원소만 모아 새 배열을 반환해줌
- **params**: callback(el, idx, 해당 arr)
- **return**: new Array

```javascript
let favMovie = { id: 3, title: 'rapunzel' };
let movies = [
	{ id: 0, 
	title: 'aloha'
	},
	{ id: 1, 
	title: 'bad moms'
	},
	{ id: 2, 
	title: 'dumpling'
	},
	{ id: 3, 
	title: 'rapunzel'
	},
	{ id: 4, 
	title: 'mummy'
	}];

const result = movies.filter(el => {
	return el.id === favMovie.id;
})

// output
// result = [{ id: 3, title: 'rapunzel' }];
```
```javascript
let test = [1, 2, , , , 4, 5, , , , , 8];

const result = test.filter(el => el);
// output
// result = [1, 2, 4, 5, 8];
// test에 0이 포함되어 있을 경우, 0도 result에 포함되지 않는다
```
- **`filter`는 빈 원소를 걸러주는 역활도 한다**