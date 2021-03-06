모듈 기반 개발
---------------
* 한 가지 일을 수행하는 독립적인 모듈들로 개발하는 것이 좋다.
* 작은 모듈이 이해하고, 디버그하고, 유지보수하기 더 쉽기 때문
* 다른 개발자와 협업하는 경우에는 특히 더!

컴포넌트?
-----------

* 컴포넌트는 개념적으로 `props`를 input으로 하고 UI가 어떻게 보여야 하는지 정의하는 `React Element`를 output으로 하는 함수입니다.
* 기본 HTML 엘리먼트를 확장하여 재사용 가능한 코드를 캡슐화하는 데 도움이 됩니다.

컴포넌트는 이래야한다
--------------------
* 너무 많거나 너무 크면 작은 컴포넌트로 나눠서 구성
* 일반적으로 100라인 미만으로 유지하는 것이 좋다.
* 독립적으로 작동하는지 확인해야한다.

사용해보자
------------------
### 등록
```html
<div id="example">
  <my-component></my-component>
</div>
```
```js
// 등록
Vue.component('my-component', {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
})
// 루트 인스턴스 생성
new Vue({
  el: '#example'
})
```

렌더링 결과
```html
<div id="example">
  <div>사용자 정의 컴포넌트 입니다!</div>
</div>
```

### 지역 등록

```js
var Child = {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 는 상위 템플릿에서만 사용할 수 있습니다.
    'my-component': Child
  }
})
```

### data는 반드시 함수여야 한다
 `Vue` 생성자에 사용할 수 있는 대부분의 옵션은 컴포넌트에서 사용할 수 있지만, 한가지 특별한 경우가 있다. `data` 는 함수여야 한다. 이렇게 쓴다면 :
```js
Vue.component('my-component', {
  template: '<span>{{ message }}</span>',
  data: {
    message: 'hello'
  }
})
```
(X) `data`는 컴포넌트 인스턴스의 함수여야한다!!
아래와 같이 수정해서 쓰자.

```html
<div id="example-2">
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
</div>
```
```js
var data = { counter: 0 }
Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // 데이터는 기술적으로 함수이므로 Vue는 따지지 않지만
  // 각 컴포넌트 인스턴스에 대해 같은 객체 참조를 반환합니다.
  data: function () {
   return {
    counter: 0
  }
 }
})
new Vue({
  el: '#example-2'
})
```

### 컴포넌트 스타일 가이드

#### 컴포넌트 표현식을 단순하게 사용하기
* Vue.js의 인라인 표현식은 100% 자바스크립트입니다. 이 기능은 매우 강력하지만 잘못 사용하면 매우 복잡해질 수 있습니다. 따라서 표현식을 단순하게 사용해야 합니다.  
```js
<!-- 권장합니다 -->
<template>
	<h1>
		{ { `${year}-${month}` } }
	</h1>
</template>
<script type="text/javascript">
  export default {
    computed: {
      month() {
        return this.twoDigits((new Date()).getUTCMonth() + 1);
      },
      year() {
        return (new Date()).getUTCFullYear();
      }
    },
    methods: {
      twoDigits(num) {
        return ('0' + num).slice(-2);
      }
    },
  };
</script>
```
```js
<!-- 피하세요! -->
<template>
	<h1>
		{ { `${(new Date()).getUTCFullYear()}-${('0' + ((new Date()).getUTCMonth()+1)).slice(-2)}` } }
	</h1>
</template>
```

#### 컴포넌트 props를 잘 사용하기
* Vue.js에서는 컴포넌트 props가 당신의 API 입니다. 강력하고 예측가능한 API를 만들면 다른 개발자가 컴포넌트를 쉽게 사용할 수 있습니다.
##### 왜 그렇게 하나요?
* 컴포넌트 props를 잘 사용하면 당신의 컴포넌트는 항상 작동할 것 입니다(방어적 프로그래밍). 나중에 다른 개발자가 당신이 생각하지 못한 방법으로 컴포넌트를 사용하더라도 마찬가지 입니다.


