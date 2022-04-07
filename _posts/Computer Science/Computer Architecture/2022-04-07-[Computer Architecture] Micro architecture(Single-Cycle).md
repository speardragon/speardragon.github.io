---
layout: single
title: "[Computer Architecture] Microarchitecture (Single Cycle)"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---





# 시작하기 앞서

micro architecture의 대략적인 큰 틀을 먼저 잡아보자.

<br>

- PC -> instruction memory, fetch instruction
- Register numbers -> register file, read registers
- Depending on instruction class
  - Use ALU to calculate
    - Arithmetic result
    - Memory address for load/store
    - Branch target address
  - Access data memory for load/store
  - PC <- target address or PC + 4

<br>

instruction이 instruction memory 상에서 4Byte 단위로 저장되어 있기 때문에 각 cycle마다 instruction을 PC에 의해 하나씩 가져오게 되는데 그래서 PC에 4를 더해야 그 다음 instruction memory address를 가리킬 수 있는 것이다.

<br>

## 구성

### 1. Datapath

:연산들을 수행하기 위한 여러 유닛들의 구조. MIPS연산에는 크기 5가지로 나누어진다.

① **I**nsturction **F**etch:(Instruction Memory) 

- 명령어를 메모리에서 CPU로 전달하는 과정
- PC가 가리키는 메모리 주소에 저장된 데이터를 반환하여 CPU는 이를 copy해서 Instruction Register로 그 명령(data)을 가져온다. 
- 현재 처리 중인 명령어는 IR에 저장된다.

