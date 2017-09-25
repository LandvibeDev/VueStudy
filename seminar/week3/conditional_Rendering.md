# Conditional Rendering

## v-if
#### v-else, v-else-if 와 함께 사용 가능하다.

```html
<h1 v-if="value > 5">5보다 크다.</h1>
<h1 v-else-if="value ===  5"> 5!</h1>
<h1 v-else> 5보다 작다. </h1>
```
 순서는 v-if, v-else-if, v-else 로 사용하며, v-else-if는 여러번 사용 가능하다.

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

 하나의 엘리먼트 뿐 아니라 template 엘리먼트에도 사용 가능하다.

#### key를 이용한 엘리먼트 제어

```html
<template v-if="loginType === 'username'">
  <label>사용자 이름</label>
  <input placeholder="사용자 이름을 입력하세요">
</template>
<template v-else>
  <label>이메일</label>
  <input placeholder="이메일 주소를 입력하세요">
</template>
```
loginType을 변경해도 입력한 내용은 지위지지 않고, placeholder만 변경된다.
input 내용도 변경하고 싶다면 아래처럼 key 속성을 추가해준다.  
```html
<input palceholder="사용자 이름을 입력하세요" key="username-input">
```

## v-show
v-if와 사용법은 거의 동일하다. v-show는 엘리먼트가 항상 렌더링되고 display: none 속성이 추가되고 사라짐으로써 화면에 보여지고 안보인다. DOM에 남아있다.

## v-if vs v-show
- v-if는 초기 렌더링에서 조건이 거짓이면 아무것도 하지 않는다.
- v-show CSS 기반 토글만으로 초기 조건에 관계없이 항상 렌더링 된다.
- v-if는 토글 비용이 높고, v-show는 초기 렌더링 비용이 높다. 자주 바꾸기를 원한다면 v-show를 사용하고, 그렇지 않다면 v-if를 사용하는 것이 좋다.
