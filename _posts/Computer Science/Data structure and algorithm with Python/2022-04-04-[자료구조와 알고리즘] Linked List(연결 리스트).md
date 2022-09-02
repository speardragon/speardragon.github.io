---
layout: single
title: "[자료구조와 알고리즘] Linked List"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures', 'Linked List']
---



# Linked List ADTs

Linked List를 ADT로 만들어 볼 것이다.

<br>

## Singly linked lists

<mark>중요한 컨셉</mark>

- head node란 리스트에서 가장 앞에 있는 노드를 말하고 그 노드를 가리키는 포인터로 head라는 변수가 존재하는 것이다.
  - head 변수가 가리키고 있는 노드가 head!

singly linked list는 두 연속적인 node들 사이의 하나의 pointer만을 가진 리스트를 말한다. 단방향으로만 이동될 수 있다.

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

### Array vs. Linked List

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

우리는 일반적으로 s가 `set type`의 변수라고 말할 것이다. 즉, s는 set이다. 그러나 이는 엄격히 따지면 맞지 않는 말이다. 

변수 s는 오히려 set에 대한 reference(a safe pointer)라고 말할 수 있다. set contructor(생성자)는 메모리 안 어딘가에 set을 하나 만들고나서 그 set이 만들어진 memory location을 return한다. 이것은 s에 저장되는 것이다. python은 우리로부터 이러한 complexity(복잡한 내부사정)를 숨긴다.

우리는 이 때문에 s는 set이고 모든 것이 잘 수행된다고 안전하게 가정할 수 있는 것이다.

<br>

pointer structure들에 대한 몇가지 이점들이 존재한다. 

- 우선적으로, 그 이점은 sequential storage space(순차적인 저장 공간)을 요구하지 않는다는 것에 있다.
- 두 번째로는 작은 데서 시작할 수 있고 당신이 구조체에 더 많은 node를 추가하면서 임의로 확장할 수 있다.

그러나 pointer가 가진 이러한 유연성은 cost로 다가온다. 우리는 주소를 저장하기 위한 추가적인 공간을 필요로하기 때문이다. 

예를 들어, 만약 integer 타입의 리스트를 가지고 있다면, 각 노드는 integer를 저장하면서 공간을 채울 것이다.

- 물론 next node에 대한 pointer를 저장하기 위한 추가적인 integer도 저장된다.

<br>

## Node class

node의 단순한 형태은 next node와의 연결 하나만을 가지는 node이다.

우리가 pointer에 대해 알고있듯이, string은 사실 노드에 저장되는 것이 아니라 오히려 실제 string에 대한 pointer가 있는 것이다.

다음 diagram에 두 개의 node를 가진 example 을 생각해 보자.

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

list는 node와는 구별되는 컨셉이다. 우리는 우리의 list를 이해하기 위해 매우 단순한 class를 만드는 것으로부터 시작할 것이다.

우리는 첫 번째 노드에서 첫 번째 노드에 대한 reference를 유지하는 constructor(생성자)부터 시작한다.(즉, 다음 코드에서 head)

```python
class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0
        
        
words = SinglyLinkedList() # words.head -> None
```

어떤 linked list에 접근하기 위해서는 그 리스트의 head node가 제일 중요하다. 모든 접근은 head node로 부터 시작한다.

<br>

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
  - node의 next pointer가 head(None)를 가리키게 하고
  - head가 node를 가리키게 한다.

