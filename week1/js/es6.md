# ES6를 소개합니다

이 문서는 ES5에서 달라진 점을 위주로 ES6 문법에 다룹니다.


## 서론

### ES?
ES는 [ECMAScript](https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)의 약자입니다. Javascript는 ECMAScript에서 파생되어 나온 언어 중 하나이며, ECMAScript와의 완벽한 호환성과 확장된 기능을 제공합니다. ECMAScript의 표준을 공부하는 것은 곧
Javascript의 표준 문법을 공부하는 것과 같으며, 대다수 브라우저들이 ES표준을 지원하기 위해서 많은!?!?노력들을 하고 있습니다.

### 브라우저의 ES 지원
크롬, 사파리, IE, Firefox등 많은 브라우저들은 **각자의 Javascript 컴파일 엔진** 을 만들어서 각자의 브라우저들을 구동시키고 있습니다. 그렇기 때문에 모든 브라우저에서 지원하는 문법의 형태가 모두 다르고, 아직도 ES6문법을 100%지원하는 브라우저는 몇개 없습니다. 때문에 개발중에도 해당 Javascript함수가 브라우저에서 지원하는지 항상 확인해야하며, 직접 확인하기 귀찮다면 [babel](https://babeljs.io/)과 같은 트랜스파일러를 사용하는 것을 추천합니다.  

브라우저의 ES6지원을 보여주는 표입니다.  
http://kangax.github.io/compat-table/es6/

ES6 스타일의 코드 작성  
es5 to es6 : https://lebab.io/try-it

## Type
### 1. 이미 선언 하셨는데요?
var 는 상당히 너그러운 친구입니다.
```js
var foo = 'bar1';
var foo = 'bar2';
```

같은 이름의 변수를 두번 선언하고 있습니다. foo라는 변수에는 ‘bar1′ 이라는 문자열이 할당되었다가 다음 선언에서 ‘bar2’라는 변수가 할당됩니다. 별다른 에러도 발생시키지 않습니다.

하지만 let과 const는 엄격합니다.
```js
let foo = 'bar1';
let foo = 'bar2';
// ERROR: Uncaught SyntaxError: Identifier 'foo' has already been declared
```
앞서 선언한 변수를 다시 선언하게 되면 칼 같은 오류를 발생시킵니다. 규모가 큰 코드에서 버그를 방지 할 수 있는 매우 바람직한 특징입니다.

### 2. 난 그때 거기 없었어요

```js
console.log(foo);
// Error: Uncaught ReferenceError: foo is not defined
```

Hoisting(호이스팅)
```js
console.log(foo); // undefined
var foo;
```
위의 구문은 실제로...
```js
var foo;
console.log(foo); // undefined
```
위와 같이 컴파일이 됩니다. 내가 작성한대로 컴파일이 되지 않다니... 어떤 치명적인 오류를 유발할지 모르는 심각한 설계 오류입니다.

**let**, **const** 의 호이스팅 방지
```js
console.log(foo);
// Error: Uncaught ReferenceError: foo is not defined
let foo; // const도 동일
```
### 3. 이 블록의 변수는 나야!
변수의 유효 범위에 대한 부분입니다. var의 경우에는 Function-scope 라고 합니다. 유효 범위가 함수 단위라는 이야기죠.
```js
//es5
var foo = 'bar1';
cosole.log(foo); // bar1

if (true) {
  var foo = 'bar2';
  console.log(foo); // bar2
}

console.log(foo); // bar2
```
위 코드가 하나의 함수 구문 안에 존재한다고 가정했을 때 if문 밖의 변수 foo와 if문 안의 변수 foo는 동일한 변수가 됩니다. 중복 선언을 했지만 앞서 말한바와 같이 별다른 에러를 발생시키지 않고, 값마저 ‘bar2’로 변경해 버렸습니다.

하지만 let과 const Block-scope 라고 합니다. 유효 범위가 블록, 즉 {}로 감싸지 범위라는 이야기 입니다.
```js
// es6
let foo = 'bar1';
console.log(foo); // bar1

if (true) {
  let foo = 'bar2';
  console.log(foo) // bar2
}

console.log(foo); // bar1
```
### 4. let과 const는 적절한 관계

그럼 let과 const는 무슨 차이가 있는 걸까요. 일단 이름으로만 봐서는 const는 상수 선언으로 보이는데 말입니다. 실제로 원시형(Primitives type: string, number, boolean, null, undefined)에서 const는 상수로 동작합니다. 따라서 const 로 선언되면 값을 재할당 할 경우 에러가 발생합니다(당연하지만 초기값을 설정하지 않아도 에러가 발생합니다)
```js
const foo = 0;
foo = 1;
// Error: Uncaught TypeError: Assignment to constant variable.
```
따라서 단순형의 경우 값의 변경이 있는 경우에는 let으로, 상수로 사용하는 경우에는 const로 선언하는 것이 바람직하겠습니다.

하지만, 참조형(Complex type: array, object, function)의 경우 결론부터 말씀드리면 **const 로 선언하는 것이 바람직합니다.** 참조형은 const로 선언하더라도 멤버값을 조작하는 것이 가능합니다.
```js
const foo = [0, 1];
const bar = foo;

foo.push(2);
bar[0] = 10;

console.log(foo, bar)
// [10, 1, 2] [10, 1, 2]
```
### 5. 결론
– ES6 에서는 var는 지양하고 가급적 let과 const를 사용하자  
– 원시형에서 변수는 let, 상수는 const로 선언한다  
– 참조형은 const로 선언한다  

## Loop

초창기 JS에서 Loop를 구현하는 방법. C 같죠?
```js
for (var index = 0; index < myArray.length; index++) {
  console.log(myArray[index]);
}
```

ES5의 **forEach** loop, 지금도 많이 쓰는 방식입니다.
```js
myArray.forEach(function (value) {
  console.log(value);
});
```
그러나 break나 return 구문을 사용할 수 없는 치명적인 단점 존재합니다.

**for-in** loop, 실제 상황에서는 사용하지 마세요
```js
for (var index in myArray) {    
  console.log(myArray[index]);
}
```
for–in 루프는 몇가지 이유로 나쁜 방법입니다.

- 만약 ```myArray=['1','2','3']```이라면 ```index```에 할당되는 값들은 ```"0"```, ```"1"```, ```"2"```과 같은 문자열입니다. for문 내에서 연산이 이루어 진다면, ```"2" + 1 == "21"```과 같은 결과를 초래할 수 있습니다.
- 루프 구문이 배열 요소들만을 순회하지 않습니다. 대신 누군가에 의해 추가된 [확장속성(expando)](https://developer.mozilla.org/en-US/docs/Glossary/Expando)들도 순회합니다. 예를 들어, 배열이 myArray.name이라는 속성을 가지고 있다면, 이 루프는 배열 요소들 말고도 index == "name" 속성을 대상으로 한번 더 실행될 것입니다. 뿐만 아니라 배열의 프로토타입 체인(prototype chain)도 순회할 것입니다.
- 가장 당혹스러운 것은, 어떤 환경에서는 이 루프의 순회 순서가 무작위라는 점입니다.

간단히 말해, for–in 구문은 일반 Object의 문자열 키(key)를 순회하기 위해 만들어진 문법입니다. Array를 다루는데는 그다지 유용하지 않습니다.

**for-of 루프** 는 위의 모든 단점을 커버할 수 있습니다.
```js
for (let value of myArray) {
  console.log(value);
}
```
- 문법적으로 가장 간결하고, 직접적입니다.
- 이 구문은 for–in 구문의 모든 단점들을 커버합니다.
- forEach() 구문과 달리, break, continue, 그리고 return 구문과 함께 사용할 수 있습니다.


## Class
기존 JavaScript는 Class 기반의 OOP를 지양했는데, 이를 포기하고 다양한 클래스 기능들을 제공하기 시작했습니다.

```js
// ES6
class User {
  //생성자
  constructor(name) {
	  this._name = name;
  }

  say() {
	  return 'My name is ' + this._name;
  }
}


// 상속
class Admin extends User {

  // override
  say() {
	return '[Administrator] ' + super.say();
  }
}

const user = new User('Alice');
console.log(user.say()); // My name is Alice

const admin = new Admin('Bob');
console.log(admin.say()); // [Administrator] My name is Bob
```

## Function Arguments
디폴트 인수, 가변길이 인수 사용이 가능하게 되었습니다.

```js
// ES6

// default parameter
loop(func, count = 3) {
  for (let i = 0; i < count; i++) {
	  func();
  }
}
// variable parameter
sum(...numbers){
  return numbers.reduce((a, b) => a + b);  
}

loop(() => console.log('hello')); // hello hello hello
console.log(sum(1, 2, 3, 4)); // 10
```

## Arrow Function
콜백 함수의 간단한 표현식 지원을 지원합니다.
```js
//Specifying parameters:
    () => { ... } // no parameter
     x => { ... } // one parameter, an identifier
(x, y) => { ... } // several parameters

// Specifying a body:
x => { return x * x }  // block
x => x * x  // expression, equivalent to previous line
```

 이 콜백함수를 등록하는 시점에서 `this`에 콜백 함수안의 억세스하고 싶은 경우도 자주 있습니다만, 여태까지는 클로져를 사용해 `this`를 보존하던가, `Function.prototype.bind`를 사용해 `this`를 속박하거나 했습니다. ES6에서는 Arrow Function라고 불리는 새로운 함수 정의 식이 도입되어, 이 `this`에 관한 번거로움을 해소하고 있습니다.


```js
// ES5
'use strict';

var ClickCounter = {
  _count: 0,

  start: function(selector) {
	var node = document.querySelector(selector);
	node.addEventListener('click', function(evt){
	  this._count++;
	}.bind(this));  // bind this!
  }
};

ClickCounter.start('body');
```

```js
// ES6
class ClickCounter{
  constructor(){
    this._count=0;
  }
  start(selector) {
  	const node = document.querySelector(selector);
  	node.addEventListener('click', (evt)=>{
  	  this._count++;
  	});
    }
}

ClickCounter.start('body');
```

## Promise
ES6에서는 Promise라는 비동기처리를 종합적으로 처리하는 방법이 언어레벨에서 재공되게 되었습니다. 사용법은, 비동기처리를 행하는 함수는 Promise를 반환값으로 반환해, 부른 쪽에서는 Promise에 콜백 함수를 등록한다는 것입니다.

```js
//Promise 선언
const _promise = (param) => {
	return new Promise((resolve, reject) => {
		window.setTimeout(() => {
			if (param) {
				resolve("해결 완료"); // 성공
			} else {
				reject(Error("실패!!")); //실패
			}
		}, 3000);
	});
};

//Promise 실행
_promise(true)
.then(text => {
	// 성공시
	console.log(text);
})
.catch((err)=> {
  // 실패시
  console.log(err);
});
```

여러개의 비동기 작업들이 실행되고 이들이 모두 완료되었을 때 작업을 진행하고 싶으면 ```Promise.all``` API를 활용하면 된다.

```js
const promise1 = new Promise((resolve, reject) => {
	window.setTimeout(() => {
		console.log("첫번째 Promise 완료");
		resolve("success1");
	}, Math.random() * 20000 + 1000);
});

const promise2 = new Promise((resolve, reject) => {
	window.setTimeout(() => {
		console.log("두번째 Promise 완료");
		resolve("success2");
	}, Math.random() * 10000 + 1000);
});


Promise.all([promise1, promise2])
.then((values) => {
	console.log(values); // ['success1','success2']
});

```
모든 Promise가 완료되면 최종적으로 전체값을 보여줍니다.

## 참조 링크  
http://blog.nekoromancer.kr/2016/01/26/es6-var-let-%EA%B7%B8%EB%A6%AC%EA%B3%A0-const/
https://gist.github.com/marocchino/841e2ff62f59f420f9d9  
http://meetup.toast.com/posts/85
http://programmingsummaries.tistory.com/325
http://hacks.mozilla.or.kr/2015/08/es6-in-depth-iterators-and-the-for-of-loop/
