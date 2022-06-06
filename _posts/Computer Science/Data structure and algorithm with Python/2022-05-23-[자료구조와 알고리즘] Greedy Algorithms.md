---
layout: single
title: "[자료구조와 알고리즘] Greedy Algorithms"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Greedy Algorithms']
---

<br>



12-2

## 12. Greedy Algorithms

For many **optimization problems**, using dynamic programming to determine the best choices is overkill; simpler, more efficient algorithms will do. A **greedy algorithm** always makes the choice that looks best at the moment. That is, <mark>it makes a locally optimal choice in the hope that this choice will lead to a globally optimal solution.</mark>

- 현재 상황에서 가장 좋아보이는 선택을 한다.
- local optimum
  - 전체가 어떻게 생겼는지는 모르지만 현 상황에서 오른쪽에서는 올라가고 왼쪽에서는 내려간다.
  - 그렇다면 현 상황에서는 오른쪽으로 가는 것이 최선의 선택이고 가다가 기울기가 0인 지점이 극댓값 즉, local optimum이라고 한다.
  - 이 local optimum이 global optimum이 되기를 바라고 하는 것이 그리디 알고리즘
- global optimum
  - 전체 그래프 상에서 최댓값을 global optimum이라고 한다.
- greedy algorithm을 적용할 수 있는 상황은 local optimum을 찾았을 때 그것이 global optimum이 될 수 있는 경우이다. (예를 들어, 위로 볼록한 이차 함수의 그래프 y = - x<sup>2</sup>)

<br>

**Greedy algorithms do not always yield optimal solutions**, but for many problems they do. 

We shall first examine, in Section 16.1, a simple but nontrivial problem, the activity-selection problem, for which a greedy algorithm efficiently computes an optimal solution.

- 항상 optimal solution을 제공하지는 않는다.
- 하지만 많은 경우에 써 먹을 수 있다.

---

12-3

## Review: rod-cutting

The rod-cutting problem is the following. Given a rod of length n inches and a table of prices pi for i = 1,2,..,n, determine the <mark>maximum revenue</mark> r<sub>n</sub> obtainable by cutting up the rod and selling the pieces.

