---
layout: single
title: "[자료구조와 알고리즘] Recursion and Backtracking"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures and algorithms', 'Python']
---



# Recursion

우리는 목표를 성취하는 것을 돕기 위해 어떠한 메소드가 다른 메소드를 호출할 수 있다는 사실을 알고있다. 이와 유사하게 메소드는 자기 스스로 또한 호출할 수 있다.

Recursion(재귀) 방식은 programming technique 중 하나로 메소드가 목적을 달성하기 위해서 본인 스스로를 호출할 수 있는 메소드를 말한다.

<br>

- 두 가지 접근 to repetitive algorithms
  - iteration(순회)
    - loops(for, while, do-while)
  - recursion(재귀)
    - Function calls itself
      - 함수가 자기 자신을 계속해서 반복하여 호출하는 방법
      - Suitable for problem **breaking down**
        - <mark>break down</mark> : 주어진 문제를 더 작은 크기의 문제로 줄이는 행위
      - <span style="color:red">Drastically simplifies an algorithm in many cases</span>
        - 어떤 문제를 해결하려 할 때 그 문제에 recursion을 적용할 수 있는 경우가 있고 아닌 경우가 있는데 만약 적용할 수 있는 경우에 그 문제를 엄청나게 간단하게 만들어 준다.



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
print("%d! = %d" % (num, factorial(num)))
```

```
7! = 5040
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
print("%d! = %d" % (num, factorial(num)))
```

```
7! = 5040
```



<br>
recursive 함수의 핵심은 두 가지 경우가 존재해야 한다.

- **Baes cases**: 계산을 안 해도 한 번에 답이 바로 나오는 경우(위의 factorial 예시에서 n = 0 일 때 1 인 상황)
  - These tell the recursion when to terminate, meaning the recursion  will be stopped once the base condition is met
- **Recursive cases**: 함수 호출을 종료 시킬 수 있는 조건이 있는 경우
  - The function calls itself and we progress towards achieving the base criteria
  - 함수가 자기자신을 호출하여 base criteria를 성취하는 방향으로 진행하는 것.

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

- base case 존재!

<br>

따라서 power()도 recursive로 구현할 수 있는 조건을 만족한다.

```python
def power(base, exponent):
    if exponent == 0:
		return 1
    else:
        return base * power(base, expoenet-1)
    

a, b = 5, 3
print("%d**%d = %d" %(a, b, power(a, b)))
```

```
5**3 = 125
```



<br>

## Complexity of recursive algorithms

recursive algorithm의 order(차수)를 정의하는 것은 recursion의 order를 결정하는 문제와 그것과 recursive method body의 order를 곱하는 것에 대한 문제이다.

- 즉 재귀 호출 횟수 x 재귀 문의 line 갯수

<br>

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

f(n) = c + f(n-1)    //  n = n - 1을 계속 대입한다.

= 2c + f(n-2)

= ic + f(n-i)

= n*c + f(0)  

따라서 O(n)을 갖는다. (이 예제의 경우, for loop와 동일한 O(n))

![image](https://user-images.githubusercontent.com/79521972/159201643-c299fc43-c604-430f-9392-d974a519b579.png)

따라서 body는 O(1) (위에서는 c로 표현함)이고 recursion은 O(n) 이기 때문에 전체 algorithm의 order는 O(n) 이다.



<br>

## Divide and Conquer

- Divide and conquer (분할 및 정복)
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
- 그 후, original problem을 해결하기 위한 solution을 만들기 위해 위 결과로 나온 solution들을 결합한다.(Combine)

The divide-and-conquer 패러다임은 다음과 같은 세 가지 단계(recursion의 각 레벨)를 수반한다.

1.  같은 문제의 더 작은 instances인 수많은 sub-problem들로 **Divide** 한다.

2. 그들을 재귀적으로 풀어냄으로써 작은 문제들을 **정복**한다.
3.  작은 문제들에 대한 솔루션을 **결합**하여 original problem에 대한 솔루션을 만들어낸다.(만약 필요하다면)



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

```
Y
```



![image](https://user-images.githubusercontent.com/79521972/159203233-35147d0f-b630-4252-b466-ba0bd3d2603c.png)

그니까 크기가 1인 거는 그냥 아무거나 리턴해도 되니까 (base case니까) 1인거에 대해서는 가장 먼저 처리를 한 거고 만약에 리스트가 나눠 졌는데 (2,1) 로 나눠지면 2부분은 (1,1) 로 나눠져서 각각 x,y에 들어가서 비교를 한 뒤에 더 큰 걸 반환하고 (2,1)로 올라가서 1부분과 도 비교하여 거기서 큰 걸 반환해서 또 그 위로 올라가고 하는 식.

그니까 x,y는 계속해서 재귀 호출이 진행되면서 left branch와 right branch로 분기되는데 <mark>그 때마다의</mark> left-list의 max값과, right-list의 max값을 의미하게 된다.

- 그래서 현재 돌고있는 left-list의 max값이 정해지면 y을 알아내야 하기 때문에 mid+1, right으로 지정하여 재귀호출을 하는 것이다.

<br>

### 분할정복의 큰 틀

```python
function F(x):
    if F(x)의 문제가 간단 then :
        return F(x)를 직접 계산한 값
    else:
        x를 y1, y2로 분할
        F(y1);
        F(y2);
        return F(y1), F(y2)로부터 F(x)를 구한 값
