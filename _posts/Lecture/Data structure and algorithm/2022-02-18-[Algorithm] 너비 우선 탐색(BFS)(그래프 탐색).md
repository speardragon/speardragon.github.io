---
layout: single
title: "[Algorithm] 너비 우선 탐색(BFS)(그래프 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Graph']
tag: ['Data structure', 'Algorithm', 'BFS', '너비 우선 탐색']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

<br>

## 그래프 탐색 

### **그래프 탐색이란**

- 하나의 정점으로붙 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것
- EX) 특정 도시에서 다른 도시로 갈 수 있는 지 없는 지, 전자 회로에서 특정 단자와 단자가 서로 연결 되어 있는 지

<br>

<mark>Queue를 이용하여 구현</mark>

### BFS (Breath-First Search, 너비 우선 탐색 )

> DFS는 그래프의 개념이 반드시 선행되어야 하기 때문에 다음 포스트를 먼저 확인하길 바람.
>
> >[<u>그래프</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-1.-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)
> >
> >[<u>트리</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/Algorithm-CH7.-Tree(%ED%8A%B8%EB%A6%AC)/)

<br>

#### **너비 우선 탐색(BFS)이란**

루트 노드(혹은 임의의 다른 노드)에서부터 시작하여 인접한 노드를 먼저 탐색해 나가는 방법

- 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다.
- 즉, 깊이(deep)탐색하기 전에 넓게(wide) 탐색하는 것이다.
- <mark>사용하는 경우</mark>: 두 노드 사이의 **최단 경로** 혹은 **임의의 경로**를 찾고 싶을 때 이 방법을 선택한다.
  - 예를 들어, 지구상의 존재하는 모든 인관관계를 표현한 후 David와 Scarlet 사이에 존재하는 경로를 찾는 경우
  - 깊이 우선 탐색(DFS)의 경우 - 모든 인간 관계를 다 살펴봐야 할 수도 있다.
  - 너비 우선 탐색(BFS)의 경우- David와 가까운 관계부터 탐색한다.
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
  - 다시말해서 선입선출(FIFO)의 원칙으로 탐색
  - 다양한 방법이 있지만 queue의 형태로 구현하는 것이 바람직함
- 이후에 배울 'Prim', 'Dijkstra' Algorithms과 비슷하다.

<br>

#### BFS의 과정

그림을 통해 알아보도록 하겠습니다.

<img src="https://user-images.githubusercontent.com/79521972/154630160-34ef0e79-d3f4-4c08-ae9f-c312c48a4a46.png" alt="image" style="zoom:50%;" />

![image](https://user-images.githubusercontent.com/79521972/154632795-e60767d0-372f-47ac-aa04-9051b1a7a7dc.png)

1. 시작 노드 A를 방문한다.
   - 큐에 방문된 노드를 삽입(enqueue)한다.
   - 초기에는 큐 안에 시작 노드만이 저장된다.
   - A 노드를 방문(삽입)했으면 이 노드와 인접한 이웃 노드들을 방문한다.
2. 큐에서 꺼낸 노드와 인접한 노드들을 순차대로 방문한다.
   - 큐에서 꺼낸 노드를 방문한다.
   - 이 노드와 인접하는 노드들을 **모두** 방문(enqueue)한다.
     - 인접한 노드가 없으면 큐의 맨 앞(peek)에서 노드를 꺼낸다(dequeue).
   - 큐에 방문된 노드를 삽입(enqueue)한다.
3. 큐가 소진될 때까지 반복한다.

큐에서 데이터가 빠짐(dequeue)과 동시에 해당 데이터는 초록색이 되고 이와 인접한 노드는 노란색이 되면서 방문(enqueue)을 한 상태가 되고 큐에서 순서대로 데이터를 빼면서(초록색으로 만들면서) 인접 노드들이 큐에 삽입되는(노란색이 되는,  큐에 진입) 과정을 반복하는 것이다. (모든 노드가 초록색이 될 때까지, 큐가 빌 때까지) 

> 방문순서 : A-B-C-D-E-F-G-H



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

## 관련된 문제

[<u>백준 2667번 단지번호 붙이기</u>](https://www.acmicpc.net/problem/2667)

[<u>백준 2468번 ABCDE</u>](https://www.acmicpc.net/problem/2468)

<br>





## 관련된 Post

- 자료 구조 [<u>트리(Tree)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/Algorithm-CH7.-Tree(%ED%8A%B8%EB%A6%AC)/)
- 자료 구조 [<u>그래프(Graph)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-1.-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)

- [<u>깊이 우선 탐색(DFS, Depth-First Search)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-3.-%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(DFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)



























