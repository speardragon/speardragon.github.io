---
layout: single
title: "[자료구조와 알고리즘] Greedy Algorithms"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Greedy Algorithms']
---

<br>



12-2

## 16. Greedy Algorithms

For many optimization problems, using dynamic programming to determine the best choices is overkill; simpler, more efficient algorithms will do. A greedy algorithm always makes the choice that looks best at the moment. That is, <mark>it makes a locally optimal choice in the hope that this choice will lead to a globally optimal solution.</mark>

<br>

Greedy algorithms do not always yield optimal solutions, but for many problems they do. We shall first examine, in Section 16.1, a simple but nontrivial problem, the activity-selection problem, for which a greedy algorithm efficiently computes an optimal solution.

---

12-3

## Review: rod-cutting

The rod-cutting problem is the following. Given a rod of length n inches and a table of prices pi for i = 1,2,..,n, determine the maximum revenue rn obtainable by cutting up the rod and selling the pieces.

![image](https://user-images.githubusercontent.com/79521972/169721392-ea4c7e64-d245-4a23-95a0-323aa0e06017.png)

- Greedy approach 
  - Taking the pieces in order of greatest price per length

![image](https://user-images.githubusercontent.com/79521972/169721446-b5b29ee4-2745-4a2c-81a0-1139aaad026a.png)

---

12-4

## Review: 정수삼각형

![image](https://user-images.githubusercontent.com/79521972/169721456-0291793e-a385-4192-9a1e-7ba87fbf712b.png)

- Greedy approach 
  - At a node, go to the node of max(left child, right child)

---

12-5

## Review: 0-1 knapsack

![image](https://user-images.githubusercontent.com/79521972/169721482-9c2be40e-8d85-4b38-abbd-2a22e8e59ebc.png)

- Greedy approach 
  - Taking the items in order of greatest value per pound

---

12-6

## Fractional knapsack problem

In the fractional knapsack problem, the setup is the same, but the thief can take fractions of items, rather than having to make a binary (0-1) choice for each item.

![image](https://user-images.githubusercontent.com/79521972/169721525-7be22dc2-bd9f-46e1-9dcc-773b60245c30.png)

- For the fractional knapsack problem, the greedy algorithm yields an optimal solution

---

12-7

## 16.2 Element of the greedy strategy

A greedy algorithm obtains an optimal solution to problem by making a sequence of choices. At each decision point, the algorithm makes choice that seems best at the moment. This heuristic strategy does not always produce an optimal solution, but as we saw in the activity-selection problem, sometimes it does.

<br>

How can we tell whether a greedy algorithm will solve a particular optimization problem? No way works all the time, but the greedy-choice property and optimal has these properties, then we are well on the way to developing a greedy algorithm

<br>

- 휴리스틱(heuristic) 또는 발견법이란 불충분한 시간이나 정보로 인하여 합리적인 판단을 할 수 없거나, 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 간편추론의 방법이다.

<br>

---

12-8

**Greedy-choice property**

The first key ingredient is the greedy-choice property: we can assemble a globally optimal solution by making locally optimal (greedy) choices. In other words, when we are considering which choice to make, we make the choice that looks best in the current problem, without considering results from subproblems.

<br>

**Optimal substructure**

A problem exhibits optimal substructure if an optimal solution to the problem contains within it optimal solutions to subproblems. This property is a key ingredient of assessing the applicability of dynamic programming as well as greedy algorithms.

---

12-9

## 16.1 An activity-selection problem

 Suppose we have a set S  = {a<sub>1</sub>, a<sub>2</sub>, ... , a<sub>n</sub>} of n proposed activities that wish to use a resource, such as a lecture hall, which can serve only one activity at a time. Each activity a<sub>i</sub> has a start time s<sub>i</sub> and a finish time f<sub>i</sub>, where 0 <= s<sub>i</sub> <= f<sub>i</sub> < ∞. If selected, activity a<sub>i</sub> takes place during the half-open time interval [s<sub>i</sub>, f<sub>i</sub>). Activities a<sub>i</sub> and a<sub>j</sub> are compatible if the intervals [s<sub>i</sub>, f<sub>i</sub>) and 
[s<sub>i</sub>, f<sub>i</sub>) do not overlap.

<br>

In the activity-selection problem, we wish to select <mark>a maximum-size subset of mutually compatible activities.</mark> We assume that the activities are sorted in monotonically(단조롭게) increasing order of finish time:

![image](https://user-images.githubusercontent.com/79521972/169722112-e7236d1e-ef3b-4631-a69a-10fef1bfaf53.png)

---

12-10

## Making the greedy choice 

What do we mean by the greedy choice for the activity-selection problem? Intuition suggests that we should choose and activity that leaves the resource available for as many other activities as possible. Now, of the activities we end up choosing, one of them must be the first one to finish. Our intuition tells us, therefore, to choose the activity in S with the earliest finish time, since that would leave the resource available for as many of the activities that follow it as possible.

<br>

Furthermore, we have already established that the activity-selection problem exhibits optimal substructure. Let ![image](https://user-images.githubusercontent.com/79521972/169722241-ccf2aeec-142d-46ef-ac63-16246acc9b0c.png) be the set of activities that start after activity a<sub>k</sub> finishes. If we make the greedy choice of activity a1, then S1 remains as the only subproblem to solve.

---

12-11

## Activity-selector

![image](https://user-images.githubusercontent.com/79521972/169722279-4ce827a5-5370-43ea-bdfc-b04547fe45b1.png)

---

12-12

One bit question remains: is our intuition correct? Is the greedy choice-in which we choose the first activity to finish - always part of some optimal solution? The following theorem shows that it is.

<br>

**Theorem 16.1**

Consider any non-empty subproblem Sk, and let am be an activity in Sk with the earliest finish time. Then am is included in some maximum-size subset of mutually compatible activities of Sk.

Thus, we see that although we might be able to solve the activity-selection problem with dynamic programming, we don't need to.

Greedy algorithms typically have his top-down design: make a choice and then solve a subproblem, rather than the bottom-up technique of solving subproblems before making a choice.

---

12-13

## An iterative implementation

```python
def selector(s: list, f: list): # 1
    selected_act = 1		# currently selected activity
    ans = [selected_act]
    
    for act in range(2, len(s)):
        if s[act] >= f[selected_act]:
            selected_act = act
            ans.append(selected_act)
            
    return ans # 10


# s[0] and f[0] are dummis
_s = [0, 1, 3, 0, 5, 3, 5, 6, 8, 8, 2, 12] # 14
_f = [0, 4, 5, 6, 7, 9, 9, 10, 11, 12, 14, 16]

print(selector(_s, _f))

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

---

12-16

![image](https://user-images.githubusercontent.com/79521972/169722598-7c3946c2-1865-4a10-8308-1459f346e95d.png)

---

12-17

![image](https://user-images.githubusercontent.com/79521972/169722618-431e4511-18a6-4845-a738-4710fd40d904.png)

[https://www.acmicpc.net/problem/5585](https://www.acmicpc.net/problem/5585)



