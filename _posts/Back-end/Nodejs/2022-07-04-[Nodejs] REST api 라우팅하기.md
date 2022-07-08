---
layout: single
title: "[Nodejs] express 모듈로 서버 구축"
categories: ['Back-end', 'Nodejs']
tag: ['Nodejs']
toc: true
toc_sticky: true
---

<br>

## REST API 라우팅하기

- 지난 시간 코드

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



<br>

지난 시간에는 루트 경로로 들어오는 라우팅만 동작하도록 처리를 했었는데 이제 이것들이 어떤 주소로든 들어와도 처리되도록 할 수 있다.

<br>

### 라우팅(Routing)이란?

라우팅이란 네트워크에서 사용하는 용어로 어떤 네트워크가 있을 때 이 안에서 통신되는 데이터를 보낼 경로를 선택해내는 과정을 말한다. 이는 HTTP 메소드(GET, POST 등...)를 어떤 걸 사용하느냐에 따라 달라질 수 있는데 해당 기능을 하는 함수를 바로 사용할 수도 있다.

```js
app.get('')
app.post('')
```

<br>

간단한 예로 get과 post 메소드를 사용해 보자.

```js
...

app.get('/', (req, res) => {
  res.send('Root - GET')
})

app.post('/', (req, res) => {
  res.send('Root - POST')
})

app.listen(PORT, () => {
  console.log(`The Express server is listening at port: ${PORT}`)
})
```

일단 루트 경로에 대해 처리를 하도록 했고 각각 HTTP 메소드가 GET일 때와 POST일 때 response로 string을 반환해 보았다.

httpie나 postman을 통해 확인해 보면 각각의 메소드로 요청을 보냈을 때 알맞은 문구가 뜨는 것을 확인할 수 있을 것이다.

<br>

## path

path 즉, 경로에는 여러가지 방식으로 작성할 수가 있는데 그 종류에 대해서 한 번 알아보자.

<br>

- 문자열 패턴

  - ```js
    app.get('/ab?cd', function(req, res) {
      res.send('ab?cd');
    });
    ```

    - 문자열 패턴을 기반으로 하는 라우트 경로의 모습이며 위의 라우트 경로는 acd 및 abcd와 일치한다.
    - 물음표 전에 온 문자에 대해서는 있을 수도 있고 없을 수도 있다는 의미.

  - ---

    ```js
    app.get('/ab+cd', function(req, res) {
      res.send('ab+cd');
    });
    ```

    - 위의 라우트 경로는 abcd, abbcd 및 abbbcd 등과 일치한다.
    - \+ 전에 온 문자에 대해서는 해당 문자가 여러번(**한 번 이상**) 올 수 있다는 의미.

  - ```js
    app.get('/ab*cd', function(req, res) {
      res.send('ab*cd');
    });
    ```

    - 위의 라우트 경로는 abcd, abxcd, abRABDOMcd 및 ab123cd 등과 일치한다.
    - \* 의 의미는 해당 부분에는 종류와 길이에 상관없이 어떤 문자열이든 올 수 있다는 의미.

  - ```js
    app.get('/ab(cd)?e', function(req, res) {
     res.send('ab(cd)?e');
    });
    ```

    - 위의 라우트 경로는 /abe 및 /abcde 와 일치한다.
    - 괄호를 묶으면 그룹으로 생각하면 된다.(+, ? 와 같은 기호가 한 번에 적용됨)

- 정규식 기반(/something/) -> /로 감싼 형태

  - 정규식으로 표현할 때는 따옴표를 넣지 않는다!

  - ```js
    app.get(/a/, function(req, res) {
      res.send('/a/');
    });
    ```

    - 위의 라우트 경로는 라우트 이름에 "a"가 포함된 모든 항목과 일치한다.
    - 반드시 a는 포함해야 한다는 의미.

  - ```js
    app.get(/.*fly$/, function(req, res) {
      res.send('/.*fly$/');
    });
    ```

    - 위의 라우트 경로는 butterfly 및 dragonfly와 일치하지만 butterflyman 및 dragonfly man 등과는 일치하지 않는다.
    - ~~로 끝나야 한다는 $를 사용하여 사용할 수 있음

  - ```js
    app.get(/^\/abcd$/, (req, res) => {
      res.send('/^\/abcd$/')
    })
    ```

    - 위의 정규표현식대로 라우팅 경로를 지정하면 딱 저 문자열(/abcd)과 정확히 일치하는 uri만을 받도록 할 수 있다.

