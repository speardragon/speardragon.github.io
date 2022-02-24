---
layout: single
title: "[Algorithm] CH5. Hash(해시)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Hash']
tag: ['Data structure', 'Algorithm', 'Hash', '해시']
toc: true
toc_sticky: true
---

이번 시간에는 해시에 대해 배워 볼 것이다.

<br>

해시 자체는 어려운 개념이 아니나 이를 구현할 때는 노드와 LinkedList를 이용하여 구현하기 때문에 이전 과정이 복습이 잘 된 상태여야 이해가 더 잘 될 것입니다.

LinkedList를 참고하기 위해서는 아래 링크를 참조하시오.

- [<u>LinkedList(단순 연결 리스트)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/linkedlist/Algorithm-CH2-2.-LinkedList/)

## 해시란?

여러분이 컴퓨터와 전혀 상관없는 곳에서 아르바이트를 하고 있다고 가정하자.

<img src="https://user-images.githubusercontent.com/79521972/153736330-ecc6f12e-85a5-49fd-b52c-1eb88ece5cdd.png" alt="image" style="zoom:67%;" />

그런데 만약 여러분이 맡은 일이 첫 방문한 새로운 회원을 받으면 그 회원의 신상 정보를 등록하는 것과 재방문 시 얼굴을 확인하여 방문 정보를 기록하는 작업이라고 하자. 

회원 정보를 저장할 수 있는 수단은 아래와 같은 12칸의 서랍이 있다.

