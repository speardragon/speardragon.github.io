---
layout: single
title: "[Nodejs] npm, yarn 등 패키지 매니저 그리고 package.json 파일의 의미"
categories: ['Back-end', 'Nodejs']
tag: ['Nodejs']
toc: true
toc_sticky: true
---



<br>

## nodejs의 핵심 개념 정리

자바 스크립트에서 특정 변수나 객체를 보내기 위해 파일들끼리 서로 보내고 가져올 수 있는 require에 대해서 알아보자

<br>

만약 아래와 같은 animals.js 파일에 animals라는 배열을 가져오고 싶다고 하자.

```js
const animals = ['dog', 'cat']

module.exports = animals
```

이 변수를 main.js 파일에서 사용하고 싶다면 그냥 단지 해당 변수를 적기만 하면 없다고 에러를 띄우기 때문에 해당 변수를 가지고 있는 파일에서 특정 객체에 대해 export(수출, 내보내기)를 명시해 주어야 한다.

<br>

그래서 이 변수를 사용할 때는 이 변수가 필요하다는 의미로 require('파일 경로')를 사용한다.

그럼 이 변수를 main.js에 가져와 사용해 보자.

먼저 require를 사용하면 잘 가져와지는지 한 번 찍어 보도록 하겠다.

```js
console.log(require('./animals'))
// ['dog', 'cat']
```

원하는 모습대로 잘 가져오는 것을 볼 수 있다.

require를 사용할 때 주의해야 할 점에 대해서 알아보자.

1. require를 사용할 때는 항상 그 인자로 가져오고자 하는 파일의 상대경로를 적어주어야 에러가 발생하지 않을 수 있다.

위에서 봤던 것과 같이 `./animals` 처럼 인자에 적어주면 된다.

<br>

2. 하지만 node standard library에 있는 모듈(ex. http)은 절대경로를 지정해 가져와야 한다.

일례를 들어 node를 통해 간단히 서버를 열 수 있는 모듈 중 하나로 http 모듈을 가져오기 위해선 다음과 같이 작성하기만 하면 가져올 수 있다.

```js
const http = require('http')
```

<br>

3. 이 프로젝트 내의 다른 파일은 상대경로를 지정해 가져온다.(사실상 1번과 같은 말이다.)

하지만 예외 사항이 하나 있는데

4. 프로젝트 내의 `node_modules`에 있는 모듈은 절대경로를 지정해도 된다.

```js
const animals = require('animals')
console.log(animals)
// ['dog', 'cat']
```

이처럼 animals.js 파일을 node_modules 파일에 넣어두고 이를 가져오는 파일이 node_modules와 같은 디렉터리에 있다면 절대 경로를 지정할 수 있다.

이는 다음과 같은 코드를 통해 이해할 수 있을 것이다.

```js
const {path} = module
console.log({path})
```

module에는 사용할 수 있는 모듈 몇 개가 있는데 그 중 path는 절대 경로를 지정했을 때 가져올 수 있는 모듈들의 위치를 보여준다.

