# 클래스와 스타일 바인딩

 데이터바인딩(v-bind)
 1. DOM 엘리먼트의 클래스 목록 조작
 2. 인라인 스타일 조작  


 Vue에서 class와 style에 **v-bind:class, v-bind:style** 사용으로 특별히 향상된 기능 제공
 (문자열외 객체, 배열에도 가능)

## HTML 클래스 바인딩
### 객체구문
 클래스를 동적토글하기 위해, **v-bind:class** 에 객체 전달 가능 .
 다음은 isActive==true일시, active클래스 가 존재한다 .
```html
<div v-bind:class="{ active: isActive }"></div>
```
렌더링하면
```html
<div class="active"></div>
```
과 같다.  

 객체에 필드가 더 있다면, 여러 클래스를 토글할 수 있으며, v-bind:class 디렉티브는 일반 class속성과도 공존
 ```html
 <div class="static"
     v-bind:class="{ active: isActive, 'showing-error': hasError }">
</div>
 ```
 데이터는
 ```javascript
 data: {
  isActive: true,
  hasError: false
}
 ```

이면,
```html
<div class="static active"></div>
```

 만약, **hasError=true**로 수정되면, 클래스목록도 그에 따라 "**static active showing-error**"로 업데이트됨

#### 바인딩 된 객체 시
 바인딩 된 객체는 인라인 일 필요 x
```html
 <div v-bind:class="clickObject"></div>
```

```javascript
 data: {
  clickObject: {
    active: true,
    'showing-error': false
  }
}
```
 와 같다. 또한, 객체를 반환하는 '계산된속성'에도 바인딩 가능.=>일반적이지만 강력함

```javascript
 data: {
  isActive: true,
  hasError: null
},
computed: {
  clickObject: function () {
    return {
      active: this.isActive && !this.hasError,
      'showing-error': this.hasError && this.hasError.type === 'fatal'
    }
  }
}
```
### 배열 구문
 v-bind:class에 배열을 넣어서, 클래스 목록 지정할 수 있다.

 ```html
 <div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'showing-error'
}
```
위처럼 배열화 한것을 렌더링 하면 아래와 같다
```html
<div class="active showing-error"></div>
```

목록에 있는 클래스를 조건부 토글 =>삼항 연산자 이용
=> 하지만 논리식은 최대한 배제하는 것이 좋다. 길어질 경우 복잡함.  
그래서 배열구문내에 객체구문 사용하는 방법을 사용하기도 한다.

```html
//errorCLass는 항상 적용하나, isActive=true일 경우에만, activeClass만 적용됨
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

```html
//배열구문내에 객체구문 사용
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### 컴포넌트와 함께 사용하는 방법
 사용자 정의 컴포넌트로 **class** 속성을 사용하면, 클래스가 컴포넌트의 루트 엘리먼트에 추가된다.
 이 엘리먼트는 기존 클래스를 덮어쓰지 x

```javascript
 Vue.component('my-component', {
  template: '<p class="I am the new component.">Hi</p>'
})
```
위처럼 컴포넌트를 선언하였다.

```html
<my-component class="나는 새로 추가된 클래스야"></my-component>
```
위처럼 사용할 클래스 일부를 추가했다.
그러면 , 아래처럼 렌더링 된다
```html
<p class="I am the new component.나는 새로 추가된 클래스야">Hi</p>
```

클래스 바인딩도 동일하다!
```html
<my-component v-bind:class="{ active: isActive }"></my-component>
```
isActive=true 이면,
```html
<p class="i'm the new component. active">Hi</p>
```


# 인라인 스타일 바인딩

### 객체구문
 v-bind:style 객체구문은 매우 직관적. css처럼 보이지만, javascript객체다.  
_ex) 케밥표기법(kebab-case), 카멜표기법(camelCase, 따옴표함께) 사용_

```html
<div v-bind:style="{ color: myColor, fontSize: myFontSize + 'px' }"></div>
```
```javascript
data: {
  myColor: 'red',
  myFontSize: 30
}
```
 이렇게 표시가능, **하지만** 스타일 객체에 직접 바인딩 하여 템플릿이 더 간결하게 만들면 good
 ```html
 <div v-bind:style="myObject"></div>
```

```javascript
data: {
  myObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### 배열 구문

**v-bind:style 배열**은 같은 스타일의 엘리먼트에 여러 개의 스타일 객체를 사용 ok
```html
<div v-bind:style="[aStyles, bStyles]"></div>
```
#### 자동 접두사
v-bind:style 에 **브라우저 벤더 접두어** 가 필요한 css 속성을 사용하면, Vue는 자동으로 해당 접두어를 감지하여 스타일 적용한다
