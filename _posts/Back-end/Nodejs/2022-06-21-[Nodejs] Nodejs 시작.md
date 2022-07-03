---
layout: single
title: "[Nodejs] Nodejs 시작"
categories: ['Back-end', 'Nodejs']
tag: ['Nodejs']
toc: true
toc_sticky: true
---



<br>

## JavaScript식 비동기 처리 방식

- 전과 차이점

  - 끝나기 전까지 다음 줄로 절대 넘어갈 수 없었는데

  - 일단 다 요청을 보내버리고 응답이 올 때마다 하나씩 대응(No more blocking!)

- 저수준의 오래 걸리는 일은 Node에게, 고수준의 로직은 메인 스레드에서.
  - Node가 빠른 속도와 매우 높은 확장성을 갖는 근본적인 이유

### 저수준 처리의 개선

- C와 WebAssembly(기계어로 컴파일 하기 때문에 매우 빠름, 최적화 효율을 끌어올림)
  - javascript의 단점: 저수준 처리는 Node가 빠르게 처리하기 매우 어려운 부분이다.
  - Nods.js는 C와 WebAssembly 모듈을 바인딩해 사용하는 방법을 제공하고 있다.
  - C는 node-gyp를 통해, WebAssembly는 node 12 버전부터 제공되고 있다.
  - 이를 통해 저수준 처리를 해결

<br>

### npm

- 방대한 오픈 소스 생태계
  - 방문자수



<br>

### Glitch

node 서버를 바로 띄워서 구현 가능한 사이트



<br>

package.json: 사용하는 의존성을 나열하는 점이 중요함



Type checking

- 다른 언어에서는 미리 compile을 해두어 type checking을 하기 때문에 걱정이 없지만 js에서는 실행되는 순간에서야 발견하게 된다.

<br>

### Typescript

자바 스크립터로 변환되는



TypeScript가 node 환경에서도 동작하게 하려면 types node package를 설치해주어야 한다.





### [nodemon] app crashed

else문을 빠져 나왔는데도 statusCode가 있고  res.end를 하는 경우



<br>

응답의 타입을 JSON 문서로 보내고 있기 때문에 브라우저에서든 어디에서든 이를 쉽게 알아보기 위해서는 json 형태로 content type이 내려간다는 것을 알려주는 것이 좋다.

```javascript
res.setHeader("Content-Type", "application/json; encoding = utf-8")
```

그래서 위와 같은 코드를 추가해 주면 아래와 같은 형태로 응답을 받을 수 있는 것을 확인할 수 있다.

```json
{
    "posts": [
        {
            "id": "my_first_post",
            "title": "My first post"
        },
        {
            "id": "my_second_post",
            "title": "나의 두번째 포스트"
        }
    ],
    "totalCount": 2
}
```



<br>

```
http POST localhost:4000/posts title=foo content=bar --print=hHbB
```

--print의 내용을 hHbB로 하였는데 소문 h와 b는 각각 응답의 header와 body를 의미하고 대문자 H와 B는 각각 요청의 Header와 Body를 의미한다.

그래서 보낸 것만 보고 싶을 때는 --print=HB로 설정하고  
받은 것만 보고 싶을 때는 --print=hb로 설정하면 된다.



<br>

## REST API와 HTTP 메서드

REST API는 HTTP 메서드를 사용한 방식이라고 할 수 있다.

- 메서드(method): ex) GET, POST
- URI: 행위의 목적 -> ex) /users/userId/youtube

<br>

### 메서드의 종류

- **GET** : 자료를 **요청**할 때 사용
- **POST** : 자료의 **생성**을 요청할 때 사용
- **PUT** : 자료의 **전체의 수정**을 요청할 때 사용
- **PATCH** : 자료의 **일부의 수정**을 요청할 때 사용
- **DELETE** : 자료의 **삭제**를 요청할 때 사용

<br>

### GET과 POST의 차이

- GET: only Header 정보만 필요하다. 
  - 어떠한 값을 조회할 것인지에 대한 정보만 필요하기 때문에

- POST: Header, Body 둘 다 필요하다.
  - 어디에 무엇을 저장할지에 대한 정보와 Body에 넣을 정보도 필요하다.

 DELETE 기능은 가능한 사용하지 않는다. 데이터 하나하나는 모두 기업에게는 귀중한 자산이기 때문에 함부로 삭제하지 않기 위함이다.

