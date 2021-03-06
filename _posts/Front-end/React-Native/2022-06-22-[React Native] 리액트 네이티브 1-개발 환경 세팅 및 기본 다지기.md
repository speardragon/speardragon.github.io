---
layout: single
title: "[React Native] 리액트 네이티브 1-개발 환경 세팅 및 기본 다지기"
categories: ['FrontEnd', 'ReactNative']
toc: true
toc_sticky: true
---

<br>

리액트 네이티브에 대한 소개와 개발 환경 세팅을 진행하면서 발생한 문제점들에 대해서 서술하였다.

# Chapter 1. 리액트 네이티브 개발 환경 갖추기

## 리액트 네이티브 소개

리액트 네이티브를 소개하기 전에 리액트 프레임워크에 대해서 먼저 알아보자면 **리액트**는 2013년에 페이스북에서 발표한 오픈소스 자바스크립트 프레임워크이다.

**네이티브(native)**라는 단어는 `운영체제를 만들 때 사용한 프로그래밍 언어와 똑같은 언어로 만든`이라는 의미를 내포하고 있는 것으로 네이티브 앱은 모바일 운영체제(안드로이드-자바, IOS-objectC, 오브젝티브-C)로 만든 앱을 의미한다.

네이티브 앱은 실행 속도가 빠른 장점이 있지만 습득해야 할 지식이 많고 똑같은 기능을 안드로이드와 ios용으로 따로 만들어야 한다.

그래서 **크로스플랫폼**이라는 것이 등장하였는데 이는 하나의 소스 코드로 여러 운영체제에서 동작하는 앱을 개발 할 수 있다. 네이티브 앱보다는 조금 느리지만 개발 시간 비용을 크게 단축, 절약할 수 있다는 장점도 있다.

<br>

우리가 앞으로 사용할 리액트 네이티브는 **브릿지 방식**으로 동작한다. 또한 웹 브라우저에서 자바 스크립트 엔진 부분만 떼어 자바 스크립트 코드로 구현된 'View' 클래스를 네이티브 쪽 안드로이드 프레임워크(안드로이드 스튜디오), 아이폰(iOS) UIKit 프레임 워크의 'View' 클래스 호출로 연결하는 방식으로 동작하고 이를 브릿지 방식이라고 한다.

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


안드로이드 앱은 안드로이드 스튜디오 설치를 요구하는데 이는 윈도우10, 맥, 리눅스 운영체제 모두에서 설치가 되기 때문에 모든 운영체제에서 개발할 수 있지만 iOS앱은 애플이 제공하는 Xcode 개발 도구가 필요한데 이는 오직 맥에서만 동작하므로 다른 운영체제(윈도우, 리눅스)에서는 개발할 수 없는 것이다.

<br>



### 타입스크립트로 리액트 네이티브 앱을 만드는 이유

**자바 스크립트**는 **타입(type) 기능이 없는 언어**이므로 개발자의 사소한 입력 오류 등을 알아채지 못한다.(컴파일을 하기 전까지는 알지 못함.) 따라서 자바스크립트로 개발을 하면 어디서 어떤 오류가 발생했는지 알기 어려워 디버깅이 쉽지 않다. 이는 곧 개발과 유지 보수 비용을 높이는 원인이 된다.

그러나 **타입스크립트**는 자바스크립트와 100% 호환하면서도 **타입 기능을 제공**하므로 자연스럽고 알기 쉬운 구문을 코드를 작성할 수 있다. 그리고 타입스크립트 컴파일러는 코드에 문제가 있으면 **미리** 문제의 원인을 친절하게 알려주기 때문에 자바스크립트의 단점을 극복할 수 있다.

추가적으로 **run-time error**는 실행을 하고 나서 발생하는 오류이기 때문에 상당히 프로그래머 입장에서는 곤란한 오류이다. 그렇기 때문에 이를 사전에 방지하지 못하는 자바스크립트의 단점을 보완하여 나온 것이 타입스크립트라고 보면 될 것 같다.

그러나 브라우저는 자바스크립트로 돌아가기 때문에 타입스크립트로 작성한 .ts파일이 자바스크립트로 변환되어야 하는데 이는 알아서 자동으로 해주니 걱정할 필요도 없다.

> 이러한 장점으로 인해 리액트 팀은 리액트나 리액트 네이티브를 사용할 때 타입스크립트를 사용하라고 권한다.

<br>



## 윈도우에서 개발 환경 갖추기

윈도우에서 리액트 네이티브를 사용하기 위해선 다음과 같은 프로그램을 설치한다.

- node.js
  - 리액트 네이티브 개발은 node.js 버전에 영향을 받는다. 따라서 지원이 확실한 LTS버전을 사용하는 것이 권장된다.
- JDK
  - 리액트 네이티브를 사용하여 안드로이드 앱을 만드려면 안드로이드 SDK 빌드 도구가 필요하기 때문에 JDK를 설치한다.
- 비주얼 스튜디오(Vscode)
  - 비주얼 스튜디오는 마이크로소프트에서 제공하는 오픈소스 편집기로 모든 소스코드가 이곳에 저장되어 관리한다.
- 안드로이드 스튜디오
  - 안드로이드 앱을 만드려면 구글이 제공하는 안드로이드 스튜디오를 설치해야 한다.
  - 이 안드로이드 스튜디오에서 추가적으로 안드로이드 SDK또한 설치를 해 준다.

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

그래서 최종적으로 에뮬레이터를 생성하여 실행시켜 보았을 때 다음과 같은 화면이 에뮬레이터에 나오면 에뮬레이터 준비는 다 된 것이다.

<img src="https://user-images.githubusercontent.com/79521972/175045011-4a0a4db0-a414-47d5-a7a0-badc408f3ab8.png" alt="image" style="zoom:50%;" />

### 첨언

