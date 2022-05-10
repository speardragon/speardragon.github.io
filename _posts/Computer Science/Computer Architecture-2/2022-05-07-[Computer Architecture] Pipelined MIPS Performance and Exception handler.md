---
layout: single
title: "[Computer Architecture] Pipelined MIPS Performance and Exception Handler"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Pipeline']
---

<br>

## Pipelined Performance Example(시험)

- SPECINT2000 benchmark: 
  - 25% loads 
  - 10% stores 
  - 11% branches 
  - 2% jumps 
    - jump 명령이 fetch되고 decode 되면서 jump라는 것을 알 수 있는데 그 때 PC에 BTA를 넣어주어야 하는데 이미 그 다음 줄 instruction이 들어와 있다. 그래서 그 명령을 flush 해 주는 clock이 하나 더 소비 되기 때문에 jump 명령의 CPI는 2이다.
  - 52% R-type 
- Suppose: 
  - 40% of loads used by next instruction -> lode use hazard
    - CPI = 2 (due to stalling)
  - 25% of branches mispredicted
    - CPI = 2 (due to flush)  
  - All jumps flush next instruction 
    - All jump CPI = 2
- What is the average CPI?
  - Load/Branch CPI = 1 when no stalling, 2 when stalling
  - CPI<sub>lw</sub> = 1(0.6) + 2(0.4) = 1.4 
  - CPI<sub>beq</sub> = 1(0.75) + 2(0.25) = 1.25
  - Average CPI = (0.25)(1.4) + (0.1)(1) + (0.11)(1.25) + (0.02)(2) + (0.52)(1) = **1.15**

jump는 CPI가 2이지만 비율상 2%이기 때문에 크게 영향을 미치지 않았다.

<br>

- Pipelined processor critical path: 

- Tc = max { 

  t<sub>pcq</sub> + t<sub>mem</sub> + t<sub>setup</sub> 

  2(t<sub>RFread </sub>+ t<sub>mux </sub>+ t<sub>eq </sub>+ t<sub>AND </sub>+ t<sub>mux </sub>+ t<sub>setup </sub>) 

  t<sub>pcq </sub>+ t<sub>mux </sub>+ t<sub>mux </sub>+ t<sub>ALU </sub>+ t<sub>setup</sub> 

  t<sub>pcq </sub>+ t<sub>memwrite </sub>+ t<sub>setup</sub> 

  2(t<sub>pcq </sub>+ t<sub>mux </sub>+ t<sub>RFwrite</sub>) }
  
  RFread or RFwrite은 falling edge trigger이기 때문에 2 배만큼의 clock이 더 걸리게 된다.

