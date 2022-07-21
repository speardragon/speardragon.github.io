---
layout: single
title: "[Nodejs] express 모듈로 서버 구축"
categories: ['BackEnd', 'NodeJs']
tag: ['Nodejs']
toc: true
toc_sticky: true
---



<br>

이번 시간엔 node.js에서 가장 대표적인 웹 프레임워크 중 하나인 express.js에 대해서 알아보도록 하겠다.

<br>

## Express란?

Express.js는 Node.js를 위한 빠르고 개방적이고 간결한 웹 프레임워크이며, python의 Django, Java의 Sprint bott와 같은 프레임워크들 처럼 javascript에서 사용하는 웹 프레임워크 중 하나이다.

또한 여러 코어 등을 비롯해 노드 웹 프레임워크 중에 가장 대표적인 것으로 가장 많이 쓰인다.

물론 express만이 답이다! 라는 것은 아니지만 express가 가장 대표적이기 때문에 express의 사용법만 잘 익혀 놓더라도 노드 기반의 다른 웹 프레임워크로 넘어가더라도 큰 무리 없이 적응 할 수 있을 것이다.

<br>

**express? 왜? ** 

Express.js 프레임워크를 사용해야 하는 이유는 웹서버를 구현할 때 이미 만들어진 것을 가져다 쓰기 때문에 코드의 양을 엄청나게 줄여줄 수 있고 이러한 장점으로 인해 코드의 가독성이 높아져 추후에 코드 유지보수를 쉽게 할 수 있게 하여 안전하고 튼튼한 웹서버를 만들어줄 수 있기 때문이다.

또한 npm(노드 패키지 매니저)를 통해 간단하게 설치가 가능하고 이를 통합합 서버 프로그램을 구현할 수도 있다.

마지막으로 Node.js에서 가장 많이 이용하는 템플릿 엔진인 EJS(Embedded JavaScript)를 이용할 수 있기 때문에 만들어 놓았던 EJS 템플릿을 그대로 사용 가능하다는 장점도 있기 때문에 express.js를 사용한다.

<br>

그렇다면 바로 코드로 뛰어 들어가 express에 대해 알아보도록 하겠다.

<br>

## 사전 세팅

먼저 express를 사용하기 위해 필요한 패키지들을 설치해 주어야 한다.

1. 가장 먼저 node.js 공식 홈페이지에서 원하는 버전의 node를 설치해 준다.

2. **npm**(Node Package Manager)라는 것을 통해 **노드로 만든 패키지의 관리**를 더욱 편하게 할 수 있다.

   - ```
     npm init
     ```

   - 프로젝트를 진행할 폴더를 만들고 해당 폴더 위에서 위와 같은 명령어를 입력한다.(설치는 노드를 설치할 때 달려왔기 때문에 별도의 설치는 필요없음)

   - 그러면 package.json 파일이 생겼을 것이다.

     - 앞으로 설치하는 모듈은 모두 이곳에 명시될 것이고 원하는 scripts 를 이곳에서 만들어 사용할 수도 있다.

3. express 모듈 깔기

   - ``` 
     npm install express
     ```

   - 설치 된 모듈이 제대로 설치 되었는지를 확인하기 위해선 프로젝트 디렉터리 위치에 package.json 파일에 dependencies 필드로 명시되었는 지를 확인하면 된다.

   - 이 과정을 통해 node_modules 폴더가 생성되고 이 안에는 실제로 설치한 모듈들이 저장되어 관리 되는 곳이라고 보면 된다.



<br>

2. @types/express 깔기

```
npm install --save-dev @types/express
```

타입 도움을 받기 위해 사용하는 타입스크립트를 지원하는 express는 실제 프로덕션 서버에 존재할 필요가 없는 패키지 이기 때문에 `--save-dev`와 같은 옵션을 추가하여 install한다.

<br>

추가적으로 eslint, prettier, ts-node 등이 깔려 있어야 한다.(이 과정의 설명은 생략 함.)

<br>

## 세상에서 제일 간단한 웹 서버 구축해 보기

설치가 완료되었으면 이제 express 모듈을 사용할 수 있다.

```js
const express = require('express')
```

위와 같은 코드는 타입스크립트에 의해 @types/express 모듈을 가져와주도록 하여 express 변수에 저장한다.

<br>

원래는 express를 사용하지 않았다면 nodejs의 내장 모듈인 http를 사용하여 구현했을 것인데 기본적인 골격은 http를 사용하는 것과 그렇게 차이가 나지 않는다.

<br>



