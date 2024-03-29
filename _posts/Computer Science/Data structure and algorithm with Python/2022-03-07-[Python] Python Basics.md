---
layout: single
title: "[python] Python Basis"
categories: ['Computer Science', 'Data structures and algorithms with Python', 'Language', 'Python']
tag: ['Data structures and algorithms', 'Python']
---

 

프로그램 실행 위주

이론 위주

어떻게 돌아가는 건지 + 코드를 바꾸면 어떻게 돌아갈지



# Python Flow Control

프로그램은 통상적으로 순서에 따라 실행되지만 프로그램 실행의 흐름(flow)를 제어하기 위한 주된 두 가지 방식이 존재한다.

- conditional statements(is, else,...)
- loops(for, while ...)

<br>

## if - else statements

if...else 와 elif statement는 statement의 conditional execution을 control 한다. general format은 일련의 if 와 elif statement와 잇따라서 오는 마지막의 else statement이다. 

```python
x = 'one'
if x == 0:
    print("false")
elif x == 1:
    print("true")
else:
    print("Something else")
```

```
Something else
```

if - elif - else 가 **같은 개행에 있는 경우** 그 중 조건에 맞는 한 가지 경우의 코드 블럭만 실행한다.

<br>

## Equility operator(==) and is operator

- `==` compares the <mark>objects </mark>(values)
- `is` checks whether both variables point to <mark>a same object</mark>

<br>

## range

- **range** object is created by `range()` function
- It contains a sequence of numbers
- Immutable and iterable

```python
>>> a = range(5)
>>> type(a)
class 'range'
>>>
>>> b = a.__iter__()
>>> type(b)
class 'range_iterator'
>>>
>>> b.__next__()
0
>>> b.__next__()
1
>>> b.__next__()
2
>>> b.__next__()
3
>>> b.__next__()
4
```

<br>

## for loops

```python
a = range(5)
for i in a:
    print(i, end=' ')
print()    
for i in range(1, 7, 2):
    print(i, end=' ')
print()       
for i in range(1, -4, -1):
    print(i, end = ' ')
print()
```

```
0 1 2 3 4
1 3 5
1 0 -1 -2 -3
```

- Python's `for loop` iterates over the items of any iterable object(list, string, etc)

- 사용법
  - range(start, end, step)


<br>

## break / continue

```python
for num in range(2, 5):
	if num % 2 == 0:
        print('Found an even number', num)
    print('Found an odd number', num)
```

```
Found an even number 2
Found an odd number 3
Found an even number 4
```

<br>

```python
for num in range(2, 5):
	if num % 2 == 0:
        print('Found an even number', num)
        break
    print('Found an odd number', num)
```

```
Found an even number 2
```

- break: 뒤의 코드는 건너 뛰고(skip) 반복문을 종료
- continue: 뒤의 코드는 건너 뛰고(skip) 반복문의 맨 처음으로 이동

<br>

## while loops

```python
>>> x = 0
>>> while x < 4:
    x += 1
    print(x)
1
2
3
4
```

```python
x = 0
while x < 4:
    x += 1
    if x % 2 != 0:
        continue
    print(x)
```

```
2
4
```

<br>

```python
x = 0
while x < 4:
    x += 1
    if x > 2:
        break
    print(x)
```

```
1
2
```

<br>

```python
x = 0
while x < 4:
    x += 1
    if x > 2:
        break
    print(x)
else:
    print('finished!')
```

```
1 
2
```



<br>

# Built-in Data Types

1. List
2. String

<br>

## List

- List is the standard multi-element container in Python

```python
>>> a = ['p', 'r', 'o', 'b', 'e']
>>>
>>> a[0]
'p'
>>> a[4]
'e'
>>> a[-1]
'e'
>>> a[-5]
'p'
```

<br>

### List is mutable(값을 바꿀 수 있는)

tuple, string은 인덱스로 값을 바꿀 수 없는 immutable data type이다.

<span style="color:red">리스트의 값을 하나만 바꾸는 것은 만약 그 리스트의 이름이 a라면 a의 address는 기존의 address와 다르지 않지만 리스트를 새로 할당하면 주소가 바뀌게 된다.</span>

기존의 리스트는 anonymous object가 되고 이는 메모리 낭비를 하기 때문에 이를 수집하여 없애는 garbage collection이 존재한다.(파이썬 시스템에서)

