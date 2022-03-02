---
layout: single
title: "[자료구조] Heap & Priority Queue(힙과 우선순위 큐)"
categories: ['Lecture', 'Data structures and algorithms with Java', 'Algorithm', 'Heap']
tag: ['Data structure', 'Algorithm', 'Heap', '힙', 'Priority Queue']
toc: false
toc_sticky: true
---

ㅣ이번 시간에는 Heap 자료구조와 우선순위 큐 (Priority Queue)에 대해서 알아보자.

<br>

## 힙 - Heap

`Heap` 자료구조에 대해서 배워 보도록 하자.

<br>

> 시작하기 앞서

Computer Science(CS) 분야에서 힙이라는 것은 두 가지의 의미를 가지고 있습니다.

1. 자료구조 힙(Heap)
2. Java의 메모리 영역

이 두 가지는 같은 용어를 사용하지만 엄연히 다른 의미를 갖고 있기 때문에 구분하여 알아두어야 한다.

<br>

그렇지만 자료구조 및 알고리즘 수업이기 때문에 여기서는 자료구조 힙에 대해 먼저 보도록 하겠다.

<br>

### 자료구조 힙

힙(heap)은 이진 힙(binary heap)이라고도 하고, 최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전 이진 트리를 기본으로 한 자료구조이다.

<br>

 자료구조 힙은 다음과 같은 속성을 가지고 있다.

1. 완전 이진 트리 구조임
2. 부모 노드의 키값과 자식 노드의 키값 사이에는 대소 관계가 성립함
   - 키값 대소 관계는 오로지 부모 자식 간에만 성립되며 형제 노드 사이에는 대소 관계가 성립되지 않는 다는 것임.

<br>

자료구조 힙에 대해 한 문장을 요약하자면 다음과 같다.

> <span style="color:red">최솟값 또는 최댓값을 빠르게 찾아내기 위한 완전 이진 트리 형태로 만들어진 자료구조</span>

**완전 이진 트리**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조

<br>

먼저 힙에는 두 가지 종류가 있는 지 보겠다.

- **최대 힙(Max Heap)**
  - 부모 키값이 자식 노드 키값보다 큰 힙
  - `최대 힙이란 부모 노드의 값은 항상 자식 노드보다 크거나 같을 때`를 의미한다.
  - 즉, `루트 노드 = 트리의 최댓값` 일때를 말한다.
  
- **최소 힙(Min Heap)**
  - 부모 키값이 자식 노드 키값보다 작은 힙
  - `최소 힙이란 부모 노드의 값은 항상 자식 노드보다 작거나 같을 때`를 의미합니다.
  - 즉 , `루트 노드 = 트리의 최솟값`입니다.


따라서, 새로운 노드가 추가되더라도 힙에서는 이러한 구조가 항상 유지 되어야 하고 <span style="color:purple">좌우 노드에 대한 크기는 신경 쓰지 않고 상하 관계의 크기만을 신경 쓴 구조</span>라고 볼 수 있다.

<br>

---

아래 그림을 살펴 보면서 최대 힙과 최소 힙을 이해해 보자.