보통 이 과정에서 문제가 되는 것은 환경변수의 문제인 경우가 많았다. 이는 구글에 쳐보면 많은 해결 방법이 나오고 나 또한 해당 문제를 겪어 올바른 환경 변수 설정으로 문제를 해결할 수 있었다.

또한 에뮬레이터를 실행할 때 installBug가 나타날 경우가 있는데 이는 AVD manager에서 해당 에뮬레이터 설정에서 wipe data를 통해 용량을 정리해 주면 된다.



## VScode 환경설정

- 타입스크립트 설치

  - 터미널에서: `npm i -g typescript ts-node`

- prettier 확장 기능 설치 및 동작 환경 설정

  - vscode 확장 메뉴에서 prettier를 다운 받고 setting.json에 대한 설정을 한다.

  - ```json
    {
      "terminal.integrated.defaultProfile.windows": "PowerShell",
      "editor.wordWrap": "on",
      "editor.formatOnSave": true,
      "[typescript]": {
        "editor.formatOnPaste": true,
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "editor.tabSize": 2,
      "prettier.semi": false
    }
    
    ```

  - 이를 통해 파일을 저장할 때 항상 포맷(즉, 린트) 기능을 수행한다. 그리고 타입스크립트 파일일 때는 포맷 프로그램으로 앞서 설치한 prettier를 실행한다.

  - prettier를 사용하려면 디렉터리에 .prettierrc.js 파일을 만들어야 하는데 이 파일은 prettier가 소스 코드를 포맷할 때 참조하는 파일이기에 .js 파일로 구현된다.

  - ```js
    module.exports = {
      arrowParens: 'avoid',
      bracketSameLine: true,
      bracketSpacing: false,
      singleQuote: true,
      trailingComma: 'all',
    };
    
    ```

  - 각각에 해당하는 기능을 알고 싶으면 다음 링크를 통해 원하는 설정을 할 수 있다.

    - [https://prettier.io/docs/en/configuration.html](https://prettier.io/docs/en/configuration.html)

<br>

# Chapter 2. 리액트 네이티브 기본 다지기

최종적으로 에뮬레이터 작동까지 확인하였으면 이제 타입스크립트용 리액트 네이티브 프로젝트를 생성하고 ch02_1 디렉터리를 대상으로 vscode를 실행한다.(디렉터리 명은 마음대로 해도 된다.)

프로젝트를 생성하는 명령어는 앞으로 새 프로젝트를 생성해야 할 때마다 실행해야 하기 때문에 외워두면 좋다.

```shell
> npx react-native init ch02_1 --template react-native-template-typescript
```

- 여기서 사용하는 npx 명령어(혹은 npm)가 가끔씩 에러를 띄우는 경우가 있는데 이는 해당 명령어를 치는 터미널의 경로가 이 프로젝트를 생성한 폴더 안이어야 하기 때문이다. 때문에 항상 명령어를 치기 전에 올바른 경로에 있는지를 확인해야 한다.



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

앱이 잘 동작하는지 확인하기 위해서는 터미널을 동시에 두 개를 켜고 하나에서는 `npm start`를 나머지 하나에서는 `npm run android`를 친다.

- 이는 package.json에 설정된 script 명령이다.

그러면 안드로이드 에뮬레이터에 hello world가 찍힌 것을 볼 수 있을 것이다.

<br>

## DOM과 렌더링

앞선 1장에서 살펴봤듯이 리액트 네이티브 프레임워크는 리액트 프레임워크에 기반을 둔 기술이다. 이 때문에 리액트 네이티브의 동작 원리를 이해하려면 먼저 리액트의 동작 원리를 알아야 한다. 

그런데 리액트의 동작 원리를 이해하려면 이번엔 물리 DOM(physical DOM)과 가상 DOM(virtual DOM)이란 개념을 알아야 한다.

<br>

> 기존 웹서버에서 HTML 문서를 생성하여 웹 브라우저로 전송하는 정적 HTML방식에서 벗어나 웹 브라우저에서 자바 스크립트 코드를 실행하여 동적으로 HTML을 생성하는 방식으로 동작하는 웹 기술로, 이것이 발전하여 오늘날의 리액트와 같은 프론트엔드 프레임워크가 되었다.
>
> > 잘 생각해 보면 과거에는 댓글을 달면 해당 페이지가 새로고침 되었다면 요즘에는 새로고침되지 않더라도 댓글이 딱! 하고 나오는 것을 알 수 있을 것이다.

<br>

- DOM이란?
  -  **웹 페이지를 이루는 태그들을 자바스크립트가 이용할 수 있게끔 브라우저가 트리구조로 만든 객체 모델을 의미한다.**
  - 말 뜻 그대로 문서 객체 모델을 의미하며 문서 객체(html, head, body와 같은 태그)를 자바스크립트가 이용할 수 있는(memorable) 객체를 의미한다.
  - 정리하자면 DOM은 `HTML과 스크립팅 언어(자바스크립트)를 서로 이어주는 역할`이라고 보면 된다.

DHTML 방식은 자바 스크립트가 `<div>Hello world!</div>`와 같은 텍스트를 만드는 것이 아니라 객체 지향 언어의 상속 관계로 설계한 문서 객체 모델(Document Object Model, DOM) 타입스크립트 객체를 생성하는 방식으로 동작한다.

비록 웹브라우저는 `<div>, <h1>`과 같은 HTML 형태로 보여주지만 자바스크립트 코드 관점에서는 `<div>`는 **HTMLDivElement 클래스의 인스턴스**이고 `<h1>`은 **HTMLHeadingElement 클래스의 인스턴스**이다. 

이러한 클래스를 **DOM**이라 부르고 DOM 클래스의 인스턴스를 **DOM객체**라고 한다. 

DOM 객체는 무수히 많아서 **트리구조**를 이루고 있고 이를 **DOM 구조**라 한다.

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

즉 느려진다는 말인데 이것이 정확한 말은 아니다.

DOM자체는 빠르다.  읽고 쓸 때의 성능은 자바스크립트 객체를 처리 할 때의 성능과 비교하여 다르지 않다.

단, 웹브라우저 단에서 DOM 변화가 일어나면 웹브라우저가 CSS를 다시 연산하고 레이아웃을 구성하고, 페이지를 리-페인트 즉 렌더링이 일어 나는 이 과정에서 시간이 허비되는 것이다.

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

**리액트**는 모든 것이 자바스크립트로 동작하기에 React.render라는 DOM 렌더러의 동작을 코드로 확인할 수 있지만,

**리액트 네이티브**는 네이티브 렌더러의 모습을 확인할 수 없는데, 이는 리액트 네이티브 프로젝트의 android와 ios 디렉터리에 있는 자바나 오브젝티브-C로 구현한 **네이티브 모듈 쪽에서 렌더링이 진행되기 때문이다.**

네이티브 모듈 쪽에는 JavaScriptCore라는 이름의 자바스크립트 엔진이 동작하고 이 C++ 언어로 구현된 JavaScriptCore 엔진은 안드로이드에서는 JNI(Java Native Interface) 방식으로, iOS에서는 오브젝티브-C의 FFI 방식으로 연결되어 동작한다.

> 엔진과 라이브러리는 같은 개념인데 일반적으로 코드 분량이 방대한 라이브러리를 엔진이라고 부르는 경향이 있다.

리액트 네이티브 전용 패키지에는 항상 자바스크립트 스레드에서 동작하는 쪽과 UI 스레드에서 동작하는 쪽이 따로 있다. 때문에 자바스크립트 쪽만 설치하고 UI 스레드에서 동작하는 쪽을 설치하지 않으면 패키지가 정상 동작하지 않는다.

npx react-native link, npx pod-install 과 같은 명령은 앞으로 네이티브에서 동작하는 부분을 설치하는 것을 의미한다고 알고 있으면 된다.

(현재 일자, 22-06-29를 기준으로 npx react-native link는 동작하지 않음)



<br>

### React.createElement API가 하는 일

리액트와 리액트 네이티브에서 React.createElement API는 **가장 저수준 기능**으로 서 가상 DOM 객체를 생성한다.

HTML로 웹 브라우저에 'Hello world' 등의 텍스트를 출력하려면 다음과 같이 작성한다.

```html
<p>
    Hello world!
</p>
```

그러면 웹 브라우저가 렌더링하여 해당 내용을 화면에 웹 페이지를 표시한다.

<br>

자바스크립트로 앞선 HTML 텍스트와 동일한 효과를 구현하면 다음과 같은 코드를 통해 **물리 DOM 객체**를 생성할 수 있다.

```react
const pElement = document.createElement('p')
pElemenet.innerText = 'Hello JavaScript'
```

그리고 이 물리 DOM 객체는 **다음의 코드**가 반드시 나와야 렌더링 되어 화면에 나타나는데 HTML과 달리 자바스크립트 세계에서는 이처럼` DOM 객체 생성 과정`과 `렌더링` 등 두 과정이 필요하게 된다.

```react
document.body.appendChild(pElement)
```

<br>

이제 **리액트** 프레임워크를 통해서 앞의 **자바스크립트 효과**를 구현해 볼 것인데 가장 먼저 가상 DOM 객체를 생성해 준다.

```react
const pElement = React.createElement('p', null, 'Hello React World')
```

<br>

다음과 같은 코드로 이 pELement 가상 DOM 객체를 물리 DOM 객체로 변환하여 화면에 표시할 수 있다.

```react
import ReactDOM from 'react-dom'

ReactDOM.render(pElement, document.body)
```



<br>

리액트 네이비브도 리액트와 동일하게 가상 DOM 객체를 생성한다.

```react
const textElement = React.createElement(Text, null, 'Hello world!')
```

그러나 리액트 네이티브 렌더러는 네이티브에서 동작하기 때문에 다음처럼 가상 DOM 객체를 **네이티브로 넘겨주는 방식**으로 동작한다.

```react
export default function App() {
    const textElement = React.createElement(Text, null, 'Hello world!')
    return textElement
}
```

<br>

react-native-cli(Not expo)는 네이티브 모듈에서 동작하는 자바스크립트 엔진이 가상 DOM 객체를 넘겨주는 App의 존재를 알 수 있도록 다음 내용의 index.js 파일을 만든다.

```js
// index.js
import {AppRegistry} from 'react-native'
import App from './App'
import {name as appName} from './app.json'

AppRegistry.registerComponent(appName, () => App)
```

또한 네이티브 모듈에서 동작하는 자바 스크립트 엔진이 위 파일의 존재를 알 수 있도록 다음과 같은 코드를 가진 자바 파일도 만들어 준다.

```java
// MainApplication.java
(...생략...)
@Override
protected String getJSMainModulName() {
    return "index"
};
(...생략...)
```

<br>

이제 리액트 네이티브 앱을 폰(혹은 에뮬레이터)에서 실행하면 MainApplication.java가 실행되고 자바스크립트 스레드에서 실행되는 자바스크립트 엔진이 getJSMainModuleName을 통해 index.js 파일의 존재를 알게 된다.

이 index.js 파일을 통해 App의 존재를 알고 App을 호출하여 얻은 가상 DOM 객체를 브리지를 통해 네이티브로 넘겨 마침내 'Hello world!'를 출력한다.

<br>

### 안드로이드 네이티브 모듈과 npm run android 명령과의 관계

npm run adroid 명령은 사실 다음과 같은 안드로이드 앱 빌드 명령을 실행한다. 이 명령은 명령 줄 방식으로 안드로이드 스튜디오에서 빌드 명령을 실행하는 것과 같다.

> 이 명령을 실행하기 위해선 항상 안드로이드 스튜디오의 에뮬레이터가 미리 실행되어 있어야 함을 명심하라.

```shell
cd android
./gradlew installDebug
```

그런데 이 안드로이드 빌드 명령은 리액트 네이티브 앱을 에뮬레이터에 설치만 할 뿐 실행은 하지 않기 때문에` npm run android `명령을 사용하는 것이다.

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

**JSX 구문**은 React.createElement 대신 가상 DOM 객체를 쉽게 만들 수 있다.

리액트나 리액트 네이티브 코드 작성자는 복잡한 여러 번의 React.createElement 호출 코드를 작성하는 대신 훨씬 간결한 JSX 코드만 작성하면 되기 때문에 빠르고 간결한 코드를 작성할 수 있으므로 개발 생산성이 대폭 향상된다.

JSX 구문이 있는 타입스크립트 코드는 확장자가 .tsx 이며 JSX 구문이 있는 코드는 다음 import 문이 필요하다.

```react
import React from 'react'
```

흥미롭게도 이는 마치 자바스크립트나 타입스크립트 문법에 XML 구문이 있는 것처럼 사용한다. 리액트와 리액트 네이티브에서는 이처럼 자바스크립트와 XML 구문을 결합해 사용하는 코드를 JSX라고 한다.

<br>

페이스북은 JSX 구문을 컴파일하고자 @babel/plugin-transform-react-jsx라는 바벨 플러그인을 실행해 여러 개의 React.createElement 함수를 호출하는 평범한 자바스크립트로 변환한다. 타입스크립트 컴파일러 tsc도 바벨의 이런 동작 방식과 비슷하게 JSX 코드를 만나면 React.createElement 함수 호출 자바스크립트로 변환한다.

때문에 JSX 구문이 주는 이런 간편성은 개발 생선성의 대폭 증가로 이어졌고 이에 더하여 세상에서 가장 인기있는 프론트엔드 자바스크립트 프레임워크가 되는데 큰  역할을 했다.

그런데 JSX 구문이 있는 타입스크립트 코드는 확장자가 .tsx여야 한다. 또한 JSX 구문이 있는 코드는 `import React from 'react'` import 문이 필요하다.

<br>

## 마크업 언어 용어

HTML 마크업 언어 < XML 마크업 언어

- XML은 태그나 속성을 맘대로 확장할 수 있기 때문에
- HTML은 문서를 구성하는 요소의 태그 이름이 모두 고정되어 있음

```html
<div id="root" style="display: flex">
    <h1>
        Hello world
    </h1>
</div>
```

위처럼 div와 같은 태그를 꺽쇠 기호(<\>)로 감싼 시작 태그를 `<div>`처럼 만들고 `</div>`처럼 태그 이름 앞에 '/' 기호를 추가한 끝 태그로 감싼 `<div></div>`형태가 기본이다. 

- 시작 태그에는 id, style과 같은 속성(attribute)을 함께 기술할 수 있으며 항상 속성값은 작은 따옴표나 큰따옴표로 감싸야한다.
- 시작태그와 끝 태그 사이에는 `<h1>Hello world</h1>`와 같은 자식 요소를 삽입할수 있는데 자식요소란 XML 요소나 문자열을 의미한다.
  - 문자열이 자식 요소인 경우 따옴표는 생략한다.
  - 자식 요소가 없다면 `<div />` 형태로 표현할 수 있는데, 이를 스스로 닫는 태그(self-closing tag)라 한다.

<br>

- createElemenet 사용 방법

  - ```react
    가상_DOM_객체 = createElement(컴포넌트_이름_또는_문자열, 속성_객체, 자식_컴포넌트)
    ```

  - 따라서 아래 두 표현은 같은 것이다.

  - ```react
    const textElement = React.createElement(Text, null, 'Hello world!')
    ```

  - ```xml
    <Text>Hello world!</Text>
    ```

  - XML 파서(parse)가 이를 React.createElement(Text, null, 'Hello word!') 자바스크립트 코드로 변환해 준다면 이 텍스트를 자바스크립트에서 이용할 수 있다. JSX 구문은 이런 아이디어에서 비롯된 것이다. 

    - 위에서 언급한  @babel/plugin-transform-react-jsx 바벨 플러그인이 이 아이디어를 구현한 parser이다.

<br>

## JSX 구문에서 중괄호 {}의 의미

JSX는 다음 코드에서 보듯 XML 마크업 구조에 중괄호({ })를 사용하여 자바스크립트 코드를 감싸는 형태의 문법을 제공한다.

```jsx
<Text>
	{person}
</Text>
```

이런 식으로 자바스크립트의 변수값을 XML 구문 안에 표현할수 있다.

<br>

## XML 요소 간의 자식 관계와 컴포넌트 간의 자식 관계의 유사성

XML은 요소를 부모 자식 관계로 표현하는데 보통 부모 태그가 자식 태그를 감싼 형태이다.

그리고 JSX 문은 React.createElement 호출이므로 JSX 문 자체를 변수에 담을 수 있고 이 과정까지 생략하고 다음처럼 함수의 반환값으로 사용하는 것이 일반적이다.

```react
export default function App() {
    return <SafeAreaView><Text>Hello JSX world!</Text></SafeAreaView>
}
```



<br>

## 표현식과 실행문 그리고 JSX

JSX 코드 안에는 반드시 return 키워드 없이 값을 반환해야 한다. 즉 실행문(if와 같은)은 그 자체로는 '값'이 아니기 때문에 JSX 코드 안에서 사용하지 못하고 오로지 표현식만이 포함되어 있어야 한다.

또한 해당 코드 안에는 가상 DOM 객체를 반드시 포함해야 하기 때문에 이를 구성하는 코드 한 줄 한 줄이 모드 React.createElement 호출 코드로 변환되어야 하는데 이로 변환 될 수 없는(console.log()) 와 같은 것은 오류를 일으킬 것이다.

이럴 때는 해당 코드는 JSX 문 바깥에 두어서 해결하도록 한다. 혹은 단축 평가 형태로 구현한다. 따라서 다음과 같은 코드는 오류를 일으키지 않고 정상 동작한다.

### 조건에 따라 분기되는 JSX 문 작성법

1. if문을 JSX 문 바깥쪽에 구현하여 문제 해결

```react
import React from 'react'
import {SafeAreaView, Text} from 'react-native'

export default function App() {
    const isLoading = true
    if (isLoading) {
        return (
            <SafeAreaView>
                {isLoading && <Text>Loading...</Text>}
                {!isLoading && <Text>Hello JSX world!</Text>}
            </SafeAreaView>
        )
    }
}
```

2. 조건문을 단축 평가 코드로 바꿔 문제 해결

```react
import React from 'react'
import {SafeAreaView, Text} from 'react-native'

export default function App() {
    const isLoading = true
    if (isLoading) {
        return (
            <SafeAreaView>
                {isLoading && <Text>Loading...</Text>} /* this means isLoading ? <Text>Loading...</Text> : undefined
                {!isLoading && <Text>Hello JSX world!</Text>}
            </SafeAreaView>
        )
    }
}
```

3. JSX 문을 변수에 담아 문제 해결

```react
import React from 'react'
import {SafeAreaView, Text} from 'react-native'

export default function App() {
    const isLoading = true
    const children = isLoading ? (
        <Text>Loading...</Text>
    ) : (
        <Text>Hello JSX world!</Text>	
    )
    return <SafeAreaView>{children}</SafeAreaView>
}
```



<br>

## 배열과 JSX 구문

여러개의 JSX 문을 배열 형태로 만들어 이를 자식 컴포넌트로 넘겨줄 수도 있다.

<br>

이 때 주의해야 할 점이 있는데 여러 개의 자식 컴포넌트가 있을 때는 반드시 XML 작성 원칙을 준수해야 한다.

- <mark>XML 문법에서 부모 요소 없이는 여러 개의 XML 요소를 만들 수 없다.</mark>

JSX 역시 XML이므로 여러 개의 컴포넌트를 배열로 담은 children 변수가 부모 컴포넌트 없이 홀로 {children} 형태로 존재해서는 안된다.



<br>

### 배열 만들어 보기

Array 클래스가 제공하는 map 메서드를 사용하여 내용이 조금씩 다른 컴포넌트 배열을 만들어 보자.

```react
import React from 'react'
import {SafeAreaView, Text} from 'react-native'

export default function App() {
    const children = new Array(100)
    .fill(null)
    .map((notUsed, index) => <Text>Hello world! {index}</Text>)
    
    return <SafeAreaView>{children}</SafeAreaView>
}
```

100의 아이템을 undefined로 하는 배열을 만들어 이를 모두 null로 채우고(undefined 아이템에는 map을 적용할 수 없음) map을 통해 Text 컴포넌트를 만든다.

<br>

## faker 모듈

faker 모듈은 그럴듯한 가짜 데이터를 생성하는 것을 도와주는 패키지이다.

개발을 할 때 그럴듯한 아바타, 텍스트, 이미지 파일이 필요한데 이 패키지에서 이러한 다양한 가짜 데이터를 제공해 준다.

- 설치 : `npm i faker` , `npm i -D @types/faker`
- 우리는 타입스크립트를 사용하기 때문에 위 두 모듈을 다 다운 받는다.
- 참고로 모듈을 설치할 때는 항상 메트로 서버를 중단시키고 다운을 받아야 한다.(메트로 서버 실행 도중에는 node_modules가 락 상태가 되기 때문에)

## faker 모듈을 설치하면서 발생한 문제

계속해서 faker 모듈을 import 하면 선언되지 않았다고 하는 문제가 발생하였다.

-> 5.5.3 버전을 명시하여 설치하였더니 해결함

```shell
npm i faker
npm i faker@5.5.3
npm i @types/faker@5.5.3
```

<br>

## 가짜 데이터 생성

다음과 같은 파일들을 src/data 디렉터리 밑에 만든다.

- IPerson.ts: id, name, image 등 각 속성의 타입 정보를 저장
- util.ts: 배열을 만들고, 랜덤 숫자를 생성하고 unsplash.com에서 랜덤 이미지를 얻고, ui-avatar.com으로부터 이름에 따른 랜덤 아바타 이미지를 얻는 콜백함수들을 생성한다.
- faker.ts: 실제로 랜덤하게 얻을 데이터를 faker 모듈의 여러 함수를 통해 각 변수에 저장시킨다.
  - 널 병합 연산자: `??` 는 연산자 앞의 값이 null이나 undefined라면 연산자 뒤의 값을 사용하라는 의미의 연산이다.

- createRandomPerson.ts

  - ```typescript
    import type {IPerson} from './IPerson'
    import * as F from './faker'
    import * as U from './util'
    
    export const createRandomPerson = (): IPerson => {
        const anme = F.randomName()
        return {
            id: F.randomId()
            createdDate: new Date,
            ...
            counts: {
            	comment: U.random(10, 100),
                retweet: U.random(10, 100),
                heart: U.random(10, 1000)
        	}
        }
    }
    ```

  - 이제 그럴 듯한 가짜 데이터를 가진 IPerson 타입 객체는 createRandomPerson 함수를 호출하면 얻을 수 있게 되었다.



<br>

그리고 위와 같이 src/data 디렉터리의 파일을 사용하는 코드의 import 문을 간결하게 하고자 index.ts 파일을 다음처럼 작성한다.

```typescript
export * from './util'
export * from './faker'
export * from './IPerson'
export * from './createRandomPerson'
```

그래서 `import * as D from './src/data'`를 하면 위 파일을 모두 import 할 수 있게 된다.

<br>

이렇게 만든 객체는 JSX에서 사용할 때는 `<Text>JSON.stringify(person, null, 2)</Text>`와 같이 문자열로 변환하여 사용해야 하는데 이는 XML 구문에서 자식 요소는 문자열이거나 XML 요소여야 하기 때문이다.

그래서 JSON.stringify(객체, null, 2) 함수를 사용하여 2개의 공백 문자를 속성값에 붙여 보기 좋게 출력 되도록 한다.



<br>

## 리액트 네이티브가 제공하는 두 가지 서비스

1. 코어 컴포넌트
   - View, Text
   - 화면에 어떤 내용을 렌더링 해야 할 때
2. API
   - Platform, Alert
   - 폰의 하드웨어나 운영체제가 제공하는 기능이 필요할 때



### 사용자 컴포넌트

컴포넌트: UI를 담당하는 클래스

리액트 네이티브 프레임워크에서는 객체지향 방식인 클래스 컴포넌트를 사용했다.

하지만 그 이후 단순히 함수로 구현할 수 있는 함수 컴포넌트(Function Component)가 도입되었고 최근 리액트 훅 기능이 새로 도입되면서 근래에는 이 함수 컴포넌트와 리액트 훅을 사용할 것을 장려한다.

- 리액트 훅은 함수 컴포넌트에서만 사용 가능

<br>

**사용자 컴포넌트**: 리액트 네이티브가 제공하는 코어 컴포넌트를 화면에 렌더링해야 한다.

- 또한 앱 사용자의 화면 터치나 텍스트 입력 등을 이벤트 형태로 얻어 이에 따른 적절한 내용을 코어 컴포넌트에 알려 화면에 반영하는 일도 해야 한다.

<br>

### 클래스 컴포넌트 만들기

함수 컴포넌트를 주요 사용할 것이기 때문에 이 부분은 그렇게 중요하게 다루지 않아도 된다.

클래스 컴포넌트는 반드시 react 패키지가 제공하는 Component 클래스를 상속해야 한다.

그런데 컴포넌트를 제작하는 목적이 **코어 컴포넌트**를 화면에 렌더링하는 일이므로 리액트와 리액트 네이티브는 클래스 컴포넌트가 **render라는 이름의 메서드**를 가져야 한다고 강제한다 

- render 메서드가 반환하는 것은 다음으로 제한된다.
  - null
  - undefiend
  - React.createElement 호출로 얻은 반환값
  - JSX 문

- 사용 형식

  - ClassComponent.tsx

    - ```react
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

  - App.tsx

    - ```react
      import React from 'react'
      import {SafeAreaView, Text} from 'react-native'
      import ClassCompnenet from './src/screen/ClassComponent'
      
      export default function App() {
          return (
              <SafeAreaView>
                  <ClassComponent />
              </SafeAreaView>
          )
      }
      ```



<br>

### (화살표 방식) 함수 컴포넌트 만들기

보통 속성이 없는 컴포넌트는 function 키워드를 사용하여 작성하는 것이 편리하고 속성이 있는 경우 화살표 함수로 만드는 것이 편리하다.

우리가 앞서 작성했던 App.tsx 에서는 function 키워드만을 사용했는데 App 컴포넌트에는 속성이 없었기 때문이었다.

- 사용 형식

  - ClassComponent.tsx

    - ```react
      import React, {Component} from 'react'
      import {Text} from 'react-native'
      import * as D from '../data'
      
      const person = D.createRandomPerson()
      export default ArrowComponent = () => {
      	return <Text>{JSON.stringify(person, null, 2)}</Text>
      }
      export default ArrowComponent
      ```

  - App.tsx

    - ```react
      import React from 'react'
      import {SafeAreaView, Text} from 'react-native'
      import ClassCompnenet from './src/screen/ArrowComponent'
      
      export default function App() {
          return (
              <SafeAreaView>
                  <ArrowComponent />
              </SafeAreaView>
          )
      }
      ```

<br>

- 클래스 컴포넌트
  - Component 클래스를 사용해야 하고 render 메소드를 구현해야 하는 등 이것저것 알고 있어야 할 것이 많다.
- 함수 컴포넌트
  - 단순히 JSX 구문을 반환한다는 정도만 알면 충분하다.

따라서 함수 컴포넌트가 클래스 컴포넌트보다 만들기 더 편하다.

<br>

### 속성이란?

- **클래스의 멤버 변수**

리액트 네이티브는 컴포넌트의 속성이 바뀌면 이를 즉각 화면에 반영해야 한다.

리액트와 리액트 네이티브에서는 바뀐 속성값을 화면에 반영하는 것을 **재렌더링**한다고 한다.

따라서 속성이란 `클래스 속성 + 재렌더링`을 의미한다.

<br>

### JSX 속성 설정 구문

JSX는 XML이기 때문에 모든 속성은 따옴표(" " 혹은 ' ')로 감싸야 한다.

이에 따라 string 타입과 달리 age와 같은 number 타입은 string으로 되면 안되기 때문에 중괄호 기호로 감싸준다. (ex. age={23})

- 또한 객체 ({}로 묶여져 있는 형태의 구조) 도 { }안에 넣어준다.

<br>

### 속성을 왜 사용해?

앞의 ArrowComponent.tsx는 person이란 변수에 담긴 데이터를 화면에 렌더링 한다.

그런데 person 부분을 ArrowComponent.tsx가 아닌 App.tsx 쪽에 아래와 같이 구현하고 싶다고 하면 즉, 부모 컴포넌트 App의 데이터를 자식 컴포넌트 ArrowComponent 쪽으로 전달하고 싶다고 하면,

<mark>속성은 이처럼 부모 컴포넌트가 자식 컴포넌트 쪽으로 데이터를 전달하고 싶을 때 사용한다.</mark>



## 함수 컴포넌트의 타입

속성을 가진 함수 컴포넌트를 만들려면 먼저 함수 컴포넌트의 타입을 알아야 한다.

<br>

초반에 배웠던 createElement 함수 정의를 보면 FunctionComponent<p\> 와 ComponentClass<p\> 라는 제네릭 타입을 볼 수 있다.

여기서 타입 변수 p는 Property의 첫 글자이다.

그런데 FunctionComponent는 이름이 너무 길므로 react 패키지는 좀 더 간결한 이름인 FC 타입을 제공한다.

결론적으로 FunctionComponent는 FC 타입이라고 하고 함수 컴포넌트의 타입은 FC이다.

<br>

## import type 구문

타입스크립트 3.8 버전에 새로 등장한 import type 구문에 대해 알아보자

다음 코드에서 FC 타입은 import type 구문을 사용하지만 Component 클래스는 단순히 import 구문을 사용한다.

```react
import type {FC} from 'react'
import {Component} from 'react'
```

타입은 타입스크립트가 코드를 자바스크립트로 컴파일 할 때만 필요한 정보이다. 타입스크립트 코드가 자바 스크립트 코드로 컴파일 되고 나면 타입 관련 내용은 자바스크립트 코드에서는 완전히 사라진다.

이와 달리 클래스는 물리적으로 동작하는 메소드와 속성을 가지기 때문에 자바스크립트 코드로 변환해도 컴파일된 형태 그 자체 그대로 남아있다.

이러한 이유로 앞 코드에서 FC는 컴파일하면 사라지는 정보이기 때문에 import type 구문을 사용한다.

정리하면 import type을 사용하는 이유는 다음과 같다.

- FC와 같이 타입스크립트 컴파일 때만 필요한 타입을 명시하기 위해

<br>

## 함수 컴포넌트 구현하기 by using Typescript

- <Person person={person} /\> 형태의 코드가 가능하도록 Person 컴포넌트에 person 속성을 만드는 것을 목표로 한다.

<br>

- person 속성

  - ```react
    import React, {Component} from 'react'
    import {Text} from 'react-native'
    import * as D from '../data'
    
    export type PersonProps = {
        person: D.IPerson
    }
    const Person: FC<PersonProps>
    ```

- Person 컴포넌트의 함수 몸통

  - Person 컴포넌트의 함수 몸통은 1번방식으로 구현할 수 있지만 코드가 길기 때문에 `매개변수에 적용하는 비구조화 할당 구문`을 적용하여 2번과 같이 구현을 한다.

  - ```react
    const Person: FC<PersonProps> = (props) => {
        const {person} = props
        return <Text>{JSON.stringify(person, null, 2)}</Text>
    }
    ```

  - ```react
    const Person: FC<PersonProps> = ({person}) => {
        return <Text>{JSON.stringify(person, null, 2)}</Text>
    }
    ```

- 합친 코드

  - ```react
    import React, {Component} from 'react'
    import {Text} from 'react-native'
    import * as D from '../data'
    
    export type PersonProps = {
        person: D.IPerson
    }
    const Person: FC<PersonProps> = ({person}) => {
        return <Text>{JSON.stringify(person, null, 2)}</Text>
    }
    export default Person
    ```

    

이를 통해 <Person person={person} /\>와 같이 Person 컴포넌트에게 person 속성을 전달할 수 있게 되었다.

<br>

## ScrollView 코어 컴포넌트와 Key 속성

데이터를 한 화면 안에 모두 볼 수 없을 때 스크롤 기능이 있어야 하는데 리액트 네이티브에서는 ScrollView 코어 컴포넌트를 제공한다.

그래서 위에서 만든 Person 컴포넌트를 여러개 만들어서 보려고 하려면 Person 컴포넌트를 이 ScrollView의 자식 컴포넌트로 만들면 ScrollView가 제공하는 스크롤 기능을 이용해 모든 Person 컴포넌트를 볼 수 있다.

다음 App.tsx는 100개의 D.IPerson 타입 객체를 만들어 people 변수에 저장한다. 그리고 Person 컴포넌트 100개를 만들어 각 Person 컴포넌트의 person 속성에 D.IPerson 타입 객체를 전달한다.

```react
import React, { Children, Component } from "react"
import { SafeAreaView, ScrollView, Text } from "react-native"
import ClassComponent from "./src/screens/ClassComponent"
import ArrowComponent from "./src/screens/ArrowComponent"
import * as D from './src/data'
import Person from "./src/screens/Person"

//const person = D.createRandomPerson()
const people = D.makeArray(100).map(D.createRandomPerson)

export default function App() {
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

- 앞서 `데이터 배열을 컴포넌트의 배열로 만들기` 화면에서 'Each child in a list have a unique "key"...' 라는 경고 메시지가 나왔는데 이유는 다음과 같다.
  - 모든 리액트, 리액트 네이티브 컴포넌트는 **key**, children, ref 등 3개 속성을 **기본적**으로 가진다.
  - 이 중 key 속성은 리액트 프레임워크가 컴포넌트의 렌더링 속도를 최적화 하는 데 필요한 속성이다.
  - 해당 경고는 모든 자식 컴포넌트는 구분할 수 있는 키값을 가져야 한다. 라는 뜻이다.
  - 그래서 리액트 네이티브로 하여금 여러 컴포넌트 (위에서는 Person) 을 구분할 수 있도록 key 속성에 person.id 값을 설정한다.




<br>

## 컴포넌트의 이벤트 속성

사용자가 버튼을 터치하거나 텍스트를 입력했을 때 발생하는 이벤트를 처리하는 방법을 알아보자

<br>

### 이벤트 속성

- 리액트 네이티브 속성 중에는 onPress, onChangeText처럼 이름에 항상 'on~'이란 접두사가 붙는 속성이 있는데 이를 **이벤트 속성**이라고 한다.
- 이 이벤트 속성에는 항상 **콜백 함수**를 설정해야 하는데, 이를 이벤트 `콜백 함수` 또는 `이벤트 처리기`라고 한다.

<br>

### Button 코어 컴포넌트

```react
<Button title="home" color="blue" onPress={() => console.log('home pressed.')} />
```

- Button 코어 컴포넌트의 속성
  - onPress
  - title -> 반드시 설정해야 함
  - color

<br>

### Alert API

API: Application Programming Interface의 약자로 리액트 네이티브에서 API는 JSX 구문에서 사용되는 **코어 컴포넌트**와는 달리 타입스크립트 코드에서 사용하는 **기능**을 의미한다.

- 정적 메서드인 alert() 를 제공

  - static alert(타이틀, 메시지)

- ```react
  return (
  	<SafeAreaView>
      	<Button title='home'
              onPress={() => Alert.alert('home pressed.', 'message')} />
      </SafeAreaView>
  )
  ```

<br>

### 터처블 코어 컴포넌트

그런데 Button이 가지는 한가지 문제점은 디자인에 융통성이 없다는 것이다. 때문에 리액트 네이티브는 접두어 'Touchable~'이 붙는 다음 두 가지 코어 컴포넌트를 제공한다. 

```react
import {TouchableOpacity, TouchableHighlight} from 'react-native'
```

이 두 컴포넌트의 특징은 다음 2가지로 요약할 수 있다.

1) 컴포넌트 영역에 터치가 일어나면 onPress 이벤트 속성에 설정된 이벤트 핸들러 콜백 함수를 호출한다.
2) 단 한 개의 자식 컴포넌틈만 올 수 있다.