```python
a = [1,2,3]
print(id(a))

#modify the object
a[2] = 9
print(a, id(a))

#assign a new object
a = [1, 2, 9]
print(id(a))
```

```
2083801980552
[1, 2, 9] 2083801980552
2083801479944
```



<br>

### List is iterable(순회하며 값을 가져올 수 있는)

- An iterable object is capable of returning its items one at a time
- Iterable objects can be used in
  - for loops
  - <mark>map() functions</mark>

```python
a = [1, 2, 3, 'four']
for i in a:
    print(i, end=' ')
```

```
1 2 3 four
```

<br>

### List slicing

```python
>>> a = ['p', 'r', 'o', 'b', 'e']
>>>
>>> a[1:4]
['r', 'o', 'b']
>>>
>>> a[:3]
['p', 'r', 'o']
>>>
>>> a[::-1]
['e', 'b', 'o', 'r', 'p']
```

slicing된 리스트들은 다시 어떤 변수에 넣지 않는 이상 새로운 object가 만들어진 것이기 때문에(id가 다른) anonymous object 가 된다.

<br>

## add/change List elements

```python
>>> a = [2,4,6]
>>>
>>> a[0] = 1  # change an item
>>> a
[1, 4, 6]
>>> a[1:3] = [3, 5]	# change a range of items
>>> a
[1, 3, 5]
>>> a.append(7)	# add one item
>>>a
[1, 3, 5, 7]
>>>a.extend([9, 10]) # add multiple items
>>>a
[1, 3, 5, 7, 9, 10]
>>>
>>> a.append([13,15]) # add one item
>>>a
[1, 3, 5, 7, 9, 10,[13, 15]]
```

<br>

## delete/remove List elements

```python
>>> a = ['a', 'b', 'c', 'd', 'e']
>>> del a[2] # delete one item
>>> a
['a', 'b', 'd', 'e'] 
>>> del a[1:3]	# delete multiple items
>>> a
['a', 'e']
>>>
>>> a = ['a', 'b', 'c', 'd', 'e']
>>> a.remove('c')	# remove one item
>>> a
['a', 'b', 'd', 'e']
>>>
>>> a.pop(2) # remove and return one item , 추출
'd'
>>> a.pop() # remoe and return the last item: stack에서 pop의 정의 
'e'
>>> a
['a', 'b']
>>> a.clear() # empty a list
>>> a
[]
```

<br>

## String is immutable and iterable

스트링은 그 안의 값을 배열의 인덱스로 접근하여 값을 바꾸는 행위가 불가능하나 하나하나씩 순회는 가능하다.

```python
data = ['have', 'a', 'nice', 'day']
one_string = ' '.join(data)
print(one_string)
```

```
have a nice day
```



<br>

## Functions

```python
def absolute(num): #absolute라는 변수가 함수 object를 가리키고 있는 것
    if num >= 0:
        return num
    else:
        return -num
    
print(absolute(2), absolute(-3))
#2 3
```

absolute(2) 에서 안에 넣어주는 인자가 argument

def absolute(num)에서 안에 넣어주는 인자는 parameter



<br>

### local/global variable

```python
def test():
    a = 512
    print(a, id(a)) #local variable

a = 512 #global variable
print(a, id(a))
# 512 2297106833872

test()
# 512 2297106833776
```

문법상 문제는 없지만 되도록 오해를 방지하기 위해서 local과 global 변수를 같은 variable name은 피하도록 해야한다.



### Function parameters

함수로 넘긴 인자의 값이 바뀌게 할 경우

```python
#All parameters in Pythion are passed by reference

def list1(a):
    a.append(9)
    print('inside the function: ', a)
    
b = [1, 2, 3]

list1(b)
#inside the function: [1, 2, 3, 9]

b
#[1, 2 ,3, 9]
```

<br>
함수로 넘긴 인자의 값이 바뀌지 않길 바라는 경우

```python
#Reference link is broken if we assign a new object

def list2(a):
    a = a[:] #local 변수로 뒤집어 씌운다.
    a.append(9)
    print('inside the function: ', a)
    
b = [1, 2, 3]

list2(b)
#inside the function: [1, 2, 3, 9]

b
#[1, 2 ,3]
```