![image](https://user-images.githubusercontent.com/79521972/162255854-7e3aa9d0-4a68-45f2-9a34-669d5e1c23d1.png)



② **I**nstruction **D**ecode & Read Register File:(Register File)

- 명령을 해석하고 필요한 레지스터 값을 읽는다.

![image](https://user-images.githubusercontent.com/79521972/162255895-9ee88682-6ffa-409a-9843-dc89da1e831a.png)

③ **E**xecute operation: (Arithmetic Logic Unit)

- 명령에 필요한 연산을 수행한다. 

![image](https://user-images.githubusercontent.com/79521972/162256012-2b626ee9-fda7-4d99-ad98-5778d054c3ff.png)

④ Read from **M**emory: 

- lw와 같이 메모리를 읽는 연산의 경우 메모리의 정보를 가져온다. (Memory)

![image](https://user-images.githubusercontent.com/79521972/162256364-866a7a94-626c-46de-9854-642a24b0d0ee.png)

⑤ **W**rite **B**ack to register file

- 명령의 결과를 필요로 할 경우 레지스터 파일에 쓴다. (Register file)

![image](https://user-images.githubusercontent.com/79521972/162256489-85f8bf37-ddb5-4068-96f0-5d1c1f735633.png)



<br>

# Micro architecture

![image](https://user-images.githubusercontent.com/79521972/162123262-b3e2bb08-8745-4277-9bb2-96ef88adacf0.png)



위 과정을 1clock에 수행하는 것을 single cycle이라고 한다.

<br>

위 과정은 Abstraction 된 것으로 실제와는 조금 다르다. 그렇지만 직관적이기 때문에 위의 그림으로 설명을 하자면 실행은 다음과 같이 이루어 진다.

1. PC(Program Counter)를 전달하고 PC는 PC + 4연산이 진행된다.
2. 해당 PC + 4 된 값은 또 가산기에 들어가서 업데이트 된다. (branch target address)
3. 2 개의 출력이 ALU에 각각 들어간다.
4. ALU의 출력이 다시 Data memory에 들어간다.
5. Data memory에서도 다시 출력이 레지스터로 들어간다.

<br>

이제 이 과정을 바탕으로 구조를 점점 구체화시켜 나갈 것이다.



## Introduction

- Microarchitecture: how to  implement an architecture in hardware
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

Single cycle implementation이란 **instruction 하나**를 **하나의 cycle**에 수행하는 것을 말한다. 

Cycle이 끝나면 새로운 instruction을 수행한다.

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

- clock edge에서 program counter가 바뀜

- Instruction Memory:
  - data가 들어오면 바뀔 필요가 없는 ROM 이다.
  - 여러 memory 중에서도 클락과 무관하게 address를 받으면 바로 output으로 나오는 Asynchronous ROM이다.(read만 가능)

- Register File
  - read port 두 개 사용
    - ALU에 두 개의 input을 받기 때문에
  - Address A1, A2를 받기(읽기) 위한 각각의 read port RD1, RD2가 존재한다.
  - Write port도 존재(WE, CLK에 의해)

- Data memory: 
  - read port가 두 개(ALU연산을 위하여)
  - 데이터를 넣었다 뺐다 자유자재로 바꾸어야 하기 때문에 RAM
  - WE(write enable)이 active 일 때만 write을 한다.
  - RAM(read/ write 모두 가능)



<br>

## Single Cycle MIPS Processor

- Datapath
- Control





<br>



## Reivew)

![image](https://user-images.githubusercontent.com/79521972/162221949-152b8e0d-115c-4842-bc3b-a80ae7335e99.png)



<br>

# lw

일단 load의 역할을 짚어보면 memory에 있는 data를 register file에 불러오는 것이다. 따라서 memory로부터 받은 정보를 저장할 곳과 memory에 저장된 값을 불러올 건지를 결정하는 부분이 정의되어야 할 것이다.

## Single-Cycle Datapath: lw fetch

**STEP 1: Fetch instruction**

`lw $3, 4($4)`

`lw rt, address(rs)`

![image](https://user-images.githubusercontent.com/79521972/162219565-6b4a32b0-c817-4667-a867-c8d2783faee7.png)

PC에서 받아온 값에 의해 IM에서 명령어가 결정된다.(fetch)

<br>



## Single-Cycle Datapath: lw Register Read

**STEP 2: Read source operands from RF**

![image](https://user-images.githubusercontent.com/79521972/162152113-1c19ab56-51ea-4f5a-b65f-cc9af72f7526.png)

I[25:21]은 memory의 주소로 Read input으로 들어간다.

즉, source register(rs; $3)를 읽어서 RF에 저장시킨다.



<br>

## Single-Cycle Datapath: lw Immediate

**STEP 3: Sign-extend the immediate**

![image](https://user-images.githubusercontent.com/79521972/162152239-6e1d92c7-8001-41f2-ab44-6245571e0d2e.png)

memory의 정확한 address 값을 얻기 위해서 immediate value를 구하여 rs와 더한 결과 값을 구해야 한다.

그런데 이 때 유의해야 할 것은 immediate value 자체는 16bit로 받아오는데 실제 주소의 위치는 32bit로 표현되어 16bit를 32bit로 바꿔주는 과정이 필요한데 이를 위하여 sign extend 모듈에서 해당과정을 수행한다.



<br>

## Single-Cycle Datapath: lw address

**STEP4: Compute the memory address**

![image](https://user-images.githubusercontent.com/79521972/162152342-01c6f27b-8ba1-4a40-9391-848167e78cd1.png)

위 과정을 통해 뽑아온 immediate 값을 사용하기 위해서는 추후에 나올 것이지만 위 그림과 같이 SrcB로 바로 들어가는 것이 아니라 control signal에 포함된 ALUSrc 값이 1로 세팅될 때 해당 값을 사용할 수 있다.

ALU를 통해 data memory address를 계산하여 값을 data memory로 넘긴다.

이 때 덧셈 연산을 해야하므로 ALUControl에 add 신호를 넣어주고 연산의 결과가 ALU의 output으로 나오는데 이 값이 바로 memory의 address이다.

<br>

## Single-Cycle Datapath: lw Memory Read

**STEP 5: Read data from memory and write it back to register file**

![image](https://user-images.githubusercontent.com/79521972/162152459-794c0d55-d335-4f99-b44e-d235734ee63b.png)



target register(데이터를 쓸 공간)인 rt는 Write input(A3)으로 들어간다.

그런 후에 data memory에서 앞서 구한 memory 주소를 바탕으로  해당 값을 찾은 후에 target register인 rt에 해당하는 값을 저장하게 된다.

lw는 RF에 write을 하는 것이기 때문에 RegWrite이 enable이 될 때 저장이 되고 memory로부터 data를 읽어서 RF의 WD3에 들어오고 rt에 해당하는 A3에 읽어온 데이터가 쓰인다.

<br>

## Single-Cycle Datapath: lw PC Increment

**STEP 6: Determine address of next instruction**

![image](https://user-images.githubusercontent.com/79521972/162152574-22ad8304-8a85-4a42-befe-c7c355b43c55.png)

PC가 다음 명령을 가져오기 위해서 PC 값에 4를 더하여 다음 명령을 fetch할 준비를 한다.

<br>

![image](https://user-images.githubusercontent.com/79521972/162253084-494fd202-1b3f-4da4-8698-4a7f48b6b5be.png)

<br>

---

## Single-Cycle Datapath: sw

**Write data in rt to memory**

![image](https://user-images.githubusercontent.com/79521972/162152657-f8ceeedd-3120-4f42-9c49-3c7b62680629.png)

sw(store word) 명령도 역할 자체가 target register의 값을 data memory에 저장하는 것이기 때문에 기본적인 구조는 lw와 비슷하다.

다만, sw는 data memory address를 연산하여 해당 address에다가 write하는 것이기 때문에 A2(rt)의 data를 read하여 MemWrite Control이 1이 될 때 memory에 값을 write한다.

<br>

![image](https://user-images.githubusercontent.com/79521972/162253143-6c2eadec-38ec-4ca0-ad25-cd83c8bb0ad8.png)

<br>

## Single-Cycle Datapath: R-Type

![image](https://user-images.githubusercontent.com/79521972/162230096-851c460f-a01e-4a84-97f3-98da49824d23.png)

- Read from rs and rt
- Write ALUResult to register file

- Write to rd (instead of rt)
- `add $3, $4, $5`

![image](https://user-images.githubusercontent.com/79521972/162152809-5e9f85f4-c430-4a1e-a240-e0174c13aba9.png)

rs에 해당하는 I[25:21]과 rt에 해당하는 I[20:16]을 Read Register에 넘겨준다.

또한 결과는 destination register인 rd에 저장할 것이기 때문에 저장할 rd도 A3에 저장된다. 

- 이 때 A3는 그냥 들어가는 것이 아니라 RegDst에 의해 결정된 값이 들어가게 되는데 instruction format에 따라서 나눠지는 bit수가 달라지기 때문이다.
- 무슨 말이냐 하면, 해당 control signal은 destination register로 사용할 것인지를 결정하는 것이므로 R-Type의 경우에만 1로 설정이 된다.

그리고 ALUControl에 의해서 무슨 연산을 할 건지를 빼오는데 add에 해당하는 control signal로 설정 후에 두 레지스터 rs, rt에 대해서 연산이 이루어지고 해당 결과가 memory를 거치지 않고(MemtoReg = 0) WD3에 들어와 이 내용이 rd(A3)에 쓰여진다.



**Q) R-Type의 경우 opcode가 0이고 funct을 봐야하는데 이 때는 어떻게 하죠?**

A) opcode와 funct에 의해 control signal이 결정됨

Q) funct이 없는 경우도 있는데 이 경우를 어떻게 판별??



![image](https://user-images.githubusercontent.com/79521972/162253385-7f5586b4-4c44-433e-a31f-80c915d431d8.png)

<br>

## Single-Cycle Datapath: beq

beq 명령어는 사실 직접적으로 memory access를 하지 않으면서도 I-Type instruction을 사용한다.

컨셉은 정상적인 수행일 때는 PC += 4가 행해지고,

만약 branch인 경우에는 지정된 주소를 기준으로 immediate 만큼 떨어진 곳으로 이동하게끔 된다. 그런데 이 때 PC는 4byte 단위의 값을 지정하기 때문에 immediate 값에 4를 곱해 주는 것이 key point 이다.

- Determine whether values in rs and rt are equal
  - equal하면 zero를 의미한다. -> 두 값을 뺀 걸 볼 것이기 때문

- Calculate branch target address:
  - BTA = (sign-extended immediate << 2) + (PC + 4)
- `beq $3, $4, Imm`

![image](https://user-images.githubusercontent.com/79521972/162152990-2c3d35cb-4084-4704-bd4e-42feb8ba5841.png)

먼저 2개의 register(rs, rt)를 register file로부터 읽는다.

ALU에서 2개의 값을 비교할텐데 두 개의 차가 0이면 zero signal을 1로 설정된다.

jump할 immediate 값을 sign extention하고 word offset에서 byte offset으로 바꾼다.

위에서 구한 BTA 식으로 target address를 구한다.

- 그래서 위의 그림과 같이 PC+4으로 업데이트 된 값을 사용하는 것이 아니라 ALU와의 연산을 통한 address를 사용하게 된다. 
- zero signal이 1인 경우: PC <- target address(BTA)
- zero signal이 0인 경우: PC <- PC + 4

<br>

![image](https://user-images.githubusercontent.com/79521972/162253494-a475d3d3-497a-46ae-93ed-9761ab6082a5.png)



<br>



## Single-Cycle Processor (Data path)

![image](https://user-images.githubusercontent.com/79521972/162127622-e2f1200a-fb43-4628-877f-f7e22cb57e43.png)

전체적인 Single-Cycle Processor의 datapath는 위와 같다.

**Q) Register File에서 보면 lw인 경우에는 write을 하기 위해서 아래 RegDst에 결정되는 MUX 쪽으로 가고 add인 경우에는 read를 하기 위해서 register file의 A2쪽으로 가게되는데 이런 것은 Instruction Memory에서 Decoding 된 것으로 어느 노드로 갈지가 결정되는 건가요? 아니면 두 노드 모두로 이동하게 되나요?**

<br>



`add $4, $5, $6` 이라는 명령어를 실행할 때 각 control 에는 어떤 신호가 들어있을 것인가? -> 시험문제

![image](https://user-images.githubusercontent.com/79521972/162250108-94896b45-c7ea-42d1-890e-7f737a3ef161.png)

<br>

`lw $3, 8($4)`

![image](https://user-images.githubusercontent.com/79521972/162250187-0a4d8c03-ab30-4d1a-8168-795761bbb0c6.png)



<br>

`beq $3, $4, 5`

![image](https://user-images.githubusercontent.com/79521972/162250214-b8349de1-ba7a-427c-9425-624564192434.png)

resgister에 write하는 과정은 하나도 없기 때문에 RegDst와 MemtoReg는 don't care이다.

<br>

**Jump**

![image](https://user-images.githubusercontent.com/79521972/162253610-c967b834-ddc1-4da5-816a-4f8f40952750.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/162155339-23cfcec3-c01d-47a8-b5a5-caad09895935.png)





<br>

### Control signal 정리

- RegDst
  - write할 register의 번호를 결정한다.
  - R-format 일때만 high
- ALUSrc
  - ALU 연산에 들어가는 데이터를 결정한다.
  - lw/sw일 때만 high : ALU = register + immediate
- MemtoReg
  - register에 write 할 것을 결정한다.
  - lw일 때만 high : register ← memory에서 read 한 값
- RegWrite
  - register에 값을 write 할지 결정한다.
  - R-format, lw 때만 high
- MemRead
  - data memory를 read 할 지 결정한다.
  - lw 일 때만 high : memory read
- MemWrite 
  - data memory에 write할지 결정한다.
  - sw일 때만 high : memory write
- PCSrc = Branch
  - branch instruction인지 아닌지 결정한다.
  - beq일 때만 high : PC←PC + target offset
- ALUOp1, ALUOp2
  - ALU control에 input으로 들어가서 ALU의 연산 종류를 결정한다.



---

Q) single cycle인데 clk이 3개

A) 다음 시간에



Q) data memory에는 clock이 없는가?

A) 메모리에도 종류가 여러가지이다. 이 챕터에서 다룬 메모리는 Aysnc data 방식이기 때문



**Q) Register File에서 보면 lw인 경우에는 write을 하기 위해서 아래 RegDst에 결정되는 MUX 쪽으로 가고 add인 경우에는 read를 하기 위해서 register file의 A2쪽으로 가게되는데 이런 것은 Instruction Memory에서 Decoding 된 것으로 어느 노드로 갈지가 결정되는 건가요? 아니면 두 노드 모두로 이동하게 되나요?**



Q) instruction register는 어디에...?

---



