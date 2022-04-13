---
layout: single
title: "[자료구조와 알고리즘] Stack and Queues"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures', 'Stack', 'Queue']
---



# Stacks and Queues



## Stack ADT



**Stacks**

![image](https://user-images.githubusercontent.com/79521972/162659075-854b0c3b-305d-4e02-bd65-8f7b52a14f73.png)

stack은 부엌에서 접시를 쌓는 것과 유사한 data를 저장하는 자료구조이다.

스택의 제일 윗 부분(top)에 접시를 둘 수 있고, 접시가 필요할 때 이 스택의 제일 윗 부분(top)에서 가져올 수 있다. 스텍에 추가되는 마지막 접시는 그 스택으로부터 제일 먼저 집어 들어지는 것이다.

이와 유사하게 stack data structure는 우리로 하여금 one end로부터 data를 읽고 저장할 수 있도록 하고 마지막에 추가되는 element가 첫 번째로 집어진다.

따라서, stack은 `last in, first out`(즉, LIFO) 구조라고 한다.



## Basic operations

stack구조에서 INSERT(삽입) operation은 Push라고 불리고  DELETE operation은 element 인자를 받지않는 것으로 POP이라고 불린다.

![image](https://user-images.githubusercontent.com/79521972/162659513-3ef7ce6f-8d38-47c6-a7d1-2c4375fd10e1.png)



<br>

## Stack implementation using Linked list

![image](https://user-images.githubusercontent.com/79521972/162659696-efbceca6-54c4-4903-a5c1-88fb4ca9558a.png)

```python
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None
        
        
class Stack:
    def __init__(self):
        self.top = None
        self.size = 0
 
```

Stack을 사용하기 위해서는 Stack object(instacne)를 만들고 해당 object로 stack class의 변수 및 함수에 접근한다.



<br>

### Push operation

![image](https://user-images.githubusercontent.com/79521972/162659893-f54aa9e5-8d14-4ef2-9780-3f0388721bd4.png)

self가 의미하는 것은 현재 method가 갖고 있는 object를 의미한다.

```python
def push(self, data):
        node = Node(data)
        
        if self.top: #스택이 비어있지 않다면
            node.next = self.top # 1
        self.top = node # 2
        sefl.size += 1
```

- 가장 먼저 저장할 data를 가지로 있는 Node object를 하나 만든다.
- 이제 이 node를 stack 구조에 넣어야 할 차례이다.
- `if self.top`이 의미하는 것은 스택이 현재 비어 있지 않는지를 물어보는 것이다.
  - 만약 비어있지 않다면 top을 새로 생성한 node의 next pointer가 가리키도록 한다.



<br>

### Pop operation

![image](https://user-images.githubusercontent.com/79521972/162660455-349f6814-e59b-4e44-a115-b1ccbd49bf6b.png)

```python
def pop(self):
	if self.size == 0:
        return None
    
    data = self.top.data # 1
    self.top = self.top.next # 2
    self.size -= 1 
    return data
```



<br>

### Peek operation

![image](https://user-images.githubusercontent.com/79521972/162660685-57fdb501-245f-4b10-963a-070c90609c45.png)

```python
def peek(self):
    if self.size == 0:
        return None
    return self.top.data
```

스택의 구조를 바꾸지 않고 top의 값은 read해 오기만 한다.



**전체 코드**

```python
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None
        
        
class Stack:
    def __init__(self):
        self.top = None
        self.size = 0
        
	def push(self, data):
        node = Node(data)
        
        if self.top: #스택이 비어있지 않다면
            node.next = self.top # 1
        self.top = node # 2
        sefl.size += 1
        
	def pop(self):
        if self.size == 0:
            return None

        data = self.top.data # 1
        self.top = self.top.next # 2
        self.size -= 1 
        return data   
    
    def peek(self):
        if self.size == 0:
            return None
        return self.top.data
```



<br>

## Bracket-matching application

이제 stack 구현을 어떻게 할 수 있는지를 보여주기 위한 예제를 살펴보자

**괄호 검사**

```python
def check_brackets(statement):
    stack = Stack()
    for ch in statement:
        if ch in ('{', "[", '('):
            stack.push(ch)
        if ch in ('}', ']', ')'):
            last = stack.pop()
            if last == '{' and ch == '}':
                continue
            elif last == '[' and ch == ']':
				continue
            elif lst == '(' and ch == ')':
                continue
            else:
                return False
    if stack.size > 0:
        return False
    else:
        return True
            

sl = ("{(foo)(bar)}[hello](((this)is)a)test",
      "{(foo)(bar)}[hello](((this)is)a test)",
      "{(foo)(bar)}[hello](((this)is)a)test((")

for s in sl:
    m = check_brackets(s)
    print("{}: {}".format(s, m))
```





### Stack implementation using Python list

```python
data = "nse*a*qys** **eu***ot*i***"

stack = []
for ch in data:
	if ch == '*':
        print(stack.pop(), end='')
    else:
        stack.append(ch)
```

python의 list와 관련된 method를 적절히 사용하여 stack구조를 충분히 구현해 낼 수 있다.



**관련된 문제**

[백준 9012 괄호](https://www.acmicpc.net/problem/9012)

- 한 test case를 진행 할 때마다 Stack()를 만들어주는 이유는 한 스택을 계속해서 사용하게 되면 스택이 비어있지 않은 경우가 발생할 수 있기 때문이다.

<br>

## Stack Implementation in C

### Array implementation

아래 그림에서 볼 수 있듯이, S[1...n] 배열에서 최대 n element의 stack을 구현할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/162662763-74c18a79-50f8-4d6b-bf3d-eda4e4e614f5.png)

- S 스택의 **배열 구현**.

- 스택 원소는 옅게 칠해진 위치에만 보인다.
- index는 1부터 시작

- (a) 
  - Stack S는 4개의 element를 가진다.
  - top element는 9
- (b)
  - Push(S, 17)와 Push(S, 3) 를 호출한 후의 Stack S 모습이다.
- (c) 
  - Pop(S)을 호출한 후 Stack S는 가장 최근에 pushed 된 요소 3을 반환하였다.
  - 비록 배열에 3이라는 요소가 여전히 보이겠지만, 얘는 더 이상 stack 안에 있는 것이 아니다.
  - top element는 17
- 즉, pop이라는 method는 배열 안의 값을 실제로 삭제할 필요없이 top의 위치만으로 기능을 구현할 수 있는 것이다.(top 이후의 index에는 값이 있더라도 없는 것처럼 생각하는 것이다.)

<br>

top = 0 일 때, stack은 어떠한 element도 포함하지 않고 비어있는 것이다.

query operation 인 STACK-EMPTY에 의해 stack이 비어 있는지 테스트해 볼 수 있다. 만약, 빈 stack을 pop하려고 시도하면 stack이 underflow 되었다고 하고 이는 보통 error로 다루어진다.

또한 만약 top이 n을 초과하였다면 stack은 overflows 되었다고 한다.

<br>

```c
#include <stdio.h>
#include <stdlib.h>

#define N 100
char stack[N + 1];
int top = 0;

int is_empty();
{
    return top == 0;
}

int is_full();
{
    return top = N;
}

void push(char item){
    if (is_full()) {
        printf("stack overflow\n");
        exit(1);
    }
    stack[++top] = item;
}

char pop() {
    if (is_empty()) {
        printf("stack underflow\n");
        exit(1);
    }
    return stack[top--];
}

int main()
{
    char *data = "nse*a*qys** **eu***ot*i***";
    
    for (; *data; data++) {
        if (*data == '*')
            printf("%c", pop());
        else
            push(*data);
    }
    printf("\n");
    return 0;
}
```



<br>

### Linked-list implementation

![image](https://user-images.githubusercontent.com/79521972/163073108-d8f65cf0-d06a-4a45-b915-20627196b13a.png)

#### push

![image](https://user-images.githubusercontent.com/79521972/163073142-30557ae0-ac14-4cd6-917d-3e4bd4d748c1.png)

#### pop

![image](https://user-images.githubusercontent.com/79521972/163073164-009e34fd-cb05-42bc-a589-d515101a3200.png)



```c
#include <stdio.h>
#include <stdlib.h>

typedef struct _Node {
    char data;
    struct _Node *next;
} Node;

Node *top = NULL; //stack top

int is_empty()
{
    return top == NULL;
}

void push(char item)
{
    Node *ins;
    
    ins = (Node *) malloc(sizeof(Node));
    ins -> data = item;
    ins -> next = NULL;
    
    if(!is_empty())
        ins -> next = top;
    top = ins;
}

char pop()
{
    Node *del;
    char item;
    
    if (is_empty()) {
        printf("stack underflow!\n");
        exit(1);
    }
    
    item = top->data;
    del = top;
    top = del->next;
    free(del);
    
    return item;
}
```



<br>

# Queue ADT

## Queues

queue는 다음과 같이 동작한다.

- queue에 참석한 첫 번째 사람은 대게 첫 번째로 음식을 받는다.
- 그리고 모든사람은 그들 각자가 queue에 어떻게 참가했는지의 순서에 따라 음식을 받을 것이다.
- FIFO가 queue의 개념을 가장 잘 설명한다.
  - FIFO: first in, first out(선입 선출)

![image](https://user-images.githubusercontent.com/79521972/163073865-4fa1d14d-d147-4e1d-bc53-74a7e5a83608.png)



<br>

## Basic operation

![image](https://user-images.githubusercontent.com/79521972/163073912-c211f0c6-a9ec-4cb4-a404-cc6ebda042b3.png)



<br>

## Queue implementation using Linked list

![image](https://user-images.githubusercontent.com/79521972/163073961-74ada31b-c247-482a-9f44-d87de8a6b2bb.png)

```python
class Node:
    def __init__(self, data=None):
        self.data = data
        self.next = None
        
        
class Queue:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0
```



<br>

### The enqueue operation

```python
def enqueue(self, data):
    node = Node(data)
    
    if self.size == 0: # 큐가 비어있는 경우
        self.head = node
        self.tail = node
    else:
        self.tail.next = node
        self.tail = node
    self.size += 1
```

![image](https://user-images.githubusercontent.com/79521972/163076245-98c0a647-ed28-42d4-a5fd-26fb1cf1172c.png)

초기에 

<br>

### The dequeue operation

```python
def dequeue(self):
    if self.size == 0:
        return None
    self.size -= 1
    
    data = self.head.data
    self.head = self.head.next
    
    return data
```

![image](https://user-images.githubusercontent.com/79521972/163076251-96c9332c-2aa5-4f64-8e7b-0c01798b8a81.png)

<br>

## Python Deque

```python
from collections import deque

deq = deque([1, 2, 3])

deq.append(4)
deq.appendleft(5)
print(deq)

print(deq.pop())
print(deq.popleft())
```

```
deque([5, 1, 2, 3, 4])
4
5
```



![image](https://user-images.githubusercontent.com/79521972/163074579-b8bca512-bd6d-4f60-ac53-fe023de6f8f1.png)

list의 경우 삽입과 삭제의 경우 리스트의 값들을 한 칸씩 밀어야 되기 때문에 O(n)의 시간 복잡도를 갖고

deque의 경우 linked list로 구현이 되어 있기 때문에 노드들 간의 연결만 바꾸어 주면 되므로 O(1)의 시간 복잡도를 갖는다.

<br>

## Queue implementation using Python deque

```python
from collections import deque

data = "ea*sy **q*ue***st*i*on****"

queue = deque()
for ch in data:
    if ch == '*':
        print(queue.popleft(), end='')
        
    else:
        queue.append(ch)
```

```
easy question
```

<br>

### 백준 문제

[백준 2164번 카드 2](https://www.acmicpc.net/problem/2164)

![image](https://user-images.githubusercontent.com/79521972/163074901-4ba2c014-af21-47fd-91f5-6d18ddd6d3e4.png)

```python
n = int(input())
q = deque()
```



<br>

## Queue implementation in C

### Array implementation

![image](https://user-images.githubusercontent.com/79521972/163074951-a95240be-2446-4cda-a183-9456d7028517.png)

<br>

queue안의 element들은 head, head+1, ..... tail - 1 과같은 주소에 위치하고 있고 그러한 구조에서 우리는 location 1이 location n을 바로 뒤따라 가고 있는 circular order 라는 것을 이해하면 된다.

head=tail 일 때, queue는 비어있다. 처음에 우리는 head = tail = 1로 설정한다. 만약 빈 queue로부터 element를 dequeue하려고 한다면, queue underflows가 발생한다.

head = tail + 1일 때, queue는 가득 차 있고 만약 element를 enqueue하려고 한다면 queue는 overflows가 발생한다.

<br>

우리는 array1의 size가 client가 queue에서 보기 원하는 element의 최대값보다 더 크도록 만든다.

```c
#include <stdio.h>
#include <stdlib.h>

#define N 10
int queue[N + 1];
int head, tail;

int is_empty()
{
    return (head == tail);
}

int is_full()
{
    //queue에서 한 공간만 남았을 때를 꽉찼다고 하는데 이는 배열이 실제로 꽉차게 되면 head와 tail이 같아져 빈 queue와 구분할 방법이 없기 때문이다.
    //modulo 대신에 tail == N인 경우에 1로 되도록 하는 ternary operator를 사용하였다.
    //head == tail + 1
    return (head == ((tail) == N) ? 1: (tail + 1)));

}

int q_size()
{
    int count = 0;
    int temp = head;
    
    while (temp != tail) {
        count++;
        temp = (temp == N) ? 1 : (temp + 1); //circular 구조를 고려
    }
    return count;
}

void enqueue(int item)
{
    if (is_full()) {
        printf("queue overflow\n");
        exit(1);
    }
    
    queue[tail] = item;
    tail = (tail == N) ? 1 : (tail + 1);
}

int dequeue()
{
    int item;
    
    if (is_empty()) {
        printf("queue underflow\n");
        exit(1);
    }
    
    item = queue[head];
    head = (head == N) ? 1 : (head + 1);
    return item;
}
```



<br>

### 백준 문제(요세푸스 문제)

[백준 1158번 요세푸스 문제](https://www.acmicpc.net/problem/1158)

입력: 첫째 줄에 N과 K가 빈 칸을 사이에 두고 순서대로 주어진다. 

```c
int main()
{
    int M, K, item, i, j;
    
    scanf("%d %d", &M, &K);
    head = tail = 1;
    for (i = 1; i<=M; i++)
        enqueue(i);
    
    printf("<");
    while (q_size() > 1)
    {
        ...
    }
}
```



















