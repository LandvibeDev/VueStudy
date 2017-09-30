# Babel
## Babel이란?
간단히 말해서 es6,7..의 코드를 es5로 변환 시켜주는 도구입니다. babel이 필요한
이유는 모든 브라우저가 es6를 포함한 상위 버전을 지원하지는 않기 때문입니다. ( 익스플로어..)

## Babel 사용
babel만 이용할 수도 있지만, webpack을 쓸 것이기 때문에 webpack에 달아서 쓰는 연습을
해보겠습니다.

### babel 설치
일단 필요한 babel을 설치합니다.
```
npm install --save-dev babel-core babel-preset-env babel-preset-stage-2
```

* 설명
   * preset : plugin 의 모음
   * stage :EcmaScript중에서 비공식 실험적인 기술들을 사용할 수 있게 해주는 프리셋
      * 0,1 : 자주 바뀌는 기술들
      * 2 : 안정적인 기술
   * env 프리셋은 최신 ecmascript 사용, 사용자 환경에 맞춰서 polyfill까지 넣어줌
env를 쓰지 않을 꺼면 babel-preset-es2015 를 설치하여 사용합니다.

### babel-polyfill
babel만 사용하면 단순히 es6의 문법을 es5로만 바꾸어줍니다. 하지만 이 것이 문제가 되는
이유는 ES6의 새로운 객체(Promise, Map, Set 등등)과
메소드(Array.find, Object.assign 등등)을 사용할 수 없습니다.
예를 들어 설명하겠습니다.

```js
const foo = {
  name: 'Homer'
};
const bar = Object.assign({}, foo, {age: '?'});
console.log(Object.keys(foo), Object.keys(bar));
```

```js
'use strict';
var foo = {
  name: 'Homer'
};
var bar = Object.assign({}, foo, { age: '?' });
console.log(Object.keys(foo), Object.keys(bar));
```
출처 : https://stackoverflow.com/questions/36196592/why-do-we-need-to-use-import-babel-polyfill-in-react-components
아래의 es5문법에서는 Object.assign을 사용할 수 없지만 babel만 사용하면 단순히
저러한 식으로 변환이 되기 때문에 polyfill이 필요합니다.

poly-fill을 설치합니다. (env를 쓰면 설치할 필요 없습니다)
```js
npm install --save-dev babel-polyfill
```
webpack 사용시에는
```js
import 'babel-polyfill';
```
를 사용하면 webpack이 알아서 적용합니다.