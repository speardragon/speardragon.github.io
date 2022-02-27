---
layout: single
title: "[Algorithm] 깊이 우선 탐색(DFS)(그래프 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Graph']
tag: ['Data structure', 'Algorithm', 'DFS', '깊이 우선 탐색']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

이번 시간에는 그래프 탐색 방법 중 깊이 우선 탐색(DFS)에 대해서 배워볼 것이다.

<br>

## 그래프 탐색이란

- 하나의 정점으로부터 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것
- EX) 특정 도시에서 다른 도시로 갈 수 있는 지 없는 지, 전자 회로에서 특정 단자와 단자가 서로 연결 되어 있는 지

그래프는 탐색하는 동안 동일한 정점으로 다시 이동할 수 있는 싸이클이 있을 수 있다. 그래서 동일한 정점이 다시 처리되지 않도록 하려면 처리 후 정점을 방문(visited)했다는 표시를 함으로써 중복 방문을 피하도록 하는 것이 그래프 탐색의 핵심이다.

<br>

## DFS (Depth-First Search, 깊이 우선 탐색 )

> DFS는 그래프의 개념이 반드시 선행되어야 하기 때문에 다음 포스트를 먼저 확인하길 바람.
>
> > **Tree(트리) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조
> >
> > **Graph(그래프) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)을 참조

<br>

전에 배웠던 트리 탐색 중 `Preorder`,  `Inorder`, `Postorder`가 바로 DFS의 예이다.

<mark>Stack을 이용하여 구현</mark>

- 재귀 호출로 구현하는 방법도 있으나 재귀 호출 자체가 call stack이 계속 쌓이는 형태이기 때문에 스택을 이용해서 구현 할 것이다.
- 물론 재귀 호출을 이용한 DFS도 이후 내용에 있다.

<br>

BFS와의 가장 큰 차이점이라 하면, DFS는 탐색을 한 뒤 이전의 정점으로 돌아오는데 이것을 백트래킹(Backtracking)이라고 한다.

- 백트래킹 또한 알고리즘 중에 하나이며 이에 대한 포스팅도 추후에 올릴 것이다.

<br>

#### **깊이 우선 탐색(DFS)이란**

> 갈 수 있는 최대한 멀리까지 탐색해 보는 방법

루트 노드(혹은 임의의 다른 노드)에서부터 시작하여 다음 branch로 넘어가기 전에 해당 branch를 먼저 완전히 탐색해 나가는 방법

- 미로와 같은 것을 탐색한다고 할 때 한 방향으로만 갈 수 있을 때까지 계속 가다가 더 이상 갈 수 없게 되면 뒤로 돌아 가장 가꾸운 갈림길로 가서 그곳에서부터 다른 방향으로 다시 탐색을 하는 방법이라고 생각하면 편하다.
- 즉, 넓게(wide) 탐색하기 전에 깊이(deep) 탐색하는 것이다.
- <mark>사용하는 경우</mark>: **모든 노드를 방문** 하고 싶을 때 사용한다. 
  - 예를 들어, 지구상의 존재하는 모든 인관관계를 표현한 후 철수와 영희 사이에 존재하는 경로를 찾는 경우
  - DFS가 BFS에 비해서 간단한 편이다.
  - 단순히 탐색 속도 자체만 봤을 때는 DFS가 BFS에 비해서 느리다.
    - 모든 노드를 방문해야 하기 때문

<br>

#### DFS의 특징

DFS의 특징 몇 가지를 살펴 보도록 하겠다.

- 자기 자신을 호출하는 **순환 알고리즘**으로 구현할 수 있다.

- 트리 탐색에서 배웠던 전위 탐색(Pre-order) 과 함께 다른 트리 탐색 방법(In-order, Post-order)들도 모두 DFS의 한 종류이다.
- 이후 구현에서 다룰 것이지만 **반드시** 그래프를 탐색하면서 **어떤 노드를 방문했었는 지의 여부를 검사**해야한다. (구현에서 visited 변수)
  - 무한루프에 빠질 위험이 있기 때문
- 스택의 구조이기 때문에 **후입선출(LIFO)**의 원칙에 따라 구현된다.

나중에 들어오는 데이터를 먼저 탐색해 나가는 방식을 따르면 인접한 데이터를 먼저 순회하는 것이 아닌 더 깊이 들어가는 방향으로 순회를 하는 것이기 때문에 스택 구조를 사용하는 것이다.

<br>

#### DFS의 과정

<img src="https://user-images.githubusercontent.com/79521972/154639584-ea1af857-dbfb-4601-86cb-0a9b6bd7dd9b.png" alt="image" style="zoom:50%;" />

