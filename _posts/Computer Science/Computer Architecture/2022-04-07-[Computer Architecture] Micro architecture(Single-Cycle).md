---
layout: single
title: "[Computer Architecture] Microarchitecture (Single Cycle)"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



# Micro architecture

PC가 Instruction memory(IM)를 지정하면 거기에 있는 명령어가 fetch되어 IR로 읽어드림.





**lw $4, 4($5)**

4라는 constant는 IR에 들어있음

(사진)



위 과정을 1clock에 수행하는 것을 single cycle이라고 한다.



## Introduction

- Microarchitecture: how to  implement an architecture  in hardware
- Processor: 
  - Datapath: functional blocks 
  - Control: control signals

![image](https://user-images.githubusercontent.com/79521972/162123262-b3e2bb08-8745-4277-9bb2-96ef88adacf0.png)



<br>

## Microarchitecture

- Multiple implementations for a single  architecture: 
  - Single-cycle: Each instruction executes in a  single cycle 
  - Multicycle: Each instruction is broken into series  of shorter steps 
  - Pipelined: Each instruction broken up into series  of steps & multiple instructions execute at once







## Processor Performance

- Program execution time
  - Execution Time = (#instructions)(cycles/instruction)(seconds/cycle)

- Definitions:
  - CPI: Cycles/instruction 
  - clock period: seconds/cycle
  - IPC: instructions/cycle = IPC

- Challenge is to satisfy constraints of: 
  - Cost 
  - Power 
  - Performance



## MIPS Processor

- Consider subset of MIPS instructions: – 
  - R-type instructions: *and, or, add, sub, slt* 
  - Memory instructions: *lw, sw* 
  - Branch instructions: *beq*







state logic vs. combinational logic





## Architectural State

- Determines everything about a processor:
  - PC
  - 32 registers
  - Memory



ALU, Decode : Combinational logic





## MIPS State Elements



![image](https://user-images.githubusercontent.com/79521972/162123912-b8dbf469-c43a-48df-88c3-0052082d1edd.png)

register는 state element이기 때문에 d와 q, clk, EN, Reset이 있다. 



clock edge에서 program counter가 바뀜



Instruction Memocy는 여러 memory 중에서도 Asynchronous ROM이다.(read만 가능)



data memory는 RAM(read/ write 모두 가능)



register file: read port가 두 개(ALU연산을 위하여)

- 무엇을 read?: A1, A2
- write port(WD3)
- A3: write address
- write은 아무 때나 하면 안 되기 때문에 WE이 들어올 때만 write



<br>

## Single Cycle





## Single-Cycle Datapath: ls fetch

STEP 1: Fetch instruction

`lw $3, 4($4)`

![image](https://user-images.githubusercontent.com/79521972/162124914-a7da0f4d-0d04-48c9-846a-34a4d0a8d01a.png)





## Single-Cycle Datapth: lw Register Read

STEP 2: Read source opernads from RF









## Single lw Register Read





## Single-Cycle Datapath: lw memory Read

- STEP 5: Read data from memory and write it back to register file





### sw

- Wr





write을 할 거냐 말 거냐 등 -> control



## R-Type

`add $3, $4, $5`





### beq

`beq $3, $4, Imm`





## Single-Cycle Processor (Data path)

![image](https://user-images.githubusercontent.com/79521972/162127622-e2f1200a-fb43-4628-877f-f7e22cb57e43.png)



`add $4, $5, $6` 이라는 명령어를 실행할 때 각 control 에는 어떤 신호가 들어있을 것인가? -> 시험문제

- reg : op+5+6+4+0+f
- A1 : 5
- A2: 6



![image-20220407143612159](C:\Users\c_dragon\AppData\Roaming\Typora\typora-user-images\image-20220407143612159.png)

그림 수정

<br>

`lw $3, 8($4)`

![image](https://user-images.githubusercontent.com/79521972/162128281-b2ac954c-aba6-479f-901a-77738165b4f2.png)





<br>

`beq $3, $4, 5`

![image](https://user-images.githubusercontent.com/79521972/162128852-4b9a6599-7bc6-438e-9fea-18e2fdf50542.png)



---

Q) single cycle인데 clk이 3개

A)



Q) data memory에는 clock이 없는가?

---



