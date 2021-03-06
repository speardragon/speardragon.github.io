---
layout: single
title: "[Computer Architecture] 03.08 컴퓨터 구조"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



## Review)

instruction: 컴퓨터가 알아듣도록 컴퓨터에게 내리는 명령, 기계가 알아듣도록 하는 기본 function

- turn left, turn right, ...(기본적인 동작부터 복잡한 동작까지 있음)
- 이것이 명령어



hareware: processor가 어떻게 만들어져있는지



instruction set (spec,스펙): 하드웨어가 어떻게 만들어져 있는지

Instruction Set Architecture(ISA): Hardware와 Software의 bridge 역할



microarchitecture

<br>

RISC-V 새로 나옴, Open source로 공개 되어 있다.

- 원하는만큼 processor를 만들 수 있다.





우리 수업에서는 MIPS를 사용할 것임

---

<br>

**목차**

0. Performance(중요)
   - 하드웨어가 얼마나 잘 만들어져 있는지, 잘 만든다는 게 무엇일까?

1. Instruction Set
2. 1에 대한 Micro Architecture(Hardware)
3. Memory
4. I/O
5. Parallel-computer

<br>

## Computer Architecture

- #### What is Computer Architeture?
  
  - <span style="color:blue">a set of rules</span> starting how software and hardware join together and interact to make them work
  - <span style="color:blue">Structure and behavior</span> of the computer as seen by the user(programmer)
  - <span style="color:blue">specification</span> describing how hardware and software technologies interact to create a computer platform of system.(spec이라고도 함.)
  
- #### Three categories of computer architecture 
  
  - **lSA** (instrsuction set architecture)
  - **Microarchitecture** (computer organization): tells how ISA are implemeted(하드웨어)
  - **System design**: includes all of the other hareware components(DMA, virtualization, multiprocessing, GPU, memory controller, etc.)
  
- #### CISC and RISC
  
  복잡하고 간단한 명령들이 나열되어 있음
  
  - CISC (complex instruction set architecture)
    - many complex instructions(fewer lines of code, less memory, but take longer to complete instructions)
    - 자주 쓰이는 것이 있고 자주 쓰이지 않는 것이 공존하여 굉장히 복잡한 것
    - ex) x86 
  - RISC (reduced  instruction set architecture)
    - keep hardware as simple and fast as possible(complex instrcutions can be performed with simpler instruction)
    - 자주 쓰이지 않는 것은 만들지 않고 자주(많이) 쓰이는 것들로만 구성한 것
    - 그럼 자주 쓰이지 않는 것은 어떻게 만들까?
    - 이미 있는 간단한 instruction들을 합쳐서 복잡한 것을 만드는 개념
    - ex) ARM, MIPS, RISC -V
    - 명령어들이 x86에 비해 set이 훨씬 작음 
  
- #### Von Neumann architecture and Harvard architecture
  
  - **Von Neumann** : `instructions` and `data` are both loaded into the same memory unit
  - **Harvard** : keeps intructions and data in seperate memories (separated buses)

---

<br>

```c
// data 부분(선언(declare) 부분)
int a,b,c

//program 부분(text, code)
main()
...
a=b+c;
...
```

- 위 코드가 컴퓨터에서는 어떻게 실행될까?
- CPU를 통해 +, -, *, /, and, or 등의 연산을 하고 싶음 -> ALU에서 연산
- 폰노이만 구조는 data와 프로그램이 한 구조에 있음
- **메모리는 세 부분으로 나뉘어짐**(데이터가 선언될때 메모리가 필요함)
  - 맨 위 : fff...f 번지, stack memory
    - 스택 = temporary
      - 즉, 임시로 잠깐 저장할 때 사용하는 공간
  - 중간: data memory
    - 데이터가 들어가는 자리.
    - 컴파일이 되면, a, b, c, 방을 잡아준다.(선언 부분, 어디에 잡아줄 지는 compiler가 결정)
  - 맨 아래: 0 번지, program memory
    - 프로그램이 들어있음
    - instruction set이 들어있는 곳
    - instruction에 따라서 수행할 연산을 program counter가 decode하여 전달해줌
    - 컴파일러에 의해 high level language에서 컴퓨터가 알아듣는 assembly language로 변환된 후 이곳(program memory)으로 들어온다.



<br>

b와 c를 연산하고 싶기 때문에 이를 ALU에 연결하고 싶으나 RISC는 이를 직접 바로 연결하지 못하고, register file(여러개의 register를 모아둔 곳) 을 거쳐서 ALU로 넘겨준다. 또한 이 결과는 다시 register file을 거쳐서 memory로 들어간다.

- load: memory의 데이터를 register로 가져오는 행위

- store: register에서 ALU로 보내 연산을 마친 결과 데이터를 다시 memory로 보낸다.

<br>
CISC는 다양한 방법을 제공하지만 RISC는 무조건 register를 통해서 들어가야 한다. 그래서 RISC를 load-store architecture라고도 한다.

<br>

컴파일러가 high-level언어를 machine이 이해하는 language로 변환 아래와 같이

```assembly
load b -> rf1 #register file_1
load c -> rf3
addf rf1 + rf3 -> rf5
store rf5 -> a
```

위처럼 하나하나의 **instruction**으로 바꿔준다.

