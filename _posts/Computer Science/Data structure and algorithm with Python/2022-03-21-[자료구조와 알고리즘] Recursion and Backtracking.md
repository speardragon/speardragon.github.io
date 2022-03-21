---
layout: single
title: "[자료구조와 알고리즘] Recursion and Backtracking"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures and algorithms', 'Python']
---



# Recursion

우리는 목표를 성취하는 것을 돕기 위해 다른 메소드를 호출할 수 있는 함수 한 가지를 알고있다. 유사하게 메소드는 그 스스로 또한 호출할 수 있다.

Recursion(재귀) 방법은 programming technique으로 메소드의 목적을 달성하기 위해서 본인 스스로를 호출할 수 있는 메소드를 말한다.

<br>

- 두 가지 접근 to repetitive algorithms
  - iteration
    - loops(for ,wihle, do-while)
  - recursion
    - Function calls itself
      - 함수가 자기 자신을 계속해서 반복하여 호출하는 방법
      - Suitable for problem breaking down
        - <mark>break down</mark> : 주어진 문제를 더 작은 size로 줄이는 행위
      - <span style="color:red">Drastically simplifies an algorithm in many cases</span>
        - 어떤 문제를 해결하려 할 때 그 문제에 recursion을 적용할 수 있는 경우가 있고 아닌 경우가 있는데 만약 적용할 수 있는 경우 엄청나게 간단하게 만들어 준다.



<br>

### Iterative factorial algorithm