<br>

- TouchableOpacity: 터치가 일어나면 컴포넌트 바탕색의 투명도를 바꾸는 방식으로 효과를 줌.

사실 Text 컴포넌트도 onPress 이벤트 속성을 제공한다.

<br>

### TextInput 코어 컴포넌트

- TextInput 코어 컴포넌트의 특징

  1. defaultValue 속성에 초깃값을 설정할 수 있다.

  2. 입력된 텍스트는 value 속성값으로 얻을 수 있다.

  3. 텍스트가 입력될 때 **onChangeText** 이벤트 처리기를 실행한다.

  4. placeholder 속성을 사용하여 어떤 값을 설정해야 하는지 문자열로 출력할수 있다.

  5. editable 속성값에 false를 설정하면 입력을 못하게(disable)할 수 있다.

  6. keyboardType 속성에 'default', 'numeric', 'email-address'등의 값을 설정할 수 있다.

  7. 포커스를 가지게 하는 focus 메서드와 포커스를 잃게 하는 blur 메서드가 있다.

  8. 텍스트를 입력할 수 있는 상태(포커스를 가진 상태)가 되면 onFocus 이벤트를 호출하고 텍스트를 입력할 수 없는 상태(포커스를 잃은 상태) 가 되면 onBlur 이벤트를 호출한다.

  9. 텍스트 입력이 모두 끝나면 onEndEditing 이벤트를 호출한다.

  10. 자식 요소를 가지지 못한다.

