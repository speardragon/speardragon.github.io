---
layout: single
title: "[자료구조] 리스트(List) - ArrayList"
categories: ['Lecture', 'Data structures and algorithms with Java']
tag: ['Data structure', 'Algorithm', '리스트', 'ArrayList', '배열', 'LinkedList']
---

이번 시간에는 리스트(배열)에 대해서 알아 볼 것이다.

<br>

#### 리스트란?

리스트는 선형적인 자료구조 중의 하나로서 데이터를 일렬로 늘여 놓은 형태이다. 그렇기 때문에 리스트는 순서를 가진다는 특징이 있다.

모든 언어를 사용하든 간에 가장 기본적으로 배우게 되는 메모리 공간 형태이며 자료형(Data Type)이 둘 이상의 값을 저장할 수 있다.(Python은 예외)

이곳에서는 리스트를 어떻게 생성하고 어떻게 사용하는 지 보다는 리스트라는 자료구조에서 데이터를 어떻게 다루는 지를 더 알고 싶기 때문에 이 챕터에서는 리스트를 관리를 할 때 중요한 기능 3가지를 다루어 보겠다.

- 데이터 삽입하기
- 데이터 삭제하기
- 리스트 탐색하기

이 3가지는어떤 데이터 구조를 다룰 때도 항상 중요한 요소이기 때문에 앞으로 이에 대해 초점을 맞춰 진행을 해 보도록 하겠다.

<br>

**리스트의 종류**

- ArrayList
- LinkedList
  - Single Linked List
  - Double Linked List

리스트의 종류는 위와 같습니다. 먼저 ArrayList가 무엇인지 알아보도록 하겠다.



### ArrayList

단어 뜻 그대로 **배열 기반의 리스트**로 메모리 공간을 **연속적**으로 사용한다. 배열 기반의 구조이기 때문에 배열과 동일한 인덱스를 사용하고 인덱스를 이용한 데이터 접근은 배열의 크기가 아무리 크더라도 인덱스로 접근을 하게되면 값을 찾는데 항상 동일한 시간이 걸린다. -> O(1)

<img src="https://user-images.githubusercontent.com/79521972/152724760-07a4a709-c02e-4237-a2a3-f073b3e47fdb.png" alt="image" style="zoom:80%;" />

ArrayList는 컴퓨터의 실제 메모리 공간에서도 연속적인 공간를 사용하고 있기 때문에 컴퓨터가 연산하기 쉬운 구조로 되어있다. (다음 데이터에 접근하기 쉽기 때문) 

그렇기 때문에 이전 인덱스에서 다음 인덱스로 접근하는 속도가 매우 빠르다.

반면 LinkedList는 뒤에서 더 설명하겠지만 데이터를 논리적으로 연결시켜서 마치 연결된 것처럼 사용할 수는 있지만 실제 컴퓨터 메모리 공간 상에서는 연속적인 공간을 차지하고 있는 것이 아니기 때문에 데이터 간의 이동 시간 자체는 ArrayList보다 느릴 수 있다.

<br>



##### 추가

