---
layout: single
title: "[Algorithm] CH9-3. 깊이 우선 탐색(DFS)(그래프 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Graph']
tag: ['Data structure', 'Algorithm', 'DFS', '깊이 우선 탐색']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

<br>

## DFS (Depth-First Search, 깊이 우선 탐색 )

전에 배웠던 트리 탐색 중 `Preorder`,  `Inorder`, `Postorder`가 DFS의 예이다.

<mark>Stack을 이용하여 구현</mark>

- 재귀 호출로 구현하는 방법도 있으나 재귀 호출 자체가 call stack이 계속 쌓이는 형태이기 때문에 스택을 이용해서 구현 할 것이다.

#### **깊이 우선 탐색(DFS)이란**

> 갈 수 있는 최대한 멀리까지 탐색해 보는 방법

루트 노드(혹은 임의의 다른 노드)에서부터 시작하여 다음 branch로 넘어가기 전에 해당 branch를 먼저 완전히 탐색해 나가는 방법

- 미로와 같은 것을 탐색한다고 할 때 한 방향으로만 갈 수 있을 때까지 계속 가다가 더 이상 갈 수 없게 되면 뒤로 돌아 가장 가꾸운 갈림길로 가서 그곳에서부터 다른 방향으로 다시 탐색을 하는 방법이라고 생각하면 편하다.
- 즉, 넓게(wide) 탐색하기 전에 깊이(deep) 탐색하는 것이다.
- <mark>사용하는 경우</mark>: **모든 노드를 방문** 하고 싶을 때 사용한다. 
  - 예를 들어, 지구상의 존재하는 모든 인관관계를 표현한 후 David와 Scarlet 사이에 존재하는 경로를 찾는 경우
  - DFS가 BFS에 비해서 간단한 편이다.
  - 단순히 탐색 속도 자체만 봤을 때는 DFS가 BFS에 비해서 느리다.
    - 모든 노드를 방문해야 하기 때문

<br>

#### DFS의 특징

DFS의 특징 몇 가지를 살펴 보도록 하겠습니다.

- 자기 자신을 호출하는 순환 알고리즘으로 구현할 수 있다.

- 트리 탐색에서 배웠던 전위 탐색(Pre-order) 과 함께 다른 트리 탐색 방법(In-order, Post-order)들도 모두 DFS의 한 종류이다.
- 이후 구현에서 다룰 것이지만 반드시 그래프를 탐색하면서 어떤 노드를 방문했었는 지의 여부를 검사해야한다. (구현에서 visited 변수)
  - 무한루프에 빠질 위험이 있기 때문
- 스택의 구조이기 때문에 후입선출(LIFO)의 원칙에 따라 구현된다.



<br>

#### DFS의 과정

<img src="https://user-images.githubusercontent.com/79521972/154639584-ea1af857-dbfb-4601-86cb-0a9b6bd7dd9b.png" alt="image" style="zoom:50%;" />

![image](https://user-images.githubusercontent.com/79521972/154644141-2a333942-5494-4be1-8523-72c3f11246a4.png)

1. 시작 노드 A를 방문한다.
   - 스택에 방문된 노드를 삽입(enqueue)한다.
   - 초기에는 스택 안에 시작 노드만이 저장된다.
   - A 노드를 방문(삽입)했으면 이 노드와 인접한 이웃 노드(B)들을 방문한다.
2. 큐에서 꺼낸 노드와 인접한 노드들을 순차대로 방문한다.
   - 스택에서 꺼낸 노드를 방문한다.
   - 이 노드와 인접하는 노드들을 **모두** 방문(enqueue)한다.
     - 인접한 노드가 없으면 큐의 맨 앞(peek)에서 노드를 꺼낸다(dequeue).
   - 스택에 방문된 노드를 삽입(enqueue)한다.
3. 스택이 소진될 때까지 반복한다.

<br>

DFS는 인접한 노드들을 바로 다 탐색하는 것이 아니라 인접한 노드 중에서 더 깊이 들어갈 수 있는 노드가 있으면 그 분기(branch)를 먼저 다 탐색해야 그 전의 노드들을 탐색할 수 있다.

- 그렇기 때문에 스택의 특성과 매우 잘 맞는 것이다.
  - 후입선출: 나중에 들어온 것 중에서 깊이를 탐색하여 이를 높은 우선순위로 탐색 진행

스택에서 데이터가 빠짐(dequeue)과 동시에 해당 데이터는 초록색이 되고 이와 인접한 노드는 노란색이 되면서 방문(enqueue)을 한 상태(스택에 진입한 상태)가 되고 큐에서 순서대로 데이터를 빼면서(초록색으로 만들면서,or visited에 들어가면서) 인접 노드들이 큐에 삽입되는(노란색이 되는, 스택에 진입) 과정을 반복하는 것이다. (모든 노드가 초록색이 될 때까지, 스택이 빌 때까지) 

>  방문순서 : A-B-E-D-H-G-C-F

<br>



### BFS 탐색 구현(Java)

이전에 시간 구현을 했던 스택 구조를 가져와서 DFS를 구현해 보도록 하겠습니다.

```java
public static List<Integer> dfs(IGraph graph, int from) {
    List<Integer> result = new ArrayList<>();
    Set<Integer> visited = new HashSet<>();
    
    //dfs를 위한 Mystack
    IStack<Integer> stack = new MyStack<>();
    
    //자바의 스택 사용을 원하면
    //Stack<Integer> stack = new Stack<>();
    
    stack.push(from);
    visited.add(from);
    
    while (stack.size > 0) {
        Integer next = stack.pop();
        result.add(next);
        
        for (Integer n : graph.getNodes(next)) {
            if (!visited.contains(n)) {
                stack.push(n);
                visited.add(n);
            }
        }
    }
    return result;
}
```

Input parameter: 지난 시간 구현한 그래프(iGraph), 시작 노드(from)

Return: 방문한 노드를 순서대로 저장한 리스트

1. 처음에 시작할 때는 시작 노드(from)부터 시작한다.
   - stack에 시작노드를 삽입하고 방문하였기 때문에 visited에도 넣어준다.
2. 스택이 비어있을 때까지 다음을 반복한다.
   - next변수에 스택의 데이터를 하나씩 빼온다.(pop연산)
   - 해당 데이터를 result 리스트에 넣어주고 
   - next와 인접한 노드들이 0개, 1개 혹은 여러개 일 것이기 때문에 for문으로 이를 하나씩 순회하여 만약 방문하지 않았다면 (visited에 존재하지 않는다면) stack에 삽입하고 visited에도 삽입한다.
     - 여러 노드들은 getNodes()메소드를 통해 가져온다.
3. 스택이 모두 비었다면 어떤 순서로 탐색을 진행했는지의 결과인 result를 반환하여 마무리한다.



| 너비 우선 탐색 과정                                          | 깊이 우선 탐색 과정                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![Animated_BFS](https://user-images.githubusercontent.com/79521972/154641634-65bd2679-21ed-43d2-b8cd-6d828873c440.gif) | ![220px-Depth-First-Search](https://user-images.githubusercontent.com/79521972/154642490-211ab9c7-b333-4107-92a7-a0cc2c806a8e.gif) |
| [Wikipedia](https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) | [Wikipedia](https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) |