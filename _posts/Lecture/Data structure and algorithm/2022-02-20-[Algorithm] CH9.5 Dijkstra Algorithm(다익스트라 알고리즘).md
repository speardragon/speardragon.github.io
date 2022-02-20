---
layout: single
title: "[Algorithm] CH9-5. Dijkstra Algorithm(다익스트라 알고리즘)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Dijkstra']
tag: ['Data structure', 'Algorithm', 'Dijkstra', '다익스트라']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

<br>

## Dijkstra Algorithm - 다익스트라 알고리즘

이번 시간에는 다익스트라 라는 최단 거리를 알아내는 알고리즘에 대해 배워 보도록 하겠습니다.

### 다익스트라란?

> 그래프에서 최단 경로를 찾아내는 알고리즘은 많이 나와있지만 그 중에서 가장 기본이 되는 알고리즘을 뜻함.

다른 최단 거리 알고리즘은 다익스트라를 기반으로 나온 알고리즘이다.

- 한 vertex에서 다른 vertex까지의 최단 경로
- Edge의 가중치(weight)은 양수만 가능(음수 불가능)
  - 가중치에 음수가 있는 경우에는
    - Bellman-Ford's algorithm(벨만-포드 알고리즘) 혹은 Floyd-Warshall(플로이드-워셜) 알고리즘을 사용
  - 대부분의 생활 속의 문제에서 가중치는 양수를 갖기 때문에 보편적으로 다익스트라 사용

다익스트라 알고리즘은 우선순위큐를 이용하여 구현하기 때문에 우선순위 큐에 대한 내용을 다음 포스팅에서 확인하기 바람.

> [<u>자료구조 힙(Heap)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/Algorithm-CH8.-Heap(%ED%9E%99)/)



<br>

### 다익스트라의 컨셉(The concept of Djkstra's)

