---
layout: single
title: "[자료구조와 알고리즘] Dynamic Programming"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Dynamic Programming']
---

<br>

# Dynamic Programming

greedy algorithm과 dynamic programming은 굉장히 많이 쓰이는 알고리즘이다.

---

11-2

## Fibonacci series

Let's consider and example to understant how dynamic programming works. We use the Fibonacci series to illustrate

The sequence of numbers
![image](https://user-images.githubusercontent.com/79521972/168503404-9961b12a-10f9-48ce-a26d-331fbdfb3d14.png)

that are defined  by the formula

![image](https://user-images.githubusercontent.com/79521972/168503427-c3af3aa6-15bf-4c8d-9ba3-88f5c36158f2.png)

are known as the fibonacci numbers

- 0 번째와 1 번쨰는 0과 1로 항상 정해져 있는 값이다.

---

11-3

## Recursive implementation

```python
import time # 1
start_time = time.time()


def fibonacci(n): # 5 
    if n < 2:
        return n
    else:
        return fibonacci(n-1) + fibonacci(n-2)
    
    
print(fibonacci(36)) # 12
print(time.time() - start_time)

```

```
14930352
8.691221952438354
```



---

11-4

![image](https://user-images.githubusercontent.com/79521972/168503542-8cb82ca0-76b5-4267-a9a9-8973195a8b30.png)



똑같은 계산 과정이 반복이 돼서(중복적으로) 일어나게 된다.

위 그림은 7에 대한 fibonacci 결과를 tree로 나타낸 것인데 36에 대한 fibonacci 결과를 tree로 그린다면 굉장히 크고 중복이 수도 없이 많은 구조를 확인할 수 있을 것이고 이는 확실히 좋지 않은 성능을 야기한다는 것을 알 수 있을 것이다.

---

11-5

## Dynamic Programming

Dynamic programming, like the divide-and-conquer method, <mark>solves problems by combining the solutions to subproblems.</mark> ("Programming" in this context refers to a tabular method, not to writing compter code.)

- tabular method: 테이블에 기록하는 방식

**Divide-and-conquer algorithms** partition the problem into disjoint subproblems, solve the subproblems recursively, and then combine their solutions to solve the original problem.
In contrast, **dynamic programming** applies <span style="color:red">when the subproblems overlap</span>-that is, when subproblems share subsubproblems. In this context, a divide-and-conquer algorithm does more work than necessary, repeatedly solving the common subsubproblems. <mark>A dynamic-programming algorithm solves each subsubproblem just once and then saves its answer in a table</mark>, thereby avoiding the work of recomputing the answer every time it solves each subsubproblem.

- 작은 size의 문제를 딱 한 번만 풀고 그 푼 결과를 table에 저장해 두는 것이다.
  - 나중에 그 문제가 또 중복되어 나왔을 때 사용하기 위해서

<br>

하노이 문제의 경우 divide-and-conquer 로 풀었었는데 그 문제에서 나눠졌던 subproblem 들에 대해서 중복된 문제가 하나도 없었다.

즉, 주어진 문제를 subproblem으로 나누었을 때 중복이 없다면 divide-and-conquer라면
중복이 굉장히 많이 나온다면 dynamic programming 이다.



---

11-6

Dynamic programming thus **uses additional memory to save computation time**; 

- it serves an example of a **time-memory trade-off**. 

The savings may be dramatic: an exponential-time solution may be transformed into a polynomial-time solution.

There are usually two equivalent ways to implement a dynamic-programming approach.

<mark>The first approach is top-down with memoization.</mark> In this approach, we write the procedure recursively in a natural manner, but modified to save the result to each subproblem (usually in an array or hash table). The procedure now first checks to see whether it has previously solved this subproblem. If so, it returns the saved value, saving further computation at this level; if not, the procedure computes the value in the usual manner. We say that the recursive procedure has been memoized; 

- it "remembers" what results it has cimputed previously.

<br>
한 size의 문제를 조금씩 줄여나간다.

- f(n) - > f(n-1) -> f(n-2) -> ... 
  - recursion



---

11-7

## top-down with memoization

```python

def fibonacci_td(n): # 5
    if n < 2:
        return n
    
    if _memo[n] is None: # 9
        _memo[n] = fibonacci_td(n - 1) + fibonacci_td(n - 2)
        
    return _memo[n]


_memo = [None] * 50 # 15
print(fibonacci_td(36))
```

The numbers grow exponentially, so the array is smal - for example, F<sub>46</sub> = 1836311903 is the largest Fibonacci number that can be represented as a 32-bit integer

- 32-bit integer로 저장할 수 있는 최대 fibonacci 수는 46이다.
- 그 이상은 long long 데이터 타입을 사용해야 한다.

list에 저장되어 있는지를 확인하여 

- 만약 저장되어 있지 않다면: 더 작은 size의 문제를 subproblem을 나눠서 memo에 저장하고
- 만약 저장되어 있다면 

---

11-8

<mark>The second approach is the bottom-up method.</mark> This approach typically depends on some natural notion of the "size" of a subproblem, such that solving any paricular subproblem depends only on solving "smaller" subproblems. We sort the subproblems by size and solve them in size order, smallest first. When solving a particular subproblem, we ahve already solved all of the smaller subproblems its solution depends upon, and we have saved their solutions.

- 제일 작은 size인 f(1)부터 시작하여 f(2), f(3), ... 이런 식으로 차근차근 올라가면서 푸는 방법이다.

```python

def fibonacci_bu(n): # 5
    memo = [0, 1]  # base case
    
    for i in range(2, n+1):
        memo.append(memo[i - 1] + memo[i - 2])
        
    return memo[-1] # 11


print(fibonacci_bu(36)) # 14
```

리스트의 마지막에 담긴 값이 최종 결과임

---

11-9

## top-down vs. bottom-up

뭐가 더 좋은 방법일까? -> 결론부터 얘기 하자면 문제에 따라 다르다.(case by case)

그러나 일반적으로 특징을 비교해 보자면 아래 글과 같다.

Indeed, we can use the bottom-up approach any time that we use the top-down approach, although we need to take care to ensure that we compute the function values in an appropriate order, so that each value that we need has been computed when we need it.

In top-down dynamic programming, we save known values; in bottom-up dynamic programming, we precompute them. **We generally prefer top-down to bottom-up dynamic programming, because**

- It is a mechanical transformation of a natural problem solution.
  - 좀 더 자연스러운 문제 해결 가능

- The order of computing the subproblems takes care of itself.
  - 계산 순서를 스스로 알아서 해결

- We may not need to compute answer to all the subproblems.
  - 모든 subproblem에 대한 답을 계산할 필요가 없다.




주어진 size의 문제를 더 작은 size의 문제로 나누는 것 -> recurrence equation

---

11-10

![image](https://user-images.githubusercontent.com/79521972/168504697-5cddf421-4bf6-432f-ba2c-79e343ff0c27.png)



[https://www.acmicpc.net/problem/2748](https://www.acmicpc.net/problem/2748)



---

11-11

## 15.1 Rod cutting

Serling Enterprises(철강 회사) buys long steel rods and cuts them into shorter rods, which it then sells. Each cut is free. The management of Serling Enterprises want to know the best way to cut up the rods.

The rod-cutting problem is the following. Given a rod of length n inches 
and a table of prices pi for i = 1, 2, ...., n, determine the <mark>maximum revenue r<sub>n</sub></mark> obtainable by cutting up the rod and selling hte pieces.

![image](https://user-images.githubusercontent.com/79521972/168504861-a767d9d3-a0f1-4695-9403-6afa5829d3a4.png)

철봉의 길이에 따라 시장 가격이 정해져 있다. 그렇다면 철강회사에서 철봉을 어떻게 잘라야 돈을 많이 벌까?


---

11-12

![image](https://user-images.githubusercontent.com/79521972/168504887-6619a606-b0f4-4659-b32d-fa1e51b0b112.png)

위 그림과 같이 어떻게 나눠서 파느냐에 따라 받을 수 있는 가격이 다 다르다.

그렇다면 n인치의 철봉이 주어졌을 때 어떻게 나눠서 팔아야 할지를 알아보자.

이러한 문제 구조를 Optimization Problem이라고 한다.

- 그리고 optimization 문제는 대부분 DP로 해결한다.

---

11-13

More generally, we can frame the values rn for n >= 1 in terms of optimal revenues from shorter rods:

![image](https://user-images.githubusercontent.com/79521972/168504950-864d8a40-dbe7-4f7f-8184-b61bb55994e8.png)

- r<sub>n</sub>: n인치에서 얻을 수 있는 최대 수익
- p<sub>n</sub>: 시장 가격
- 총 n가지 경우에 대해 조사를 해서 최대값을 구한다.
- r<sub>1</sub>+r<sub>n-1</sub> 은 사실상 p<sub>1</sub>+r<sub>n-1</sub> 과 같다.
  - 그래서 앞에 더해지는 rn을 모두 pn으로 대치하여 하는 것이 더 편하다. 

In a related, but slightly simpler, way to arrange a recursive structure for the rod-cutting problem, we view a decomposition as consisting of a first piece of length i cut off the left-hand end, and then a right-hand remainder of length n - i. Only the remainder, and not the first piece, may be further divided. We thus obtain the sollowing simpler version of equation (15.1):

![image](https://user-images.githubusercontent.com/79521972/168505091-16374575-d984-43aa-ba39-7ec3cfedd609.png)



예를 들어, 4인치 나무인 경우

r4 -> 

- p1 + r3
- p2 + r2
- p3 + r1
- p4

중에서 가장 큰 값을 구하는 것이다.



---

11-14

```python
import math # 1


def cut_rod(length): # 4 
    if length == 0: # r_0
        return 0
    
    max_revenue = -math.inf # -infinity
    for i in range(1, length+1): # 9
        revenue = _price[i] + cut_rod(length-i) 
        if revenue > max_revuenue:
            max_revenue = revenue # 가장 큰 값을 저장하는 logic
            
    return max_revenue


_price = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30] # 17
_length = 8
print(cut_rod(_length))

```



---

11-15

**Why is CUT-ROD so inefficient?** The problem is that CUT-ROD calls itself recursively over and over again with the same parameter values; 

- it solves the same subproblems repeatedly. -> 굉장히 비효율적인 계산이 된다.

![image](https://user-images.githubusercontent.com/79521972/168517940-278d7008-2267-4aa8-afe2-4a4da2e58256.png)

그렇기 때문에 이를 top-down 방식의 DP와 bottom-up 방식의 DP로 바꾸어보자.



---

11-16

## top-down with memoization

```python
def cut_rod_td(length): # 4
    if length == 0:
        return 0
    
    if _memo[length] is None: # 8
        max_revenue = -math.inf
        for i in range(1, length+1):
            revenue = _price[i] + cut_rod_td(length-i) # 11
            if revenue > max_revenue:
                max_revenue - revenue
        # save r_i
        _memo[length] = max_revenue
        
    return _memo[length] # 17


_price = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30] # 20
_memo = [None] * 11
_length = 8
print(cut_rod_td(_length))
```



---

11-17

## bottom-up method

```python
def cut_rod_bu(length):# 4 
    
    for i in range(1, length+1):
        max_revenue = -math.inf
        for j in range(1, i+1):
            revenue = _price[j] + _memo[i-j] # 9
            if max_revenue < revenue:
                max_revenue = revenue
        # save r_i
        _memo[i] = max_revenue # 13 
        
    return _memo[length]


_price = [0, 1, 5, 8, 9, 10, 17, 17, 20, 24, 30] # 18
_memo = [0] * 11
_length = 8
print(cut_rod_bu(_length))
```

i=1: p1

i=2: p1+ r1

​		p2+r0



---

11-18

## 15.3 Elements of dynamic programming

- We typically **apply** dynamic programming to **optimization problems**
  - n size 문제를 더 작은 size의 문제로 나눈다.


![image](https://user-images.githubusercontent.com/79521972/168519662-d5fd7824-3d7e-4639-8367-7948afbede0c.png)



- Two key ingredients that an optimization problem must have to apply dynamic programming:

1. **Optimal substructure**
   - divide-and-conquer
   - An optimal solution contains optimal solutions to subproblems
2. **Overlapping subproblems**
   - divide-and-conquer와는 다른 점
   - Finding the solution involves solving the same subproblem multiple times
   - A divide-and-conquer approach always generating new subproblems


---

11-19

We typically apply dynamic programming to **optimization problems**. <mark>Such problems can have many possible solutions.</mark> Each solution has a value, and we wish to find a solution with the optimal (minumum or maximum) value. We call such a solution an optimal solution to the problem, as opposed to the optimal solution, since there may be several solutions that achieve the optimal value.

When developing a dynamic-programming algorithm, we fllow a sequence of four steps:

1. Characterize the structure of an optimal solution.
2. Recursively define the value of an optimal solution.
3. Compute the value of and optimal solution
4. Construct an optimal solution from computed information.

Steps 1-3 form the basis of a dynamic-programming solution to a problem. If we need only the value of an optimal solution, and not the solution itself, then we can omit step 4.



---

11-20

## Reconstructing a solution

Here is an **extended version** of BOTTOM-UP-CUT-ROD that computes, for each rod size j, not only the maximum revenue r<sub>j</sub>, but also s<sub>j</sub>, the optimal size of the first piece to cut off:

```python
			if max_revenue < revenue: # 10
        		max_revenue = revenue
            	_first[i] = j # 앞에거(j)를 얼마만큼 잘라야지 revenue를 얻을 수 있는지
```

```python
_memo = [0] * 11 # 20
_first = [0] * 11
_length = 8
print(cut_rod_bu(_length)) # 23
while _length:
    print(_first[_length], end=' ')
    _length -= _first[_length]
```

12, 21번 line이 새로 추가됨

_first: 앞에거(j)를 얼마만큼 잘라야지 revenue를 얻을 수 있는지를 기록하는 용도 (r4의 경우 2)

코드 돌려볼 것!!!!!!!!!

---

11-21

![image](https://user-images.githubusercontent.com/79521972/168520429-c4b15bc3-4476-4503-b976-eccf6a971072.png)

[https://www.acmicpc.net/problem/1932](https://www.acmicpc.net/problem/1932)



---

11-22

![image](https://user-images.githubusercontent.com/79521972/168520466-1525764a-6524-4ef3-ba3e-33aa3ee4af3b.png)

![image](https://user-images.githubusercontent.com/79521972/168976111-49a8837a-9c3c-4740-8659-4b8d367e05e1.png)

optimal structure

maxV(0, 0)에서 얻을 수 있는 최대값



---

11-23

```python

def max_path(row, col): # 5
    if row == _size - 1: # 제일 밑에 줄인 경우는 그냥 그 자리의 값이 결과값임
        return _triangle[row][col]
    
    path_left = _triangle[row][col] + max_path(row+1, col) # 9
    path_right = _triangle[row][col] + max_path(row+1, col+1)
    
    return max(path_left, path_right)


_triangle = [] # 15
_size = int(input())
for _ in range(_size):
    _triangle.append(list(map(int, input().split()))) # 18
    
print(max_path(0, 0))

```

![image](https://user-images.githubusercontent.com/79521972/168520638-9dc137d7-4607-4c73-b3ce-9abd6fe35493.png)

- 중복 많아서 시간초과 발생
  - so, DP로 변경해야 함.
- Top down의 경우 triangle과 크기가 완전히 똑같은 list 하나를 만들고 max_path 함수를 호출했을 때 memo에 저장되어 있는지를 확인 후 저장이 안되어 있다면 함수를 실행하도록 하고 저장이 되어 있다면 그냥 그 값을 리턴하도록 한다.
- Bottom up의 경우 loop를 사용하여 밑에서부터 위로 계산하면서 올라가면 된다.

---

11-24

## 0-1 Knapsack problem

The **0-1 knapsack problem** is the following. A thief robbing a store finds n items. THe *i*-th item is worth v<sub>i</sub> dollars and weighs w<sub>i</sub> pounds, where v<sub>i</sub>and w<sub>i</sub> are integers. The thief wants to take as valuable a load as possible, but he can carry at most W pounds in his knapsack, for some integer W. Which items should he take?

![image](https://user-images.githubusercontent.com/79521972/168520845-99c2122c-af74-467f-8aee-f6524079ac88.png)

- objective function
- constraint
- 물건을 담으면 1 안 담으면 0

---

11-25

![image](https://user-images.githubusercontent.com/79521972/168520896-ba378f1f-0178-438b-8e5d-1aa97f5e4d9c.png)





---

11-26

![image](https://user-images.githubusercontent.com/79521972/168520917-cfa3fa1d-b85d-47b7-b79f-83bbc32da83a.png)

[https://www.acmicpc.net/problem/12865](https://www.acmicpc.net/problem/12865)

V(7,0)

0번 물건을 넘기면 얻을 수 있는 최대 수익



---

11-27

![image](https://user-images.githubusercontent.com/79521972/168521024-e60e76d4-63a6-4a73-a16d-b8e67444b91a.png)



---

11-28

```python
def knapsasck(capacity, item):
    # capacity: current capacity of the knapsack, [0.._capacity]
    # item: index of the item to be considered, [0..number-1]
    # _number: number of items
    # _capacity: capacity of the knapsack
    # _weight: weight list of the items
    # _value: value list of the items
    
    if capacity == 0 or item >= _number:
        return 0
    
    if_weight[item] > capacity:
        return knapsack(capacity, item+1)
    
    with_the_item = _value[item] + knapsack(capacity - _weight[item], item+1)
    without_the_item = knapsack(capacity, item+1)
    
    return max(with_the_item, without_the_item)
```

![image](https://user-images.githubusercontent.com/79521972/168521280-1c569835-402d-4e3b-a4ff-7111ba655705.png)

Top down으로 바꾼다. - memo를 딕셔너리로 만들어서 한 노드(튜플)을 key로 해서 값을 계속 저장해서



bottom up으로 하면 저장해야 하는 key 수가 그렇게 만지 않은데 이차원 리스트의 사이즈가 WxN 만큼으로 만들어서 메모리 비효율?









