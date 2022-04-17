---
layout: single
title: "[python] Python objects"
categories: ['Computer Science', 'Data structures and algorithms with Python', 'Language', 'Python']
tag: ['Object', 'Python']
---

<br>

## Object

- (general meaning)
  - a material thing that can be seen and touched
- in programming 
  - a combination of variables and functions that is in memory
- Everything in Python is an object
  - integers 
  - strings
  - functions
  - files
  - etc.



<br>

### A Python Integer is more than just an integer

The standard Python implementation is written in C. This means that every Python object is simply a cleverly disguised C structure, which contains not only its value, but other information as well. For example, when we define an integer in Python, such as x = 10000, x is not just a 'raw' integer. It's actually a pointer to a compound C structure, which contains several values.



![image](https://user-images.githubusercontent.com/79521972/163694540-ae657d21-c5d1-49fd-809f-b4ab5740b9e0.png)



<br>



![image](https://user-images.githubusercontent.com/79521972/163694549-1406bf02-4d95-4a70-981b-1cc87b7ade73.png)





<br>

## Python variables

Variables are labels that are attached to the objects. Variables are not objects nor containers for objects; they only act as a pointer or a <mark>reference </mark>to the object.

![image](https://user-images.githubusercontent.com/79521972/163694593-873fc207-672b-43a7-bfe4-c19f2439d49f.png)

= 연산을 하면 단순히 값을 복사하는 것이 아니라, 가리키는 pointer 자체를 옮기는 것이다.



<br>

## Immutable objects

- An immutable object cannot be changed after it is created 
- Immutable objects in Python: int, float, string, etc.

![image](https://user-images.githubusercontent.com/79521972/163694621-21df1088-9be1-40ec-bfc6-be2e2329c804.png)

- <span style="color:red">Reference link is broken if we assign a new object</span>



<br>

## Data types

- Each object has a data type (for example, int, float, etc.)
- The variable which points to the object does not have a specific type
  - No need to declare data types for variables



![image](https://user-images.githubusercontent.com/79521972/163694640-b4a87ddb-5e76-450e-a236-76adeb200dd8.png)

- python에서는 변수를 만들어서 데이터를 저장할 때 데이터 타입을 지정해 주지 않는다.
  - 우리가 저장하려는 데이터를 전달하면, 그 타입에 따라서 object가 만들어지고 그 object에 의해 data type이 결정 된다.
  - 그래서 변수라는 애는 단순히 그 object를 가리키는 reference로써 기능을 하기 때문에 data type이 없는 것이다.

<br>

- Each object has a data type, but variables don't

![image](https://user-images.githubusercontent.com/79521972/163694669-cbb8b035-71d3-4486-820b-53ab93b9d5fe.png)

<br>

## Python Built-in Data types

![image](https://user-images.githubusercontent.com/79521972/163694686-069dc50f-2a4b-4984-b924-35476ac2e42e.png)











