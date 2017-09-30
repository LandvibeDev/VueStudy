
### Computed Properties and Watchers

#### Computed Properties

##### 보간법 사용의 좋지 않은 예

```html
<div id="app">
  <p>original: {{ message }}</p>
  <p>reversed: {{ message.split('').reverse().join('') }}</p>
</div>
```

`Vue.js`와 같은 프론트엔드 프레임워크를 사용하는 본질은 `html` 문법과 `javascript` 문법을 논리적으로
분리하여 코드의 재사용성을 줄이고 유지보수를 편리하게 함에 있다. 만약 위와 같은 문법을 여러 번 사용해야할
경우를 생각해보자.

```html
<div id="app">
  <p>original: {{ message }}</p>
  <p>reversed: {{ message.split('').reverse().join('') }}</p>
  <p>reversed: {{ message.split('').reverse().join('') }}</p>
  <p>reversed: {{ message.split('').reverse().join('') }}</p>
</div>
```

복잡한 코드를 여러번 사용하여 코드의 재사용성을 증가시킬 뿐만 아니라 해당 문법을 바꿀 경우 3군데 모두
바꾸어주어 유지보수에도 효율성이 떨어진다. 만약 바꿔야하는 소스코드가 10개 혹은 100개 이상일 경우에는,
상상하지 말자.


##### Computed Properties Example

```html
<div id="app">
  <p>original: {{ message }}</p>
  <p>reversed: {{ reversedMessage }}</p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  computed: {
    reversedMessage: function(){
      return this.message.split('').reverse().join('');
    }
  }
})
```

위의 예제에서와는 다르게 `this.message.split('').reverse().join('');` 코드를 불필요하게
반복하는 일이 없어졌고 소스코드를 수정해야할 경우에는 `reversedMessage` 함수의 정의만 바꾸어주면
되기 때문에 유지보수에도 편리하다.


##### Computed Caching vs Methods

```html
<div id="app">
  <p>original: {{ message }}</p>
  <p>reversed: {{ message.split('').reverse().join('') }}</p>
  <p>reversed: {{ reversedMessage }}</p>
  <p>reversed: {{ reverseMessage() }}</p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  computed: {
    reversedMessage: function(){
      return this.message.split('').reverse().join('');
    }
  },
  methods: {
    reverseMessage: function () {
      return this.message.split('').reverse().join('');
    }
  }
})
```

`computed` 속성과 `methods` 속성의 차이점은 캐싱에 있다. 간단히 말해서 메소드에 존재하는 함수를
호출하면 호출할 때마다 함수를 실행하는 반면에 종속적인 데이터의 변환이 없는 한 `computed`는
`이전의 계산된 결과를 즉시 반환`하여 불필요한 함수의 호출을 방지할 수 있다.


##### Computed & Watched Property

```html
<div id="app">
  <p>full name: {{ fullName }}</p>
  <p>full name: {{ getFullName }}</p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    firstName: 'foo',
    lastName: 'bar',
    fullName: 'foo bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName;
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val;
    }
  },
  computed: {
    getFullName: function(){
      return this.firstName + ' ' + this.lastName;
    }
  }
})
```

위의 예제를 간단히 설명하겠다. 만약에 아프리카에 사는 사람은 이름을 5가지나 사용한다고 생각해보자.
`firstName`, `secondName`, `thirdName`, `fourthName`, `lastName`으로 구성된 이름을
사용한다고 하였을 때, 변수에 종속적인 `watcher` 콜백 함수로 설계하면 총 5가지 변수를 감시해야 한다.
하지만 `computed` 속성의 함수로 정의하면 하나의 함수에 5가지 변수를 종속적으로 설계할 수 있다.


##### Computed Setter

```js
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

`computed` 속성은 기본적으로 `getter` 함수만 가지고 있지만 필요에 따라 `setter` 함수를 선언할 수
있는데 `getter`, `setter` 함수에 대한 이해가 있으면 따로 설명할 소프트웨어 디자인의 이슈는 없다.


#### Watchers

계산된 속성보다 사용자 정의 감시자가 더 필요한 경우가 있는데 데이터 변경에 대한 응답으로 비동기식 또는
시간이 많이 소요되는 조작을 수행하려는 경우에 가장 유용합니다. 간단히 설명하면 종속적인 데이터의 변환이
시작되고 결과값에 반영되기까지 시간이 오래 걸리는 작업이 필요한 경우 사용자 정의 감시자가 더 적합하다는
뜻이다.

```html
<div id="app">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
      },
      // This is the number of milliseconds we wait for the
      // user to stop typing.
      500
    )
  }
})
```

`input` 폼의 값이 달라짐에 따라 결과값인 `answer` 변수가 변하는데 과정이 복잡하고 시간이 오래 걸리기
때문에 `computed` 속성의 함수로는 설계할 수 없다. 이런 경우에 사용자 정의 `watcher` 함수를 사용하여
설계하는 것이 더 적합하다.
