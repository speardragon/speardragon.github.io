---
layout: single
title: "[Computer Architecture] Multi-cylcle MIPS"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---

<br>



## Review: Processor Performance

**Program Execution time**

= (#instructions)(cycles/instruction)(seconds/cycle)

=  \# instructions x CPI x T<sub>C</sub>



<br>

### HDL description

![image](https://user-images.githubusercontent.com/79521972/163663941-1bd6af52-4b51-4e53-aded-d893480455ec.png)



![image](https://user-images.githubusercontent.com/79521972/163663964-e1630f8b-37e8-466e-9ba5-f27cdf12294f.png)

<br>



### Review: Processor Performance

- Program Execution Time 

  = (#instructions/program)(cycles/instruction)(seconds/cycle)

  = # instructions x CPI x T<sub>C</sub>

<br>



## Single-Cycle Performance

### Critical path

combinational logic에서 어떤 거는 빠르게 오고 어떤 거는 느리게 올텐데 이 중 가장 느린 경로가 critical path라고 한다.

![image](https://user-images.githubusercontent.com/79521972/163316568-c5716494-8f5a-4759-8c45-c3de1fb67105.png)

> T<sub>C</sub> limited by critical path (lw)
>
> clock period가 이를 수용할 수 있을 정도로 길어야 함.

<br>

## Single-Cycle Performance

- Single-cycle critical path:
  - T<sub>c</sub> = t<sub>pcq_PC</sub> + t<sub>mem</sub> + max(t<sub>RFread</sub>, t<sub>sext </sub>+ t<sub>mux</sub>) + t<sub>ALU </sub>+  t<sub>mem </sub>+ t<sub>mux </sub>+ t<sub>RFsetup</sub>
- Typically, limiting paths are: 
  - memory, ALU, register file
  - Tc = t<sub>pcq_PC </sub>+ 2<sub>tmem</sub> + t<sub>RFread </sub>+ t<sub>mux </sub>+ t<sub>ALU </sub>+ t<sub>RFsetup</sub>



- t<sub>sext</sub> : sign extention

<br>



### Example

![image](https://user-images.githubusercontent.com/79521972/163317091-e2320006-0d1c-4fdf-b88e-d079dd6acfd7.png)

T<sub>C</sub> = t<sub>pcq_PC </sub>+ 2<sub>tmem</sub> + t<sub>RFread </sub>+ t<sub>mux </sub>+ t<sub>ALU </sub>+ t<sub>RFsetup</sub>

 = [30 + 2(250) + 150 + 25 + 200 + 20] ps

 = 925ps



<br>

**Program with 100 billion instrucitons:**

Execution Time = # instructions x CPI x T<sub>C</sub>

​	= (100 x 10<sup>9</sup>) (1) (925 x 10<sup>-12</sup>s) 

​	= 92.5 seconds



<br>

## Single cycle의 문제점

명령어 별로 걸리는 시간차이가 다르기 때문에(lw는 엄청 느리고, J 나 beq는 굉장히 빠르다) 가장 긴 instruction 시간에 맞춰서 clock time을 정해야 하기 때문에 짧은 instruction은 클락이 끝나지 않았는데도 이미 벌써 끝나서 다음 클락을 기다리기 떄문에 비효율적인 상황이 발생한다.

그래서 이를 해결하기 위한 방법 두 가지를 앞으로 소개해 보도록 하겠다.

- Multi-Cycle
- Pipelining



<br>

## Multi-cycle MIPS Processor

- Single-cycle
  - simple
  - cycle time limited by longest instruction(lw)
  - 2 adders/ALUs & 2 memories
- Multicycle
  - higher clock speed 
  - simpler instructions run faster 
  - **reuse** expensive hardware on multiple cycles (IM, DM, ALU 등)
  - sequencing overhead paid many times

- Same design steps: datapath & control

<br>

## Multicycle State Elements

- Replace Instruction and Data memories with a **single unified memory** - more realistic

![image](https://user-images.githubusercontent.com/79521972/163318235-4f83f79c-64c3-4b23-812a-ebedc5e426d2.png)



<br>

## Multicycle Datapath

<img src="https://user-images.githubusercontent.com/79521972/163664613-fdb2c99d-de65-49f9-98dc-3995fd39767d.png" alt="image" style="zoom: 67%;" />

위와 같이 나누어서 생각해 보자.(골고루 나누어야 효율적이기 때문)

그러면 중간 중간 데이터를 담아야할 register를 끼워 주어야 함(clock이 지나면 잊어버리기 때문에 저장해 놓는 것)

<br>

### Instruction Fetch

**STEP 1**: Fetch instruciton

![image](https://user-images.githubusercontent.com/79521972/163318563-20683324-2094-4bee-94d9-37186b3f57e8.png)

- Instruction Fetch를 진행한다.

- Instruction Register에 저장.

- 이 때, ALU는 놀고 있기 때문에 program + 4도 진행한다.

<br>

### lw Register Read

**STEP 2a**: Read source operands from RF

![image](https://user-images.githubusercontent.com/79521972/163318650-50b76804-035f-4fac-bff4-9c7c3d0b05a8.png)

- opcode를 보고 이 코드를 통해 control에서 무슨 명령어인지 파악하여 각 control signal을 보내준다.

<br>

**STEP 2b**: Sign-extend the immediate

![image](https://user-images.githubusercontent.com/79521972/163318711-cdff79c6-1284-495b-800c-2ceb6cff3c7e.png)



<br>

### lw Address

STEP 3: Compute the memory address

![image](https://user-images.githubusercontent.com/79521972/163318760-c989c8e5-da19-472a-b497-468eb22d3fe6.png)

+ \+ 연산을 진행하여 결과를 register에 저장.



<br>

### lw Memory Read

STEP 4: Read Data from memory

![image](https://user-images.githubusercontent.com/79521972/163318817-65fc381f-74b8-4396-bd74-080fa6167986.png)

IorD에 따라 data memory에 저장해야 하기 때문에 IorD는 1이 될 것이고 이 결과가 data memory에 저장된다.

data memory에 있는 것을 읽어서 asych하게 **data register**에 넣어준다.



<br>

### lw Write Register

**STEP 5**: Write data block to register file

![image](https://user-images.githubusercontent.com/79521972/163318898-592f5575-5710-4d3c-85f0-a06d0b94580a.png)



<br>

### Increment PC

**STEP 6**: Increment PC -> 사실상 step 1에서도 가능하다 (step1(fetch)에서 ALU는 놀고있기 떄문에)

![image](https://user-images.githubusercontent.com/79521972/163318941-75401a83-a096-46c1-9ecf-54e7a8c70606.png)



<br>

## Multicycle Datapath: sw

Write data in `rt` to memory

![image](https://user-images.githubusercontent.com/79521972/163319230-0b43383e-4633-4a92-83d2-1dc064600bc8.png)

RF에 저장할 필요가 없기 때문에 4cycle이면 된다.



<br>

## Multicycle Datapath: R-Type

- Read from rs and rt
- Write ALUResult to register file
- Write to rd (instead of rt)

![image](https://user-images.githubusercontent.com/79521972/163319372-c75ed7cd-c410-41e2-8f63-7d0c1b1df1ce.png)



<br>

## Multicycle Datapath: beq

- rs == rt?
- BTA = (sign-extended immediate << 2) + (PC+4)

![image](https://user-images.githubusercontent.com/79521972/163319461-a42a7688-4c69-4d92-be12-3e0239f0c223.png)



3번째 clock에서는 ALU를 쓰고 있기 때문에(빼기 연산) BTA를 계산하는 과정을 3번째 clock에는 넣으면 안 된다.

그러므로 두 번째 cycle에서 BTA를 만들어 놓고 3번째 clock에서 두 값의 조건을 확인하는 용도로 ALU를 사용한다.



<br>

## Multicycle Processor

Finite State Machine이 사용됨

- lw 명령에서는 총 5번에 해당하는 cycle이 진행되어 각 클락마다, 즉 현재 어떤 상태이냐에 따라 값이 다를 수 있기 때문에 FSM을 사용하는 것이다.



![image](https://user-images.githubusercontent.com/79521972/163319943-0a3c6e37-1aed-4115-bd87-43eb7917b054.png)



<br>

### Multicycle control

Control에는 **Mux control**과 **Write enable 신호**가 있다.

![image](https://user-images.githubusercontent.com/79521972/163319968-2d16856f-81aa-4f71-a46b-0273be9288da.png)



<br>

## Main Controller FSM: 

### Fetch

- **MUX select signals** are listed only when their value matters; otherwise, they are  don't cares.
- Enable signals are listed only when they are asserted; otherwise, they are 0.

![image](https://user-images.githubusercontent.com/79521972/163320341-b9098d78-663d-4ca7-ab21-2d58dee31868.png)



### Fetch

IR <- Mem[PC]    // IorD = 0 , IRWrite

PC <- PC + 4     //AluSrcA, ......

![image](https://user-images.githubusercontent.com/79521972/163320381-9f302d66-0863-41bd-bce0-f615fcc5bfce.png)

___

Q) PC + 4는 첫번째 cycle에서 진행



---



<br>

### Decode

A <- RD[A1]

B <- RF[A2]

![image](https://user-images.githubusercontent.com/79521972/163320512-6b024061-4437-4127-be2c-4e6d5bf6695f.png)



control 신호가 필요없어서 안 씀



<br>

### Address

![image](https://user-images.githubusercontent.com/79521972/163320826-761c5989-975c-4c5e-a42d-0fc48a41098d.png)





<br>

ALUResult <- A + Imm

![image](https://user-images.githubusercontent.com/79521972/163320906-af47a9a6-bbaf-4376-81aa-73a52853b231.png)



<br>

### lw

![image](https://user-images.githubusercontent.com/79521972/163320993-9691fc38-f9a4-4329-9e48-ef2414b4fcae.png)

- 5 clock 걸림

Data <- Mem[ALUout]         :4번째

RF[A3] <- Data						: 5번째



<br>

### sw

![image](https://user-images.githubusercontent.com/79521972/163321169-eb29fbb9-da87-40bc-aa3a-9ada7138e4d2.png)



<br>

### R-Type

![image](https://user-images.githubusercontent.com/79521972/163321190-12038b72-9d49-450a-9c3a-154a22f8aeb9.png)

state에 따라 각각의 control signal이 다르기 때문에 FSM 사용하는 것.

<br>

### beq

ALUout <- PC + 4 + Imm << 2        //BTA 계산

if (A == B) PC <- ALUout



![image](https://user-images.githubusercontent.com/79521972/163321398-a5f42e57-717c-4815-b4d5-03ba489c06b3.png)



<br>

![image](https://user-images.githubusercontent.com/79521972/163321552-d17db61e-fb61-4825-9632-306b32b2aca1.png)



<br>

### Extended Functionality: addi

![image](https://user-images.githubusercontent.com/79521972/163321595-b80cc480-a235-4b79-b38e-88e8785c25e5.png)





<br>

## Main Controller FSM: addi

![image](https://user-images.githubusercontent.com/79521972/163321678-9b4b3426-b3bd-402b-8626-938b4a040ed7.png)



<span style="color:red">시험문제에는 multi cycle이 안 나온다.</span>













