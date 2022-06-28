---
layout: single
title: "[React Native] 리액트 네이티브 1-개발 환경 세팅 및 기본 다지기"
categories: ['Front-end', 'React-Native']
toc: true
toc_sticky: true
---

<br>

리액트 네이티브에 대한 소개와 개발 환경 세팅을 진행하면서 발생한 문제점들에 대해서 서술하였다.

# Chapter 1. 리액트 네이티브 개발 환경 갖추기

## 리액트 네이티브 소개

리액트 네이티브를 소개하기 전에 리액트 프레임워크에 대해서 먼저 알아보자면 **리액트**는 2013년에 페이스북에서 발표한 오픈소스 자바스크립트 프레임워크이다.

**네이티브(native)**라는 단어는 `운영체제를 만들 때 사용한 프로그래밍 언어와 똑같은 언어로 만든`이라는 의미를 내포하고 있는 것으로 네이티브 앱은 모바일 운영체제(안드로이드-자바, IOS-objectC, 오브젝티브-C)로 만든 앱을 의미한다.

네이티브 앱을 실행 속도가 빠른 장점이 있지만 습득해야 할 지식이 많고 똑같은 기능을 안드로이드와 ios용으로 따로 만들어야 한다.

그래서 **크로스플랫폼**이라는 것이 등장하였는데 이는 하나의 소스 코드로 여러 운영체제에서 동작하는 앱을 개발 할 수 있다. 네이티브 앱보다는 조금 느리지만 개발 시간 비용을 크게 단축, 절약할 수 있다는 장점도 있다.

<br>

우리가 앞으로 사용할 리액트 네이티브는 **브릿지 방식**으로 동작한다. 또한 웹 브라우저에서 자바 스크립트 엔진 부분만 떼어 자바 스크립트 코드로 구현된 'View' 클래스를 네이티브 쪽 안드로이드 프레임워크(안드로이드 스튜디오) 아이폰(iOS) UIKit 프레임 워크의 'View' 클래스 호출로 연결하는 방식으로 동작하고 이를 브릿지 방식이라고 한다.

<br>

### 리액트 네이티브 개발 환경

리액트 네이티브 개발환경은 기본적으로 Node.js 개발 환경과 같다. 나는 Node.js 설치 후 VScode 편집기로 코드를 작성하였다.

맥과 Windows에서 리액트 네이티브 개발 환경하는 방법은 다르기 때문에 운영체제에 맞는 개발 환경을 설정한다.

<br>

#### 운영체제별 개발할 수 있는 앱

| 개발 운영체제 | 안드로이드 앱 개발 | iOS 앱 개발 |
| ------------- | ------------------ | ----------- |
| 윈도우 10     | 가능               | 불가능      |
| 맥            | 가능               | 가능        |
| 리눅스        | 가능               | 불가능      |

- 정리하자면 다음과 같다.
  - Node.js 설치 후 Visual Studio Code(VScode) 편집기로 코드 작성이 가능하다.
  - 안드로이드 앱의 경우, 윈도우, 맥, 리눅스 운영체제에서 개발 가능
  - iOS 앱의 경우, 맥에서만 개발 가능


안드로이드 앱은 안드로이드 스튜디오 설치를 요구하는데 이는 윈도우10, 맥, 리눅스 운영체제 모두에서 설치가 되기 때문에 모든 운영체제에서 개발할 수 있지만 iOS앱은 애플이 제공하는 Xcode 개발 도구가 필요한데 이는 오직 맥에서만 동작하므로 다른 운영체제(윈도우, 리눅스)에서는 개발할 수 없다.

<br>



### 타입스크립트로 리액트 네이티브 앱을 만드는 이유

**자바 스크립트**는 **타입(type) 기능이 없는 언어**이므로 개발자의 사소한 입력 오류 등을 알아채지 못한다. 따라서 자바스크립트로 개발을 하면 어디서 어떤 오류가 발생했는지 알기 어려워 디버깅이 쉽지 않다. 이는 곧 개발과 유지 보수 비용을 높이는 원인이 된다.

