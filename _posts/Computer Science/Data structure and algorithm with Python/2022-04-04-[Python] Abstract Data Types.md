---
layout: single
title: "[Python] Abstract Data Types"
categories: ['Computer Science', 'Data structures and algorithms with Python', 'Python']
tag: ['Abstract Data Types', 'ADT']
---



# Abstract Data Types(ADT)

1\) Encapsulation

2\) Inheritance

3\) Polymorphism

<br>

**Encapsulation**

멤버 함수의 구현과 object의 data에 대한 구현이 class를 사용하는 programmer들에게 알려지지 않도록 혹은 적어도 무관하도록 class 정의하는 것은 여러 용어로 알려져 있다.

사용되는 것 중에 가장 흔한 용어는 **information hiding**, **data abstraction**, and **encapsulation**이고 이들 각각은 어떠한 class의 구현의 세부사항이 그 class를 사용하는 programmer로부터 숨겨져 있다는 것을 의미한다.

이러한 원칙은 OOP의 주요한 기조 중에 하나이다.

1. 우리는 적어도 **class 바깥**의 code가 **class 안**에서 선언된 변수의 값을 바꾸거나 접근하는 것을 어렵게 해야한다.
2. 구현의 구체적인 내용으로부터 class를 어떻게 사용하는지와 같은 설명을 분리해야 한다.
   - 구체적인 내용이라 함은 예컨데, class의 method가 어떻게 정의 되었는지와 같은 것들을 말한다.
   - you separate the description of how to use a class from the implementation details, such as how the class methods are defined.

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

- object는 member 함수를 통해서만 access 가능

Object는 시스템의 나머지로부터 encapsulated 되어야 하고 그 object가 제공하는 서비스를 정의하는 구체적인 여러 함수들을 통해서만 프로그램의 다른 부분과 상호작용 해야 한다. 

<br>

**이러한 과정을 통해 얻는 장점은?**

1. 시스템을 설계한 사람의 의도대로 동작을 하게 할 수 있다.
   - 오작동을 방지한다.

2. interface (함수)를 이용하여 거대한 규모의 응용 프로그램을 만들었는데 클래스가 개선되기를 원할 때 아무리 수정을 해도 **client 입장에서는 코드 사용법이 바뀌지 않는 다는 것이다.**


![image](https://user-images.githubusercontent.com/79521972/161468148-4b0ec812-a219-43df-bb7a-5e33d5e89c2b.png)



<br>

## ADT(Abstract Data Type)

**Data type**은 이러한 value들에 정의된 <mark>기본 동작(basic operation)</mark>의 집합과 함께 <mark>value</mark>의 집합으로 구성된다.

또한 이는 그 type을 사용하는 프로그래머가 `value와 operation이 어떻게 구현되었는지의 세부사항`에 접근하지 않는다면 abstract data type이라고 불린다.

The predefined types(ex. int)는 모두 ADT이다. 우리는 operation(such as +, *)이 int type에서 어떻게 구현 되었는지 알지 못한다.

<br>

ADT를 만들기 위한 방법의 하나가 Class 인 것이다. (class가 encapsulation을 제공하기 때문에)

즉, 프로그래머가 정의한 타입(programmer-defined types)인 <mark>Classes</mark>는 ADT 또한 될 수 있다.

다시말해서, operation이 어떻게 구현 되는지의 세부사항을 이 class를 사용하는 어떠한 프로그래머로부터 숨겨야만하고 그들과 덜 관련없게 해야하는 것이다.

<br>

어떤 class를 사용하는 프로그래머는 class의 data가 어떻게 구현되는지를 알 필요가 없어야 한다. data의 구현은 member functions의 구현처럼 숨겨져야 한다.



<br>

## model for an ADT

- Application program should have no knowledge of the data (structure)
  - application program은 데이터의 지식을 갖지 않아야 한다.


![image](https://user-images.githubusercontent.com/79521972/161468848-eabeff24-3184-462c-90e7-14f2e9f4de40.png)





<br>

## ADT examples

지금까지 우리는 string, lists, sets, and dictionaries의 built-in data type들을 봤었다. (추가적으로 decimal and fraction modules)

이들은 종종 ADT 관점에 의해 묘사된다. ADTs는 data에게 수행되어질 수 있는 operation들의 집합에 대한 수학적인 specifications으로 생각될 수 있다.

또한 이들은 their implementation이라기 보다는 their behavior에 의해 정의된다.

우리가 봤던 ADT에 덧붙여서, built-in data type들에대한 extension을 제공하는 몇가지 python library들이 있다.

![image](https://user-images.githubusercontent.com/79521972/161469272-eecfad61-a546-4d04-8606-775191f9d482.png)

다양한 application에서 유용하다고 판명된 몇 가지 흔한 ADTs의 리스트가 위에 있다.

이러한 ADT 각각은 다양한 방식과, 다른 종류로 정의 될 수 있다. (다 똑같이 필수적인 건 아니지만)

<br>

## Data structure and ADT

Data structure란 접근과 수정을 용이하게 하기 위해 data를 `store and organize`하는 방법이다. 

어떠한 single data structure도 모든 목적을 잘 수행할 수는 없다. 그렇기 때문에 그것들의 몇가지 장점과 단점을 아는 것이 중요하다.

<br>

- Example: stack, queue, linked list, tree, etc

<br>

- Data structure를 만들 때 

  - ADT로 만들 수 있다. or

  - data type이 아닌 것으로 만들 수 있다.

- 어떤 ADT는 수많은 data structure가 포함될 수 있다.

<br>









