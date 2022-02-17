---
layout: single
title: "[Algorithm] CH3. Stack(스택)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Stack']
tag: ['Data structure', 'Algorithm', 'Stack', '스택']
toc: true
toc_sticky: true
---

<br>

# 스택

**stack**

`stack`은 후입선출(Last-In-First-Out - LIFO)의 대표적인 선형 자료구조 중의 하나입니다.

후입선출이란 'Last-In-First-Out'이라는 뜻 그대로 '나중에 들어온 데이터가 가장 먼저 나간다.' 의 의미를 가지고 있습니다.

 그 예로 인터넷 브라우저에서 뒤로 가기 기능같은 경우 그 버튼을 눌렀을 때 가장 최근의 페이지가 나오는데 이 역시 가장 나중에 들어간 페이지가 가장 먼저 나오는 것이기 때문에 스택 구조라고 할 수 있습니다.

또한, Ctrl + z 와 같은 실행 취소 기능역시 제일 나중에 했던 동작을 취소하는 것이므로 스택 구조라고 볼 수 있습니다.

![image](https://user-images.githubusercontent.com/79521972/153334791-de3d66bd-db92-4985-b84a-cd539241eee2.png)

위 그림은 스택의 구조를 시각화한 자료입니다.

흔히 스택은 접시쌓기를 생각하면 이해하기 편하실 겁니다. 가장 마지막에 내려놓은 접시를 가장 먼저 빼야지 쌓아놓은 접시들이 무너지지 않겠죠?

이때, 데이터를 넣는 작업을 Push, 빼는 작업을 Pop이라고 합니다.



## 연산 메소드

- push(): 데이터를 가장 위에 추가하는 작업
- pop():가장 위의 데이터를 삭제하면서 그 데이터를 읽어오는 작업
- top(), peek(): 구조는 그대로 둔 채 가장 위의 데이터를 읽어오는 작업



<br>

---



# Stack 실습

**IStack.java**

```java
package stack;

public interface IStack<T> {
    void push(T data);
    T pop();
    T peak();
    int size();
}
```

IStack이라는 interface에 push(), pop(), peak(), size() 메소드들을 정의하였습니다.

<br>

**MyStack.java**

```java
package stack; 

public class MyStack<T> implements IStack<T> {
    
    ...
}
```

MyStack 클래스에서 IStack을 구현상속 해 보겠습니다.

<br>

## 노드 클래스

```java
private class Node {
    T data;
    Node next;
    
    Node(T data) { this.data = data; }
    
    Node(T data, Node next) {
        this.data = data;
        this.next = next;
    }
}
```

먼저 stack도 마찬가지로 노드를 기반으로 하여 구현할 것이기 때문에 지난 시간에 만들었던 Node 클래스를 가져와 줍니다.



## 멤버변수

스택의 크기 변수와 head 노드를 선언해 줍니다.

```java
private int size;
private Node head;
```



<br>



## 생성자

위에서 선언한 멤버변수의 값을 초기화 시켜주겠습니다.

head 노드를 null로 초기화 시키는 것도 가능하지만 그렇게 되면 코드가 나중에 복잡해 질 가능성이 있기 때문에 새로운 null 노드 객체를 생성시켜 대입하겠습니다. 

```java
public MyStack() {
    this.size = size();
 	this.head = new Node(null);
    // this.head = null; 코드가 복잡해 질 수 있다.
    
}
```



<br>

## push

새로 push할 노드를 생성하는데 이 노드의 next pointer가 가리키고 있는 노드는 head노드의 next pointer가 가리키고 있던 노드를 가리키게 됩니다.

그 후 head 노드의 next pointer는 새로운 노드를 가리키게 해 주고, 스택의 크기를 1 증가시키면서 끝나게 됩니다.

이로써 head next pointer는 항상 새로 들어온 노드를 가리키는 것을 알 수 있습니다.

```java
@Override
public void push(T data) {
    Node node = new Node(data, this.head.next);
    this.head.next = node;
    
    this.size++;
}
```



<br>

 

## pop

head노드의 next pointer가 가리키는 노드는 항상 스택 맨 위에 있는 노드이기 때문에 이를 curr 노드로 지정하고 head 노드의 next pointer를 curr 노드의 다음 노드와 연결합니다. 

그 이후 curr 노드의 next pointer 연결을 null로 초기화 시켜 끊어주고 스택의 size를 1 감소 시킵니다.

pop()연산은 스택의 데이터를 삭제하면서 그 삭제한 데이터를 가져오는 것이기 때문에 curr의 데이터를 반환해 줍니다.

단 스택이 비어있는 경우 가져올 데이터 혹은 노드가 없기 때문에 반환할 객체가 없으므로 null 객체를 리턴해 줍니다.

```java 
@Override public T pop() {
    if (this.isEmpty()) {
        return null;
    }
    
    Node curr = this.head.next;
	this.head.next = curr.next;
    curr.next = null;
    this.size--;
    return curr.data;
}
```



<br>

## peak

peak() 연산은 스택의 구조를 건들이지 않고 가장 꼭대기의 값을 가져오는 것이기 때문에 head 노드의 next pointer가 가리키고 있는 노드의 data만을 그대로 return 해 주면 됩니다.

이 역시도 스택이 비어있는 경우에는 리턴할 객체가 없기 때문에 null을 반환해 줍니다.

```java
@Override
public T peek() {
    if (this.isEmpty()) {
        return null;
    }
    return this.head.next.data;
}
```



<br>

## size

```java
@Override
public int size() {
    return this.size;
}
```

size()는 언제나 그랬듯이 간단하게 멤버변수 size를 리턴해 주면 됩니다.



---

이렇게 스택의 각 연산에 대해서 알아보았습니다. 이 역시도 노드를 통해 이루어진 자료구조이지만 일반적인 연결리스트와는 데이터의 삽입, 삭제 방식(코드 구현)이 더 간편하다고 볼 수도 있겠습니다.

<br>

# 관련된 문제

챕터 3에서 배운 스택과 관련된 풀어 볼 만한 문제는 백준 사이트의 9012번 괄호 문제가 있으니 풀어보면 좋을 것 같습니다.

[백준](https://www.acmicpc.net/problem/9012 "9012 괄호")



이상 스택 자료구조에 대한 내용이었습니다.