![image](https://user-images.githubusercontent.com/79521972/152729616-ac832bb7-5d7d-4c22-872d-89cfcf155548.png)



이 그림은 크기가 7인 ArrayList를 시각화 한 모습이다. 현재 0번부터 2번 index까지 각 데이터가 한 자리씩을 차지하고 있는 모습이다.

<br>



![image](https://user-images.githubusercontent.com/79521972/152729639-3efe5dcd-8504-4b67-a66a-4b307d876354.png)

데이터가 추가 될 때는 반드시 위 그림과 같이 가장 마지막으로 채워져 있던 2번 index의 바로 옆에 3번 index자리에 데이터가 추가된다.

따라서 리스트에서 데이터의 **추가**라는 것은 추가하고 싶은 데이터를 리스트의 가장 마지막으로 채워진 index 바로 뒤에 데이터를 삽입하는 것을 말한다.



##### 삽입

추가와 달리 데이터의 **삽입**은 데이터를 맨 뒤에 추가하는 개념이 아닌 내가 원하는 위치에 데이터를 삽입하는 것으로 데이터가 **이미** 들어가 있더라도 해당 index의 데이터와 그 뒤의 데이터들을 전부 뒤로 밀고 그 자리에 데이터가 삽입되는 형태를 말한다.

![image](https://user-images.githubusercontent.com/79521972/152729809-d9f2c3ac-0401-4ae2-91f5-6f5559f56ef6.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/152729740-94b1800e-3d57-45dd-acd6-6d000b6d8016.png)

그런데 이때 데이터를 한칸씩 밀어주는 과정에서 <span style="color:red">만약 데이터가 매우 많다면 많은 만큼의 시간이 소요</span>가 되므로 미는 과정은 O(N)의 시간 복잡도를 가진다고 말할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/152729754-d4c10e69-f184-429f-b140-d470a74d4a59.png)

**주의할 점**

삽입의 경우 추가하고자 하는 index에 데이터를 아무런 과정없이 그냥 넣게 되면 기존에 있던 데이터는 사라지게 되므로 **데이터 손실**이라는 매우 중요한 결함이 생기게 되므로 이에 유의하여 삽입을 사용해야 한다.

<br>

##### 삭제

index 기반의 데이터 삭제를 할 때에도 데이터를 이동시켜주는 작업이 반드시 필요하다.

![image](https://user-images.githubusercontent.com/79521972/152730056-a57271a0-d3d2-4a7f-b8d5-a85b800f56b3.png)

만약 2번 index를 삭제하고자 한다.<br>
<br>

![image](https://user-images.githubusercontent.com/79521972/152730118-a4f91044-38f7-488c-bd12-b40904d3a308.png)

그래서 삭제를 하고 그냥 아무런 과정을 진행하지 않으면 이 ArrayList는 중간이 뻥 뚫리게 되는 구조를 가지게 되므로 이 경우 오류가 발생할 것이다. 따라서 빈 공간을 뒤의 데이터를 당겨오면서 메꿔 주어야 한다.<br>

![image](https://user-images.githubusercontent.com/79521972/152730447-08e78c65-6f60-4aae-9bda-ae8dd7308aa4.png)

그 결과 그림처럼 빈 공간 없이 List에 데이터가 나열된 모습을 보실 수 있습니다.

<br>

##### 검색 by index

마지막으로 index에 의한 검색 과정이다. ArrayList는 **Random access**로 해당 index에 **직접** 접근하게 된다. 데이터가 아무리 많아져도 해당 index에 접근하는 것이기 때문에 이에 대한 탐색 속도는 매우 빠르고 이렇듯 데이터 갯수와는 상관없이 소요시간이 일정하다면 이런 경우에는 **O(1)**의 시간 복잡도를 갖는다.

![image](https://user-images.githubusercontent.com/79521972/152732341-48855b47-17e4-4eb8-a12b-ac910139d8a8.png)

<br>

### 시간 복잡도

다음은 ArrayList의 시간 복잡도로 데이터를 읽기만 할 때는 항상 똑같은 시간이 소모됨을 확인 할 수 있다.

반면 데이터를 추가하거나 삭제할 때는 굉장히 느린 것을 보실 수 있다.

다음에 배울 LinkedList와는 정반대이다.

| 연산      | 평균 시간 | 최악의 시간 |
| --------- | --------- | ----------- |
| read(get) | O(1)      | O(1)        |
| insert    | O(n)      | O(n)        |
| delete    | O(n)      | O(n)        |
| search    | O(n)      | O(n)        |





<br>

## ArrayList 실습

ArrayList를 구현하기 위한 list interface

```java
package list;

public interface IList<T> {
    
    void add(T t);
    
    void insert(int index, T t);
     
    void clear();
    
    boolean delete(T t);
    
    boolean deleteByIndex(int index);
    
    T get(int index);
    
    int indexOf(T t);
    
    boolean isEmpty();
    
    boolean contains(T t);
    
    int size();
}
```

IList라는 인터페이스를 선언하여 List의 기능을 확인할 각 메소드의 프로토타입들을 선언한 부분이다.

<br>



```java
package list;

publick class MyArrayList<T> implements IList<T> {
    
    ...
}
```

MyArrayList 파일은 위에서 선언한 인터페이스를 **구현 상속**한 클래스이다. 또한 리스트 안의 데이터 타입은 정해두지 않았기 때문에 제네릭 타입으로 설정하였다. 

앞으로 모든 자료구조 구현에서는(그래프 제외) 제네릭 타입으로 데이터를 받아 올 것이다.

- 자료 구조에는 어떤 타입이 들어가도 되기 때문이다.
- 그래프는 구현 방식이 어려워 정수 타입만 받도록 할 것이다.

위 코드를 타이핑하면 프로그램 기능으로 IList에 있는 구현되지 않은 메소드들의 바디가 자동완성으로 불러올 수 있다.

<br>

### 멤버변수

```java
private int size;
private T[] elements;
```

MyArrayList의 멤버변수로는 데이터를 저장할 수 있는 T타입의 배열 element와 구현의 편의성을 위해 배열의 size를 관리해야 하는데 이를 제어할 수있는 변수 size를 선언한다.

<br>

### 생성자

```java
private static final int DEFAULT_SIZE = 50;
public MyArrayList() {
    this.size = 0;
    this.elements = (T[]) new Object[DEFAULT_SIZE];
}
```

MyArrayList가 생성되는 시점의 생성자 코드를 생성하는 단계이다.

초기화 될 때는 아무런 값도 있지 않을 것이기 때문에 size는 0으로 초기화 해 주고 element에는 DEFAULT_SIZE, 즉 50 크기를 가지는 배열 객체를 생성하여 저장한다.

- 여기서 배열의 크기를 밖에서 DEFAULT_SIZE로 선언하고 안에서 이것으로 사용한 이유는 배열의 크기를 바꾸어야 할 때 하나하나 전부 바꾸어 주는 불편함을 해소하는 것과 같은 코드의 <span style="color:red">유지보수 측면에서 더 좋기 때문이다.</span>

<br>

### add

```java
@Override 
public void add(T t) {
   	if(this.size == this.elements.length) {
        this.elements = Arrays.copyOf(this.elements, this.size*2)
    }
    this.elements[this.size++] = t;
}
```

배열에 element를 추가할 때는 **인덱스**로 접근하여 그곳에 그대로 대입한다. 

위에서 배웠듯이 ArrayList의 추가는 배열의 현재 위치(빈 공간)에 추가를 하고 현재위치를 바로 그 다음 위치(빈 공간)로 넘기면 되기 때문에 인덱스를 this.size로 설정한다.

위치를 옮기는 과정은 this.size++로 배열의 인덱스를 1만큼 증가시키면 된다.

- 그래서 size의 위치는 항상 비어있는 위치 중 가장 첫 번째 위치이다.

단 배열이 꽉 차있는 경우 더 이상 데이터를 추가할 곳이 없다. 즉 배열의 인덱스로 지정한 size가 배열의 크기보다 커지기 때문에 오류가 발생할 수 있는데 이 경우를 if문으로 구별하여 해당 배열을 자바의 Arrays.copyOf 메소드로 원하는 크기로 복사를 한다.

- 위 코드에서는 size의 두 배 크기로 재 조정하여 다시 element에 전의 배열을 복사하였다.

<br>

### insert

```java
@Override 
public void insert(int index, T t); {
    for (int i = index; i < this.size; i++) {
    this.elements[i + 1] = this.elements[i];
	}
    this.elements[index] = t;
    this.size++
}

```

insert같은 경우는 **배열 인덱스의 정보**도 필요하고 해당 인덱스부터 현재 위치 전까지의 모든 데이터들을 **한 칸씩 미루어 넣는 과정**도 필요하다. 그리고 원하는 인덱스를 배열의 인덱스로 하여 그곳에 인자로 받아온 데이터 t 를 넣고 현재 위치를 1만큼 늘려 갱신한다.

<br>

### deleteByindex

```java
@Override
public boolean deleteByindex(int index){
	if(index < 0 || index > this.size - 1){
        return false
    }
    
    for (int i = index; i < this.size - 1; i++){
        this.elements[i] = this.elements[i+1];
    }
    this.size--;
    return true;
}
```

index에 의한 delete 과정을 진행할 때는 **해당 index부터** 현재위치를 의미하는 size의 바로 전전칸까지 데이터를 한 칸씩 앞당겨오는 과정을 진행해야 한다. 

- for 문에서 반복 범위가 i < this.size - 1 인 이유는 -1이 붙어있지 않다면 뒤의 데이터를 앞당겨 와야하는 과정에서 size는 빈 공간을 의미하는데 빈 공간의 데이터를 당겨올 수는 없기 때문이다.

삭제가 잘 이루어졌다면 size를 -1해주고 true를 리턴한다.

반면, index가 음수인 경우이거나 메소드 호출 시 지정한 index변수가` this.size - 1` 보다 크게 되는 경우 위에서 지적한 문제가 발생하기 때문에 if문으로 구별하여 false를 리턴한다. 

<br>

### delete

```java
@Override
public boolean delete(T t) {
    for (int i = 0; i < this.size; i++) {
        if(this.elements[i].equals(t)) {
            for (int j = i; j < this.size - 1; j++) {
                this.elements[j] = this.elements[j + 1];
            }
            this.size--;
            return true;
        }
    }
    return false;
}
```

element값을 받아 삭제하는 경우는 리스트 안의 모든 원소를 **순회**하면서 해당하는 원소를 찾아야 하기 때문에 먼저 for문으로 데이터를 하나씩 순회한다.

그리고 중간에 만약 인자로 받아온 **t**와 현재 돌고 있는 배열의 **element**의 값이 일치하면 현재 for문이 지정하고 있는 i가 삭제를 해야하는 index 이므로 그 값부터 시작하여 다시 for문을 통해 위 deleteByindex 에서 진행했던 밀어주기 과정을 진행한다.

<br>

### get

```java
@Override
public T get(int index){
    if(index < 0 || index > this.size - 1){
        throw new IndexOutOfBoundsException();
    }
    return this.elements[index];
}
```

get은 비교적 간단하다. 기본적으로 T 타입의 원소를 반환해 주는데 

그 값 인자로 받아온 index에 의해 접근되는 리스트의 value를 리턴한다.

index의 범위가 이 전의 deleteByindex에서 발생한 문제와 같은 것이 발생하지 않도록 예외처리 또한 해줍니다.

- T타입을 반환하는 메소드 이기 때문에 예외 상황에서 false를 리턴 하지 않고 위와 같이 예외 문구를 throw 할 수도 있다.

<br>

### contains

```java
@Override
public boolean contains(T t) {
    for (int i = 0; i < this.size ;i++) {
        if(this.elements[i].equals(t)) {
            return true;
        }
    }
    return false;
}
```

contains 메소드는 리스트를 순회하면서 인자로 받아온 데이터가 해당 리스트 안의 원소인지 아닌지를 판별하는 메소드이다.

 그 동안 했던 것과 마찬가지로 반복문을 통해 리스트 데이터를 하나씩 돌면서 equals 메소드로 리스트의 원소와 일치하면 true(1) 아니면 false(0)을 반환해 주는 logic을 구현한다.

<br>

### indexOf

```java
@Override
public boolean indexOf(T t) {
    for (int i = 0; i < this.size ;i++) {
        if(this.elements[i].equals(t)) {
            return i;
        }
    }
    return -1;
} 
```

indexOf 메소드는 위의 contains 메소드와 거의 동일한 과정으로 리스트를 돌면서 인자로 받아온 **데이터**가 반복문을 돌던 중 해당 element와 일치한다면 현재 for문의 i 즉, index를 반환한다.

- 만약 리스트 안의 원소가 아닐 경우, 즉 for문을 돌아도 반환되는 값이 없는 경우 -1을 반환하는데 그 이유는 만약, 0을 반환하게 되면 배열의 0번 index로 인식하여 원치않은 결과를 발생시킬 수 있기 때문이다.

<br>

### size

```java
@Override
public int size() {
    return this.size;
}
```

다른 기능 구현의 편의를 위해서, 선언했던 size 변수의 값은 현재 리스트의 크기를 알려주는 것이기 때문에 이를 리턴하는 size 메소드도 구현하였다.

<br>

### isEmpty

```java
@Override 
public boolean isEmpty() {
    return this.size == 0;
}
```

isEmpty 메소드는 리스트가 비어있는 지 아닌 지를 판별하는 메소드로 비어있다면 1(true) 그렇지 않다면 0(false)을 리턴하므로 'this.size == 0'  을 리턴하면 이 logic이 쉽게 해결된다.

<br>

### clear

```java
@Override
public void clear() {
    this.size = 0;
    this.elements = (T[]) new Object[DEFAULT_SIZE];
}
```

clear는 리스트 안의 원소를 모두 비우는 것인데 사실상 배열을 새로 만드는 생성자의 역할과 동일한 기능을 수행하는 것이므로 생성자에서 했던 것과 똑같은 과정이다.

<br>

### 자료구조 시간 복잡도 비교

- 평균 시간 복잡도(Average)

| 자료구조           | Access              | Search              | Insertion           | Deletion            |
| ------------------ | ------------------- | ------------------- | ------------------- | ------------------- |
| Array              | O(1)                | O(n)                | O(n)                | O(n)                |
| Stack              | O(n)                | O(n)                | O(1)                | O(1)                |
| Queue              | O(n)                | O(n)                | O(1)                | O(1)                |
| Singly Linked List | O(n)                | O(n)                | O(1)                | O(1)                |
| Doubly Linked List | O(n)                | O(n)                | O(1)                | O(1)                |
| Hash Table         | O(1)                | O(1)                | O(1)                | O(1)                |
| Binary Search Tree | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) |
| AVL Tree           | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) |
| B Tree             | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) |



- 최악의 경우 시간 복잡도(Worst)

| 자료구조           | Access              | Search              | Insertion           | Deletion            |
| ------------------ | ------------------- | ------------------- | ------------------- | ------------------- |
| Array              | O(1)                | O(n)                | O(n)                | O(n)                |
| Stack              | O(n)                | O(n)                | O(1)                | O(1)                |
| Queue              | O(n)                | O(n)                | O(1)                | O(1)                |
| Singly Linked List | O(n)                | O(n)                | O(1)                | O(1)                |
| Doubly Linked List | O(n)                | O(n)                | O(1)                | O(1)                |
| Hash Table         | O(n)                | O(n)                | O(n)                | O(n)                |
| Binary Search Tree | O(n)                | O(n)                | O(n)                | O(n)                |
| AVL Tree           | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) |
| Binary Tree        | O(n)                | O(n)                | O(n)                | O(n)                |
| B Tree             | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) | O(log<sub>2</sub>n) |







### 정리

이로써 MyArrayList라는 클래스에서 배열을 이용하여 리스트를 구현한 **ArrayList**의 수많은 기능을 모두 구현해 보았다. 자료구조 및 알고리즘은 메소드 사용 방법을 공부하는 과목이 아니다. 그렇기 때문에 이러한 그 안의 메소드들을 하나하나 직접 구현해 보면서 작동하는 원리를 습득해야 알고리즘을 구현하기 훨씬 쉬워질 것이다.

다음시간에는 리스트의 구현 방법 중 LinkedList에 대해서 알아보도록 할 것이다.