![image](https://user-images.githubusercontent.com/79521972/158312148-c799a509-16ff-472d-afa3-6b2fda80db8f.png)

- data path (register file + ALU)
  - data memory에 있던 data가 register로 들어가고(load 과정) ALU로 가서 연산 결과를 다시 register로 보내서 이를 다시 data memory로 전달함(store 과정)
- control(Instruction Register + Decoder)
  - instruction을 하나씩 control 부분의 IR(Instruction Register)에 가져와서 무슨 명령어인지 분석을 진행한다.
  - 근데 instruction을 하나씩 가져오려면 그것들의 위치를 알아야 하는데 이 위치를 알도록해 주는 게 Program Counter(PC)이다.
    - 주소는 4씩 차지하는 공간이기 때문에 4씩 늘려가면서 접근하면 instruction 주소에 하나하나씩 접근할 수 있는 것이다.
  - program memory에서 instruction들을 **가져오는** 행위를 `fetch`라 한다.

- 그래서 load와 fetch는 둘 다 memory에서 CPU로 object를 가져오는 행위로 같아보이지만 그것이 data memory냐 program memory냐에 따라 다르다.
- CPU와 memory 사이에 cache라는 것이 존재
  - cache는 cpu chip 안에 들어있음
  - data용 cache와 instruction용 cache가 있음

<br>

---

## Classes of Computer

- **Personal computers**
  - General purpose, variety of software
  - Subject to cost/performance tradeoff
- **Server computers**
  - Network based(Google server, storage server,...)
  - High capacity, performance, reliability
  - Range from small servers to building sized
- **Super computers**
  - 기상청에서 슈퍼컴퓨터 사용
  - Type of server
  - High-end scientific and engineering calculations
  - Highest capability but represent a small fraction of the overall computer market
- **Embedded computer**(휴대폰, 냉장고 등에 들어있는 프로세서)
  - Hidden as components of systems
  
  - Stringent(순) <mark>power / performance / cost constraints</mark>
  
    -  배터리로 동작하기 때문에 power가 매우 중요
    - ARM processor가 power가 좋기 때문에 전세계적으로 사용하는 것
    - cost와 size는 직결
    - power ∝ V<sub>dd</sub><sup>2</sup> * f * C<sub>L</sub>
    - 컴퓨터 구조에서는 cost, power는 다루지 않을 것임(중요하긴 하지만 out of range)

<br>

## What  We Will Study (from COD)

- How programs are translated into the **machine language**
  - And how the hardware executes them
- The hardware/software interface
- <span style="color:red">What determines program **performance**</span>
  - And how it can be improved
- How hardware designers **improve** performance
- What is **parallel processing** (from COD)



power는 굉장히 중요하지만 hardware적 측면이 강한 부분으로 이곳에서는 다루지 않을 것이다. 

<br>

## Performance(from COD)

<mark>컴퓨터 구조에서 제일 중요하게 다뤄질 부분</mark>



<br>

아래는 performance를 결정하는 중요한 요인들이다.

- performance is defined `1/실행시간(execution time)`
  - 즉, speed와 관련된 것

- **Algorithm**
  - Sorting(퀵 정렬, 버블 정렬,...) , Graph 등...
  - Determines # of operations executed
- **Programming language, compiler, architecture**
  - Determine # of machine instructions executed per operation
- **Processor and memory system**
  - Determine how fast instructions are executed
- **I/O system(including OS)**
  - Determines how fast I/O operations are executed

<br>

## Seven Great Ideas(from COD)

high speed CPU를 이룰 수 있던 motivation은 무엇일까?

- Use **abstraction** to simplify design

- Make the **common case fast**
  - 많이 사용되는 것들을 가장 빠르게 하는 것이 key idea
- Performance via **parallelism**
  - 부족하면 여러개를 이어붙여서 사용하는 것
- Performance via **pipelining**
  - 나중에 배움
- Performance via **prediction**
  - 다음 가져올 데이터를 미리 예측하여 미리 가져오는 것
- **Hierarchy** of memories
  - 하드디스크, main memory, cache memory, cpu
- **Dependability** via redundancy(중복성, 여분)
  - 데이터의 speed도 중요하지만 서버가 날라가면 큰일 나기 때문에 서버에 있는 컴퓨터들은 보통 하드디스크를 하나만 쓰지 않고 여러개 나눠 사용한다.

<br>

## Levels of Program Code (from COD)

Compiler에 의해 language를 machine이 알아듣도록 계속 변환 시킴

<br>

- **High-level language**

  - Level of abstraction closer to problem domain

  - Provides for productivity and portabiltiy

  - compiler에 의해 assembly language로 변환해 줌

  - ```c
    swap(int v[], int k)
    {
        int temp;
        temp = v[k];
        v[k] = v[k+1];
        v[k+1] = temp;
    }
    ```

- **Assembly lanuage**

  - Textual representation of instrcutions

  - compiler에 의해 machine language(Hardware representation)으로 변환해 줌

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

- **Hardware representation**

  - Binary digits(bits)

  - Encoded instructions and data

  - ```
    00000000000000000000011010100000100000000000100110
    101000000000000001111111000000000001111111000001
    010100000000000111111101010010
    ```

    

<br>