그러나 **타입스크립트**는 자바스크립트와 100% 호환하면서도 **타입 기능을 제공**하므로 자연스럽고 알기 쉬운 구문을 코드를 작성할 수 있다. 그리고 타입스크립트 컴파일러는 코드에 문제가 있으면 **미리** 문제의 원인을 친절하게 알려주기 때문에 자바스크립트의 단점을 극복할 수 있다.

추가적으로 run-time error는 실행을 하고 나서 발생하는 오류이기 때문에 상당히 프로그래머 입장에서는 곤란한 오류이다. 그렇기 때문에 이를 사전에 방지하지 못하는 자바스크립트의 단점을 보완하여 나온 것이 타입스크립트라고 보면 될 것 같다.

그러나 브라우저는 자바스크립트로 돌아가기 때문에 타입스크립트로 작성한 .ts파일이 자바스크립트로 변환되어야 하는데 이는 알아서 자동으로 해주니 걱정할 필요도 없다.

> 이러한 장점으로 인해 리액트 팀은 리액트나 리액트 네이티브를 사용할 때 타입스크립트를 사용하라고 권한다.

<br>



#### Installed Build Tools revision 31.0.0 is corrupted. Remove and install again using the SDK Manager.

처음에 설치한 안드로이드 스튜디오에서 에뮬레이터를 돌려보려고 했을 때 위와 같은 오류가 발생하였고 구글링을 통해 알아본 결과 주된 원인은 다음 두 파일이 없기 때문이었다.

1. dx.bat
2. dx.jar

그래서 31.0.0이 설치된 곳의 경로로 가보면("C:\Users\user\AppData\Local\Android\Sdk\build-tools\31.0.0") dx.bat과 lib 폴더에 d8.jar가 있는데 이 두 파일의 이름을 각각 d8 -> dx로 바꾸어 주었더니 해결되었다.

