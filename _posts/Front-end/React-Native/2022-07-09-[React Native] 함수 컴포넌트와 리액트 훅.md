---
layout: single
title: "[React Native] 함수 컴포넌트와 리액트 훅"
categories: ['FrontEnd', 'ReactNative']
toc: true
toc_sticky: true
---



<br>

리액트 훅과 커스텀 훅

훅을 이용하면 클래스를 만들지 않고도 여러가지 리액트 기능을 사용 가능하다.



### 리액트 훅 사용 시 주의해야 할 점

1. 같은 리액트 훅을 여러 번 호출할 수 있다.
2. 함수 컴포넌트 몸통이 아닌, 몸통 안 복합 실행문(compound statement)의 { }에서는 호출할 수 없다.
3. 비동기 함수(async 키워드가 붙은 함수)는 콜백 함수로 사용할 수 없다.



<br>

## useMemo와 useCallback 훅



- 전역 변수
  - people 데이터는 한 번 만들어지면 바뀌지 않게 하고 싶었기 때문에 전역 변수로 설정했었음
- 리액트 훅의 등장 배경
  - 로컬 변수인 people을 마치 전역 변수처럼 사용하고 싶다는 데서 시작
  - 로컬 변수 people이 실제 데이터를 가지지 않고 실제 데이터는 어디인가 캐싱하고 나서 로컬 변수 people에는 필요할 대 이 캐시한 데이터를 찾을 수 있는 일종의 키(**reference**)를 저장하는 방법을 고안



### 의존성

리액트 프레임워크 내부에서 관리하는 캐시는 어떤 상황이 일어나면 갱신해야 할 때가 있는데 리액트 훅에서는 이를 의존성이라고 한다.

그런데 캐시한 값을 갱신하는 의존성은 여러 개일 수 있다.

리액트 훅에서는 이와 같은 여러 개의 의존성을 배열 형태로 모아 놓은 것을 **의존성 목록**이라 한다. 리액트 프레임워크는 의존성 목록 중 `어느 것 하나라도 조건이 충족되면` 다음 그림에서 보는 것처럼 캐시를 자동 갱신하고 해당 컴포넌트를 재렌더링하여 변경사항을 화면에 반영한다.



<br>

### useMemo 훅

- 컴포넌트를 처음 렌더링할 때 한 번만 데이터가 생성되도록 react 패키지는 useMemo 훅을 제공한다.
- useMemo 뿐 아니라 useEffect, useCallback 훅은 의존성 목록에 있는 의존성에 변화가 생길 때마다 콜백 함수를 자동으로 호출하여 의존성을 반영한다.
- 대부분 콜백 함수는 한 번만 호출하면 충분하기 때문에 '의존성이 전혀 없다'. 라는 의미로 []를 사용한다.
- useMemo 사용법

```
const 캐시된_데이터 = useMemo(콜백_함수, [의존성1, 의존성2, ...])
콜백_함수 = () => 원본_데이터
```

- useMemo의 함수 시그니처

```
useMemo<T>(() => T, [의존성1, 의존성2, ...]): T
```

- 생성한 데이터를 캐시에 저장

```
import React, {useMemo} from 'react'
import * as D from '../data'
export default function Memo(){
	const people = useMemo(() => D.makeArray(2).map(D.createRandomPerson), [])
	return <></>
}
```

위 코드는 Memo 컴포넌트를 처음 렌더링할 때 D.makeArray(2).map(D.createRandomPerson)으로 생성한 데이터를 리액트 프레임워크가 관리하는 캐시에 저장하여 코드의 효율성을 높인다.

useMemo(데이터)가 아니라 useMemo(() => 데이터)임에 주의한다.



<br>

### useCallback 훅

- 컴포넌트가 재렌더링될 때마다 iconPressed라는 콜백함수를 지속적으로 만드는 문제가 생길 수 있다.
- useMemo 훅이 데이터나 함수 호출의 결과값을 캐시하는 반면,
- useCallback 훅은 콜백 함수를 캐시한다.

- 고차 함수
  - 함수의 반환 값이 함수인 것



<br>

## useState 훅

- In compute science -> state: 시간이 지나도 값이 유지되며 필요에 따라서는 값을 바꿀 수 있는 어떤 것
- In programming -> state: 클래스의 멤버 속성이나 전역 변수 형태로  만들게 되며, 보통 함수 몸통의 지역 변수 형태로는 상태를 만들지 못한다.
  - 또한 어떤 값을 유지하는 변수를 의미하기도 함.
  - 저장된 값을 변경할 수도 있어야 함.
  - 그래서 상태는 오래전부터 클래스의 속성(property, member variable)으로 구현했다.
- 그런데 useState 훅은 함수 컴포넌트 내부에 클래스의 멤버 속성처럼 값을 유지하고 변경할 수도 있는 상태를 만들 수 있게 한다.

- useState 훅의 사용법

```
const [값, 값을_변경하는_함수] = useState(초깃값)
```

여기서 값을 변경하는 함수를 호출하면 '값'부분을 변경하고 동시에 컴포넌트를 재렌더링한다. 이 재렌더링 때문에 useState 훅은 값과 setter 함수를 튜플 형태의 배열로 반환한다.