```

큰 문제를 2개 이상의 작은 문제로, 즉 y1과 y2로 나누고 만약 y1 또는 y2가 바로 계산할 수 있다면 (base case) 계산한 값을 반환함으로써 작은 문제를 하나씩 해결해 나가면서 큰 문제를 해결할 수 있다.



---

Q) size에 따라 iterative 방법과 recursive 방법이 더 좋은 것이 있을 것인데 이를 어떻게 판단하는가?

A) 각각의 장단점이 존재하고 무엇을 쓸지에 대한 명확한 기준은 없으나 프로그래머가 판단했을 때 몇 줄만에 코드가 끝나고 size가 적당하다면 recursive를 사용하지만 data의 size가 너무 크면 iterative를 사용한다.

---





<br>

## The Towers of Hanoi

[백준 11729번 하노이 탑 이동 순서]()

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



<mark>코드로 구현(recursion, 하노이 탑)</mark>

```python
def hanoi(disks, src, dst, extra):
    if disks == 1:
		hanoi.count += 1
        print(f'{hanoi.count}: move from {src} to {dst}') #formatted printing
        
    else:
        # f(n-1)
        hanoi(disks-1, src, extra, dest)
        
        # 1
        hanoi.count += 1
        print(f'{hanoi.count}: move from {src} to {dst}')
        
        # f(n-1)
        hanoi(disks-1, extra, dst, src)
        
