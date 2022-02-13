---
layout: single
title: "[Algorithm] CH5. Hash(해시)"
categories: ['Lecture', 'Data structure and algorithms']
tag: ['Data structure', 'Algorithm', 'Hash', '해시']
toc: true
toc_sticky: true
---

<br>

# 해시

이번 시간에는 해시에 대해 배워보도록 하겠습니다.

해시 자체는 어려운 개념이 아니나 이를 구현할 때는 노드와 LinkedList를 이용하여 구현하기 때문에 이전 과정이 복습이 잘 된 상태여야 이해가 더 잘 될 것입니다.

여러분이 컴퓨터와 전혀 상관없는 곳에서 아르바이트를 하고있다고 가정합시다.

<img src="https://user-images.githubusercontent.com/79521972/153736330-ecc6f12e-85a5-49fd-b52c-1eb88ece5cdd.png" alt="image" style="zoom:67%;" />

그런데 만약 여러분이 맡은 일이 첫 방문한 새로운 회원을 받으면 그 회원의 신상정보를 등록하는 것과 재방문 시 얼굴을 확인하여 방문 정보를 기록하는 작업입니다. 

회원 정보를 저장할 수 있는 수단은 12칸의 서랍이 있습니다.

![image](https://user-images.githubusercontent.com/79521972/153736402-335172be-e472-473e-8199-b0764fd31e05.png)

회원을 등록할 때에는 서랍을 하나하나 열어보면서 어느 칸이 비어있는 지 맨 앞의 칸부터 열어보면서 최대 12칸의 서랍을 열어봄으로써 확인을 해야합니다.

그리고 회원이 재방문 시 해당 회원의 정보가 어느 칸에 들어있는 지 확인하기 위해서 또 최대 12칸의 서랍을 열어봄으로써 확인을 해야겠죠. 굉장히 불편할 것 같습니다.

그래서 여러분들은 회원의 얼굴 정보에 따라 12가지 색 중 하나의 고유한 색깔을 찾아주는 프로그램을 만들었습니다.

1. 회원 등록 시 얼굴에 따라 색깔을 지정해줍니다.
2. 재방문 시 회원 정보를 확인 할 때는 다시 얼굴을 확인하여 해당하는 색깔 정보로 회원을 확인합니다.

이와 같이 서랍을 하나하나 열어보지 않더라도 회원을 확인할 수 있으며, 서랍이 12칸이 아니라 100칸, 1000칸 ... 과 같이 점점 더 커질 때마다 이 프로그램의 효용성은 더 커지겠죠?

<br>

그런데 이게 hash와 무슨 연관이 있냐고요? 

그렇다면 이제 앞서서 든 예를 우리가 배울 hash 자료구조와 엮어 설명해 보겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153736564-d56c4537-d938-4f9e-9327-5e6d03376b49.png)

우리가 저장하고자 하는 정보는 `value`입니다. 그리고 이 정보를 구분할 수 있는 데이터를 `key`라고 정의 합니다. key는 중복되지 않은 값을 사용해야 합니다.

앞서 설명했던 고유의 색깔을 찾아주는 프로그램은 `Hash function`이라하고 이에 따라 나오는 output인 12가지 색 중 하나를 뜻했던 것은 `해시값`이라고 합니다. 그리고 이 해시값을 인덱스화 해서 데이터를 저장하게 되는 것이죠.