![image](https://user-images.githubusercontent.com/79521972/175042098-cd1e329e-4c44-409e-aa04-231d4012f835.png)



<br>

#### Manifest merger failed : Apps targeting Android 12 and higher are required to specify an explicit value for `android:exported` when the corresponding component has an intent filter defined. See https://developer.android.com/guide/topics/manifest/activity-element#exported for details.

앞선 오류를 해결했더니 다시 또 다른 오류가 발생하였는데 이는 AndroidManifest.xml에 android:exported="true" 구문을 추가해 줌으로써 해결할 수 있었다.



![image](https://user-images.githubusercontent.com/79521972/175044925-be5d4531-0eb8-4447-bbd0-e9f405cab3c9.png)



<br>

그래서 최종적으로 다음과 같은 화면이 에뮬레이터에 나오면 에뮬레이터 준비는 다 된 것이다.

<img src="https://user-images.githubusercontent.com/79521972/175045011-4a0a4db0-a414-47d5-a7a0-badc408f3ab8.png" alt="image" style="zoom:50%;" />



<br>

# Chapter 2. 리액트 네이티브 기본 다지기

최종적으로 에뮬레이터 작동까지 확인하였으면 이제 타입스크립트용 리액트 네이티브 프로젝트를 생성하고 ch02_1 디렉터리를 대상으로 vscode를 실행한다.

프로젝트를 생성하는 명령어는 앞으로 새 프로젝트를 생성해야 할 때마다 실행해야 하기 때문에 외워두면 좋다.

```shell
> npx react-native init ch02_3 --template react-native-template-typescript
```

<br>

기본적인 App.tsx 파일 세팅은 다음과 같다. -> 이는 화면 좌측 상단에 Hello, world를 띄우는 가장 기초적인 단계로 이제 화면에 무언갈 구성할 수 있게 되었다.

```react
import React from 'react'
import {Text} from 'react-native'

export default function App() {
  // console.log('App called')
  const textElement = React.createElement(Text, null, 'Hello, world!')
  return textElement
}
```



<br>
앞선 1장에서 살펴봤듯이 리액트 네이티브 프레임워크는 리액트 프레임워크에 기반을 둔 기술이다. 이 때문에 리액트 네이티브의 동작 원리를 이해하려면 먼저 리액트의 동작 원리를 알아야 한다. 

그런데 리액트의 동작 원리를 이해하려면 이번엔 물리 DOM(physical DOM)과 가상 DOM(virtual DOM)이란 개념을 알아야 한다.

<br>

> 기존 웹서버에서 HTML 문서를 생성하여 웹 브라우저로 전송하는 정적 HTML방식에서 벗어나 웹 브라우저에서 자바 스크립트 코드를 실행하여 동적으로 HTML을 생성하는 방식으로 동작하는 웹 기술로, 이것이 발전하여 오늘날의 리액트와 같은 프론트엔드 프레임워크가 되었다.
>
> > 잘 생각해 보면 과거에는 댓글을 달면 해당 페이지가 새로고침 되었다면 요즘에는 새로고침되지 않더라도 댓글이 딱! 하고 나오는 것을 알 수 있을 것이다.



<br>

### DOM과 렌더링

- DOM이란?
  -  **웹 페이지를 이루는 태그들을 자바스크립트가 이용할 수 있게끔 브라우저가 트리구조로 만든 객체 모델을 의미한다.**
  - 말 뜻 그대로 문서 객체 모델을 의미하며 문서 객체(html, head, body와 같은 태그)를 자바스크립트가 이용할 수 있는(memorable) 객체를 의미한다.
  - 정리하자면 DOM은 `HTML과 스크립팅 언어(자바스크립트)를 서로 이어주는 역할`이라고 보면 된다.

DHTML 방식은 자바 스크립트가 `<div>Hello world!</div>`와 같은 텍스트를 만드는 것이 아니라 객체 지향 언어의 상속 관계로 설계한 문서 객체 모델(Document Object Model, DOM) 타입스크립트 객체를 생성하는 방식으로 동작한다.

비록 웹브라우저는 `<div>, <h1>`과 같은 HTML 형태로 보여주지만 자바스크립트 코드 관점에서는 `<div>`는 HTMLDivElement 클래스의 인스턴스이고 `<h1>`은 HTMLHeadingElement 클래스의 인스턴스이다. 

이러한 클래스를 **DOM**이라 부르고 DOM 클래스의 인스턴스를 **DOM객체**라고 한다. 

DOM 객체는 무수히 많아서 트리구조를 이루고 있고 이를 **DOM 구조**라 한다.

> 그래서 웹 브라우저가 HTML을 parsing하여 자바스크립트 DOM 구조로 만드는 것을 렌더링(rendering)이라 한다.

<br>

### 물리 DOM과 가상 DOM

리액트 프레임워크에는 독특하게 물리 DOM과 가상 DOM이 나누어져 있는데 이 둘의 차이는 다음과 같다.

- 물리 DOM(physical DOM)
  - 웹 브라우저에서 자바스크립트 코드가 생성하는 실제 DOM 구조
- 가상 DOM(virtual DOM)
  - 리액트 코드가 생성한 자바스크립트 객체 구조

리액트는 특정 시점에 이 가상 DOM 구조를 물리 DOM 구조로 만드는데 , 이를 `리액트가 렌더링한다.`라고 하고 이 기능을 수행하는 패키지를 렌더러(renderer)라는 것으로 따로 구분을 해 두었다

<br>

> 리액트 프레임워크 vs. 리액트 네이티브 프레임워크
>
> > 리액트는 가상 DOM 구조를 react-dom이란 렌더러(DOM 렌더러) 패키지를 사용하여 물리 DOM 구조로 렌더링하는 방식으로 동작
>
> > 리액트 네이티브는 react-native 라는 렌더러(네이티브 렌더러) 패키지를 사용하여 렌더링하는 방식으로 동작하는 프레임워크

### 그래서 가상 DOM이 왜 나온건데?

요즘 흔히 접하는 큰 규모의 웹 애플리케이션(트위터,페이스북)은 스크롤바를 내릴 수록 수많은 데이터가 로딩된다. 그리고 각 데이터를 표현하는 요소들이 있다.

요소 개수가 몇 백 개, 몇 천 개 단위로 많은 규모가 큰 웹 애플리케이션에서 DOM에 직접 접근하여 변화를 주다 보면 성능 이슈가 조금씩 발생하기 시작한다.

즉 느려진다는 말인데요. 이것이 정확한 말은 아니다.

DOM자체는 빠르다.  읽고 쓸 때의 성능은 자바스크립트 객체를 처리 할 때의 성능과 비교하여 다르지 않다.

단, 웹브라우저 단에서 DOM 변화가 일어나면 웹브라우저가 CSS를 다시 연산하고 레이아웃을 구성하고, 페이지를 리 페인트 즉 렌더링이 일어 나는 이 과정에서 시간이 허비되는 것이다.

그리고 이 렌더링 과정은 상황에 따라 여러번 반복하여 발생할 수 있고, 돔이 추가,삭제 혹은 태그 위치가 변하는 경우 렌더링이 일어납니다.

> 렌더링 : 브라우저 로딩 과정 중 스타일 이후의 과정(스타일-> 레이아웃 -> 페인트 -> 합성)을 렌더링이라고 한다.

**결론**

속도적인 부분과 많은 일을 수행하다 버그가 발생하거나 브라우저가 죽는 일 등등의 일을 개선하고자

가상돔(*Virtual DOM*)이 나왔다.

<br>

### react 패키지의 역할

리액트 네이티브 프레임워크는 react라는 패키지를 사용하며 react 패키지는 App.tsx와 같은 파일을 가상 DOM 구조로 만드는 역할을 하는 패키지이다.

네이티브 렌더러는 **리액트 요소**를 안드로이드 프레임워크나 iOS용 UIKit 프레임워크의 화면 UI 객체로 바꿔주는 역할을 한다.



<br>

### 브리지 방식 렌더링

리액트는 모든 것이 자바스크립트로 동작하기에 React.render라는 DOM 렌더러의 동작을 코드로 확인할 수 있지만,

리액트 네이티브는 네이티브 렌더러의 모습을 확인할 수 없는데, 이는 리액트 네이티브 프로젝트의 android와 ios 디렉터리에 있는 자바나 오브젝티브-C로 구현한 **네이티브 모듈 쪽에서 렌더링이 진행되기 때문이다.**

네이티브 모듈 쪽에는 JavaScriptCore라는 이름의 자바스크립트 엔진이 동작하고 이 C++ 언어로 구현된 JavaScriptCore 엔진은 안드로이드에서는 JNI(Java Native Interface) 방식으로, iOS에서는 오브젝티브-C의 FFI 방식으로 연결되어 동작한다.

> 엔진과 라이브러리는 같은 개념인데 일반적으로 코드 분량이 방대한 라이브러리를 엔진이라고 부르는 경향이 있다.

리액트 네이티브 전용 패키지에는 항상 자바스크립트 스레드에서 동작하는 쪽과 UI 스레드에서 동작하는 쪽이 따로 있다. 때문에 자바스크립트 쪽만 설치하고 UI 스레드에서 동작하는 쪽을 설치하지 않으면 패키지가 정상 동작하지 않는다.

npx react-native link, npx pod-install 과 같은 명령은 앞으로 네이티브에서 동작하는 부분을 설치하는 것을 의미한다고 알고 있으면 된다



<br>

### React.createElement API가 하는 일

리액트와 리액트 네이티브에서 React.createElement API는 가장 저수준 기능으로 서 가상 DOM 객체를 생성한다.

HTML로 웹 브라우저에 'Hello world' 등의 텍스트를 출력하려면 다음과 같이 작성한다.

```html
<p>
    Hello world!
</p>
```

그러면 웹 브라우저가 렌더링하여 해당 내용을 화면에 웹 페이지를 표시한다.

<br>

자바스크립트로 앞선 HTML 텍스트와 동일한 효과를 구현하면 다음과 같은 코드를 통해 물리 DOM 객체를 생성할 수 있다.

```react
const pElement = document.createElement('p')
pElemenet.innerText = 'Hello JavaScript'
```

그리고 이 물리 DOM 객체는 다음의 코드가 반드시 나와야 렌더링 되어 화면에 나타나는데 HTML과 달리 자바스크립트 세계에서는 이처럼 DOM 객체 생성 과정과 렌더링 등 두 과정이 필요하게 된다.

```react
document.body.appendChild(pElement)
```



<br>

이제 리액트 프레임워크를 통해서 앞의 자바스크립트 효과를 구현해 볼 것인데 가장 먼저 가상 DOM 객체를 생성해 준다.

```react
const pElement = document.createElement('p', null, 'Hello React World')
```

<br>

다음과 같은 코드로 이 pELement 가상 DOM 객체를 물리 DOM 객체로 변환하여 화면에 표시할 수 있다.

```react
import ReactDOM from 'react-dom'

ReactDOM.render(pElement, document.body)
```



<br>

### 안드로이드 네이티브 모듈과 npm run android 명령과의 관계

android 디렉터리를 대상으로 안드로이드 스튜디오를 실행해서 안드로이드 앱을 빌드

npm run adroid 명령은 사실 다음과 같은 안드로이드 앱 빌드 명령을 실행한다. 

```shell
cd android
./gradlew installDebug
```

이 명령은 명령 줄 방식으로 안드로이드 스튜디오에서 빌드 명령을 실행하는 것과 같다.

> 이 명령을 실행하기 위해선 항상 안드로이드 스튜디오의 에뮬레이터가 미리 실행되어 있어야 함을 명심하라.

그런데 이 안드로이드 빌드 명령은 리액트 네이티브 앱을 에뮬레이터에 설치만 할 뿐 실행은 하지 않기 때문에 npm run android 명령을 사용하는 것이다.

<br>

- npm run android: 
  - 타입스크립트 코드와는 상관없이 android 디렉터리만을 대상으로 자바 언어로 작성한 네이티브 앱(ch02_1/android) 부분을 빌드하여 에뮬레이터에 앱을 설치하는 명령이다. 
- npm start:
  - 설치된 앱이 처음 구동될 때(Not hot-reloading) 앱이 수신해야 할 ES5 자바스크립트 코드로 컴파일된 타입스크립트 코드 번들을 제공하는 메트로 서버를 실행하는 명령이다.

npm start가 선행 되어야 함 (npm run android보다)

<br>

> 새로운 프로젝트 시작 전이나 새로운 패키지를 설치할 때는 메트로 서버를 중지해야 한다.

<br>

## JSX 구문

JSX 구문은 React.createElement 대신 가상 DOM 객체를 쉽게 만들 수 있다.

리액트나 리액트 네이티브 코드 작성자는 복잡한 여러 번의 React.createElement 호출 코드를 작성하는 대신 훨씬 간결한 JSX 코드만 작성하면 되기 때문에 빠르고 간결한 코드를 작성할 수 있으므로 개발 생산성이 대폭 향상된다.

JSX 구문이 있는 타입스크립트 코드는 확장자가 .tsx 이며 JSX 구문이 있는 코드는 다음 import 문이 필요하다.

```react
import React from 'react'
```



HTML 마크업 언어 < XML 마크업 언어

- XML은 태그나 속성을 맘대로 확장할 수 있기 때문에


컴포넌트는 여러 개의 속성과 하나 이상의 자식 컴포넌트를 가질 수 있다.



<br>

### 조건에 따라 분기되는 JSX 문 작성법

1. if문을 JSX 문 바깥쪽에 구현하여 문제 해결
2. 조건문을 단축 평가 코드로 바꿔 문제 해결
3. JSX 문을 변수에 담아 문제 해결



<br>

## faker 모듈

faker 모듈은 그럴듯한 가짜 데이터를 생성하는 것을 도와주는 패키지이다.

개발을 할 때 그럴듯한 아바타, 텍스트, 이미지 파일이 필요한데 이 패키지에서 이러한 다양한 가짜 데이터를 제공해 준다.

## faker 모듈을 설치하면서 발생한 문제

계속해서 faker 모듈을 import 하면 선언되지 않았다고 하는 문제가 발생하였다.

-> 5.5.3 버전을 명시하여 설치하였더니 해결함

```shell
npm i faker
npm i faker@5.5.3
npm i @types/faker@5.5.3
```



<br>
컴포넌트를 제작하는 목적이 **코어 컴포넌트**를 화면에 렌더링하는 일이므로 리액트와 리액트 네이티브는 클래스 컴포넌트가 **render라는 이름의 메서드**를 가져야 한다고 강제합니다. 

<br>

```react
import React, {Component} from 'react'
import {Text} from 'react-native'
import * as D from '../data'

const person = D.createRandomPerson()
export default class ClassComponent extends React.Component {
    render() {
        return <Text>{JSON.stringify(person, null, 2)}</Text>
    }
}
```



<br>

### 화살표 방식 함수 컴포넌트 만들기

보통 속성이 없는 컴포넌트는 function 키워드를 사용하여 작성하는 것이 편리하고 속성이 있는 경우 화살표 함수로 만드는 것이 편리하다.



<br>



## React.FC를 쓰는 이유

1. 항상 children을 가진다는 것을 의미

















### 속성이란?

**클래스의 멤버 변수**

리액트 네이티브는 컴포넌트의 속성이 바뀌면 이를 즉각 화면에 반영해야 한다.

리액트와 리액트 네이티브에서는 바뀐 속성값을 화면에 반영하는 것을 **재렌더링**한다고 한다.

따라서 속성이란 `클래스 속성 + 재렌더링`을 의미한다.

<br>

`속성은 부모 컴포넌트가 자식 컴포넌트 쪽으로 데이터를 전달하고 싶을 때 사용한다.`

<br>

```react
import React, { Children, Component } from "react"
import { SafeAreaView, ScrollView, Text } from "react-native"
import ClassComponent from "./src/screens/ClassComponent"
import ArrowComponent from "./src/screens/ArrowComponent"
import * as D from './src/data'
import Person from "./src/screens/Person"

//const person = D.createRandomPerson()
const people = D.makeArray(100).map(D.createRandomPerson)

export default function app() {
  const children = people.map((person) => (
    <Person key={person.id} person={person} />
  ))
  return (
    <SafeAreaView>
      <ScrollView>{children}</ScrollView>
    </SafeAreaView>
  )
}
//const person = D.createRandomPerson()
//<Text>{JSON.stringify(person, null, 2)}</Text>
```

person.id라는 속성을 추가한 이유

- 모든 리액트 컴포넌트는 key, children, ref 등 3개 속성을 기본적으로 가진다.
- 이 중 key 속성은 리액트 프레임워크가 컴포넌트의 렌더링 속도를 최적화 하는 데 필요한 속성이다.
- 그래서 리액트 네이티브로 하여금 여러 컴포넌트 (위에서는 Person) 을 구분할 수 있도록 key 속성에 person.id 값을 설정한다.



<br>

### 이벤트 속성

- TextInput 코어 컴포넌트의 특징
  - defaultValue 속성에 초깃값을 설정할 수 있다.
  - 입력된 텍스트는 value 속성값으로 얻을 수 있다.
  - 텍스트가 입력될 때 onChangeText 이벤트 처리기를 실행한다.
  - placeholder 속성을 사용하여 어떤 값을 설정해야 하는지 문자열로 출력할수 있다.
  - editable 속성값에 false를 설정하면 입력을 못하게(disable)할 수 있다.
  - keyboardType 속성에 'default', 'numeric', 'email-address'등의 값을 설정할 수 있다.
  - 포커스를 가지게 하는 focus 메서드와 포커스를 잃게 하는 blur 메서드가 있다.
  - 텍스트를 입력할 수 있는 상태(포커스를 가진 상태)가 되면 onFocus 이벤트를 호출하고 텍스트를 입력할 수 없는 상태(포커스를 잃은 상태) 가 되면 onBlur 이벤트를 호출한다.
  - 텍스트 입력이 모두 끝나면 onEndEditing 이벤트를 호출한다.
  - 자식 요소를 가지지 못한다.
