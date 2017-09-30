
## npm이란?
npm은 node packaged modules 의 약자로 node.js에서 필요한 모듈을 패키지로
모아 놓은 것입니다. npm을 통해 다른 사람들이 만든 모듈을 다운 받을 수 있습니다.
사실 npm은 nodejs를 위해 나온 것이지만 (node.js를 깔면 같이 깔린다)
요즘은 Front-End에서도 활용하고 있습니다.

## npm 설치
npm은 nodejs를 깔 때 같이 깔리게 되어 node.js를 받아야 합니다.
<a href ="https://nodejs.org/ko/">설치 </a>
LTS로 설치를 주로 하니 LTS로 설치하는 것을 권장합니다.
환경변수는 알아서 다 잡아줍니다.

## npm 기본 사용법
설치가 완료 되면 cmd 창에서 node,npm 명령어를 사용할 수 있습니다.
npm -v 을 이용하여 버전을 확인합니다.
저는 현재 5.4.0인데 만약 최신이 아니라면
```
npm install -g npm
```
으로 최선버전으로 업데이트 해줍니다.( 여기서 -g는 global 옵션입니다.)
```
C:\Users\사용자\AppData\Roaming\npm\node_modules
```
-g 옵션의 기본 설치 경로입니다.

이제 자신의 프로젝트 디렉토리에 가서

```
npm init
```
명령어를 칩니다. 그럼 여러가지 입력창이 나오는데 모두 엔터눌러도 상관 없습니다. (연습이라)
입력을 모두 마치면 package.json 파일이 하나 생깁니다.
package.json은 해당 프로젝트의 필요한 module들의 의존성을 관리합니다.
```
# 최신 안정화 버전
$ npm install vue --save
```
간단하게 vue.js를 설치해봅니다.
```json
{
  "name": "vueTest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "vue": "^2.4.2"
  }
}
```
그럼 이러한 형식으로 package.json의 `dependencies`에 vue가 추가되는 것이 보입니다.
`dependencies`는 해당 프로젝트의 모듈의 의존성을 적어 둡니다.(--save 옵션으로)
또한 node_modules라는 폴더에 vue폴더가 생긴 것을 볼 수 있습니다.
이렇게 사용하는 이유는 git에 많은 module들을 올리는 것은 비효율적이고, module들의
버전을 적어 놓음으로써 다른 버전을 사용해서 올 수 있는 이슈들을 막을 수 있습니다.
```
npm install
```
명령어를 치면 `dependencies`에 있는 모듈들을 node_module 폴더에 설치합니다.

`scripts`는 자신이 쓰고자 하는 스크립트를 적어 놓을 수 있습니다.
예를 들어
```
npm run test
```
를 하면 "echo \"Error: no test specified\" && exit 1" 명령어를 실행합니다.