![image](https://user-images.githubusercontent.com/79521972/177002770-21510e96-9243-487d-b88a-12a6042aa8a8.png)

따라서 절대경로를 require에 지정하면 module.path의 경로 중 하나에서 해당 모듈이 있는지 검사해 가져온다.

이 때 가장 가까이 있는 node_modules 디렉터리를 순서대로 검사하여 가져오는 것이다. 

<br>

공식 문서에 의하면 require는 모듈을 가져오는 방식이기 때문에 사실상 require 앞에는 module.이 생략된 것으로 볼 수 있다.

- console.log(module.require('./animals'))

<br>

그렇다면 모듈이란 무엇을 말하는 것일까?

<br>

**모듈이란?**

각 파일 하나하나가 모듈인 셈이고 이 모듈을 가져올 수 있게 도와주는 함수가 require인 것이다.



<br>
모듈에는 크게 두 가지 방식이 존재한다.

하나는 node에서 사용하는 commonjs방식과, 나머지 하나는 ECMAScript의 모듈인 ESMA 모듈이다.

- CommonJs: require 함수 사용하는 방식

- ESMAScript: import, export 사용



<br>

### ECMAScript 방식

- animals.mjs

  - ```js
    const animals = ['dog', 'cat']
    
    export default animals
    ```

- main.js

  - ```js
    import animals from './animals.mjs'
    
    console.log(animals)
    // ['dog', 'cat']
    ```

위처럼 ECMAScript 방식으로 사용하기 위해선 내보낼 때는 export를 사용하고 이를 받을 때는 import로 받아오면 된다.

- 이 때 받아오는 파일의 경로를 명시할 때 `./animals.mjs`와 같이 확장자를 반드시 제대로 적어주어야 에러가 발생하지 않는다.



<br>

추가적으로 ECMAScript 방식에서는 CommonJs 방식도 import 할 수 있고 ESM(ES6 Module)표준 대로도 가져올 수 있다.



<br>

그렇다면 다음과 같은 상황을 생각하면서 require에 대해 이해해 보자.

<br>

```js
const animalA = require('./animals')
const animalB = require('./animals')
const animalC = require('./animals')

console.log(animalA, animalB, animalC)
// ['dog', 'cat'] ['dog', 'cat'] ['dog', 'cat']
```

위에서 알 수 있듯이 똑같은 모듈을 세 번 가져와 이를 모두 출력해 보면 모두 같은 결과가 나오는 것을 알 수 있다.

하지만 여기서 의문은 `이 세 객체가 모두 같은 것일까?`라는 것이다.

이를 확인해 보기 위해서 javascript 문법 `===`을 통해서 각각이 같은 객체인지를 알아보면

```js
console.log(animalA === animalB)
console.log(animalA === animalC)
// true
// true
```

이 결과를 통해 세 번의 require를 했음에도 각각의 객체는 모두 같음 객체임을 알 수 있다.



<br>

그런데 여기서 흥미로운 사실이 하나 있다.

위 코드가 몇 번 실행되는지 알아보기 위해서 animals.js 파일에서 출력하는 구문을 한 줄 추가해 보자.

- animals.js

  - ```js
    const animals = ['dog', 'cat']
    
    console.log('animals.js is executed')
    
    module.exports = animals
    ```

- main.js

```js
console.log(animalA === animalB)
console.log(animalA === animalC)
// animals.js is executed
// True
// True
```

위와 같은 결과로 알 수 있는 흥미로운 사실은 require을 세 번 했음에도 해당 모듈 즉, 해당 파일은 단 한 번만 가져와졌다는 사실이다.

- 같은 객체였던 사실과 일관성 있는 결과이므로 합리적인 결과이긴 하다.



<br>

## npm, yarn 등 패키지 매니저와 package.json

- npm
  - registry: 여러 노드 패키지를 다운 받으려면



- package.json
  - npm을 통해 모듈을 설치하면 package.json에 버전과 함께 해당 모듈이 설치 되었음을 표시하게 된다.
  - 이 때 표기된 버전은 대략적인 버전임을 `^`기호를 통해 암시할 수 있는데 이 때문에 실제 node_modules에 설치된 모듈의 버전과 package.json 파일에 표시된 버전은 다를 수도 있다.
    - Patch releases: `1.0` or `1.0.x` or `~1.0.4`
    - Minor releases: `1`  or `1.x` or `^1.0.4`
    - Major releases: `*` or `x`
  - npm update
    - Minor releases의 경우 npm update를 하면 현재 버전에서 minor(중간 숫자)가 최대로 올라갈 수 있는 버전으로 update 됨을 의미한다.
    - 그럼 Patch release로 바꾸면 npm update를 했을 때 가장 마지막 숫자가 올라가는 것은 직관적으로 알 수 있을 것이다.
  - npm에서는 버전은 예를 들어 5.0.0 과 같이 세 개의 숫자를 나누어 표기하게 되는데 이는 `Semantic Versioning`이라고 한다.
    - `MAJOR.MINOR.PATCH` 의 의미를 갖고 있음
    - MAJOR: 이전 버전과 맞지 않는 API 변화가 있을 때
    - MINOR: 똑같은 라이브러리를 버전을 올려서 쓰더라도 전혀 기존 동작에는 문제를 일으키지 않음(backward compatible 방식에서 기능을 추가했을 때)
    - PATCH: backward compatible 버그 fix를 할 때



- package-lock.json
  - 이곳에는 실제로 설치된 모듈의 정확한 버전이 명시되게 된다.
  - 그렇기 때문에 깃을 통한 프로젝트 협업 시 이 파일을 올림으로써 버전의 문제가 없이 원할한 프로젝트를 할 수 있도록 할 수 있다.



<br>

### 패키지를 설치하면

그래서 모듈을 설치하고 나면 node_modules 밑에 해당 모듈 이름에 상응하는 디렉터리가 하나 생기게 되고 그곳에는 여러 파일이 있지만 그 중에서도 index.js 의 내용을 보면 이젠 예상할 수 있듯이 module.export를 통해 어떤 특정한 것을 내보내는 것을 알 수 있을 것이다.

모듈마다 다르겠지만 보통은 어떠한 행위를 넘겨주고 싶을 것이기 때문에 행위 자체를 의미하는 함수가 들어있고 그 중에서도 Arrow function(화살표 함수)가 들어있는 경우가 다반사이다.

<br>

그렇기 때문에 `require('모듈이름')`을 하는 것과 `require('모듈이름/index')`을 하는 것은 정확히 일치한다.



<br>

### 개발 전용 패키지/실행 전용 패키지

우리는 종종 npm을 통하여 모듈을 설치할 때 `--save-dev`옵션을 주는 경우가 있는데 이를 통하여 설치를 한 후 package.json에서 보면 새로운 field(devDependencies)가 생겨 이곳에 해당 모듈과 그 모듈의 버전이 명시되게 된다.

이것의 의미는 개발하는 환경에서만 필요한 패키지라는 뜻이다.

반대로 그냥  dependencies에 명시된 모듈은 실제 실행 환경에서 필요한 패키지라는 의미이다.

<br>

예를 들어 eslint와 같은 패키지의 경우 코드를 작성할 때 도움을 주는 것이기 때문에 실제 실행환경에서는 이러한 패키지가 필요 없는 경우가 많다.

그런데 그렇게 계속 다운 받다보면node_modules의 용량이 너무 커지게 될 것이고 이는 실행 시간에 영향을 끼칠 수도 있기 때문에 실행 시에 불필요한 패키지는 따로 분류를 해두게 되는 것이다.

<br>

그래서 추후에 production을 위해 모듈을 설치할 때는 devDependencies에 있는 것은 제외한 채 설치가 되게 된다.

<br>

### scripts 필드

eslint를 잘 적용을 했다면 우리가 `var x = 1`이라는 코드를 작성했을 때 이에 대한 오류를 잡아낼 수 있는데 경우에 따라 어떤 환경에서는 eslint가 제대로 작동을 하지 않아 이 코드에 대해 어떠한 오류도 잡을 수 없을 것이다.

그렇기 때문에 이러한 문제 상황을 애초부터 merge가 되는 것을 막기 위해서 ci에서 여러가지 검사를 미리해서 merge를 했을 때 문제되는 상황을 막을 수 있다.

하지만 eslint는 vscode 환경에서만 linter가 작동을 잘 하기 때문에 실제로 ci 즉, 커맨드 라인을 통해서 linter가 모든 파일에 대해서 문제가 없는지 검사를 하지는 미지수이다.

이를 확인해 보기 위해선 다음과 같은 명령어를 통해 확인할 수 있다.

- `./node_modules/.bin/eslint src/\*\*/*
  - 'src 밑의 모든 파일을 검사해라'

이를 쳐보면 알겠지만 오류가 하나라도 있어도 linter는 오류를 찾아내어 merge를 막을 수 있게 된다.

<br>

방금까지 eslint의 binary 파일 사용법을 알아보았다. 그렇다면 scripts와 이것이 무슨 관계가 있는 것일까?

<br>

package.json에는 scripts 필드를 추가할 수 있는데 이는 유저 정의 스크립트를 의미한다.

```
{
	"scripts": {
		"hello": "echo Hello"
	}
}
```

예를 들어 위와 같이 추가를 하면 `npm run hello`를 커맨드에 입력했을 때 `echo Hello`라는 명령어가 실행되어 콘솔 창에 Hello라는 문구가 뜨게 될 것이다.

기본적으로 이런 구조에서 돌아가는 것을 말한다.

<br>

그런데 shell script 파일을 하나 별도로 만들어서 관리하면 되지 왜 굳이 package.json에 scripts를 추가했는지 의문이 들 수 있을 것이다.

여기에는 단순한 이유가 하나 있는데 바로 이전에는 node_module/.bin/eslint src/\*\*/* 를 통해 굉장히 복잡하게 입력했는데 scripts를 이용하면 다음의 코드만 입력하면

```
"lint": "eslint src/**/*"
```

npm run lint만 쳐도 linter 검사를 할 수 있게 되는 것이다.

<br>

### yarn

yarn은 npm의 대체제의 대표로 꼽히는 프로그램으로 초기에 가장 큰 sales point로 caching을 이용한 빠른 설치 속도와 특수한 커맨드를 들고 등장했지만 최근 npm도 속도의 개선이 이루어 졌기 때문에 사실상 거의 차이가 없다고 볼 수 있어 npm을 사용해도 되지만 yarn도 yarn만의 매력이 있기 때문에 이를 찾는 사람도 적지 않게 찾아볼 수 있다.


