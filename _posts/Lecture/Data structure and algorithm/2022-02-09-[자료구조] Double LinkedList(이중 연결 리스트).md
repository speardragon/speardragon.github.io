---
layout: single
title: "[자료구조] Double LinkedList(이중 연결 리스트)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'LinkedList']
tag: ['Data structure', 'Algorithm', '이중연결리스트', 'DoubleLinkedList']
toc: true
toc_sticky: true
---

이번 시간에는 LinkedList의 종류 중에서 Double LinkedList에 대해서 알아 볼 것이다.

이전 내용에 있는 LinkedList는 앞에 'Single'이 생략 되어있는 셈이라 생각하면 된다. 그럼 무엇이 single이고 double일까?

![image](https://user-images.githubusercontent.com/79521972/153207800-fa48ed48-48d6-45da-97cd-606ccc15a8a7.png)

위 그림은 Double LinkedList의 대략적인 구조이다.

<br>

아래 그림을 보자.

![image](https://user-images.githubusercontent.com/79521972/153209242-01e760b7-84a2-4df0-a505-d87011d271d6.png)

 LinkedList와는 다르게 한 노드에서 다른 노드를 가리키는 pointer가 양방향으로 있습니다. 즉, 다음 노드를 가리키는 next pointer 말고도 이전노드를 가리키는 prev pointer 하나가 더 추가 된 것이다.

이것이 LinkedList와는 구분되는 Double LinkedList의 특성이다.

그렇다면 왜 Double LinkedList구조는 LinkedList보다 공간은 더 많이 차지할텐데 나오게 된 것일까?

그 이유는 다음과 같다.

- LinkedList는 순회를 할 때 Random Access(임의 접근 방식)가 불가능하고 반드시 Head 노드에서 출발해야 했기 때문에 O(N)의 시간복잡도가 발생하는 반면 Double LinkedList는 시작점이 Head 노드 뿐만 아니라 Tail 노드가 될 수도 있어 탐색 시간이 절반으로 절감 된다는 장점이 있기 때문이다.

<br>

Double LinkedList는 LinkedList와는 다르게 두 가지의 dummy node를 사용하여 구성된다.

- LinkedList는 한 가지의 dummy node(head)

![image](https://user-images.githubusercontent.com/79521972/153209822-06f57bab-db1f-4784-8ff3-2addcdaa42ee.png)

위 그림은 head 노드의 next pointer는 tail 노드를 tail노드의 prev pointer는 head 노드를 서로서로 가리키고 있는 모습이다.(초기 상태)

<br>

## 추가

LinkedList에서 추가는 null을 가리키고 있는 tail 노드 뒤에 추가해 주었는데 이렇게 하기 위해서는 head 노드부터 타고 들어가 null이 나올 때까지 반복문을 돌며 tail 노드를 찾았었는데 

Double LinkedList는 시작점이 head와 tail 두 가지의 선택지가 있어 추가를 할 때는 tail 노드를 타고 들어가 tail 노드의 pointer들을 수정하면 LinkedList보다 더 훨씬 효율적으로 추가가 가능해 진다.

![image](https://user-images.githubusercontent.com/79521972/153213564-9ea3b929-7885-4b4a-b70f-863dde470333.png)

위의 그림처럼 추가를 할 때는 tail 노드를 타고 들어가 tail 노드의 prev pointer는 추가된 노드를 가리키게 하고 tail 노드의 prev pointer가 가리키고 있던 노드의 next pointer를 추가 노드에 연결해 주면 된다.

- 따라서 이에 대한 시간 복잡도는 리스트 크기와 상관없이 무조건 1이므로 O(1)이다.



## 검색 by index

ArrayList(배열) 같은 경우는 index를 통해 Random Access(임의 접근)가 가능했기 때문에 한 번만에 접근이 가능했고 (Single) LinkedList는 head 노드를 통해 타고 들어가서 순회하는 것이기 때문에 O(N)의 시간 복잡도가 발생했다.

아래와 같이 index로 구분할 수 있는 3개의 노드로 이루어진 이중 연결 리스트가 있다고 하자.

![image](https://user-images.githubusercontent.com/79521972/153216083-095930a2-6a79-42c1-9d3c-6d82f10777fb.png)

Double LinkedList가 기본적으로 순회하는 방식은 LinkedList와 거의 동일하지만 위 그림에서 0번 인덱스에 접근 하고자 할 때는 head 노드에서 타고 들어가 순회를 하는 것이 빠를 것이고 3번 인덱스에 접근 하고자 할 때는 tail 노드에서 타고 들어가 순회를 하는 것이 더 빠를 것이다.

- 따라서 이를 사용하기 위해 리스트의 크기의 절반 값을 기준으로 그것보다 작으면 head 노드에서 그것보다 크면 tail 노드에서 시작하는 것으로 검색을 진행한다.
- 그렇기 때문에 시간 소요가 Single Linked List보다 절반만큼 더 빠를 것을 예상할 수 있지만 시간 복잡도를 봤을 때 점근 표기법에 의해 상수항은 생략되기 떄문에 O(N/2)이 아닌 O(N)을 가지게 되는 것이다.(하지만 실제 시간 소요는 더 효율적임)

이처럼 head 노드를 타고 들어가는 것과 tail 노드를 타고 들어가는 선택지를 만든 것이 이중 연결 리스트의 핵심이고 이를 최대한 활용할 것이다.

<br>



## 삽입(by index)

앞서 추가는 tail 노드를 타고 들어가 pointer 연결의 수정으로 할 수 있었습니다. 그렇다면 특정 위치(index)에 노드를 삽입하고 싶을 땐 어떻게 할까?

![image](https://user-images.githubusercontent.com/79521972/155283417-62ad47ab-b225-42e3-9686-ab422b066b80.png)

우선 원하는 노드를 알아내야 하기 때문에 Linked List에서 했던 것과 마찬가지로 curr 노드를 이동시켜 찾는다.

- 단, Double LinkedList의 이점을 살려 head 노드에서 타고 들어갈 지와 tail 노드에서 타고 들어갈 지에 따라 시간 소모가 줄어들 수 있다.

curr 노드가 알맞은 위치에 있다면 새로운 노드를 생성 후 이 노드를 prev 노드와 curr 노드 사이에 끼워넣는 연결을 만들어 줍니다.



![image](https://user-images.githubusercontent.com/79521972/153219119-052f9bc6-0437-488f-b129-ef7a0f7d743e.png)

![image](https://user-images.githubusercontent.com/79521972/153218893-3e2c21ce-2497-4be9-a13c-edddbdfd69a2.png)



1. new node의 next pointer는 curr노드를 가리키고 prev pointer는 prev 노드를 가리키게 한다.
2. prev 노드의 next pointer를 new node에 연결한다.
3. curr노드의 prev pointer를 new node에 연결한다.



## 삭제(by index)

![image](https://user-images.githubusercontent.com/79521972/155283479-c23b8b18-517d-4fc3-9ba3-f0b48add3592.png)

index에 의한 삭제 역시, 먼저 해당 index까지 순회하여 접근한다.

그 이후 

1. curr노드의 next pointer가 가리키고 있던 노드의 prev pointer를 prev 노드와 연결한다.
2.  curr노드의 prev pointer가 가리키고 있던 노드(prev 노드)의 next pointer를 curr 노드의 next pointer가 가리키고 있던 노드에 연결시킨다.



![image](https://user-images.githubusercontent.com/79521972/153220708-d422888c-e30b-42b8-97e7-7e641df800b3.png)

위 사진은 그 결과 prev 노드와 삭제된 노드 다음에 있던 노드가 직접적으로 연결되어 나타난 모습이다.



<br>

# Double LinkedList 실습

이전의 리스트(ArrayList, LinkedList) 실습들과 똑같이 이번에도 IList를 구현상속하여 Double LinkedList의 기능들을 코드로써 구현해 보자.<br>

MyDoubleLinkedList.java

```java
package list;

public class MyDoubleLinkedlist<T> implements IList<T> {
    
    ...
}
```

<br>

### 노드 클래스

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

Double LinkedList 역시 노드 기반 구조이기 때문에 Node 클래스를 선언하였고 이 안의 내용은 위에서 이론으로 배웠던 것과 일치한다.

- 제네릭 타입의 data
- prev 노드포인터 / next 노드포인터
- data만을 인자로 받는 생성자와 data, prev pointer, next pointer를 인자로 받는 생성자

  

### 멤버 변수 선언

```java
private Node head;
private Node tail;
private int size;
```

- 리스트에 타고 들어가기 위한 dummy node인 head 노드와 tail 노드 선언
- 리스트 크기를 관리할 변수 size 선언



### 생성자

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

- 아무 데이터(원소)가 들어가 있지 않은 빈 리스트 이므로 head 노드와 tail 노드 모두 null 노드로 초기화한다
- 리스트가 비어있기 때문에 head 노드의 next pointer 는 tail노드를 가리키고 tail 노드의 prev pointer는 head 노드를 가리키고 있는 구조이다.

<br>

### size

```java
@Override
public int size() {
    return this.size;
}
```

가장 쉬운 size메소드 먼저  입력해 주겠습니다. 앞선 ArrayList, LinkedList들과 마찬가지로 멤버변수인 size를 리턴한다.

<br>

### clear

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

그 다음으로 쉬운 메소드는 clear()인데 여기서는 리스트에 아무런 데이터가 들어오지 않은 상태로 만들어 주는 과정, 즉 생성자에서 진행했던 코드를 그대로 사용한다.

<br>

### add

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

add 메소드는 Single LinkedList에서와 달리 tail 노드도 dummy node로써 역할을 하기 때문에 tail 노드의 prev pointer가 가리키는 노드가 리스트의 마지막 데이터이고 이를 last 노드에 저장한다.

그리고 새로이 추가 할 노드는 t라는 data를 담고 있으며 이전 노드가 last, next 노드가 tail인 노드이다.

그 이후 각각의 pointer들끼리 연결을 해 주어야 하기 때문에 last의 next pointer는 new node에 연결하고 tail 노드의 prev pointer 또한 new node에 연결 시킨다.

마지막으로 노드를 추가했으므로 사이즈를 1만큼 증가 시키는 과정도 잊지 말자.

<br>

### get

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

get은 항상 그랬듯이 일단 먼저 index의 범위를 예외처리를 먼저 진행한다.

index로 탐색하는 것은 **어디랑 더 가깝냐에 따라** head에서 시작하는 것과 tail에서 시작하는 것으로 나뉘는데 이는 리스트의 크기의 절반을 기준으로 나뉘기 때문에 if문으로 이를 구분한다.

- **head 노드와 더 가까운 경우 :**
  - 처음 시작할 위치인 curr 노드를 head 노드의 다음 노드로 지정한다.
  - 반복문을 index만큼만 돌면 되기 때문에 while의 조건을`index보다 작은 것`으로 하였고 curr을 원하는 index에 갈 때까지 순회시킨다.
- **tail 노드와 더 가까운 경우:**
  - 처음 시작할 curr 노드를 tail 노드의 이전 노드로 지정한다.
  - 반복문을 index만큼 돌면 안 되고 거꾸로 index를 찾아가는 것이기 때문에 size - index - 1 만큼 반복을 진행해야 마지막 노드에서 size - index - 1만큼 순회를 했을 때 해당 인덱스에 도달하게 되기 때문에 범위를 위처럼 지정한 것이다.

<br>

### insert

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

insert는 삽입하고자 하는 노드의 시점을 기준으로 prev 노드와 curr노드의 연결을 이어주는 것이 중요하다.

1. 먼저 prev 노드와 curr노드를 선언한다.
2. insert 역시도 head와 tail 노드 중 어디서부터 시작할 지를 size / 2를 기준으로 나눈다.
3. head 노드와 가까운 경우:
   - curr노드는 head 노드의 next pointer가 가리키고 있는 다음노드, prev노드는 head 노드가 된다.
   - 반복문을 통해 index만큼 반복하여 prev노드와 curr노드가 알맞은 위치에 놓이게 한다.
4. tail 노드와 가까운 경우:
   - curr 노드는 tail 노드, prev노드는 tail 노드의 prev pointer가 가리키는 이전노드가 된다.
   - 반복문을 통해 이번에는 curr노드가 tail 노드이기 때문에 size - index만큼 반복하여 prev 노드와 curr 노드를 알맞은 위치에 놓이게 한다.(tail노드 부터 시작하기 때문에 get에서 size - index - 1 인것과 1만큼의 차이가 있음)
5. curr 노드와 prev 노드가 알맞게 위치했으니 이제 새로운 노드를 만드는데 이 노드는 t라는 data를 가지고 있고 prev pointer로는 prev노드, next pointer로는 curr노드를 가진 노드가 된다.
6. prev 노드의 next pointer를 new node를 가리키게 하고 curr 노드의 prev pointer역시 new node를 가리키도록 한다.
7. 마지막으로 역시 리스트의 크기가 늘어났으므로 size를 1 증가시킨다.

<br>

### deleteByindex

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

deleteByindex는 index로 노드를 찾아 해당 노드를 삭제시키는 것이기 때문에 일련의 과정들은 바로 앞에서 진행했던 것과 유사하게 흘러간다.

1. 여기서는 prev 노드, curr 노드와 더불어 next 노드까지 선언한다.
   - tail 노드부터 시작하는 경우 더 편하게 생각하기 위함
2. **index가 head노드와 더 가까운 경우:**
   - 삭제하고자 하는 노드를 curr 노드로 할 것이기 때문에 prev 노드는 head 노드, curr노드는 head 노드의 next pointer가 가리키는 노드가 된다.
   - head노드에서 출발하기 때문에 index만큼 반복문을 진행하고 그 안에서 prev 노드와 curr노드가 index 위치까지 순회하도록 한다.
   - prev 노드의 next pointer를 curr 노드의 next pointer가 가리키고 있는 노드와 연결한다.
   - curr 노드의 next pointer가 가리키고 있는 노드의 prev pointer를 prev 노드와 연결한다.
   - curr 노드의 모든 pointer 연결을 null로 초기화 시킴으로써 끊는다.
   - 붕떠 있는 curr 노드는 자바의 가비지 컬렉터에 의해 수집되어 사라진다.
3. **index가 tail노드와 더 가까운 경우:**
   - 삭제하고자 하는 curr 노드는 tail노드의 prev pointer가 가리키고 있는 노드, next 노드는 tail 노드가 된다.
   - curr 노드가 tail 노드의 이전노드에 있으므로 size - index - 1 만큼 반복문을 진행하고 그 안에서 curr 노드와 next 노드가 반대방향으로 순회하도록 prev pointer를 타면서 이동한다.
   - curr 노드의 prev pointer가 가리키고 있는 노드의 next pointer를 next 노드에 연결한다.
   - next 노드의 prev pointer를 curr 노드의 prev pointer가 가리키고 있는 노드와 연결한다.
   - curr 노드의 모든 pointer 연결을 null로 초기화 시킴으로써 연결을 끊는다.
4. 삭제 과정이 이루어 졌으므로 size를 1 감소시킨다.

<br>

### contains

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

contains 메소드 역시 마찬가지로 반복문을 통해 하나씩 돌면서 equals 메소드로 리스트의 원소와 일치하면 true(1) 아니면 false(0)을 반환해 주는 알고리즘이다.

- 이 역시도 노드끼리의 연결을 바꿀 필요가 없기 때문에 curr 노드만 지정하여 구현한다.
- curr 노드가 끝까지 순회하는데 중간에 curr의 data가 null이 아니면서(and) 인자로 받아온 t와 같다면 true를 반환해 주고 그렇지 않으면 while문이 종료되고 false를 반환하도록 한다.

<br>

### indexOf

😊**첫 번째 방법**

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

indexOf 메소드는 인자로 받아온 t와 일치하는 노드의 index를 반환하는 메소드로 curr 노드를 생성하고 while 반복문을 index를 0부터 증가시키면서 리스트의 끝까지 반복시키고 curr 노드를 이동시킨다. 

그리고 반복문이 종료된 후 curr 노드의 data와 인자 t가 일치할 때의 index를 반환하면 된다.

만약 없다면 -1을 반환한다.

<br>

😊**두 번째 방법**

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

### isEmpty

```java
@Override 
public boolean isEmpty() {
    return this.head.next == this.tail;
}
```

Double LinkeList에서 비어있다는 의미는 head 노드와 tail 노드가 서로를 가리키고 있는 구조이기 때문에 head 노드의 next pointer가 가리키고 있는 노드가 tail 노드인지 아닌 지를 판별하면 된다.

<br>



## 관련된 문제

챕터 2에서 배운 내용과 관련된 문제

- [<u>11728 배열 합치기</u>](https://www.acmicpc.net/problem/11728 "백준")

<br>

이상 List(리스트) 자료구조에 대한 내용이었습니다.

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

























