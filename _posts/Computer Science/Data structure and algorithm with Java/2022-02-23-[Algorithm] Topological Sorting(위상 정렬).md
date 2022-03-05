---
layout: single
title: "[Algorithm] Topological Sorting(위상 정렬)"
categories: ['Computer Science', 'Data structures and algorithms with Java', 'Algorithm', 'Graph']
tag: ['Data structure', 'Algorithm', 'Graph', 'Topological Sorting']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

이번 시간에는 위상정렬에 대해서 배워 보도록 할 것이다.

<br>

### 위상 정렬이란?

어떤 일을 하는 순서를 찾는 알고리즘으로 **방향 그래프**에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점(vertex)을 나열하는 것이다.

그래프가 DAG가 아닌 경우 그래프에 대한 위상 정렬은 불가능하다.

- <mark>비순환 방향 그래프</mark>(DAG: Directed Acyclic Graph)에서 정점을 선형으로 정렬하는 것(순서대로 출력)
- 순서가 있는 task에서 순서를 찾아주는 알고리즘

![image](https://user-images.githubusercontent.com/79521972/154828367-4d066c12-3d7c-4455-9f2c-56e4c5d1acfe.png)

그래프 방문 결과가 <u>순서대로</u> 출력 되어야 하기 때문에 그래프의 `시작 지점`과 `방향`이 존재해야 하기 때문에 그래프에 cycle이 있으면 위상 정렬로 sorting하는 것이 불가능함.

리스트와 같은 경우 그 자체가 순서이지만 그래프처럼 비선형적(non-linear)이지만 순서가 있는 구조에서는 이 순서를 찾아주는 알고리즘 자체를 의미.

<br>

**위상 정렬 예시**로는 대학교 선수과목이 있다.

![image](https://user-images.githubusercontent.com/79521972/155874457-ed6a26af-c1d0-438c-8840-d47ff1d64576.png)

위 그림은 우리 과의 선수과목 이수 체계도를 나타낸 것이며 여기서 특정 수강과목에 선수과목이 있다면 그 선수과목부터 수강을 해야 한다. 여기서 위상 정렬은 특정 수강과목을 위해 필요한 선수과목의 정렬이다.

<br>

이 외에도 컴파일 작업순서 결정, git과 같은 버전 히스토리 관리, 교착 상태 탐지 등이 위상 정렬을 사용한다.

<br>

### 구현 방법

#### In-degree 방법(BFS 방식)

[**동작 방식**]

1. 모든 정점마다 in-degree 수를 설정한다.
2. in-degree가 0인 정점은 방문한 것으로 표시하고 큐에 해당 정점을 추가한다.

3. 큐가 빌 때까지 순회하며 다음 작업을 수행한다.
   - 큐의 앞 요소를 dequeue()를 통해 가져와 리스트에 append한다.
   - dequeue()한 정점에 인접 정점 중 방문하지 않은 정점의 indegree를 하나 감소시킨다.
   - in-degree 감소 후 값이 0이면 해당 정점은 queue에 enqueue()하고 방문한 것으로 표시한다.

<br>

그림으로 자세히 보자.

**Queue(큐) 사용**: 진입 차수를 계산하며 sorting하는 방식

- 진입 차수(indegree) - 한 노드에 들어오는 다른 간선의 수

1. 모든 vertex의 indegree 수를 세기
2. 큐에 indegree가 0인 vertex를 삽입
   - 자신을 향해 들어오는 edge가 없으면 출발 노드이기 때문
3. 큐에서 vertex를 꺼내 연결된(나가는 방향, outdegree) edge 제거
4.  3번에서 indegree가 0이 된 vertex를 큐에 삽입(enqueue)
5. 큐가 빌 때까지 3~4 반복

- 만약 cycle이 존재하는 그래프라면, 모든 노드를 돌기 전에 큐가 비기 때문에 정렬이 종료된다.

![image](https://user-images.githubusercontent.com/79521972/154829129-0e7c4668-9444-49ac-a354-a09a308cf880.png)

<br>

#### **위상 정렬이 불가능한 경우**

- 그래프 내 cycle이 존재하는경우(C <-> E)

![image](https://user-images.githubusercontent.com/79521972/154829825-13aa663c-b2a9-444e-bbc9-8cfdd5f8d1d7.png)

cycle에 속하는 vertex가 있다면 해당 vertex는 항상 진입차수 indegree 가 1 이상이기 때문에 큐에 삽입 될 수 없기 때문이다.

<br>

- 처음부터 진입차수가 0인 vertex가 존재하지 않는 경우 

![image](https://user-images.githubusercontent.com/79521972/154829828-a373751e-a74f-44d3-abbd-df7b2c7a6cda.png)

큐에 넣을 수 있는 vertex가 없기 때문에 위상 정렬이 불가능



<br>



### **stack(DFS)**

[**동작 방식**]

DFS 방식에서는 in-degree를 사용하지 않고 재귀 혹은 스택을 사용한다.

1. 모든 정점을 순회하면서 미방문 정점에 대해서 DFS를 수행한다.
2. DFS 수행 방식
   - 하나의 정점에서 시작한다.
   - 방문표시를 하면서 간선을 따라 다음 정점으로 방문한다.
   - 더이상 방문할 간선이 존재하지 않으면 리스트 맨 앞에 정점을 추가하고 백트래킹을 통해 이전 정점으로 되돌아가면서 미방문 간선이 있는지 체크한다.
   - 방문 가능한 간선이 만약 있다면 다시 간선을 따라서 다음 정점으로 이동한다.
   - 모든 정점을 탐색할 때까지 위 과정을 반복한다.

![image](https://user-images.githubusercontent.com/79521972/155874737-88d436c3-1166-433d-a9e8-8f8cd00b3ff2.png)

<br>

이해를 위해 다음 그림을 보자.

- DFS 역순으로 진행

앞에서 진행했던 그래프와 동일한 그래프로 진행

![image](https://user-images.githubusercontent.com/79521972/154830182-402a77d4-8981-41e1-b66d-0f7ae5c8831a.png)

1. **시작은 동일하게 A vertex부터 시작**
   - 다음으로 이어지는 B나 C둘 중 어느 것이라도 상관 없지만 B로 선택
2. **A - B - D - F - E - G 순으로 방문**
3. **이의 역순인 G - E- F - D - B 순으로 스택에 삽입**
   - A노드는 아직 탐색할 노드(C)가 남아있기 때문에 스택에 삽입 하지는 않음
4. **A에서 C로 방문하기 때문에 이를 뒤집은 C -> A순으로 스택에 삽입**
5. **방문을 할 노드가 더 이상 남아있지 않다면 스택에 위에서 부터 순서대로 정렬이 된 것**
   - A - C - B - D - F - E - G

![image](https://user-images.githubusercontent.com/79521972/154830147-3236a64b-3953-48af-82bf-0b0f02b23589.png)

<br>



### 위상 정렬 구현

- queue를 이용한 구현

```java
public class GraphAlogorithms {
    
    public static Lists<Integer> bfs(iGraph, int from) {...}
    
    public static List<Integer> dfs(iGrpah, int from) {...}
    
    //queue를 이용한 방법
    public static List<Integer> topologicalSortIndegree(IGraph graph) {
        //<vertex, indegree 갯수>
        Map<Integer, Integer> indegreeCounter = graph.getIndegrees(); //1
        
        List<Integer> result = new LinkedList<>();
        
        IQueue<Integer> queue = new MyLinkdQueue<>();
        
        //2
        for (int v : graph.getVertexes()) {
            int count = indegreeCounter.getOrDefault(v, 0);
            if (count == 0) {
                queue.offer(v);
            }
        }
        
        while (!queue.isEmpty()) {
            int node = queue.poll();
            result.add(node);
            
            for (int nn : graph.getNodes(node)) {
                if(indegreeCounter.containsKey(nn)) {
                    int count = indegreeCounter.get(nn);
                    if (count - 1 == 0) {
                        queue.offer(nn);
                    }
                    indegreeCounter.put(nn, count - 1);
                }
            }
        }
        return result;
    }    
}
```

parameter: 그래프 타입

return 타입: List < Integer>

1. 모든 vertex의 indegree 수를 세기

2. 큐에 indegree가 0인 vertex(시작 노드) 삽입
   - if문을 통해 indegree 가 0인 것만 큐에 삽입
3. 큐에서 vertex를 꺼내 연결된(나가는 방향) edge 제거
   - 시작 노드와 연결된 노드들을 하나씩 nn에 가져와 indegreecounter에 존재한다면 그 노드의 진입차수에 -1
   - 실제로 edge를 제거하게 되면 그 노드를 향한 진입차수 indegree가 1이 줄어드는 것이기 때문
4. 3번에서 indegree(진입차수)가 0이 된 vertex를 큐에 삽입
5. 큐가 빌 때까지 3~4 반복

<br>

- 스택을 이용한 구현

```java
//Stack 구현 
public static List<Integer> topologicalSort(IGraph graph) {
    List<Integer> result = new ArrayList<>();
    IStack<Integer> stack = new MyStack<>();
    Set<Integer> visited = new HashSet<>();
    
    Set<Integer> vertexes = graph.getVertexes();
    for (Integer vertex : vertexes) {
        if(!visited.contains(vertex)) {
            //dfs
            topologicalSort(graph, vertex, visited, stack);
        }
    }
    
    while (stack.size() > 0) {
        result.add(stack.pop());
    }
    return result;
}

private static void topologicalSort(IGraph graph, int vertex, Set<Integer> visited, Istack<Integer> stack) {
    visited.add(veretex);
    
    List<Integer> nodes = graph.getNodes(vertex);
    for (Integer n : nodes) {
        if(!visited.contains(n)) {
            topologicalSort(graph, n, visisted, stack);
        }
    }
    stack.push(vertex)
}
```

parameter: 탐색 그래프, vertex, visited, stack

- result: 결과를 출력하기 위한 리스트
- stack: DFS 탐색 결과를 저장하기 위한 스택
- visited: 중복 방지를 위해 방문 정보를 저장하는 Set

1. 방문하지 않은 노드들에 한하여 dfs 탐색을 진행
   - 노드와 연결된 노드들에 대하여 만약 방문했던 노드라면 방문하지 않고 방문하지 않은 경우에만 재귀 호출을 통해 DFS탐색을 계속해서 진행
2. DFS가 **종료**되었을 때 stack에 vertex를 삽입
   - 이러한 순서로 vertex를 역순으로 스택에 삽입 가능
   - 만약 DFS 탐색을 하기 전 stack에 push를 한다면 시작 노드가 스택에 맨 처음에 삽입(정방향) 되는 경우가 생겨버림

3. 스택이 빌 때까지 stack에 있는 데이터를 하나씩 출력



<br>

## 관련된 Post

- 자료 구조 [<u>트리(Tree)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/tree/Algorithm-CH7.-Tree(%ED%8A%B8%EB%A6%AC)/)
- 자료 구조 [<u>그래프(Graph)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-1.-Graph(%EA%B7%B8%EB%9E%98%ED%94%84)/)
- [<u>너비 우선 탐색(BFS, Breath-First Search)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-2.-%EB%84%88%EB%B9%84-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(BFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)
- [<u>깊이 우선 탐색(DFS, Depth-First Search)</u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-CH9-3.-%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(DFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)























