---
layout: single
title: "[Algorithm] 너비 우선 탐색(BFS)(그래프 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Graph']
tag: ['Data structure', 'Algorithm', 'BFS', '너비 우선 탐색']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

이번 시간에는 그래프 탐색 방법 중 너비 우선 탐색(BFS)에 대해서 배워볼 것이다.

<br>

## **그래프 탐색이란**

- 하나의 정점으로붙 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것
- EX) 특정 도시에서 다른 도시로 갈 수 있는 지 없는 지, 전자 회로에서 특정 단자와 단자가 서로 연결 되어 있는 지

그래프는 탐색하는 동안 동일한 정점으로 다시 이동할 수 있는 싸이클이 있을 수 있다. 그래서 동일한 정점이 다시 처리되지 않도록 하려면 처리 후 정점을 방문(visited)했다는 표시를 함으로써 중복 방문을 피하도록 하는 것이 그래프 탐색의 핵심이다.

<br>

<mark>Queue를 이용하여 구현</mark>

### BFS (Breath-First Search, 너비 우선 탐색 )

> DFS는 그래프의 개념이 반드시 선행되어야 하기 때문에 다음 포스트를 먼저 확인하길 바람.
>
> >**Tree(트리) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조
> >
> >**Graph(그래프) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)을 참조

<br>

#### **너비 우선 탐색(BFS)이란**

루트 노드(혹은 임의의 다른 노드)에서부터 시작하여 인접한 노드를 먼저 탐색해 나가는 방법이다.

또한 BFS는 그래프에서 최단 경로를 찾는 정점 기반 알고리즘으로 유명하다.

<br>

특징은 다음과 같다.

- 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다.
- 즉, 깊이(deep)탐색하기 전에 넓게(wide) 탐색하는 것이다.
- <mark>사용하는 경우</mark>: 두 노드 사이의 **최단 경로** 혹은 **임의의 경로**를 찾고 싶을 때 이 방법을 선택한다.
  - 예를 들어, 지구상의 존재하는 모든 인관관계를 표현한 후 철수와 영희 사이에 존재하는 경로를 찾는 경우
  - 깊이 우선 탐색(DFS)의 경우 - 모든 인간 관계를 다 살펴봐야 할 수도 있다.
  - 너비 우선 탐색(BFS)의 경우- 철수와 가까운 관계부터 탐색한다.
- `너비 우선 탐색(BFS)`이 `깊이 우선 탐색(DFS)`보다 더 복잡하다.

<br>

#### BFS의 특징

BFS의 특징 몇 가지를 살펴 보도록 하겠습니다.

- 직관적이지 않다.
  - BFS는 시작 노드를 기준으로 거리에 따라 단계별로 탐색하는 것이기 때문에 다소 오해가 발생할 수 있다.
- BFS는 재귀적으로 동작하지 않는다.(DFS와 달리)
- 이후 구현에서 다룰 것이지만 반드시 그래프를 탐색하면서 어떤 노드를 방문했었는 지의 여부를 검사해야한다. (구현에서 visited 변수)
  - 무한루프에 빠질 위험이 있기 때문
- BFS는 대기순서에 따라 반복적인 방문을 해야하기 때문에 큐(Queue)를 사용하여 구현한다.
  - 다시말해서 **선입선출(FIFO)의 원칙**으로 탐색
  - 다양한 방법이 있지만 queue의 형태로 구현하는 것이 바람직함
    - 먼저 들어온 데이터 들을 먼저 탐색하는 방식이기 때문.
- 이후에 배울 'Prim', 'Dijkstra' Algorithms과 비슷하다.

<br>

#### BFS의 과정

그림을 통해 알아보도록 하겠습니다.

<img src="https://user-images.githubusercontent.com/79521972/154630160-34ef0e79-d3f4-4c08-ae9f-c312c48a4a46.png" alt="image" style="zoom:50%;" />