onChangeText 속성에 설정할 수 있는 콜백 함수는 다음과 같은 함수 시그니처(function signature)를 가진다.

```
함수 시그니처
onchangeText(text: string) => void
```

- 함수 시그니처: 
  - 타입스크립트 언어에서 모든 변수는 어떤 타입을 가진다. 그리고 함수도 어떤 타입을 가진다.
  - 여기서 함수 선언문에서 함수 이름만 제외한 부분을 함수 타입, 즉 함수 시그니처라 한다.

<br>

- onFocus와 onBlur, onEndEditing에는 아무런 매개변수도 없는 콜백 함수를 설정한다. 다음 코드는 TextInput의 전형적인 사용 예이다.

  - ```react
    import React from "react"
    import { SafeAreaView, Alert, Button } from "react-native"
    import {TouchableOpacity, TouchableHighlight, Text} from 'react-native'
    import {TextInput} from 'react-native'
    
    const onPress = () => Alert.alert('home pressed.', 'message')
    
    export default function App() {
      return (
        <SafeAreaView>
          <Button title='home' onPress={onpress} />
          <TouchableOpacity onPress={onpree}>
          	<Text>TouchableOpcity</Text>    
          </TouchableOpacity>
          <TouchableHighlight onPress={onpree}>
          	<Text>TouchableHighlight</Text>    
          </TouchableHighlight>
          <TextInput
              placeholder="enter your name"
              onChangeText={(text: string) => console.log(text)}
              onFocus={() => console.log('onFocus')}
              onBlur={() => console.log('onBlur')}
              onEndEditing={() => console.log('onEndEditing')}
              />
        </SafeAreaView>
      )
    }
    ```

  - 이 코드는 TextInput을 터치하여 포커스를 가지게 한 것이다. TextInput이 포커스를 가지면 화면 아래에서 위쪽으로 키보드가 올라온다.

  - 메트로 서버를 실행하고 에뮬레이터에서 해당 코드가 실행되면 아래의 과정이 이루어 짐을 알 수 있게 된다.

    - TextInput을 터치하면 onFocus 이벤트가 발생한다.
    - 'Hello'를 입력하는 동안 onChangeText 이벤트가 5번 발생하고 현재 TextInput에 표시한 텍스트를 출력한다.
    - 그리고 'Enter' 입력은 onEndEditing 이벤트를 발생시킨다.
    - 화면 다른 곳을 터치하면 TextInput은 포커스를 잃게 되어 onBlur 이벤트가 발생한다.

  - ```
    onFocus
    H
    He
    Hel
    Hell
    Hello
    onEndEditing
    onBlur
    ```

