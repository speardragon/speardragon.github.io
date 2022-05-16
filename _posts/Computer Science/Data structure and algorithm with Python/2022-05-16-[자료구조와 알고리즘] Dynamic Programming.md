---
layout: single
title: "[자료구조와 알고리즘] Dynamic Programming"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Dynamic Programming']
---

<br>

# Dynamic Programming



---

11-2

## Fibonacci series

Let's consider and example to understant how dynamic programming works. We use the Fibonacci series to illustrate

The sequence of numbers
![image](https://user-images.githubusercontent.com/79521972/168503404-9961b12a-10f9-48ce-a26d-331fbdfb3d14.png)

that are defined  by the formula

![image](https://user-images.githubusercontent.com/79521972/168503427-c3af3aa6-15bf-4c8d-9ba3-88f5c36158f2.png)

are known as the fibonacci numbers

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





---

11-5

## Dynamic Programming

Dynamic programming, like the divide-and-conquer method, solves problems by combining the solutions to subproblems. ("Programming" in this context refers to a tabular method, not to writing compter code.)

Divide-and-conquer algorithms partition the problem into disjoint subproblems, solve the subproblems recursively, and teh combine their solutions to solve the original problem.
In contrast, dynamic programming applies when the subproblems overlap-that is, when subproblems share subsubproblems. In this context, a divide-and-conquer algorithm does more work than necessary, repeatedly solving the common subsubproblems. A dynamic-programming algorithm solves each subsubproblem just once and then saves its answer in a table, thereby avoiding the work of recomputing the answer every time it solves each subsubproblem.



---

11-6

Dynamic programming thus uses additional memory to save computation time; it serves an example of a time-memory trade-off. The savings may be dramatic: an exponential-time solution may be transformed into a polynomial-time solution.

There are usually two equivalent ways to implement a dynamic-programming approach.

The first approach is top-down with memorization. In this approach, we write the procedure recursively in a natural manner, but modified to save the result to each subproblem (usually in an array or hash table). The procedure now first checks to see whether it has previously solved this subproblem. If so, it returns the saved value, saving further computation at this level; if not, the procedure computes the value in the usual manner. We say that the recursive procedure has been memoized; 
it "remembers" what results it has cimputed previously.



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

The numbers grow exponentially, so the array is smal - for example, F46 = 1836311903 is the largest Fibonacci number that can be represented as a 32-bit integer



---

11-8

The second approach is the bottom-up method. This approach typically depends on some natural notion of the "size" of a subproblem, such that solving any paricular subproblem depends only on solving "smaller" subproblems. We sort the subproblems by size and solve them in size order, smallest first. When solving a particular subproblem, we ahve already solved all of the smaller subproblems its solution depends upon, and we have saved their solutions.

```python

def fibonacci_bu(n): # 5
    memo = [0, 1]  # base case
    
    for i in range(2, n+1):
        memo.append(memo[i - 1] + memo[i - 2])
        
    return memo[-1] # 11


print(fibonacci_bu(36)) # 14
```



---

11-9

## top-down vs. bottom-up

Indeed, we can use the bottom-up approach any time that we use the top-down approach, although we need to take care to ensure that we compute the function values in an appropriate order, so that each value that we need has been computed when we need it.

In top-down dynamic programming, we save known values; in bottom-up dynamic programming, we precompute them. We generally prefer top-down to bottom-up dynamic programming, because

- It is a mechanical transformation of a natural problem solution.
- The order of computing the subproblems takes care of itself.
- We may not need to compute answer to all the subproblems.



---

11-10

![image](https://user-images.githubusercontent.com/79521972/168504697-5cddf421-4bf6-432f-ba2c-79e343ff0c27.png)



[https://www.acmicpc.net/problem/2748](https://www.acmicpc.net/problem/2748)



---

11-11

## 15.1 Rod cutting

Serling Enterprises buys long steel rods and cuts them into shorter rods, which it then sells. Each cut is free. The management of Serling Enterprises want to know the best way to cut up the rods.

The rod-cutting problem is the following. Given a rod of length n inches 
and a table of prices pi for i = 1, 2, ...., n, determine the maximum revenue r<sub>n</sub> obtainable by cutting up the rod and selling hte pieces.

![image](https://user-images.githubusercontent.com/79521972/168504861-a767d9d3-a0f1-4695-9403-6afa5829d3a4.png)




---

11-12

![image](https://user-images.githubusercontent.com/79521972/168504887-6619a606-b0f4-4659-b32d-fa1e51b0b112.png)



---

11-13

More generally, we can frame the values rn for n >= 1 in terms of optimal revenues from shorter rods:

![image](https://user-images.githubusercontent.com/79521972/168504950-864d8a40-dbe7-4f7f-8184-b61bb55994e8.png)
In a related, but slightly simpler, way to arrange a recursive structure for the rod-cutting problem, we view a decomposition as consisting of a first piece of length i cut off the left-hand end, and then a right-hand remainder of length n - i. Only the remainder, and not the first piece, may be further divided. We thus obtain the sollowing simpler version of equation (15.1):

![image](https://user-images.githubusercontent.com/79521972/168505091-16374575-d984-43aa-ba39-7ec3cfedd609.png)





























