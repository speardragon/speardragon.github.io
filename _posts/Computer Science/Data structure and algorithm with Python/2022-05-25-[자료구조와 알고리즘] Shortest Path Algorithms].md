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
  - 그 바로 앞에 있는 노드역시 처음이기 때문에 값을 아직 받은 것이 없어 None로 초기화 한다.
  
  - s.d 즉, source에서 source까지의 distance는 당연히 0이다.(같은 노드이니까)

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
- for문 - {|G.V| - 1 }번 
  - |G.V| -> vertex의 갯수
  - Edge의 집합에 속하는 각 edge마다 한 번씩 relax를 해 준다.
    - 모든 edge를 update
- 이 결과 negative (weight) cycle이 있다면 이상한 결과가 들어가 있을 것이고 그렇지 않으면 shortest path가 구해졌을 것이다.

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

- 처음에 초기화를 한다.
  - source에서 c까지의 distance -> 무한대
  - '' 에서 e, f까지의 distance -> 무한대
- bellman-ford 알고리즘
  - 노드 4개 -> for loop 3번
    - 모든 edge들에 대해서 relax
      - c = 5
      - e 혹은 f로 오는 경로는 없기 때문에 여전히 무한대로 둔다.
      - 현재 얻은 distance가 shortest distance인지를 판단한다.
      - c.d = 5 -> shortest distance
      - e.d, f.d 모두 더 짧은 경로가 존재하지 않기 때문에 negative cycle가 위 그림에는 없다.
      - True 리턴 : 내가 가진 그래프에 negative cycle가 없다.
      - 그러나 e와 f는 negative cycle이다.
      - 이 경우에 negative cycle을 detect 하기 위해서 어떻게 해야 하는가?
      - 각 vertex에서 distance를 100으로 초기화한다.



<br>

- vertex를 모두 100으로 초기화 하면
- c를 먼저 relax 하면 5로 update된다. 
- e를 relax하면 f를 통해 오는 경우 94만에 올 수 있기 때문에 update
- f는 e를 통하면 97만에 올 수 있다.(e가 방금 94로 update 되었기 때문에)
- c는 그대로고 e는 계속해서 내려갈 것이고 이에 따라 f도 계속해서 shortest가 update 될 것이다.
- 그러면 결과적으로 c는 아무리 for문을 돌려도 그대로 일 것이지만
- e는 더 작은 shortest distance가 존재하기 때문에 False가 리턴 될 것이고 negative cycle을 detect 할 ㅜㅅ 있게 되는 것이다.
- 이를 통해 무조건 inf로 초기화하는 것 보다 충분히 큰 값으로 초기화 시키면 detect 할 수 있는 경우가 있다.
  - <mark>edge의 갯수 x weight 의 최댓값</mark>



