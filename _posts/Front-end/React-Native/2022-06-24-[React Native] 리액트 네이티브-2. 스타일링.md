---
layout: single
title: "[React Native] 리액트 네이티브-2. 스타일링"
categories: ['Front-end', 'React-Native']
toc: true
toc_sticky: true
---

<br>

## 3.1 Style 속성과 StyleSheet API 이해하기

style을 적용하는 방법은 두 가지가 있다.

1. 인라인 스타일 설정
2. StyleSheet API 사용

<br>

### 스타일 속성과 스타일 객체

style 속성에 style 객체를 설정한다. (Not string)

```react
<SafeAreaView style={{}}>

</SafeAreaView>
```

- 안쪽 중괄호는 객체를 의미하는 문법, 바깥쪽 중괄호는 JSX 구문에서 자바스크립트 코드를 설정할 때 쓰는 문법

<br>

- 스타일 객체가 가지는 속성을 스타일 속성이라고 한다.

- 스타일 속성에는 요가 엔진이 지정한 이름만 사용할 수 있으며 소문자로 시작한다.

- 스타일 속성 설정 값이 배열이면 배열 안의 스타일 객체를 모두 결합하여 하나의 스타일 객체로 만들어준다.

  - style 속성에 배열 설정 구문

  - ```
    <컴포넌트_이름 style={[스타일_객체1, 스타일_객체2, ...]} />
    ```

- View 컴포넌트(코어 컴포넌트 중 'View' 자가 들어간 컴포넌트) backgroundColor 속성으로 자신의 바탕색을 설정할 수 있다.
  - Text는 자신의 바탕을 설정할 수 없지만 color 속성을 글자색은 바꿀 수 있고 해당 텍스트를 View 컴포넌트로 감싸면 가능하다.

<br>

### Sytle Sheet API

위에서 style 속성에 설정한 스타일 객체를 인라인 스타일이라고 하는데 style 속성에는 인라인 스타일 외에도 StyleSheet API를 사용할 수 있다.

<br>

StyleSheet는 create 메서드를 제공하는데, 이 메서드를 사용하여 캐시된 스타일 객체를 생성할 수 있다.

- StyleSheet.create 메서드 사용법

  - ```react
    const styles = StyleSheet.create({
        키_이름1: 스타일_객체1
        키_이름2: 스타일_객체2
        (... 생략 ...)
    })
    ```

    

- StyleSheet.create의 목적이 자바스크립트 언어로 만든 스타일 객체를 네이티브 모듈 쪽으로 옮겨 주는 것이므로 **여러 번 호출**하여 스타일 객체 여러 개를 일일히 전달하는 것보다는 한꺼번에 전달하는 것이 효율적이다. 

- StyleSheet.create 함수는 매개변수에 설정된 스타일 객체를 네이티브 모듈 쪽에 전달한다. 
  - 매개변수에는 키와 이에 해당하는 스타일 객체 쌍을 여러 개 만드는 방식으로 사용한다.
- 그리고 네이티브 모듈 쪽은 이렇게 전달 받은 스타일 객체를 자신의 로컬 저장소에 보관한다.(즉, 캐시 형태로 저장한다.). 이후 네이티브 모듈 쪽의 렌더링은 로컬 저장소에 보관된 스타일 객체를 참조하므로 불필요한 자바스크립트 엔진 스레드와 네이티브 UI 스레드 간의 데이터 전송이 일어나지 않아 전체적인 렌더링 속도가 빨라진다.

<br>

### 인라인 스타일과 StyleSheet 스타일의 차이

컴포넌트는 필요에 따라 리액트 네이티브에 의해 재렌더링된다. 재렌더링은 상황에 따라 반복해서 발생하는데 이런 상황을 고려하여 **인라인 스타일 방식**은 자바스크립트 엔진 쪽 스레드에서 UI 스레드 쪽으로 **브리지를 경유**하여 옮겨 가므로 내용이 컴포넌트 로직에 의해 바뀌지 않을 때는 앱의 디스플레이 속도가 떨어진다.

반면 StyleSheet.create로 생성된 스타일 객체는 UI 스레드 쪽에 캐시되므로 앱 전체의 디스플레이 속도가 빨라진다. 

<mark>그래서 내용이 변하지 않는 스타일(즉, 정적 스타일) 객체는 StyleSheet .create 방식으로 구현하는 것이 효과적이고 컴포넌트 구현 로직에 따라 동적으로 변하는 스타일 객체는 인라인 스타일 방식으로 구현하는 것이 일반적이다.</mark>

<br>

### 구글 material design 색상과 react-native-paper 패키지