![image](https://user-images.githubusercontent.com/79521972/154834579-477e4921-ffaf-4576-879e-91c8c3427e50.png)

여러 도시 중 A라는 도시에서 G라는 도시까지 간다고 가정

- 경로는 여러가지가 있을 것임
  - A - B - G
  - A - D - F - G
  - ....
- 만약 한 경로마다 소모되는 시간은 다음 그림과 같다고 하자

![image](https://user-images.githubusercontent.com/79521972/154834650-cb0eff4d-a627-4b9f-b02a-2a79beacd932.png)

그렇다면 이때 가장 최단 거리의 경로는 어떤 것일까?

다익스트라 알고리즘은 이 문제를 다음과 같은 명제로 제시한다.

> "부분 경로에서 최단 거리의 집합"

<br>

<span style="color: blue">파란색 숫자</span>: 예상 소요 시간

<span style="color: red">빨간색 숫자</span>: 해당 노드까지 실제 소요한 시간

![image](https://user-images.githubusercontent.com/79521972/154835352-bd412f19-f984-478d-8fab-f333a3d93780.png)

1. **A 도시에서 출발**

   - 다음에 갈 수 있는 도시는 3개뿐 : B, C, D

   - <img src="https://user-images.githubusercontent.com/79521972/154835469-b6a45245-b16e-4859-a40e-043a0683cb8e.png" alt="image" style="zoom:50%;" />

2. **다음 세 도시(B, C, D) 중 다음으로 갈 수 있는 경로 탐색**

   - B도시에서 다음으로 갈 수 있는 경로는
     - E 도시(A-B-E): 6시간 소모
     - <span style="color: green">G 도시(A-B-G): 11시간 소모 (도착)</span>
       - <img src="https://user-images.githubusercontent.com/79521972/154835618-f426528b-c188-4809-86e0-dd9aa77bd65a.png" alt="image" style="zoom:50%;" />

   - C도시에서 다음으로 갈 수 있는 경로는
     - E 도시(A-C-E): 8시간 소모
       - <img src="https://user-images.githubusercontent.com/79521972/154838981-d6d98dc5-8860-4d6a-95bb-61c55b53fbce.png" alt="image" style="zoom:50%;" />
       - 위에서 B를 거쳐 E를 가는 것이 더 빠르기 때문에 이 경로는 제외한다.
     - F 도시(A-C-F): 4시간 소모
       - <img src="https://user-images.githubusercontent.com/79521972/154839064-6ed86dd2-d7f2-4923-a9a1-fcd65b9120bd.png" alt="image" style="zoom:50%;" />
   - D도시에서 다음으로 갈 수 있는 경로는 
     - F 도시(A-D-F): 9시간 소모
       - <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220220195424676.png" alt="image-20220220195424676" style="zoom:50%;" />
       - 위에서 C를 거쳐 F에 가는 것이 더 빠르기 때문에 이 경로는 제외한다.

3. **남은 두 도시(E, F)에서 다음으로 갈 수 있는 경로 탐색**
   - E 도시에서 다음으로 갈 수 있는 경로는
     - <span style="color: green">G 도시(A-B-E-G): 7시간 소모 (도착)</span>
       - <img src="https://user-images.githubusercontent.com/79521972/154839232-0ce04ef7-82a3-4e4b-8f48-de91bf57d3f1.png" alt="image" style="zoom:50%;" />
       - 이전에 A에서 B를 거쳐 G에 도착했던 경로(11시간 소모)보다 시간 소모가 더 적기 때문에 해당 경로로 update한다.
   - F도시에서 다음으로 갈 수 있는 경로는
     - <span style="color: green">G도시(A-C-F-G): 6시간 소모(도착)</span>
       - <img src="https://user-images.githubusercontent.com/79521972/154839343-cb11c938-41ae-455f-898c-79021b5c50c3.png" alt="image" style="zoom:50%;" />
       - 바로전에 A-B-E-G 경로에 비해 1시간이 더 적은 6시간이 소모되므로 이 경로로 다시 update한다.

<br>

이를 통해 다익스트라는 전체 경로의 최단 경로를 찾기 위해서 중간중간 부분 노드에서의 최단 경로를 찾고 새로 탐색한 경로가 내가 이전에 찾았던 경로보다 더 짧은 거리라면 해당 거리로 계속해서 업데이트한다.

이런 방식을 통해 최종적으로 목적지에 도착했을 때의 최단 거리를 찾아내는 방식인 것이다.

<br>

#### **정리**

- 시작 노드에서 출발하여
- 연결되어 있는 노드들을  탐색하며
- 최단 거리를 업데이트

-> 특정 노드까지의 최단 거리 경로를 저장하는 방식이 중요함 -> 배열 사용

<br>

그렇다면 배열을 어떻게 활용하여 이를 저장하는 지 보자.

![image](https://user-images.githubusercontent.com/79521972/154845740-5ffa6eed-576a-40e2-820e-e2d1250fd5aa.png)

위 그림에서 A노드에서 F노드까지 가는 경로를 탐색한다고 가정

배열에 값이 업데이트 된다는 것은 우선순위 큐에 삽입된다는 뜻이기도 하다.

1. 노드 수만큼 배열을 생성
2. 배열의 값을 출발 노드에는 0, 나머지는 infinite(무한)값으로 넣는다.
   - 0번 index의 값 0은 A노드에서 A노드까지의 거리를 의미
   - 1번 index의 값은 A노드에서 B노드까지의 거리를 의미

3. 출발 노드 큐에 삽입

   - <img src="https://user-images.githubusercontent.com/79521972/154840696-61890654-8afe-4507-806b-df11cc6a1d4a.png" alt="image" style="zoom:50%;" />

4. A 노드에서 갈 수 있는 경우 따져보기

   - ![image](https://user-images.githubusercontent.com/79521972/154840812-173ba1c8-5575-4cea-855d-5d2da330cc78.png)

   - B노드(1번 그림): 5
     - 초기 컨셉에 의해 B노드의 초기 거리는 무한대로 A를 거쳐 B로 가는 거리가 무조건 더 짧을 것이기 때문에 이 값으로 업데이트한다.
   - C노드(2번 그림): 2
     - 위의 이유와 같이 C노드 까지 가는 경로 중 A에서 C로 가는 거리가 가장 짧을 것이기 때문에 업데이트를 한다.
   - D노드(3번 그림): 4
     - 위와 같은 이유로 D노드 까지 가는 경로가 아직 초기화 되어있지 않기 때문에 A에서 C까지 가는 거리가 무조건 짧을 것이기 때문에 이 값으로 업데이트를 한다.

5. 그 다음 큐에서 데이터를 빼서 탐색 계속 진행(C 노드)

   - C 노드에서 갈 수 있는 경우 따져보기
     - ![image](https://user-images.githubusercontent.com/79521972/154841832-135beb2a-8a90-424d-b66f-c7a9cc49fca8.png)
     - B노드(1번 그림): 3
       - C노드에서 B노드로 가는 경우(3)와 기존의 B노드(5)까지 가는 경우를 비교해 봤을 때 C노드를 거쳐서 가는 것이 더 짧기 때문에 값을 업데이트 한다.
     - D노드/F노드 : 3/7
       - D노드도 위와 같은 이유로 업데이트 해야하고
       - F노드는 이전에 기록된 노드가 없었기 때문에 해당 값을 추가한다.

6. 다음 우선순위 큐에 있는 노드에 대하여 탐색을 진행(B 노드)
   - B노드에서 갈 수 있는 경우는 F노드 뿐이다.
   - ![image](https://user-images.githubusercontent.com/79521972/154843281-2f8df2df-8793-4c66-80c8-b9ea6e2162e6.png)
   - 이전에 기록된 F의 값은 7로, B노드를 거치는 경우가 업데이트 되었기 때문에 이를 적용하여 계산한 결과가 5이기 때문에 다시 F값이 업데이트 된다.
7. 우선순위 큐에서 D노드 꺼내 탐색 진행
   - ![image](https://user-images.githubusercontent.com/79521972/154843616-42244722-8311-4d9a-9e2b-906be03d9f6b.png)
   - E노드(1번 그림): 9
     - E노드는 이전 데이터가 무한이기 때문에 D노드를 거쳐 가는 것이 무조건 최단 경로일 것이기 때문에 이 값으로 업데이트한다.
8. 그 다음 큐도 D 노드이기 때문에 7번 과정을 진행 하지만 이 값은 업데이트 되지 않는다.
9. <img src="https://user-images.githubusercontent.com/79521972/154844116-0dfa49e7-f85d-4f89-ba6a-a4aa9cc6bf2e.png" alt="image" style="zoom:67%;" />
   - 마찬 가지로 유선순위 큐에 있는 큐는 순서대로 탐색하며 위 과정을 반복하여 큐를 비운다.



![image](https://user-images.githubusercontent.com/79521972/154844165-79a7717a-cec3-47ea-8f3c-677f84bf9ad3.png)

최종적으로 위 표와 같이 노드 별 거리가 배열의 데이터로 초기화 된 값으로 저장이 된 모습이다.

> 이 표가 의미하는 바는 굉장히 중요한데 A의 값이 0이라는 것부터 A노드는 기준이 되어 다른 index의 값이 A 노드와의 최단 거리를 의미하게 된다.
>
> >  따라서 A노드부터의 최단거리를 구하고 싶으면 해당 노드의 index만 알면 된다!

<br>

## 다익스트라 구현

우선순위 큐를 사용하여 최단 거리를 매번 계산하지 않고 가져올 수 있기 때문에 이를 활용하여 구현해 볼 것이다.

```java
public static int dijkstraShortestPath(Igraph graph, int src, int dst) {
    // 1.
    int size = 0;
    for (int n : graph.getVertexes()) {
        if (size < n) {
            size = n + 1;
        }
    }
    
    // 2.
    //distance 배열을 노드 갯수 만큼 초기화
    int[] dist = new int[size];
    for (int i = 0; i < dist.length; i++) {
        dist[i] = Integer.MAX_VALUE; //distance 값을 INF로 초기화
    }
    dist[src] = 0; // 시작 노드의 distance = 0
    
    // 3.
    //<vertex, distance>
    // distance를 기준으로 하는 민힙(minHeap)
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
        return a[1] - b[1];
    });
    
    pq.add(new int[] {src, 0});
    
    // 4.
    while (!pq.isEmpty()) {
        int[] top = pq.poll();
        int veretex = top[0];
        int distance = top[1];
        
        if (dis[vertex] < distance) {
            continue;
        }
        
        for (int node : graph.getNodes(vertex)) {
            if (dist[node] > dist[vertex] + graph.getDistance(vertex, node)) {
                dist[node] = dist[vertex] + graph.getDistance(vertex, node);
                pq.add(new int[] {node, dist[node]});
            
        }
	}
    // 5.
    return dist[dst];
}

```

parameter:

- graph: 그래프
- src: 출발 노드
- dst: 도착 노드

return:

- 출발 노드로부터 도착 노드까지의 최단 거리

1. 가장 먼저 노드가 몇 개인지 센다.
   - getVertexes() 메소드로 vertex를 하나씩 가져와서 size변수에 계속 초기화 시키면 마지막 vertex의 값이 가장 큰 vertex 이므로 해당 숫자가 노드의 갯수이다.
     - +1을 해주는 이유는 노드가 5이면 5번 index까지 값을 넣을 수 있게 하기 위함이다.(index는 0번부터 count하기 때문에)
2. distance 배열은 노드 갯수만큼 있기 때문에 1번에서 구한 값으로 distance 배열을 초기화한다.
   - 앞에서 구한 size 변수가 배열의 크기가 되어 배열이 생성
   - 배열의 모든 데이터에 무한대를 넣어야 하는데 코드 상으로 무한을 구현하기 어려우므로 integet(정수)형의 최대 크기 값을 배열의 넣어준다.
   - 단, 배열의 0번째 index, 출발 노드의 값은 0으로 초기화한다.
3. 우선순위 큐를 생성해 준다.
   - 우선순위 큐에는 두 가지 데이터가 들어간다.
   - 첫 번째(0번 index)는 vertex(노드 번호), 두 번째(1번 index)는 해당 노드까지의 최단 거리이다.
   - 이 우선순위 큐에 시작노드를 넣어준다

4. 큐가 빌 때까지 다음 과정을 반복한다.

   - 우선순위 큐의 top에서 데이터 하나를 빼내온다.

   - 해당 데이터의 0번 index는 vertex, 1번 index는 최단 거리로 각각 특정 변수에 저장해 놓는다.

   - 만약 지금 vertex의 배열에서의 위치가 최단 거리보다 짧으면 업데이트 시킬 필요 없기 때문에 continue로 아래 과정은 생략하고 다시 반복문을 처음부터 진행한다.

   - 지금 지정된 vertex의 주변 노드들에 대하여 하나씩 순회하면서 다음 과정을 진행

     -  '주변 노드'가 '지정된 vertex + vertex와 주변 노드까지의 거리' 보다 크면  해당 노드의 최단 거리를 초기화(업데이트) 시킨다.

     - ```java
       dist[node] > dist[vertex] + graph.getDistance(vertex, node)
       ```

     - 그리고 해당 노드가 우선 순위 큐의 방식에 의하여 삽입된다.

5. 큐가 비어서 4번 반복문이 종료가 되면 최단 거리 배열의 목적지에 해당하는 index의 데이터 값을 리턴하면서 마무리한다.

<br>

## 관련된 문제

[<u>백준 14567 선수과목 (Prerequisite)</u>](https://www.acmicpc.net/problem/14567)

<br>

## 관련된 Post

- 자료 구조 [<u>트리(Tree)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/Algorithm-CH7.-Tree(%ED%8A%B8%EB%A6%AC)/)
- 자료 구조 [<u>그래프(Graph)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-1.-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)
- [<u>너비 우선 탐색(BFS, Breath-First Search)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-2.-%EB%84%88%EB%B9%84-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(BFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)
- [<u>깊이 우선 탐색(DFS, Depth-First Search)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-3.-%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(DFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)



