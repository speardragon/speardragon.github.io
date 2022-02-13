---
layout: single
title: "[Algorithm] CH2-3. Double LinkedList"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'LinkedList']
tag: ['Data structure', 'Algorithm', '이중연결리스트', 'DoubleLinkedList']
toc: true
toc_sticky: true
---

<br>

# Double LinkedList

이번 시간에는 LinkedList의 종류 중에서 Double LinkedList에 대해서 배워보겠습니다.

이전 내용에 있는 LinkedList는 앞에 'Single'이 생략 되어있는 셈이라 생각하면 됩니다. 그럼 무엇이 single이고 double일까요?

![image](https://user-images.githubusercontent.com/79521972/153207800-fa48ed48-48d6-45da-97cd-606ccc15a8a7.png)

위 그림은 Double LinkedList의 대략적인 구조입니다.

![image](https://user-images.githubusercontent.com/79521972/153209242-01e760b7-84a2-4df0-a505-d87011d271d6.png)

 LinkedList와는 다르게 한 노드에서 다른 노드를 가리키는 pointer가 양방향으로 있습니다. 즉, 다음 노드를 가리키는 next pointer 이외의 이전노드를 가리키는 prev pointer가 하나 더 추가 된 것입니다.

이것이 LinkedList와는 구분되는 Double LinkedList의 특성입니다.

그렇다면 왜 Double LinkedList구조는 LinkedList보다 공간은 더 많이 차지할텐데 나오게 된 것일까요?

그 이유는 다음과 같습니다.

- LinkedList는 순회를 할 때 Random Access가 불가능하고 반드시 Head 노드에서 출발해야 했기 때문에 O(N)의 시간복잡도가 발생하는 반면 Double LinkedList는 시작점이 Head 노드 뿐만 아니라 Tail 노드가 될 수도 있어 탐색 시간 절감 된다는 장점이 있기 때문입니다.

<br>

![image](https://user-images.githubusercontent.com/79521972/153209822-06f57bab-db1f-4784-8ff3-2addcdaa42ee.png)

Double LinkedList 또한 LinkedList처럼 두 가지의 dummy node를 사용하여 구성됩니다.

위 그림은 head 노드의 next pointer는 tail 노드를 tail노드의 prev pointer는 head 노드를 서로서로 가리키고 있는 모습입니다.



## 추가

LinkedList에서 추가는 null을 가리키고 있는 tail 노드 뒤에 추가해 주었는데 이렇게 하기 위해서는 head 노드부터 타고 들어가 null이 나올 때까지 반복문을 돌며 tail 노드를 찾았었는데 Double LinkedList는 시작점이 head와 tail 두 가지의 선택지가 있어 추가를 할 때는 tail 노드를 타고 들어가 tail 노드의 pointer들을 조작해 주면 훨씬 효율적으로 추가가 가능해 집니다.

![image](https://user-images.githubusercontent.com/79521972/153213564-9ea3b929-7885-4b4a-b70f-863dde470333.png)

위의 그림처럼 추가를 할 때는 tail 노드를 타고 들어가 tail 노드의 prev pointer는 추가 노드를 가리키게 하고 tail 노드의 prev pointer가 가리키고 있던 노드의 next pointer를 추가 노드에 연결해 주면 됩니다.

- 따라서 이에 대한 시간 복잡도는 리스트 크기와 상관없이 무조건 1이므로 O(1)이다.



## 검색 by index

ArrayList(배열) 같은 경우는 index를 통해 Random Access가 가능했기 때문에 1번만에 접근이 가능했고 (Single) LinkedList는 head 노드를 통해 타고 들어가서 순회하는 것이기 때문에 O(N)의 시간 복잡도가 발생했습니다.

![image](https://user-images.githubusercontent.com/79521972/153216083-095930a2-6a79-42c1-9d3c-6d82f10777fb.png)

Double LinkedList는 기본적으로 순회하는 방식은 LinkedList와 같지만 위 그림과 같이 index로 구분할 수 있는 3개의 노드가 있다고 가정할 때 0번 인덱스에 접근 하고자 할 때는 head 노드에서 타고 들어가 순회를 하는 것이 빠를 것이고 3번 인덱스에 접근 하고자 할 때는 tail 노드에서 타고 들어가 순회를 하는 것이 더 빠를 것입니다.

- 따라서 이를 사용하기 위해 리스트의 크기의 절반 값을 기준으로 그것보다 작으면 head 노드에서 그것보다 크면 tail 노드에서 시작하는 것으로 검색을 진행합니다.
  - 그렇기 때문에 시간 소요가 Single LinkedList보다 절반만큼 더 빠를 것을 예상할 수 있지만 시간 복잡도를 봤을 때 점근 표기법에 의해 상수항은 생략되어 O(N/2)이 아닌 O(N)을 가지게 되는 것입니다.





## 삽입(by index)

앞서 추가는 tail 노드를 타고 들어가 pointer 연결의 수정으로 할 수 있었습니다. 그렇다면 특정 위치(index)에 노드를 삽입하고 싶을 땐 어떻게 할까요?

![image-20220209231028735](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220209231028735.png)

우선 원하는 노드를 알아내야 하기 때문에 LinkedList에서 했던 것과 마찬가지로 curr 노드를 이동시켜 찾습니다.

- 단, Double LinkedList의 이점을 살려 head 노드에서 타고 들어갈 지와 tail 노드에서 타고 들어갈 지에 따라 시간 소모가 줄어들 수 있습니다.



![image](https://user-images.githubusercontent.com/79521972/153219119-052f9bc6-0437-488f-b129-ef7a0f7d743e.png)

![image](https://user-images.githubusercontent.com/79521972/153218893-3e2c21ce-2497-4be9-a13c-edddbdfd69a2.png)

curr 노드가 알맞은 위치에 있다면 새로운 노드를 생성 후 이 노드와 prev 노드 curr 노드 사이에 끼워넣는 연결을 만들어 줍니다.

1. new node의 next pointer는 curr노드를 가리키고 prev pointer는 prev 노드를 가리키게 합니다.
2. prev 노드의 next pointer를 new node에 연결합니다.
3. curr노드의 prev pointer를 new node에 연결합니다.



## 삭제(by index)

![image-20220209232404971](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220209232404971.png)

index에 의한 삭제 역시 가장 먼저 해당 index까지 순회하여 접근하는 것이 우선으로 되어야 합니다.

그 이후 

1. curr노드의 next pointer가 가리키고 있던 노드의 prev pointer를 prev 노드와 연결합니다.
2.  curr노드의 prev pointer가 가리키고 있던 prev 노드의 next pointer를 curr 노드의 next pointer가 가리키고 있던 노드에 연결시킵니다.



![image](https://user-images.githubusercontent.com/79521972/153220708-d422888c-e30b-42b8-97e7-7e641df800b3.png)

그 결과 prev 노드와 삭제된 노드 다음에 있던 노드가 직접적으로 연결되어 나타난 모습입니다.





# Double LinkedList 실습

그 전 리스트 실습들과 같이 이번에도 IList를 구현상속하여 Double LinkedList의 기능들을 코드로써 구현해 보도록 하겠습니다.<br>

MyDoubleLinkedList.java

```java
package list;

public class MyDoubleLinkedlist<T> implements IList<T> {
    
    ...
}
```

<br>

```java
private class Node {
    T data;
    Node prev;
    Node next;
    
    Node(T data) { this.data = data; }
    
    Node(T data, Node prev, Node next) {
        this.data = data;
        this.prev = prev;
        this.next = next;
    }
}
```

Double LinkedList 역시 노드 기반 구조이기 때문에 Node 클래스를 선언하였고 이 안의 내용은 이론으로 배웠던 것과 일치한다.

- 제네릭 타입의 data
- prev 노드포인터와 next 노드포인터
- data만을 인자로 받는 생성자와 data, prev pointer, next pointer를 인자로 받는 생성자

  

## 멤버 변수 선언

```java
private Node head;
private Node tail;
private int size;
```

- 리스트에 타고 들어가기 위한 dummy node인 head 노드와 tail 노드 선언
- 리스트 크기를 관리할 변수 size 선언



## 생성자

선언한 멤버 변수에 대한 초기화 과정이 이루어진다.

```java
public MyDoubleLinkedList() {
    this.size = 0;
    this.head = new Node(null);
    this.tail = new Node(null);
    
    this.head.next = this.tail;
    this.tail.prev = this.head;
}
```

- 아무 데이터(원소)가 들어가 있지 않은 빈 리스트 이므로 head 노드와 tail 노드 모두 null 노드로 초기화 시켜줍니다.
- 리스트가 비어있기 때문에 head 노드의 next pointer 는 tail노드를 가리키고 tail 노드의 prev pointer는 head 노드를 가리키고 있는 구조입니다.



## size

```java
@Override
public int size() {
    return this.size;
}
```

가장 쉬운 size메소드 먼저  입력해 주겠습니다. 앞선 ArrayList, LinkedList들과 마찬가지로 멤버변수인 size를 리턴해 줍니다.

<br>

## clear

```java
@Override
public void clear() {
    this.size = 0;
    this.head = new Node(null);
    this.tail = new Node(null);
    this.head.next = this.tail;
    this.tail.prev = this.head
}
```

그 다음으로 쉬운 메소드는 clear()인데 여기서는 리스트에 아무런 데이터가 들어오지 않은 상태로 만들어 주는 과정, 즉 생성자에서 진행했던 코드를 그대로 사용해 줍니다.

<br>

## add

```java
@Override 
public void add(T t) {
   	Node last = this.tail.prev;
    Node node = new Node(t, last, tail);
    last.next = node;
    this.tail.prev = node;
    this.size++;
}
```

add 메소드는 Single LinkedList에서와 달리 tail 노드도 dummy node로써 역할을 하기 때문에 tail 노드의 prev pointer가 가리키는 노드가 리스트의 마지막 데이터이고 이를 last 노드에 저장합니다.

그리고 새로이 추가 할 노드는 t라는 data를 담고 있으며 이전 노드가 last, next 노드가 tail인 노드로 생성을 해 줍니다.

그 이후 각각의 pointer들끼리 연결을 해 주어야 하기 때문에 last의 next pointer는 new node에 연결하고 tail 노드의 prev pointer 또한 new node에 연결시켜주게 되면 끝납니다.

마지막으로 노드를 추가했으므로 사이즈를 1증가 시키는 과정도 잊지 맙시다.

<br>

## get

```java
@Override
public T get(int index){
    if (index >= this.size || index < 0) {
        throw new IndexOutOfBoundsException();
    }
    
    int i = 0;
    Node curr = null;
    if(index < this.size / 2) { //index가 head 노드와 더 가까운 경우 
        curr = this.head.next;
        while(i++ < index) {
            curr = curr.next;
        }
    }else { //index가 tail 노드와 더 가까운 경우
        curr = this.tail.prev;
        while(i++ < (this.size - index - 1)) {
            curr = curr.prev;
        }
    }
    return curr.data;
}
```

get은 항상 그랬듯이 일단 먼저 index의 범위를 예외처리를 해줍니다.

index로 탐색하는 것은 어디랑 더 가깝냐에 따라 head에서 시작하는 것과 tail에서 시작하는 것으로 나뉘는데 이는 리스트의 크기의 절반을 기준으로 나뉘기 때문에 if문으로 이를 구별하였습니다.

- head 노드와 더 가까운 경우 :
  - 처음 시작할 curr 노드를 head 노드의 다음 노드로 지정해줍니다.
  - 반복문을 index만큼만 돌면 되기 때문에 while의 조건을 index보다 작은 것으로 하였고 curr을 원하는 index에 갈 때까지 순회시킵니다.
- tail 노드와 더 가까운 경우:
  - 처음 시작할 curr 노드를 tail 노드의 이전 노드로 지정해줍니다.
  - 반복문을 index만큼 돌면 안 되고 거꾸로 index를 찾아가는 것이기 때문에 size - index - 1 만큼 반복을 진행해야 마지막 노드에서 size - index - 1만큼 순회를 했을 때 해당 인덱스에 도달하게 되기 때문에 범위를 위처럼 지정한 것입니다.

<br>

## insert

```java
@Override 
public void insert(int index, T t);{
    if(index > this.size || index <0){
        throw new IndexOutOfBoundsException();
    }
    Node prev;
    Node curr;
    
    int i = 0 ;
    if(index < this.size / 2) {
        prev = this.head;
        curr = this.head.next;
        
        while(i++ < index) {
            prev = prev.next;
            curr = curr.next;
        }    
    } else {
        curr = this.tail;
        prev = this.tail.prev;
        
        while (i++ < (this.size - index)) {
            curr = curr.prev;
            prev = prev.prev;
        }
    }
     Node node = new Node(t, prev, curr);
     prev.next = node;
     curr.prev= node;
     this.size++;
}
```

insert는 삽입하고자 하는 노드의 시점을 기준으로 prev 노드와 curr노드의 연결을 이어주는 것이 중요합니다.

1. 먼저 prev 노드와 curr노드를 선언해줍니다.
2. insert 역시도 head와 tail 노드 중 어디서부터 시작할 지를 size / 2를 기준으로 나누어 줍니다.
3. head 노드와 가까운 경우:
   - curr노드는 head 노드의 next pointer가 가리키고 있는 다음노드, prev노드는 head 노드가 됩니다.
   - 반복문을 통해 index만큼 반복하여 prev노드와 curr노드가 알맞은 위치에 놓이게 합니다.
4. tail 노드와 가까운 경우:
   - curr 노드는 tail 노드, prev노드는 tail 노드의 prev pointer가 가리키는 이전노드가 됩니다.
   - 반복문을 통해 이번에는 curr노드가 tail 노드이기 때문에 size - index만큼 반복하여 prev 노드와 curr 노드를 알맞은 위치에 놓이게 합니다.
5. curr 노드와 prev 노드가 알맞게 위치했으니 이제 새로운 노드를 만드는데 이 노드는 t라는 data를 가지고 있고 prev pointer로는 prev노드, next pointer로는 curr노드를 가진 노드가 됩니다.
6. prev 노드의 next pointer를 new node를 가리키게 하고 curr 노드의 prev pointer역시 new node를 가리키도록 합니다.
7. 마지막으로 역시 리스트의 크기가 늘어났으므로 size를 1 증가시킵니다.

<br>

## deleteByindex

```java
@Override
public boolean deleteByindex(int index){
	if(index >= this.size || index < 0){
        throw new IndexOutOfBoundsException();
    }
    
    Node prev = null;
    Node curr = null;
    Node next = null;
    
    int i = 0;
    if(index < this.size / 2) {
        prev = this.head;
        curr = this.head.next;
        
        while(i++ < index) {
            prev = prev.next;
            curr = curr.next;
        }
        
        prev.next = curr.next;
        curr.next.prev = prev;
        curr.next = null;
        curr.prev = null;
    } else {
        curr = this.tail.prev;
        next = this.tail;
        
        while(i++ < (this.size - index - 1)) {
            curr = curr.prev;
            next = next.prev;
        }
        
        curr.prev.next = next;
        next.prev = curr.prev;
        curr.next = null;
        curr.prev = null;
    }
    this.size--;
    return true;
}
```

deleteByindex는 index로 노드를 찾아 해당 노드를 삭제시키는 것이기 때문에 일련의 과정들은 앞서 진행했던 것과 유사하게 흘러갑니다.

1. 여기서는 prev 노드, curr 노드와 더불어 next 노드까지 선언해 주겠습니다.
   - tail 노드부터 시작하는 경우 더 편하게 생각하기 위함
2. index가 head노드와 더 가까운 경우:
   - 삭제하고자 하는 노드를 curr 노드로 할 것이기 때문에 prev 노드는 head 노드, curr노드는 head 노드의 next pointer가 가리키는 노드가 됩니다.
   - head노드에서 출발하기 때문에 index만큼 반복문을 진행하고 그 안에서 prev 노드와 curr노드가 index 위치까지 순회하도록 합니다.
   - prev 노드의 next pointer를 curr 노드의 next pointer가 가리키고 있는 노드와 연결합니다.
   - curr 노드의 next pointer가 가리키고 있는 노드의 prev pointer를 prev 노드와 연결합니다.
   - curr 노드의 모든 pointer 연결을 null로 초기화 시킴으로써 끊습니다.
   - 붕떠 있는 curr 노드는 자바의 가비지 컬렉터에 의해 수집되어 사라질 것입니다.
3. index가 tail노드와 더 가까운 경우:
   - 삭제하고자 하는 curr 노드는 tail노드의 prev pointer가 가리키고 있는 노드, next 노드는 tail 노드가 됩니다.
   - curr 노드가 tail 노드의 이전노드에 있으므로 size - index - 1 만큼 반복문을 진행하고 그 안에서 curr 노드와 next 노드가 반대방향으로 순회하도록 prev pointer를 타면서 이동합니다.
   - curr 노드의 prev pointer가 가리키고 있는 노드의 next pointer를 next 노드에 연결합니다.
   - next 노드의 prev pointer를 curr 노드의 prev pointer가 가리키고 있는 노드와 연결합니다.
   - curr 노드의 모든 pointer 연결을 null로 초기화 시킴으로써 끊습니다.
4. 삭제 과정이 이루어 졌으므로 size를 1 감소시킵니다.

<br>

## contains

```java
@Override
public boolean contains(T t) {
    Node curr = this.head.next;
    
   	while (curr != null) {
        if(curr.data != null && curr.data.equals(t)) {
            return true;
        }
        curr = curr.next;
    }
    return false
}
```

contains 메소드 역시 마찬가지로 반복문을 통해 하나씩 돌면서 equals 메소드로 리스트의 원소와 일치하면 true(1) 아니면 false(0)을 반환해 주는 알고리즘입니다.

- 이 역시도 노드끼리의 연결을 바꿀 필요가 없기 때문에 curr 노드만 지정하더라고 구현이 가능합니다.
- curr 노드가 끝까지 순회하는데 중간에 curr의 data가 null이 아니면서(and) 인자로 받아온 t와 같다면 true를 반환해 주고 그렇지 않으면 while문이 종료되고 false를 반환하도록 해 줍니다.

<br>

## indexOf

```java
@Override
public boolean indexOf(T t) {
    Node curr = this.head.next;
    
    int index = 0;
    while (curr != null) {
        if(curr.data != null && t.equals(curr.data)) {
            return index;
        }
        curr = curr.next;
        index++;
    }
    return -1;
} 
```

indexOf 메소드는 인자로 받아온 t와 일치하는 노드의 index를 반환하는 메소드로 curr 노드를 생성하고 while 반복문을 index를 0부터 증가시키면서 리스트의 끝까지 반복시키고 curr 노드를 이동시킵니다. 그리고 반복문이 종료된 후 curr 노드의 data와 인자 t가 일치할 때의 index를 반환하면 됩니다.

```java 
Node curr = null;
Node prev = null;
Node next = null;

if (index < this.size / 2) {
    curr = this.head.next;
    prev = this.head;
    
    int index = 0;
    while(curr != null) {
        if(curr.data != null && curr.data.equals(t)) {
            return index;
        }
        curr = curr.next;
        prev = prev.next;
        index++;
    }
    
} else {
    curr = this.tail.prev;
    next = this.tail;
    
    int index = 0
    while(curr != null) {
        if(curr.data != null && curr.data.equals(t)) {
            return index - this.size - 1;
        }
        curr = curr.prev;
        next = next.prev;
        index++
    }
    
}
```

위 코드는 첫 번째 코드보다는 복잡하지만 tail에서 시작하는 경우를 추가하여 코드의 시간 소요가 절반으로 줄도록 할 수 있는 코드입니다.

각자의 장단점이 있으니 상황에 맞게 코딩하는 것이 가장 중요할 것 같습니다.

<br>

## isEmpty

```java
@Override 
public boolean isEmpty() {
    return this.head.next == this.tail;
}
```

Double LinkeList에서 비어있다는 의미는 head 노드와 tail 노드가 서로를 가리키고 있는 구조이기 때문에 head 노드의 next pointer가 가리키고 있는 노드가 tail 노드인지 아닌 지를 판별하면 됩니다.

<br>



# 관련된 문제

챕터 2에서 배운 내용과 관련된 문제로 백준의 11728번 배열 합치기가 있으니 이 자료구조에 대한 이해를 위해 풀어보면 좋을 것 같습니다.

[11728 배열 합치기](https://www.acmicpc.net/problem/11728, "백준")



이상 리스트 자료구조에 대한 내용이었습니다.

