![image](https://user-images.githubusercontent.com/79521972/153736402-335172be-e472-473e-8199-b0764fd31e05.png)

회원을 등록할 때에는 서랍을 하나하나 열어보면서 어느 칸이 비어있는 지 맨 앞의 칸부터 열어보면서 최대 12칸의 서랍을 열어봄으로써 확인을 해야 한다.

그리고 회원이 재방문 시 해당 회원의 정보가 어느 칸에 들어있는 지 확인하기 위해 또 다시 최대 12 칸의 서랍을 열어봄으로써 확인을 해야 하므로 굉장히 불편하다.

그래서 여러분들은 회원의 얼굴 정보에 따라 12가지 색 중 하나의 고유한 색깔을 찾아주는 프로그램을 만들 것이다. 프로그램의 기능은 다음과 같다.

1. 회원 등록 시 얼굴에 따라 색깔을 지정해 준다.
2. 재방문 시 회원 정보를 확인 할 때는 다시 얼굴을 확인하여 해당하는 색깔 정보에 의해 회원을 확인한다.

이와 같이 서랍을 하나하나 열어보지 않더라도 회원을 확인할 수 있으며, 서랍이 12칸이 아니라 100칸, 1000칸 ... 과 같이 점점 더 커질 때마다 이 프로그램의 효용성은 더 커지지 않겠는가?

<br>

그런데 이게 hash와 무슨 연관이 있을지 궁금할 수 있다. 

그렇다면 이제 앞서 들었던 예를 우리가 배울 hash 자료 구조와 엮어 설명해 보겠다.

![image](https://user-images.githubusercontent.com/79521972/153736564-d56c4537-d938-4f9e-9327-5e6d03376b49.png)

우리가 저장하고자 하는 정보는 `value`이다. 그리고 이 정보를 구분할 수 있는 데이터를 `key`라고 정의 한다. key는 중복되지 않은 값을 사용해야 한다.

앞서 설명했던 고유의 색깔을 찾아주는 프로그램은 `Hash function`이라고 하며 이에 따라 나오는 output인 12가지 색 중 하나를 뜻했던 것은 `해시 값`이라고 한다. 그리고 이 해시 값을 인덱스화 해서 데이터를 저장하게 되는 것이다.

아래 해시 테이블 그림을 보자.

![image](https://user-images.githubusercontent.com/79521972/153736676-60478c2d-278d-46b9-93d5-cb49522161b2.png)

위 그림에서 `Hash table`은 <span style="color:red">해시 값을 인덱스화 해서 데이터를 저장하는 자료구조</span>(서랍)를 뜻하며 <span style="color:red">데이터가 저장되는 하나하나의 공간</span>을 `Buckets`라고 한다.



**해시는 다음과 같이 설명할 수 있다.**

- 임의의 길이를 갖는 데이터를 고정된 길이의 데이터로 변환(mapping)하는 것

- 또한 위와 같은 기능을 하는 함수를 해시 함수(hash function)이라고 한다.
- 해시는 해시 함수를 통해 작아진 값을 말하며 해시 값(hash value), 해시 코드(hash code) 라고도 한다.
- <mark>해시 테이블의 버킷을 가리키는 주소로 사용된다.</mark>(인덱스 대신)







<br>

###  Hashing - 해싱

해싱은 데이터를 빠르게 저장하고 가져오는 기법 중 하나로 키에 특정 연산을 적용하여 테이블의 주소를 계산하는데 이때, 특정 연산이 `hash`이고 table이 `hash table`을 말하는 것이다.

- Map이라는 자료구조에서는 Key값이 중복되어서는 안되는데 이를 위한 기술이라고 생각하면 편하다.

예를 들어, 'lion'이라는 문자열이 있다고 할 때 이 문자열을 특정 길이(e.g. 256bit, 512bit...)의 데이터로 변환시키는 과정이다.

![image](https://user-images.githubusercontent.com/79521972/155490566-19b3af63-0d1e-4b20-a4d1-96972b6bb747.png)

이러한 과정을 `해싱('hashing')`한다고 하며, 해시 함수를 통해 얻어진 값을 보통 `다이제스트(digest)`라고 한다.

그렇다면 왜 굳이 이렇게 복잡한 해시 함수를 거쳐야 하는 지에 대한 의문이 들 것이다.

<br>

우리가 이전에 배웠던 ArrayList, LinkedList, Stack, Queue 등의 자료구조들에서 '특정 값'을 찾기 위해 어떤 과정을 거쳤는지 생각해 보자.

배열 혹은 노드를 순회해 가면서 특정 값과 일치하는 것을 확인 했던 것이 기억 날 것이다. <span style="color:red">하지만 이 Hash Function을 이용한다면 이 과정을 굳이 거칠 필요가 없어진다.</span>

<br>

왜냐하면 같은 메세지(값)를 갖는 것이 보장되면 같은 다이제스트를 갖는 것 또한 보장 되기 때문이다. 

즉, 쉽게 말해서 위 이미지에서 특정 문자열을 hash 함수에 돌려서 얻은 값이 '#b!c1d&.....'인데 이는 고정된 값이다. 그렇기 때문에 동일한 해시 자료구조를 사용하는 경우 동일한 값에 대해서는 무조건적으로 동일한 다이제스트를 얻게 된다는 것이다. 

<br>

**해싱의 목적을 정리하면 다음과 같다.**

- 인덱싱
  - 테이블에서 올바른 위치를 바로 찾을 수 있게 한다.
- 암호화 및 복호화
  - 데이터를 알 수 없는 값으로 바꿔서 보호하고 인증된 사용자만이 불 수 있게 한다.
- 비교
  - 대량의 데이터가 있는 경우 전체를 비교하는 것보다 짧은 해싱 결과 값으로 비교하는 것이 더욱 효과적이다.

<br>

### 해시 테이블

**해시 테이블(Hash table)이란** <mark>검색하고자 하는 키값을 받아서 해시 함수(Hash function)를 통해 얻은 해시를 배열의 인덱스로 환산해서 데이터에 접근하는 자료구조이다.</mark>

![image](https://user-images.githubusercontent.com/79521972/155494333-1fb15e91-0cd2-458e-ad91-9bd3b6d37d1e.png)

해시 테이블을 (Key, Value)쌍을 저장합니다. 그렇기 때문에 데이터 상에 순서가 존재하지 않는다는 특징이 있다.

다음 코드를 보면서 key, value 쌍을 간단히 이해해 보도록 하자.

```java
import java.util.HashMap;
import java.util.Map;

public class Main() {
    
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("Hello", "World");
        map.put("ABC", "DEF");
        map.put("Computer", "Science");
        
        System.out.println(map.get("Hello"));
        System.out.println(map.get("DEF"));
        System.out.println(map.get("Science"));
    }
}
//World
//DEF
//Science
```

위 코드는 Hash 자료구조를 사용하는 예시이다. 

put()을 통해 key와 value를 쌍으로 저장을 하고 출력을 할 때에는 get()으로 key값을 통해 value값을 출력해 낼 수 있다.



### Hashing - Key

해시 테이블에서 value는 key를 기준으로 관리되기 때문에 key는 고유한 값을 가진다. 특징은 다음과 같다.

- 해시 함수에서 key는 자연수로 가정한다.(N = {1, 2, 3, ...})
- 만약 자연수가 아닌 charater 형이나 string 형이라면 자연수 형태로 변환하여 사용한다.

<br>

`회원 정보를 받을 때 이름을 key값으로 하면 되지 않나?` 라는 의문을 가지실 수 있을 것이다. 하지만 이름을 key값으로 할 때에는 **동명이인**이라는 변수가 존재하기 때문에 시스템 상의 혼돈을 일으킬 여지가 굉장히 많다. 그렇기 때문에 사람의 고유한 얼굴이나 주민등록번호를 key로 설정해야 하는 것이다.



### Hashing - Hash Function

해시 함수는 임의의 길이의 키값을 고정된 길이의 해시로 변환해 주는 mapping function이다.

어떤 해시 함수를 사용하느냐에 따라 해시 검색의 성능이 결정되기 때문에 좋은 해시함수를 사용해야 한다. 

<br>

**좋은 해시 함수란?**

좋은 해시 함수(hash function)는 키 값을 고르게 분포 시켜야 한다. 이는 뒤에서 더 자세히 설명하도록 할 것이다. 또한 해시 값을 계산해 주는 계산이 굉장히 빨라야 하고 충돌(collision)을 최소화 해야 한다. 

요약하면 다음과 같다.

1. 키를 고르게 분포 시켜야 함.
2. 충돌(collision) 발생 빈도가 적어야 함.
3. 빠른 연산



<br>

**충돌(collision)?**

 해시라는 자료구조에는 `해시 충돌(Hash Collision)`이라는 것이 존재한다.(뒤에서 나올 내용이다.)

해시 테이블 크기보다 키의 갯수가 더 많기 때문에 두 개의 키가 동일한 slot에 hashing 될 수 있는데 이를 해시 충돌이라고 한다.

예를 들어, 만약 한 집에 사는 가족 구성원이 회원등록에 시도한다고 가정해 보자. 그렇다면 이들의 얼굴 혹은 주민 번호는 분명히 다를 것이기 때문에 key값이 각자가 다 다를 것입니다. 하지만 주거지는 같기 때문에 value는 같은 값을 가지게 되는 상황이 발생하는 것과 같다.

 이후에 나올 내용이지만 해시 충돌은 자바에서 LinkedList를 이용한 'chaining' 방식을 통해 해결할 것이다. 



<br>

Hash function은 다양한 방법으로 구현할 수 있다.(여러 알고리즘이 존재)

이 방법에 대해서 알아보자.

<br>

##### 1)**나눗셈 방법(Division-remainder method)**

- 저장소의 크기를 전달 될 paramter(인자) key 값에 나누어서 인덱싱(indexing)하는 방식.
  - 나머지 연산(%) 사용
- 이 방식은 충돌일 일어날 가능성이 많아 충돌에 대비한 추가적이 대책이 요구된다.

- 검색키 k을 해시 테이블의 크기 m으로 나눈 나머지를 해시로 사용한다.

- m은 대개 소수(prime number)를 사용하며 특히 2의 제곱수와 거리가 먼 소수를 사용하는 것이 좋다.

- ```
  m : 7, k : 124
  
  hash(k) = 124 % 7 = 5
  ```

  - 해시 테이블의 크기(m)에 따라 충돌 빈도가 달라지기 때문에 효율적인 m의 값를 정해야 한다.

**효율적인 m선택 방법**

- m= 2<sup>p</sup>인 경우는 피한다.
  - m= 2<sup>p</sup>가 되면 해시함수 hash(k)은 키 k의 하위 p 비트가 된다.
  - 해시 함수가 key값 전체에 따라 바뀌지 않고 하위 p비트에만 영향을 받는다. 물론 하위 p비트가 고르게 나온다면 문제가 없지만 보통 그렇게 되지 않기 때문에 m= 2<sup>p</sup>인 경우는 피해야 한다.
  - ![image](https://user-images.githubusercontent.com/79521972/155515702-2a341f26-774b-413d-814b-6c2f51c01716.png)
- m = 2<sup>p</sup>에 너무 가깝지 않은 소수를 선택한다.
  - 예를 들어, 키의 수가 2000인 경우 테이블의 크기(m)는 701로 하는 것이 좋다.

- 해시 함수의 특성 때문에 해시 테이블의 크기가 정해진다는 단점이 있다.

<br>

##### 2) 접기 방법(Fording method)

접기(Fording) 방법은 검색 키를 분해하고 조합하여 해시를 만들어 내는 방법이다. 접기에는 이동 접기(Shift folding)와 경계 접기(Boundary folding)가 있다.
<br>

- **이동 접기**(Shift folding)
  1. 해시 테이블 크기의 자릿수 만큼 키를 분해한다.
  2. 분해된 키들을 모두 더한다.
  3. 더한 값이 테이블 크기를 초과하면 초과한 자릿수는 버린다.

![image](https://user-images.githubusercontent.com/79521972/155518071-b9dbd6e8-33f3-4bef-93f9-9ee497e4906b.png)

table size = 1000 (0 ~ 999) 3자리 - 키를 세 자리수로 분해함.

<br>

- **경계 접기(Boundary folding)**

1. 해시 테이블 크기의 자릿수로 키를 분해한다.
2. 나누어진 각 부분의 경계 부분 값을 역으로 정렬한다.
3. 분해된 키들을 모두 더한다.
4. 더한 값이 테이블 자릿수를 초과한다면 초과한 자릿수는 버린다.

![image](https://user-images.githubusercontent.com/79521972/155518295-4b3741a9-9166-4acb-9abc-9ad526ab31b3.png)
table size = 1000 (0 ~ 999) 3자리 - 키를 세 자리수로 분해함.

<span style="color:red">붉은색 숫자</span> 부분은 경계 부분으로 값을 역으로 정렬하여 더하였음.

<br>

##### 3) 중간 제곱 법(Mid-Square method)

키 값을 제곱한 뒤에 나온 결과 값에 대해서 중간 부분에 있는 몇 비트만 선택하여 해시로 사용하는 방법이다.

즉, 테이블 크기의 자릿수만큼 중간값을 가져오게 된다.

![image](https://user-images.githubusercontent.com/79521972/155520836-ddf0c43b-c097-4b04-9d2f-17879eb59c9f.png)

table size = 100 (0~99) 2자리 - 중간 2자리 수를 가져옴

<br>



##### 4) 숫자 분석 방법

키 중에서 편중(한쪽으로 치우친)되지 않은 수들에 대하여 해시 테이블의 크기에 적합하게 조합하여 사용하는 방법이다.

<br>

##### 5) Radix transformation 방법

예를 들어, 10진수 숫자의 key를 16진수 등으로 바꾸듯이 다른 sequence로 indexing하는 방식

<br>

##### 6)Digit rearrangement 방법

각 자리의 숫자를 상호 이동시키거나 거꾸로 돌리는 등의 작업을 해서 indexing하는 방식

<br>

이것들 외에도 다양한 좋은 hashing  algorithm이 존재하며, 이러한 내용은 구글링을 한다던지의 방법을 통해 찾아보는 것이 더욱 도움이 될 것이다. 각각의 내용은 이곳에서 더 깊이 다루지는 않겠다.

## Hash 구현

그럼 Hash 자료구조에 대해서 Node를 이용하여 실습을 진행해 보도록 하겠다.

아래 코드를 보면 알 수 있듯이, MyLinkedHashTable은 key와 value를 받아 IHashTable을 구현상속하여 진행해 보도록 할 것이다.

```java
package hashtable;

public class MyLinkedHashTable<K,V> implements IHashTable<K,V> {
    
    ...
}
```



### 노드

노드를 사용하여 구현하기 때문에 노드 클래스를 만든다.

```java
private class Node {
    K key;
    V value;
    
    Node(K key, V data) { 
    	this.key = key;
        this.data = data;
    }
}
```

노드 클래스 안에는 노드 생성시 key와 value를 포함한 노드가 만들어지도록 생성자를 만들었다.

<br>

### 멤버변수

Hash를 구현하는데 필요한 멤버변수들을 선언한다.

```java
private List<Node>[] buckets;
private int size;
private int bucketSize;
```

buckets라는 배열은 노드가 담기는 리스트를 객체로 담는 배열이다.

그리고 size와 bucketSize를 각각 선언한다.

<br>

### 생성자

생성자는 총 두가지로 인자(parameter)를 받지 않는 것과 인자로 bucketSize를 받는 것이 있다.

```java
private static final int DEFAULT_BUCKET_SIZE = 1024;//기본 bucket size

public MyLinkedHashTable() {
   	this.buckets = new List[DEFAULT_BUCKET_SIZE];
    this.bucketSize = DEFAULT_BUCKET_SIZE;
    this.size = 0;
    
    for( int i = 0; i < bucketSize; i++) {
        this.buckets[i] = new LinkedList<>();
    }
    
    
public MyLinkedHashTable(int bucketSize) {
   	this.buckets = new List[DEFAULT_BUCKET_SIZE];
    this.bucketSize = DEFAULT_BUCKET_SIZE;
    this.size = 0;
    
    for( int i = 0; i < bucketSize; i++) {
        this.buckets[i] = new LinkedList<>();
    }    
}
```

생성자에서는 위에서 선언한 멤버변수에 대한 초기화와 노드리스트 배열의 초기화가 이루어 지게 된다. 배열을 초기화 할 때에는 빈 LinkedList 자료구조를 생성하여 하나씩 대입하는 식이다.

<br>

### hash function

hash table 구현에서 가장 중요한 것은 hash function이므로 hash function을 구현해 보도록 하겠다.

```java
private int hash(K key) {
    int hash = 0;
    for (Character ch : key.toString().toCharArray()) {
        hash += (int) ch;
    }
    return hash % this.bucketSize;
}
```

return : hash function에서는 나오게 되는 정수형의 index 값을 반환 할 것이다.

1. key로 들어온 자료의 타입을 제네릭 타입으로 들어오기 전까지 모른다. 그렇기 때문에 key값을 string으로 변환하여 주고 다시 toCharArray()를 이용하여 charater 변수의 배열로 변환한다.

2. 이렇게 되면 문자열을 받아서 이 문자열의 문자 하나하나마다 for문이 돌게 되고 해당 문자를 int형의 아스키 코드로 변환하여 hash 변수에 계속해서 더한다.

3. 그런데 이런 식으로 계속 hash 변수에 더해나가다 보면 bucketSize보다 더 커질 수도 있기 때문에 modulo연산을 통해 hash가 어떤 값이 나오더라도 bucketSize보다는 작은 값을 리턴할 수 있도록 하였다.

보면 알겠지만 가장 보편적인 나눗셈 방법(Division method를 사용하였다.)

<br>

### put(K key, V value)

이제 이것들을 바탕으로 데이터를 넣는 작업을 진행해 보도록 하겠다.

```java
@Override
public void put(K key, V value) {
    int idx = this.hash(key);
    List<Node> bucket = this.buckets[idx];
    
    for (Node node : bucket) {
        if (node.key.equals(key)) {
            node.data = value;
            return;
        }
    }
    Node node = new Node(key, value);
    bucket.add(node);
    this.size++;
}
```

1. 받아온 key값을 hash fucntion을 통해 idx에 저장 시킨다.

2. 그 후 해당하는 index의 배열(노드 객체가 담긴)의 데이터를  가져온다. 

3. for문에서는 bucket이라는 노드 객체가 담긴 배열에서 차례대로 자료(객체)를 하나씩 꺼내와 node에 넣으면서 자료 하나마다 안의 행동을 반복을 하겠다는 뜻이다. 
   - 안의 행동은 만약 key값이 중복된 경우 나중에 들어온 값으로 덮어 씌워지는 것과 같은 처리를 해주어야 하기 때문에 이를 if문으로 node의 key값이 인자로 받아온 key값과 같으면 node의 data값을 인자로 받아온 value값으로 넣어주는 과정을 추가한다.

4. 만약 정상적으로 다른 key값이라면 받아온 key와 value로 이루어진 노드를 생성하고  bucket에 이 만들어진 노드를 하나 새로 추가하며 마치게 된다.

<br>

###  get(K key)

다음은 get() 함수입니다.

```java
@Override
public V get(K key) {
    int idx = this.hash(key);
    List<Node> bucket = this.buckets[idx];
    for (Node node : bucket) {
        if(node.keyequals(key)) {
            return node.data;
        }
    }
    return null;
}
```

1. get() 함수도 똑같이 hash function으로 hash code를 idx에 담고 해당 인덱스의 bucket 배열 값(노드)을 가져와서 하나씩 순회하며 인자로 받아온 key값과 일치하는 bucket이 있으면 해당 노드의 data를 반환한다.

2. 만약 없다면 null을 반환한다.

<br>

### delete(K key)

다음은 delete() 함수입니다.

```java
@Override 
public boolean delete(K key) {
    int idx = this.hash(key);
    List<Node> bucket = this.buckets[idx];
    for (Iterator<Node> iter = bucket.iterator(); iter.hasNext();) {
        Node node = iter.next();
        if(node.key.equals(key)) {
            iter.remove();
            this.size--;
            return true;
        }
    }
    return false;
}
```

1. 삭제 과정도 위와 마찬가지로 key를 hash code로 변환하여 정수 idx에 저장한다.
2.  이에 해당하는 bucket객체를 가져와 Iterator 객체를 통해 bucket을 데이터가 남아있지 않을 때까지 하나씩 순회한다.

3. 한 번씩 순회할 때마다 해당 bucket을 node에 가져와서 이 노드가 만약 인자로 받은 key값과 같다면 해당 객체를 iter.remove()를 통해 삭제해 준다.

4. 그리고 삭제를 했기 때문에 hash의 크기를 1 감소시킵니다. 만약 삭제가 잘 이루어 졌다면 true를 리턴하고, 원하는 key를 발견하지 못했다면 false를 리턴한다.

<br>

### contains(K key)

다음은 conains() 함수입니다.

```java
@Override
public boolean contains(K key) {
    int idx = this.hash(key);
    List<Node> bucket = this.buckets[idx];
    for (Node node : bucket) {
        if (node.key.equals(key)) {
            return true;
        }
    }
    return false;
}
```

contains() 함수는 key값을 받아 해당 key값이 buckets 안에 존재하는 지의 여부를 확인하는 것이기 때문에 위와 마찬가지로 bucket을 하나씩 가져오면서 해당하는 bucket(노드)이 인자로 받아온 key값과 일치하면 true를 리턴하고 그렇지 않으면 false를 리턴 하도록 한다.

<br>

### size()

다음은 size() 함수입니다.

size() 함수는 간단하게 멤버 변수 size를 반환하면서 끝내주도록 하자.

```java
public int size() {
    return this.size;
}
```

<br>

## 해시 충돌

지금 까지 해싱에 대해서 알아보았으니, 충돌에 대해서도 알아보도록 할 것이다.

<br>

먼저 해시 충돌에 대해서 이해하기 위해서는 먼저 적재율(Load Factor)을 이해해야 한다.

적재율이란 해시 테이블의 크기 대비, 키의 개수를 말한다.

- 적재율(α) = n/m (n: 키의 개수, m: 테이블의 크기)

해시 테이블은 테이블의 크기가 키의 개수보다 작기 때문에 적재율이 1을 초과한다. 그렇기 때문에 해시 충돌이 발생할 수밖에 없는것이다.

<br>

만약, 충돌이 발생하지 않는다면 테이블의 탐색, 삽입, 삭제 연산은 모두 O(1)의 시간 복잡도를 요구하지만 충돌이 발생할 경우 충돌에 대한 추가 작업이 필요하게 되어 더 복잡한 시간 복잡도를 요구하게 될 수도 있다.

충돌은 해시 테이블 연산 효율을 떨어뜨리기 때문에 충돌을 최소화하는 것이 해시 테이블의 핵심이다. 또한 위에서 배웠던 해시 함수를 통해 충돌을 줄일 수도 있다.

<br>

위에서 잠깐 말했듯이 같은 거주지에 살지만 분명히 다른 사람인 회원들이 등록을 한다고 가정할 때 key(얼굴,주민번호)값은 다르지만 value(주거지)는 같은 값일 때 발생하는 경우를 말한다고 했다.

- <u>**다른 말로 동일한 key값에 여러 value 값이 mapping된 현상**</u>이라고도 한다.

이러한 해시 충돌은 프로그램 상의 혼란을 야기할 수 있기 때문에 최소화 시켜야 한다. 여기서 최소화라고 한 이유는 해시 충돌을 줄일 수는 있지만 그 자체를 완전히 없애는 것은 거의 불가능에 가깝기 때문이다.

<br>

Computer Science(CS)에서는 이 원리를 `비둘기 집 원리`로 설명한다.
<br>

**비둘기 집 원리(Pigeonhole Principle)**

![image](https://user-images.githubusercontent.com/79521972/153741692-382d3ca8-a41f-49e6-a432-03e06aa2c4fb.png)

비둘기 집 원리는 N+1 개의 물건을 N개의 상자에 넣을 때 적어도 어느 한 상자에는 두 개이상의 물건이 들어있다는 원리를 말한다.

쉽게 말해서 앞서 보았던 hash table은 12칸이 다였지만 등록하려는 회원 수가 15명, 20명 과 같이 hash table의 수보다 더 많으면 모든 회원의 정보를 저장하기 위해서는 어느 서랍 칸에서는 반드시 2명 이상의 정보를 저장해야 하는 문제가 생긴다.

hash table에서는 index의 수가 한정되어 있다. 하지만 우리가 사용 가능한 데이터의 key의 숫자는 hash table의 index 수보다 많고 hash function의 특성 상 무한대의 key 입력도 가능하다.

따라서 해시충돌이 발생할 수 밖에 없는 구조를 설명하는 원리이다.

<br>

**생일 문제(Birthday Problem)**

이를 설명하는 하나의 문제가 더 있다. 바로 Birthday Problem(생일 문제)이다.

이는 <span style="color:red">임의의 사람 N명이 모였을 때 그 중 생일이 같은 두 명이 존재할 확률</span>을 의미하는 것으로 생일의 가능한 가짓수는 366개이지만(2월 29일 포함) 여기서 의문이 생길 수 있다.

> "366명이 모여야 생일이 같은 경우의 수가 발생할까?"

당연히 아니다. 실제로는 23명만 모여도 50.7%의 확률로 같은 생일인 사람들이 존재하고 50명인 경우 97%라고 한다.

왜 이 확률이 나오는지에 대해서는 이곳에서 다루지 않겠다. 하지만 궁금하다면 아래 링크를 걸어 둘테니 참고하면 된다. 

[생일문제 - 위키백과](https://ko.wikipedia.org/wiki/%EC%83%9D%EC%9D%BC_%EB%AC%B8%EC%A0%9C)

<br>

그래서 **결론**은 <mark>key의 갯수가 hash table의 index수와 비슷한 수준까지는 아니더라도 충돌 가능한 확률은 생각보다 크다</mark>라는 것을 말하고 싶은 것이고 이에 대한 **솔루션**이 반드시 존재해야 한다.

<br>

그렇다면 해시 충돌에 대한 솔루션은 어떤 것들이 있을까?

<br>

### (Separate)Chaining

그 방법(solution) 중 첫 번째로 `chaining`에 대해서 알아보자.

기존에 사용하던 구조에서는, 해시 주소 하나에 slot 하나가 mapping되어 이미 점유된 slot을 피해야 했다. 하지만 이는 점유된 슬롯을 피하지 않고, 그곳에 자료를 어떻게든 추가하는 방식으로 LinkedList를 이용하여 구현한다.

- 간단히 말해서 체이닝은 충돌이 일어났을 때 동일 slot에 연결리스트로 저장하는 방법이다.

메커니즘은 다음과 같다.

![image-20220213154727349](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220213154727349.png)

![image-20220213154748373](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220213154748373.png)

- 특정 key에 hash function으로 나온 hash 값을 index화해서 데이터를 저장할 것이다. 그런데 다른 key를 넣었는데도 똑같은 hash 값이 나왔다면 똑같은 index값을 가리키고 있을 것이다. 
  그런데 이때, 이미 저장하고 있는 데이터가 존재한다면 마치 LinkedList 구조와 같이 bucket에 새로운 노드를 **연결**해서 기존의 데이터가 추가, 저장되는 형태이다. 이론 상 계속해서 똑같은 value가 들어오더라도 그 뒤에 Node가 계속 연결 될 수 있다.

이 모습이 마치 chain이 연결된 것과 같다고 하여 `chaining` 이라고 한다.

이를 사용하면 항목을 탐색하거나 저장, 삭제 할 때 계산한 해시 주소에 해당하는 연결 리스트에서 독립적으로 수행되기 때문에, **수행 시간면에서 효율**적이고 LinkedList로 구현하기 때문에 **메모리 소모도 없다.**

<br>

하지만 hash function으로 hash code값을 찾는데 O(1)의 시간복잡도가 걸리지만 index의 위치에서 해당 값을 찾기 위해 리스트를 하나 씩 탐색해야 하기 때문에 노드 연결이 길어질 수록 탐색 속도도 그 만큼 늘어나기 때문에 O(N)의 시간 복잡도를 가질 수 있고 이러한 이유로 최근 자바에서는 리스트 대신 트리라는 자료구조를 써서 시간복잡도를 줄일 수 있다. 
(트리 구조는 이후 챕터에서 배울 것이다.)

<br>

자세한 내용에 대해서 살펴보겠다. 다음 그림을 보자.

![image](https://user-images.githubusercontent.com/79521972/155548857-946f7514-462e-497b-ba66-f4887dcb0fab.png)

각 해시 테이블 slot T[j]는 해시 값이 j인 모든 키를 연결 리스트로 저장하여 충돌을 해결한다.

- hash(k<sub>1</sub>) = hash(k<sub>4</sub>) , hash(k<sub>2</sub>) = hash(k<sub>5</sub>) = hash(k<sub>6</sub>)

<br>

chaining의 기본 연산은 다음과 같다.

- CHAINED-HASH-INSERT(T, x)
  - T[hash(x.key)] : 리스트에 x를 삽입한다.
- CHAINED-HASH-SEARCH(T, k)
  - T[hash(k)] : 리스트로부터 키 k를 찾는다.
- CHAINED-HASH-DELETE(T, x)
  - T[hash(x.key)] : 리스트에 x를 삭제한다.

<br>

다음 그림은 chaining의 동작 예시 입니다.

![image](https://user-images.githubusercontent.com/79521972/155549506-4dc9a9e7-a89b-4c04-828b-e605c8cbefdd.png)

Division method로 hash function이 연산되어 만약 같은 값을 갖게 되면 LinkedList로 계속해서 데이터가 연결되어 충돌을 방지할 수 있다.

<br>

**장점**

1. 구현이 굉장히 단순하다.
2. LinkedList로 저장하기 때문에 테이블이 가득 차지 않는다.
3. 키의 삽입이나 삭제 횟수와 빈도를 알 수 없을 때 주로 사용된다.
4. 해시 함수 및 적재율(load factor)에 영향을 덜 받습니다.

<br>

**단점**

1. 사용하지 않는 slot이 생겨 불필요한 메모리 소모가 발생한다.
2. 연결 리스트를 사용하여 키를 저장하기 때문에 cache(캐시) 성능은 떨어지게 된다.
3. 한 slot에서 계속해서 저장된다면 체인이 길어져 최악의 경우 검색 시간에 O(N)을 소모할 수 있다.
4. 링크 주소를 저장하기 위해서 추가 메모리 공간이 요구된다.

<br>

#### 시간 복잡도

- 평균 소모 시간
  - O(1+α) (α = 적재율)
  - 테이블 slot에 접근하기 위해서는 O(1)의 시간이 요구되지만 해당 slot에 있는 리스트를 검색하기 위해서는 O(α)의 시간이 요구되기 때문이다.
- 최악의 경우
  - O(N)
  - 모든 key가 하나의 slot으로만 계속해서 hashing되는 경우, 길이가 N인 연결 리스트가 생성되어 이를 탐색하게 될 경우 O(N)의 시간 복잡도가 요구된다.



### Open Addressing(개방 주소법)

두 번째 방법은 `open addressing` 이다. 

<br>

충돌이 발생할 경우에 다른 버킷에 데이터를 저장하는 방식이다. 다시말해서 충돌이 일어난 키 값을 비어 있는 다른 주소를 찾아 저장하는 방법이다.

모든 요소를 해시 테이블 자체에 저장하기 때문에 테이블의 크기는 키의 개수보다 반드시 크거나 같아야 한다.

<br>

**기본 연산은 다음과 같은 것들이 있다.** 

1) insert(key)

- 빈 슬롯을 찾을 때까지 계속 조사(probing)하고 빈 slot을 찾으면 키를 삽입한다.

<br>

2) search(key)
   - slot의 key가 찾고자 하는 key와 동일하거나 빈 slot에 도달할 때까지 계속 조사한다.

<br>

3. delete(key)
   - 삭제된 키의 슬롯에 삭제 표시를 한다.
   - 삽입은 삭제된 슬롯에 삽입할 수 있지만 검색은 삭제된 슬롯에서 멈추지 않는다.

<br>

근데 이때 다른 bucket을 어떻게 찾느냐에 따라 방법이 또 나뉘게 된다.

1. 선형 탐색(Linear Probing)
2. 제곱 탐색(Queadratic Probing)
3. 이중 해시(Double Hashing)

<br>

#### **1) 선형 탐색**

해시 충돌 시에 n칸을 건너뛴 다음 버킷에 저장하는 방식이다. 이 n은 1칸 혹은 2칸, 3칸... 이 될 수 있다.

아래와 같은 배열을 생각해 보자.

![image](https://user-images.githubusercontent.com/79521972/153742736-65661d00-d0c8-44d4-8aa5-931459b5b6b0.png)

10번 index 부터 19번 index까지 있을 때 10번, 14번 ,18번 index에 데이터가 있다고 가정하자. 

근데 이때, 다른 key를 넣었는데 10번 index를 가리키는 hash값이 또 나오게 된다면 10번 index에는 이미 데이터가 있으므로 이를 건너 뛰고 다음 bucket인 11번 index에 값이 들어가게 되는 개념이다. **(n = 1인 경우)**



<br>

그렇다면 각 연산에 대해 알아보자.

##### Insert

충돌이 발생하면 비어있거나(empty) 삭제된(deleted) slot을 발견할 때까지 index를 키워가며 탐색을 진행한다.

만약 알맞은 위치를 발견하면 그곳에 key값을 삽입한다.

![image](https://user-images.githubusercontent.com/79521972/155552735-d7a0dede-e33b-450b-aeda-8b1f14e873e8.png)

18의 key값에 입력을 할 때 18은 modulo연산(배열의 크기가 11인)에 의해 7이므로 앞에서 넣었던 7의 위치와 중복되어 이곳에 1이 더해진 8에 데이터가 들어간 모습이다. 

데이터의 추가는 이런 식으로 계속 진행된다. 

<br>

##### Delete

삭제하기 위한 slot을 찾고 slot의 상태를 deleted로 변경한다.

만약 deleted 표시를 하지 않고 삭제만 한다면 search를 했을 때 이 위치가 탐지가 되지 않는 경우가 발생할 수 있기 때문이다.

- 예를 들어, 아래 그림에서 8번 slot을 empty 상태로 둔고 deleted 표시를 하지 않는다면 62 키 값을 찾을 수 없게 된다.

![image](https://user-images.githubusercontent.com/79521972/155553464-d3a4439a-1aca-4bfb-80c1-d1ef18e38e14.png)

<br>

##### Search

검색은 키를 찾거나 빈 slot에 도달할 때까지 탐색을 계속한다.

중간에 삭제된 slot이 있더라도 탐색을 멈추지 않는다.

![image](https://user-images.githubusercontent.com/79521972/155553561-8e6e3df3-bda9-4e19-b85b-dd64319335b4.png)

<br>

**장점**

- 구현이 단순하다.

<br>

**단점**

- primary clustering 문제가 있다.

  - 연속된 데이터 그룹이 생기는 현상을 클러스터링(clustering)이라고 한다. 이는 탐색 시간을 오래 걸리게 하여 hashing의 효율을 떨어트리는 원인이 된다.
  - 탐색의 간격을 1 이외의 값으로 정할 수가 있는데 이때, 테이블의 크기 값과 서로소 관계에 있는 소수(prime number)로 정해야 클러스터링 현상을 막을 수 있다.

  

   

<br>

정리하면 선형 탐색은 다음과 같은 특징들을 갖는다.

- 계산이 단순하다.
- 검색 시간이 많이 소요된다.
  - 이는 다음 bucket에 데이터를 넣으려고 하는데 계속 해서 데이터가 들어있는 bucket만 나오게 된다면 빈 bucket을 찾기 위해 O(N)의 시간복잡도가 생기고 마는 것이다.
- 데이터들이 특정 위치에만 밀집된다.
  - 앞서 말했듯 좋은 hash function은 key를 고르게 분포시켜야 한다. 그 이유는 바로위에서 검색 시간이 많이 소요되는 것과 일맥상통하는 이유로 고르게 분포되어 있지 않으면 데이터가 들어있는 bucket이 뭉쳐지게(clustering) 되기 때문에 재탐색 횟수가 많아 지기 때문이다.

<br>



#### **2) 제곱 탐색**

위의 선형 탐색의 단점인 clustering을 보완하기 위하여 제곱 탐색을 사용한다.

제곱 탐색은 단순히 n번째 칸을 건너 뛰는 것이 아니라 N<sup>2</sup>칸(1,4,9,16,...)을 건너뛴 버킷에 데이터를 저장하는 것이다.

<br>

즉, 충돌이 발생하면 2차 함수 꼴로 증가하면서 빈 slot을 계속해서 찾는 방식이다.

- hash(k) → <span style="color:red">충돌</span> → hash(k) + **1<sup>2</sup>** → <span style="color:red">충돌</span> → hash(k) + **2<sup>2</sup>** → <span style="color:red">충돌</span> → ...

<br>

이를 사용하면 데이터가 들어있는 bucket이 뭉쳐지는 현상은 피할 수 있을 것이다. 

하지만 처음 hash function을 통해 나온 hash code값이 동일하다면 마찬가지로 건너 뛴 칸도 똑같기 때문에 여전히 n번의 탐색을 하게 될 수 있다는 문제가 여전히 존재하는 것이다.

<br>

##### Insert

![image](https://user-images.githubusercontent.com/79521972/155554882-22fe769f-1e19-4fd7-879e-27db806c0eb0.png)

delete연산과 search 연산은 선형 탐색과 유사하기 때문에 생략하였다.

<br>

**장점**

- 선형 탐색보다는 clustering이 적게 일어난다.

<br>

**단점**

- Secondary clustering 문제가 있다.
  -  만약 두 key의 처음 probe 값이 동일하다면 빈 slot을 찾는 과정이 동일하기 때문에 같은 slot을 탐색한다.
  - 즉, 처음 충돌한 위치가 같다면 다음 충돌 위치에서도 반복적으로 계속 충돌이 일어나게 된다.

- ![image](https://user-images.githubusercontent.com/79521972/155555482-a27089e2-f8ce-4ec6-acd1-3e996bfd70c0.png)

<br>

이를 해결 하기 위하여 나온 방법은 다음과 같다.

#### **3) 이중 해시**

이중 해시는 말 그대로 해시 값에 다른 hash function을 한 번 더 적용하는 것이다.

이 때 첫 번째 해시함수와 두 번째 해시함수로 hash 값을 구하게 된다.

- Hashfunction1(): 최초의 해시 값을 구함
- Hashfunction2(): 충돌 발생 시 탐사(probe) 이동 폭을 구함

이 두 함수를 사용하면 최초의 hash값이 같더라도 이동 폭이 다르기 때문에 clustering(뭉쳐짐) 문제를 해결 가능한 것이다. 

따라서 이 방법은 bucket을 탐색하는 데 있어서 <span style="color:red">규칙성을 완전히 없애버린 방법</span>으로 볼 수 있다.

<br>

hash<sub>1</sub>(k) → <span style="color:red">충돌</span> →  hash<sub>1</sub>(k) + **1** \* hash<sub>2</sub>(k) → <span style="color:red">충돌</span> → hash<sub>1</sub>(k) + **2** \* hash<sub>2</sub>(k) → <span style="color:red">충돌</span> →hash<sub>1</sub>(k) + **3** \* hash<sub>2</sub>(k) → ...

<br>

**탐색 과정**

- 처음 탐색하는 위치는 T[hash<sub>1</sub>(k)]이다.
- 그 다음부터는 hash<sub>2</sub>(k) 만큼 이동하면서 탐색한다.
  - 즉, 충돌이 발생했을 때, <span style="color:blue">이동하는 거리가 hash function에 의해 계산되고 무작위로 빈 slot을 찾게 되어 clustering을 피하는 개념이다.</span>



##### Insert

![image](https://user-images.githubusercontent.com/79521972/155556604-9ceddeae-97f4-4384-aa02-6656550942fd.png)

이중 해시 또한 delete와 search 연산은 선형 탐색과 비슷하기 때문에 생략하였다.



<span style="color:red">**주의점** </span>

- hash<sub>2</sub>(k) 함수는 해시 테이블의 크기 m과 서로소 관계여야 한다.
  - m을 2의 지수승으로 하고 h<sub>2</sub>가 항상 홀수가 되도록 한다.
  - m을 소수로 하고 h<sub>2</sub>를 m보다 작은 양수로 정한다.



**장점**

- 클러스터링이 다른 두 방식보다 적게 발생한다.

<br>

**단점**

- 연산이 다른 두 방식(선형 탐색, 제곱 탐색)에 비해 오래 걸린다.

<br>

####  시간 복잡도

Open addressing은 배열의 크기가 한정되어 있기 때문에 적재율(Load Factor)이 1 이하이다. 

또한 계산 복잡성은 탐사 횟수에 비례하게 되는데 그렇기 때문에 search 횟수를 통해 시간 복잡도를 표현할 수 있다.

적재율을  α라고 했을 때 선형 탐색에서 시간 복잡도는 다음과 같다.

- successful search : 1/2∗1 + 1/(1−α)
- unsuccessful search : 1/2∗1 + 1/(1−α)<sup>2</sup>

<br>

선형 탐색에서 적재율이 0.5 이상이 되면 unseccessful search 횟수는 급격하게 늘어나게 된다.

따라서 적재율이 0.5 이상이 되면 해시 테이블의 크기를 늘리는 등의 작업을 통해 적재율을 0.5 미만이 되도록 해야 한다.

물론 제곱 탐색, 이중 해시도 적재율을 0.5 미만으로 유지하는 것이 좋습니다.





## 관련된 문제

hash(해시) 와 관련된 문제로 백준 사이트의 1920번 수 찾기가 있다.

[백준 1920번 수 찾기](https://www.acmicpc.net/problem/1920)



<br>

## References

- [https://yoongrammer.tistory.com/82?category=956616](https://yoongrammer.tistory.com/82?category=956616)