![image](https://user-images.githubusercontent.com/79521972/154644141-2a333942-5494-4be1-8523-72c3f11246a4.png)

1. **시작 노드 A를 방문한다.**(하나의 정점에서 시작한다.)
   - 시작 정점을 스택에 push한다.
   - 스택에 맨 위에 있는 정점을 pop하고 방문 표시(visited, 초록색 표시)를 한다.

<br>

2. **큐에서 꺼낸 노드와 인접한 노드들을 순차대로 방문한다.**(간선을 따라 다음 정점으로 방문한다.)

- pop된 정점에서 방문하지 않은 인접한 모든 정점들을 스택에 push한다.
- 방문하지 않은 노드들 중에서 인접한 노드가 없으면 큐의 맨 앞(peek)에서 노드를 pop한다.

<br>

3. **스택이 소진될 때까지 반복한다.**

<br>

DFS는 인접한 노드들을 바로 다 탐색하는 것이 아니라 인접한 노드 중에서 더 깊이 들어갈 수 있는 노드가 있으면 그 분기(branch)를 먼저 다 탐색해야 그 전의 노드들을 탐색할 수 있다.

- 그렇기 때문에 스택의 특성과 매우 잘 맞는 것이다.
  - 후입선출: 나중에 들어온 것 중에서 깊이를 탐색하여 이를 높은 우선순위로 탐색 진행

<br>

스택에서 데이터가 빠짐(pop)과 동시에 해당 데이터는 <span style="color:green">초록색</span>이 되고 이와 인접한 노드는 <span style="color:yellow">노란색</span>이 되면서 방문(push)을 한 상태(스택에 진입한 상태)가 되고 큐에서 순서대로 데이터를 빼면서(초록색으로 만들면서,or visited에 들어가면서) 인접 노드들이 큐에 삽입되는(노란색이 되는, 스택에 진입) 과정을 반복하는 것이다. (모든 노드가 초록색이 될 때까지, 스택이 빌 때까지 반복한다.) 

>  **방문순서 : A-B-E-D-H-G-C-F**

<br>



### BFS 탐색 구현(Java)

이전에 시간 구현을 했던 스택 구조를 가져와서 DFS를 구현해 보도록 하겠다.

```java
public static List<Integer> dfs(IGraph graph, int from) {
    List<Integer> result = new ArrayList<>();
    Set<Integer> visited = new HashSet<>();
    
    //dfs를 위한 Mystack
    IStack<Integer> stack = new MyStack<>();
    
    //자바의 스택 사용을 원하면
    //Stack<Integer> stack = new Stack<>();
    
    //1.
    stack.push(from);
    visited.add(from);
    
    //2.
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
   - 해당 데이터를 result(방문) 리스트에 넣어주고 
   - next와 인접한 노드들이 0개, 1개 혹은 여러개 일 것이기 때문에 for문으로 이를 하나씩 순회하여 만약 방문하지 않았다면 (visited에 존재하지 않는다면) stack에 삽입하고 visited에도 삽입한다.
     - 여러 노드들은 getNodes()메소드를 통해 가져온다.
3. 스택이 모두 비었다면 어떤 순서로 탐색(방문)을 했는지를 저장한 result를 반환하여 마무리한다.

<br>

## 심화) BFS 순환(Cycle) 탐지

1. <mark>방향 그래프(Directed Graph) 순환 탐지 하기 (Cycle Detection)</mark>

DFS에서 cycle의 존재 여부를 확인하는 방법은 Back Edge가 있는지를 확인하면 된다.

Back Edge는 자기 자신을 가리키거나(Loop) 자신의 이전 정점을 가리키는 경우의 Edge(간선)을 의미하며 이 Back Edge가 있으면 순환이 있다고 본다.

<br>

아래 그림을 통해 예시를 살펴보자.

![image](https://user-images.githubusercontent.com/79521972/155873489-41fc3b01-dc5e-427f-90c8-f55846f2209e.png)

위 그림의 2번 노드에서 시작한다고 하면 여기서 Back Edge는 **1 → 2를 가리키는 것**, **3이 자기 자신을 가리키는 것**, 그리고 **0이 2를 가리키는 것** 이렇게 세 가지가 Back Edge가 된다.

<br>

과정은 다음과 같다.

1. 재귀 DFS를 수행할 때, 각 정점의 index와 더불어 방문 여부 배열(visited), 재귀 stack을 전달한다.
2. 현재 정점을 visited 된 것으로 하고 현재 정점을 재귀 stack 에 push한다.
3. 인접 정점 중 방문 되지 않은 정점에 대하여 재귀 호출한다.
4. 만약 현재 방문하고 있는 정점이 재귀 stack에 있다면 cycle이 있는 것으로 간주하여 true를 리턴한다.
5. 자바의 경우 이 전체를 구현하기 위한 wrapper 클래스를 구현하여 cycle 발생 시 true를 리턴하는 식이다.

<br><br>

<br>

2. <mark>무방향 그래프 순환 탐지하기</mark>

무방향 그래프는 상호 연결되어 있기 때문에 방향 그래프에서 순환 탐지 하듯이 탐지를 시도하면 무조건 순환이 있는 것으로 탐지된다.

<br>

그래서 이를 보완한 방법은 다음과 같다.

1. 그래프를 만들고 나서 재귀 형태로 탐색을 진행하는 메소드를 생성하여 현재 정점, 방문 여부 배열(visited), 재귀, parent 배열을 전달한다.
   - parent는 현재 노드와 이어진 이전에 탐색된 노드가 저장된 배열이다.
2. 현재 탐색 정점을 방문 완료로 표시하고 parent는 이전 정점으로 저장한다.
3. 현재 정점에 이어진 인접정점을 모두 차례로 탐색 수행한다.
4. 인접 정점이 아직 방문되지 않은 상태라면 재귀 호출 하여 그 결과를 받아 반환한다.
5. 이 과정이 핵심인데, 인접 정점이 방문 된 상태이면서 parent는 현재 정점 값이 아니면 cycle이 존재하는 것으로 결과 값을 반환한다.

<br>

이해를 위해 다음과 같은 예시를 들어보겠다.

<br>

인접 정점의 parent가 현재 정점인 경우를 생각해 보자.

현재를 A, 인접 정점을 B라고 할 때, A를 방문하고 B에서 다시 A를 방문했다면 무방향 그래프에서 상호 인접 정점끼리는 무조건 재탐색 수행을 하고자 하기 때문에 이것을 제외 시켜야 한다.

<br>

하지만 만약, 현재 정점이 parent가 아니라면 A → B → A 의 순이 아니라 A → B → ? → ? → A처럼 중간에 다른 정점들을 추가로 탐색 했다는 의미이며, 이는 당연히 cycle이 있는 것이다.

<br>

<br>

| 너비 우선 탐색 과정                                          | 깊이 우선 탐색 과정                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![Animated_BFS](https://user-images.githubusercontent.com/79521972/154641634-65bd2679-21ed-43d2-b8cd-6d828873c440.gif) | ![220px-Depth-First-Search](https://user-images.githubusercontent.com/79521972/154642490-211ab9c7-b333-4107-92a7-a0cc2c806a8e.gif) |
| [Wikipedia](https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) | [Wikipedia](https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) |

<br>

## 너비 우선 탐색 응용

1. 최단 경로 찾기 및 minimum spanning tree
2. 순환 탐지
3. 위상 정렬
4. Puzzle 풀이(하나의 solution 찾기)

<br>

### DFS vs. 백트래킹(back tracking)

DFS는 그래프 탐색 방법 중 하나로 그래프 구조에서 모든 정점을 탐색할 수 있는 알고리즘 중에 하나이다. 깊이를 우선순위로 하여 탐색을 진행하기 때문에 재귀 혹은 스택 구조를 활용한다.

<br>
재귀를 이용하여 탐색을 진행한다는 점에서 완전탐색 알고리즘의 재귀 / 백트래킹과 유사하다고 느낄 수 있다.

<br>

하지만, 재귀라는 것은 자기 자신의 함수(메소드)를 호출하는 방식이기 때문에 DFS는 재귀 방식을 이용해 수행하는 많은 알고리즘 중에 하나인 것이다.

또한 백트래킹은 재귀를 통해 알고리즘을 풀어 가는 기법으로 가지지기를 통해서 탐색을 하다가 유망하지 않다고 생각이 들면 추가 탐색을 진행하지 않고 뒤로 돌아(Back) 다시 탐색(Tracking)을 진행하는 방식인 것이다.

<br>

그래서 DFS와 백트래킹 간에는 이러한 차이점이 있는 것이다.

<br>

## 관련된 문제

[<u>백준 2667번 단지번호 붙이기</u>](https://www.acmicpc.net/problem/2667)

[백준 13023번 ABCDE](https://www.acmicpc.net/problem/13023)

<br>

## 관련된 Post

- **Tree(트리) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조

- **Graph(그래프) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)을 참조

- **BFS**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-%EB%84%88%EB%B9%84-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(BFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)을 참조<br>

  



 