- 참고로 이 코드는 타입스크립트의 '배열에 적용하는 비구조화 할당 구문'을 사용한다. 이 구문은 '객체에 적용하는 비구조화 할당 구문'과 달리 할당받는 변수 이름을 자유롭게 지을 수 있다는 장점이 있다.

- useState의 타입 정의

  - ```
    function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>]
    ```

  - 여기서 S는 상태 타입 State의 첫 글자임.

- Dispatch와 SetStateAction의 타입 정의

  - ```
    type Dispatch<A> = (value: A) => void
    type SetStateAction<S> = S | ((previousState: S) => S)
    ```

- useState에 사용한 다양한 변수 타입

  - ```
    const [yes, setYes] = useState<boolean>(false)
    const [age, setAge] = useState<number>(22)
    const [name, setName] = useState<string>('Jack')
    const [person, setPerson] = useState<D.IPerson>(D.createRandomPerson)
    const [people, setPeople] = useState<D.IPerson>([])
    ```

  - 

- **상태를 사용하는 컴포넌트 구현**

  - 리액트와 리액트 네이티브에서 속성은 불변 데이터임
    - read-only 변수
  - 변하는 매개변수는 다른 이름으로 변하도록 해야 코드 작성자와 열람자가 혼란스럽지 않음

- 값을 상태로 했을 때의 구현 방법

  - setComment, setRetweet, setHeart setter 함수는 컴포넌트의 매개변수 person의 멤버를 바꾸는 것이 아니라 지역 변수 comment, retweet, heart의 값을 바꾸기 위한 용도의 함수이다.

  - commentPressed 함수

    - ```
      const commentPressed = () => {setComment(comment + 1)}
      ```

    - useCallback을 이용하면

    - ```
      const commentPressed = useCallback(() => {setComment(comment + 1)})
      ```

    - 이 코드의 구현 방식에는 심각한 버그가 있는데 이는 comment 값이 단 한 번만 +1 증가하는 것이다.

    - 그 이유는 comment의 초깃값이 N으로 설정되었다고 할 때 처음 setComment(comment + 1)을 호출하면 comment는 N+1이 된다. 그런데 이 N+1로 바뀐 comment가 useCallback설정된 콜백 함수에 반영되려면 comment가 의존성 목록에 있어야 하지만 앞의 코드는 '의존성이 없다' 라는 의미의 []를 의존성 목록으로 사용했다.

    - 이 때문에 바뀐 comment의 값을 콜백 함수에 반영하지 않은 현상(버그)이 생긴 것인데 이를 없애기 위해선 comment를 useCallback의 의존성 목록에 추가하면 된다.

    - ```
      const commentPressed = useCallback(() => {setComment(comment + 1)}, [comment])
      ```

    - 근데 사실 더 좋은 방법은 앞서 본 SetStateAction\<S>의 (previousState:S) => S 형태로 setComment 함수를 호출하는 것이다. 다음 코드는 (previousState:S) => S 방식으로 setComment를 호출하여 앞선 버그를 해결한다.

    - 이 방식의 장점은 setComment 함수 내부에서 comment의 현재 값을 넘겨주기 때문에 useCallback의 의존성 목록에 comment를 추가할 필요가 없다는 것이다.

    - ```
      const commentPressed = useCallback(() => {setComment(comment => comment + 1)}, [])
      ```

- 객체를 상태로 했을 때의 구현 방법

  - 앞의 PersonUsingvalueState 컴포넌트에는 number 타입의 useState 훅 코드 3개를 작성했는데 useState 훅은 다음처럼 객체를 대상으로 사용할 수도 있다.

  - ```
    const PersonUsingObjectState: FC<PersonProps> =({person: initialPerson}) => {
    const [person, setPerson] = useState<D.IPerson>(initialPerson)
    ...
    }
    ```

  - comment, retweet, heart를 0으로 초기화

  - ```
    const [person, setPerson] = usestate<D.IPerson>({
    	...initialPerson, 
    	counts: {comment: 0, retweet: 0, heart: 0}
    })
    ```

  - 올바르게 구현한 코드

  - ```
    const commentPressed = useCallback(() => setPerson(person => \
    ({ ...person, counts: {...person.counts, comment: person.counts.comment + 1}})
    }), [])
    ```

  - 



- 자식 컴포넌트에서 부모 컴포넌트의 상태 변경하기
  - 코드 분량이 점점 늘어나다 보면 가독성이 떨어지고 관리 및 유지보수가 어렵기 때문에 컴포넌트를 작은 기능의 컴포넌트 여러 개로 분할하여 개발하는 것은 필수 작업이다.
  - PersonIcons 컴포넌트로 분할하여 단순하게 구현한다.





<br>

- 컴포넌트의 생명주기란?
  - 리액트와 리액트 네이티브에서는 컴포넌트를 생성하여 최초 렌더링 과정을 끝마치면 컴포넌트를 마운트 했다고 한다. 마운트한 컴포넌트는 이후 구현 로직에 따라 재렌더링을 거듭하다가 어떤 시점에서 구현 로직에 의해 파괴되어 화면에서 사라지는데, 컴포넌트가 파괴되어 더는 렌더링 과정에 참여하지 못하면 이를 언마운트 되었다고 한다. 리액트와 리액트 네이티브에서는 컴포넌트의 마운트 과정과 언마운트 과정을 합쳐 컴포넌트의 생명주기라고 한다.
