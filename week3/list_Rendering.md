# List Rendering

## v-for
#### 사용방법

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
```html
<ul id="example-2">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```
- Foo
- Bar

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```
- Parent-0-Foo
- Parent-1-Bar

부모 속성에 대한 모든 권한 있고, (item,index)를 이용하는 옵션도 제공한다.
v-if와 마찬가지로 \<template> 태그를 사용하여 엘리먼트 블럭도 렌더링 가능하다.

#### 객체 속성
```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
```html
<ul id="v-for-object" class="demo">
  <li v-for="(value, key, index) in object">
    {{ index }}. {{ key }} : {{ value }}
  </li>
</ul>
```
0. firstName : John
1. lastName : Doe
2. age : 30

객체 속성에 index, key, value 값으로 다 접근이 가능하다.

###### 객체를 반복할 때 순서는 Object.keys()의 키 나열 순서에 따라 결정되므로 JavaScript 엔진 구현에 따라 다르다.

#### v-for의 범위
```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
1부터 10까지 차례대로 출력된다.

#### v-for 와 v-if
v-for 가 v-if 보다 더 높은 우선순위를 가진다.
```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```
for문이 일단 실행되고 todo.isComplete에 결과에 따라 결과값이 결정된다.
```html
<ul v-if="shouldRenderTodos">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
```
shouldRenderTodos의 true/false 결과에 따라 v-for 사용여부가 결정된다.

#### key
```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- content -->
</div>
```
Vue가 렌더링 된 엘리먼트 목록을 갱신할 때 각 요소를 적절한 위치에 패치하고 해당 인덱스에서 렌더링할 내용을 반영하는지 확인한다.
목록의 출력 결과가 하위 컴퍼넌트 상태 또는 임시 DOM 상태에 의존하지 않는 경우에 적합하다.
Vue가 기존 엘리먼트를 재사용하고 재정렬하려면 고유한 key 속성을 제공해야하고 v-bind를 사용하여 동적 값에 바인딩 해야 한다.

#### 배열 변경 감지

###### 메소드
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

원본 배열을 변경한다.

예 :
```js
example1.items.push({ message: 'Baz' })
```

- filter()
- concat()
- slice()

원본 배열이 아닌 새 배열을 반환한다. 하지만 전체 목록을 다시 렌더링하지 않고 효율적으로 구현한다.

예:
```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
##### 주의사항
###### 배열에 변경 감지
1. 인덱스로 배열에 있는 항목을 직접 설정할 경우
```js
//x
v.items[index] = newvalue
```
```js
//o
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
```
```js
//o
//Array.prototype.splice
example1.items.splice(indexOfItem, 1, newValue)
```
2. 배열의 길이를 수정할 경우
```js
//x
v.items.length = newlength
```
```js
//o
example1.items.splice(newLength)
```

###### 객체에 변경 감지
```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 는 반응형입니다.
vm.b = 2
// `vm.b` 는 반응형이 아닙니다.
```
속성 및 삭제를 감지하지 못한다.
```js
Vue.set(vm.userProfile, 'age', 27)
```
```js
this.$set(this.userProfile, 'age', 27)
```
위 두가지를 사용하여 새로운 객체 속성을 추가할 수 있습니다.
또는
```js
this.userProfile = Object.assign({}, this.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```
Object.assign()을 사용해 새로운 객체를 생성해 기존의 객체에 새 속성을 추가할 수 있다.

#### 필터링/정렬된 결과
```html
<li v-for="n in evenNumbers">{{ n }}</li>
```
```js
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