hanoi.count = 0   #static variable of hanoi()
hanoi(3, 1, 3, 2)
```

![image](https://user-images.githubusercontent.com/79521972/159204338-dd811bd1-d2fc-4734-a492-7e3cd26b61a7.png)

hanoi()가 호출되는 횟수 만큼 호출 스택이 쌓이고 변수도 그만큼 존재하게 되는데 hanoi.count와 같이 static 변수로 선언하게 되면 모든 hanoi() object에서 같은 변수를 공유하게 된다. 그래서 한 hanoi()에서 count 값을 바꾸면 모든 hanoi() object의 count 변수가 해당 값으로 변하게 된다.

<br>

N개의 disk의 스택을 가진 이 퍼즐을 풀기 위해서는 2<sup>N</sup>-1 개의 각각의 disk의 이동을 만들어 내야 한다. 따라서, 하노이의 탑 알고리즘은 O(2<sup>n</sup>) 의 시간 복잡도를 갖는다.

f(n) = 2f(n-1) + 1
       = 2<sup>2</sup>f(n-2) + 2+ 1
       = 2<sup>3</sup>f(n-3) + 2<sup>2</sup> + 2 + 1
       = 2<sup>i</sup>f(n-i) + 2<sup>i-1</sup> + 2<sup>i-2</sup> + ... + 1
       = 2<sup>i</sup>f(n-i) + 2<sup>i</sup> - 1
       = 2<sup>n-1</sup>f(1) + 2<sup>i</sup> - 1
       = 2<sup>n</sup> - 1

만약 1초당 하나 씩 disk를 옮긴다면 n=40인 경우, 적어도 다 끝내기 위해서 348 세기가 걸리는 것이다.



<br>

## Backtracking

- techqniue
- paradigm
- method
- algorithm

라고 한다.

<br>

### Brute-force algorithm(완전 탐색 알고리즘)

- `Brute-force search` or `exhaustive search`
  - A very general problem-solving technique(paradigm)
  - Systematically enumerating all possible candidates for the solution and checking whether each candidate satisfies the problem's statement
  - solution에 대해 가능성 있는 모~든 후보자들을 시스템적으로 나열하고 각 후보자들이 problem의 상태를 만족하는지 아닌지 확인하는 알고리즘이다.

<br>

For **Sudoku**, there are 9<sup>n</sup> ways to fill the n blank squares.

![image](https://user-images.githubusercontent.com/79521972/159652991-fbcd3b09-640f-4dae-9168-8c9e057a37bf.png)

만약 스도쿠 문제가 있다고 가정해 보자.

<br>

이 문제를 brute-force 기법으로 풀려면 9x9 size의 스도쿠 판의 각 칸마다 1~9까지의 수들을 전부 넣어 보면서 조건을 만족하는지 하나하나 확인하는 방법인 것이다. 이 경우 시간 복잡도가 O(9<sup>n</sup>)이 나오게 된다.

때에 따라서 브루트포스가 쓰이는 경우가 있기는 하나 스도쿠와 같이 일일히 하나하나 따졌을 때 굉장히 피곤한 경우에는 사용할 수가 없다.

**그렇다면 어떤식으로 스도쿠 문제를 풀어나가야 할까?**

<br>

만약 첫 번째 칸에 1을 넣고 바로 다음 칸에 1을 넣는 경우 그 뒤는 볼 것도 없이 해당 경로는 스도쿠가 만족하지 않는 코드인 것이다. 이를 활용하여 이 경우를 잘 판별해서 다시 뒤로 돌아갈 수 있는 방법을 찾아야 할 것이다.

<br>

<mark>코드로 구현(브루트 포스)</mark>

브루트 포스 알고리즘의 예시를 코드를 통해 살펴보자. 아래는 주어진 string s와 길이 n에 대하여 가능한 모든 배치 방식을 생성하기 위한 recursive 접근 방식을 사용했다.

```python
def bit_str(ans):
    if len(ans) == n:
        print(*ans)
    else:
        for s in data:
            ans.append(s)
            bit_str(ans)
            ans.pop()
            
            