그렇기 때문에 API를 구현함에 있어서 회원 탈퇴 등을 처리할 때 PATCH나 PUT 등을 통해 status 즉 상태를 변화시키는 방향으로 진행하는 것이 좋다.



<br>

### URL, URI의 2 가지 형식

- path variable: `:` 
  - 지정한다.
  - 특정한 값을 지목해서 처리하는 방식
  - ex) userId = 1, videoId = 123 과 같은 데이터를 원할 때
- query-string: `?`
  - 일종의 필터링
  - 필터링을 활용한 처리
    - ex) 활성화 상태인 동영상을 원할 때



<br>

### 데이터의 2가지 형태

- XML: HTML 형태와 같이 태그 형식으로 된 데이터를 주고 받는다.

  - ```xml
    	<SafeAreaView>
        	<Button title='home'
                onPress={() => Alert.alert('home pressed.', 'message')} />
        </SafeAreaView>
    ```

- JSON: key, value 형태로 된 데이터를 주고 받는다.

  - ```json
    {
      "compilerOptions": {
        "strict": true
      },
      "include": ["src/**/*"]
    }
    
    ```

<br>



### 도메인 폴더 구조

- Route - Controller - Provider/Service -DAO

  - Route: Request에서 보낸 라우팅 처리해준다.
  - Controller: Request를 처리하고 Response 해주는 곳이다. (Provider/Service에 넘겨주고 다시 받아온 결과값을 형식화), 형식적 Validation
  - Provider/Service: 비즈니스 로직 처리한다. 의미적 Validation
  - DAO: Data Access Object의 줄임말. Query가 작성되어 있는 곳이다.

   

![image](https://user-images.githubusercontent.com/79521972/176808720-0f3ca066-39d9-43f7-a78d-db6a935998a6.png)

<br>

- Provide: Read의 비지니스 로직 처리
- Service: Create, Update, Delete의 로직 처리

와 같은 식으로 처리할 수도 있다.

<br>

### Validation 처리

서버 API 구성을 할 때에는 Validation 처리를 잘해야 한다.

외부에서 어떤 값을 날리는지(오류가 발생할만한)와 상관없이 Validation만 잘 처리하면 서버가 다운되는 일은 없을 것이다.

보통, 

-  값, 형식, 길이 등의 **형식적 Validation**은 **Controller**에서 처리한다.
-  DB에서 검증해야 하는 **의미적 Validation**은 **Provider** 혹은 **Service**에서 처리한다.



<br>

### --save-dev

실제 프로덕션 서버에 존재할 필요가 없는 패캐지는 이와 같은 옵션을 추가하여 install한다.



<br>

무엇이든지 마찬가지이지만 데이터 구조와 관련된 것을 확인하기 위해서는 여타 검색을 통해 나온 블로그보다도 node.js 공식 문서를 보는 것이 제일 정확하다.

## Buffer

파일은 어디까지나 바이스 시퀀스(byte sequence)이기 때문에 이진파일과 다를 바 없을 것이고 이를 출력을 하면 Buffer의 형태로 결과가 출력 될 것이다.

이를 확인해 보기 위해서 test라는 파일에 확장자가 없이 Hello라는 문구를 적고 main.js 파일에서 이 파일을 불러와 출력을 해 보면 이해가 될 것이다.

- main.js

  - ```js
    const fs = require('fs')
    
    const result = fs.readFileSync('src/test')
    console.log(result)
    // <Buffer 48 65 6c 6c 6f>
    ```

버퍼에 들어간 내용을 보면 hexadecimal로 표현된 형태들이 나열된 것을 볼 수 있다.

또한 이 문자들은 아스키코드 값과 정확히 일치한다.



<br>

## stream

stream은 Buffer와 함께 양대 산맥을 이루는 데이터 타입으로 서버에서 대용량 데이터를 사용한다면 무조건 쓰일 수 밖에 없는 데이터 타입이다.

```js
const fs = require('fs')

const stream = fs.createReadStream('src/test')

stream.pipe(process.stdout)
```

데이터의 파이프와 같은 것으로 a에서 b로 꼽을 수 있고 a에서 c로 꼽을 수 있고 b로 꼽았던 것을 c로 꼽을 수 있고...

이처럼 자유자재로 스트림 파이프를 마음대로 조절할 수 있기 때문에 데이터의 흐름을 제어할 수 있는 역할을 하고 그렇기 때문에 RAM에 모든 데이터를 다 들고 있지 않아도 처리를 할 수 있는 구조를 갖고 있다.

