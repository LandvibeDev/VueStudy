
vue.js 적용해보기


## vue 시작하기
```html
// index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vueTest</title>
</head>
<body>
<div id="app">
 { {message} } // { 붙여야..
</div>

</body>
<script src="bundle/index.bundle.js"></script>
</html>
```

```js
// index.js
import Vue from "vue"

let app = new Vue({
    el: '#app',
    data: {
        message: '안녕하세요 Vue!'
    }
});
```
```js
// webpack.config.js
module.exports = {
    entry: {
        "index" : './index.js'
    },

    output: {
        path: __dirname + '/bundle/',
        filename: '[name].bundle.js'
    },
    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    }
}
```

```
webpack
```
을 통해 빌드를 한 후 확인합니다.



(아직 runtime과 compiler를 포함한다는 것에 대한 개념이 확실치 않아서
vue를 runtime-only가 아닌 runtime-only + compiler를 포함한 코드를 사용
하였습니다.)

