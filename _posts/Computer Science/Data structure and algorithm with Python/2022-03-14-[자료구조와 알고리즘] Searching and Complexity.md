---
layout: single
title: "[자료구조와 알고리즘] Searching and Complexity"
categories: ['Computer Science', 'Data structures and algorithms with Python']
tag: ['Data structures and algorithms', 'Python']
---



## Search Algorithms

### linear search 

the searching operation은 정렬된 데이터로부터 주어진 item을 찾아내는 것이다. 만약 발견한 item이 정렬된 리스트에서 가져올 수 있다면 그것이 위치한 index 위치를 반환하거나 발견하지 못했다는 것을 반환한다.

리스트 안의 item을 search하는 가장 쉬운 방식은 linear search 방법이며, 이는 전체 리스트의 item을 하나씩(one-by-one) 찾는 방식이다.

\<주의 사항>
리스트의 item's type을 개념 이해를 위해서 이 chapter에서는 interger 변수로 할 것이다. (정수가 비교적 제일 이해가 쉽기 때문에). 하지만 리스트 item's type은 어떠한 data type도 가능 하다는 것을 잊지마라.



### Unordered linear search

item이 정렬된 순서가 없기 때문에 끝까지 다 찾아봐야 함

The linear search의 접근 방식은 item이 어떻게 정렬되어 있는 지에 달려있다. 즉, item들이 정렬되어 있는지 혹은 어떠한 순서도 없이 정렬되어 있지 아닌지에 따라 다른 것이다.



 ```python
 import random#실습 데이터 만들기 위한 모듈
 
 def search(unordered_list, target):
     for i in range(len(unordered_list)):
         if target == unordered_list[i]:
             return i
     return None
 
 test = []
 for _ in range(10):
     # random integer n such that 1 <= n <= 15
     n = random.randint(1, 15)
     test.append(n)
 print(test)
 
 _target = 11 #local varible과 global variable 간의 차이를 두기 위함(pycharm에서는 오류이기 때문)
 result = search(test, _target)
 if result is None: # equality(==)는 그 안의 값을 비교해야 하기 때문에 is 보다 더 오래걸림
     print("%s is not found." % _target) # % _target : 중간에 띄어쓰기에 유의!
 else:
     print("%s is at index %s" %(_target, result))
 ```

==항상 변수 name을 짓는 것은 알아보기 쉽게 직관적으로 지을 것==





### Ordered linear search

알고리즘이 다음과 같은 단계로 감소한다.

1. list를 순차적으로 이동한다.
2. 만약 찾는 item(target)이 현재 loop에서 inspection하고 있는 object 혹은 item보다 작으면  종료하고 None을 반환한다.



```python
def search(ordered_list, target):
    ordered_list_size = len(ordered_list)
    for i in range(ordered_list_size):
		if target == ordered_list[i]:
			return i
        elif ordered_list[i] > target:
            return None
        
    return None

# random sampling without replacement
test = random.sample(range(1, 15), 10) #모두 다른 숫자를 가지는 10개의 random 숫자를 뽑는다.
test.sort()
print(test)

_target = 11
result = result(test, _target)
if result is None:
    print('%s is not found.' % _target)
else:
    print("%s is at index %s" % (_target, result))
#if-if 문으로 해도 똑같지만 반드시 두 if를 거치게 되지만 if-elif 문으로 하면 if가 맞으면 elif문은 보지 않기 때문에 시간이 더 빠르다.
```



문제는 이론보다 코딩 위주(강의 자료에 있는 코드를 이해하고 있는가, 공부를 했는가)

<br>

### Binary search

linear search보다 좋은 알고리즘이지만 그만큼 더 복잡하다.(모든 알고리즘에서 적용되는 rule)