```js
// @ts-check

/* eslint-disable no-console */

const express = require('express')

const app = express()

const PORT = 5000

app.use('/', (req, res) => {
  res.send('Hello express!')
})

app.listen(PORT, () => {
  console.log(`The Express server is listening at port: ${PORT}`)
})

```

위 코드는 src/ 디렉터리 밑에 있는 main.js 파일을 만들어 해당 파일에 내용을 작성한 것이다.

위 코드의 특징을 하나하나 뜯어보자면 다음과 같다.

- ts-check: vscode에서 타입스크립트를 사용하기 위해 작성하는 주석으로 자바 스크립트에서 **TypeScript** 분석을 켜려면 **vs code** 에디터상에서 @ts-check라는 텍스트가 있는 주석을 파일의 시작 부분에 추가하기만하면 된다. 그런 다음 파일의 아무 곳에나 **TypeScript**의 **JSDoc** 유형의 주석을 추가하면 된다.
- require를 통해 express 모듈을 가져와서 app이라는 변수를 통해 이 모듈을 사용할 수 있도록 하였고 가장 간단하게 서버를 열기 위해선 listen을 통해 PORT 번호와 서버 측에서 실행할 콜백 함수를 지정하면 서버가 실행이 된다.
  - require에 대해 자세히 알고 싶다면 전 포스트 [http](http) 이곳을 참고하길 바란다.

- app.use 부분은 가장 간단한 기능을 추가해 보기 위해 사용한 것으로 서버에 접속한 클라이언트 측에 Hello express 라는 문자열을 보내주는 역할을 한다.

<br>

클라이언트 입장에서 해당 서버를 접속해야 서버가 제대로 동작하는지 아는데 이를 위해선 다음과 같은 방법들이 있다.

- httpie
  - python 으로 개발된 콘솔용 http client 유틸리티로 curl 대신 http 개발 및 디버깅 용도로 사용 가능하며 다음과 같은 특징이 있다.
  - 콘솔을 통해 다운 받을 수 있다.