![image](https://user-images.githubusercontent.com/79521972/161471913-3e5c39a1-a7a5-4ea6-bd2d-c081d1952d6b.png)

![image](https://user-images.githubusercontent.com/79521972/161471948-875094c6-6b5e-488a-9f42-61a93049d85f.png)



<br>

### Traverse()

```python
 def traverse(self):
        current = self.head
        while current:
            yield current.data
            current = current.next
            
            
words = SinglyLinkedList() # words.head -> None
words.insert(None, "eggs")
words.insert(words.head, "ham")
for word in words.traverse():
    print(word)
```

![image](https://user-images.githubusercontent.com/79521972/161563411-6f33c3dc-7dca-4be4-8a15-a55f13e3cec2.png)



<br>

### Function returns: yield

yield는 return은 반환하면 해당 함수가 종료하기 때문에 종료되지 않게 하고 싶을 때 사용하는 키워드이다.

- **yield**는 요구에 의해 one by one으로(하나씩) 반환한다.
  - Generator functions
  - good for time and memory

```python
>>> def simple_generator():
    	for n in range(4):
            yield n + 1

>>>
>>> for i in simple_generator():
    	print(i, end=' ')
1 2 3 4        
```



<br>

### Delete(prev_node)

```python
def delete(self, prev_node):
        self.size -= 1
        
        #delete a non-head node
        if prev_node:
            prev_node.next = prev_node.next.next
            
        # delete the head node
    	else:
        	self.head = self.head.next
        
        
words.delete(words.head)
```

![image](https://user-images.githubusercontent.com/79521972/161564028-758021ec-7931-41a6-aaf9-69f13aca8fec.png)

<br>

```python
def delete(self, prev_node):
        self.size -= 1
        
        #delete a non-head node
        if prev_node:
            prev_node.next = prev_node.next.next
            
        # delete the head node
    	else:
        	self.head = self.head.next
        
        
words.delete(None)
```

![image](https://user-images.githubusercontent.com/79521972/161927621-5c6e6365-0468-4b54-9cc0-9c6d6092d7ca.png)

<br>

### 전체 코드

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
        	self.head = self.head.next
        
words = SinglyLinkedList() # words.head -> None
words.insert(None, "eggs")
words.insert(words.head, "ham")
for word in words.traverse():
    print(word)
words.delete(words.head)
```



<br>

## Doubly linked lists

Doubly linked list는 양방향으로 traverse 될 수 있다. Doubly linked list에서 한 노드는 쉽게 그것의 이전 노드를 해당 노드의 track을 유지하는 변수를 가지지 않고도 필요에 의해 언제들지 참조할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/161928656-3666f92c-31f7-4bd1-9326-93d636610a5a.png)



```python
class Node(object):
    def __init__(self, data=None, next=None, prev=None):
        self.data = data
        self.next = next
        self.prev = prev
```



<br>

## Circular lists

맨 마지막 노드가 NULL을 가리키는 것이 아니라 맨 처음 노드를 가리킴.

- Circular singly linked list

![image](https://user-images.githubusercontent.com/79521972/161928942-6610dd5c-35ed-4de4-8c00-0ea95af5e308.png)



- Circular doubly linked list

![image](https://user-images.githubusercontent.com/79521972/161928968-fe595dfe-60a5-4f99-b1af-16f6f43b2150.png)





<br>

### insert(prev_node, data)

```python
class CircularList: 	#Circular Singly Linked List
    def __init__(self):
        self.head = None
		self.size = 0
        
    def insert(self, prev_node, data):
        node = Node(data)
        self.size += 1
        
        #list is not empty
        if prev_node:
            node.next = prev_node.next
            prev_node.next = node
        #list is empty
    	else:
            node.next = node # 자기 자신을 가리킴
            self.head = node
            
words = CircularList()
words.insert(None, "eggs")
words.insert(words.head, "ham")
words.insert(words.head, "spam")
```

<br>

**SLL vs. CSLL**

SLL: 새로운 노드를 head node 다음에 삽입하고 싶으면 prev_node로 None을 전달하면 된다. 

CSLL: 노드가 원형 순환구조로 이어져 있기 때문에 head 다음에 삽입하는 것이나 중간에 삽입하는 것이나 똑같지만 prev_node 인자로 None을 넘긴다는 것은 해당 list 가 비어있음을 의미한다.



<br>

### Traverse()

```python
def traverse(self):
    current = self.head
    while True:
        yield current.data
        if current.next == self.data:
            break
        else:
            current = current.next

           
words = CircularList()
words.insert(None, "eggs")
words.insert(words.head, "ham")
words.insert(words.head, "spam")
for word in words.traverse():
    print(word)
```



<br>

### Delete(prev_node)

```python
def delete(self, prev_node):
    self.size -= 1
    
    # delete a non-head node
    if prev_node.next != self.head
    	prev_node.next = prev_node.next.next
        
    # delete the head node
	else:
    	# multiple nodes in list
        if prev_node != self.head:
            self.head = self.head.next
            prev_node.next = self.head
        # only one node in list
    	else:
            self.head = None
        
        
words.delete(words.head)
for word in words.traverse():
    print(word)
```

head node를 delete하는 경우에도 리스트에 노드가 하나만 있는 경우와 여러개가 있는 경우로 나뉜다.

![image](https://user-images.githubusercontent.com/79521972/161936211-aa54b9ac-ebd9-4b79-87c5-8ddb0c0a079f.png)

![image](https://user-images.githubusercontent.com/79521972/161936294-e1e4337a-b7be-4fdb-8e71-78bf8941da70.png)

<br>

- 백준 1158번 요세푸스 문제
  - [백준 1158번 요세푸스 문제](https://www.acmicpc.net/problem/1158)

![image](https://user-images.githubusercontent.com/79521972/161932271-10fc618a-eeb0-4709-a8de-de8a1f0fc9a6.png)

n,k 인 경우에 노드를 하나씩 삭제해 가기 때문에 k-1만큼씩 prev_node를 지정한다.

```python
n, k = map(int, input().split())

jo = []
for i in range(1, n+1):
    jo.append(i)
    
print('<', end='')

index = 0

while len(jo) > 0:
    index += k - 1
    if len(jo) <= index:
        index %= len(jo)

    if len(jo) > 1:
        print(jo.pop(index), end=', ')
    else:
        print(jo.pop(index), end='')

print('>', end='') 
```

<br>

## Circular List using Python List

### Python List Methods

```python
fruits = ['orange', 'apple', 'banana']
print(len(fruits))
# 3

fruits.append('kiwi')
print(fruits)
# ['orange', 'apple', 'banana', 'kiwi']

fruits.insert(2, 'pear')
print(fruits)
# ['orange', 'apple', 'pear', 'banana', 'kiwi']

print(fruits.pop(1))
# 'apple'
print(fruits.pop())
# 'kiwi'
fruits.remove('banana')
print(fruits)
# ['orange', 'pear']
```

<br>

### Circular list: Increasing the index

```python
fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi']

index = 3
print(fruits[index])
# 'banana'

index += 2
print(fruits[index])
# IndexError: list index out of range
```

```python
index = 3
index += 2
if index >= len(fruits):
    index = index % len(fruits)
    
print(fruits[index])
# 'orange'
```



<br>

### Circular list: pop

```python
fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi']

index = 3
fruits.pop(index)
# 'banana'

print(fruit[index])
# 'kiwi'

print(fruits.pop(index))
# 'kiwi'

print(fruits[index])
# IndexError: list index out of range

fruits = ['orange', 'apple', 'pear', 'kiwi']
print(fruits.pop(index))
# 'kiwi'

if index == len(fruits):
    index = 0
    
print(fruits[index])
# 'orange'
```



<br>

### Example

```python
words = []
words.append('eggs')
words.append('spam')
words.append('ham')

index = 0
while len(words) > 0:
    index += 2
    if index >= len(words):
        index %= len(words)
            
    print(words.pop(index))
    if index == len(words): #can be skipped
            index = 0		#            
```

```
ham
eggs
spam
```



<br>

## 요세푸스 문제

```python
n, k = map(int, input().split())

jo = []
for i in range(1, n+1):
    jo.append(i)
    
print('<', end='')

index = 0

while len(jo) > 0:
    index += k - 1
    if len(jo) <= index:
        index %= len(jo)

    if len(jo) > 1:
        print(jo.pop(index), end=', ')
    else:
        print(jo.pop(index), end='')

print('>', end='') n, k = map(int, input().split())

jo = []
for i in range(1, n+1):
    jo.append(i)
    
print('<', end='')

index = 2

while len(jo) > 0:
    index += 2
    
    print(jo.pop())
```





















