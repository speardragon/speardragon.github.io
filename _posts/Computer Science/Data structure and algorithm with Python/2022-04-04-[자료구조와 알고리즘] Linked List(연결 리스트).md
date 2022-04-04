---
layout: single
title: "[자료구조와 알고리즘] Linked List"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures', 'Linked List']
---



# Linked List ADTs

Linked List를 ADT로 만들어 볼 것이다.



## Singly linked lists

singly linked list는 두 연속적인 node들 사이의 one pointer만을 가진 리스트를 말한다. 단방향으로만 이동될 수 있다.

- 한 노드가 다른 노드를 가리킴
- 각 노드마다 데이터가 들어있음
- 포인터는 다음 노드를 가리킴

즉, 리스트의 첫 번째 node에서부터 마지막 node로 갈 수 있지만 last node에서 first node로 갈 수는 없는 것이다.

![image](https://user-images.githubusercontent.com/79521972/161469856-2a2ee571-ba87-4c8c-9e25-699c91a70f05.png)

<br>

- Singly linekd list ADT includes the following operations:
  - insert()
  - delete()
  - traverse()

우리가 Linked List를 ADT로 만들면 위와 같은 함수를 사용하여 데이터 관리를 할 수 있다.

<br>

**Array**

- read, write이 빠르다.(O(1))
  - index가 있기 때문에 random access가 가능하기 때문

- insert, delete가 느리다.
  - 삽입 위치를 비워두고 한 칸씩 밀어야 되기 때문에 
  - O(n)

<br>

**Linked List**

- read, write할 때에 head node부터 시작하여 원하는 노드를 찾아야 하기 때문에 느리다.
  - O(n)
- insert, delete가 빠르다.(찾는 과정 미포함)
  - O(1)

<br>

## Pointer structures

```python
>>> s = set()
```

우리는 일반적으로 s가 `set type`의 변수라고 말할 것이다. 즉, s는 set이다. 그러나 이는 엄격히 맞지 않는 말이다.

- 변수 s는 set에 대한 reference(a safe pointer)인 것이다.





## Node class

node의 단순한 형태은 next node와의 연결 하나만을 가지는 node이다.

아래의 그림을 살펴보자.



```python
class Node:
	def __init__(self, data=None):
        self.data = data
        self.next = None
```





![image](https://user-images.githubusercontent.com/79521972/161470940-7e5d3193-b2ce-4fa6-8c1b-77eb7ea3979d.png)



```python
class Node:
	def __init__(self, data=None): #data는 local variable
        self.data = data  # self.data는 instance variable
        self.next = None
        
a = Node('eggs')
```



<br>

## Singly linked list class

list는 node와는 구별되는 컨셉이다. 우리는 list를 이해하기 위해 매우 단순한 class를 만드는 것으로부터 시작할 것이다.



```python
class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0
        ...
    def insert(self, prev_node, data):
        node = Node(data)
        self.size += 1
        
        # insert as a non-head node
        if prev_node:
            node.next = prev_node.next
            prev_node.next = node
        # insert as the head node (empty or not)
    else:
        node.next = self.head
        self.head = node
        
    def traverse(self):
        current = self.head
        while current:
            yield current.data
            current = current.next
            
    def delete(self, prev_node):
        self.size -= 1
        
        #delete a non-head node
        if prev_node:
            prev_node.next = prev_node.next.next
            
        # delete the head node
    else:
        else.head = self.head.next
        
words = SinglyLinkedList() # words.head -> None
words.insert(None, "eggs")
words.insert(words.head, "ham")
for word in words.traverse():
    print(word)
```





linked list에 접근하기 위해서는 그 리스트의 head node가 제일 중요하다. 모든 접근은 head node로 부터 시작한다.



### Insert(prev_node, data)

```python
def insert(self, prev_node, data):
        node = Node(data)
        self.size += 1
        
        # insert as a non-head node
        if prev_node:
            node.next = prev_node.next
            prev_node.next = node
        # insert as the head node (empty or not)
    else:
        node.next = self.head
        self.head = node
        
       
words = SinglyLinkedList() # words.head -> None
words.insert(None, "eggs")
words.insert(words.head, "ham")
```



- 노드 instance 생성
- 크기를 1 증가
- 중간에 삽입하는 경우
  - prev_node의 next pointer가 가리키는 값(None)을 node의 next pointer가 가리키게 한다.
  -  prev_node의 next pointer가 생성한 node를 가리키게 한다.
- 리스트의 맨 앞에 삽입하는 경우(빈 리스트이거나 맨 앞에 삽입)
  - prev_node가 None인 경우
  - head 노드를 node의 next pointer가 가리키게 하고
  - node를 head

![image](https://user-images.githubusercontent.com/79521972/161471913-3e5c39a1-a7a5-4ea6-bd2d-c081d1952d6b.png)

![image](https://user-images.githubusercontent.com/79521972/161471948-875094c6-6b5e-488a-9f42-61a93049d85f.png)



<br>

### Traverse()





### Function returns: yield

return은 반환하면 해당 함수가 종료하기 때문에 종료되지 않게 하고 싶을 때 사용하는 키워드이다.

- **yield**는 요구에 의해 one by one으로 반환한다.
  - Generator functions
  - goot for time and memory

```def
```