![image](https://user-images.githubusercontent.com/79521972/169721392-ea4c7e64-d245-4a23-95a0-323aa0e06017.png)

- Greedy approach 
  - Taking the pieces in order of greatest price per length
    - 위 정보를 통해 얻을 수 있는 정보는 greatest price per length가 있다.
  
  - ![image](https://user-images.githubusercontent.com/79521972/169721446-b5b29ee4-2745-4a2c-81a0-1139aaad026a.png)
  - 4인치 짜리의 rod가 있을 때 이를 2인치 두 개로 나누었을 때 가장 높은 가격을 얻을 수 있는데 greedy로 생각해 보면 현재 가진 정보를 이용해서 최적의 결과를 내도록 하는 것이기 때문에 3인치와 1인치를 합쳤을 때가 최적의 결과가 된다. 하지만 이것은 global optimum이 아니라 local optimum이 되어버린다.
  - 8인치 짜리를 봤을 때는 6인치를 사용했을 때 가장 최적의 선택이 되는데 실제로 봤을 때도 6+2로 나누는 것이 제일 가격이 높으므로 이 상황이 global optimum이 된다.
  - 하지만 local optimum이 나올 수 있다는 경우가 있기 때문에 greedy algorithm 을 사용하지 못하는 case의 문제이다.
  - dynamic programming으로 했을 때는 느리더라도 무조건 찾을 수 밖에 없다.





---

12-4

## Review: 정수삼각형

![image](https://user-images.githubusercontent.com/79521972/169721456-0291793e-a385-4192-9a1e-7ba87fbf712b.png)

- 내가 현재 7에 있고 3이나 8로 갈 수 있다.
- 뒤에 어떻게 될지는 모르지만 현재의 최선의 선택을 8로 가는 것이다.
- 이러한 greedy algorithm을 통한 path는 다음과 같이 된다.



- 이런 경우에 합이 28이다.
- 하지만 global optimum은 30이기 때문에 greedy algorithm을 적용하지 못한다.
- 따라서 이런 경우도 모든 case를 다 뒤져봐야 하기 때문에 dynamic programming을 사용하면 해결 가능하다.

<br>

- Greedy approach 
  - At a node, go to the node of <mark>max(left child, right child)</mark>

---

12-5

## Review: 0-1 knapsack

![image](https://user-images.githubusercontent.com/79521972/169721482-9c2be40e-8d85-4b38-abbd-2a22e8e59ebc.png)

- (a)에서 무게당 가격을 보면 다 다르다. 
- greedy algorithm을 적용하면 다음과 같다.
- item1부터 쭉 보면서 
- dynamic programming을 적용하면 (b)의 첫 번째 경우가 되기 때문에 global optimum을 반드시 찾을 수 있는데 greedy algorithm을 적용하면 (b)의 나머지 경우가 되기 때문에 local optimum이 구해지기 때문에 이 문제도 풀 수가 없다.



- Greedy approach 
  - Taking the items in order of greatest value per pound
  - 단위 무게당 가격이 높으면 집어 넣는다(현 상황에서 최적의 선택)

---

12-6

## Fractional knapsack problem

In the fractional knapsack problem, the setup is the same, but the thief can take fractions of items, rather than having to make a binary (0-1) choice for each item.

![image](https://user-images.githubusercontent.com/79521972/169721525-7be22dc2-bd9f-46e1-9dcc-773b60245c30.png)

- For the fractional knapsack problem, the greedy algorithm yields an optimal solution
- fractional knapsack problem의 경우는 item을 나눌 수 있기 때문에 무조건 단위무게당 가격이 높은 애들을 집어 넣는 식으로 진행 하는 것이 greedy algorithm이기 때문에 이 경우에는 global optimum을 찾을 수 있게 되고 또한 <mark>dynamic programming과 비교했을 때 무척 빠르게 해결할 수 있게 된다.</mark>

---

12-7

## 16.2 Element of the greedy strategy

A greedy algorithm obtains an optimal solution to problem by making a sequence of choices. <mark>At each decision point, the algorithm makes choice that seems best at the moment.</mark> This heuristic strategy does not always produce an optimal solution, but as we saw in the activity-selection problem, sometimes it does.

<br>

How can we tell whether a greedy algorithm will solve a particular optimization problem? No way works all the time, but the **①greedy-choice property** and **②optimal substructure** are the two key ingredients. 

If we can demonstrate that the problem has these properties, then we are well on the way to developing a greedy algorithm

<br>

- **휴리스틱**(heuristic) 또는 발견법이란 불충분한 시간이나 정보로 인하여 합리적인 판단을 할 수 없거나, 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 **간편추론의 방법**이다.

<br>

---

12-8

**Greedy-choice property**

The first key ingredient is the **greedy-choice property**:

we can assemble a globally optimal solution by making locally optimal (greedy) choices. In other words, when we are considering which choice to make, we make the choice that looks best in the current problem, without considering results from subproblems.

- 즉, 이전의 선택이 이후의 선택에 영향을 주지 않는다.

<br>

**Optimal substructure**

A problem exhibits optimal substructure if an optimal solution to the problem contains within it optimal solutions to subproblems. This property is a key ingredient of assessing the applicability of dynamic programming as well as greedy algorithms.



위 두 조건이 만족되면 greedy algorithm을 적용할 수 있는 것이다.(It means global optimum을 찾을 수 있다.)

---

12-9

## 16.1 An activity-selection problem

 Suppose we have a set S  = {a<sub>1</sub>, a<sub>2</sub>, ... , a<sub>n</sub>} of *n* proposed activities that wish to use a resource, such as a lecture hall(회의실), which can serve only one activity at a time. Each activity a<sub>i</sub> has a **start time** s<sub>i</sub> and a **finish time** f<sub>i</sub>, where 0 <= s<sub>i</sub> <= f<sub>i</sub> < ∞. If selected, activity a<sub>i</sub> takes place during the half-open time interval [s<sub>i</sub>, f<sub>i</sub>). Activities a<sub>i</sub> and a<sub>j</sub> are compatible if the intervals [s<sub>i</sub>, f<sub>i</sub>) and 
[s<sub>i</sub>, f<sub>i</sub>) do not overlap.

- n 개의 회의가 잡혀있다고 생각
- i번의 회의는 각각 시작시간과 종료 시간이 존재한다.
- 회의실 하나를 사용하기 때문에 스케쥴링을 잘해서 가급적 많은 갯수의 회의를 할 수 있도록 해야하고 최대 회의 횟수가 이 경우의 global optimum 값이 되는 것이다.

<br>

In the activity-selection problem, we wish to select <mark>a maximum-size subset of mutually compatible activities.</mark> We assume that the activities are sorted in monotonically(단조롭게) increasing order of finish time:

![image](https://user-images.githubusercontent.com/79521972/169722112-e7236d1e-ef3b-4631-a69a-10fef1bfaf53.png)

- 2번 회의는 3시에 시작해서 5시에 끝나는데 5시 쪼금 전에 끝나기 때문에 range가 다음과 같이 된 것이다.
  - [s<sub>i</sub>, f<sub>i</sub>)
- 그렇기 때문에 2번 회의가 끝난 다음에는 4번 회의가 가능한 것이다.
- 끝나는 시간을 기준으로 회의가 sorting 되어 있음.
- 제일 일찍 끝나는 회의인 1번 회의를 집어 넣는다.
  - 그러면 2번, 3번은 겹치지 때문에 집어 넣을 수 없다.
  - 그러면 1번 회의가 끝나고 나서 시작하는 회의 중에서 가장 빨리 시작할 수 있는 회의를 골라야 한다.
  - 4번 가능
  - 4번은 7시에 끝나기 때문에 7시 이후에 시작하는 회의 중에서 가장 빨리 시작하는 회의인 8번 회의가 가능하고
  - 또 8번 회의가 끝나는 11시 이후에 시작하는 회의 중에서 가장 빨리 시작하는 회의는 11이기 때문에
  - 가장 많은 회의를 하려면 1, 4, 8, 11 번 순서대로 회의를 진행하면 된다.



물론 이 문제도 모든 경우를 다 뒤져서 최댓값을 찾는 dynamic programming으로도 풀 수 있지만 greedy algorithm으로도 풀 수 있다.

- greedy: 현재 available한 resource에 대해서 회의 시간이 빨리 끝나는 애부터 집어 넣으면 최대한 많이 넣을 수 있을 것이다.(회의 시간이 적은 회의 부터) -> 이것이 greedy의 intuition

---

12-10

## Making the greedy choice 

What do we mean by the greedy choice for the activity-selection problem? Intuition suggests that we should choose and activity that leaves the resource available for as many other activities as possible. 

Now, of the activities we end up choosing, one of them must be the first one to finish. Our intuition tells us, therefore, to choose the activity in S with the earliest finish time, since that would leave the resource available for as many of the activities that follow it as possible.

<br>

Furthermore, we have already established that the activity-selection problem exhibits **optimal substructure**. Let ![image](https://user-images.githubusercontent.com/79521972/169722241-ccf2aeec-142d-46ef-ac63-16246acc9b0c.png) be the set of activities that start after activity a<sub>k</sub> finishes. 

If we make the greedy choice of activity a1, then S1 remains as the only subproblem to solve.

- S0: 0번 activity가 끝난 후에 시작되는 모든 회의의 집합(전체 집합)
  - S0 = a1 + S1
- -> 그 중에서 가장 빨리 시작하는 1번 회의 선택
- S1: 1번 회의가 끝난 후에 시작되는 모든 회의에 대한 집합

---

12-11

## Activity-selector

![image](https://user-images.githubusercontent.com/79521972/169722279-4ce827a5-5370-43ea-bdfc-b04547fe45b1.png)

---

12-12

One big question remains: 
is our intuition correct? Is the **greedy choice **- in which we choose the first activity to finish - always part of some optimal solution? The following theorem shows that it is.

<br>

**Theorem 16.1**

Consider any non-empty subproblem S<sub>k</sub>, and let a<sub>m</sub> be an activity in S<sub>k</sub> with the earliest finish time. Then am is included in some maximum-size subset of mutually compatible activities of S<sub>k</sub>.

Thus, we see that although we might be able to solve the activity-selection problem with dynamic programming, we don't need to.

**Greedy algorithms** typically have his **top-down design**: make a choice and then solve a subproblem, rather than the bottom-up technique of solving subproblems before making a choice.

- greedy를 써도 될 지 말지가 고민 되면 그냥 dynamic programming으로라도 해야 한다.



---

Theorem: 정리 공리

Lemma: 보조 정리

Corollary: 따름 정리

- Lemma에 의해서 어떠한 성질이 있음을 유추하는 -> 그래서 증명이 없음(theorem에서 결론이 났기 때문)

---

12-13

## An iterative implementation

```python
def selector(s: list, f: list): # 1
    selected_act = 1		# currently selected activity
    ans = [selected_act]
    
    for act in range(2, len(s)): # 5
        if s[act] >= f[selected_act]:
            selected_act = act
            ans.append(selected_act)
            
    return ans # 10


# s[0] and f[0] are dummis
_s = [0, 1, 3, 0, 5, 3, 5, 6, 8, 8, 2, 12] # 14
_f = [0, 4, 5, 6, 7, 9, 9, 10, 11, 12, 14, 16]

print(selector(_s, _f))

```

- def selector(s: list, f: list)
  - 이 함수를 호출을 할 때 두 개의 parameter를 전달해야 하는데 두 값이 지정한 형식대로 받아야 한다는 것을 의미.
- 1번 act는 무조건 선택
  - ans = [1]
- for문을 통해 2번 act부터 11번 act까지 돌면서
  - 시작시간과 종료시간을 잘 맞물리게 하여(si >= fk 인 경우 -> 그 act를 answer list에 집어 넣음)

- 실행 결과

```
[1, 4, 8, 11]
```



---

12-14

![image](https://user-images.githubusercontent.com/79521972/169722542-a777ab6c-80dd-4bab-b785-70f26deb7095.png)

[https://www.acmicpc.net/problem/1931](https://www.acmicpc.net/problem/1931)

---

12-15

![image](https://user-images.githubusercontent.com/79521972/169722564-c694ffb9-017b-4c1e-84ed-545d68d76c0a.png)

```python
n = int(input())
acts = []
for _ in range(n):
    a, b = map(int, sys.stdin.readline().split())
    acts.append((a,b))
   
```

- 위를 제출하면 틀렸다고 나옴
- 함정
  - 예제 입력이 주어지는데 입력 데이터가 끝나는 시간으로 sorting이 되어있다는 조건이 문제에 붙지 않았기 때문에 저대로 돌리면 틀리게 되는 것이다.
- 그러면?
  - sort() 와 lambda를 이용해서 끝나는 시간을 기준으로 sorting하여 앞의 greedy 코드와 같이 돌리면 됨
- 또 하나의 함정
  - 12-9페이지를 잘 보면 i번째의 회의 경우에 시작시간과 끝나는 시간의 크기가 명시되어 있다.
  - 하지만 이 문제에서는 그런게 없기 때문에 시작하자마자 끝나는 경우가 있을 수도 있다.

- sort()를 할 때
  - 먼저 끝나는 시간을 기준으로 sorting을 하고 끝나는 시간이 같은 경우에는 시작 시간을 기준으로 다시 sorting해야 한다.
  - sort(acts, lambda x: (x[1], x[0])
- 그래서 예제 입력 2의 경우 답이 [1,2,3] -> 3이다.

```python
import sys


def selector(a_list):
    selected_act = 0
    result = [selected_act]

    for act in range(1, len(acts)):
        if a_list[act][0] >= a_list[selected_act][1]:
            selected_act = act
            result.append(selected_act)

    return len(result)


sys.stdin = open('bj1931_in.txt', 'r')
input = sys.stdin.readline

n = int(input())
acts = []

for _ in range(n):
    a, b = map(int, input().split())
    acts.append((a, b))

acts.sort(key=lambda x: (x[1], x[0]))
print(acts)
print(selector(acts))

```



```
[(1, 4), (3, 5), (0, 6), (5, 7), (3, 8), (5, 9), (6, 10), (8, 11), (8, 12), (2, 13), (12, 14)]
4
```




---

12-16

![image](https://user-images.githubusercontent.com/79521972/169722598-7c3946c2-1865-4a10-8308-1459f346e95d.png)

---

12-17

![image](https://user-images.githubusercontent.com/79521972/169722618-431e4511-18a6-4845-a738-4710fd40d904.png)

[https://www.acmicpc.net/problem/5585](https://www.acmicpc.net/problem/5585)

<br>

- 손님이 380원을 샀고 1000을 냈으면 거스름 돈을 620원을 주어야 하는데 이 때 동전의 갯수를 최소한으로 주는 경우 몇 개의 동전을 줄 수 있는가? 를 찾아내는 문제
- dynamic programming으로 모든 경우를 다 뒤져서 가장 작은 경우를 찾을 수 있겠지만 greedy로 하면 빠르게 찾을 수 있다.
- 쓸 수 있는 동전 중에서 큰 단위의 동전을 최우선으로 줄 수 있는 만큼 주면 된다.
  - 500원으로 줄 수 있는만큼 다 주고
  - 100원으로 줄 수 있는 만큼 다 주고
  - 10원짜리로 다 채우면 
  - 그 때의 동전을 count 한 것이 답.

```python
cost = 1000 - int(input())

coin = [500, 100, 50, 10, 5, 1]

count = 0
for i in coin:
    count += cost // i
    cost %= i

print(count)

```

```
4
```