### 컴포넌트 작성
컴포넌트는 부모-자식 관계에서 함께 사용하기 위한 것입니다. 부모는 자식에게 데이터를 전달해야 할 수도 있으며, 자식은 자신에게 일어난 일을 부모에게 알릴 필요가 있습니다. 그러나 부모와 자식이 가능한한 분리된 상태로 유지하는 것도 매우 중요합니다. 이렇게하면 유지 관리가 쉽고 잠재적으로 쉽게 재사용 할 수 있습니다.  
  `Vue.js`에서 부모-자식 컴포넌트 관계는 `props`는 아래로, `events`는 위로 라고 요약 할 수 있습니다. 부모는 `props`를 통해 자식에게 데이터를 전달하고 자식은 `events`를 통해 부모에게 메시지를 보냅니다. 어떻게 작동하는지 보겠습니다.


Props [https://kr.vuejs.org/v2/api/#props]
---------------
### Props로 데이터 전달하기
모든 컴포넌트 인스턴스에는 자체 격리 된 범위 가 있습니다. 즉, 하위 컴포넌트의 템플릿에서 상위 데이터를 직접 참조 할 수 없으며 그렇게 해서는 안됩니다. 데이터는 `props` 옵션 을 사용하여 하위 컴포넌트로 전달 될 수 있습니다.  
`prop`는 상위 컴포넌트의 정보를 전달하기위한 사용자 지정 특성입니다. 하위 컴포넌트는 `props` 옵션을 사용하여 수신 할 것으로 기대되는 `props`를 명시적으로 선언해야합니다. 
```js
Vue.component('child', {
  // props 정의
  props: ['message'],
  // 데이터와 마찬가지로 prop은 템플릿 내부에서 사용할 수 있으며
  // vm의 this.message로 사용할 수 있습니다.
  template: '<span>{{ message }}</span>'
})
```
```html
<child message="안녕하세요!"></child>
```
~~~
안녕하세요!
~~~

<img src = "https://cdn-images-1.medium.com/max/1600/1*1JzmOFt70B-EF3rQzrI9PQ.png" width = "600"  height = "300"></img>
<img src = "https://kr.vuejs.org/images/props-events.png" width = "600"  height = "500"></img>


### Props를 원시 자료형으로 사용하기
Vue.js는 다양한 속성을 통해 복잡한 JavaScript 객체를 전달할 수 있지만 컴포넌트 옵션을 가능한 원시 자료형으로 유지하는 것이 좋습니다. 즉, JavaScript 원시 값 (string, number, boolean) 및 함수만을 사용하고 복잡한 Object는 피하는게 좋습니다.
#### 왜 그렇게 하나요?
* 개별적으로 prop 속성을 사용함으로써 컴포넌트는 명확한 API를 가집니다.
* 개별적으로 prop 속성을 사용하면 다른 개발자가 컴포넌트 인스턴스로 전달되는 내용을 쉽게 이해할 수 있습니다.
* 복잡한 Object를 전달할 때 전달한 Object의 속성과 메서드가 실제로 사용자 정의 컴포넌트에서 사용되는지 분명하지 않습니다. 이렇게 사용하면 코드를 리팩토링하기가 어려워지고 코드가 더러워 질 수 있습니다.

### 단방향 데이터 흐름
모든 `props`는 하위 속성과 상위 속성 사이의 단방향 바인딩을 형성합니다. 상위 속성이 업데이트되면 하위로 흐르게 되지만 그 반대는 안됩니다. 이렇게하면 하위 컴포넌트가 실수로 부모의 상태를 변경하여 앱의 데이터 흐름을 추론하기 더 어렵게 만드는 것을 방지할 수 있습니다.  
또한 상위 컴포넌트가 업데이트 될 때마다 하위 컴포넌트의 모든 `props`가 최신 값으로 갱신됩니다. 즉, 하위 컴포넌트 내부에서 `prop`을 변형하려고 시도하면 안됩니다. 만약 그럴 경우, `Vue`가 콘솔에서 경고합니다.