![image](https://user-images.githubusercontent.com/79521972/158100915-5c9840dd-4a4a-4723-a23a-3f564c68a5f9.png)

- 가장 가운데 값을 먼저 본다
  - 0~11의 index의 중간 index = (0+11)/2 = 5

- 해당 값과 target값을 비교한다.
  - 43(target)>37
- 더 작기 때문에 큰 부분으로 위 과정을 반복한다.
  - 6~11 -> (6+11)/2 = 8



```python
def binary_search(ordered_list, target):
    left, right = 0, len(order_list)-1
    
    while left <= right: #left와 right가 교차하기 전까지
        mid = (left + right) // 2
        ...
        
        if target < ordered_list[mid]:
            right = mid - 1
        elif target > ordered_list[mid]:
            left = mid + 1
        else:
            return mid
        
     return None #위 반복문에서 return하지 못했다면 찾지 못한 것이기 때문에 None 객체를 반환한다.

# random sampling with replacement(중복을 허용한다.)
test = random.choices(range(1, 15), k = 10) # choices() vs. sample() : k를 반드시 정해야 함.
test.sort()
print(test)
```



시험문제는 100% 강의 자료에 나옴



### bisect - Array bisection algorithm

- bisect.bisect_left(a, x, lo=0, hi=len(a))
  - x라는 point를 a리스트에 삽입을 하는데 order를 유지한 채 삽입할 수 있는 위치를 알려준다.
  - lo라는 파라미터와 hi라는 파라미터는 서브리스트를 만들 수 있는 start, end index를 정할 수 있다.
    - default는 전체 리스트에 대해 보는 것으로 한다.
  - 만약 리스트 안에 이미 들어있는 값과 동일한 값에 대하여 삽입을 원하는 경우 가장 왼쪽값으로 넣게 된다.
  - [<u><span style="color:blue">bisect — Array bisection algorithm — Python 3.8.7 documentation</span></u>](https://docs.python.org/3.8/library/bisect.html)

```python
import bisect

# initializing test
li = [1, 3, 4, 4, 4, 6, 7]

# using bisect_left() to find index to insert new element
# return 2 (left most possible index)
print(bisect.bisect_left(li, 4))
```

<br>

**예제코드**

```python
import random
import bisect

def binary_search(ordered_list, target):
    index = bisect,bisect_left(ordered_list, target)
    
    # 주어진 리스트 안의 범위만 확인하도록(out of index 방식)
    if index < len(ordered_list) and ordered_list[index] == target:
        return index
    else:
        return None
    
test = random.choices(range(1, 15), k = 10)
test.sort()
print(test)

_target = 11
result = binary_search(test, _target)
if result is None:
    print('%s is not found.' % _target)
else:
    print('%s is at index %s' % (_target, result))
```

<br>

## Time Complexity of an Algorithm(알고리즘의 시간복잡도)

### Time Complexity of an Algorithm

무엇이 좋은 알고리즘인지를 판단하는 데 사용되는 지표

- time complexity(efficiency)

- space complexity
  - space complexity는 요즘 메모리 성능이 좋아지면서 중요도가 낮아지고 있음

<br>

### Running time *f(n)* of an algorithm

중첩된 loop내에 연속된 구문들에 대하여, 각 구문의 시간복잡도들을 더하고(add) 구문이 실행되는 횟수들의 갯수에 의해 곱해진다. 예를 들어:

```python
n = 500 #c0

#executes n times
for i in range(0, n):
    print(i) #c1
    
for i in range(0,n):
    #executes n times
    for j in range(0,n):
        print(j) #c2
```

이는 다음과 같이 표현될 수 있다. 

- c~0~ + c~1~ n + cn^2^

<br>

다음과 같이 logarithmic complexity도 정의할 수 있다.

- 일정한 시간동안 1/2만큼 문제의 크기가 줄어드는 알고리즘의 시간 복잡도
- 예를 들어, 다음과 같은 경우를 고려해 보자.

```python
i=1
while i<n:
    i=i*2
    print(i)
```

f(n) = 2logn + 1



<br>

### f(n) = 15n^2^ + 45n + 2000

이중 for-loop 안에 15줄

단일 for-loop 안에 45줄

for-loop에 들어가있지 않은 2000줄이 있다고 가정해 보자.



![image](https://user-images.githubusercontent.com/79521972/158539694-e4acbef2-2b2b-493d-9839-0163fd69bbf7.png)

\<comparison of terms in running time function>

- n이 커질 수록 n을 많이 포함한 항일수록 더 지배적이게 된다.
  - n=1일 때는 3번째 텀(상수항)이 지배적이지만



<br>

### order vs. constant factor

- **C~1~n** vs. **C~2~n^2^** (c~1~ > c~2~ 는 일정하다)
  - Regardless of C~1~ and C~2~ , there exists a break even point.
  - C~1~, C~2~와 관계없이 분기점이 존재한다.

![image](https://user-images.githubusercontent.com/79521972/158543160-d48615a6-3eef-44b0-9072-fb9261da6569.png)

- 매우 큰 n에 대해서는
  - f(n)의 차수가 중요하다.
  - 상수항은 무시될 수 있다.
    - n이 매우 크기 때문에 n과 관련된  term만 살아남는다.
  - `1000n`은`2n^2`^보다 효율적이다.



<br>

### the rate of growth

- 만약 *n*이 충분히 크다면 **order(rate) of growth**만이 중요하게 고려된다.
- 상수 계수는 n이 작지 않으면 중요하지 않다.

![image](https://user-images.githubusercontent.com/79521972/158543404-5393fabc-3c3b-45ee-a8ff-f6b74eb45a93.png)

아무 생각없이 짜다 2^n^과 같은 코드를 짜게 되면 굉장히 난감한 시간이 걸리는 것이다.

<br>

### ==Big O natation==

- The big-O notation represents ***order of*** f(n)

- **최고차항의 order**만 표시한 표기법(낮은 차수 무시)
- 상수항, 낮은 차수들 무시
- 즉, 단일 for loop이냐 이중 for-loop냐 인지를 나타낼 수 있음

![image](https://user-images.githubusercontent.com/79521972/158543797-bc30f63d-ac30-45dd-a8b2-aa298b69ecf8.png)

<br>



growth율이 function의 order로 정의되는 인식에서, big O notation에서 O라는 문자는 order를 대표하여 붙여졌다. 

또한 이는 가장 최악의 경우 runnig time complexity를 측정한다. 즉, 알고리즘에 의해 수행될 maximum time을 고려하는 것이다. 

쉽게 말해서 f(n)이라는 함수는 g(n)이라는 또다른 함수의 big O이고 우리는 이를 다음과 같이 정의한다.

f(n) = O(g(n)) if there exist constants n~0~ and c such that f(n) <= cg(n) for all n >= n~0~



<br>

### complexity classes

- Time complexity of an algorithm

O(1) < O(log n) < O(n) < O(n log n) < O(n^2^) < O(n^3^) < O(2^n^) < O(n!)

-  Efficiency of an algorithm

O(1) > O(log n) > O(n) > O(n log n) > O(n^2^) > O(n^3^) > O(2^n^) > O(n!)

<br>
이진 탐색 알고리즘의 running time complexity 최악의 경우는 O(log n)이고 반면, 선형 탐색의 경우 O(n)이다.

- 선형 탐색은 for-loop를 사용하지만 이진 탐색은 data range가 1/2 씩 감소하기 때문에

참고로 binary search의 f(n) = O(log n) + 1



<br>

---

Q.

시간 복잡도를 고려할 때 언어의 종류와 CPU는 고려하지 않아도 되는가?

A.

언어와 CPU에 따른 time complexity를 생각하지 않는 이유는 물론 runnin time이 짧아지고 길어지긴 하겠지만  비율은 여전히 똑같을 것이기 때문에 이 과목에서는 무의미한 비교이기 때문이다.

---

big O가 점진적인 분석과 관련되어 가장 많이 사용되긴 하지만, 이와 관련된 두 개의 다른 notation들이 존재한다. 이에 대해 짧게 알아보도록 하겠다. 

#### Big Omega notation(Ω)

Big Omega notation은 tight한 upper bound를 묘사하는 big O notation과 유사한 tight한 algorithm에서 lower bound를 묘사하는 것이다.

이는 즉, ==best-case running time== complexity를 계산하는 것이다.

<br>

#### Big Theta notation(Θ)

이는 주어진 function의 upper bound와 lower bound 둘 다 모두 같은 경우 고려하는 것이며 이의 목적은 만약 이 경우인지 아닌지를 판단하기 위함에 있다.

<br>

백준 수 찾기(1920) 과제



### Map

과제에 유용하게 사용할 수 있는 함수를 배워보겠다.

- map()은 iterable객체(list, tuple, etc.)의 각 item에 대한 주어진 function에 적용한다.

```python
a = '1 2 3 4'
b = map(int, a.split())

print(list(b))
#[1,2,3,4]

# read numbers from the key board
a = list(map(int, input().split()))
>>> 1 2 3
print(a)
#[1,2,3]

a, b, c = map(int, input().split())
>>>1 2 3
print(a, b, c)
#(1, 2, 3)
```



### Input from stdin

```python
# use a file as the standard input (keyboard)

import sys
sys.stdin = open('input.txt', 'r')

a = list(map(int, input().split()))

print(a)
#[4, 1, 5, 2, 3]

a = list(map(int, input().split()))
print(a)
#[1, 3, 7, 9, 5]

sys.stdin = open('input.txt', 'r')
a = list(map(int, sys.stdin.readline().split()))
print(a)
#[4, 1, 5, 2, 3]
```



### 과제

1. linear search를 하는 코드
2. binary search를 하는 코드
3. bisect를 사용한 코드



이 코드를 바탕으로 위 세 가지 방법을 구현하여 어떻게 나오는지 봐보기

```python
import sys
import bisect

sys.stdin = open("bj1920_in.txt", "r") # 이 코드는 제출할 때는 제거하고 제출

N = int(input())
card = list(map(int, input().split()))
card.sort()
M = int(input())
target = list(map(int, input().split()))
```



이 때 중요한 점은 binary search의 경우 sort를 해야만 하기 때문에 이 과정을 추가하는데 이는 시간 복잡도에 영향을 미치지 않는다. 이에 대해서는 다음 시간에 계속 알아보도록 하겠다.

















