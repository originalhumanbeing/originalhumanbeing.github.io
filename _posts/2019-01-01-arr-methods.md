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
	if( e.indexOf('bison') !== -1 ) {
		bisons.push(idx);
	}
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
- **`filter`는 빈 원소를 걸러주는 역할도 한다**


### 배열을 하나로 합치고 싶을 때
#### 0. concat
- 인자로 받는 값, 배열 등을 하나의 배열로 합쳐준다
- **params**: value
- **return**: new Array (인자로 넣었던 배열들, 값들은 영향받지 않는다)

```javascript
let arr1 = ['a', 'b', 'c'];
let arr2 = ['d', 'e'];
arr1.concat(arr2);
// output: ["a", "b", "c", "d", "e"]

arr1.concat([['1', '12']]);
// output: ["a", "b", "c", Array(2)]
// value를 직접 넣어도 된다
// 1차원 이상 배열은 이를 flat화시켜서 합쳐주지 않는다, 배열 내부를 재귀하지 않으므로 주의 
```
- **배열을 복사해서 새 배열을 반환할 때, 참조 복사가 되므로 새 배열의 값을 수정할 경우, 원본 배열의 값도 수정되니 유의**


### arr의 원소들을 단일 값으로 만들어야 할 때
#### 0. reduce
- **덧셈 메소드가 아님에 주의**, 아래 예시를 보면 알겠지만 여러 원소를 가지고 있던 배열을 하나의 값(acc)으로 만드는 역할을 한다
- 그 과정에서 조건에 맞는 값만을 추릴 수도, depth를 줄여나갈 수도 있다
- **params**: function(accumulator(최종 값이 될 인자), currentVal, currentIdx, originalArr), initialVal 
- **return**: accumulator


```javascript
const euros = [29.76, 41.85, 46.5];
const average = euros.reduce((acc, val, idx, arr) => {
  acc += val;
  if( idx === arr.length - 1) { 
    acc = acc / arr.length;
  }
  return acc;
});

average // 39.37
```

```javascript
const euros = [29.76, 41.85, 46.5];
const doubled = euros.reduce((acc, val, idx, arr) => {
  acc.push(val * 2);
  return acc;
}, []);

doubled // [59.52, 83.7, 93]
```

```javascript
const euro = [29.76, 41.85, 46.5];
const above30 = euro.reduce((acc, val, idx, arr) => {
  if (val > 30) {
    acc.push(val);
  }
  return acc;
}, []);

above30 // [ 41.85, 46.5 ]
```

```javascript
const fruitBasket = ['banana', 'cherry', 'orange', 'apple', 'cherry', 'orange', 'apple', 'banana', 'cherry', 'orange', 'fig' ];
const tally = fruitBasket.reduce( (acc, val, idx, arr) => {
	if (acc.hasOwnProperty(val)) {
		acc[val] += 1; 
	} else {
		acc[val] = 1;
	}
	return acc;
} , {})

tally // { banana: 2, cherry: 3, orange: 3, apple: 2, fig: 1 }
```

```javascript
const data = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
function flatten(acc, val, idx, arr) {
	acc.concat(val);
	return acc;
}

data.reduce(flatten, []);
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

```javascript
const data = [
  {a: 'happy', b: 'robin', c: ['blue','green']}, 
  {a: 'tired', b: 'panther', c: ['green','black','orange','blue']}, 
  {a: 'sad', b: 'goldfish', c: ['green','red']}
];
function totalColor(acc, val, idx, arr) {
	if (val.hasOwnProperty('c') && Array.isArray(val['c'])) {
		acc = acc.concat(val['c']);
	}

	return acc;
}

test.reduce(totalColor, []);
// ["blue", "green", "green", "black", "orange", "blue", "green", "red"]
```

```javascript
const data = [
  {a: 'happy', b: 'robin', c: ['blue','green']}, 
  {a: 'tired', b: 'panther', c: ['green','black','orange','blue']}, 
  {a: 'sad', b: 'goldfish', c: ['green','red']}
];
function uniqueColor(acc, val, idx, arr) {
	val['c'].forEach(e => {
		if (acc.indexOf(e) === -1) {
			acc.push(e);
		}
	})
	
	return acc;
}

test.reduce(uniqueColor, []);
// ["blue", "green", "black", "orange", "red"]
```

```javascript
function increment(val) {
	val += 1;
	return val;
}
function decrement(val) {
	val -= 1;
	return val;
}
function double(val) {
	val *= 2;
	return val;
}
const pipeline= [increment, double, decrement];

function reducer(acc, val, idx, arr) {
	return val(acc);
}

pipeline.reduce(reducer, 1);
// 3
```