---
layout: single
title: "[Computer Architecture] week 1-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



## Review)

![image](https://user-images.githubusercontent.com/79521972/157591408-f57299f3-b57e-459f-b18c-eb17239509b9.png)

- ```
  int a, b, c
  ```

  ```
  main
  ...
  a = b + c
  ```

- memory가 있고 선언한 데이터들이 data memory에 저장된다.

- RISC의 경우 memory에서 ALU에 <mark>바로 전달하지 않고</mark> 중간에 register file(bank)에 저장하는 과정이 있다.
- 연산을 하기 위해 compiler에 의해 만약 data b와 data c를 각각 빈 register 공간에 저장한다.
- register에 있던 b, c 는 ALU에 들어가 연산을 하게 되고 그 결과가 다시 register 의 빈 공간에 저장된다.
- 이 결과가 register 에서 다시 data memory로 전달되어 마무리 된다.

이러한 과정처럼 단순한 instruction들을 모아서 복잡한 기능을 하는데 이러한 기능을 모은 것을 Instruction Set Architecture라고 한다.

**ISA**

```assembly
ld [b] -> r2
ld [c] -> r3
r2 + r3 -> r10
store r10 -> [a]
```

위는 assembly 언어이지만 실제로 컴퓨터 메모리에는(특히 program memory) 이 IS들이 0과 1로만 구성된 것들이 저장되어 있다.

<br>

그런데 CPU 안에는 register로 바로 가기 전에 cache를 거쳐서 가게 된다. 이 cache는 받는 object에 따라 두 개로 나뉘어 존재하는데 하나는 data memory에서 받아오는 **Data Cache(DC)**, 또 하나는 program memory에서 받아오는 **Instruction Cache(IC)**가 있다.

그러면 IC에 존재하는 instruction을 하나씩 받아오기 위해서 memory의 주소를 알아야 하는데 이를 알려주기 위한 Program Counter(PC)가 존재한다.

정리하면 다음과 같이 되는 것이다.

- 맨 처음에 PC값을 setting한다.
  - 명령어 시작점은 항상 PC이다.

- 해당 값에 상응하는 instruction들을 IC에서 Instruction Register(IR)로 하나씩 가져온다. (<span style="color:red">fetch</span>)
- 그러고 나서 (Combinational)control logic 에서 이 값을 분석한다.(decoder 역할)
- 봤을 때 `데이터 b를 load` 이기 때문에 이에 대한 control 신호들이 막 날라간다. 
- 이 신호에 의해 DC(data cache)에 있던 b가 register 전달되어 저장된다.(<span style="color:red">load</span>)
- c도 마찬가지로 register에 전달되고
- b와 c가 ALU에 들어가 연산을 진행 후 나온 결과가 register로 전달되고
- 이 결과 data가 다시 DC의 a로 전달된다.(<span style="color:red">store</span>)

<br>

이러한 것을 설계하는 것이 목적이다.

CPU가 어떤 동작을 수행해야 하는가? -> ISA

<br>

그런데 이를 설계할 때 고려해야 하는 중요한 세 가지가 존재한다.

- <mark>performance(speed)</mark>
- area(cost) (area는 작아지면 빨라지기 때문에 perfomance와도 관련이 있다.)
- power(energy): power를 줄이려 해야함
  - power ∝ V<sub>dd</sub><sup>2</sup> * f * C<sub>L</sub>




#### performance

perf = $\frac{1}{exec time}$



#### Execution time

- ① Latency(response time) 
- ② Throughput

<br>

![image](https://user-images.githubusercontent.com/79521972/158405696-94b7a679-93de-44bc-9ed5-0c1a33ac5813.png)

case A: 사람이 위와 같은 통로를 지나는데 걸리는 시간을 10sec라고 하자.

![image](https://user-images.githubusercontent.com/79521972/158405925-f8cc4e92-5cdf-409d-9177-6dceb1c0937e.png)

case B: 또한 위와 같은 통로를 지나는데 중간 중간에 칸막이가 쳐져있고 이 칸막이 하나당 지나가는데 2sec가 걸린다고 하자.

<br>
이 때 두 경우의 latency는 모두 10sec이다. 한 사람이기 때문에 둘 간의 차이가 존재하지 않는 것처럼 보이나 한 번 100명의 사람이 위 두 통로를 지난다고 해 보자.

A의 경우 한 명당 10초가 걸려 1000초가 걸릴 것이다.

B의 경우 첫 사람은 10초가 걸리나 이 사람이 칸막이를 하나씩 지나면서 다른 사람들도 빈 곳에 계속해서 줄을 서있을 수 있게 된다. 그러므로 한 명이 빠져나가기 시작하면 2초 간격으로 한 명씩 빠져나갈 수 있게 된다.



#### case A: 10초에 사람 한 명(clock 하나 당 실행)

latency = 10 sec

total exec_time = 10 x 100 = 1000 (sec)

throughput = 0.1 (task/sec)

<br>

#### case B: 10초에 사람 5 명(clock의 주기가 위 보다 짧음)

어느 순간(10초 이후)에는 동시에 작업이 일어남

latency = 10 sec

total exec_time = 10 + (99 x 2) = 208 (sec) ≒ 200 (sec)

throughput = 0.5 (task / sec)

<br>

위 두 경우는 약 두 배 가량의 total exec_time 차이가 난다.

B의 경우를 pipelining이라고도 하며 해당 칸막이는 그냥 나오지 않고 이를 만드는 시간이 조금 걸리긴 하지만 명령어가 길때는 훨씬 더 효율적일 것이다.

caseB보다 caseA이 5배 만큼 높은 throughput(성능, performance)을 가짐 

<br>

**Throughput**(bandwidth) =  # of task / time unit(hour, sec)

<br>





<br>

---

<br>

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

## Inside the Processor(CPU) (from COD)

- Datapath: performs operations on data
- Control: sequences data path, memory
- Cache memory
  - small fast SRAM memory for immediate access to data

<br>

- Appl A12 Bionic Processor

<br>

## A safe Place for Data (from COD)

- Volatile **main memory**
  - Loses instructions and data when power off
- Non-volatile **secondary memory(storage)**
  - Magnetic disk
  - Flash memory
  - Optical disk (CDROM, DVD)

<br>

## Semiconductor Technology

- Silicon: semiconductor
- Add materials to transform properties:
  - Conductors 
  - Instulators
  - Switch



<br>

## Manufacturing ICs

![image](https://user-images.githubusercontent.com/79521972/157595430-1c2f2c1a-03b2-488d-b029-2f55f1f6efc5.png)

- silicon ingot을 자른다 -> wafer
- wafer를 design
- test를 통해 wafer상의 detect를 잘라서 버림
- custom에게 전달 전 최종 test
- custom에게 전달



<br>

반도체 공정의 중요 3요소

- fab
- design
- test

<br>

## Integrated Circuit Cost

![image](https://user-images.githubusercontent.com/79521972/158492340-bc2a7b04-2b0d-4b27-982d-9488018d9e90.png)

- Cost per die
- Dies per wafer
  - approximation
- Yield
  - empirical
  - $n$ related to number of critical processing steps(제곱으로 되어는 있지만 사실상 매우 중요하기 때문에 n제곱으로 생각할 수 있다. 공정에 따라서 5제곱이 되기도 함)
  - yield에 따라 안정되었냐 안 되었냐를 알 수 있음

칩의 크기에 따라 detected를 받아들이는 정도가 매우 다를 수 있다.(detect가 4개인데 칩의 크기가 매우 커서 wafer에 칩이 8개가 있다면 그 중에서 4개는 매우 결함이 있는 것이다.)

<br>

## Defining Performance

- Which airplane has the best performance?

![image](https://user-images.githubusercontent.com/79521972/158492868-0ca0d45d-8ad3-4f80-a66f-ea7699d76791.png)



## Response Time and Throughput

computer architecture 관점에서는 performance를 throughput으로 볼 것이다.



- Response time(latency)
  - How long it takes to do a task
- Throughput
  - Total work done per unit time
    - e.g., tasks/ transactions/ ... per hour
- How are response time and throughput affected by
  - Replacing the processor with a faster version?
  - Adding more processors?
- We'll focus on response time for now...
  - Throughput은 나중에 또 고려를 해 볼 것이다.











