---
layout: single
title: "[Nodejs] Nodejs 시작"
categories: ['Back-end', 'Nodejs']
tag: ['Nodejs']
toc: true
toc_sticky: true
---



## JavaScript식 비동기 처리 방식

콜백



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

### 

package.json: 사용하는 의존성을 나열하는 점이 중요함



Type checking

- 다른 언어에서는 미리 compile을 해두어 type checking을 하기 때문에 걱정이 없지만 js에서는 실행되는 순간에서야 발견하게 된다.

<br>

### Typescript

자바 스크립터로 변환되는



TypeScript가 node 환경에서도 동작하게 하려면 types node package를 설치해주어야 한다.