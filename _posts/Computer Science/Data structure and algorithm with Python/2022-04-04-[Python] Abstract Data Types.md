---
layout: single
title: "[Python] Linked List"
categories: ['Computer Science', 'Data structures and algorithms with Python', 'Python']
tag: ['Abstract Data Types', 'ADT']
---



# Abstract Data Types(ADT)

1\) Encapsulation

2\) Inheritance

3\) Polymorphism

<br>

**Encapsulation**

멤버 함수의 구현과 object의 data의 구현이 알려지지 않도록 하기 위해 혹은 덜 중요하게 class 정의하는 것은 수많은 다른 terms에 의해 알려진다.(class를 사용하는 프로그래머들에게)

사용되는 가장 흔한 term은 information hiding, data abstraction, and encapsulation으로 각각은 class의 세부적인 구현이 그 class를 사용하는 프로그래머에게 숨겨졌다는 것을 의미한다.

이러한 원칙은 OOP의 주요한 기조 중에 하나이다.

1. 우리는 최소한 class 밖의 code가 class 안에서 선언된 변수의 값을 바꾸고 접근하는 것을 어렵게 해야한다.
2. 구현의 세부사항으로부터 class를 어떻게 사용하는지의 설명을 분리해야 한다.
   - 예컨데, class의 method가 어떻게 정의 되는지와 같은 것들 말이다.

<br>

## If the abstraction(encapsulation) fails

```python
class Dat:
    def __init__(self):
        self.month = 1
        self.day = 1
        
    def output(self):
        print(f'Month: {self.month}, Day: {self.day}')
        
        
today = Day()
today.month, today.day = 12, 37
today.output()
```

information hiding이 되지 않아서 누구나 접근이 가능하기 때문에 문제가 발생할 수 있다.

그래서 information hiding원칙을 지켜 마음대로 access하지 못하게 하고 사용자에게는 사용법만 알려주어야 한다.



<br>

## Objects interact only through the member functions.

object는 member 함수를 통해서만 access 가능

어떠한 Object는 시스템의 나머지로부터 encapsulated 되어야 한다.



**이러한 과정을 통해 얻는 장점은?**

1. 시스템을 설계한 사람의 의도대로 동작을 하게 할 수 있다.
   - 오작동을 방지한다.

2. interface (함수)를 이용하여 거대한 규모의 응용 프로그램을 만들었는데 클래스가 개선되기를 원할 때 아무리 수정을 해도 client 입장에서는 코드 사용법이 바뀌지 않는 다는 것이다.

![image](https://user-images.githubusercontent.com/79521972/161468148-4b0ec812-a219-43df-bb7a-5e33d5e89c2b.png)





<br>

## ADT(Abstract Data Type)

**Data type**은 이러한 value들에 정의된 <mark>기본 동작(basic operation)</mark>의 집합과 함께 <mark>value</mark>의 집합으로 구성된다.

또한 이는 그 type을 사용하는 프로그래머가 `value와 operation이 어떻게 구현되었는지의 세부사항`에 접근하지 않는다면 abstract data type이라고 불린다.

The predefined types(ex. int)는 모두 ADT이다. 당신은 operation(such as +, *)이 int type에서 어떻게 구현 되었는지 알지 못한다.

<br>

ADT를 만들기 위한 방법의 하나가 Class 인 것이다. (class가 encapsulation을 하기 때문에)

즉, 프로그래머가 정의한 타입(programmer-defined types)인 <mark>Classes</mark>는 ADT또한 될 수 있다.

다시말해서, operation이 어떻게 구현 되는지의 세부사항을 이 class를 사용하는 아무 프로그래머로부터 숨겨야만하고 덜 관련없게 해야하는 것이다.

<br>

어떤 class를 사용하는 프로그래머는 class의 data가 어떻게 구현되는지를 알 필요가 없어야 한다. data의 구현은 member functions의 구현처럼 숨겨져야 한다.



<br>

## model for an ADT

- Application program should have no knowledge of the data (structure)

![image](https://user-images.githubusercontent.com/79521972/161468848-eabeff24-3184-462c-90e7-14f2e9f4de40.png)





<br>

## ADT examples

So far, 우리는 string, lists, sets, and dictionaries의 built-in data type들을 봤었다.(추가적으로 decimal and fraction modules도 봤었다.)

이들은 종종 ADT 관점에 의해 묘사된다. ADTs는 data에 수행되어질 수 있는 operation들의 집합에 대한 수학적인 specifications으로 생각될 수 있다.

또한 이들은 their implementation이라기 보다는 their behavior에 의해 정의된다.

우리가 봤던 ADT에 덧붙여서, built-in data type들에대한 extension을 제공하는 몇가지 python library들이 있다.

![image](https://user-images.githubusercontent.com/79521972/161469272-eecfad61-a546-4d04-8606-775191f9d482.png)

<br>

## Data structure and ADT

Data structure란 접근과 수정을 용이하게 하기 위해 data를 `store and organize`하는 방법이다.

<br>

- Example: stack, queue, linked list, tree, etc

<br>

- Data structure를 만들 때 

  - ADT로 만들 수 있다. or

  - data type가 아닌 것으로 만들 수 있다.

- 어떤 ADT는 수많은 data structure를 포함될 수 있다.

<br>