![image](https://user-images.githubusercontent.com/79521972/154383404-3fe2fdde-125e-4eb2-93e9-a3761a73a5e4.png)

왼쪽 그림은 최소 힙(min heap)으로 트리의 어떤 부모 노드도 자식 노드보다 큰 노드는 존재하지 않는다. 따라서 <mark>루트 노드가 트리의 데이터 중에 최솟값을 갖게 된다.</mark>

오른쪽 그림은 최대 힙(max heap)으로 트리의 어떤 부모 노드도 자식 노드보다 작은 노드가 존재하지 않는다. 따라서 <mark>루트 노드가 트리의 데이터 중에 최댓값을 갖게 된다.</mark>

따라서 힙은 '부모 노드는 항상 자식 노드보다 우선순위가 높다' 는 것을 의미하는 구조이며 이를 조금만 돌려서 생각해 보면 루트 노드는 항상 우선순위가 더 높은 노드라는 것을 의미한다.

<br>

이를 통해, 최댓값과 최솟값을 빠르게 찾아낼 수 있다는 장점(시간 복잡도 : O(1))과 함께 삽입, 삭제 연산시에도 부모노드가 자식노드보다 **우선순위**만 높으면 되므로 결국 트리의 깊이만큼만 비교를 하면 되기 때문에 O(log<sub>2</sub>n)의 시간 복잡도를 가져 매우 빠르게 수행이 가능하다.

<br>

#### 시간 복잡도 특징

힙 구조는 **최대/ 최소를 기준**으로 데이터를 빠르게 찾는 연산이다.

- 그렇기 때문에 항상 일정한 시간이 소요 되는 O(1) 시간 복잡도를 갖는다.

<br>

또한 **삽입**에 있어서는 O(log<sub>2</sub>n) 의 시간 복잡도를 갖고, **삭제** 역시 O(log<sub>2</sub>n)의 시간 복잡도를 갖는다.

<br>



## Heap을 배열로 표현하기

힙은 중간에 빈 값이 존재하지 않는 완전 이진 트리이기 때문에 일차원 배열로 표현해도 문제 없습니다.

![image](https://user-images.githubusercontent.com/79521972/154385953-eeea984b-4a42-496f-bc17-9aae685f6f2d.png)

위 그림처럼 배열에 값을 채울 수 있고 0번 index부터 채울 수도 있지만 앞 Tree구조에서 설명 드린 노드 탐색의 용이성 때문에 1번 index부터 루트 노드의 데이터를 채운 모습입니다.

**Tree(트리) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조

<br>

지난 시간 배웠던 성질을 복습해 보도록 하겠다.

[**성질**]

| <mark>**루트 노드**</mark> | <mark>**1**</mark> |
| -------------------------- | ------------------ |
| 노드 i의 부모              | i/2                |
| 노드 i의 왼쪽 자식         | i*2                |
| 노드 i의 오른쪽 자식       | i*2 + 1            |

<br>

![image](https://user-images.githubusercontent.com/79521972/154390799-5d305df1-4daa-4321-876a-1ac6da7f6618.png)<br>



### **Heapify** - 데이터 상향(or 하향) 재정렬

최대 힙을 예로 설명하겠다.

- 일반 트리 구조에서 힙 구조를 만들거나 
- 힙의 구조에서 특정 상황에 따라 힙의 재구조화를 해야 할 때가 존재하는데, 이를 heapify 연산이라고 하며 이는 힙 연산에서 굉장히 중요하다. 이를 통해 데이터의 추가/삭제 후에도 힙의 속성을 유지하는 것을 도와주기 때문이다.

<br>

예를 들어,

![image](https://user-images.githubusercontent.com/79521972/154387095-813aed40-973b-475a-80a4-b7f717ad4185.png)

위와 같은 힙 구조가 있다고 가정해 보자.

- 위 구조에 데이터 1을 추가한다고 하면 루트 노드가 1로 바뀌어야 Min Heap구조가 유지 되기 때문에 힙의 재구조화가 일어나야 한다.
- 혹은 데이터 3을 제거한다고 하면 루트 노드가 5로 바뀌어야 힙 구조가 유지되기 때문에 이 역시도 힙의 재구조화가 일어나야 하는 상황이다.

<br>

max heap에서 heapify 작업 과정은 다음과 같다.

1. 요소(element) 값과 자식 노드 값을 비교한다.
2. 만약 자식 노드 값이 크면 왼쪽 오른쪽 자식 중 가장 큰 값으로 교환한다.
3. 힙 속성이 유지 될 때까지 위의 1, 2번 과정을 반복한다.

<br>

### 힙 구조화 - Build Heap

완전 이진 트리에서 heap 구조를 만드는 작업을 한다.

![image](https://user-images.githubusercontent.com/79521972/155867299-eeb20187-0137-4f15-bc10-18f481139df3.png)

배열에 표현된 힙은 루트 노드의 index가 1이라고 할 때, n/2 + 1(중간에서 다음 칸) 부터 n(마지막 노드) 까지는 모두 leaf 노드라는 속성을 갖고 있다.

leaf 노드를 제외한 나머지 노드(1 ~ n/2)에 heapify를 수행하면 heap 구조화를 진행할 수 있다.

<br>

- 시간 복잡도 O(n)

<br>

#### 힙에 데이터 삽입

그렇다면 heapify는 어떤 식으로 진행되는 지 설명 드리도록 하겠다.

- **Min heap에 데이터 '1' 삽입하기**

![image](https://user-images.githubusercontent.com/79521972/154387346-cd3ebd5d-74b6-4ce0-83b0-cb02d413870f.png)

위와 같이 1을 삽입하기 위해서는 

1. 완전 이진 트리의 형태를 유지해야 하기 때문에 우선 leaf노드에 삽입합니다.
2. 그 후 힙의 조건(Min Heap)을 만족하는 지 확인한다.
   - 만약 조건을 만족한다면 삽입을 바로 종료하게 된다.
3. 조건을 만족하지 않는다면 삽입된 노드(1)와 해당 노드의 부모 노드의 값을 서로 바꾼다.

![image](https://user-images.githubusercontent.com/79521972/154387812-b00079f3-c431-4ad5-a4a7-bdc0f56f6e06.png)

여기서는 1과 7의 위치가 바뀐 것을 볼 수 있다. 바꾸기를 진행했다면 다시 2번으로 돌아가 조건을 만족하는 지를 확인하고 만족하지 않는다면 다시 3번 과정으로 가 바꾸기 과정을 진행하고 이 과정을 삽입이 종료(조건 만족)되기 전까지 계속해서 반복한다.

![image](https://user-images.githubusercontent.com/79521972/154388031-201b46aa-99db-40cf-b4e0-82954ec4fa4d.png)

이번에는 힙의 조건을 만족하므로 삽입 연산이 종료된다.

<br>

#### 힙에서 데이터 삭제하기

다음은 삭제 연산에 대해서 보도록 하겠다.

힙은 최솟값이나 최댓값을 알아내기 편한 구조이기 때문에 루트 노드에서만 삭제 연산이 이루어 지는 경우만 다뤄보도록 하겠다.

![image](https://user-images.githubusercontent.com/79521972/154388291-195d0e89-3586-4f04-bf56-b7cfef6ebd10.png)

- Max Heap에서 최댓값 삭제하기
  1. 마지막 노드(가장 아래 가장 오른쪽)를 루트 노드로 가져온다.
     - 그림에서는 가장 마지막 노드가 5이기 때문에 이것을 루트 노드로 가져온다.
     - 그러면 아래의 그림과 같이 된다.
     - ![image](https://user-images.githubusercontent.com/79521972/154390295-335440f5-ec78-40d1-b119-f8bfd29ff38b.png)
  2. 이 상황에서 힙의 조건을 만족하는 지를 확인한다.
     - 조건을 만족한다면 삭제를 종료한다.
  3. 조건을 만족하지 않는다면 <u>자식 노드</u>와 위치를 바꾼다.

위 그림에서는 루트 노드 5가 조건(Max Heap)을 만족하지 않기 때문에 자식 노드와 위치를 바꾸어 준다. 

이 과정으로 다음그림과 같이 된다.

![image](https://user-images.githubusercontent.com/79521972/154390492-d9847d2f-4e89-4f4d-bc70-3f20f135cb94.png)

위 그림에서 다시 2번 조건으로 돌아가 힙의 조건을 만족하는 지를 봤을 때 Max Heap을 만족하는 구조이므로 삭제연산이 종료 된다.

맨 처음 그림과 비교하였을 때 힙의 구조를 망가트리지 않고 9만 잘 제거된 것을 볼 수 있다.

<br>

### 시간 복잡도

힙에서 삽입과 삭제는 루트 노드의 위치만 알면 되기 때문에 모두 O(log<sub>2</sub>n)의 시간 복잡도를 갖는다.

<br>



**[장점]**

1. 최악의 경우에도 O(nlog<sub>2</sub>n)으로 유지가 된다.
2. 힙의 특성상 **부분 정렬**을 할 때 효과가 좋다.



**[단점]**

1. 일반적인 O(nlog<sub>2</sub>n) 정렬 알고리즘에 비해 성능은 약간 떨어진다.
2. 한 번 최대 힙을 만들면서 불안정 정렬 상태에서 최댓값만 가지고 정렬을 하기 때문에 안정 정렬이 아니다.

<br>

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

힙 구현에 있어 필요한 프로토타입을 선언한다.

<br>

최대 힙과 최소 힙 중 한 가지만 구현하면 나머지 한 가지는 부등호만 반대로 해 주면 되기 때문에 여기서는 **최대 힙**만 구현할 것이다.

```java
package heap;

public class MaxHeap<T extends Comparable<T>> implements IHeap<T> {
    
    ...
}
```

<br>

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

데이터를 배열의 1번 index 부터 삽입을 하기 때문에 크기를 maxSize+1만큼 설정했다.

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

트리 구조를 배열로 나타냈을 때 index를 통해 특정 노드의 위치를 찾을 수 있다.

<br>

### leaf노드 판별 메소드

```java
private boolean isLeaf(int pos) {
    return (pos > (size / 2) && pos <= size);
}
```

리프 노드의 인덱스도 위에서 설명했듯이 인덱스를 통해 알아낼 수 있다.

- <span style="color:gray">배열에 표현된 힙은 루트 노드의 index가 1이라고 할 때, n/2 + 1(중간에서 다음 칸) 부터 n(마지막 노드) 까지는 모두 leaf 노드라는 속성을 갖고 있다.</span>

<br>

### insert(T val)

삽입은 leaf에 데이터를 삽입한 후 값의 위치를 찾아주는 연산이 진행된다.

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

- 삽입할 val을 data배열에 넣어준다.
- current는 현재 데이터를 삽입한 위치가 된다.
- 부모 노드가 null이 아니고 current 인덱스의 값이 current 노드의 parent 값보다 크면 두 위치를 바꾸어 준다.
  - 자바 api Collections의 swap 메소드 사용
- 계속해서 바뀐 노드 위치가 힙의 조건을 만족하는 지 확인하기 위하여 current 변수에 parent값을 넣으면서 반복한다.



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
```

**pop()**

- 삭제 연산은 루트 노드의 값을 삭제하는 것이므로 <u>루트 노드</u>의 값을 가져온다.(인덱스 1)
- 이진 트리를 만족하기 위해 가장 마지막 값을 가져와야 하기 때문에 size index에 해당하는 값을 루트 노드 위치에 가져온고 크기를 1 감소시킨다. 
- 가져온 값이 힙의 조건을 만족하는 지 보기 위해서 1번 index에 대해 메소드 heapfy를 진행한다.
- 루트 노드인 top을 리턴한다.

<br>

```java
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

**heapify()**

- 리프 노드인 경우 종료한다.
- current, left, right 값을 각각 가져온다.
- left 자식 노드와 right 자식 노드 중 어느 것이라도 current 값보다 크다면 
  - left와 right 중 더 큰 노드와 비교를 하여 더 큰 것과 교환을 진행한다.
  - 바꾸기만 하고 끝이 나는 것이 아니라 바꾸고 나서도 힙의 조건을 만족하는 지 보기 위하여 재귀호출로 heapify를 조건이 만족할 때까지 계속 진행한다.

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

힙(heap)은 BST(이진 탐색 트리)와 다르게, 값을 비교하면서 Child를 타고 내려가는 것이 아니기 때문에 for문을 통해 값 비교를 하는 방식이다.

- 0번 index는 진행하지 않으므로 1번 index부터 for문을 진행한다.

data에 val이 존재하면 true를 리턴하고 그렇지 않으면 false를 리턴한다.

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

가장 최상위 값인 루트 노드의 값을 가져온다.

<br>

### size()

```java
@Override
public int size() {
    return this.size;
}
```

사이즈를 리턴하는 메소드이다.

<br>



## 우선순위 큐 - Priority Queue

힙의 대표적인 응용 사례는 우선순위 큐가 있다.

우선순위 큐는 말 그대로 우선 순위가 높은 것을 먼저 꺼내기 위해 만들어진 구조이며 힙이라고도 불린다.

<br>

여기서 의문이 생길 수 있다.

> 그럼 배열로 각 데이터를 비교해서 새로 저장하면 되지 않나?

물론 가능하긴 하지만 효율적인 방법이 아니다. 왜냐하면 선형으로 처음부터 비교를 하면서 위치를 찾으면 시간복잡도 O(n)을 요구하고 n 이 충분히 크면 시간 초과가 발생할 것이기 때문이다.

<br>

그래서 이를 더 빠르게 구현하기 위해서 힙이 나온 것이다.

<br>

- **일반 큐**
  - 우리가 앞에서 배운 일반적인 큐는 먼저 들어온 데이터가 먼저 나가는 **FIFO** 데이터 구조였다.

- **우선순위 큐**
  - 반면 우선순위 큐는 데이터가 들어온 순서에 <u>**상관없이**</u> 특정 기준에 따라 우선순위가 높은 데이터 순으로 처리를 한다.(중요하거나 빨리 처리해야 하는 것을 기준으로)
  - 예를 들어, 작업 스케줄링과 같은 것이 있다.

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
//	출력 결과
// [중요도: 1] 알고리즘 문제 풀기
// [중요도: 2] 강의 듣기
// [중요도: 3] 취업 공고 찾아보기
// [중요도: 4] 키보드 청소하기
```

t1의 priority 와 t2의 priority를 빼기 연산으로 어떤 것의 우선순위가 더 높은 지를 판단 하는 연산을 추가한다.

이 연산을 통해 정해지는 우선순위를 기반으로 큐가 정렬이 되는 것이다.

만약 이 과정을 그냥 리스트를 이용해 구현했으면 우선순위를 찾기 위해 n번의 순회를 거쳐야 하기 때문에 O(n)의 시간 복잡도를 갖는다.

하지만 힙으로 구현하여 O(logN)의 시간 복잡도 안에 이 과정을 수행할 수 있게 되는 것이다.

<br>

### 힙 정렬 - Heap Sort

우선순위 말고도 힙 정렬이라는 자료구조도 있다.

과정은 다음과 같다.

1. n개의 요소를 하나씩 힙에 삽입한다.
2. n번에 걸쳐 힙에서 요소 하나씩을 삭제하고 반환된 값을 순차적으로 정렬한다.

<br>

이는 힙 자료구조의 특성을 이용한 정렬 방법으로 아래 그림을 한 번 보자.

![image](https://user-images.githubusercontent.com/79521972/154385566-068bc670-2b9b-41c5-b3b0-c178583ffb64.png)

그림의 왼쪽 처럼 정렬 되지 않은 데이터가 들어올 때 이 데이터들로 힙 자료구조를 생성하게 된다. 가운데 그림을 보면 Min Heap으로 생성됨을 볼 수있다.

루트 노드에 대해 pop연산을 통해 데이터를 추출해 오면 항상 최소 값 만을 가져올 수 있기 때문에 정렬된 상태의 데이터를 나열할 수 있게 된다.

만약 Min Heap이 아니라 Max Heap이라면 오름차순으로 정렬되는 것이 아니라 내림차순으로 정렬 된다.

<br>

또한 Dijkstra 알고리즘이라는 곳에도 쓰인다.

**Dijkstra 알고리즘**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/dijkstra/Algorithm-Dijkstra-Algorithm(%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)/)을 참조

<br>

### 정렬 간 시간복잡도 비교

| 정렬 방식              | Average                  | Worst                    | Memory   | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| ---------------------- | ------------------------ | ------------------------ | -------- | ----------- | ------------- | --------------------------------- |
| Bubble 정렬            | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | O           | O             | 7.438                             |
| Selection 정렬         | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | X           | O             | 10.842                            |
| Insertion 정렬         | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | O           | O             | 22.894                            |
| Shell 정렬             | O(nlog<sub>2</sub>n)     | O(n<sup>2</sup>)         | O(1)     | X           | O             | 0.056                             |
| Merge 정렬             | O(nlog<sub>2</sub>n)     | O(nlog<sub>2</sub>n)     | O(n)     | O           | X             | 0.014                             |
| Quick 정렬             | O(nlog<sub>2</sub>n)     | O(n<sup>2</sup>)         | O(1)     | X           | O             | 0.034                             |
| <mark>Heap 정렬</mark> | **O(nlog<sub>2</sub>n)** | **O(nlog<sub>2</sub>n)** | **O(1)** | **X**       | **O**         | **0.026**                         |

<br>



## 관련된 문제

우선 순위 큐를 이용한 알고리즘 문제

[백준 1655번 가운데를 말해요](https://www.acmicpc.net/problem/1655)

<br>

**Bubble 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Bubble-Sort(%EB%B2%84%EB%B8%94-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Insertion 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Quick 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Quick-Sort(%ED%80%B5-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>