![image](https://user-images.githubusercontent.com/79521972/154632795-e60767d0-372f-47ac-aa04-9051b1a7a7dc.png)

1. 시작 노드 A를 방문한다.
   - 시작 정점을 방문하여 방문 표시(visited, 초록색 표시)한 후 해당 노드를 enqueue한다.

2. 큐에서 첫번째 정점을 제거하고 제거된 정점과 **인접한 정점**에 대하여 방문하지 않은 정점을 방문한다.

- 큐에서 꺼낸 노드를 방문한다.
- 이 노드와 인접하는 노드들을 **모두** 방문(enqueue)한다.
- 인접한 노드가 없으면 큐의 맨 앞(peek)에서 노드를 꺼낸다(dequeue).

3. 큐가 소진될 때까지 위 과정을 반복한다.

<br>

큐에서 데이터가 빠짐(dequeue)과 동시에 해당 데이터는 초록색이 되고 이와 인접한 노드는 노란색이 되면서 방문(enqueue)을 한 상태가 되고 큐에서 순서대로 데이터를 빼면서(초록색으로 만들면서) 인접 노드들이 큐에 삽입되는(노란색이 되는,  큐에 진입) 과정을 반복하는 것이다. (모든 노드가 초록색이 될 때까지, 큐가 빌 때까지 반복한다.) 

> **방문순서 : A-B-C-D-E-F-G-H**

<br>

### BFS 탐색 구현(Java)

```java
package graph;

import java.util.*;

public class GraphAlgorithms {
	public static List<Integer> bfs(IGraph iGraph, int from) {
        List<Integer> result = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        
        queue.add(from);
        visited.add(from);
        while (!queue.isEmpty()) {
            Integer next = queue.poll();
            result.add(next);
            for (Integer n : iGraph.getNodes(next)) {
                if(!visited.contatins(n)) {
                    queue.add(n);
                    visited.add(n);
                }
            }
        }
        return result;
    }
}
```

Input parameter: 지난 시간 구현한 그래프(iGraph), 시작 노드(from)

Return: 방문한 노드를 순서대로 저장한 리스트

1. 처음에 시작할 때는 시작 노드(from)부터 시작한다.
   - queue에 시작노드를 삽입하고 방문하였기 때문에 visited에도 넣어준다.
2. 큐가 비어있을 때까지 다음을 반복한다.
   - next변수에 큐의 데이터를 하나씩 빼온다.
   - 해당 데이터를 result 변수에 넣어주고 
   - next와 인접한 노드들이 0개, 1개 혹은 여러개 일 것이기 때문에 for문으로 이를 하나씩 순회하여 만약 방문하지 않았다면 (visited에 존재하지 않는다면) queue에 삽입하고 visited에도 삽입한다.
3. 큐가 모두 비었다면 어떤 순서로 탐색을 진행했는 지의 결과인 result를 반환하여 마무리한다.



| 너비 우선 탐색 과정                                          | 깊이 우선 탐색 과정                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![Animated_BFS](https://user-images.githubusercontent.com/79521972/154641634-65bd2679-21ed-43d2-b8cd-6d828873c440.gif) | ![220px-Depth-First-Search](https://user-images.githubusercontent.com/79521972/154642490-211ab9c7-b333-4107-92a7-a0cc2c806a8e.gif) |
| [Wikipedia](https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) | [Wikipedia](https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89) |

<br>

### BFS 사용 예시 - 최단 경로 문제

너비 우선 탐색(BFS)는 퍼즐 게임 등을 해결할 때 많이 쓰이는 알고리즘이다. 또한 다익스트라 알고리즘에도 사용되며 이에 대한 포스팅은 다음을 참고하라.

- **다익스트라 알고리즘**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/dijkstra/Algorithm-Dijkstra-Algorithm(%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)/)을 참조

<br>

**최단 경로 문제**

최단 경로 문제는 코딩테스트나 여러 문제 사이트에서 난이도 있는 문제이며 단골 문제이기도 하다. 

특히, 이는 DFS가 아닌 BFS로 풀어야 문제의 결과를 신속하게 낼 수 있다. 왜냐하면 BFS를 통해 구현하면 모든 정점방문 결과를 최단 경로로 보장이 가능하지만 DFS는 보장할 수 없기 때문이다.

하지만 BFS로 최단 경로 문제를 풀때는 다음과 같은 제약 조건이 있다.

- <span style="color:red">가중치가 1이어야 함.</span>

- 그 가중치가 구하려는 문제의 답에 해당해야 함
  - 최단 거리면 거리가 가중치
  - 최단 시간이면 시간이 가중치

- 정점과 간선의 수가 너무 많으면 안된다.

  - O(V+E)의 시간 복잡도를 갖는데 이때문에 보통 E가 V보다 많기 때문에 E가 너무 많으면 시간 내에 해결이 안됨

  

아래 그림을 보면서 최단 경로 문제를 이해해 보도록 하자.

![image](https://user-images.githubusercontent.com/79521972/155872170-748c75dd-dddc-4a5f-b179-d80e367e748a.png)

 만약 <span style="color:orange">주황색</span>으로 칠해진 곳에서 출발하여 <span style="color:red">빨간색</span>으로 칠해진 곳으로 가야하는 것이 문제라고 하자.

이때 유일한 경로는 <span style="color:blue">파란 화살표</span>와 <span style="color:red">빨간 화살표</span>가 있을 수 있다.(다른 경로에 비해 대표적인 경로)

 <br>

이 상황에서 <mark>BFS</mark>로 탐색을 하면 <span style="color:red">빨간 부분</span>과 <span style="color:blue">파란 부분</span>의 인접 정점을 동시에 차례 대로 탐색을 하면서 시작점이 고정이기 때문에 무조건적으로 모든 인접 정점이 최소의 경우로만 이동하는 것이 보장 된다.

그래서 BFS로 탐색 시, 원하는 위치에 도착했다면 추가적인 탐색을 종료하고 바로 결과가 나오는 것이다.

<br>

그러나 <mark>DFS</mark>로 탐색을 하면, 상단의  <span style="color:red">빨간색 부분</span>부터 우선 탐색이 이루어질 수 있는데, 하지만 이 경로는 최소 경로라는 것을 보장 받지 못한다. 실제로 최소경로가 아니기도 하다.

보장을 받기 위해서는 이미 방문한 정점에 대해서 미 방문 상태로 전환하고 다른 동일 위치에 도달할 수 있는 경우를 모두 체크해야 하는데, 이는 DFS가 아니라 Broute Force(~~무차별 대입 방법~~)

이러한 이유로 최단 경로 문제를 풀 때는 BFS가 상당히 유리하게 작용한다.

<br>

## 심화) BFS 순환(Cycle) 탐지

1. <mark>방향 그래프(Directed Graph) 순환 탐지 하기 (Cycle Detection)</mark>

아래와 같은 방향 그래프에 대해 생각해 보자.

![image](https://user-images.githubusercontent.com/79521972/155872937-f2115559-7889-4ccf-aef1-4d08773093eb.png)

BFS를 통해 순환을 탐지하기 위해서는 위상 정렬을 사용할 수 있다.

- **Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>

<br>

방식은 다음과 같다.

1. 처음에 내차수가 0인 정점을 전부 찾아 각 정점을 큐에 넣는다.
2. 큐에서 하나씩 빼서 방문 완료 처리하고 방문 완료 정점의 수를 1만큼 늘린다.
3. 해당 정점의 인접 정점의 내차수를 1씩 뺀 다음 인접 정점 중에서 내채수가 0이 되는 것만 큐에 넣는다.
4. 큐가 빌 때까지 2, 3번 과정을 반복한다.
5. 큐가 비었는데도 전체 정점의 갯수와 방문 완료한 정점의 갯수가 다르면 해당 그래프에는 Cycle이 있는 것이다.

<br>

2. <mark>무방향 그래프 순환 탐지하기</mark>

무방향 그래프는 상호 연결되어 있기 때문에 방향 그래프에서 cycle을 탐지했던 것처럼 탐지하면 무조건 cycle이 있는 것으로 탐지된다.

<br>

따라서 다른 방법을 사용 해야 한다. 방법은 아래와 같다.

<br>

1. 그래프를 만들고 BFS 탐색 함수를 생성해서 현재 정점 위치와 방문 여부에 대한 배열을 전달한다.
2. 현재 탐색 정점을 방문 완료로 표시하고 parent 배열을 따로 생성한 후에 최초 탐색 정점의 parent는 -1로 저장한다.
   - parent는 현재 노드와 이어진 이전에 탐색된 노드가 저장된 배열이다.
3. BFS탐색을 위해 큐를 생성하고, 최초 탐색의 수행 정점을 큐에 넣는다.
4. 큐가 비어 있지 않은 동안은 반복문을 수행하면서 큐의 정점을 하나씩 빼고 방문되지 않은 인접 정점들을 탐색한다.
5. 이 단계가 굉장히 중요한데, 인접 정점이 방문을 한 상태이면서 parent가 현재 정점 값이 아니라면 cycle이 존재하는 것이다.

<br>

이해를 위해 다음과 같은 예시를 들어보겠다.

<br>

인접 정점의 parent가 현재 정점인 경우를 생각해 보자.

현재를 A, 인접 정점을 B라고 할 때, A를 방문하고 B에서 다시 A를 방문했다면 무방향 그래프에서 상호 인접 정점끼리는 무조건 재탐색 수행을 하고자 하기 때문에 이것을 제외 시켜야 한다.

<br>

하지만 만약, 현재 정점이 parent가 아니라면 A → B → A 의 순이 아니라 A → B → ? → ? → A처럼 중간에 다른 정점들을 추가로 탐색 했다는 의미이며, 이는 당연히 cycle이 있는 것이다.

<br>

 

## 너비 우선 탐색 응용

1. 최단 경로 찾기 및 minimum spanning tree
2. P2P 네트워크
3. 검색 엔진 Crawling
4. Ford - Fulkerson Algorithm
5. Network Broadcasting
6. Garbage Collection
7. GPS 네비게이션 시스템
8. Social Networking

<br>

이 예시들은 모두 인접한 데이터에 대해 우선적으로 찾는 것이 중요한 것들이다.



## 관련된 문제

[<u>백준 2667번 단지번호 붙이기</u>](https://www.acmicpc.net/problem/2667)

[<u>백준 2468번 ABCDE</u>](https://www.acmicpc.net/problem/2468)

<br>





## 관련된 Post

- **Tree(트리) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Tree,-Binary-Tree(%ED%8A%B8%EB%A6%AC,-%EC%9D%B4%EC%A7%84-%ED%8A%B8%EB%A6%AC)/)을 참조
- **Graph(그래프) 자료구조**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)을 참조
- **DFS**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(DFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)을 참조<br>

<br>

긴 글 읽어주셔서 감사합니다.



