CSS에서 색상은 16진수 RGB(#fff, #21963f)로 표현 되어 있는데 이를 white or Colors.blue 와 같이 하도록 도와주는 패키지이다.

#### 구글 머티리얼 디자인 가이드

![image](https://user-images.githubusercontent.com/79521972/176381923-d40021d4-4a43-4445-a139-2df6044c2c60.png)

구글은 모든 안드로이드 앱을 가능한 한 이 가이드라인에 따라 디자인 할 것을 권고한다.

구글 색상표에서 세로줄의 색 이름은 색상을 HSL 방식으로 표현하고 나서 Hue 값을 30도씩 회전해 만든 것이다. 이와는 달리 가로줄은 Light를 조금씩 어둡게 해가며 만든 것이다.

react-native-paper 란 패키지는 Colors라는 심벌을 제공하는데 이 심벌을 이용하면 구글 색상표에서 Blue열 500번 색상을 Colors.blue500처럼 표현할 수 있다.

<br>

#### color 패키지

- 구글 머티리얼 디자인은 앱의 색상을 주 색상과 보조 색상으로 나눠서 애브이 테마 색상을 결정할 것을 권고한다.

- 앱의 테마 색상이 결정되면 글자가 모든 색상에서 잘 보일 수 있도록 글자 색상도 잘 결정해야 한다.

- color 패키지는 위 경우에 어떤 색보다 좀 더 밝게 또는 어둡게 하거나 투명도를 가감하여 좋은 색상을 정하는데 유용하다.

```react
text: {fontSize: 20, color: Color(Colors.blue500).lighten(0.9).string()}
```

다음과 같이 Color로 감싼 색상에 대해서 가장 잘 보이는 색으로 대치해 주는 것이다.

<br>

## View 컴포넌트와 CSS 박스 모델

리액트 네이티브는 각종 'View' 가 들어간 컴포넌트를 제공한다. 이는 여러 개의 리액트 네이티브 컴포넌트를 자식 요소로 가질 수 있고 화면 UI와 레이아웃과 스타일링을 담당한다.

<br>

### Platform과 Dimensions API

- **Platform API**
  - 현재 앱이 실행되는 폰이 무엇인지를 알기 위한 모듈

- **Dimensions API**
  - 현재 실행된 폰의 화면 크기를 알아야 할 때 사용하는 API
- alpha: 투명도
- lighten: 밝기

- 궁금증: import 문에서 {}의 의미는? -> 할 때도 있고 안 할 때도 있고
  - 차이점은 보내주는 export 방식의 차이이다.
  - 모듈을 불러올 때 import라고 쓰는 것처럼 모듈을 다른 파일에 보내려면 export라고 명시해야 한다.
  - 이 때 다른 모듈로부터 가져오는데 해당 모듈에서 이를 export할 때 default(해당 모듈 자체를 import하면 내보내지는 것)로 내보낸 것은 괄호가 필요없지만 나머지의 경우 필요하다.

<br>

### 뷰 컴포넌트의 backgroundColor 스타일 속성

---

- import 문 쪽에 `// prettier-ignore`를 작성해 놓으면 prettier가 불필요하게 import 문을 여러 줄로 포맷하는 것을 방지한다.

- CSS와 달리 리액트 네이티브의 fontSize 스타일 속성에는 반드시 number 타입이나 undefined를 값으로 설정해야 한다.

---

그래서 스타일을 설정한 뷰 컴포넌트의 자식 컴포넌트로 텍스트를 넣게되면 뷰 컴포넌트의 바탕색이 화면 전체가 아니라 단순히 Text의 높이 부분까지만 덮는다.

- 바로 아래에서 이것에 대한 이유가 나온다.(1번 방법)

<br>

### View의 기본 width값과 height값

리액트 네이티브는 CSS 박스 모델을 적용한 컴포넌트를 사용하여 width와 height 스타일 속성으로 자신의 크기를 설정할 수 있다.

- 이 widht와 height 스타일 속성값에는 다음 4가지 방법 중 하나를 골라 설정하도록 한다.
  1. 명시적으로 widht, height를 설정하지 않고 리액트 네이티브의 기본 설정(default) 방식을 따르는 방법
  2. 픽셀(pixel, px) 단위의 숫자를 직접 설정하는 방법
  3. 부모 요소의 width, height를 기준으로 자식 컴포넌트의 크기를 퍼센트(%)로 설정하는 방법
  4. flex 속성을 사용하여 여러 자식 컴포넌트가 부모 컴포넌트의 크기를 분할하여 가지는 방법

<br>

#### 1. 명시적으로 widht, height를 설정하지 않고 리액트 네이티브의 기본 설정(default) 방식을 따르는 방법

SafeAreaView와 같은 뷰 컴포넌트에 width와 height 스타일 속성값을 직접 명시하지 않았을 때 리액트 네이티브는 'View'의 width는 부모 컴포넌트의 width를 그대로 설정하고 `height는 자식 요소의 height를 'View'의 height로 설정한다.`

- 자식 요소를 수평으로 배치한 경우 -> 자식 요소 중 가장 높이 있는 요소의 height 값
- 자식 요소를 수직으로 배치한 경우 -> 자식 요소의 height 값을 모두 더한 값

그래서 앞에서 SafeAreaView의 높이가 수직으로 배열된 자식 요소 3개의 높이를 모두 더한 값이 리액트 네이티브에 자동 설정 된 것이다.

<br>

#### 2. 명시적으로 픽셀 단위의 값을 설정하는 방법

앞서 Dimensions API를 이용해 폰의 크기를 width와 height에 저장한 바가 있다. 따라서 Dimensions.get('window')를 통해 얻은 값으로 height를 설정할 수 있다.

- 이 과정에서 'height: height'와 같이 작성을 할 텐데 이는 height 단어 하나로 단축하여 쓸 수 있다.
  - 속성이름과 값을 담은 변수 이름이 똑같을 때는 생략할 수 있는 단축 구문을 제공함.

<br>

#### 3. 부모 요소의 크기를 기준으로 퍼센트를 설정하는 방법

- 가장 바깥의 'View' 컴포넌트(코어 컴포넌트)의 부모는 App(사용자 컴포넌트)임.
  - 하지만 App과 같은 사용자 컴포넌트는 렌더링에 참여하지 않고 리액트 네이티브 코어 컴포넌트만 화면에 직접 렌더링 함.
  - 그래서 렌더링 관점에서만 봤을 때는 SafeAreaView의 부모 컴포넌트는 App이 아니라 네이티브 쪽 모듈에서 생성된 자바나 오브젝티브-C로 구현한 네이티브 컴포넌트가 된다.
  - 그리고 이 네이티브 컴포넌트의 크기는 폰의 크기와 같다.
    - 즉, Dimensions.get('window')의 반환값은 네이티브 모듈 쪽의 최상위 컴포넌트의 크기이다.

- CSS에서 퍼센트값은 항상 부모 컴포넌트의 크기를 기준으로 봤을 때의 비율이다.
  - height='100%'의 의미는 네이티브 쪽 최상위 컴포넌트 height 값의 100%라는 의미이다.



<br>

### 4. flex 스타일 속성

- 앞서 width, height 스타일 속성(100%, 50% 등으로 설정하던 것)을 flex 스타일 속성에 적용하면 1은 100%를 0.5는 50%를 의미하도록 할 수 있다.

- 간혹 컴포넌트 스타일을 할 때 flex와 (width, height)를 동시에 사용하는 경우가 있다.

  - 이 경우 flex가 더 낮은 우선순위를 갖음.

  - ```react
    <View style={[flex: 1, height: 50]} />
    ```

  - 위 코드에서는 따라서 컴포넌트의 height는 부모 요소의 height값(flex: 1)이 아니라 height 스타일 속성값 50이 설정된다.



<br>

### margin

대부분 코어 컴포넌트에는 margin이라는 스타일 속성을 설정할 수 있다.

margin 스타일 속성은 부모/자식 간 혹은 이웃한 형제 요소간의 간격을 조정한다.

- marginLeft와 marginRight 값이 같은 상황(즉, marginLeft=marginRight일 때)이면
  -  **marginHorizontal** 스타일 속성으로 이 둘을 동시에 설정할 수 있으며

- marginTop=marginBottom 상황이면 
  - **marginVertical**을 사용하여 두 값을 한꺼번에 설정할 수 있다.

- marginHorizontal=marginVertical 상황이면
  - **margin** 속성을 사용하여 이 두 값을 한꺼번에 설정할 수 있다.




<br>

### padding

padding 스타일 속성은 부모/자식 간의 관계에서 **부모 컴포넌트 쪽에 적용하는 스타일 속성**이다. 

대부분 부모 컴포넌트 내부에 자식 컴포넌트를 배치할 때 자식 컴포넌트가 자신의 영역을 꽉 채우지 않고 간격을 주는 것이 시각적으로 좋다.

<br>

- paddingLeft와 paddingRight 값이 같은 상황(즉, padidngLeft=paddingRight일 때)이면
  -  **paddingHorizontal** 스타일 속성으로 이 둘을 동시에 설정할 수 있으며
- paddingTop=paddingBottom 상황이면 
  - **paddingVertical**을 사용하여 두 값을 한꺼번에 설정할 수 있다.
- paddingHorizontal=paddingVertical 상황이면
  - **padding** 속성을 사용하여 이 두 값을 한꺼번에 설정할 수 있다.

안드로이드에서 SafeAreaView는 단순히 View로 동작하지만 iOS에서 SafeAreaView는 View가 아니기 때문에 iOS에서는 padding 스타일 속성은 동작하지 않는다.

<br>



### margin vs. padding

margin과 padding의 차이는 아래 그림으로 명확히 알 수 있다.

![image](https://user-images.githubusercontent.com/79521972/176390398-ec6e6117-0b1a-45a4-b2f0-6bbbd96190a6.png)

<br>

### border 관련 속성

- 리액트 네이티브 코어 컴포넌트는 **대부분** 자신 영역의 **경계**를 설정할 수 있는 다음과 같은 스타일 속성을 사용할 수 있다.
  - **borderWidth**: border 넓이
    - borderLeftWidth, borderRightWidth, borderTopWidth, borderBottomWidth
  - **borderColor**: border 색상
    - borderLeftColor, borderRightColor, borderTopColor, borderBottomColor
  - **borderRadius**: border 모서리 둥근 정도를 의미
    - borderTopLeftRadius, borderTopRightRadius, borderBottomLeftRadius, borderBottomRightRadius
  - **borderStyle**: 실선, 점선 등 border 스타일을 의미
    - 'solid', 'dotted', 'dashed'

<br>

- 궁금증: 만든 스타일객체로 스타일을 설정하는데 객체 안에 이미 들어 있는 속성을 배열에 다른 객체를 추가하여 더하여도 추가한 걸로 적용이 되는가?
  - 된다.

<br>

### Platform.select 메서드

platform 별로 다른 스타일을 적용할 수 있다.

```react
paddingLeft: Platform.select({ios: 0, android: 20})
```



<br>

## 3-3. 자원과 아이콘 사용하기

필수 자원은 앱에 포함하여 배포를 해야하기 때문에 사용하는 자원과 아이콘이 필요하다.(서버가 오프라인일 때 앱이 디자인대로 보이지 않음 - 로고 같은 거)

```
npm i react-native-vector-icons
npm i -D @types/react-native-vector-icons
```



### ImageBackground 코어 컴포넌트 사용하기

- 리액트 네이티브에서 이름에 'Image'자가 포함된 Image나 ImageBackground 컴포넌트는 항상 source 속성에 require를 사용하여 읽는 방식으로 파일을 설정해야 한다. 
  - bg.jpg 와 같은 파일이 있다고 하면 이 이미지 파일 자원은 별도의 파일이 아닌 소스 코드에 삽입된 형태로 배포된다는 것을 알 수 있다.
- Image와 ImageBackground를 사용하려면 반드시 width와 height 스타일 속성값을 설정해야 한다.



<br>

## 폰트 직접 설치하고 사용하기

구글에서 'dancing script'를 검색하여 나온 사이트에서 폰트를 다운 받아 src 디렉터리 밑에 assets 폴더의 fonts 디렉터리에 다운 받은 4개의 파일을 저장한다. 

src/assets/fonts 디렉터리의 자원 파일은 앱에 포함하려면 `npx react-native link`라는 명령을 실행해야 한다. 그런데 이 명령은 다음 구성 파일을 필요로 한다.

- react-native.config.js

해당 파일의 내용은 다음과 같이 작성한다.

```react
module.exports = {
  project: {
    ios: {},
    android: {},
  },
  assets: ['./src/assets/fonts'],
};

```

여기서 중요한 점은 ios와 android라는 키를 설정하고 있는데, 이 키는 link 명령이 ios와 android 디렉터리를 대상으로 동작해야 한다는 것을 지정하기 떄문에 project 키가 반드시 있어야 하는 것이다.

<br>

### 갑자기 발생한 delete 'CR' 문제

모든 행에서 prettier가 delte 'CR' 문제를 일으켰다. 이를 해결하기 위해서는 vscode 하단 부분의 화살표가 가리킨 부분을 CRLF에서 LF가 되도록 바꾸면 된다.

![image](https://user-images.githubusercontent.com/79521972/176086918-2de7bd4f-1ef7-4a21-a47f-5dd2deb68e2e.png)

```
CR : 현재 커서를 줄 올림 없이 가장 앞으로 옮기는 동작 
LF : 커서는 그 자리에 그대로 둔 상황에서 종이만 한 줄 올려 줄을 바꾸는 동작
```



<br>

## 3-3. 자원과 아이콘 사용하기

모바일 앱 개발을 함에 있어서 자원(assets, resource)은 앱에 **포함**하여 배포하는 이미지, 폰트, 아이콘 등의 **파일**을 의미한다.

그런데 모바일 앱은 통ㅅ니이 끊어지는 오프라인 상황을 염두에 두어야 한다. 회사 로고 이미지나 폰트, 아이콘 등을 원격지 서버에서 가져오는 방식으로 구현하면 오프라인일 때 앱이 디자인대로 보이지 않는 문제가 발생한다.

때문에 필수적인 자원은 앱에 포함해 같이 배포해야 한다.

<br>

### ImageBackground 코어 컴포넌트 사용하기

```react
<ImageBackground style={{flex: 1}} source ={require('./src/assets/images/bg,jpg')} />
```

리액트 네이티브에서 이름에 'Image'자가 포함된 Image나 ImageBackground 컴포넌트는 항상 source 속성에 require를 사용하여 읽는 방식으로 파일을 설정해야 한다.

따라서 위 코드는 bg.jpg와 같은 이미지 파일 자원은 별도의 파일이 아닌 소스 코드에 삽입된 형태로 배포된다.

또한 Image나 ImageBackground를 사용하려면 반드시 width와 height 스타일 속성값을 설정한다.

ImageBackground는 화면 전체를 덮는 방식으로 사용하는 컴포넌트이므로 flex: 1 스타일 속성을 지정하여 이미지의 width와 height를 설정한다. 그런데 이 코어 컴포넌트는 비록 이름에 'View'가 없지만 'View'자가 들어간 컴포넌트처럼 자식 컴포넌트를 가질 수 있다.

<br>

### Image 코어 컴포넌트

Image 코어 컴포넌트는 ImageBackground처럼 이미지 파일을 화면에 렌더링하는 기능을 제공한다.

하지만 Image는 ImageBackground와 달리 자식 컴포넌트를 가질 수 없다.

그리고 Image와 ImageBackground에 앱의 자원이 아닌 다른 원격지 서버에서 파일을 내려받아 렌더링할 때는 다음 코드에서 보듯 source 속성에 {uri: 이미지_파일\_웹\_주소} 형태의 객체를 설정해야 한다.





### react-native link 명령으로 폰트 자원 링크하기

이미지 파일과 달리 폰트 파일은 반드시 `npx react-native link`명령을 실행해야 사용할 수 있다.

그러나 이는 현재 일자 기준(22-06-28)으로 더 이상 지원되지 않는 서비스라 하여 autolinking이 되어 그냥 src/assets/fonts 에 폰트를 저장하고 사용하면 된다.



<br>

## flex: 1의 의미

만약 View 컴포넌트에 있던 flex: 1 스타일 속성이 없애면 

- 'ImageBackground의 heigt-(paddingTop + paddingBottom) - Image의 height - Icon의 height 만큼의 높이 여분이 생긴다.

ImageBackground와 같은 부모 컴포넌트의 Image, View, Icon 등 자식 컴포넌트 중에 flex 스타일 속성에 0이 아닌 값을(1, 2 ...) 가지는 컴포넌트가 **없다면** 이 여분은 그대로 남을 것이다.

하지만 지금처럼 View가 flex: 1로 설정되면 ImageBackground와 같은 부모 컴포넌트의 height 여분이 모두 flex: 1인 컴포넌트의 높이가 된다.

따라서 View는 이 높이 여분을 자신의 높이로 삼으므로 View 아래에 있는 Icon은 화면 아래에 위치하게 된다.

<br>

정리하자면 이렇게 flex 속성을 설정하지 않아서 생기는 남은 여분에는 아무런 컴포넌트도 존재하지 않게 되는 것이다.

<br>

## 3-4. 컴포넌트 배치 관련 스타일 속성 탐구하기

### flex: 1과 height: '100%'의 차이

Content 부분의 높이를 flex와 height로 각각 하였을 때 어떤 차이가 있는지 알아보았다.



![image](https://user-images.githubusercontent.com/79521972/176095861-2ace527c-7f2b-4add-9e03-0b142011a125.png)

이것이 height: '100%' 로 설정했을 때이고

<br>

![image](https://user-images.githubusercontent.com/79521972/176095815-30e15bd2-1a02-49e7-ad23-04fb7991058b.png)

이것이 flex: 1 로 설정했을 때의 모습이다.

<br>

flex: 1 스타일 속성을 부여하면 Content의 형제 요소인 TopBar와 BottomBar의 높이가 반영된 부모 컴포넌트의 높이를 가져온다.

- 즉 (부모-Topbar-BottomBar)

이에 반해 'height: '100%'는 TopBar와 BottomBar의 높이와 무관하게 부모 컴포넌트의 높이를 모두 가져오므로 (즉, Content의 루트 View의 height에 Dimensions.get('window').height값을 설정한 것과 같은 효과) Content의 높이가 너무 커져 BottomBar가 화면에 나타나지 못하게 된 것이다.



<br>

### 여러 개의 형제 컴포넌트가 모두 0이 아닌 flex값을 가질 때의 효과

flex의 값을 각 형제에게 0, 1, 2로 주면

Content의 루트 View는 현재 flex: 1 스타일링이기 때문에 '화면 높이 - (TopBar 높이 + BottomBar 높이) ' 만큼의 높이가 된다. 

이러한 상황에서 첫 번째 flex: 0인 View는 flex 스타일 속성값이 0이므로 이것은 부모 컴포넌트의 높이를 전혀 나눠 갖지 않고 자식 컴포넌트인 
`<Text>flex: 1</Text>`의 높이를 자신의 높이로 가지겠다고 의사표시를 한다. 따라서 이제 여분의 높이는 이것이 차지하는 높이를 뺀 것이 된다.

그런데 나머지 두 개의 View는 각각 flex: 1, 2로 설정했는데 이럴 때 리액트 네이티브의 요가 엔진은 여분 높이를 각각 `여분 높이/(1+2)x1`, `여분 높이/(1+2)x2`로 나누어 이 두 개 View 각각의 높이로 설정한다.

그리고 이런 비율 계산법이 적용되므로 flex 속성값을 1, 2가 아닌 100,200 으로 설정해도 비율이 같으므로 같은 결과가 될 것이다.

<br>

### flexDirection 스타일 속성

플렉스박스 레이아웃은 부모 컴포넌트의 크기가 고정일 때 자식 컴포넌트를 자신의 영역에 배치하는 기법이다. 그런데 플렉스박스 레이아웃 기능은 부모 컴포넌트가 자식 컴포넌트를 배치할 때 수평이나 수직 방향 한쪽으로만 가능하다.

그리고 이 방향은 flexDirection이란 스타일 속성으로 조정할 수 있다.

- 'row'
- 'column'

이 두 가지로 설정할 수 있고 기본(default)값은 column이다. 그 동안 우리가 배치했던 컴포넌트가 배치되던 방식을 생각하면 된다.(bottomBar가 가장 밑에 있던 이유)

<br>

탑바를 다음과 같이 구성하기 위해서 이미지와 아이콘 사이에 텍스트를 row 형태로 배치하는데 이 때 텍스트는 이미지와 아이콘 사이 공간을 모두 차지하고 싶음에도 Text 컴포넌트는 flex 스타일 속성을 부여할 수 없다. 

그렇기 때문에 View 컴포넌트가 이를 감싸고 이 View 스타일 속성에 flex: 1로 설정을 하면 된다.

<br>

다음으로 계속해서 여러가지 스타일 속성에 대해서 알아보도록 하겠다.



- #### alignitems 스타일 속성

  - 부모 요소의 높이나 넓이에 여분이 있을 때 이 여분을 이용하여 자식 요소의 배치 간격을 조정하는데 사용

  - ```
    left, center, right, stretch
    ```

  - stretch 값은 부모 컴포넌트의 크기에 여분이 있으면 자식 컴포넌트의 크기를 늘림.

    - stretch가 동작하려면 자식 컴포넌트 크기는 고정되지 않아야 한다.

  - 그런데 alignItems는 flexDirection 속성값에 따라 동작 방향이 달라진다. 앞서 Topbar를 구성할 때 view는 flexdirection 속성값이 'row'였다.

    - flexDirection 속성값이 'row'라면 alignItems는 자식 컴포넌트의 수직 방향 배치에 영향을 준다. 
    - 반대로 flexDirection 속성값이 'column'이라면 alignItems는 자식 컴포넌트의 수직 방향 배치에 영향을 준다. 

- #### jutisfyContent 스타일 속성

  - flexDirection에 따라 진행 방향이 달라짐

  - ```
    flex-start, center, flex-end, space-around, space-between, space-evenly
    ```

  - flex-start: 앞쪽에 몰아서

  - flex-end: 끝쪽에 몰아서

  - center: 중앙에 몰아서

    - 위 세 개는 부모 요소의 수평 방향 여백을 자식 요소 간의 간격에 전혀 반영하지 않았다.

  - space-around: 폰 양 끝에 padding을 적용함

  - space-between: 폰 양 끝에 padding을 적용하지 않음

  - space-evenly: 여분 넓이를 균등하게 부여함 -> `부모 컴포넌트 넓이 -(자식 컴포넌트 넓이 합) 
    -> 공백수가 자식 컴포넌트 수보다 하나 많으므로 (자식 컴포넌트 수 + 1)로 나누어 얻은 여분 넓이를 균등하게 부여하는 방식

    - 위 세 개는 부모 요소의 여백을 자식 요소의 간격에 반영한다.

- #### flexWrap 스타일 속성

  - 폰 화면의 폭보다 길이가 긴 요소들을 렌더링 할 때 줄을 바꿔가면서 렌더링 하도록하는

  - ```
    nowrap, wrap, wrap-reverse
    ```

- #### overflow 스타일 속성

  - 전체 콘텐츠의 크기가 컴포넌트 크기보다 클 때 이를 어떻게 할 지를 결정하는 속성

  - ```
    visible, hidden, scroll
    ```

  - visible이면 콘텐츠는 컴포넌트의 크기와 무고나하게 컴포넌트 바깥쪽으로도 렌더링된다.
  
  - hidden이면 오른쪽이면 컴포넌트 바깥쪽에 렌더링되지 않는다.
  
  - 그런데 웹과는 달리 overflow에 scroll을 설정해도 스크롤 효과는 발생하지 않는데 
  
    - 리액트 네이티브에서 스크롤은 ScrollView나 FlatList 코어 컴포넌트 위에서만 가능하기 때문이다.
  



<br>

### ScrollView의 contentContainerStyle 속성

- ScrollView는 다른 코어 컴포넌트와 달리 style 이외에 contentContainerStyle 속성을 **별도로** 제공한다.
  - contentContainerStyle 속성은 스크롤 대상 콘텐츠 컴포넌트에 적용되는 속성이다. 
  - 이 속성을 설정할 때는 flex: 1 부분이 없어야 스크롤이 정상 작동한다.

<br>



## 화면에 뜬 효과 주기: floating action button(FAB)

- 화면 어딘가에 떠 있는 아이콘혹은 이미지 효과를 줄 수 있는 것으로 floaing action button이라고 한다.
- 이런 FAB 효과를 주려면 <mark>position, left, right, top, bottom과 같은 스타일 속성과 리액트가 제공하는 Fragment 컴포넌트를 반드시 이해해야 한다.</mark>



<br>

### React.Fragment 컴포넌트와 `<></>` 단축 구문

먼저 floating icon을 만드는 방법은 다음과 같다.

```react
<View style={[styles.absoluteView]}>
	<Icon name="feather" size={50} color="white" />
</View>
```



이를 App.tsx에 bottombar 컴포넌트 아래에 추가하면 다음과 같이 되는데 이 이유는 아이콘이 SafeAreaView의 자식 컴포넌트 형태로 JSX가 구성되기 때문이다.

요컨데 **FAB 효과를 주려면 아이콘이 SafeAreaView의 자식 컴포넌트여서는 안된다**. 그런데 계속 다뤘던 내용에서는 반드시 SafeAreaview가 최상위 컴포넌트여야만 했던 것을 알 수 있다.

리액트에서는 이런 딜레마를 해결하기 위해서 Fragment 컴포넌트를 제공한다.

JSX는 XMl 문이기 때문에 다음처럼 부모 컴포넌트 없이는 여러 개의 컴포넌트가 올 수 없다.

```react
<SafeAreaview />
<View />
```

<br>

Fragment는 실제 존재하지는 않지만 XML 문법이 요구하는 부모 컴포넌트로 동자가도록 만들어진 컴포넌트이다. 즉, 동작하는 방식이 다음과 같은 것이다.

```react
<Fragment>
    <SafeAreaView />
    <View />
</Fragment>
```

<br>

그런데 Fragment는 이름이 좀 긴 감이 있기 때문에서 인지 `<></>`와 같은 단축 구문을 제공한다.

```react
<>
    <SafeAreaView />
    <View />
</>
```

<br>

이를 적용하여 FAB을 구성하여도 별 차이는 없어보인다.

<br>

## left, right, top, bottom과 position 스타일 속성

이 스타일 속성들은 컴포넌트가 렌더링되는 위치를 바꾸고 싶을 때 사용한다.

그런데 이 스타일 속성은 position 스타일 속성은 position 스타일 속성값을 'absolute'로 지정해야 비로소 설정값이 컴포넌트에 반영된다.(절대적인 위치를 지정하고 싶은 것이기 때문일 것으로 추정)

- position: 'absolute'

예를 들어 left 스타일 속성에 10과 같은 값을 설정하면 컴포넌트 왼쪽으로 기준으로 10픽셀 오른쪽으로 이동해 컴포넌트가 렌더링 되는 식이다.

<br>

최종적으로 FAB 아이콘을 띄우는 방법은 다음과 같다.

- View로 Icon을 감싼다.
- View의 스타일 속성에는 position을 'absolute'로 설정해 주고 아이콘이 띄워질 절대적인 위치를 적절한 픽셀값만큼 옮긴다.
- 초기에는 좌측 상단에 있기 때문에 이 위치를 기준으로 옮기면 되는데,(그래서 right, bottom만 건들이면 된다.)
  - android의 경우, `right: 30, bottom: 80`
  - ios의 경우, `right: 30, bottom: 100` 



<br>

## 재사용 할 수 있는 컴포넌트 만들기

JSON.stringify 가 아닌 실제로 컴포넌트를 스타일링하는 방법을 알아보자

<br>

### 필요한 패키지

```
faker
@types/faker
react-native-vector-icons
@types react-native-vector-icons
moment
```

<br>

moment 패키지는 보기 좋게 날짜를 출력할 수 있도록 도와주는 패키지 이다. 다운을 받기 위해서는 다음과 같은 명령어를 입력하면 된다.

```shell
npm i moment moment-with-locales-es6
```

<br>



### ScrollView 대신 FlatList 코어 컴포넌트 사용하기

지금까지는 배열로 만든 아이템을 렌더링하기 위해서 ScrollView 컴포넌트를 사용하여 화면에 렌더링했다.

그런데 리액트 네이티브는 렌더링에 최적화된 FlatList 코어 컴포넌트를 제공한다. 똑같은 컴포넌트를 여러 개 렌더링할 때는 FlatList를 사용하는 것이 좀 더 속도가 빠르다.

<br>

- FlatList 사용법

  - ```react
    <FlatList data={people} />
    ```

그런데 FlatList의 사용법은 조금 복잡하다.

이는 위에서 보듯이 **data**라는 속성을 제공하는데, 출력하고 싶은 데이터를 data 속성에 설정하는 것이다.

또한 **renderItem**이란 속성을 제공한다.

<br>

타입스크립트 관점에서 타입 T의 배열 T[] 타입 데이터를 data 속성에 설정하려면 renderItem에는 ({item}: {item: T}) => {} , 즉 T 타입 데이터이며 item이란 이름의 속성이 있는  객체를 매개변수로 하는 콜백 함수를 설정한다.

그런데 renderItem이 반환하는 리액트 요소에는 key 속성을 설정하는 부분이 빠졌다. 

<span style="color:red">FlatList는 다음과 같이 keyExtractor 속성에 item과 index값이 매개 변수인 콜백 함수를 지정해 renderItem에 설정한 콜백함수가 반환하는 컴포넌트의 key 속성에 설정할 값을 얻는다.</span>

```react
<FlatList
    data={people}
    renderItem={({item} => <Person person={item} />)}
    keyExtractor={(item, index) => item.id} />
```

- 이 때 item.id 혹은 index.toString() (id와 같은 속성이 없는 데이터의 경우에)

<br>

#### FlatList가 제공하는 속성 종류 및 예시

- data
  - data={people}
- renderItem
  - {(item) => <Person person={item} /\>}
- keyExtractor
  - keyExtractor는 item.id를 반환하지만 id와 같은 속성이 없는 데이터라면 index.toString()을 반환해도 된다.
  - {(item, index) => item.id}
  - {(item, index) => index.toString()}
  
- ItemSeparatorComponent
  - 이 속성은 설정한 콜백 함수가 반환하는 컴포넌트를 아이템과 아이템 간의 구분자(Item Separator) 역할을 하는 컴포넌트로 삽입한다.
  - 이를 아래와 같이 설정해 주면 여러 아이템이 나열 될 때 각각을 구분해 주는 선이 생기는 것을 볼 수 있다.
  - {() => <View style={styles.itemSeparator} /\>}



<br>

### 객체를 리턴할때는 어떻게 하지?

객체 자체가 괄호에 쌓인 형태이기 때문에, 객체를 리턴하려면 기존 방식을 사용해야 하지만, ES6 화살표 함수는 이런 문제를 해결할 수 있는 기능을 이미 내장하고 있다.

```react
// 괄호 안에 이상한 문법이 들어가 버렸습니다...!
const newFunctionWrong = () => { a: '나는객체요소' };

// 소괄호로 감싸게 되면 객체 자체를 리턴할 수가 있게 됩니다.
const newFunction = () => ({ a: '나는객체요소' });
```

리턴할 객체 위에 소괄호를 감싸면 객체 자체를 리턴할 수 있는 형태가 된다.



<br>

### moment 패키지 기능 사용하기

자바스크립트와 타입스크립트에서는 날짜와 시간 관련 기능을 처리하는 Date 클래스를 제공한다. 그런데 왜 moment 패키지를 써야하는 걸까?

그 이유는 Date 클래스가 제공하지 않는 몇 가지 유용한 기능을 moment 패키지가 제공해 주기 때문이다.

moment 패키지를 활용하는 법은 다음과 같다.

```react
import moment from 'moment'

// person = createRandomPerson()
console.log('createDate', moment(person.createdDate).format('llll'))
console.log('modifiedDate', moment(person.modifiedDate).format('llll'))
// createDate Mon, Nov 23, 2020 2:25 AM
// createDate Mon, Nov 23, 2020 7:53 PM
```

<br>

또한 moment 패키지를 쓰는 가장 큰 이유라고 할 수 있는 것은 다음 코드처럼 과거와 현재의 시간 차이를 알기 쉬운 형태로 알려주기 때문이다.

```react
console.log(
	moment(person.createdDate).startOf('day').fromNow() //20 hours ago
)
```

<br>

#### moment-with-locales-es6 패키지 사용하기

앞에서 이 패키지를 설치할 때 이 패키지를 설치했었는데 이 패키지를 통해 '20 hours ago ' 라고 나오던 것을 '20시간 전'으로 나오게 할 수 있다.

즉, 한국에서 사용하는 형식으로 나오도록 할 수 있는 것이다.

그러기 위해서 다음과 같이 사용하면 된다

```react
import moment from 'moment-with-locales-es6'

...
moment.locale('ko')
```



<br>

### Person 컴포넌트 초기 스타일

컴포넌트를 개발할 때는 관련된 정보를 따로따로 모아 화면에 알기 쉬운 형태로 나타나게 하는 것이 중요하다. 화면에 이것저것 보이면 세부 스타일링이 쉽기 때문이다.

Person 컴포넌트를 스타일링하여 FlatList로 여러 개를 화면에 렌더링하면 다음과 같은 화면이 나오는데 각 컴포넌트 아래에 있는 숫자의 의미를 따로 부여하고 싶기 때문에 이를 아이콘을 붙여주고 TouchableView와 iconText등 재사용 할 수 있는 컴포넌트로 구현해 보자.

<br>

### 재사용할 수 있는 컴포넌트란?

- JSX로 구현한 댓글 갯수

  - ```react
    <View>
    	<Icon name='comment' size={24} color='blue'
            onPress={() => Alert.alert('comment clicked')} />
        <Text>{person.counts.comment}</Text>
    </View>
    ```

예를 들어 60이 댓글 개수라면 JSX로는 위와 같이 구현할 수 있는 것이다. 그런데 이 패턴은 리트윗이나 좋아요 등에도 똑같이 적용한다.

그러므로 앞 형태의 코드보다는 다음 IconText처럼 범용(general purpose)으로 사용할 수 있는 사용자 컴포넌트를 적용해야 더 코드 유지보수 차원에서 바람직하다.

- 사용자 컴포넌트로 구현

  - ```react
    <IconText viewStyle={styles.touchableIcon} onPress={onPress}
        name="comment" size={24} color='blue'
        textStyle={styles.iconText} text={person.counts.comment} />
    ```



댓글, 리트윗, 좋아요와 같은 아이콘들에 각각 이름을 붙여주고 싶다면 각각의 icon이 부착된 View 컴포넌트 안에서만 가능했지만 재사용할 수 있는 컴포넌트를 사용하면 이를 하나 만들어 놓고 각각의 Icon 컴포넌트를 감싸기만 하면 된다.

- 그렇기 때문에 유지보수가 뛰어난 코드가 만들어질 수 있는 것이다.

이처럼 `하나의 목적에만 부합하는 것이 아니라 어떤 패턴의 코드에 항상 적용할 수 있는 사용자 컴포넌트`를 **재사용할 수 있는 컴포넌트**라고 한다. 

그런데 타입스크립트로 재사용할 수 있는 컴포넌트를 만드려면 ReactNode라는 타입, children이란 속성, 그리고 수신한 속성을 한꺼번에 다른 컴포넌트에 전달하는 기법 등에 익숙해야 한다.

<br>

- 재사용할 수 있는 컴포넌트 만들기

  - ```react
    import type {FC, ReactNode} from 'react'
    
    type SomeComponent = {
        children?: ReactNode
    }
    
    export const SomeComponent: FC<SomeComponentProps> = ({children}) => {
        return <View>{children}</View>
    } 
    ```

- 재사용할 수 있는 컴포넌트 적용

  - ```react
    <View>
    	<SomeComponent><Image /></SomeComponent>
        <SomeComponent><Text>some text</Text></SomeComponent>
    </View>
    ```

SomeComponent를 이처럼 구현하면 자식 컴포넌트를 자유롭게 바꿔가며 자식 컴포넌트에는 없는 기능을 재사용할 수 있는 방식으로 제공할 수 있다.

TouchableView 컴포넌트에 이를 그럼 실제로 적용해 보자.

<br>

----

#### 타입스크립트 교집합 타입 구문

타입스크립트는 algebraic data type, ADT(대수 데이터 타입)을 지원한다. ADT는 A와 B를 타입이라고 할 때 'A타입 이거나 B타입' 혹은 'A타입이면서 B타입' 과 같은 타입을 만들고 싶을 때 적용하는 타입이다. 대수 데이터 타입은 |(합집합 타입) 기호와 &(교집합 타입) 기호를 사용하여 다음처럼 타입을 결합할 수 있다.



---

### TouchableView 컴포넌트 만들기

#### ComponentProps 타입

react 패키지는 ComponentProps 타입을 제공한다. `import type {componentProps} from 'react'`

<br>

ComponentProps 타입은 다음처럼 사용하는 제네릭 타입이다.

```react
속성_타입 = ComponentProps<typeof 컴포넌트_이름>
```

다음 코드는 TouchableOpacity 컴포넌트의 속성 타입을 ComponentProps로 알아낸 다음, 이를 TouchableOpacityProps 타입으로 만든다.

```react
type TouchableOpacityProps = ComponentProps<typeof TouchableOpacity>
```

<br>

#### JSX {...props} 구문

앞서 TouchableOpacity의 onPress 이벤트 속성에 다음처럼 onPress 속성을 설정한 코드를 본 적이 있다.

```react
<TouchableOpacity onPress={onPress}>
```

이 때 TouchableView는 다음처럼 구현할 수 있다.

```react
export type TouchableViewProps = {
    children?: ReactNode
    onPress?: () => void
}
    
export const TouchableView: FC<ToucahbleViewProps> = ({children, onPress}) => {
    return (
    	<TouchableOpacity onPress={onPress}>
        	<View>{children}</View>
        </TouchableOpacity>
    )
} 
```

그러나 좀 더 좋은 방식은 다음처럼 구현하는 것이다.

<br>

- 타입스크립트 교집합 타입과 JSX 전개 연산자 구문 혼합 사용

  - ```react
    import React from 'react';
    import type {FC, ReactNode, ComponentProps} from 'react';
    import {TouchableOpacity, View} from 'react-native';
    
    type TouchableOpacityProps = ComponentProps<typeof TouchableOpacity>;
    
    export type TouchableViewProps = TouchableOpacityProps & { // 7행
      children?: ReactNode;
    };
    
    export const ToucahbleView: FC<TouchableViewProps> = ({
      children, 
      ...touchableProps // 12행
    }) => {
      return (
        <TouchableOpacity {...touchableProps}>
          <View>{children}</View>
        </TouchableOpacity>
      );
    };
    
    ```

  - 이 코드의 의의 07행에서 타입스크립트의 교집합 타입을 사용하여 TouchableViewProps타입을 선언하고 12행에서 잔여 연산자 구문으로 TouchableOpacityProps 부분을 얻은 다음, 14행에서 JSX의 {...props} 구문으로 TouchableOpacity의 속성을 한꺼번에 넘겨준다는 데 있다.

<br>

---

#### 잔여 연산자?

ESNext 자바스크립트와 타입스크립트는 점을 연이어 3개 사용하는 잔여 연산자를 제공한다.

```react
let address: any ={
    country: 'Korea',
    city: 'Seoul',
    address1: 'Gangnam-gu',
	address2: 'Sinsa-dong 123-456',
    address3: '789 street, 2 Floor ABC building',
}
const {country, city, ...detail} = address
```

address 변수의 6개 속성 중 country와 city를 제외한 나머지 속성을 별도의 detail이라는 변수로 저장하고 싶다면 detail 변수 앞에 잔여 연산자(...)를 붙인다.



---



#### FC 타입과 children 속성

그런데 사실 함수 컴포넌트의 타입인 FC 타입은 ReactNode 타입인 children 속성을 포함한다.

---

**ReactNode이 뭐였지?**

클래스형 컴포넌트는 render메소드에서 ReactNode를 리턴한다. 함수형 컴포넌트는 ReactElement를 리턴한다. JSX는 바벨에 의해서 React.createElement(component, props, ...children) 함수로 트랜스파일된다.

ReactNode는 모든 것을 지칭한다고 볼 수 있다.
`string, number, boolean, null, undefined, ReactElement, ReactFragment, ReactPortal`

클래스형 render함수의 리턴타입, PropsWithChildren의 children 타입으로 사용된다.
`type PropsWithChildren<P> = P & { children?: ReactNode };`

포함 관계를 따져보면 다음 그림과 같다.

![image](https://user-images.githubusercontent.com/79521972/176857842-c7190821-096e-4dc5-a309-ac78b92344c8.png)

---

- FC 타입이 children 속성ㅇ르 제공하는 것에 착안한 구현

  - ```react
    import React from 'react'
    import type {FC, ReactNode, ComponentProps} from 'react'
    import {TouchableOpacity, View} from 'react-native'
    
    type TouchableOpacityProps = ComponentProps<typeof TouchableOpacity>
       
    export type TouchableViewProps = TouchableOpacityProps
    
    export const TouchableView: FC<TouchableViewProps> = ({
        children, ...touchableProps}) => {
        return (
        	<TouchableOpacity {...touchableProps}>
            	<View>{children}</View>
            </TouchableOpacity>
        )
    }
    ```

  - 이 코드에서 보듯 TouchableViewProps 속성은 단순히 5행의 타이 선언으로 충분하다.

  - 10행의 children 매개변수는 FC타입이 제공하는 속성을 얻은 것이다.

<br>

#### StyleProp타입

react-native 패키지는 StyleProp 타입을 제공한다.

그리고 모든 리액트 네이티브 컴포넌트는 다음처럼 `컴포넌트_이름Style` 형식의 타입을 제공한다.

```react
import type {ViewStyle, TextStyle, ImageStyle} from 'react-native'
```

StyleProp 타입을 사용하는 방식은 다음과 같다.

```react
viewStyle?: StyleProp<ViewStyle>
```

<br>

TouchableView.tsx 파일의 코드를 종합하면 다음과 같이 된다.

```react
import React from 'react'
import type {FC, ReactNode, ComponentProps} from 'react'
import {TouchableOpacity, View} from 'react-native'
import type {StyleProp, ViewStyle} from 'react-native'

type TouchableOpacityProps = ComponentProps<typeof TouchableOpacity>
   
export type TouchableViewProps = TouchableOpacityProps & {
    viewStyle?: StyleProp<ViewStyle>
}

export const TouchableView: FC<TouchableViewProps> = ({
    children, ...touchableProps}) => {
    return (
    	<TouchableOpacity {...touchableProps}>
        	<View style={[viewStyle]}>{children}</View>
        </TouchableOpacity>
    )
}
```

JSX 문의 TouchableOpacity와 View 컴포넌트에는 flex: 1과 같은 스타일링이 없다. 이는 {children}의 크기를 자신의 크기로 사용하겠다는 의사표시이다.

<br>

### Avatar 컴포넌트 만들기

이번엔 TouchableView를 사용하여 Avatar라는 재사용 컴포넌트를 구현해 보겠다.

Avatar 컴포넌트를 다음 JSX 코드를 염두에 두고 동작하도록 설계한 컴포넌트이다. uri와 size는 Avatar 고유 속성이고 viewStyle과 onPress는 Avatar를 구현하는 데 사용하는 TouchableView의 속성이다.

```
<Avatar uri={person.avatar} size: {50} viewStyle:{styles.avatar} onPress={onPress} />
```





### IconText 컴포넌트 만들기



#### Text코어 컴포넌트의 속성 탐구



### Person 컴포넌트 스타일 완료