list2 함수에서 만들어졌던 리스트는 함수가 종료후 garbage(anonymous object)가 된다

<br>

### Multiple return values

```python
def power(num):
    return num**2, num**3

a, b = power(2)

print(a, b)
#4 8

a = power(3)
print(a)
#(9, 27)
```

multiple return values는 tuple 형식으로 반환된다.

<br>

## Input

- input() function **read a line** from the standard input(keyboard, and returns it as a **string**.)

```python
>>> a = input()
Hello!
>>> type(a)
<class 'str'>
>>> a
'Hello!'
>>>
>>> #read a number from the keyboard
>>> n = int(input())
123
>>> type(n)
<class 'int'>
>>> n
123
```



# 수업 실습 관련

백준 사이트 이용

![image](https://user-images.githubusercontent.com/79521972/157386280-cdf8fd6c-9e2a-4841-befe-8fcc4c6b83f6.png)

- 제한 조건 중 시간 제한이 매우 중요



<br>

# Python Classes

1. Class and object
2. Defining a class
3. Modules



<br>

## Data types

- data type consists of two parts:
  - a set of values
  - functions that can be performed on the values

![image](https://user-images.githubusercontent.com/79521972/163695221-97e809ad-6100-4d90-afbe-79d38f4e308c.png)

<br>

## Classes and Objects

- **Class** is the definition of **a data type** 
  - A class has its own values and functions
  - Built-in classes: int, float, string, etc.
  - We can create a new class -> creating a new data type

- Object is variable of the data type
  - A value in memory that is created by the class definition
  - Instance (or entity) of the class

![image](https://user-images.githubusercontent.com/79521972/163695258-46e64595-43da-4b97-8081-84bf282d7bf9.png)



<br>

## Examples of (User-defined) Classes

![image](https://user-images.githubusercontent.com/79521972/163695263-1f535c4f-483b-4cee-92e5-abae5c06bb5e.png)



### Defining a class

![image](https://user-images.githubusercontent.com/79521972/163695277-5a25cdd5-61cc-46ef-bb70-9ad0c2f441a0.png)

### Making objects of a class

![image](https://user-images.githubusercontent.com/79521972/163695281-76556b52-445f-48ab-b5dc-7880b7adbec3.png)



<br>

## Inheritance

Inheritance는 새로운 class(즉, derived class로 알려진) 가 base class라고 불리는 다른 class로부터 만들어지는 과정이다.

A derived class는 자동으로 base class가 가진 부수적인 모든 member variables과 모든 ordinary member functions을 가지고, 추가적인 member functions과 추가적인 member variables을 가질 수 있다.

일반적으로, 새로운 classes는 inheritance를 통해 더 빠르고 쉽고, 더 싸게 만들어질 수 있다.(그것들을 scratch를 통해 쓰는 것보다)

Inheritance는 software 재사용의 idea를 돕는 한 가지 방법이다.

![image](https://user-images.githubusercontent.com/79521972/163695349-4916cc1e-2040-4e5b-ba1b-9d0bd2f3d66b.png)



<br>

## Save the class definitions

![image](https://user-images.githubusercontent.com/79521972/163695359-801a3896-0cb7-4e19-8c1c-af00c6116ee9.png)



<br>

## Python Modules

- Module is a file with .py extension
- It contains functions and/or classes you want to include in your application
- There are several ways to import modules:
- import \<module>

![image](https://user-images.githubusercontent.com/79521972/163695372-9d28411f-cb4b-4baa-ab69-51fb954cb330.png)

- import \<module> as \<alias>

![image](https://user-images.githubusercontent.com/79521972/163695382-9e9adaaa-2cce-4752-a586-bdf9b2b198ee.png)

- from <module> import <class/function>

![image](https://user-images.githubusercontent.com/79521972/163695394-d8d5beb8-c273-458c-8233-01ce63803abd.png)

<br>

## Module-related terms(module관련 용어 정리)

- Script
  - A Python (.py extension) file that’s intended to be run directly
- Module
  - A Python (.py extension) file that contains collections of functions, global variables, and classes
- Package
  - A directory that has a collections of modules

- Library
  - A generic term that loosely means “a bundle of code” 
  - It can have tens or even hundreds of individual modules





