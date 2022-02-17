---
layout: single
title: "[Algorithm] CH8. Heap(힙)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Heap']
tag: ['Data structure', 'Algorithm', 'Heap', '힙']
toc: false
toc_sticky: true
---

<br>

# 힙 - Heap

이번 시간에는 `Heap` 자료구조에 대해서 배워 보도록 하겠습니다.

> 시작하기 앞서

힙이라는 것은 두 가지의 의미가 있습니다.

1. 자료구조 힙(Heap)
2. Java의 메모리 영역

이 두 가지는 같은 용어를 사용하지만 엄연히 다른 의미를 가지기 때문에 구분하여 알아두셔야 합니다.

<br>

그렇다면 자료구조 힙에 대해 먼저 보도록 하겠습니다.

## 자료구조 힙

먼저 자료구조 힙은 완전 이진 트리의 속성을 가지고 있습니다.

자료구조 힙에 대해 한 문장을 요약하자면 다음과 같습니다.

`최솟값 또는 최댓값을 빠르게 찾아내기 위한 완전 이진 트리 형태로 만들어진 자료구조`

완전 이진 트리가 뭔지 모르겠으면 다음 링크를 통해 이해하고 오시면 좋습니다.
[`완전 이진 트리`](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/Algorithm-CH7.-Tree(%ED%8A%B8%EB%A6%AC)/)

먼저 힙에는 어떤 종류가 있는 지 보겠습니다.

- 최대 힙(Max Heap)
  - 최대 힙이란 부모 노드의 값은 **항상** 자식 노드보다 크거나 같을 때를 의미합니다.
  - 즉, `루트 노드 = 트리의 최댓값`이라고 볼 수 있겠네요.
- 최소 힙(Min Heap)
  - 최소 힙이란 부모 노드의 값은 **항상** 자식 노드보다 작거나 같을 때를 의미합니다.
  - 즉 , `루트 노드 = 트리의 최솟값`입니다.

따라서, 새로운 노드가 추가되더라도 힙 에서는 이러한 구조가 유지 되어야 하고 좌우 노드에 대한 크기는 신경 쓰지 않고 상하 관계의 크기만을 신경 쓴 구조라고 볼 수 있습니다.

<br>

아래 그림을 살펴 보면서 최대 힙과 최소 힙을 이해해 봅시다.