- 문자열 - 여러 uri 지정

  - ```js
    app.get(['/abc', '/xyz'], (req, res) => {
      res.send('/^\/abcd$/')
    })
    ```

    - 위처럼 여러 uri를 받고 싶을 때는 배열에 여러 uri를 넣어 표현할 수도 있다.
    - 물론 배열의 원소로 정규표현식을 사용하는 것도 가능하다.



<br>

### prefix



```js
app.get('/users', (req, res) => {
  res.send('User list')
})

app.get('/users:id', (req, res) => {
  res.send('User info with ID')
})

app.post('/users', (req, res) => {
  // Register user
})
```

위 코드는 총 세 개의 API를 구현해 본 것으로 위에서부터 각각 User list를 GET하는 API, 특정 id를 가진 user의 정보를 GET 하는 API, 새로운 user를 등록(POST)하는 API이다.

이 때는 모두 /users 라는 경로로 들어가야 하기 때문에 prefix가 동일하다고 볼 수 있고 이처럼 prefix를 공유하는 경우에 Router 기능을 사용할 수 있다. 

router 역시 일종의 미들웨어이다.





<br>

위 코드를 바꾼 코드는 다음과 같이 될 것이다. (Router를 적용한 코드)

```js
const userRouter = express.Router()

userRouter.get('/', (req, res) => {
  res.send('User list')
})

userRouter.get('/:id', (req, res) => {
  res.send('User info with ID')
})

userRouter.post('/', (req, res) => {
  // Register user logic
  res.send('User registered')
})
```

그러면 이 userRouter라는 라우터는 /users 라는 prefix에만 반응을 하도록 해 주어야 하기 때문에 다음과 같은 구문이 추가 되어야 한다.

```js
app.use('/users', userRouter)
```



<br>

### path

- path variable: `:` 
  - 지정한다.
  - 특정한 값을 지목해서 처리하는 방식
  - ex) userId = 1, videoId = 123 과 같은 데이터를 원할 때
  - 경로에 존재하는 내용이 없을 시
  - 404 Error 발생
    - resource를 식별해야 하는 경우에 적합
- query-string: `?`
  - 일종의 필터링
  - 필터링을 활용한 처리
    - ex) 활성화 상태인 동영상을 원할 때
  - 데이터가 없는 경우
  - 빈 리스트가 나옴. => 추가적인 예외 처리 필요
    - 정렬, 필터링을 해야 하는 경우에 적합



<br>

path variable을 통해 :id 를 path uri에 지정하지 않아도 123과 같은 숫자만 적어도 처리가 될 수 있다.

무슨 말이냐 하면 다음과 같은 코드를 한 번 생각해 보자.

```js
const USERS = {
  15: {
    nickname: 'foo',
  },
}

userRouter.param('id', (req, res, next, value) => {
  console.log('id parameter', value)
  // @ts-ignore
  req.user = USERS[value]
  next()
})

userRouter.get('/:id', (req, res) => {
  console.log('userRouter get ID')
  res.send(req.user)
})

userRouter.post('/', (req, res) => {
  // Register user
})

app.use('/users', userRouter)

```

USERS라는 유저 정보가 저장되는 객체가 있다고 생각하고 request uri에 :id 에 해당하는 것이 있다고 판단이 되면 userRouter.param에는 'id'라는 인자를 받는 것을 대기하고 있으므로 해당 구문에 걸리게 되고,

이 때 걸린 id라는 값은 value라는 parameter로 받아서 제대로 받아왔는지 server console에 이 값을 찍어주고 해당 id에 해당하는 정보가 USERS객체에 존재하면 이 값을 req.user로 넘겨주어 다음 미들웨어에서 서버 콘솔에 ID를 받았다는 문구를 출력하고 client 측으로 req.user 값을 send한다. 

