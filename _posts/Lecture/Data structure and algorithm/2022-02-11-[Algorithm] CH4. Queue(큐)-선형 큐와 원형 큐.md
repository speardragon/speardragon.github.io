---
layout: single
title: "[Algorithm] CH4. Queue(큐)-선형 큐와 원형 큐"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Queue']
tag: ['Data structure', 'Algorithm', 'Queue', '큐', '선형 큐', '원형 큐']

toc_sticky: true
---



이번 시간에 배워 볼 내용은 `Queue`(큐)이다. 

### 큐(Queue)란?

queue는 [<u>지난시간</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/Algorithm-CH3.-Stack(%EC%8A%A4%ED%83%9D)/, "Stack")(링크 참조)에 배운 스택과 달리 선입선출(First-In-First-Out-FIFO)의 구조를 가지고 있다.

즉, 가장 먼저 들어간 것이 가장 먼저 나오게 되는 구조인 것이다.

 선입선출(FIFO)의 예로는 순서가 보장된 처리를 가지고 있는 '사용자가 몰리는 서버'와 같은 경우가 있다. 대학생이 수강신청을 하러 인터넷에 들어간 경우나 내가 좋아하는 가수의 티켓팅을 하기 위해 인터넷에 들어갔을 때 대기 순서와 같은 것을 본 적이 있을 것이다. 이러한 경우 가장 먼저 들어간 사람을 우선순위로 다음 페이지로 연결이 되기 때문에 FIFO 구조라고 할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/155306726-4f009a19-8e93-405d-9db1-22c63241e886.png)

<img src="https://user-images.githubusercontent.com/79521972/155306788-0ed500c1-7eed-4f7f-9cbf-c8ea64e95bd3.png" alt="image" style="zoom:67%;" />

<br>

큐와 스택은 나중에 배울 자료구조와 탐색에 기저가 되는 내용이기 때문에 아주 자세히 다루어 볼 것이다.

큐에는 원형 큐와 선형 큐가 있는데 먼저 선형 큐에 대하여 먼저 알아보도록 하겠다.


## 큐의 기본 동작

- push(), offer(), add() : 큐에도 스택과 마찬가지로 데이터를 넣는 push()와 같은 기능이 존재한다.
  하지만 언어마다 API명이 다른 경우가 있어 이에 대해 offer() 혹은 add()로 사용하기도 한다.
- pop(), poll() : 데이터를 추출하는 작업. poll()로 쓰기도 합니다.
- peek() : 데이터를 확인하는 작업

<br>



## 큐의 구조

아래 그림은 선형 큐의 구조를 도식화 한 것입니다.