![image](https://user-images.githubusercontent.com/79521972/167231399-ea48621b-1799-4eb1-8a1b-cfacda039b74.png)

<br>

## Pipelined Performance Example

![image](https://user-images.githubusercontent.com/79521972/167231421-ca272749-21d3-47ec-92b5-3386d4a8b412.png)



Tc = 2(t<sub>RFread </sub>+ t<sub>mux </sub>+ t<sub>eq </sub>+ t<sub>AND </sub>+ t<sub>mux </sub>+ t<sub>setup </sub>) 

 =2[150 + 25 + 40 + 15 + 25 + 20] ps = **550 ps**

<br>
Program with 100 billion instructions

**Execution Time** = (#instructions) x CPI x Tc 

​							= (100 x 10<sup>9</sup>)(1.15)(550 x 10<sup>-12</sup>)

​							= **63 seconds**



<br>

## Processor Performance Comparson

![image](https://user-images.githubusercontent.com/79521972/167231529-bcfd75e7-142b-4220-a724-f0f406f4fef8.png)

1.47배 만큼 더 빨라진 것을 볼 수 있음

<br>

## Review: Exceptions

- <u>Unscheduled function call</u> to **exception handler** 
- Caused by: 
  - Hardware, also called an **interrupt**, e.g. keyboard 
  - Software, also called **traps**, e.g. undefined instruction 
  - Divide-by-zero, overflow, debugger breakpoint,  hardware malfunction, attempt to read nonexistent  memory, accessing privileged instruction or memory 
- When exception occurs, the processor: 
  - Records cause of exception (**Cause register**) 
  - **Jumps** to exception handler (0x80000180) 
  - Returns to program (**EPC register**를 보고 다시 리턴한다.)



<br>

## Example Exception

![image](https://user-images.githubusercontent.com/79521972/167232373-bf3ed245-8202-451a-8f44-d18793de49a3.png)





<br>

## Exception Register

- Not part of register file 
  - **Cause** 
    - Records cause of exception 
    - Coprocessor 0 register 13 
  - **EPC** (Exception PC) 
    - Records PC where exception occurred 
    - **Coprocessor 0** register 14 
      - Coprocessor: 별도의 process를 처리하는 processor
- Move from Coprocessor 0 
  - mfc0 $t0, Cause 
    - Cause의 내용을 읽어서 $t0에 write
  - Moves contents of Cause into $t0

![image](https://user-images.githubusercontent.com/79521972/167232457-642bbe82-f2f4-4c21-9556-f46b12d3e6cd.png)



<br>

## Exception Causes

![image](https://user-images.githubusercontent.com/79521972/167232481-2b991e5c-1b58-4659-93de-c889ca03e6f5.png)

<span style="color:red"> Extend multicycle MIPS processor to handle last two types of exceptions</span>



<br>

## Exception Hardward: EPC &amp; Cause

![image](https://user-images.githubusercontent.com/79521972/167232533-c587f576-5ca7-423f-8f74-995d9ef7d4ee.png)

Exception이 발생하면(EPCWrite active) **현재 실행되고 있는 PC** **값**이 EPC register에 저장이 되고

**어떤 exception인지**를 mux가 결정하여 그 값이 cause register에 저장이 된다.



<br>

## Exception Hardware: mfc0

mfc0 $3, Cause

![image](https://user-images.githubusercontent.com/79521972/167232552-63c549d0-73c0-4a47-b6d4-5f910d3aea34.png)

writereg에 cause 정보를 저장하는 path

- mux를 통해 cause 혹은 EPC 값을 전달한다.

<br>

## Control FSM with Exceptions

![image](https://user-images.githubusercontent.com/79521972/167232571-ae1fa4ae-d2ac-449e-b054-a2fe7f25bb47.png)





<br>

## Advanced Microarchitecture

- Deep Pipelining 
- Branch Prediction 
- Superscalar Processors 
- Out of Order Processors 
- Register Renaming 
- SIMD 
- Multithreading 
- Multiprocessors



<br>

## Deep Piplining

-  10-20 stages typical (너무 높으면 오히려 성능 저하가 생기기 때문에 적당한 stage로 구성)
- Number of stages limited by: 
  - Pipeline hazards 
  - Sequencing overhead 
  - Power - frequency에 비례하기 때문에 
  - Cost

![image](https://user-images.githubusercontent.com/79521972/167232671-7a2387d6-8709-4ad9-897f-13b00fc8147f.png)![image](https://user-images.githubusercontent.com/79521972/167232676-041e4c70-3f62-4cbc-9325-e70d5187967a.png)



<br>

## Branch Prediction

- **Guess** whether branch will be taken 
  - Backward branches are usually taken (loops) 
  - Consider history to improve guess 
- Good prediction **reduces fraction of branches  requiring a flush**

<br>

- Ideal pipelined processor: CPI = 1 
- Branch misprediction increases CPI  (CPI = 2)
  - flush 하는 과정 때문에 CPI가 2

- **Static branch prediction:** 
  - Check direction of branch (forward or backward) 
  - If backward, predict taken 
  - Else, predict not taken 
- **Dynamic branch prediction:** 
  - Keep **history** of last (several hundred) branches in branch target buffer, record: 
    - Branch destination 
    - Whether branch was taken

<br>

## Branch Prediction Example

![image](https://user-images.githubusercontent.com/79521972/167232731-263c42f3-e664-4934-bd5b-10e197b2b6fa.png)

- loop를 10번 돌리는 과정
- history 1 bit가 있는데 처음에는 T(aken) 으로 되어 있는데 실제로는 일어나지 않아야 하기 때문에 틀리게 되고 NT로 바뀌어서 loop를 10번 진행할 동안에는 모두 NT이기 때문에 계속 맞다가 마지막에는 beq명령어가 done으로 이동하기 때문에 T인데 NT로 되어있기 때문에 틀리고 다시 NT로 바뀌게 된다.

- 따라서 맨 처음과 맨 끝은 틀리게 된다.

<br>

## 1-Bit Branch Predictor

![image](https://user-images.githubusercontent.com/79521972/167233622-17bfc5df-87ae-4d0d-ab7c-8f5729d143a7.png)

- **Remembers** whether branch was taken the last time and **does the same thing** 
- **Mispredicts** **first** and **last** branch of loop

![image](https://user-images.githubusercontent.com/79521972/167232747-dc0010a7-e241-4f75-8e96-8e7d06ee37b9.png)

- 이 방법도 한계가 있는데, 가령 doubly nested loop를 들어보면 위와 같은 구조는 branch instruction에서 miss가 날 때마다 prediction state가 변경된다. 그런데 전체적으로 루프 한 개가 다른 loop 한 개를 포괄하고 있는 상태에서 전체 state를 예측하는 것이 확실하다고 보장하지 못할 것이다.
  다시 말해 내부 loop가 수행되면서 branch가 taken이 되더라고 마지막 반복구문에서는 그 branch에 대한 prediction이 일어나지 않으므로 그 상태가 not taken으로 변경된다. 그런데 이 상태에서 안쪽 loop를 다시 수행하면 처음 branch predict를 수행하면 다시 수행하기 위해 state가 taken으로 변경되면서 결론적으로 안쪽 루프의 처음 state와 나중 state가 맞지 않는 현상이 발생하는 것이다.

<br>

## 2-Bit Branch Predictor

![image](https://user-images.githubusercontent.com/79521972/167233618-dd69bad8-d1a6-4051-8334-6c36e9d1782b.png)

![image](https://user-images.githubusercontent.com/79521972/167232887-a148a4ce-1593-491f-b5b5-e0c6d932d340.png)

연속적으로 계속 taken이 일어나고 있는지를 확인한다.

> Only mispredicts last branch of loop

연속적으로 Not Taken이 일어났을 때야 Weakly Not Taken으로 예측한다.

<br>

- 2bit에서는 branch prediction이 두 번 연속으로 틀릴 경우에만 state가 변경되기 때문에 위의 케이스 경우에는 안쪽 루프의 마지막 instruction이 mispredict 되더라도 state가 taken을 유지하게 된다. 물론 그 상태에서 다시 안쪽 루프를 수행하더라도 전체적으로 prediction state가 맞게 되는 것이다.



<br>

### Superscalar

만약 CPU의 ALU를 통해서 덧셈과 곱셈의 연산결과를 뽑으라는 instruction을 줬다고 가정했을 때 앞에서 말한 IPC=1인 환경에서는 한 cylce이 지나면 곱셈의 결과를 얻을 수 있어야 한다. 그런데 만약 곱셈의 결과가 덧셈의 결과를 바탕으로 나오는 것이라면 한 cylce이 지나기 전에 곱셈의 결과를 얻기는 어려울 것이다. 왜냐면 덧셈의 instruction을 수행하는데도 1cylce이 소모되기 때문이다. 이렇게 컴퓨터 성능을 막는 요소 중 하나를 instruction 간의 dependency라고 한다.

이걸 극복하기 위해선 instruction이 처리되는 path를 여러개 만들고 각각의 instruction을 해당 path를 통해 처리하게 하면 된다.(위 예제에서 덧셈과 곱셈) 물론 완전히 똑같으면 성능 측면에서 의미가 없으니까 하나는 data만 처리하게 하고, 다른 하나는 instruction만 처리할 수 있도록 구성하면 instruction을 읽어왔을 때 data pipeline을 통해 결과를 뽑을 수 있게 한다.

이 구조를 바로 SuperScalar 구조라고 하고 앞에서 잠깐 언급한 path를 SuperScalar에서는 way로 표현한다.

- Multiple copies of datapath execute multiple  instructions at once
- Dependencies make it tricky to issue multiple  instructions at once CLK CL
- 내부구조는 아래 그림과 같이 생겼다.



![image](https://user-images.githubusercontent.com/79521972/167233034-0e35857a-ad52-44af-a866-5269771a1d5f.png)

기존의 pipeline과 다른 점은 superscalar는 두 개의 instruction을 한 번에 처리할 수 있다는 점이다.

<br>

## Superscalar Example

![image](https://user-images.githubusercontent.com/79521972/167233056-b86a5cd5-69ed-4048-a343-4fd62aec0f2b.png)



<br>

## Superscalar with Dependencies

![image](https://user-images.githubusercontent.com/79521972/167233074-32540c85-14a5-4ab5-84e5-4b1b5cf9d31c.png)

t0를 load 하는데 그 다음 명령에서 t0를 사용해야 한다.

6개 명령어가 들어가는데 5clock이 걸렸다. -> IPC = 6/5

datapath가 두 개로 늘긴 했지만 그만큼의 효과를 보지 못했다.(due to dependencies)

<br>

## Out of Order (OOO) Processor

들어가는 instruction 순서와 실행되는 순서가 뒤죽박죽 by compiler (hazard를 줄이기 위해)

- Looks ahead across multiple instructions 
- Issues as many instructions as possible at once 
- Issues instructions out of order (as long as no  dependencies) 
- **Dependencies:** 
  - **RAW** (read after write): one instruction writes, later  instruction reads a register 
  - **WAR** (write after read): one instruction reads, later  instruction writes a register 
  - **WAW** (write after write): one instruction writes, later  instruction writes a register

Q) RAR는 dependency에 속하지 않는 이유



<br>

## Out of Order Processor

- **Instruction level parallelism (ILP):** 
  - number  of instruction that can be issued  simultaneously (average < 3) 
- **Scoreboard**: table that **keeps track of**(기록): 
  - Instructions waiting to issue 
  - Available functional units 
  - Dependencies



<br>

## Out of order Processor Example

![image](https://user-images.githubusercontent.com/79521972/167233195-99ba81fd-ce7c-46a5-8427-91a34f0e962a.png)

dependencies를 따진 후에 or, sw의 위치를 위로 올려서 stall을 없앴다. -> IPC 향상



<br>

## Register Renaming

rename register - nonarchitectural register (우리가 사용할 수 없는 register; MIPS: $r0~$r19)

![image](https://user-images.githubusercontent.com/79521972/167233205-929c4bcf-b77c-4d6f-8405-eb394f8ed3a3.png)

t0를 r0로 rename 함으로써 add명령을 아래로 내릴 수 있었고 이를 통해  IPC의 향상을 이끌어냈다.

<br>

## SIMD

- **Single Instruction Multiple Data** (SIMD) 
  - Single instruction acts on multiple pieces of data at once 
  - Common application: graphics
  - Perform short arithmetic operations (also called packed  arithmetic) 
- For example, add four 8-bit elements

![image](https://user-images.githubusercontent.com/79521972/167233225-2da9ac57-e535-403e-a217-48dd967cd023.png)



<br>

## Advanced Architecture Techniques

- **Multithreading** 
  - Wordprocessor: thread for typing, spell checking,  printing 
- **Multiprocessors** 
  - Multiple processors (cores) on a single chip

<br>

- processor vs. process
  - processor는 hardware
  - process는 software(task)이다.



<br>

## Threading: Definitions

- **Process:** program runnig on a computer
  - Multiple processes can run at once: e.g., surfing  Web, playing music, writing a paper 
- **Thread:** part of a program 
  - Each process has multiple threads: 
  - e.g., a word  processor may have threads for typing, spell  checking, printing



<br>

## Threads in Conventional Processor

**Single-Core system** 

- One thread runs at once 
- When one thread stalls (for example, waiting  for memory): 
  - Architectural state of that thread stored 
  - Architectural state of waiting thread loaded into  processor and it runs 
  - Called **context switching** 
- Appears to user like all threads running  simultaneously
  - 동시에 실행이 되는 것처럼 보이지만 실제로는 이거했다 저거했다 context switching이 이루어지는 것



<br>

## Multithreaded processor

thread를 여러개로 동시에 할 수 있는 hardware

- Has multiple copies of **architectural state** (PC, reg)
- Multiple threads can be **active** at a time: 
  - When one thread stalls, another can run immediately  (without any delay) because PC and registers are  already available. 
  - If one thread can’t keep all execution units busy,  another thread can use idle units. 
- Does not increase instruction-level parallelism  (ILP) of single thread, but increases  throughput  

> Intel calls this “hyperthreading”



<br>

## Multiprocessors

- Multiple processors (cores) with a method of  communication between them 
- Types: 
  - Homogeneous: multiple cores with shared memory 
  - Heterogeneous: separate cores for different tasks (for  example, DSP and CPU in cell phone) 
  - Clusters: each core has own memory system



<br>

## 참고문헌 (reference)

- Patterson & Hennessy’s: Computer  Architecture: A Quantitative Approach 
- Conferences: 
- [www.cs.wisc.edu/~arch/www/](www.cs.wisc.edu/~arch/www/ )
- ISCA (International Symposium on Computer  Architecture) 
- HPCA (International Symposium on High Performance  Computer Architecture)




