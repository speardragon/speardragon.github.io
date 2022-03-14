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

<주의 사항>
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

## Time Complexity of an Algorithm