이 때 param 메소드에서 next()를 사용한 이유는 res.send와 같이 response를 해 주어 다음 미들웨어로 넘겨주는 일련의 과정이 생략되고 단지 req.user에 값만 넘겨주었기 때문에 따로 next()를 넣어주어 정삭적으로 다음 미들웨어로 넘어갈 수 있도록 한 것이다.

<br>

```
// server
id parameter
userRouter get ID
```

<br>

```
// client
{
    "nickname": "foo"
}
```

<br>

재밌는 사실은 client가 send를 통해 받은 응답의 타입이 원래는 string 객체로 넘겨주었는데 req.user를 넘겨주니까 content-type을 보면 application/json으로 되어있는 것을 확인할 수 있고 이는 express가 자동으로 json 형식으로 바꾸어 준 것으로 보아 express의 기능이 좋다는 것을 다시 한 번 느낄 수 있을 것이다.

<br>
그렇다면 이제 id와 nickname을 클라이언트로부터 받아서 server측에 추가(POST) 해 주는 기능을 추가해 보도록 하겠다.

<br>
그렇다면 우리가 가장 먼저 기대할 수 있는 사실은 req.body의 형식이 다음과 같다는 것을 알 수 있다.

```
req.body = {"nickname": "bar"}
```





<br>

다음과 같이 코드를 먼저 구현해 보고 이를 토대로 user를 추가해 보도록 하겠다.

```js
userRouter.post('/:id/nickname', (req, res) => {
  // req.body = {"nickname": "bar"}
  const { user } = req
  const { nickname } = req.body
  // @ts-ignore
  user.nickname = nickname

  res.send(`User nickname updated: ${nickname}`)
})
```

```
http POST localhost:5000/users/15/nickname nickname=bar
```

위와 같이 접속을 하여 추가를 하려고 하면 오류가 나는 것을 볼 수 있는데

<br>

```
Cannot destructure property 'nickname' of 'req.body' as it is undefined.
```

req.body가 undefined인데 거기서 nickname이라는 property를 가져오려고 해서 오류가 난 것이다.

이를 해결하기 위해서는 body parser라는 모듈을 통해 해결할 수 있다.

<br>

**Why 'body parser?'**

지금 express 자체에는 어떤 요청이 오던지 간에 body를 parsing하고 있지는 않다. 

지금 상황은 application/json 형태로 POST 요청이 온 것인데 이것에 대해서 어떠한 일관적 반응을 하도록 되어 있지는 않는다는 뜻이다. 그런데 body parser를 사용하여 이 문제를 해결할 수 있기에 해당 모듈을 사용하는 것이다.

<br>
body parser 모듈을 다운받기 위해서는 다음과 같은 명령어를 입력하면 된다.

- 현재 실행 중인 서버는 Ctrl-C를 눌러서 잠시 종료 시킨 뒤에 설치하도록 한다.

```
npm install body-parser
```

<br>

설치가 완료되었으면 `const bodyParser = require('body-parser')`를 통해 body parser를 가져오는 부분을 추가해 주도록 한다.

<br>

그리고 이 bodyParser를 적용하기 위해서는 미들웨어를 또 끼우면 된다. 즉, `app.use(bodyParser.json())` 구문을 추가하면 된다.

<br>
그래서 이번에 다음과 같이 POST 요청을 하게 되면 올바르게 수정사항이 변경되도록 할 수 있다.

```
http POST localhost:5000/users/15/nickname nickname=bar
```

이를 확인해 보기 위해선 다음과 같이 입력한다.

```
http localhost:5000/users/15
```

그러면 15번에 해당하는 유저의 정보가 json 형태로 응답을 받게 되고 해당 객체 안의 nickname 부분이 'foo' 에서 'bar'로 바뀐 것을 볼 수 있다.

<br>

\+ 그러나 최근 express에서 body parser를 제공하여 body parser 모듈을 사용하지 않고 다음과 같이 하면 body가 parsing되어 코드가 정상적으로 실행되도록 기능이 추가되었다.

```js
app.use(express.json())
```