![image](https://user-images.githubusercontent.com/79521972/153736676-60478c2d-278d-46b9-93d5-cb49522161b2.png)

그래서 `Hash table`이란 해시값을 인덱스화 해서 데이터를 저장하는 자료구조를 뜻하며 데이터가 저장되는 하나하나의 공간을 `Buckets`라고 합니다.

<br>

###  Hashing - 해싱

해싱은 데이터를 빠르게 저장하고 가져오는 기법 중 하나로 키에 특정 연산을 적용하여 테이블의 주소를 계산합니다. 이 때 특정 연산이 hash, table이 hash table을 말하는 것입니다.



<br>

### 해시 테이블

해시 테이블을 (Key, Value)쌍을 저장합니다. 그렇기 때문에 데이터 상에 순서가 존재하지 않는다는 특징이 있습니다.

다음 코드를 보면서 key, value 쌍을 간단히 이해해 보도록 하겠습니다.

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

Hash 자료구조를 사용하는 예시입니다. 

put()을 통해 key와 value를 쌍으로 저장을 하고 출력을 할 때에는 get()으로 key값을 통해 value값을 출력해 낼 수 있습니다.



### Hashing - Key

해시 테이블에서 value는 key를 기준으로 관리되기 때문에 key는 고유한 값을 가집니다.

`회원 정보를 받을 때 이름을 key값으로 하면 되지 않나?` 라는 의문을 가지실 수 있습니다. 하지만 이름을 key값으로 할 때에는 동명이인이라는 변수가 존재하기 때문에 시스템상의 혼돈을 일으킬 여지가 굉장히 많습니다. 그렇기 때문에 사람의 고유한 얼굴이나 주민등록번호로 key를 설정해야 하는 것입니다.



### Hashing - Hash Function

좋은 해시 함수(hash function)는 키 값을 고르게 분포 시켜야 합니다. 이는 뒤에서 더 자세히 설명하도록 하겠습니다. 또한 해시값을 계산해 주는 계산이 굉장히 빨라야 하고 충돌을 최소화 해야 합니다.

<br>

**충돌?**

 해시라는 자료구조에는 `해시 충돌(Hash Collision)`이라는 것이 존재합니다.

예를 들어, 만약 한 집에 사는 가족 구성원이 회원등록에 시도한다고 가정해 봅시다. 그렇다면 분명히 얼굴 혹은 주민 번호는 다를 것이기 때문에 key값을 각자가 다 다를것입니다. 하지만 주거지 같은 경우 같기 때문에 value는 같은 값을 가지게 되는 상황이 발생하는데 이를 해시 충돌이라고 합니다.

 이후에 나올 내용이지만 해시충돌은 자바에서 linkedlist를 이용한 'chaining' 방식을 통해 해결할 것입니다. 





## Hash 실습

그럼 Hash 자료구조에 대해서 Node를 이용하여 실습을 진행해 보도록 하겠습니다.

아래 코드를 보면 알 수 있듯이. MyLinkedHashTable은 key와 value를 받아 IHashTable을 구현상속하여 진행해 보도록 하겠습니다.

```java
package hashtable;

public class MyLinkedHashTable<K,V> implements IHashTable<K,V> {
    
    ...
}
```



### 노드

노드를 사용하여 구현하기 때문에 노드 클래스를 만들어 줍니다.

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

노드 클래스 안에는 노드 생성시 key와 value를 포함한 노드가 만들어지도록 생성자를 만들었습니다.

<br>

### 멤버변수

Hash를 구현하는데 필요한 멤버변수들을 선언하겠습니다.

```java
private List<Node>[] buckets;
private int size;
private int bucketSize;
```

buckets라는 배열은 노드가 담기는 리스트를 객체로 담는 배열입니다.

그리고 size와 bucketSize를 각각 선언해 줍니다.

<br>

### 생성자

생성자는 총 두가지로 인자(parameter)를 받지 않는 것과 인자로 bucketSize를 받는 것이 있습니다.

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

생성자에서는 위에서 선언한 멤버변수에 대한 초기화와 노드리스트 배열의 초기화가 이루어 지게 됩니다. 배열을 초기화 할 때에는 빈 LinkedList 자료구조를 생성하여 하나씩 대입하는 식입니다.

<br>

### hash function

hash table 구현에서 가장 중요한 것은 hash function이겠죠? hash function을 구현해 보도록 하겠습니다.

```java
private int hash(K key) {
    int hash = 0;
    for (Character ch : key.toString().toCharArray()) {
        hash += (int) ch;
    }
    return hash % this.bucketSize;
}
```

hash function에서는 나오게 되는 정수형의 index 값을 반환 할 것입니다.

key로 들어온 자료의 타입을 제네릭 타입으로 들어오기 전까지 모릅니다. 그렇기 때문에 key값을 string으로 변환하여 주고 다시 toCharArray()를 이용하여 charater 변수의 배열로 변환해 줍니다.

이렇게 되면 문자열을 받아서 이 문자열의 문자 하나하나를 for문이 돌게 되고 해당 문자를 int형의 아스키 코드로 변환하여 hash 변수에 더합니다.

그런데 이런 식으로 계속 hash 변수에 더해나가다 보면 bucketSize보다 더 커질 수도 있기 때문에 modulo연산을 통해 hash가 어떤 값이 나오더라도 bucketSize보다는 작은 값을 리턴할 수 있도록 하였습니다.

<br>

### put(K key, V value)

이제 이것들을 바탕으로 데이터를 넣는 작업을 진행해 보도록 하겠습니다.

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

받아온 key값을 hash fucntion을 통해 idx에 저장시킵니다.

그 후 해당하는 index의 배열을 가져옵니다. 

for문에서는 bucket이라는 배열에서 차례대로 자료(객체)를 하나씩 꺼내와 node에 넣으면서 자료 하나마다 안의 행동을 반복을 하겠다는 뜻입니다. 

안의 행동은 만약 key값이 중복된 경우 나중에 들어온 값으로 덮어씌워지는 것과 같은 처리를 해주어야 하기 때문에 이를 if문으로 node의 key값이 인자로 받아온 key값과 같으면 node의 data값을 인자로 받아온 value값으로 넣어주는 과정을 추가합니다.

만약 정상적으로 다른 key값이라면 받아온 key와 value로 이루어진 노드를 생성하고  bucket에 이 만들어진 노드를 하나 새로 추가하며 마치게 됩니다.

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

get() 함수도 똑같이 hash function으로 index를 받고 해당 인덱스의 bucket 배열 값(노드)을 가져와서 하나씩 순회하며 인자로 받아온 key값과 일치하는 bucket이 있으면 해당 노드의 data를 반환합니다.

만약 없다면 null을 반환합니다.

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

삭제 과정도 위와 마찬가지로 key를 정수 index값으로 저장하여 이에 해당하는 bucket객체를 가져오고 Iterator 객체를 통해 bucket을 데이터가 남아있지 않을 때까지 하나씩 순회합니다.

한 번씩 순회할 때마다 해당 bucket을 node에 가져와서 이 노드가 만약 인자로 받은 key값과 같다면 해당 객체를 iter.remove()를 통해 삭제해 줍니다.

그리고 삭제를 했기 때문에 hash의 크기를 1 감소시킵니다. 만약 삭제가 잘 이루어 졌다면 true를 리턴하고, 원하는 key를 발견하지 못했다면 false를 리턴합니다.

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

contains() 함수는 key값을 받아 해당 key값이 buckets 안에 존재하는 지의 여부를 확인하는 것이기 때문에 위와 마찬가지로 bucket을 하나씩 가져오면서 해당하는 bucket(노드)이 인자로 받아온 key값과 일치하면 true를 리턴하고 그렇지 않으면 false를 리턴 하도록 합니다.

<br>

### size()

다음은 size() 함수입니다.

size() 함수는 간단하게 멤버 변수 size를 반환하면서 끝내주도록 합니다.

```java
public int size() {
    return this.size;
}
```

<br>

## 해시 충돌

해시 충돌이란 위에서 잠깐 말했듯이 같은 거주지에 살지만 분명히 다른 사람인 회원들이 등록을 한다고 가정할 때 key(얼굴,주민번호)값은 다르지만 value(주거지)는 같은 값일 때 발생하는 경우를 말한다고 했습니다.

이러한 해시 충돌은 프로그램 상의 혼란을 야기할 수 있기 때문에 최소화 시켜야 합니다. 여기서 최고화라고 한 이유는 해시충돌은 줄일 수는 있지만 완전히 없애기는 거의 불가능에 가깝기 때문입니다.

<br>

Computer Science에서는 이 원리를 `비둘기 집 원리`라고 합니다.

#### 비둘기 집 원리(Pigeonhole Principle)

![image](https://user-images.githubusercontent.com/79521972/153741692-382d3ca8-a41f-49e6-a432-03e06aa2c4fb.png)

비둘기 집 원리는 N+1 개의 물건을 N개의 상자에 넣을 때 적어도 어느 한 상자에는 두 개이상의 물건이 들어있다는 원리를 말합니다.

쉽게 말해서 앞서 보았던 hash table은 12칸이 다였지만 등록하려는 회원 수가 15명, 20명 과 같이 hash table의 수보다 더 많으면 모든 회원의 정보를 저장하기 위해서는 어느 서랍 칸에서는 반드시 2명 이상의 정보를 저장해야 하는 것입니다.

hash table에서는 index의 수가 한정되어 있습니다. 하지만 우리가 사용가능한 데이터의 key의 숫자는 hash table의 index 수보다 많고 hash function의 특성 상 무한대의 key 입력도 가능합니다.

따라서 해시충돌이 발생할 수 밖에 없는 구조라는 설명입니다.

<br>

#### 생일 문제(Birthday Problem)

이를 설명하는 하나의 문제가 더 있습니다. 바로 Birthday Problem(생일 문제)입니다.

이는 임의의 사람 N명이 모였을 때 그 중 생일이 같은 두 명이 존재할 확률을 의미하는 것으로 생일의 가능한 가짓수는 366개이지만(2월 29일 포함) 여기서 의문이 생깁니다.

> "366명이 모여야 생일이 같은 경우의 수가 발생할까?"

당연히 아닙니다. 실제로는 23명만 모여도 50.7%의 확률로 같은 생일인 사람들이 존재하고 50명인 경우 97%라고 합니다.

왜 이 확률이 나오는 지는 이 수업에서 다루지 않겠습니다. 하지만 궁금하다면 링크를 걸어드릴테니 한 번 확인 해 보시면 되겠습니다.

[생일문제 - 위키백과](https://ko.wikipedia.org/wiki/%EC%83%9D%EC%9D%BC_%EB%AC%B8%EC%A0%9C)

<br>

그래서 결론은 key의 갯수가 hash table의 index수와 비슷한 수준까지는 아니더라도 충돌 가능한 확률은 생각보다 크다는 것을 말하고 싶은 것이고 이에 대한 솔루션이 반드시 존재해야 합니다.



### Chaining

그렇다면 이 해시 충돌을 막아야 할 솔루션에 대하여 배워보도록 하겠습니다.

그 방법 중 첫 번째로 `chaining`에 대해서 말씀드리겠습니다.

기존에 사용하던 구조에서는 해시 주소 하나에는 하나의 슬롯이 존재하여 이미 점유된 슬롯을 피해야 했습니다. 하지만 이는 점유된 슬롯을 피하지 않고, 그곳에 자료를 어떻게든 추가하는 방식으로 LinkedList를 이용하여 구현합니다.

간단히 말해서 메커니즘은 다음과 같습니다.

![image-20220213154727349](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220213154727349.png)

![image-20220213154748373](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220213154748373.png)

- 특정 key에 hash function으로 나온 hash 값을 index화해서  데이터를 저장할 것입니다. 그런데 다른 key를 넣었는데도 똑같은 hash 값이 나왔다면 똑같은 index를 가리키고 있겠죠? 그런데 이 때, 이미 저장하고 있는 데이터가 존재한다면 마치 LinkedList 구조와 같이 bucket에 새로운 노드를 연결해서 기존의 데이터가 추가, 저장되는 형태입니다. 이론 상 계속해서 똑같은 value가 들어오더라도 그 뒤에 Node가 계속 연결 되는 것입니다.

이 모습이 마치 chain이 연결된 것과 같다고 하여 chaining 이라고 합니다.

이를 사용하면 항목을 탐색하거나 저장, 삭제 할 때 계산한 해시 주소에 해당하는 연결 리스트에서 독립적으로 수행하기 때문에, 수행 시간면에서 효율적이고 LinkedList로 구현하기 때문에 메모리 소모도 없습니다.

하지만 hash function으로 hash index값을 찾는데 O(1)의 시간복잡도가 걸리지만 index의 위치에서 해당 값을 찾기 위해 리스트를 하나씩 탐색해야 하기 때문에 노드 연결이 길어질 수록 탐색 속도도 그 만큼 늘어나기 때문에 O(N)의 시간 복잡도를 가질 수 있고 이러한 이유로 최근 자바에서는 리스트 대신 트리라는 자료구조를 써서 시간복잡도를 줄일 수 있었습니다. 
(트리 구조는 이후 챕터에서 배우겠습니다.)

<br>

### Open Addressing

두 번째 방법으로는 `open addressing` 입니다. 

충돌이 발생할 경우에 다른 버킷에 데이터를 저장하는 방식입니다. 근데 이때 다른 bucket을 어떻게 찾느냐에 따라 방법이 또 나뉘게 됩니다.

1. **선형 탐색**

해시 충돌 시에 n칸을 건너뛴 다음 버킷에 저장하는 방식입니다. 이 n은 1칸 혹은 2칸, 3칸... 이 될 수 있습니다.

아래와 같은 배열을 생각해 봅시다.

![image](https://user-images.githubusercontent.com/79521972/153742736-65661d00-d0c8-44d4-8aa5-931459b5b6b0.png)

10번 index 부터 19번 index까지 있을 때 10번, 14번 ,18번 index에 데이터가 있다고 가정합시다.근데 이 때, 다른 key를 넣었는데 10번 index를 가리키는 hash값이 또 나오게 된다면 10번 index에는 이미 데이터가 있으므로 이를 건너 뛴 다음 bucket인 11번 index에 값이 들어가게 되는 것입니다. (n = 1인 경우)

이 선형 탐색을 사용했을 경우

- 계산이 단순하다.
- 검색 시간이 많이 소요된다.
  - 이는 다음 bucket에 데이터를 넣으려고 하는데 계속 해서 데이터가 들어있는 bucket만 나오게 된다면 빈 bucket을 찾기 위해 O(N)의 시간복잡도가 생기고 마는 것입니다.
- 데이터들이 특정 위치에만 밀집된다.
  - 앞서 말했듯 좋은 hash function은 key를 고르게 분포시켜야 합니다. 그 이유는 바로위에서 검색 시간이 많이 소요되는 것과 일맥상통하는 이유로 고르게 분포되어 있지 않으면 데이터가 들어있는 bucket이 뭉쳐지게(clustering) 되기 때문에 재탐색 횟수가 많아 지기 때문입니다.

와 같은 특성이 있습니다.

<br>

2. **제곱 탐색**

위의 선형 탐색의 단점인 clustering을 보완하기 위하여 제곱 탐색을 사용합니다.

제곱 탐색은 단순히 n번째 칸을 건너 뛰는 것이 아니라 N^2칸(1,4,9,16,...)을 건너뛴 버킷에 데이터를 저장하는 것입니다.

이를 사용하면 데이터가 들어있는 bucket이 뭉쳐지는 현상은 피할 수 있을 것입니다. 하지만 처음 hash function을 통해 나온 hash값이 동일하다면 마찬가지로 건너 뛴 칸도 똑같기 때문에 여전히 n번의 탐색을 하게 될 수있다는 문제가 여전히 존재하는 것입니다.

이를 해결 하기 위하여 나온 방법은 다음과 같습니다.

<br>

3. **이중 해시**

이중 해시는 해시 값에 다른 hash function을 한 번 더 적용하는 것입니다.

이 때 첫 번째 해시함수와 두 번째 해시함수로 hash 값을 구하게 됩니다.

- Hashfunction1(): 최초의 해시 값을 구함
- Hashfunction2(): 충돌 발생 시 이동 폭을 구함

이 두 함수를 사용하면 최초의 hash값이 같더라도 이동 폭이 다르기 때문에 clustering(뭉쳐짐) 문제를 해결 가능한 것입니다. 

따라서 이 방법은 bucket을 탐색하는 데 있어서 규칙성을 완전히 없애버린 방법으로 볼 수 있습니다.



## 관련된 문제

hash(해시) 와 관련된 문제로 백준 사이트의 1920번 수 찾기가 있습니다.

[백준 1920번 수 찾기](https://www.acmicpc.net/problem/1920)