- postman
  - [https://www.postman.com/](https://www.postman.com/)
  - 위 공식 홈페이지를 통해 다운 받을 수 있고 이를 사용하면 대상 URL에 요청할 메소드를 지정할 수 있고, 호출에 필요한 파라미터의 전달 방식을 설정할 수 있으며, 결과 또한 빠르게 확인이 가능하다.
- 주소입력(로컬)
  - 로컬 서버를 구축한 경우 localhost:포트번호 와 같이 아무 웹브라우저에서 해당 주소를 입력하여 서버를 확인하는 경우이다.

세 경우 중에서 첫 번째와 두 번째 방식을 사용하는 것을 추천하며 서버의 크기가 커질 수록 혹은 협업을 통한 프로젝트를 진행한다면 API를 만들었을 때 postman을 통해 더욱 쉽고 빠르게 확인이 가능하다고 생각한다.



<br>

이렇게 가장 간단한 웹서버를 구현해 보았고 이제 여기에 기능을 하나하나 추가해 보면서 조금 더 구체적인 부분에 대해 알아보도록 하겠다.

<br>

## nodemon

서버의 API를 만들다 보면 수정 사항이 잦을 것인데 서버에 수정사항이 제때 제때 반영되지 않는다면 서버를 계속 껐다 켰다 하면서 확인하는 번거로운 과정을 거쳐야 할 것이다.

그렇기 때문에 저장만 하면 바로 반영이 되게 해 주는 모듈인 nodemon을 설치하여 이를 해결하도록 하자.

```
npm install --save-dev nodemon
```



<br>

nodemon을 설치하고 이를 통해 서버를 키기 위해선 `nodemon src/main.js`와 같은 명령어를 입력하면 되는데 이는 꽤 긴 명령어 이기 때문에 간결하게 `npm run server` 와 같이 입력하면 해당 기능이 수행되도록 package.json의 scripts 필드에 server 요소로 추가를 해 주도록 하자.

```json
{
	"scripts": {
        "server": "nodemon src/main.js"
    },
    ...
}
```



<br>

## middleware

express는 http 기본 모듈을 활용할 때보다 더 빠르게 작업을 할 수 있는데 이것들을 위한 일종의 preset들을 제공하고 있는 것이고 자주 하는 행동들에 대해서 미리 만들어져 있는 기능이 있고 이를 middleware라고 하며 이것만 잘 사용하면 금방금방 여러 요청에 대응할 수 있으며 다양한 시나리오에도 올바르게 대처할 수 있게 된다.

<br>

**middleware란?**

middleware는 express가 실행되면서 어떤 request(통칭: req)가 들어왔을 때 req가 응답이 되어 나갈 때까지 거치게 되는 모든 함수들을 뜻한다고 생각하면 된다.

정리하자면 express는 여러 함수들을 거치는 형태이고 각 함수 하나하나를 중간에 거치기 때문에 middleware인 것이다.

- 즉, 클라이언트가 요청을 하고, 서버가 거기에 응답하는 과정 중간에서 뚝딱뚝딱 여러 가지 일을 해 주는 함수라고 할 수 있다.

<br>

사용자가 웹사이트에 접속하면 index.js가 실행되고, route(경로)를 살펴볼 것이다. 그 후에는 콜백 함수를 실행할텐데 이처럼 중간에 실행되는 함수가 middleware이다.

<br>

한 번 다음과 같은 코드를 앞선 main.js에 추가적으로 작성해 보자.

```js
app.use('/', (req, res) => {
  console.log('Middleware 1')
  res.send('Hello express!')
})

app.use((req, res) => {
  console.log('Middleware 2')
  res.send('Hello, express!')
})
```

이 코드를 작성하고 저장을 한 뒤에 다시 서버에 접속해 보면 서버 측 콘솔 창에는 `Middleware 2 `만 띄워지는 것을 확인할 수 있을 것이다.

<br>

### next() 함수

middleware는 받는 인자가 정해져 있다.

- req, res와 더불어 next가 존재한다.

노드는 대부분의 작업이 비동기로 처리된다. 

그렇기 때문에 언제 어떤 작업이 끝날 지를 정확히 알지 못한다. 그래서 작업이 끝났을 때 명시적으로 알려주어야 하는 부분이 대부분인데 middleware 안에서는 굉장히 다양한 비동기 작업이 일어날 수 있기 때문에 이 middleware가 끝나고 나서 다음 middleware한테 실행을 넘기기 위해서 next() 함수를 사용한다.

그래서 앞선 코드가 원하는대로 동작하게끔 하기 위해선 다음과 같이 수정을 하면 된다.

```js
app.use('/', (req, res, next) => {
  console.log('Middleware 1')
  next()
})

app.use((req, res) => {
  console.log('Middleware 2')
  res.send('Hello, express!')
})
```

그러면 올바르게 서버 측에 Middleware1 과 Middleware2가 순서대로 출력이 되는 것을 볼 수 있을 것이다.

하지만 여기서 next()를 뺀다면 클라이언트 측에서는 무한 로딩에 걸리게 되는데 그 이유는 middleware가 언제 끝나는 지는 next()에 의해 결정되는 것이기 때문에 next()가 호출되기 전에는 middleware의 실행이 끝나지 않았다고 보는 것이기 때문이다.

- 그래서 둘 중 하나가 time out이 날 때까지는 계속 기다리게 되는 것이다.

<br>

추가적으로 위 코드에서 res.send와 더불어 next()가 필요하지 않았던 이유는 res.send나 res.json의 경우 모든 response stream에 대한 writing 과정이 끝나고 나서 response를 보내야 하기 때문에 별도의 next()를 필요로 하지 않기 때문이다.

<mark>하지만 response를 보내고 나서 추가적인 processing을 하고 싶다면 반드시 next() 함수를 호출해야 한다는 것을 잊지 말아야 한다.</mark>

<br>

next() 함수를 호출하면 앱 내의 그 다음 middleware 함수(콜백 함수)가 호출된다. 

- 원래는 하나의 middleware(콜백 함수)만 실행하고 멈추게 된다.

우리는 원하는 만큼 미들웨어 함수를 만들어서 여러 가지를 해볼 수 있다. 예컨데, 유저의 로그인 여부를 체크한다거나, 접속에 대한 로그를 기록한다던가와 같은 것들 말이다.

<br>

그런데 특정 함수(아래 예제에서 betweenHome)는 특정 화면에서만 동작하는데, 예를 들어 /profile로 접속했을 땐 실행되지 않는 경우도 있다.

그렇다면 어떤 루트로 들어오든지 간에 middleware를 실행시키고 싶을 땐 어떻게 해야할까?

바로 위에서 사용한 use를 사용하면 된다.

```js
app.use(betweenHome);
app.get('/', handleHome);
app.get('/profile', handleProfile)
```

이런식으로 해 두면 위에서 아래로 실행이 되면서, betweenHome에서 next()를 실행시켰다는 가정 하에 betweenHome -> next()를 거쳐 내가 원하는대로 동작하게 된다.

그렇기 때문에 루트 처리 이전에 원하는 만큼 middleware를 배치하면 될 것이다.

<br>

### next()가 비동기적으로 불린다면?

그러나 next()가 비동기적으로 불리는 시나리오를 생각해 보자.

```js
app.use('/', (req, res, next) => {
  console.log('Middleware 1')
  
  setTimeout(() => {
      next()
  }, 1000)
    
})

app.use((req, res) => {
  console.log('Middleware 2')
  res.send('Hello, express!')
})
```

```
Middleware1 
Middleware2
```



위 코드에서 1초가 지나고 난 뒤에 아래 Middleware2가 실행이 되어 종료가 될 것인데 이를 통해 미들웨어가 위에서부터 실행이 쭉쭉 아래로 이어지는 것을 알 수 있다.

<br>

정말 그 말이 맞는지 두 코드의 순서를 바꿔서 확인해 보면 더 명확해 질 것이다.

```js
app.use((req, res) => {
  console.log('Middleware 2')
  res.send('Hello, express!')
})

app.use('/', (req, res, next) => {
  console.log('Middleware 1')
  
  setTimeout(() => {
      next()
  }, 1000)
    
})
```

```
Middleware2
```

이 코드를 보면 알 수 있듯이 첫 번째 미들웨어를 지나고 다음 미들웨어는 건들지도 않은 채 그냥 끝나버리는 것을 볼 수 있다.



<br>

또한 미들웨어는 계속해서 작성을 해도 next()로만 잘 이어지게 한다면 쭉쭉 실행이 되게도 할 수 ㅣ있다.

```js
app.use(
    '/', 
    (req, res, next) => {
  		console.log('Middleware 1-1')
  
  		setTimeout(() => {
    	next()
  		}, 1000)
	},
    (req, res, next) => {
  		console.log('Middleware 1-2')    
        next()
})

app.use((req, res) => {
  console.log('Middleware 2')
  res.send('Hello, express!')
})
```

```
Middleware1-1
Middleware1-2
Middleware2
```

<br>

### 다음 middleware로 객체 전달

미들웨어를 통해 앞선 함수에서 실행된 결과를 뒷쪽 미들웨어로 넘겨주는 경우가 많을 것인데 이를 처리하는 logic에 대해서 알아보도록 하겠다.

```js
// @ts-check
/* eslint-disable no-console */

const express = require('express')

const app = express()

const PORT = 5000

app.use('/', (req, res, next) => {
  console.log('Middleware 1')
  const requestedAt = new Date()
  // @ts-ignore
  req.requestedAt = requestedAt

  next()
})

app.use((req, res) => {
  console.log('Middleware 2')
  // @ts-ignore
  res.send(`Hello, express!: Requested at ${req.requestedAt}`)
})

app.listen(PORT, () => {
  console.log(`The Express server is listening at port: ${PORT}`)
})

```

위 코드에서 보면 알겠지만 `req.객체이름`와 같이 사용하고 이를 통해 앞쪽 미들웨어의 정보를 저장하고 다음 미들웨어에서 이를 꺼내서 사용할 수 있도록 한 것이다.

이는 매우 유용하기 때문에 잘 알아두어야 하는 부분이다.



`// @ts-ignore`는request 객체에 원래는 없던 값을 지금 임의로 정의를 하려다 보니까 typescript 에러가 나게 되는데 현재는 연습으로 하고 있는 것이라 무엇을 하려는 지 분명히 알고 있기 때문에 무시를 하기 위해 작성한 것이다.

<br>

### async middleware

다음은 async 미들웨어를 사용하는 방식이다.

```js
// @ts-check

/* eslint-disable no-console */

const express = require('express')
const fs = require('fs')

const app = express()

const PORT = 5000

app.use('/', async (req, res, next) => {
  console.log('Middleware 1')

  const fileContent = await fs.promises.readFile('.gitignore')
  const requestedAt = new Date()

  // @ts-ignore
  req.requestedAt = requestedAt
  // @ts-ignore
  req.fileContent = fileContent
  next()
})

app.use((req, res) => {
  console.log('Middleware 2')
  // @ts-ignore
  res.send(`Requested at ${req.requestedAt}, ${req.fileContent}`)
})

app.listen(PORT, () => {
  console.log(`The Express server is listening at port: ${PORT}`)
})

```

```
Requested at Mon Jul 04 2022 01:25:11 GMT+0900 (대한민국 표준시), node_modules
.env
```

async 미들웨어를 통해 promises 객체를 await으로 받아올 수 있는 것도 확인 할 수 있다.
