---
layout: single
title: "[자료구조와 알고리즘] Shortest Path Algorithms"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Shortest Path Algorithms']
---

<br>

# 13. Shortest Path Algorithms

13-2 

## Single-Source Shortest Paths

In a shortest-paths problem, we are in given a **weighted**, directed graph G = (V, E), 
with **weight function** ![image](https://user-images.githubusercontent.com/79521972/170176160-90ec4724-0977-4ee6-8986-272a5c5ce0ee.png) mapping edges to real-valued weights. (각 edge마다 주어지는 path 값)

The **weight w(p) of path** ![image](https://user-images.githubusercontent.com/79521972/170176223-bae160b8-1367-44f5-8b69-42d7995b530d.png) is the <mark>sum of the weights of its constituent edges</mark>:

![image](https://user-images.githubusercontent.com/79521972/170176273-fbc88005-e5b9-45b4-8a29-ef4181d24ebd.png)

We define the shortest-path weight ![image](https://user-images.githubusercontent.com/79521972/170176307-bb59afbf-7f7e-4857-a314-0cee34df23f7.png) from u to v by

![image](https://user-images.githubusercontent.com/79521972/170176346-467c4514-78a0-445b-86e2-a2d558f175de.png)

A **shortest path** from vertex u to vertex v is then defined as any path p with weight ![image](https://user-images.githubusercontent.com/79521972/170176387-09e46dc4-3748-4906-bc9b-e882d1761a5d.png)

---

13-3

## Optimal substructure of a shortest path

Shortest-paths algorithms typically rely on the property that a shortest path <u>between two vertices contains</u> <u>other shortest paths within it</u>.

Recall that optimal substructure is one of the key indicators that dynamic programming (Chapter 15.) and the greedy method (Chapter 16) might apply.

shortest path는 그 안에 존재하는 sub path에서도 shortest path인 구조이다.

![image](https://user-images.githubusercontent.com/79521972/170176866-0701837a-593a-4747-9fb7-bc26d7c5a248.png)

주어진 가중, 방향성 그래프에서 weight function w에 대해서 p가 vo에서 vk까지의 최단 경로가 되도록 하려고 하면, 0<i<j<k를 만족하는 어떠한 i와 j에 대해서 vi에서 vj로 가는 p의 subpath가 되도록 하자. 

그때 pij는 vi에서 vj까지 가는 최단 경로이다.

---

13-4

## Negative-weight edges

![image](https://user-images.githubusercontent.com/79521972/170176922-e435998b-bdd8-466b-9e23-f828d82b54cd.png)

- c<->d 는 cycle인데 한 번 갔다가 다시 오면 cost 3이 든다.

- e<->f 역시도 cycle인데 한 번 갔다가 다시 오면 cost -1이 된다. -> negative-weight cycle

- 그런데 이 negative-weight cycle이 있으면 shortest path가 존재할 수 없는데 그 이유는 shortest path는 최소한의 cost가 드는 거리를 찾는 것인데 negative cycle을 발견하면 cost를 계속해서 줄이려고 하기 때문에 그 loop를 빠져나가지 않게 될 것이고 shortest path는 결국 negative infinity가 될 것이다.

- s에서 시작해서 h, i, j 까지 갈 수있는 연결된 edge가 없기 때문에 해당 노드들은 무한대의 값을 가지게 된다.
  - 이 노드들에게 가려면 무한대의 path를 가도 갈 수 없기 때문에 무한대로 한것

---

13-5

## Representing shortest paths

Given a graph G = (V, E), we maintain for each vertex v ∈ V a **predecessor(선임, 이전의)** v.*π* that is is either another vertex of NIL.

- 그래서 v.*π*는 v 노드까지 왔을 때 바로 앞 노드를 말하고
  - t.*π*  = s
- 각 vertex에서는 계속해서 predecessor를 관리해 준다.
  - predecessor는 경로마다 바뀔 수 있다.

![image](https://user-images.githubusercontent.com/79521972/170177273-0b478141-5c70-4bfe-a4cf-ed3080379dc4.png)

(a)에 대한 shortest path를 구해 보면 (b)처럼 될 수도 (c)처럼 될 수도 있다.

---

13-6

## Initialize-Single-Source

For each vertex v ∈ V, we maintain an attribute v.d, which is an upper bound on the weight of a shortest path from source s to v. We call v.d a shortest-path estimate. We initialize the shortest-path estimates and predecessors by the following ![image](https://user-images.githubusercontent.com/79521972/170177462-601c7941-2e40-4ff9-84bb-912a621c0a0b.png)-time procedure:

![image](https://user-images.githubusercontent.com/79521972/170177498-b950d302-4aaa-4b16-b0bf-caaa18fb0505.png)

- G는 내가가지고 있는 graph
- V는 vertex의 집합
- v.d 는 shortest-path estimate이다.
  - source에서 모든 vertex까지의 추정되는 shortest distance 값이므로 처음에는 모두 무한대로 초기화한다.
  - s.d 즉, source에서 source까지의 distance는 당연히 0이다.(같은 노드이니까)
- 그 바로 앞에 있는 노드역시 처음이기 때문에 값을 아직 받은 것이 없어 None로 초기화 한다.



---

13-7

## Relaxation

shortest path를 찾기 위해서 공통적으로 사용하는 방법

The process of **relaxing an edge (u, v)** consists of testing whether we can improve the shortest path to v found so far by going through u and, if so, updating v.d and v.*π* . A relaxation step may decrease the value of the shortest-path estimate v.d and update v's predecessor attribute v.*π*.

![image](https://user-images.githubusercontent.com/79521972/170177716-d41110ac-c539-4697-9674-630836c66677.png)

- 현재까지 알려진 u까지의 거리 - u.d
- 현재까지 알려진 v까지의 거리 -  v.d
- relax한다는 것은 u 를 update 한다는 것이다.
  - 현재 v의 경로 보다 u를 들리면 더 짧은 경로가 있는지를 확인하고 더 짧다면 그 경로로 update한다는 것
- 

---

13-8

## Relxation

![image](https://user-images.githubusercontent.com/79521972/170177765-be7ce602-30f2-4f2b-a003-3b8e9eb9b6ba.png)

Each algorithm in the chapter calls Initialize-single-source and then repeatedly relaxes edges. Moreover, <mark>relaxation is the only means by which shortest-path estimates and predecessors change.</mark> 

The algorithms in the chapter differ in how many times they relax each edge and the order in which they relax edges. 

Dijkstra's algorithm and the shortest-paths algorithm for directed acyclic graphs relax **each edge exactly once**. 

The Bellman-Ford algorithm relaxes **each edge |V| ㅡ 1 times.**

---

13-9

## 24.1 The Bellman-Ford algorithm

![image](https://user-images.githubusercontent.com/79521972/170178173-481702e0-472d-4e02-b3a5-4d6f32b72acc.png)

- Bellman-Ford의 장점은 edge의 weight가 negative여도 동작한다는 것이다.
  - 다익스트라는 동작 안 함.
- 가장 먼저 **negative cycle**이 있는지를 판단해야 함.
  - 만약 있다면 algorithm을 돌려도 solution이 구해지지 않는다.

---

13-10

![image](https://user-images.githubusercontent.com/79521972/170193534-6946bb44-ec0f-44eb-87de-622eabebd8e2.png)

- Initialize
- for문 - |G.V| - 1 번 
  - |G.V| -> vertex의 갯수
  - Edge의 집합에 속하는 각 edge마다 한 번씩 relax를 해 준다.
- 이 결과 negative cycle이 있다면 이상한 결과가 들어가 있을 것이고 그렇지 않으면 shortest path가 구해졌을 것이다.

![image](https://user-images.githubusercontent.com/79521972/170193554-ed2a7cd4-2c2a-4390-809e-f564bbaf4c79.png)





---

13-11

중요!!

![image](https://user-images.githubusercontent.com/79521972/170178238-1bc3166a-f4cd-48dd-9266-5b8a0556d5c7.png)

초기에는 모든 노드에 무한대가 들어있기 때문에 s에서 시작하는 것만 update 될 수 있다.

각 edge를 무슨 순서대로 relax 하는 지는 중요하지 않다.

결과적으로 source에서 dest 노드까지 가면서 거치는 노드들에 대한 각각의 shortest path에 대한 정보 (v.d, v.ㅠ)가 각자 다 갖고 있게 될 것이다.

```
# 업데이트가 어떤 식으로 되는지 한 번 그려보기


```



---

13-12

![image](https://user-images.githubusercontent.com/79521972/170178266-d8781ec5-a013-470d-98bc-abccedc57581.png)

- 앞서 |V|-1 만큼 for loop를 돌려서 얻어낸 결과가 shortest path인지 아니면 strange value인지를 알아내는 작업이 필요한데 이는 위의 코드와 같이 작성하면 된다.
  - 모든 edge에 대해서 반복하면서
  - **v.d > u.d + w(u, v)** 이면 즉, u를 통하면 더 빨리 갈 수 있는 경우가 어떤 edge에서 존재한다고 판단 되었다는 것은 bellman-Ford 알고리즘이 제대로 작동하지 않았다는 것을 의미하기 때문에 이 경우는 **negative cycle**이 있다고 유추할 수 있는 것이다.
  - 그게 아니라면 제대로 구해진 것이기 때문에 True를 리턴
- O(E(V-1)) => O(EV)

---

13-13

```python
def bellamn_ford(grpah, source): # 1
    # intialize
    distance = {} # v.d
    predecessor = {} # v.ㅠ
    for node i in graph.keys(): # 5
        distance[node] = float('inf')
        predecessor[node] = None
    distance[source] = 0
    
    # relax all edges for V-1 times
    for _ in range(len(graph) - 1): # 11
        for u in graph: # 이중 dictionary 일 것이기 때문에 u도 dictionary임
            # w is the weight of edge (u, v)
            for v, w in graph[u].items(): # 14
                if distance[v] > distance[u] + w:
                    distance[v] = distance[u] + w
                    predecessor[v] = u
                    
	for u in graph: # 20
        for v, w in grpah[i].items():
            if distance[v] > distance[u] + w:
                return False, distance, predecessor
            
    return True, distance, predecessor


Fig24_4 = { # 28; 앞에 있는 그래프 그림에 대한 adjacency list
    's': {'t': 6, 'y': 7},
    't': {'x': 5, 'y': 8, 'z': -4},
    'x': {'t': -2},
    'y': {'x': -3, 'z': 9},
    'z': {'s': 2, 'x': 7}
}
check, dist, pre = bellman_ford(Fig24_4, 's') # 35; graph와 source 노드를 넘겨줌
if check:
    print(pre)
    print(dist)
```

![image](https://user-images.githubusercontent.com/79521972/170179063-21ac162b-faed-41bf-b161-680c58855671.png)



---

```python
def bellamn_ford(grpah, source): # 1
    # intialize
    distance = {} # v.d
    predecessor = {} # v.ㅠ
    for node i in graph.keys(): # 5
        distance[node] = float('inf')
        predecessor[node] = None
    distance[source] = 0
```

for문을 한 번씩 돌 때마다 distance가 어떻게 되는지 한 번 찍어보기





---

13-15

## Detecting Negative-Weight Cycles in a Disconnected Graph

![image](https://user-images.githubusercontent.com/79521972/170179127-de736de3-dcbe-44fb-af71-0009208fda7a.png)

---

13-16

![image](https://user-images.githubusercontent.com/79521972/170179208-dc36fc66-153c-4de4-a911-dd688f251c90.png)



[https://www.acmicpc.net/problem/1865](https://www.acmicpc.net/problem/1865)



---

13-17

![image](https://user-images.githubusercontent.com/79521972/170179261-cd5f783a-bc54-4178-9427-773211289306.png)



---

13-18

![image](https://user-images.githubusercontent.com/79521972/170179271-f29f2c91-2c30-44f5-8476-8e97db1fe7ec.png)



```python
tc = int(input())
for _ in range(tc):
    N, M, W = map(int, input().split())
    road = [[] for _ in range(N+1)]
    
    for _ in range(M):
        S, E, T = map(int, sys.stdin.readline().split())
        road[S].append([E, T])
        road[E].append([S, T])
```



---

13-19

## 24.3 Dijkstra's algorithm

![image](https://user-images.githubusercontent.com/79521972/170179459-b74b4a5b-9fb0-4064-ae55-b57ea87096d4.png)





---

13-20

![image](https://user-images.githubusercontent.com/79521972/170179478-68e858a2-56ed-415a-a0a9-65f3b2251382.png)



---

13-21

![image](https://user-images.githubusercontent.com/79521972/170179529-5d54aa9f-299c-42ba-800d-1f392bd27683.png)



---

13-22

![image](https://user-images.githubusercontent.com/79521972/170179561-e858130c-3d9b-421b-bf6b-31dbb9615c50.png)



---

13-23

```python
from heapq import heappop, heappush # 1
import math


def dijkstra(graph, source):
    # initialize
    distance = {}
    predecessor = {}
    for node in grpah.keys(): # 9
        distance[node] = math.inf
        predecessor[node] = None
    distance[source] = 0
    
    selected = set()		# the set S
    min_q = [(0, source)]	# (distance from source, predecessor)
    
    while min_q: # 17
        # u is selected and added to the set S
        distance_u, u = heappop(min_q)
        selected.add(u)
        
        # w is the weight of edge (u, v)
        # relax (u, v), if v is not selected
        for v, w in graph[u].items(): # 24
            if v not in selected:
                if distance[v] > distance[u] + w:
                    distance[v] = distance[u] + w
                    predecessor[v] = u
                    heappush(min_q, (distance[v], v))
                    
	return distance, predecessor


Fig24_6 = { # 34
    's': {'t': 10, 'y': 5},
    't': {'x': 1, 'y': 2},
    'x': {'z': 4},
    'y': {'t': 3, 'x': 9, 'z': 2},
    'z': {'s': 7, 'x': 6}
}
dist, pre = dijkstra(Fig24_6, 's') # 41
print(pre)
print(dist)
```



---

13-25

![image](https://user-images.githubusercontent.com/79521972/170180173-cbadecdc-24eb-437a-b587-617aac438db1.png)

[https://www.acmicpc.net/problem/1753](https://www.acmicpc.net/problem/1753)



---

13-26

![image](https://user-images.githubusercontent.com/79521972/170180212-b7275480-402f-46c9-8423-7b7445c633a8.png)

```python
def dijkstra(graph, source):
    num = len(graph) - 1
    distance = [math.inf] * (num+1)
    distance[source] = 0
    
    selected = [False] * (num+1)
    min_q = [(0.0, source)]
    
```



---

13-27

## 22.4 Topological sort

![image](https://user-images.githubusercontent.com/79521972/170180400-cfbc2b07-0b49-4890-ac49-e9d34a02b603.png)





---

13-28

![image](https://user-images.githubusercontent.com/79521972/170180433-a38cff03-10d3-4b09-ba0f-1c3b282aa3e5.png)



---

13-29

```python
from collection import deque # 1
import sys

items = ['undershorts', 'pants', 'belt', 'shirt', 'tie', 'jacket', 'socks',
        'shoes', 'watch']
num = len(items)

# adjacency list # 8
graph = [[1, 7], [2, 7], [5], [2, 4], [5], [], [7], [], []]

# compute indegree of each node
indegree = [0] * num # 12
for i in range(num):
    for j in range(len(graph[i])):
        indegree[graph[i][j]] += 1
        
# enqueue the nodes of degree 0 
q = deque() # 18
for i in range(num):
    if indegree[i] == 0:
        q.append[i]
        
while len(q) > 0: # 23
    i = q.popleft()	# process a node(item)
    print(items[i])
    
	for j in range(len(graph[i])): # process adjacent nodes
        adj = graph[i][j] # 28
        indegree[adj] -= 1
        if (indegree[adj] == 0):
            q.append(adj)
```



---

13-31

![image](https://user-images.githubusercontent.com/79521972/170181007-23fef67c-c06d-49e0-b153-f335c16d5a5e.png)



---

13-32

![image](https://user-images.githubusercontent.com/79521972/170181025-9c7fa3b5-55a1-464c-b3f0-6804968efbfa.png)

[https://www.acmicpc.net/problem/2252](https://www.acmicpc.net/problem/2252)



---

13-33

```python
from collections import deque # 1
import sys

sys.stdin = open('bj2252.txt', 'r')
N, M = map(int, input().split())

# build the adjacency list # 7
graph = [[] for _ in range(N + 1)]
for _ in range(M):
    a, b = map(int, sys.stdin.readling().split())
    graph[a].append(b)
    
# compute indegree of each node # 13
```

