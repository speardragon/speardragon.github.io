---
layout: single
title: "[React Native] 리액트 네이티브-2. 스타일링"
categories: ['Front-end', 'React-Native']
toc: true
toc_sticky: true
---

<br>

## 3.1 Style 속성과 StyleSheet API 이해하기

style 속성에 style 객체를 설정한다. (Not string)

```react
<SafeAreaView style={{}}>

</SafeAreaView>
```

<br>

- 스타일 객체가 가지는 속성을 스타일 속성이라고 한다.

- 스타일 속성에는 요가 엔진이 지정한 이름만 사용할 수 있으며 소문자로 시작한다.
- 스타일 속성 설정 값이 배열이면 배열 안의 스타일 객체를 모두 결합하여 하나의 스타일 객체로 만들어준다.



<br>

### Sytle Sheet API

- StyleSheet.create의 목적이 자바스크립트 언어로 만든 스타일 객체를 네이티브 모듈 쪽으로 옮겨 주는 것이므로 여러 번 호출하여 스타일 객체 여러 개를 일일이 전달하는 것보다는 한꺼번에 전달하는 것이 효율적이다. 

- StyleSheet.create 함수는 매개변수에 설정된 스타일 객체를 네이티브 모듈 쪽에 전달한다. 

<br>

### 구글 material design 색상과 react-native-paper 패키지

CSS에서 색상은 16진수 RGB(#fff, #21963f)로 표현 되어 있는데 이를 white or Colors.blue 와 같이 하도록 도와주는 패키지이다.



#### color 패키지

구글 머티리얼 디자인은 앱의 색상을 주 색상과 보조 색상으로 나눠서 애브이 테마 색상을 결정할 것을 권고한다.

앱의 테마 색상이 결정되면 글자가 모든 색상에서 잘 보일 수 있도록 글자 색상도 잘 결정해야 한다.

color 패키지는 위 경우에 어떤 색보다 좀 더 밝게 또는 어둡게 하거나 투명도를 가감하여 좋은 색상을 정하는데 유용합니다.

```react
text: {fontSize: 20, color: Color(Colors.blue500).lighten(0.9).string()}
```

다음과 같이 Color로 감싼 색상에 대해서 가장 잘 보이는 색으로 대치해 주는 것이다.



<br>

## View 컴포넌트와 CSS 박스 모델

리액트 네이티브는 각종 'View' 가 들어간 컴포넌트를 제공한다. 이는 화면 UI와 레이아웃과 스타일리을 담당한다.



- **Platform API**
  - 현재 앱이 실행되는 폰이 무엇인지를 알기 위한 모듈

- Dimensions API
  - 현재 실행된 폰의 크기를 알아야 할 때 사용하는 API
- alpha: 투명도
- lighten: 밝기

- 궁금증: import 문에서 {}의 의미는? -> 할 때도 있고 안 할 때도 있고
  - 차이점은 보내주는 export 방식의 차이이다.
  - 모듈을 불러올 때 import라고 쓰는 것처럼 모듈을 다른 파일에 보내려면 export라고 명시해야 한다.
  - 이 때 다른 모듈로부터 가져오는데 해당 모듈에서 이를 export할 때 default(해당 모듈 자체를 import하면 내보내지는 것)로 내보낸 것은 괄호가 필요없지만 나머지의 경우 필요하다.



<br>

### View의 기본 width값과 height값

- 'View' 컴포넌트에 width와 height 스타일 속성값을 직접 명시하지 않았을 때 리액트 네이티브는 'View'의 width는 부모 컴포넌트의 width를 그대로 설정하고 height는 자식 요소의 height를 'View'의 height로 설정한다.
  - 자식 요소를 수평으로 배치한 경우 - 자식 요소 중 가장 높은 요소의 height 값
  - 자식 요소를 수직으로 배치한 경우 - 자식 요소의 height 값을 모두 더한 값
- 가장 바깥의 'View' 컴포넌트(코어 컴포넌트)의 부모는 App(사용자 컴포넌트)임.
  - 사용자 컴포넌트는 렌더링에 참여하지 않고 리액트 네이티브 코어 컴포넌트만 화면에 직접 렌더링 함.
- CSS에서 퍼센트값은 항상 부모 컴포넌트의 크기를 기준으로 봤을 때의 비율이다.

<br>

### flex 스타일 속성

- 앞서 width, height 스타일 속성(100%, 50% 등으로 설정하던 것)을 flex 스타일 속성에 적용하면 1은 100%를 0.5는 50%를 의미하도록 할 수 있다.

- 간혹 컴포넌트 스타일을 할 때 flex와 (width, height)를 동시에 사용하는 경우가 있다.

  - 이 경우 flex가 더 낮은 우선순위를 갖음.

  - ```react
    <View style={[flex: 1, height: 50]} />
    ```

  - 위 코드에서는 따라서 컴포넌트의 height는 부모 요소의 height값(flex: 1)이 아니라 height 스타일 속성값 50이 설정된다.



<br>

### margin

- marginLeft와 marginRight 값이 같은 상황(즉, marginLeft=marginRight일 때)이면 marginHorizontal 스타일 속성으로 이 둘을 동시에 설정할 수 있으며
- marginTop=marginBottom 상황이면 marginVertical을 사용하여 두 값을 한꺼번에 설정할 수 있다.
- marginHorizontal=marginVertical 상황이면 margin 속성을 사용하여 이 두 값을 한꺼번에 설정할 수 있다.



<br>

### padding

- padding 스타일 속성은 부모/자식 간의 관계에서 부모 컴포넌트 쪽에 적용하는 스타일 속성이다. 
- 대부분 부모 컴포넌트 내부에 자식 컴포넌트를 배치할 때 자식 컴포넌트가 자신의 영역을 꽉 채우지 않고 간격을 주는 것이 시각적으로 좋다.

- 안드로이드에서 SafeAreaView는 단순히 View로 동작하지만 iOS에서 SafeAreaView는 View가 아니기 때문이다.

<br>

### border 관련 속성

- 리액트 네이티브 코어 컴포넌트는 대부분 자신 영역의 **경계**를 설정할 수 있는 다음과 같은 스타일 속성을 사용할 수 있다.
  - borderWidth: border 넓이
    - borderLeftWidth, borderRightWidth, borderTopWidth, borderBottomWidth
  - borderColor: border 색상
    - borderLeftColor, borderRightColor, borderTopColor, borderBottomColor
  - borderRadius: border 모서리 둥근 정도를 의미
    - borderTopLeftRadius, borderTopRightRadius, borderBottomLeftRadius, borderBottomRightRadius
  - borderStyle: 실선, 점선 등 border 스타일을 의미
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

























