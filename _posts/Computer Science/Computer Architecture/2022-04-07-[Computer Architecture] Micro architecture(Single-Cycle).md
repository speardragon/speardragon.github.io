---
layout: single
title: "[Computer Architecture] Microarchitecture (Single Cycle)"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



# Micro architecture

![image](https://user-images.githubusercontent.com/79521972/162123262-b3e2bb08-8745-4277-9bb2-96ef88adacf0.png)



위 과정을 1clock에 수행하는 것을 single cycle이라고 한다.



## Introduction

- Microarchitecture: how to  implement an architecture  in hardware
- Processor는 다음 두 가지로 이루어져 있음.
  - Datapath: functional blocks 
  - Control: control signals



꼭 processor만이 아니라 어떤 hardware를 설계하더라도 디지털 시스템은 위와 같이 구성해야 한다.

<br>

## Microarchitecture

- Multiple implementations for a single  architecture: 
  - Single-cycle: Each instruction executes **in a single cycle** 
  - Multicycle: Each instruction is broken into series  of shorter steps 
  - Pipelined: Each instruction broken up into series  of steps & multiple instructions execute at once



<br>



### Processor Performance

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

<br>

### MIPS Processor

- Consider subset of MIPS instructions: – 
  - R-type instructions: *and, or, add, sub, slt* 
  - Memory instructions: *lw, sw* 
  - Branch instructions: *beq*
  - Jump: j

<br>

위와 같은 9개의 기능에 대하여 MIPS microprocessor를 구현해 보도록 하겠다.



### Single-Cycle implementation

<br>

Single cycle implementation이란 instruction 하나를 하나의 cycle에 수행하는 것을 말한다. 

Cylce이 끝나면 새로운 instruction을 수행한다.

<br>

Clock이 올라가서부터 다음 clock이 올라가기 전까지 하나의 수행을 마친다.

다음 clock이 올라가는 순간 instruction 수행 결과(state)를 update하고 , 새로운 instruction을 수행한다.



<br>

### Architectural State

- Determines everything about a processor:
  - PC: 새로운 PC로 update한다.
  - 32 registers: add, sub, lw 등
  - Memory(data memory): sw 등



ALU, Decode : Combinational logic

<br>

### MIPS State Elements



![image](https://user-images.githubusercontent.com/79521972/162123912-b8dbf469-c43a-48df-88c3-0052082d1edd.png)

register는 state element이기 때문에 d와 q, clk, EN, Reset PIN이 있다. 

- clock edge에서 program counter가 바뀜

- Instruction Memocy는 여러 memory 중에서도 Asynchronous ROM이다.(read만 가능)

- data memory는 RAM(read/ write 모두 가능)

- register file: read port가 두 개(ALU연산을 위하여)



- 무엇을 read?: A1, A2
- write port(WD3)
- A3: write address
- write은 아무 때나 하면 안 되기 때문에 WE이 들어올 때만 write



<br>

## Single Cycle MIPS Processor

- Datapath
- Control





<br>



## Single-Cycle Datapath: lw fetch

**STEP 1: Fetch instruction**

`lw $3, 4($4)`

![image](https://user-images.githubusercontent.com/79521972/162124914-a7da0f4d-0d04-48c9-846a-34a4d0a8d01a.png)



<br>



## Single-Cycle Datapth: lw Register Read

**STEP 2: Read source opernads from RF**

![image](https://user-images.githubusercontent.com/79521972/162152113-1c19ab56-51ea-4f5a-b65f-cc9af72f7526.png)





<br>

## Single-Cycle Datapth: lw Immediate

**STEP 3: Sign-extend the immediate**

![image](https://user-images.githubusercontent.com/79521972/162152239-6e1d92c7-8001-41f2-ab44-6245571e0d2e.png)



<br>

## Single-Cycle Datapth: lw address

**STEP4: Compute the memory address**

![image](https://user-images.githubusercontent.com/79521972/162152342-01c6f27b-8ba1-4a40-9391-848167e78cd1.png)



<br>

## Single-Cycle Datapth: lw Memory Read

**STEP 5: Read data from memory and write it back to register file**

![image](https://user-images.githubusercontent.com/79521972/162152459-794c0d55-d335-4f99-b44e-d235734ee63b.png)



<br>

## Single-Cycle Datapth: lw PC Increment

**STEP 6: Determine address of next instruction**

![image](https://user-images.githubusercontent.com/79521972/162152574-22ad8304-8a85-4a42-befe-c7c355b43c55.png)



<br>



## Single-Cycle Datapth: sw

**Write data in rt to memory**

![image](https://user-images.githubusercontent.com/79521972/162152657-f8ceeedd-3120-4f42-9c49-3c7b62680629.png)



<br>

## Single-Cycle Datapth: R-Type

- Read from rs and rt
- Write ALUResult to register file

- Write to rd (instead of rt)
- `add $3, $4, $5`

![image](https://user-images.githubusercontent.com/79521972/162152809-5e9f85f4-c430-4a1e-a240-e0174c13aba9.png)



<br>

## Single-Cycle Datapth: beq

- Determine whether values in rs and rt are equal
- Calculate branch target address:
  - BTA = (sign-extended immediate << 2) + (PC + 4)
- `beq $3, $4, Imm`



![image](https://user-images.githubusercontent.com/79521972/162152990-2c3d35cb-4084-4704-bd4e-42feb8ba5841.png)



<br>



## Single-Cycle Processor (Data path)

![image](https://user-images.githubusercontent.com/79521972/162127622-e2f1200a-fb43-4628-877f-f7e22cb57e43.png)



`add $4, $5, $6` 이라는 명령어를 실행할 때 각 control 에는 어떤 신호가 들어있을 것인가? -> 시험문제

- reg : op+5+6+4+0+f
- A1 : 5
- A2: 6



<br>

`lw $3, 8($4)`







<br>

`beq $3, $4, 5`



![image](https://user-images.githubusercontent.com/79521972/162155339-23cfcec3-c01d-47a8-b5a5-caad09895935.png)

---

Q) single cycle인데 clk이 3개

A)



Q) data memory에는 clock이 없는가?

A) Aysnc data 방식이기 때문

---



