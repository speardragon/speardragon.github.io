---
layout: single
title: "[Computer Architecture] Memory"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Memory']
---

# Chapter 8

## Topics

- Introduction
- Memory System Performance Analysis
- Caches
- Virtual Memory

I/O는 다른 강의자료에서 다루었음(중요하기 때문에)

- Memory-Mapped I/O
- Summary



<br>

## Introduction

- Computer performance depends on: 
  - Processor performance 
  - Memory system performance
    - RAM

앞서 clock time을 줄이기 위해 노력을 했는데 memory가 안 좋으면 말짱 도루묵이다.

![image](https://user-images.githubusercontent.com/79521972/167535136-3a56c221-5785-44ad-ab1b-45e6ebdd8476.png)

- CPU와 Memory 사이에는 speed gap차이가 상당히 크다.
  - CPU: 0.25ns / Mem: 20ns



<br>

## Processor - Memory Gap

In prior chapters, assumed access memory in 1 clock  cycle – but hasn’t been true since the 1980’s

![image](https://user-images.githubusercontent.com/79521972/167535346-d1dcca59-3f75-4cf0-9cf2-fcd64c5aaaba.png)

- 점점 processor와 memory의 speed gap이 커지고 있다.

<br>

## Memory System Challenge

- Make memory system appear as fast as  processor 
- Use hierarchy of memories 
- Ideal memory: 
  - Fast 
  - Cheap (inexpensive) 
  - Large (capacity)

<span style="color:red">But can only choose two!</span>



<br>

## Memory Hierarchy

![image](https://user-images.githubusercontent.com/79521972/167536086-6ba5ec18-962d-4745-95f1-282be7671b5f.png)

- 실질적인 data는 main memory에 있다고 생각할 수 있지만 사실은 hard disk에 있고 이를 그 중에서 몇 개만main memory에 탑재 시켜 놓은 것이다.
- CPU 안에도 Register File이라는 별도의 memory가 존재한다
  - 그래서 사실상 memory hierarchy 맨 꼭대기에는 reg file인 것이다.
- cache에서 register file로 옮겨주는 것은 우리가 앞에서부터 계속 해왔던 것임 -> assembly 
  - programmer가 작성한 코드에 의해 compiler가 이것을 해줌

- hard disk에서 main memory로 가져오는 것(VM)은 OS가 해준다.
- main memory에서 cache로 가져오는 것은 hardware 적으로 되는 것이다.
- ALU에 들어갈 수 있는 data의 수는 register file이 끽해봐야 32개이기 때문에 32개 미만의 변수만 담을 수 있다.
  - 그렇다면 hard disk에서 reg file까지 메모리가 어떻게 전달될까?
  - 필요한 것만 가져와서 용량을 맞춘다. (by locality)


<br>



![image](https://user-images.githubusercontent.com/79521972/167536852-8def1708-527a-4d40-832e-78d0e31bd4fa.png)

<br>

## Locality

Exploit locality to make memory accesses fast

한 번 access한 것에 대해 그 근처에 있는 것들을 주로 자주 access 하더라는 경험

- **Temporal Locality**: 
  - Locality in time 
  - If data used recently, likely to use it again soon 
    - 최근에 사용했던 거는 머지않아 또 사용될 가능성이 높다.
  - **How to exploit**: keep recently accessed data in higher levels of memory hierarchy 
- **Spatial Locality**: 
  - Locality in space 
  - If data used recently, likely to use nearby data soon 
    - data가 최근에 사용됐으면 그 근처에 있는 데이터가 사용될 가능성이 높다.
  - **How to exploit**: when access data, bring nearby data  into higher levels of memory hierarchy too

go to를 최대한 사용을 지양하는 이유가 이것이다.(근처에 있는 data를 건너 뛰기 때문에)

<br>

## Memory Performance

- **Hit**: <mark>data found in that level of memory hierarchy </mark>

- **Miss**: <mark>data not found (must go to next level) </mark>

  - 만약 data가 miss이면 clock penalty를 감수하고서라도 다음 level에서 가져와야 함.(매우 손해)


  Hit Rate(HR) = # hits / # memory accesses 
                 = 1 – Miss Rate 

  Miss Rate(MR) = # misses / # memory accesses 
                     = 1 – Hit Rate 

- **Average memory access time (AMAT)**: average time  for processor to access data 
  AMAT = t<sub>cache </sub>+ MR<sub>cache</sub>[t<sub>MM </sub>+ MR<sub>MM</sub>(t<sub>VM</sub>)]



Cache에 data가 없으면 (miss) main memory에서 가져오는데 10 클락 정도가 소모 되기 때문에 이는 가져올 수 있지만 

main memory에 없으면(miss) hard disk에서 가져오는 데는 백만 클락이 소모 되기 때문에 이를 가져오기는 힘들 것이다. 그래서 OS가 그것은 잠깐 fold하고 그와 동시에 다른 task를 수행하도록 하는 technology가 있음.

<br>

## Memory Performance Example 1

- A program has 2,000 loads and stores 

- 1,250 of these data values **in cache** 

- Rest supplied by other levels of memory  hierarchy 

- **What are the hit and miss rates for the cache?** 

  Hit Rate = 1250/2000 = **0.625** 

  Miss Rate = 750/2000 = **0.375** = 1 – Hit Rate



<br>

## Memory Performance Example 2

- Suppose processor has 2 levels of hierarchy:  cache and main memory 

- t<sub>cache </sub>= 1 cycle, t<sub>MM</sub> = 100 cycles 

- **What is the AMAT of the program from  Example 1?** 

  **AMAT** = t<sub>cache</sub> + MR<sub>cache</sub>(t<sub>MM</sub>) = [1 + 0.375(100)] cycles = **38.5 cycles**



<br>

## Amdahl's Law

![image](https://user-images.githubusercontent.com/79521972/167988264-3b5eeba6-1a33-43e5-910d-1b998a0d7530.png)

- parallelize 가능한 부분 (1-s)
- parallelize 불가능한 부분 (s)

<br>

n: number of processors **available for parallel processing** (parallelizable)

s: fraction of the serial natured code (cannot be parallelized)

![image](https://user-images.githubusercontent.com/79521972/167538922-779b4ac3-0ba7-4e27-9eae-72cff12413df.png)![image](https://user-images.githubusercontent.com/79521972/167538934-f0ea2d14-40c7-4176-b136-af67b384f3b8.png)

- speedup에 대한 수식을 그래프로 나타내 봤더니 parallel이 불가능한 부분 즉, S term이 커질 수록 speedup이 굉장히 줄어들기 때문에 S term이 중요하다고 볼 수 있다.(bottle neck)
  - 즉, S가 무엇이냐에 따라(I/O냐 main memory냐 ...) 달라지는 것이다.




<br>

## Memory

- ROM(read만)

- RWM:(read and write; RAM)

  - Sequential access (stack이나 queue의 경우 특정 데이터를 가져오려면 pop이나 dequeue가 되어야 함.)

  - Random access(RAM): 아무 때나 원할 때 access(by index)

<br>

- **Read-Only Memory** (ROM): 
  - non-volatile storage(비휘발성, 데이터가 사라지지 않음) 
  - ROM, PROM, EPROM, EEPROM 
    - PROM은 field에서 내가 원하는 값을 쓸 수 있도록
    - EPROM은 PROM인데 만약 데이터를 틀리게 쓴 경우 버려야 하기 때문에 이때, 지우고 다시 쓸 수 있음(물론 지우는 시간이 많이 걸리긴 하지만)
    - EEPROM(Electrical)
- **Random Access Memory** (RAM): 
  - Volatile storage
  - Static RAM (SRAM) 
    - 파워가 꺼지면 데이터가 사라짐
    - 파워만 있으면 data는 계속 유지된다는 뜻
  - Dynamic RAM (DRAM) 
    - 파워가 연결되어 있더라도 data가 없어지기도 한다.
    - 그렇기 때문에 사라지기 전에 read/write하는 refresh 과정이 필요함
  
- **Non-volatile RWM** (Flash): 
  - NAND type: for data 
  - OR type: for code

<br>

## SRAM(Static RAM)

- **Hold data without external refresh** 
  - **Simplicity**: don’t require external refresh circuitry 
  - **Speed**: SRAM is faster than DRAM 
  - **Cost**: several times more expensive than DRAMs 
  - **Size**: take up much more space than DRAMs 
    - SRAM은 6개의 TR가 필요하기 때문에(DRAM은 1개)
  - **Power**: consumes more power than DRAMs 
  - **Usage**: level 1 and level 2 cache 
- **Mainly used for on-chip memory**

<br>

## SRAM example

- Samsung 1M(eg) x 4  High-speed  CMOS SRAM 
  - 1Meg -> 20-bit address
- Fast access time:  8, 10 ns
- Low power dissipation: 5 mA(standby),  65-80 mA(operating)

![image](https://user-images.githubusercontent.com/79521972/167540915-71a95a48-d4d4-4cda-855f-f08e243bca90.png)

[https://www.slideserve.com/amity-riley/memory](https://www.slideserve.com/amity-riley/memory)

<br>

## SRAM (Static RAM)

![image](https://user-images.githubusercontent.com/79521972/167540941-f59e73da-19c6-4295-9b2f-9e3909e819a1.png)

- 6개의 TR(트랜지스터)
- inverter loop로 인해 데이터가 없어지지 않는다.

![image](https://user-images.githubusercontent.com/79521972/167540953-ed400fa9-8c39-4130-9df4-a894e46b1d32.png)

![image](https://user-images.githubusercontent.com/79521972/167540964-17809813-f41c-4f9a-a76f-2b2e42a5989f.png)

- OE: Out enable(read enable)

- OE_L이 low일 때 address값을 읽는데 address가 주어진 그 순간부터 그에 해당하는 data가 읽어지는데 그 때까지 걸린 시간을 **read access time**이라고 한다.(A to D)
  - 즉, address에 해당하는 data가 읽어질 때까지의 시간 -> read access time



[blackreas.tistory.com](blackreas.tistory.com)

<br>



## DRAM (Dynamic RAM)

- 주기적으로 읽어 주어야 함.
- Simple 1-Transistor cell 
- No direct power source requires periodic Refresh
- Share address line due to large capacity 
- Two new signals: RAS and CAS 
  - Row Address Strobe 
  - Column Address Strobe
- DRAM은 집적도가 높기 때문에 addressing  bit에 34bit가 필요한데 이를 17bit의 Row address와 17bit의 Column address를 나누었다.
- 그래서 RAS와 CAS를 통해 memory addressing을 할 수 있는 것이다.

메모리는 정보를 저장(write)하거나 저장된 정보를 읽기(read) 위하여 바둑판과 같이 열(raw, 가로 줄)과 행(column, 세로 줄)으로 구성된 matrix(행렬) 구조의 주소(address)를 가지고 있다.

이를 두고 CAS와 RAS라 부르는데 프로세서가 메모리에 있는 정보를 읽거나 메모리에 정보를 기록할 때는 먼저 가로줄에 신호(RAS, Row Address Strobe)를 보내고 나서 세로줄에 신호(CAS, Column Address Strobe)를 보내어 주소를 확인한다. 어떤 주소에 자료가 들어 있는지 아니면 비어 있는 지는 CAS가 담당하며 CAS 신호가 없어지면 그 주소에 다시 새로운 정보를 저장한다.(따라서 RAS 신호가 앞선다.)

![image](https://user-images.githubusercontent.com/79521972/167989853-efe5973b-aaa7-421d-8800-b88155e8e2a8.png)

<br>

## DRAM example

- Samsung 1Mx16 FPM DRAM 
  - Power: 3.3-5 V, 450-500 mW 
  - Access time: 50-60 ns

![image](https://user-images.githubusercontent.com/79521972/167541127-ce355567-1fa4-4d94-ae06-a70e867461fa.png)

- 10 bit의 address
  - 두 개(RAS, CAS) 합쳐서 20bit addressing수행


<br>

## DRAM (Dynamic RAM)

![image](https://user-images.githubusercontent.com/79521972/167541215-233e612f-b504-4a99-b598-5808a0c531b5.png)

- DRAM cell and DRAM array

  - TR 1개 
  - 0이나 1로 signal을 키워줌

- (example) 4M*1 DRAM

  - 사라지기 전에 주기적으로 data를 read/write하는 (refresh)


  - capacitor에 charge가 되어있으면 1, 안 되어있으면 0
  - RAS, CAS  각각 12 bit -> addressing 24 bit




[http://www.cse.scu.edu/~tschwarz/coen180/LN/DRAM.html](http://www.cse.scu.edu/~tschwarz/coen180/LN/DRAM.html)

<br>

-  read/write/burst timing/SDRAM burst read timing

![image](https://user-images.githubusercontent.com/79521972/167541291-88cc457d-9023-4e3f-b40a-0ee23f34e9dc.png)

- 1번 그림: read access time
  - RAS_L이 먼저 Low가 되었기 때문에 RAS가 먼저 왔고 CAS가 그 다음 왔음을 알 수 있음
  - RAS가 오면서 Row address를 읽고
  - CAS가 온 순간부터 데이터가 다 읽어질 때까지 걸린 시간을 read access time이라고 함.

- 2번 그림: burst timing

  - 연속적인(인접한) 데이터를 계속 가져오기 위해서는(spatial locality) RAS는 계속 두고 CAS의 신호만 계속 바꾸게 하고


  - CAS에서만 계속 access
  - 그래서 DRAM에서도 연속적인 memory를 access 할 때 굉장히 빠르다 -> burst mode를 사용하기 때문


- 3번 그림: write access time
  - CAS가 되면서부터 데이터를 쓰기 까지 걸린 시간 -> write access time

- 4번 그림: Synchronous DRAM(SDRAM)





[http://www.dewassoc.com/](http://www.dewassoc.com/)

---

Q) 일반적으로 RAS가 먼저 도착하는가?

A) 맞음

---

<br>

## Flash Types

- NOR flash: bit cell like a NOR gate 
  - Random read/write access 
  - Used for instruction memory in embedded systems 
- NAND flash: bit cell like a NAND gate 
  - Denser (bits/area), but **block-at-a-time access** 
  - Cheaper per GB 
  - Used for USB keys, media storage, … 
- Flash bits wears out after ~100,000 accesses 
  - Not suitable for direct RAM or disk replacement 
  - Wear leveling: **remap data** to less used blocks
    - 고루고루 퍼져서 access 할 수 있도록 (FTL)

<br>

- Comparison on various performance metric 
  - NAND: fast writing 
  - NOR: byte access

![image](https://user-images.githubusercontent.com/79521972/167541689-c645bed1-e1a1-436b-adc6-57fd77c9a055.png)

