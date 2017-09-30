## template이란?
 - 쉽게 생각하면 하나의 vue객체를 구성하는 태그들의 형태와 그 안의 데이터들

## 작성 문법(https://kr.vuejs.org/v2/guide/syntax.html)

#### 1. Mustache 구문(이중 중괄호)
 - 가장 기본적인 형태
 - 해당 data 객체의 message 속성값으로 대체

```html
//index.html
 <div id="app">
    {{ message }} <!-- 1. Mustache 구문 -->
 </div>
```

```js
//index.js
const app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

 - <code>v-once</code> 디렉티브를 이용한 업데이트 제거

 ```html
 <span v-once>다시는 변경하지 않습니다: {{ message }}</span>
 ```


#### 2. 원시 html
 - Mustache에서 html을 적용시키고 싶을 때 <code>v-html</code> 디렉티브 사용

```html
//index.html
 <div id="app" v-html="message"> <!-- 2. 원시 html 사용 -->
 </div>
```

```js
//index.js
const app = new Vue({
  el: '#app',
  data: {
    message: '<h1>안녕하세요 Vue!</h1>'
  }
})
```

 - 내부가 모두 <code>v-html</code>의 값으로 변한다.
 - 주의: 웹사이트에서 임의의 HTML을 동적으로 렌더링하려면 XSS 취약점으로 쉽게 이어질 수 있으므로 매우 위험할 가능성이 있습니다. 신뢰할 수 있는 콘텐츠에서는 HTML 보간만 사용하고 사용자가 제공한 콘텐츠에서는 절대 사용하면 안됩니다. (XSS 공격 기법: http://unabated.tistory.com/entry/XSS-%EA%B3%B5%EA%B2%A9-%EA%B8%B0%EB%B2%95)


#### 3. 속성
 - Mustaches는 HTML 속성으로 사용할 수 없으며 대신 <code>v-bind</code> 디렉티브을 사용

```html
//index.html
<div id="app" v-bind:style="invisible"> 3. v-bind를 이용한 속성 설정
    {{ message }}
</div>
```

```js
//index.js
const app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!',
    invisible: 'display: none'
  }
})
```


#### 4. Javascript 표현식
 - 지금까지 템플릿의 간단한 속성 키에만 바인딩했습니다. 그러나 실제로 Vue.js는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원합니다.

```html
//index.html
 <div id="app">
    <!-- {{var a = 1}} //error -->
    {{a}} 더하기 {{b}}는 {{ a + b }}

    <!-- {{ if (a+b==3) { return 'Yes' }else{return 'NO'} }} //error -->
    {{ a+b == 3 ? 'YES' : 'NO' }}
 </div>
```

```js
//index.js
const app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!',
    a:1,
    b:2
  }
})
```


## 디렉티브(https://kr.vuejs.org/v2/api/#v-text)

 - 디렉티브는 <code>v-</code> 접두사가 있는 특수 속성입니다. 디렉티브 속성 값은 단일 JavaScript 표현식 이 됩니다. (나중에 설명할 <code>v-for</code>는 예외입니다.) 디렉티브의 역할은 표현식의 값이 변경될 때 사이드이펙트를 반응적으로 DOM에 적용하는 것 입니다.


#### 1. 전달인자
 - 일부 디렉티브는 콜론(:)으로 표시되는 “전달인자”를 사용할 수 있습니다. 예를 들어, <code>v-bind</code> 디렉티브는 반응적으로 HTML 속성을 갱신하는데 사용됩니다.
 - v-bind (위의 3.속성 참고)
````html
 <a v-bind:href="url"></a>
````
 - v-on
```html
//index.html
<button id="app" v-on:click="doSomething">
    {{ message }}
</button>
```
````js
//index.js
const app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  },
  methods: {
    doSomething: function(){this.message += "click! "}
  }
})
````

#### 2. 수식어
 - 수식어는 점으로 표시되는 특수 접미사로, 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냅니다.
```html
<button v-on:click.once="doSomething">
    {{ message }}
</button>
```

#### 3. 약어
 - 자주 사용되는 <code>v-bind</code>와 <code>v-on</code>에 대해 시각적으로 보기 쉽게 만든 것

 - v-bind 약어
```html
<!-- 전체 구문 -->
<a v-bind:href="url"></a>
<!-- 약어 -->
<a :href="url"></a>
```

 - v-on 약어
```html
<!-- 전체 구문 -->
<a v-on:click="doSomething"></a>
<!-- 약어 -->
<a @click="doSomething"></a>
```