n, data = 3, 'abc'
bit_str([])
```

- 길이가 3인 모든 수열을 반환하는 문제이다.
  - aaa, aab, aac, aba ,... , ccc
- 총 3<sup>3</sup> 가지의 문자열을 모두 생성해 보는 것이다.
- 내가 위 코드를 보면서 깨달은 생각
  - 처음에 f([]) 가 호출되어 else문이 실행되고 'abc'라는 data 중에서 첫 번째인 a에 대해 for문이 진행한다.
  - 배열에 a를 추가하여 재귀호출을 진행하면 f(['a']) 가 호출되고 else문으로 넘어가 또 다시 a에 대해 for문이 진행한다.
  - 그럼 배열에 a가 또 추가되고 f(['a', 'a']) 가 호출되고 if를 아직까지도 만족하지 않기 때문에 또 else로 넘어가 또 a에 대해 for문이 진행되어 배열에 a가 추가되고 f(['a', 'a', 'a'])가 호출된다.
  - 이 때는 if문을 만족하기 때문에 배열의 값을 프린트 한다.
  - <mark>여기서부터가 중요하다.</mark>
  - 프린트를 하고 나서는 현재 함수가 종료되고 이 함수가 호출된 곳(['a', 'a', 'a'])으로 돌아간다.
  - 호출된 곳에 돌아가자마자 있는 코드는 pop()이므로 가장 마지막의 원소를 반환하면서 'a'에 대해 진행되던 for문은 그 다음 문자인 'b'로 넘어가서 b를 추가하고 aab에 대해서 해당 과정을 계속 진행하게 된다. 
  - 이를 도식화하면 다음과 같다.
  - ![image](https://user-images.githubusercontent.com/79521972/159657596-f0043349-228a-4716-8ab9-993c32865b0f.png)

<br>

for loop보다 recursion이 더 좋다.

- 2중 for loop인 경우 더 간결할 수도 있지만 n의 size가 매우 커질 경우 코드가 돌아가지 않을 것이기 때문이다.

<br>

### Backtracking

Backtracking은 traversing tree 구조와 같은 문제의 유형에 특히 유용한 recursion의 한 형태인데, 각 노드에 대한 수 많은 옵션들이 제시된 곳에서 유용한 것이다.

또한 <mark>Backtracking은 exhaustive searching(완전 탐색)을 위한 divide-and-conquer 방식이다. 중요한 점은 백트래킹은 결과를 낼 수 없는 가지들은 쳐내는 것이다.</mark>

![image](https://user-images.githubusercontent.com/79521972/159659310-fbdb74ca-8420-4582-a585-fa836e7b3d90.png)

즉 위의 그림처럼 가지를 모두 탐색하면서 조건을 일일히 따져보는 것이 아니라, 중간에 failed candidates가 발견되면 다음 가지로 넘어가지 않고 다시 되돌아가(back) 현재 노드부터 다시 tracking을 시작하여 결과를 찾아내는 알고리즘인 것이다.

<br>

아래는 3*3 스도쿠를 백트래킹 방식으로 구현한 것을 도식화 한 것이다.

![image](https://user-images.githubusercontent.com/79521972/159659655-34fd57dc-fb20-421b-bd31-ce44e24463d9.png)

<br>

[백준 15649번 N과 M(1)]()

```python
def permute(ans):
    if len(ans) == m:
        print(*ans)
    else:
        for i in range(1, n + 1):
            if i not in ans:
                ans.append(i)
                permute(ans)
                ans.pop()
                
 
n, m = map(int, input().split())
permute([])
```

```
3 2 #input
1 2
1 3
2 1
2 3
3 1
3 2
```



<br>

[백준 15650번 N과 M(2)]()

```python
def combine(ans):
    if len(ans) == m:
        print(*ans)
    else:
        for i in range(1, n + 1):
            if (len(ans) >= 1) and (i <= ans[-1]):
            	continue

            else:
                ans.append(i)
                combine(ans)
                ans.pop()
                
                
n, m = map(int, input().split())
combine([])           
```

idea: n과m 1번 문제와 동일하게 돌긴 도는데 특정 부분에서만 넘어가게 짜야 하기 때문에 그 특정 부분을 지정하는 것이 제일 중요하다.

```
4 2 # input
1 2
1 3
1 4
2 3
2 4
3 4
```

<br>



### 조건문에서 되도록 &대신 and를 사용해야 하는 이유

```python
>>> 0 < 1
True
>>> 0 < 1 & 3 < 2
True

```

연산자의 우선 순위(operator precedance)

- &의 우선순위가 더 높아서 1&3을 먼저 계산하게 되어 0<1<2 라는 식이 되므로 예상치 못한 True가 나오게 된다.
