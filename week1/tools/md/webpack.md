
## Webpack이란?
> 웹팩은 모듈 번들러 입니다.
웹팩은 상호 의존성이 있는 모듈들을 사용해 그 모듈들과 같은 역할을 하는 정적 에셋들을 생성해냅니다.
(Webpack is a module bundler. Webpack takes modules with dependencies and generates static assets representing those modules.) - 공식 사이트

webpack은 기본적으로 module bundler입니다.
module bundler가 등장하게 된 이유는 코드가 많아지다 보면 파일을 나누어서 짜야 하는 일이
생기게 됩니다. 이 때 그 나눈 파일들을 모듈로 관리하고 싶어지게 됩니다.
하지만 javascript는 이러한 모듈간의 import를 지원하지 않기 때문에 webpack을 통해 여러js파일을
하나로 번들링하여 사용할 수 있습니다.



### Webpack을 쓰지 않았을 때?
```html
// main.html
...
<script src="/resources/js/node_modules/jquery/dist/jquery.js"></script>
<script src="/resources/js/node_modules/handlebars/dist/handlebars.js"></script>
<script src="/resources/js/node_modules/@egjs/component/dist/component.js"></script>
<script src="/resources/js/handlebarsModule.js"></script>
<script src="/resources/js/flicking.js"></script>
<script src="/resources/js/review.js"></script>
```
이러한 식으로 외부 모듈의 경우 직접 node_modules안으로 파고 들어가서 찾아쓰고
개발자가 만든 모듈 또한 찾아서 html안에 script로 넣어야 하는 번거로움이 있었습니다.
더군다나 이 경우 script를 불러오는데 http통신을 6번이나 하게 됩니다.

## webpack 설치
webpack은 npm을 통해 설치 할 수 있습니다.
global하게 깔아도 되고 프로젝트 안에 깔아도 상관없습니다.
하지만 project에 설치할 때는 dev옵션을 줘서 까는 것이 좋습니다.
```
// 글로벌
npm install -g webpack
```
webpack을 깔았으면 설정을 해줘야 합니다.
```javascript
module.exports = {
    entry: {
        "index" : './index.js'
    },
    output: {
        path: __dirname + '/bundle/',
        filename: '[name].bundle.js'
    }
};
```
webpack.config.js 파일을 만들어 줍니다.
webpack은 기본적으로 저 파일을 통해 설정을 확인합니다.
`entry`는 기본적으로 실행을 하는 js파일입니다.
여러개 설정 가능합니다.
`output`은 그 entry를 바탕으로 사용하는 모듈들을 불러와서 번들링한 js파일입니다.


## Webpack loader
로더는 간단히 말하면 특정 파일을 로드할 때 거치는 과정(?) 이라고 할 수 있습니다.
### Stylesheet
stylesheet을 js파일로 번들링 할 수 있는 방법입니다.
```
npm install --save-dev style-loader css-loader
```
* style-loader : Adds CSS to the DOM by injecting a `<style>` tag
(html에 `<style>` tag를 injecting 함)
* css-loader : CSS의 @import나 url()문을 CommonJS의 require 처럼 해석할 수 있도록 만들어주는 로더

webpack.config.js에 추가합니다.
```js
module.exports = {
    entry: {
        "index" : './index.js'
    },
    output: {
        path: __dirname + '/bundle/',
        filename: '[name].bundle.js'
    },
    module: {
        rules: [
         {
            test: /\.css$/,
            use: [
              'style-loader',
              'css-loader'
            ]
         },

        ]
    }
};
```
같은 파일에 대해서는 로더가 아래쪽 부터 적용됩니다.

### babel
전에 babel을 다 설치했으니 babel-loader만 설치합니다.
```
npm install --save-dev babel-loader
```

```js
module.exports = {
    entry: {
        "index" : './index.js'
    },
    output: {
        path: __dirname + '/bundle/',
        filename: '[name].bundle.js'
    },
    module: {
        rules: [
         {
            test: /\.css$/,
            use: [
              'style-loader',
              'css-loader'
            ]
         },
         {
             test: /\.js$/,
             exclude: /node_modules/,
             loader: 'babel-loader',
             options: {
               presets: ['env', "stage-2"]
             }
           }
        ]
    }
};
```

```
<! --index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vueTest</title>
</head>
<body>
<div id="app">
test!
</div>

</body>
<script src="bundle/index.bundle.js"></script>
</html>
```

```js
// index.js
import './style.css';
```
```css
/* style.css */
#app {
    color:red;
}
```
webpack으로 빌드한 후 확인합니다.
`<style>` tag가 추가된걸 확인 할 수 있습니다.
### 추가적인 기능

> minimize : 코드 최적화

> tree shaking : 사용되는 코드만 번들링

> Code Splitting에서 : 코드를 분리한 후 비동기 적으로 import 시켜서 사용할 수 있음

> Hot Reloading : --watch 옵션으로 바뀐 코드를 자동으로 빌드

등등