![image](https://user-images.githubusercontent.com/79521972/170915293-f8b36403-a61b-4b66-b789-1778ea2ed6c3.png)



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

N: vertex의 갯수

M: edge의 갯수

W: negative edge의 갯수

S와 E가 undirected 이어졌다.

M+2 번쨰 줄 부터는 S가 시작, E가 도착이 된다. T는 negative edge



```python
tc = int(input())
for _ in range(tc):
    N, M, W = map(int, input().split())
    road = [[] for _ in range(N+1)]
    
    for _ in range(M):
        S, E, T = map(int, sys.stdin.readline().split())
        road[S].append([E, T])
        road[E].append([S, T])
    #for 
```

dictionary로 하면 안되는 이유? -> 강의 보고 적기

이차원 리스트로 저장 - vertex 갯수 + 1

초기화할 때 값을 무한대로 하면 안되고 

- edge의 갯수 x weight 의 최댓값





---

13-19

## 24.3 Dijkstra's algorithm

![image](https://user-images.githubusercontent.com/79521972/170179459-b74b4a5b-9fb0-4064-ae55-b57ea87096d4.png)

- Bellman-Ford 와 마찬가지로 single-source shortest-paths를 구하는 알고리즘이다.

- 하지만 다른 점은 모든 edge의 weight가 non-negative이다.
  - w(u, v) >= 0

- 다익스트라 알고리즘은 벨만포드보다 running time 이 빠르다.

<br>

- 다익스트라 알고리즘은 set S를 유지한다.
  - source 부터의 shortest path가 이미 결정된 vertex의 집합
  - 알고리즘을 돌리다 보면 이 set S에 한놈씩 추가가 되고
  - 알고리즘이 끝나면 모든 vertex가 set S에 들어가게 된다.
- 알고리즘을 돌리면서 아래와 같은 두 가지 동작을 반복한다.
  - 1. select: source부터의 distance가 현재까지 가장 작은 애를 선택한다. -> 얘가 set S에 추가
  - 2. relax(update): u가 1번에서 고른 vertex인데 여기서 출발하는 모든 vertex를 다시 relax 한다.

- 위 loop가 끝나면 모든 vertex에 대해서 relax가 끝나게 된다.

---

13-20

![image](https://user-images.githubusercontent.com/79521972/170179478-68e858a2-56ed-415a-a0a9-65f3b2251382.png)

- vertex가 5개 있는데 모든 vertex의 s와의 distance는 무한대로 초기화한다.
- loop
  - select: 
    - 가장 작은 distance는 s이기 때문에 s가 select에 들어간다.
  - t와 y에 대해 relax 진행
    - t: s를 통해서 가는 경우 10만에 갈 수 있으니 10으로 update
    - y: s를 통해서 가는 경우 5만에 갈 수 ㅣㅇㅆ다.
  - 이 과정이 끝나고 distance가 가장 작은 y가 set S에 들어간다.
  - y와 가까운 t, x,z에 대해 relax
    - t: y를 통해 8만에 가는 것으로 update
    - x: y를 통해 14만에 가는 것으로 update
    - z: y를 통해 7만에 가는 것으로 update
  - 또 남은 애들(s, y제외; set S에 없는 애들 중에서) 중에서 제일 작은애 set S에 넣는다. -> z
    - x -> 13으로 update
  - set S에 t 대입
    - x -> 9로 update
  - set S에 x 대입 (남은 것이 x밖에 없음)
    - update 사항 없음

<br>

- 이 경우 distance가 가장 작은 애를 update 했는데 만약에 distance가 같은 애들끼리 있으면 어떻게 함?
  - 뭘 먼저 해도 상관 없지만 -> 되도록(무조건 이렇게 해라) 알파벳 순서대로 한다.
- undirected로 되어있는 경우?
  - 모든 방향을 다 고려하여서
  - undirect라고 다를 것은 없다. 기본적인 algorithm은 그대로이다.(select, relax)

---

13-21

![image](https://user-images.githubusercontent.com/79521972/170179529-5d54aa9f-299c-42ba-800d-1f392bd27683.png)

- (bellman-ford와 유사하게) 모든 vertex를 initialize

- set S = 선언 및 초기화
- 현재 까지 distance가 제일 작은 애를 알아내기 위해서 min heap을 사용한다.(key = v.d)
  - Q : min heap
- heap 구조에서 pop을 하면 제일 작은 애가 나온다.
- Q에 남은 원소가 없을 때까지 loop를 돌린다.
  - loop를 돌리면서 Q에서 하나씩 pop을 하고 이를 set S에 넣고 

<br>

- 위 pseudo code를 그대로 쓸 수 없는 이유
  - 큐에 distance와 vertex name을 묶은 정보가 쭉 저장할 것인데
  - 

while loop안 for 문 안에서 relax를 진행하는데 relax는 vertex 당 한 번만 진행한다.

---

13-22

![image](https://user-images.githubusercontent.com/79521972/170179561-e858130c-3d9b-421b-bf6b-31dbb9615c50.png)

- 다익스트라 알고리즘은 일종의 그리디 알고리즘에 해당 된다.
  - 현재까지 알려진 것 중에 가장 작은 노드를 기준으로 진행되어 최선의 선택을 하기 때문에 그렇다.
  - 이 경우 항상 global optimum이 구해지는가? -> 그렇다 하지만 증명이 굉장히 복잡하다.

- time complexity
  - 5번 줄: O(VlogV)
  - 7,8번: O(ElogV)
  - 따라서 O(VlogV + ElogV) 인데 V >= E이기 때문에 
  - 최종적인 다익스트라 알고리즘의 time complexity는
    - O(ElogV)
  - (앞의 벨만포드는 O(EV))

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
    min_q = [(0, source)]	# (distance from source, vertex name)
    
    while min_q: # 17
        # u is selected and added to the set S
        distance_u, u = heappop(min_q)
        selected.add(u)
        
        # w is the weight of edge (u, v)
        # relax (u, v), if v is not selected
        for v, w in graph[u].items(): # 24
            if v not in selected: # selected가 안 된 애들의 경우에만 relax
                if distance[v] > distance[u] + w:
                    distance[v] = distance[u] + w
                    predecessor[v] = u
                    heappush(min_q, (distance[v], v)) #update한 정보를 다시 q에 넣는다.
                    
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

```
{'s': None, 't': 's', 'x': None, 'y': 's', 'z': None}
{'s': 0, 't': 10, 'x': inf, 'y': 5, 'z': inf}
```

node 이름이 string으로 되어있기 때문에 set으로 한 것

integer이면 자동으로 index가 붙기 때문에 그냥 리스트로 하여 메모리를 절약 + 속도 증가

---

13-25

![image](https://user-images.githubusercontent.com/79521972/170180173-cbadecdc-24eb-437a-b587-617aac438db1.png)

[https://www.acmicpc.net/problem/1753](https://www.acmicpc.net/problem/1753)

빨간줄의 의미는 딕셔너리보다 리스트로 하는 것이 더 유리하다는 것을 알려주는 정보이다.

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

set S에 들어가면 selected가 True 가 된다.

0.0인 이유는 inf가 float이기 때문에 type을 맞춰주기 위함.

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