![image](https://user-images.githubusercontent.com/79521972/159200562-da6b6c42-a0cc-473b-93a9-177c9ead1834.png)





```python
def factorial(n):
    if n == 0:
        return 1
    else:
        ans = n
        for i in range(n-1, 0, -1):
            ans *= i
        return ans
        
num = 7
print("%d != %d" % (num, factorial(num)))
```

```
7 != 5040
```

<br>

### Recursive factorial algorithm

![image](https://user-images.githubusercontent.com/79521972/159200667-5bc1d4c3-1d52-4ef7-9cc0-f02151f449a8.png)





```python
def factorial(n):
    # test for a base case
    if n == 0:
        return 1
    # general case (calculation and recursive call)
    else:
        return n * factorial(n-1) # 함수가 자기 자신을 다시 호출
    
num = 7
print("%d != %d" %(num, factorial(num)))
```

```
7 != 5040
```



<br>
recursive 함수의 핵심은 두 가지 경우가 만족해야 한다.

- Baes cases: 계산을 안 해도 답이 바로 나오는 경우(factorial 예에서 n = 0 일 때 1 인 상황)
  - These tell the recursion when to terminate, meaning the recursion  will be stopped once the base condition is met
- Recursive cases: 함수 호출을 종료 시킬 수 있는 조건이 있는 경우
  - The function calls itself and we progress towards achieving the base criteria

![image](https://user-images.githubusercontent.com/79521972/159201014-d9b70d0e-f5a2-47e1-8285-ada37d04f32d.png)

메모리의 함수 object는 최대 4개가 올라가지만 각자의 재귀 호출이 종료된 후에는 메모리가 삭제된다.

그래서 recursion은 호출하는 횟수만큼 memory를 많이 잡아 먹는다는 문제도 있다.

<br>

## Example: power function

예를 들어, 2<sup>3</sup> 이 8이기 때문에 다음은 8과 동일한 result2의 값을 설정할 것이다:

- result2 = power(2, 3)

<br>

power function의 정의는 다음 공식에 기반한다.

- x<sup>*n*</sup> is equal to x<sup>*n-1*</sup> * x

- break down 가능
- recursive case에 해당

<br>

n = 0, 즉 stopping case의 경우, 만약 n = 0이면 power(x, n)은 단순히 1을 반환한다.(x<sup>0</sup> = 1 이기 때문.)

- base case에 해당

<br>

따라서 power도 recursive로 구현할 수 있는 조건을 만족한다.

```python
def power(base, exponent):
    if exponent == 0:
		return 1
    else:
        return base * power(base, expoenet-1)
    

a, b = 5, 3
print("%d**%d = %d" %(a, b, power(a, b)))
```

<br>

## Complexity of recursive algorithms

recursive algorithm의 order를 정의하는 것은 recursion의 order를 결정하는 것과 recursive method의 body의 order에 의해 곱하는 것에 대한 문제이다.



power 함수를 실행했을 때 running time f(n)은 어떻게 되는 지 보자.

```python
def power(base, exponent): # f(n) 
    # c
    if exponent == 0:
		return 1
    
    # f(n-1)
    else:
        return base * power(base, expoenet-1)
    

a, b = 5, 3
print("%d**%d = %d" %(a, b, power(a, b)))


```

f(n) = c + f(n-1)

= 2c + f(n-2)

= ic + f(n-i)

= n*c + f(0)  

따라서 O(n)을 갖는다.

![image](https://user-images.githubusercontent.com/79521972/159201643-c299fc43-c604-430f-9392-d974a519b579.png)

따라서 body는 O(1) (위에서는 c로 표현함)이고 recursion은 O(n) 이기 때문에 전체 algorithm의 order는 O(n) 이다.



<br>

## Divide and Conquer

- Divide and conquer
  - algorithm design paradigm(방법)
  - based on <span style="color:red">multi-branched recursion</span>

앞서 다루었던 fatorial을 recursion으로 풀었던 문제는 single-branched recursion이라고 볼 수 있다.

- Divided-and-conquer technique (typical case)
  - ![image](https://user-images.githubusercontent.com/79521972/159202274-f10d3160-d1c2-4a42-917c-08653a793098.png)

- 어떤 문제를 풀 때 size가 n인 문제를 n/2인 문제 두 개로 나눈다.(recursion)
- 또 다시 위 두 문제를 n/4 짜리 문제 4개로 나누고 또 둘씩 나눠지는 것이 반복....
- 그래서 나눠진 size가 작은 문제들을 하나씩 해결한다.
- 위와 같은 방법은 2-branched-recursion임.

<br>

### divide-and-conquer approch

- 몇가지의 original problem과 유사하지만 size는 더 작은 subproblem들로 부순다(Divide).
- 각각의 subproblem들을 recursively 해결한다.(Conquer)
- original problem을 해결하기 위한 solution을 만들기 위해 위 결과로 나온 solution들을 결합한다.(Combine)













<br>

### Divide-and-conquer to find the maximum

```python
def find_max(data, left, right): 
    # base case(계산이 필요없는 base case, 원소가 하나있는 경우이므로 그냥 반환)
    if left == right:
        return data[left]
    
    #recursive case
    mid = (left + right) // 2
    x = find_max(data, left, mid)  # left branch(list)
    y = find_max(data, mid+1, right)  #right branch(list)
    
    #combine
    if x > y:
        return x
    else:
        return y
    
a = list("TINYEXAMPLE") # a = ['T', 'I', 'N' ,..., 'E']
print(find_max(a, 0, len(a)-1))
```



![image](https://user-images.githubusercontent.com/79521972/159203233-35147d0f-b630-4252-b466-ba0bd3d2603c.png)





---

Q) size에 따라 iterative 방법과 recursive 방법이 더 좋은 것이 있을 것인데 이를 어떻게 판단하는가?

A) 각각의 장단점이 존재하고 무엇을 쓸지에 대한 명확한 기준은 없으나 프로그래머가 판단했을 때 몇 줄만에 코드가 끝나고 size가 적당하다면 recursive를 사용하지만 data의 size가 너무 크면 iterative를 사용한다.

---

<br>

## The Towers of Hanoi

![image](https://user-images.githubusercontent.com/79521972/159203344-49984788-f548-4363-98bd-f04fc0561b91.png)

막대기 세 개가 존재한다. 왼쪽 부터 source, extra, destination

이 수수께끼의 목표는 source(first) peg에 모여있는 disk를 destination(third) peg으로 모두 옮기는 것이다. 우리는 이때 "extra"라는 중간에 놓인 peg를 잠시 둘 수 있는 임시 공간으로 사용할 수 있다. 

하지만 다음 세 가지 규칙을 반드시 준수해야 한다.

1. 우리는 한 번에 하나의 disk 만을 옮길 수 있다.
2. 우리는 한 disk 위에 그것보다 더 큰 disk를 놓을 수 없다.
3. 모든 disk들은 3 개의 peg 중에 한 개의 peg 위에 있어야 한다.



<br>

recursive로 해결하기 위해 제일 간단한 경우부터 생각해 보자.

### 1-disk & 2-disk cases

![image](https://user-images.githubusercontent.com/79521972/159203781-dea2eb53-c0bc-4483-9d73-904de3a58d09.png)

- Moving 1 disk - Move one disk from source to destination

  - 1 step is required
  - 계산이 필요없는 base case에 해당한다.

- Moving 2 disks

  1. Move one disk from source to extra
  2. Move one disk from source to destination
  3. Move one disk from extra to destination

  - 3 step is required

<br>

### Moving 3 disks(f(3))

![image](https://user-images.githubusercontent.com/79521972/159203874-2fa50965-dff9-4647-9241-c677ee443c7a.png)



- 위에 있는 두 개를 extra로 옮겨야 한다.(f(2) 사용 - 3 step)

- 제일 큰 거가 하나가 남는데 이는 base case이다. (1 step)
- extra로 옮겨놨던 두 개를 다시 dest로 옮긴다.(f(2) 사용 - 3 step)
- total 7 steps is required.
- f(3) = f(2) + 1 + f(2)



<br>

그렇다면 만약 7개의 disk가 있다고 생각해 보자.

- f(7) = f(6) + 1 + f(6)

- ...



<br>

### Recursive algorithm design

1. move (n-1) disk from source to extra   -> recursive case
2. move 1 disk from source to destination  -> base case
3. move (n-1) disks from extra to destination -> recursive case



EX)

![image](https://user-images.githubusercontent.com/79521972/159204122-8bdaf461-2bf8-4b43-bb1e-8f3b77221a89.png)



<mark>코드로 구현</mark>

```python
def hanoi(disks, src, dst, extra):
    if disks == 1:
		hanoi.count += 1
        print(f'{hanoi.count}: move from {src} to {dst}') #formatted printing
        
    else:
        hanoi(disks-1, src, extra, est)
        hanoi.count += 1
        print(f'{hanoi.count}: move from {src} to {dst}')
        hanoi(disks-1, extra, dst, src)
        
hanoi.count = 0   #static variable of hanoi()
hanoi(3, 1, 3, 2)
```

![image](https://user-images.githubusercontent.com/79521972/159204338-dd811bd1-d2fc-4734-a492-7e3cd26b61a7.png)

hanoi()가 호출되는 횟수 만큼 호출 스택이 쌓이고 변수도 그만큼 존재하게 되는데 hanoi.count와 같이 static 변수로 선언하게 되면 모든 hanoi() object에서 같은 변수를 공유하게 된다. 그래서 한 hanoi()에서 count 값을 바꾸면 모든 hanoi() object의 count 변수가 해당 값으로 변하게 된다.

<br>