![image](https://user-images.githubusercontent.com/79521972/154383404-3fe2fdde-125e-4eb2-93e9-a3761a73a5e4.png)

왼쪽 그림은 최소 힙(min heap)으로 트리의 어떤 부모 노드도 자식 노드보다 큰 노드는 존재하지 않습니다. 따라서 루트 노드가 트리의 데이터 중에 최솟값을 갖게 됩니다.

오른쪽 그림은 최대 힙(max heap)으로 트리의 어떤 부모 노드도 자식 노드보다 작은 노드가 존재하지 않습니다. 따라서 루트 노드가 트리의 데이터 중에 최댓값을 갖게 됩니다.

따라서 이는 '부모 노드는 항상 자식 노드보다 우선순위가 높다.' 라는 것을 의미하며 이를 조금만 돌려서 생각해 보면 루트 노드는 항상 우선순위가 더 높은 노드라는 것을 말합니다. 이를 통해 최댓값과 최솟값을 빠르게 찾아낼 수 있다는 장점(시간 복잡도 : O(1))과 함께 삽입 삭제 연산시에도 부모노드가 자식노드보다 우선순위만 높으면 되므로 결국 트리의 깊이만큼만 비교를 하면 되기 때문에 O(logN)의 시간 복잡도를 가져 매우 빠르게 수행이 가능한 것입니다.

<br>

### 시간 복잡도 특징

힙 구조는 최대/ 최소를 기준으로 데이터를 빠르게 찾는 연산입니다.

- 그렇기 때문에 항상 일정한 시간이 소요 되는 O(1) 시간 복잡도를 갖습니다.

또한 삽입에 있어서는 O(logN) 의 시간 복잡도를 갖고
삭제 역시 O(logN)의 시간 복잡도를 갖습니다.

<br>

### 우선순위 큐 - Priority Queue

힙의 대표적인 응용 사례는 우선순위 큐가 있습니다.

- **일반 큐**
  - 우리가 앞에서 배운 일반적인 큐는 먼저 들어온 데이터가 먼저 나가는 **FIFO** 데이터 구조였습니다.

- **우선순위 큐**
  - 반면 우선순위 큐는 데이터가 들어온 순서에 <u>**상관없이**</u> 특정 기준에 따라 우선순위가 높은 데이터 순으로 처리를 합니다.(중요하거나 빨리 처리해야 하는 것을 기준으로)
  - 예를 들어, 작업 스케줄링과 같은 것이 있습니다.

<br>

#### 우선순위 큐 in Java

```java
package heap;

import java.util.PriorityQueue;

public class PriorityQueueTest {
    
    static class Task {
        int priority; //일의 중요도
        String title; //업무 내용
        
        public Task(int priority, String title) {
            this.priority = priority;
            this.title = title;
        }
    }
    
    public static void main(String[] args) {
        //이곳에서 pr
        PriorityQueue<Task> pq = new PriorityQueue<>((t1, t2) -> {
            return t1.priority - t2.priority; 
            // t1, t2를 바꿀 경우 역순 가능
        });
        
        pq.add(new Task(4, "키보드 청소하기"));
        pq.add(new Task(1, "알고리즘 문제 풀기"));
        pq.add(new Task(3, "취업 공고 찾아보기"));
        pq.add(new Task(2, "강의 듣기"));
        
        while (!pq.isEmpty()) {
            Task task = pq.poll();
            System.out.println("[중요도: " + task.priority + "]" + task.title);
        }
    }
}
```

t1의 priority 와 t2의 priority를 빼기 연산으로 어떤 것의 우선순위가 더 높은 지를 판단 하도록 합니다.

만약 이 과정을 그냥 리스트를 이용해 구현했으면 우선순위를 찾기 위해 n번의 순회를 거쳐야 하기 때문에 O(n)의 시간 복잡도를 갖습니다.

하지만 힙으로 구현하여 O(logN)의 시간 복잡도 안에 이 과정을 수행할 수 있게 되는 것입니다.

<br>

### 힙 정렬 - Heap Sort

우선순위 말고도 힙 정렬이라는 자료구조가 있습니다.

이는 힙 자료구조의 특성을 이용한 정렬 방법으로 아래 그림을 한 번 보겠습니다.

![image](https://user-images.githubusercontent.com/79521972/154385566-068bc670-2b9b-41c5-b3b0-c178583ffb64.png)

그림의 왼쪽 처럼 정렬 되지 않은 데이터가 들어올 때 이 데이터들로 힙 자료구조를 생성하게 됩니다. 가운데 그림을 보면 Min Heap으로 생성 됨을 보실 수 있습니다.

Root Node의 pop연산을 통해 데이터를 꺼내 오면 항상 최소 값 만을 가져올 수 있기 때문에 정렬된 상태의 데이터를 나열할 수 있게 됩니다.

만약 Min Heap이 아니라 Max Heap이라면 오름차순으로 정렬되는 것이 아니라 내림차순으로 정렬 되겠죠?

<br>

## Heap을 배열로 표현하기

힙은 중간에 빈 값이 존재하지 않는 완전 이진 트리이기 때문에 일차원 배열로 표현해도 문제 없습니다.

![image](https://user-images.githubusercontent.com/79521972/154385953-eeea984b-4a42-496f-bc17-9aae685f6f2d.png)

위 그림처럼 배열에 값을 채울 수 있고 0번 index부터 채울 수도 있지만 앞(Tree)에서 설명 드린 노드 탐색의 용이성 때문에 1번 index부터 루트 노드의 데이터를 채운 모습입니다.

![image](https://user-images.githubusercontent.com/79521972/154390799-5d305df1-4daa-4321-876a-1ac6da7f6618.png)

지난 시간 배웠던 성질을 복습해 보도록 하겠습니다.

[성질]

1. 왼쪽 자식 노드 인덱스 = 부모 노드 인덱스 * 2 + 1
2. 오른쪽 자식 노드 인덱스 = 부모 노드 인덱스 *2 + 2
3. 부모 노드 인덱스 = (자식 노드 인덱스 - 1) / 2





**Heapify**

힙을 만들거나 힙의 구조에서 특정 상황에 힙의 재구조화를 해야 할 때가 존재합니다. 이를 heapify 연산이라고 하며 굉장히 중요합니다. 이를 통해 데이터의 추가/ 삭제 후에도 힙의 속성을 유지하는 것을 도와주기 때문입니다.



예를 들어,

![image](https://user-images.githubusercontent.com/79521972/154387095-813aed40-973b-475a-80a4-b7f717ad4185.png)

위와 같은 힙 구조가 있다고 가정합시다.

- 데이터 1을 추가한다면 루트 노드가 1로 바뀌어야 Min Heap구조가 유지 되기 때문에 힙의 재구조화가 일어나야 합니다.
- 혹은 데이터 3을 제거한다면 루트 노드가 5로 바뀌어야 힙 구조가 유지되기 때문에 이 역시도 힙의 재구조화가 일어나야 합니다.

<br>

#### 힙에 데이터 삽입

그렇다면 heapify는 어떤 식으로 진행되는 지 설명 드리도록 하겠습니다.

- **Min heap에 데이터 '1' 삽입하기**

![image](https://user-images.githubusercontent.com/79521972/154387346-cd3ebd5d-74b6-4ce0-83b0-cb02d413870f.png)

위와 같이 1을 삽입하기 위해서는 

1. 완전 이진 트리의 형태를 유지해야 하기 때문에 우선 leaf노드에 삽입합니다.
2. 그 후 힙의 조건(Min Heap)을 만족하는 지 확인 합니다.
   - 만약 조건을 만족한다면 삽입을 바로 종료하게 됩니다.
3. 조건을 만족하지 않는다면 삽입된 노드(1)와 해당 노드의 부모 노드의 값을 서로 바꿉니다.

![image](https://user-images.githubusercontent.com/79521972/154387812-b00079f3-c431-4ad5-a4a7-bdc0f56f6e06.png)

여기서는 1과 7의 위치가 바뀐 것을 볼 수 있습니다. 바꾸기를 진행했다면 다시 2번으로 돌아가 조건을 만족하는 지를 확인하고 만족하지 않는다면 다시 3번 과정으로 가 바꾸기 과정을 진행하고 이 과정을 삽입이 종료(조건 만족)되기 전까지 계속해서 반복합니다.

![image](https://user-images.githubusercontent.com/79521972/154388031-201b46aa-99db-40cf-b4e0-82954ec4fa4d.png)

이번에는 힙의 조건을 만족하므로 삽입 연산이 종료됩니다.

<br>

#### 힙에서 데이터 삭제하기

다음은 삭제 연산에 대해서 보도록 하겠습니다.

힙은 최솟값이나 최댓값을 알아내기 편한 구조이기 때문에 루트 노드에서만 삭제 연산이 이루어 지는 경우만 다뤄보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/154388291-195d0e89-3586-4f04-bf56-b7cfef6ebd10.png)

- Max Heap에서 최댓값 삭제하기
  1. 마지막 노드를 루트 노드로 가져옵니다.
     - 그림에서는 가장 마지막 노드가 5이기 때문에 이것을 루트 노드로 가져옵니다.
     - 그러면 아래의 그림과 같이 됩니다.
     - ![image](https://user-images.githubusercontent.com/79521972/154390295-335440f5-ec78-40d1-b119-f8bfd29ff38b.png)
  2. 이 상황에서 힙의 조건을 만족하는 지를 확인합니다.
     - 조건을 만족한다면 삭제를 종료합니다.
  3. <u>자식 노드</u>와 위치를 바꿉니다.

위 그림에서는 루트 노드 5가 조건(Max Heap)을 만족하지 않기 때문에 자식 노드와 위치를 바꾸어 줍니다. 이 과정으로 다음과 같이 됩니다.

![image](https://user-images.githubusercontent.com/79521972/154390492-d9847d2f-4e89-4f4d-bc70-3f20f135cb94.png)

위 그림에서 다시 2번 조건으로 돌아가 힙의 조건을 만족하는 지를 봤을 때 Max Heap을 만족하는 구조이므로 삭제연산을 종료하게 됩니다.

맨 처음 그림과 비교하였을 때 힙의 구조를 망가트리지 않고 9만 잘 제거된 것을 볼 수 있습니다.



### 시간 복잡도

힙에서 삽입과 삭제는 루트 노드의 위치만 알면 되기 때문에 모두 O(logN)의 시간 복잡도를 갖습니다.



---

**[장점]**

1. 최악의 경우에도 O(NlogN)으로 유지가 된다.
2. 힙의 특성상 부분 정렬을 할 때 효과가 좋다.



**[단점]**

1. 일반적인 O(NlogN)정렬 알고리즘에 비해 성능은 약간 떨어진다.
2. 한 번 최대 힙을 만들면서 불안정 정렬 상태에서 최댓값만 가지고 정렬을 하기 때문에 안정 정렬이 아니다.



## 힙 구현 

**IHeap interface**  

```java
package heap;

public interface IHeap<T> {
    void insert(T val);
    
    boolean contains(T val);
    
    T pop();
    
    T peek();
    
    int size();
}
```

힙 구현에 있어 필요한 프로토타입을 선언합니다.

<br>

최대 힙과 최소 힙 중 한 가지만 구현하면 나머지 한 가지는 부등호만 반대로 해 주면 되기 때문에 여기서는 최대 힙만 구현 해 보도록 하겠습니다.

```java
package heap;

public class MaxHeap<T extends Comparable<T>> implements IHeap<T> {
    
    ...
}
```



### **멤버 변수**

```java
T[] data;
int size;
int maxSize;
```

<br>

### 생성자

```java
public MaxHeap(int maxSize) {
    this.maxSize = maxSize;
    this.data = (T[]) new Comparable[maxSize + 1];
    this.size = 0;
}
```

데이터를 배열의 1번 index 부터 삽입을 하기 때문에 크기를 +1만큼 해 줍니다.

<br>

### 부모/자식 노드 찾는 메소드

```java
private int parent(int pos) {
    return pos / 2;
}

private int leftChild(int pos) {
    return pos * 2;
}

private int rightChild(int pos) {
    return (2 * pos) + 1;
}
```

트리 구조를 배열로 나타냈을 때 index를 통해 노드의 위치를 찾을 수 있습니다.

<br>

### leaf노드 판별 메소드

```java
private boolean isLeaf(int pos) {
    return (pos > (size / 2) && pos <= size);
}
```



<br>

### insert(T val)

삽입은 leaf에 데이터를 삽입한 후 값의 위치를 찾아주는 연산이 진행됩니다.

```java
@Override
public void insert(T val) {
    this.data[++this.size] = val;
    
    int current = this.size;
    
    while (this.data[parent(current)] != null &&
          this.data[current].compareTo(this.data[parent(current)]) > 0 {
              Collections.swap(Arrays.asList(this.data), current, parent(current));
              current = parent(current);
          })
}
```

- 삽입할 val을 data배열에 넣어줍니다.
- current는 현재 데이터를 삽입하는 위치가 됩니다.
- 부모 노드가 null이 아니고 current의 값이 parent 값보다 크면 두 위치를 바꾸어 줍니다.
  - 자바 api Collections의 swap 메소드 사용
- 계속해서 바뀐 노드 위치가 힙의 조건을 만족하는 지 확인하기 위하여 current값에 parent값을 넣으면서 반복합니다.



<br>

### pop() / heapify(int idx)

```java
@Override
public T pop() {
    T top = this.data[1];//루트 노드 값
    
    this.data[1] = this.data[this.size--];
    heapify(1);
    
    return top;
}

private void heapify(int idx) {
    if(isLeaf(idx)) {
        return;
    }
    
    T current = this.data[idx];
    T left = this.data[leftChild(idx)];
    T right = this.data[rightChild(idx)];
    
    if (current.compareTo(left) < 0 || current.compareTo(right) < 0) {
        if (left.compareTo(right) > 0) {
            Collections.swap(Arrays.asList(this.data), idx, leftChild(idx));
            heapify(leftChild(idx);
        } else {
            Collections.swap(Arrays.asList(this.data), idx, rightChild(idx));
            heapify(rightChild(idx);
        }
    }
}
```

**pop()**

- 삭제 연산은 루트 노드의 값을 삭제하는 것이므로 <u>루트 노드</u>의 값을 가져옵니다.

- 이진 트리를 만족하기 위해 가장 마지막 값을 가져와야 하기 때문에 size의 index의 값을 루트 노드 위치에 가져옵니다. 

- 가져온 값이 힙의 조건을 만족하는 지 보기 위한 1번 index에 대해 메소드 heapfy를 진행합니다.
- 루트 노드인 top을 리턴합니다.

**heapify()**

- 리프 노드인 경우 종료합니다.
- current, left, right 값을 각각 가져옵니다.
- left 자식 노드와 right 자식 노드 중 어느 것이라도 current 값보다 크다면 
  - left와 right 중 더 큰 노드와 비교를 하여 더 큰 것과 교환을 진행합니다.
  - 바꾸기만 하고 끝이 나는 것이 아니라 바꾸고 나서도 힙의 조건을 만족하는 지 보기 위하여 재귀호출로 heapify를 계속 해서 진행합니다.

<br>

### contains(T val) 

```java
public boolean contains(T val) {
    for (int i = 1; i < this.size; i++) {
        if (val.equals(this.data[i])) {
            return true;
        }
    }
    return false;
}
```

힙의 BST와 다르게 값을 비교하면서 Child를 타고 내려가는 것이 아니기 때문에 for문을 통해 값비교를 하는 방식으로 진행합니다.

- 0번 index는 진행하지 않으므로 1부터 for문을 진행합니다.

data에 val이 존재하면 true를 리턴하고 그렇지 않으면 false를 리턴합니다.

<br>

### peek()

```java
@Override
public T peek() {
    if (this.size < 1)
        throw new RuntimeException();
    return this.data[1];
}
```

루트 노드의 값을 가져옵니다.

<br>

### size()

```java
@Override
public int size() {
    return this.size;
}
```

<br>



## 알고리즘 별 시간 복잡도

![image](https://user-images.githubusercontent.com/79521972/154391395-5a92031a-ef76-48d4-9a12-cf9e84be682a.png)



## 관련된 문제

우선 순위 큐를 이용한 알고리즘 문제

[백준 1655번 가운데를 말해요](https://www.acmicpc.net/problem/1655)