![image](https://user-images.githubusercontent.com/79521972/153537444-6246b6f4-6dfe-4a86-99b1-3cfaa16e5cc1.png)

큐는 기본적으로 데이터를 삽입하는 `enqueue` 와 데이터를 빼내는(삭제) `dequeue` 과정이 있습니다.

먼저 데이터를 큐 안에 넣게 되면 rear(끝)쪽으로 데이터가 들어가면서 계속 쌓이게 됩니다. 그래서 가장 처음에 넣는 데이터는 front(맨 앞)에 위치하게 되어 데이터를 제거할 때는 front 쪽에 있는 데이터를 빼내게 됩니다. 

`front`는 가장 먼저 들어온 데이터의 index를 뜻하고 `rear`는 가장 나중에 들어온 데이터의 index를 나타내기 때문에 rear의 index 값은 계속해서 변하는 구조를 가지고 있습니다.
<br>

큐 역시 스택처럼 구현함에 있어서 배열과 연결리스트로 구현하는 방법이 있는데 

LinkedList로 큐를 구현할 때는 rear를 신경쓰지 않고 큐를 구현하는 반면, 배열로 구현하는 경우 rear가 굉장히 중요하다. 

*연결 리스트와 배열에 관한 포스팅은 다음 링크를 참고하시오.*

- [<u>List(리스트)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/Algorithm-CH2-1.-%EB%A6%AC%EC%8A%A4%ED%8A%B8(List)-ArrayList/)
- [<u>LinkedList(단순 연결 리스트)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/linkedlist/Algorithm-CH2-2.-LinkedList/)



### 연산 

- enqueue() - 큐의 끝(rear)에 데이터 삽입
- dequeue() - 큐의 맨 앞(front)에서 부터 데이터 추출
- peek() - 큐의 맨 앞(front)에 있는 항목 반환(read)
- isfull() - 큐가 꽉 찼는지를 boolean 형태로 반환
- isempry() - 큐가 비어 있는지를 boolean 형태로 반환

<br>

## 배열의 단점을 보완

CH1에서 배웠던 [배열](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/Algorithm-CH2-1.-%EB%A6%AC%EC%8A%A4%ED%8A%B8(List)-ArrayList/)로 큐를 구현하게 되면 문제가 발생한다.

- 배열로 구현 시 크기가 제한 되어있고 빈 공간을 사용하려면 모든 자료를 꺼내거나 자료를 한 칸씩 옮겨야 한다는 문제와 많은 수의 enqueue 및 dequeue 작업을 하는 경우 어느 시점에서 큐가 비어있더라도 자료를 삽입하지 못하는 경우가 발생하는 문제이다. 아래 그림을 보면서 살펴보자. 

아래 그림은 크기가 5인 배열을 나타낸 것이다.

![image](https://user-images.githubusercontent.com/79521972/153538177-07a65eee-5ccc-41ed-b34a-8db4b0f32859.png)

<br>

이 배열에 데이터를 추가해 보자.

![image](https://user-images.githubusercontent.com/79521972/153538315-1f40b468-d9bb-40c1-ba5b-18acd03f4994.png)

데이터 추가만 봤을 때는 큐와 그렇게 다르지 않다. 만약 여기서 데이터 하나를 큐의 방식으로 제거하게 된다면

![image](https://user-images.githubusercontent.com/79521972/153538552-64c123d3-f91a-42b4-8b9a-7f0d2aeacd2a.png)

이처럼 배열에 공백이 생기게 되고 이를 한 칸씩 밀어주는 과정이 반드시 잇따라 진행되어야 하는데 이는 시간 복잡도 O(n)을 요구하기 때문에 비효율적이다.

그렇기 때문에 dequeue를 해 줄때마다 굉장히 시간 복잡도 측면에서 비효율적이기 때문에 이를 보완하기 위하여 큐라는 구조가 나오게 된 것이다.



<br>

# 선형 Queue 구현

자 그럼 LinkedList를 이용하여 queue를 구현해 보도록 하자.



LinkedList와 Stack때 와 마찬가지로 IQueue라는 interface를 정의해서 다음과 같이 offer(), poll(), peek(), size(), clear(), isEmpty() 의 기능의 프로토타입을 선언하였다.

```java
package queue;

public interface IQueue<T> {
    void offer(T data);
    T poll();
    T peek();
    int size();
    void clear();
    boolean isEmpty();
}
```

<br>

MylinkedQueue라는 public class에 IQueue interface를 구현상속하여 기능을 추가할 것이다.

```java
public class MyLinkedQueue<T> implements IQueue<T> {
    
    ...
}
```

<br>

## 노드 클래스

먼저 Node가 필요하기 때문에 Node class를 private class로 만든다.

```java
private class Node {
    T data;
    Node next;

    Node(T data) { this.data = data;}

    NOde(T data, Node next) {
        this.data = data;
        this.next = next;
    }
}
```

<br>

### 멤버변수

head 노드와 tail 노드 그리고 큐의 크기인 size를 멤버변수로 선언한다. 

```java
private Node head;
private Node tail;
private int size;
```

<br>

### 생성자

위에서 선언한 멤버변수에 대한 초기화를 생성자에서 진행한다.

head노드와 tail노드는 모두 dummy node로써 역할을 하기 때문에 null을 가리키게 한다.

```java
public MyLinkedQueue() {
    this.size = 0;
    this.head = new Node(null);
    this.tail = this.head;
}
```

<br>

### offer(T data)

offer는 큐에 데이터를 넣는 작업이다.

```java
@Override
public void offer(T data) {
    Node node = new Node(data);//들어갈 데이터를 가진 노드 생성
    this.tail.next = node;
    this.tail = this.tail.next;
    this.size++;
}
```

첫 데이터가 들어올 때 tail 노드는 head 노드와 같은 위치의 노드이기 때문에 tail 노드의 next pointer를 새로운 노드와 연결하고 tail 노드자체를 그 새로운 노드로 이동시킨다. 그렇게 되면 처음 데이터가 들어왔을 때는 head 노드가 tail 노드를 가리키고 tail노드는 null을 가리키고 있는 구조가 됩니다.

그 이후 또 데이터가 들어오게 된다면 head 노드 -> 새로운 노드 -> tail 노드 -> null 의 구조로 계속 새로운 노드들이 추가된다.

이해하기 쉽게 아래 그림을 보자. 초기에는 dummy node를 제외하곤 어떤 데이터도 들어와 있지 않다.

![image](https://user-images.githubusercontent.com/79521972/153541289-680fe90a-798f-45d0-bc21-04f0e0b6c142.png)

여기서 데이터를 하나 더 추가해 보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153541429-df50dc2b-4d20-4158-ae47-7163167c7353.png)

새로 들어온 데이터는 tail의 다음 노드로 들어옵니다. 그 후 tail 노드 위치를 tail 노드의 next pointer가 가리키던 노드 즉, 새로운 노드로 이동하면 offer(enqueue) 과정이 끝나게 되는 것입니다.

![image](https://user-images.githubusercontent.com/79521972/153754541-335ec4f5-ca46-4488-a4a0-8ddcc3ec08f7.png)

<br>

### isEmpty

isEmpty는 큐가 비어있는 지를 확인하는 메소드로 head 노드의 next pointer가 가리키고 있는 것이 null이라면 큐에 데이터가 아무것도 없는 상태이므로 이를 리턴하면 된다.

```java
@Override
public boolean isEmpty() {
    return this.head.next == null;
}
```

<br>

### poll

poll은 큐에서 데이터를 추출하는 dequeue 과정으로 가장 맨 앞에 들어있는 데이터를 빼내야 한다.

```java
@Override
public T poll() {
    if(this.isEmpty()) {
        throw new IllegalStateException();
    }
    Node node = this.head.next;//삭제하고자 하는 노드
    this.head.next = node.next;
    node.next = null;
    this.size--;
    
    if(this.head.next == null) {
        this.tail = this.head;
    }
    return node.data;
}
```

1. 큐가 비어 있다면 poll을 진행할 수없기 때문에 예외를 발생 시켜 준다.

2. 빼내야 하는 데이터는 맨 앞에 있기 때문에 항상 head 노드의 next pointer가 가리키는 노드일 것이다. 그렇기 때문에 이 노드를 따로 지정합니다.

3. 이 노드의 next pointer가 가리키는 노드와 head 노드와 연결 시켜야 하기 때문에 삭제 노드의 next pointer가 가리키고 있는 노드를 head 노드의 next pointer가 가리키게 한다.

4. 그 후 삭제 노드의 next pointer는 null로 연결을 끊고 큐 안의 데이터를 하나 제거 되었기 때문에 size를 1 감소시킨다.

5. 만약 데이터를 빼내는 과정을 진행했는데 큐가 비어있게 됐다면 생성자에서 했던 과정인 tail노드를 head노드와 같은 노드가 되게 하는 과정을 진행한다.

6. poll은 데이터를 삭제할 뿐 아니라 삭제한 데이터를 리턴하여야 하기 때문에 해당 노드의 data를 반환하면서 마무리 된다.

<br>

### peek()

peek() 메소드는 가장 먼저들어간 데이터를 읽어오는 작업으로 만약 큐가 비어있다면 예외를 발생하고 그렇지 않다면 head 노드의 next pointer가 가리키고 있는 노드의 데이터를 반환한다.

```java 
@Override
public T peek() {
    if (this.isEmpty()) {
        throw new IllegalStateeException();
    }
    return this.head.next.data;
} 
```

<br>

### size

size()는 간단하게 멤버변수 size를 반환한다.

```java
@Override 
public int size() {
    return this.size;
}
```

<br>

### clear

clear() 또한 간단하게 생성자에서 했던 logic을 구현한다.

```java
@Override
public void clear() {
    this.head.next = null;
    this.tail = this.head;
    this.size = 0;
}
```

<br>



**이제는 원형 큐에 대해서 배워 보도록 하겠다.**

# 원형 Queue

앞서 말했듯 Queue를 구현하는 방법에는 두 가지가 있다.

1. LinkedList 를 이용한 구현
2. 배열을 이용한 원형 큐 구현
   - 배열을 이용한 선형 큐 규현은 비효율적임

이 때문에 원형 큐는 배열로 구현하되 배열로 구현했을 때의 단점을 보완할 수 있습니다.(원형 큐의 등장 이유)

원형 큐가 사용되는 예로는,

- CPU 스케줄링, 디스크 스케줄링
  - ![image](https://user-images.githubusercontent.com/79521972/155324239-3f9e6d70-d232-4a23-ba52-fb9baa6dd6e8.png)
- 서로 다른 쓰레드(thread) 또는 프로세스(process)사이에서 자료를 주고 받을 때 자료를 일시적으로 저장하는 용도로 많이 사용한다(asynchronized transmission).
  - IO 버퍼, 파이프, 파일 IO

등이 있다.

<br>

## Circular Queue(원형 큐, 환형 큐)의 구조

아래 그림은 일반적인 원형 큐의 구조를 나타낸 것입니다.

![image](https://user-images.githubusercontent.com/79521972/153546629-548d2eee-ca4b-4787-8b2d-9cf5c8e0eb9b.png)

이름 그대로 원형 형태의 구조에서 데이터가 순환하는 구조이다.

- 1차원 배열 형태의 선형 큐(Linear Queue)를 원형(Circular)으로 구성하여 배열의 처음과 끝을 연결하여 만든 것이다. 

원형 큐 역시 먼저 들어온 데이터가 먼저 나가는 선입선출의 구조인 것에는 변함 없다. 그렇기 때문에 front 와 rear의 위치를 기준으로 데이터를 넣고 빼는 것을 구현해 볼 것이다.

위의 그림처럼 front와 rear의 위치가 동일한 상태가 아무런 데이터가 큐에 들어오지 않은 초기 배열의 상태라고 볼 수 있다.



**기본 연산**

- **Enqueue**

위의 상태에서 데이터가 처음 들어오게 되면 현재 rear의 다음 위치에 데이터가 들어오게 되고 rear의 index 역시 해당 데이터가 들어온 곳의 index가 되는 것이다.

![image](https://user-images.githubusercontent.com/79521972/153547038-ccd5d318-d20e-40c7-a0df-6b198af74292.png)

<br>

이 상태에서 데이터가 또 하나 들어오게 되면 다음 그림과 같이 rear의 위치도 바뀌게 된다.

![image](https://user-images.githubusercontent.com/79521972/153547116-cba521a0-3fc6-4a3f-b990-564d5fc8c60e.png)

이러한 방식으로 데이터 삽입(enqueue)이 이루어 진다.



**Dequeue**

데이터를 삭제(dequeue) 할 때는 반대로 front의 위치가 변하면서 이루어 지게 된다.

![image](https://user-images.githubusercontent.com/79521972/153547484-a4649158-66f1-4d54-9dce-315fcd7e604f.png)

front 위치를 item1의 위치로 옮김으로써 item1이 삭제된다. 실제로 데이터가 삭제되는 것은 아니지만 front가 데이터의 마지막 위치와 관계가 있는 것이기 때문에 이 index를 옮겨 가면서 가상으로 삭제하는 것과 같은 역할을 대신할 수 있는 것이다.

<br>



![image](https://user-images.githubusercontent.com/79521972/153547563-3121c78b-75c3-4a8b-a1d1-71d1e02fd781.png)

그 후 dequeue를 한 번 더 하게되면 front의 위치가 item2가 있는 위치로 오게 되어 item2를 삭제되기 때문에 현재 큐의 모든 데이터가 삭제가 된 빈 큐 상태가 된다. 이때 큐는 비어있다고 하며, front와 rear가 같은 index에 있다는 것을 눈으로 확인 하실 수 있을 것이다.

<br>

![image](https://user-images.githubusercontent.com/79521972/153547928-5524fb81-4f3e-4a3d-9283-25a43050a38b.png)

이로써 큐가 비어있다고 front와 rear의 index가 반드시 0인 것은 아니라는 사실 또한 알 수 있다.

<br>

---

"**큐의 최대 크기 index에서 다음 index는 어떻게 될까?**"

그 다음에 데이터 6개를 순차적으로 넣어 보도록 하겠다.

![image](https://user-images.githubusercontent.com/79521972/153548101-f1241b21-b353-4e4e-a456-2a689bad1c8d.png)

원형 큐의 크기가 8이고 그렇기 때문에 index는 0~7까지 존재한다. 원형 큐의 특성상 7번 index에 데이터를 삽인한 후에 다음에 들어갈 index는 0번이며, 다시 0번 index에 rear가 위치하게 된다.



---

**큐가 꽉찬 상태**

큐가 꽉찬 상태는 흔히 데이터가 모든 index에 들어선 형태로 착각하실 수 있다. 아래의 그림과 같이 말입니다.

![image](https://user-images.githubusercontent.com/79521972/153548788-74bb40d7-15d5-4f80-8bd6-558e9b73dfb5.png)

하지만 이렇게 되면 front와 rear의 index가 같은 상태이기 때문에 원형 큐가 비어있는 상황과 구별하기가 힘듭니다. 구별 할 수 있는 유일한 방법이 사라진 것이다. 

물론 이렇게 시각적으로 보면 데이터가 꽉 찬 상태임을 한 눈에 알 수 있지만 컴퓨터 언어로 이를 구현하는 것은 완전히 다른 문제이다. 특히 front와 rear의 위치를 통해 가상으로 이러한 구조를 다루는 것이기 때문에 데이터를 삽입한 적이 있다면 dequeue 연산을 했다고 하더라도 데이터는 남아 있을 것이기 때문에 확실히 이것을 구별 해야 하는 것이다.

따라서 원형 큐의 꽉차 있는 상태는 다음과 같이 규정한다.

``` 
front = rear + 1
```

이 식을 그림으로 나타내면 아래와 같은 구조를 보인다.



![image](https://user-images.githubusercontent.com/79521972/153549115-f69a3148-b036-40b6-9ed6-d8d898c545c0.png)

원형 구조에서 <span style="color:red">rear가 front보다 한 칸 전에 있을 때</span> 우리는 원형 큐가 `꽉 차 있다`라고 할 것이다.



---

**정리하자면**

- 원형 큐는 고정된 크기의 배열로 구현할 것이다.

  - 데이터가 꽉 차지 않는 이상 무한으로 데이터를 넣고 빼고 할 수 있다.
  - 데이터가 계속해서 갱신 되는 것 일뿐이다.
  - 메모리도 효율적으로 사용하고 배열의 index로 접근하기 때문에 데이터 access 속도가 빠릅니다.
  - 데이터를 넣고 빼는 동작에 따라 index를 옮겨주는 추가 연산을 진행하지 않아도 된다.
  
- 하지만 배열이기 때문에 정해진 크기에서 연산을 진행해야 한다는 단점 또한 있다.

- 원형 큐는 순환 구조이기 때문에 index의 제한이 없어 정해진 크기 이상의 index는 modulo 연산을 통해 처리할 수 있다.
  - **modulo 연산**


```
index % QueueSize
```

- 예를 들어, queuesize가 5인 경우
  - idx = 1 -> index = 1
  - idx = 2 -> index = 2
  - idx = 6 -> index = 1
  - idx = 8 -> index = 3



---

또한 선형 큐와는 다르게 enqueue, dequeue 외에도 

- isFull()
- isEmpty()

와 같은 연산 또한 front와 rear의 위치를 통해 쉽게 알아낼 수 있다.

<br>

# 원형 큐 구현

원형 큐는 선형 큐와 다르게 코드로 구현함에 있어서 배열로 구현하기 때문에 배열의 고유한 index(front, rear)를 다루는 것이 굉장히 중요하다. 

선형 큐와 마찬가지로 IQueue 인터페이스에 정의된 메소드를 구현하는 식으로 진행해 보도록 할 것이다.

```java
package queue;

public interface IQueue<T> {
    void offer(T data);
    T poll();
    T peek();
    int size();
    void clear();
    boolean isEmpty();
}
```

<br>

MyCircularQueue라는 public class에 위의 interface를 구현상속하여 기능을 추가해 보도록 하겠다.

```java
public class MyCircularQueue<T> implements IQueue<T> {
    
    ...
}
```

<br>

### 멤버변수

원형 큐는 배열을 통해 구현을 할 것이기 때문에 노드가 필요하지 않다.

대신 데이터를 저장하기 위한 배열을 선언해 준다. 더불어 위치를 판별하기 위한 rear와 front 변수를 선언하고 크기를 알기 위한 maxSize도 선언 한다.

```java
private T[] elements;
private int rear;
private int front;
private int maxSize;
```

<br>

### 생성자

원형 큐의 생성자는 크기를 인자로 받아온다.

그리고 안에서 크기가 size + 1인 **배열을 하나 생성**해 주는데 이 배열의 크기가 size가 아니라 size + 1인 이유는 isEmpty와 isFull의 구분을 위해서 dummy space 한 칸이 있기 때문에 실 공간은 한 칸이 적다. 그렇기 때문에 헷갈리지 않도록 size에 +1을 해 주어 만약 8의 사이즈를 원한다면 총 8공간을 활용할 수 있게 되는 것이다. 

```java
public MyCircularQueue(int size) {
    this.elements = (T[]) new Object[size + 1];
    this.front = 0;
    this.rear = 0;
    this.maxSize = size + 1;
}
```

<br>

### isEmpty()

isEmpty는 큐가 비어있는 지를 확인하는 메소드로 rear의 값과 front의 값이 같을 때이다.

```java
@Override
public boolean isEmpty() {
    return this.front == this.rear;
}
```

<br>

### isFull

꽉 차있는 원형 큐는 front + 1 = rear의 식을 갖는다. 

여기서 명심해야 할 사항은 원형 큐에서 index는 큐의 크기를 넘어서도 되기 때문에 rear + 1을 한 값에 크기 만큼 modulo 연산을 해 주어야 원형 큐의 크기를 넘어선 index에 대하여 처리를 할 수 있다.

```java
@Override
public boolean isFull() {
    return (this.rear + 1) % this.maxSize == this.front;
}
```

<br>

### offer() - enqueue

offer는 큐에 데이터를 넣는 작업(enqueue) 이다.

1. 가장 먼저 큐가 꽉 차 있는 지를 확인해야 한다. 그리고 만약 꽉 차있다면 예외처리를 해 준다.

2. enqueue를 할 때는 rear의 index를 1 증가시킨다(enqueue의 정의). 물론 이때도 원형 큐의 크기 보다 더 큰 index 처리하기 위해서 modulo 연산도 진행해 준다.

3. 그 후에 rear의 위치에 데이터를 넣어주면 된다.

```java
@Override
public void offer(T data) {
    if (this.isFull()) {   //예외 처리
        throw new IllegalStateException(); 
    }
    
    this.rear = (this.rear + 1) % this.maxSize;  //modulo 연산
    this.elements[this.rear] = data;
}
```



<br>

### poll()

poll() 메소드는 큐에서 데이터를 추출하는 dequeue 과정으로 원형 큐에서는 front의 index를 증가 시킴으로써 구현한다(dequeue의 정의).

```java
@Override
public T poll() {
    if(this.isEmpty()) { //예외 처리
        throw new IllegalStateException();
    }
    
    this.front = (this.front + 1) % this.maxSize;
    return this.elements[this.front];
}
```

1. 만약 큐가 비어있다면 예외처리를 해 준다.

2. dequeue를 할 때는 front의 index를 1만큼 증가 시키고 modulo 연산까지 한다.

3. 그 후에 front 위치의 데이터를 삭제할 것이기 때문에 배열에서 이 위치의 데이터 값(value)을 반환 한다.

**여기서 한 가지 의문이 들 수도 있습니다.(앞서 들었던 의문)**

- 그냥 front를 1 증가 시키기만 하고 해당하는 index의 값에 대해서 null 처리를 해주지 않았는데 그럼 삭제를 한 것이 아니지 않은가?

맞다.

실제로는 값을 삭제하는 것이 아니다.  하지만 front를 통해 인덱스를 조절하는 것이 dequeue 과정인 이유는 rear와 front의 위치 만으로 해당 index의 값을 넣을 수 있는 지 없는 지를 판별할 뿐이고 만약 front가 지나온 위치에 rear가 위치하여 해당 index에 값이 존재함에도 불구하고 그 위치에 값을 추가 하고자 한다면 그저 값을 덮어씌우면 되기 때문에 큰 문제가 발생하지 않는 것이다.

<br>



### peek()

peek() 메소드는 poll()과 다르게 rear와 front 위치를 바꾸지 않고(즉, 구조를 바꾸지 않고) 맨 위의 데이터만 반환하는 동작이다. 

```java 
@Override
public T peek() {
    if (this.isEmpty()) {
        throw new IllegalStateeException();
    }
    return this.elements[this.front + 1];
} 
```

1. 비어있는 경우 예외처리를 먼저 해 준다.

2. poll()에서는 front위치의 데이터 값을 반환했다면 peek()에서는 front + 1의 값을 반환하는 이유는 front를 한 칸 옮기지 않고도 맨 위의 값을 알아와야 하는데 그러기 위해선 front 위치의 다음 인덱스가 가장 맨 위 값이기 때문이다.

- 기본적으로 front에는 아무 값도 없다고 생각하는 것이 원칙이다.



<br>

### size

size()는 기존의 크기를 알아내던 메소드와 다르게 원형 큐는 조금 독특하게 한다.

우선 front의 값이 rear의 값보다 작은 경우(가장 일반적인 경우)에는, 두 변수의 차이가 큐의 크기가 된다는 사실은 편하게 받아들일 수 있을 것이다.

하지만 원형 큐의 특성상 front의 값이 rear 값보다 항상 작지는 않기 때문에(front가 rear보다 더 큰 경우도 있기 때문에) 큐의 전체 크기에서 이 둘의 차이를 빼 주면 됩니다.

- front가 rear보다 큰 경우는 음수이기 때문에 이를 양수로 바꾸기 위해서 큐의 전체 크기에서 이를 빼는 것이다.

```
maxSize - (front - rear)
```

```java
@Override 
public int size() {
    if (this.front <= this.rear) {
        return this.rear - this.front;
    }
    return this.maxSize - this.front + this.rear;
}
```

<br>

### clear

clear() 는 생성자에서 했던 과정을 그대로 해 주면 된다.

```java
@Override
public void clear() {
    this.front = 0;
    this.rear = 0;
}
```

앞서 말했던 것과 같이 실제 배열의 값을 모두 초기화 시키는 것이 아니라 rear와 front의 위치를 같은 값으로 만드는 것이 사실상 배열의 초기화를 의미하는 것과 같은 것이기 때문에 front와 rear를 0으로 초기해 준다.

- front와 rear의 위치가 같기만 하면 되기 때문에 꼭 0일 필요는 없지만 clear라는 의미에서 둘 다 0으로 초기화 해 주기로 하자.

<br>

---

<span style="color:red">원형 큐는 rear와 front의 위치를 기반으로 데이터가 어떤 구조로 들어가 있는 지를 따지는 개념이다.</span>



# 관련된 문제

큐와 관련된 문제로 백준 2164번 카드 2가 있다.

- [<u>백준 2164번 카드 2</u>](https://www.acmicpc.net/problem/2164)

<br>

이상 자료구조 queue 였습니다.



























