---
layout: single
title: "[Computer Architecture] week 1-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Review)

instruction: 기계가 알아듣도록 기계에게 내리는 명령, 기계가 알아듣도록 하는 기본 function

- turn left, turn right, ...

instruction set : 하드웨어가 어떻게 만들어져 있는지

Instruction Set Architecture(ISA): Hardware와 Software의 bridge 역할

<br>

RISC-V 새로 나옴, Open source

우리 수업에서는 MIPS를 사용할 것임

<br>

**이번 시간에 배울 대충의 목차**

0. Performance
   - 하드웨어가 얼마나 잘 만들어져 있는지, 잘 만든다는 게 무엇일까?

1. Instruction Set
2. 1에 대한 Micro Architecture
3. Memory
4. I/O
5. Parallel-computer

<br>

## Computer Architecture

- What is

  - 

  - 

  - spec이라고도 함,

- Three categories of computer architecture 
  - lSA (instrsuction set architecture)
  - l
  - l

- CISC and RISC
  복잡하고 간단한 명령들이 나열되어 있음
  - CISC (complex instruction set architecture)
    - 자주 쓰이는 것이 있고 자주 쓰이지 않는 것이 공존하여 굉장히 복잡한 것
    - ex) x86 
  - RISC (reduced  instruction set architecture)
    - 자주 쓰이지 않는 것은 만들지 않고 자주 쓰이는 것들로만 구성한 것
    - 그럼 자주 쓰이지 않는 것은 어떻게 만들까?
    - 이미 있는 간단한 instruction들을 합쳐서 복잡한 것을 만드는 개념
    - ex) ARM, MIPS, RISC -V
    - 명령어들이 x86에 비해 set이 훨씬 작음 



```c
// data 부분(선언 부분)
int a,b,c

//program 부분(text, code)
main()
...
a=b+c;
...
```

- 위 코드가 컴퓨터에서는 어떻게 실행될까?

- CPU를 통해 +, -, *, / 등의 연산을 하고 싶음 -> ALU에서 연산
- 폰노이만 구조는 data와 프로그램이 한 구조에 있음

- ==**메모리는 세 부분으로 나뉘어짐**==
  - 맨 아래: 0 번지, program memory
    - 프로그램이 들어있음
    - instruction set이 들어있는 것
    - instruction에 따라서 수행할 연산을 program counter가 decode하여 전달해줌
  - 중간: data memory
    - 데이터가 들어간다.
    - a, b, c, 방을 잡아준다.(선언 부분, 어디에 잡아줄 지는 compiler가 결정)
  
  - 맨 위 : fff...f 번지, stack memory
    - 스택 = temporary
      - 즉, 임시로 잠깐 저장할 때 사용하는 공간

<br>

b와 c를 연산하고 싶기 때문에 이를 ALU에 연결하고 싶으나 RISC는 이를 직접 바로 연결하지 못하고, register file(여러개의 register를 모아둔 곳) 을 거쳐서 ALU로 넘겨준다. 또한 이 결과를 다시 register file로 가는 연결이 존재한다.

load: memory의 데이터를 register로 가져옴

store: register에서 ALU로 보내 연산을 마친 결과 데이터를 다시 memory로 보낸다.

> load-store architecture라고도 함.

CPU 와 register의 속도가 다름



컴파일러가 high-level언어를 machine이 이해하는 language로 변환 아래와 같이

```
load b -> rf1 #register file_1
load c -> rf3
addf rf1 + rf3 -> rf5
store rf5 -> a
```

위처럼 하나하나의 instruction으로 바뀐다.

data path: register file + ALU



<br>

## Classes of Computer

- Personal computers

  - General purpose, variety of software
  - Subject to cost/performance tradeoff

- Server computers

  - Network based(Google server, storage server,...)
  - High capacity, performance, reliability
  - ㅇ

- Super computers

  - Type of serve
  - High-end scientific and engineering calculations
  - 기상청에서 슈퍼컴퓨터 사용
  - Highest capability but represent a small fraction of the overall computer market

- Embedded computer

  - HIdden as components of systems

  - Stringent <mark>power / performance / cost constraints</mark>

    -  배터리로 동작하기 때문에 power가 매우 중요

    - ARM processor가 power가 좋기 때문에 전세계적으로 사용하는 것
    - power ∝ V<sub>dd</sub><sup>2</sup> * f * C<sub>L</sub>
    - 컴퓨터 구조에서는 cost, power는 다루지 않을 것임(중요하긴 하지만 out of range)



## Performance(from COD)

<mark>컴퓨터 구조에서 제일 중요하게 다뤄질 부분</mark>

<br>

아래는 performance를 결정하는 중요한 요인들이다.

- perf. is defined `1/실행시간(executed time)`

- Algorithm
  - Sorting(퀵 정렬, 버블 정렬,...) , Graph 등...
  - Determines # of operations executed
- Programming language, compilerm architecture
  - Determine # of machine instructions executed per operation

- Processor and memory system
  - Determine how fast instructions are executed
- I/O system(including OS)
  - Determines how fast I/O operations are executed

<br>

## Seven Great Ideas(from COD)

high speed CPU를 이룰 수 있던 motivation은 무엇일까?

- Use **abstraction** to simplify design
  - 
- Make the **common case fast**
  - 많이 사용되는 것들을 빠르게 하는 것이 key idea
- Performance via **parallelism**
  - 부족하면 여러개를 이어붙여 
- Performance via **pipelining**
  - 나중에 배움
- Performance via **prediction**
  - 다음 가져올 데이터를 미리 예측하여 미리 가져오는 것
- **Hierarchy** of memories
  - 하드디스크, main memory, cache memory, cpu
- **Dependability** via redundancy(중복성, 여분)
  - speed도 중요하지만 서버가 날라가면 큰일 나기 때문에 보통 하드디스크를 하나만 쓰지 않고 여러개 나눠 사용한다.



## Levels of Program Code (from COD)

Compiler에 의해 language를 machine이 알아듣도록 계속 변환 시킴

<br>

- High-level language

  - Level of abstraction closer to problem domain

  - Provides for productivity and psrtabiltiy

  - ```c
    swap(int v[], int k)
    {
        int temp;
        temp = v[k];
        v[k] = v[k+1];
        v[k+1] = temp;
    }
    ```

- Assembly lanuage

  - Textual representation of instrcutions

  - ```assembly
    swap:
    	muli $2, $5.4
    	add $2, $4.$2
    	lw $15, 0($2)
    	lw $16, 4($2)
    	sw $16, 0($2)
    	sw $15, 4($2)
    	jr $31
    ```

- Hardware representation

  - Binary digits(bits)

  - Encoded instructions and data

  - ```
    00000000000000000000011010100000100000000000100110
    101000000000000001111111000000000001111111000001
    010100000000000111111101010010
    ```

    


<br>
쿼터스는 























