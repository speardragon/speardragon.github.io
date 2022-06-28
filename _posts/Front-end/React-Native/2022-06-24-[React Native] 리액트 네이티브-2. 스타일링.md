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

### react-native link 명령으로 폰트 자원 링크하기

이미지 파일과 달리 폰트 파일은 반드시 `npx react-native link`명령을 실행해야 사용할 수 있다.

그러나 이는 현재 일자 기준(22-06-28)으로 더 이상 지원되지 않는 서비스라 하여 다른 방식을 찾아야 했다.



<br>

## flex: 1의 의미

만약 View 컴포넌트에 있던 flex: 1 스타일 속성이 없애면 

- 'ImageBackground의 heigt-(paddingTop + paddingBottom) - Image의 height - Icon의 height 만큼의 높이 여분이 생긴다.



하지만 flex: 1로 설정되면 ImageBackground와 같은 부모 컴포넌트의 height 여분이 모두 flex: 1인 컴포넌트의 높이가 된다.



<br>

### flex: 1과 height: '100%'의 차이



![image](https://user-images.githubusercontent.com/79521972/176095861-2ace527c-7f2b-4add-9e03-0b142011a125.png)

이것이 height: '100%' 로 설정했을 때이고

<br>

![image](https://user-images.githubusercontent.com/79521972/176095815-30e15bd2-1a02-49e7-ad23-04fb7991058b.png)

이것이 flex: 1 로 설정했을 때의 모습이다.

<br>

flex: 1 스타일 속성을 부여하면 Content의 형제 요소인 TopBar와 BottomBar의 높이가 반영된 부모 컴포넌트의 높이를 가져온다.

이에 반해 'height: '100%'는 TopBar와 BottomBar의 높이와 무관하게 부모 컴포넌트의 높이를 모두 가져오므로 (즉, Content의 루트 View의 height에 Dimensions.get('window').height값을 설정한 것과 같은 효과) Content의 높이가 너무 커져 BottomBar가 화면에 나타나지 못하게 된 것이다.



<br>

- alignitems 스타일 속성

  - 부모 요소의 높이나 넓이에 여분이 있을 때 이 여분을 이용하여 자식 요소의 배치 간격을 조정하는데 사용

  - ```
    left, center, right, stretch
    ```

  - stretch 값은 부모 컴포넌트의 크기에 여분이 있으면 자식 컴포넌트의 크기를 늘림.

- jutisfyContent 스타일 속성

  - flexDirection에 따라 진행 방향이 달라짐

  - ```
    flex-start, center, flex-end, space-around, space-between, space-evenly
    ```

- flexWrap 스타일 속성

  - 폭보다 길이가 긴 요소들을 렌더링 할 때 줄을 바꿔가면서 렌더링 하도록하는

  - ```
    nowrap, wrap, wrap-reverse
    ```

- overflow 스타일 속성

  - 전체 콘텐츠의 크기가 컴포넌트 크기보다 클 때 이를 어떻게 할 지를 결정하는 속성

  - ```
    visible, hidden, scroll
    ```

  - 네이티브에서 스크롤은 ScrollView나 FlatList 코어 컴포넌트 위에서만 가능하다.



<br>

### ScrollView의 contentContainerStyle 속성

contentContainerStyle 속성은 스크롤 대상 콘텐츠 컴포넌트에 적용되는 속성이다. 이 속성을 설정할 때는 flex: 1 부분이 없어야 스크롤이 정상 작동한다.



<br>

## floating action button(FAB)

FAB 효과는 아이콘이 SafeAreaView의 자식 컴포넌트여서는 안된다. 

그래서 Fragment라는 컴포넌트를 사용해야 한다.

그런데 Fragment는 이름이 좀 길기 때문에 리액트에서는 <> </> 라는 단축 구문을 제공한다.





<br>

## 재사용 할 수 있는 컴포넌트 만들기

JSON.stringify 가 아닌 실제로 컴포넌트를 스타일링하는 방법을 알아보자



### FlatList

타입스크립트 관점에서 타입 T의 배열 T[] 타입 데이터를 data 속성에 설정하려면 renderItem에는 ({item}: {item: T}) => {} , 즉 T 타입 데이터이며 item이란 이름의 속성이 있는  객체를 매개변수로 하는 콜백 함수를 설정한다.

그런데 renderItem이 반환하는 리액트 요소에는 key 속성을 설정하는 부분이 빠졌다. FlatList는 다음과 같이 keyExtractor 속성에 item과 index값이 매개 변수인 콜백 함수를 지정해 renderItem에 설정한 콜백함수가 반환하는 컴포넌트의 key 속성에 설정할 값을 얻는다.

```react
<FlatList
    data={people}
    renderItem={({item} => <Person person={item} />)}
    keyExtractor={(item, index) => item.id} />
```

- 이 때 item.id 혹은 index.toString() (id와 같은 속성이 없는 데이터의 경우에)

<br>

#### FlatList가 제공하는 속성

- data
  - data={people}
- renderItem
  - {(item) => <Person person={item} /\>}
- keyExtractor
  - {(item, index) => item.id}
  - {(item, index) => index.toString()}
- ItemSeparatorComponent
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





### 재사용할 수 있는 컴포넌트란?

댓글, 리트윗, 좋아요와 같은 아이콘들에 각각 이름을 붙여주고 싶다면 각각의 icon이 부착된 View 컴포넌트 안에서만 가능했지만 재사용할 수 있는 컴포넌트를 사용하면 이를 하나 만들어 놓고 각각의 Icon 컴포넌트를 감싸기만 하면 된다.

- 그렇기 때문에 유지보수가 뛰어난 코드가 만들어질 수 있는 것이다.

하나의 목적에만 부합하는 것이 아니라 어떤 패턴의 코드에 항상 적용할 수 있는 사용자 컴포넌트를 재사용할 수 있는 컴포넌트라고 한다. 

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



<br>

## TouchableView 컴포넌트 만들기

- 타입스크립트 교집합 타입과 JSX 전개 연산자 구문 혼합 사용

  - ```react
    import React from 'react';
    import type {FC, ReactNode, ComponentProps} from 'react';
    import {TouchableOpacity, View} from 'react-native';
    
    type TouchableOpacityProps = ComponentProps<typeof TouchableOpacity>;
    
    export type TouchableViewProps = TouchableOpacityProps & {
      children?: ReactNode;
    };
    
    export const ToucahbleView: FC<TouchableViewProps> = ({
      children,
      ...touchableProps
    }) => {
      return (
        <TouchableOpacity {...touchableProps}>
          <View>{children}</View>
        </TouchableOpacity>
      );
    };
    
    ```

  - 이 코드의 특징은 07행에서 타입스크립트의 교집합 타입을 사용하여 TouchableViewProps타입을 선언하고 12행에서 잔여 연산자 구문으로 TouchableOpacityProps 부분을 얻은 다음, 14행에서 JSX의 {...props} 구문으로 TouchableOpacity의 속성을 한꺼번에 넘겨준다는 데 있다.

<br>

## 잔여 연산자?

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





