## Vue 인스턴스 만들기
```js
var vm = new Vue({
  // 옵션
})
```
Vue 함수로 Vue인스턴스를 만드는 것이 시작
### MVC 패턴
![mvc](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/327/1262.png) <br>
MVC란 Model View Controller의 약자로 에플리케이션을 세가지의 역할로 구분한 디자인패턴<br>
Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자에게 전달하게 된다. 
- example
1. 사용자가 웹사이트에 접속한다. (Uses)
2. Controller는 사용자가 요청한 웹페이지를 서비스 하기 위해서 모델을 호출한다. (Manipulates)
3. 모델은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후에 그 결과를 리턴한다.
4. Controller는 Model이 리턴한 결과를 View에 반영한다. (Updates)
5. 데이터가 반영된 VIew는 사용자에게 보여진다. (Sees)
### MVVM 패턴
![mvvm](https://joshua1988.github.io/images/posts/web/vuejs/view-model.png) <br>
MVC패턴괴 비슷한 구조 <br>
ViewModel이 View로 부터 event를 받아들여 Model로 전달하고 DB나 네트워크 처리를 담당하는 Model에서 Data binding을 시켜 View로 전달<br>
웹페이지는 DOM과 JavaScript로 이루어지는데 DOM이 View의 역할 JavaScript가 Model의 역할을 한다. 뷰모델이 없는 아키텍처에서는 getElementById 같은 돔 API를 직접 이용해서 모델과 뷰를 연결해야 한다.<br>
뷰모델에 자바스크립트 객체와 돔을 연결해 주면 뷰모델은 이 둘간의 동기화를 자동으로 처리한다. 이것이 뷰js가 하는 역할이다.

## 프록시 처리
각 Vue 인스턴스는 data 객체에 있는 모든 속성을 프록시 처리 합니다.
데이터가 변경되면 화면은 다시 렌더링됩니다. data의 속성은 인스턴스가 생성될때 있었다면 반응형입니다. 즉 다음과 같이 새 속성을 추가합니다.

## 옵션
- el,data,methods<br>
```js
import Vue from 'vue'

var myVue = new Vue({
  el: '#app',  // DOM 엘리먼트에  Vue객체를 마운트(연결) 할 때 사용된다.
  data: {      // Vue객체가 가지고 있는 데이터
    a: 1,
    name: 'Vue'
  },
  methods: {   // Vue객체가 가지고 있는 함수
    message: function () {
      return 'Hello' + this.name;
    }
    plus: function () {
       this.a++
    }
  }
})
console.log(myVue.$el)
myVue.plus()
console.log(myVue.$data.a)
```
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>firstprac</title>
</head>
<body>
    <div id="app">  <!-- Vue객체가 마운트될 DOM 엘리먼트 -->
        {{ name }}  <!-- data 렌더링 -->
        {{ message ()  }}  <!-- method 실행, return값 렌더링 -->
    </div>
</body>
</html>
```
```output
안녕하세요 Vue
```
## 옵션에 대한 접근 ($를 사용)
- myVue.$el<br>
```html
<div id="app">
        안녕하세요 Vue
</div>
```
- myVue.$data.a<br>
```
2
```
## 인스턴스 라이프사이클
![lifecycle](https://kr.vuejs.org/images/lifecycle.png)
```js
new Vue({
  data: {
    a: 1,
    message: '안녕하세요 Vue'
  },
  created: function () {
    console.log('created')
  },
  mounted: function () {
    this.$nextTick(function () {
    // 모든 화면이 렌더링된 후 실행합니다.
      console.log('mounted')
    })
  }
})
```
```output
created
```
created가 실행되었지만 mounted는 실행되지 않는다.
```js
el: '#app',
```
```
created
mounted
```
```js
methods: {
    plus: function () {
      this.a++
    }
  },
updated: function () {
  console.log('updated')
}
```
```html
<div id="app">
    {{ message }}
    {{ a }}
</div>
```
data가 렌더링되어있고 그 값이 바뀌면 updated가 실행된다.
```
created
mounted
updated
```
```js
methods: {
    plus: function () {
      this.a++
    },
    remove: function () {
      this.$destroy(true)
    }
  },
destroyed: function () {
    console.log('destroyed')
  }
myVue.remove()
```
myVue에 remove를 호출하면 destroyed가 실행된다.
```
created
mounted
destroyed
updated
```

