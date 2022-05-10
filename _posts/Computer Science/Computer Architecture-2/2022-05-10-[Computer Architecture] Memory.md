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
- Memory-Mapped I/O
- Summary



<br>

## Introduction

- Computer performance depends on: 
  - Processor performance 
  - Memory system performance

앞서 clock time을 줄이기 위해 노력을 했는데 memory가 안 좋으면 말짱 도루묵이다.

![image](https://user-images.githubusercontent.com/79521972/167535136-3a56c221-5785-44ad-ab1b-45e6ebdd8476.png)

Processor는 0.25ns clock time, Memory는 CPU에 비해 몇 십배 성능이 뒤쳐진다.



<br>

## Processor - Memory Gap

In prior chapters, assumed access memory in 1 clock  cycle – but hasn’t been true since the 1980’s

![image](https://user-images.githubusercontent.com/79521972/167535346-d1dcca59-3f75-4cf0-9cf2-fcd64c5aaaba.png)



<br>

## Memory System Challenge

- Make memory system appear as fast as  processor 
- Use hierarchy of memories 
- Ideal memory: 
  - Fast 
  - Cheap (inexpensive) 
  - Large (capacity)

But can only choose two!



<br>

## Memory Hierarchy

![image](https://user-images.githubusercontent.com/79521972/167536086-6ba5ec18-962d-4745-95f1-282be7671b5f.png)

CPU 안에도 Register File이라는 memory가 존재한다







.

![image](https://user-images.githubusercontent.com/79521972/167536852-8def1708-527a-4d40-832e-78d0e31bd4fa.png)

<br>

## Locality

Exploit locality to make memory accesses fast

한 번 access한 것에 대해 그 근처에 있는 것들을 주로 자주 access 하더라는 경험

- Temporal Locality: 
  - Locality in time 
  - If data used recently, likely to use it again soon 
  - **How to exploit**: keep recently accessed data in higher  levels of memory hierarchy 
- Spatial Locality: 
  - Locality in space 
  - If data used recently, likely to use nearby data soon 
  - **How to exploit**: when access data, bring nearby data  into higher levels of memory hierarchy too



<br>

## Memory Performance

- **Hit**: data found in that level of memory hierarchy 

- **Miss**: data not found (must go to next level) 

  Hit Rate = # hits / # memory accesses 
                 = 1 – Miss Rate 

  Miss Rate = # misses / # memory accesses 
                     = 1 – Hit Rate 

- **Average memory access time (AMAT)**: average time  for processor to access data 
  AMAT = t cache + MRcache[tMM + MRMM(tVM)]


<br>

## Memory Performance Example 1

- A program has 2,000 loads and stores 

- 1,250 of these data values in cache 

- Rest supplied by other levels of memory  hierarchy 

- What are the hit and miss rates for the cache? 

  Hit Rate = 1250/2000 = 0.625 

  Miss Rate = 750/2000 = 0.375 = 1 – Hit Rate



<br>

## Memory Performance Example 2

- Suppose processor has 2 levels of hierarchy:  cache and main memory 

- t<sub>cache </sub>= 1 cycle, t<sub>MM</sub> = 100 cycles 

- What is the AMAT of the program from  Example 1? 

  AMAT = t<sub>cache</sub> + MRcache(tMM) = [1 + 0.375(100)] cycles = 38.5 cycles



<br>

## Amdahl's Law

n: number of processors available for parallel processing 

s: fraction of the serial natured code (cannot be parallelized)

![image](https://user-images.githubusercontent.com/79521972/167538922-779b4ac3-0ba7-4e27-9eae-72cff12413df.png)![image](https://user-images.githubusercontent.com/79521972/167538934-f0ea2d14-40c7-4176-b136-af67b384f3b8.png)

S term의 중요성



<br>

## Memory

- Read-Only Memory (ROM): 
  - **non-volatile storage**(비휘발성, 사라지지 않음) 
  - ROM, PROM, EPROM, EEPROM 
    - PROM은 그냥 field에서 내가 원하는 값을 쓸 수 있도록
    - EPROM은 지우고 다시 쓸 수 있음(지우는 시간이 많이 걸리긴 하지만)
- Random Access Memory (RAM): 
- Volatile storage
  - Static RAM (SRAM) 
  - Dynamic RAM (DRAM) 
    - dynamic means 파워가 연결되어 있어도 data가 없어진다.
- Non-volatile RWM (Flash): 
  - NAND type: for data 
  - OR type: for code

<br>

- ROM

- RWM -> 

  - Seq access

  - Random acess(RAM)



<br>

## SRAM(Static RAM)

- Hold data without external refresh 
  - Simplicity: don’t require external refresh circuitry 
  - Speed: SRAM is faster than DRAM 
  - Cost: several times more expensive than DRAMs 
  - Size: take up much more space than DRAMs 
  - Power: consumes more power than DRAMs 
  - Usage: level 1 and level 2 cache 
- Mainly used for on-chip memory

<br>

## SRAM example

- Samsung 1Mx4  High-speed  CMOS SRAM 
- Fast access time:  8, 10 ns
- Low power  dissipation: 5  mA(standby),  65-80  mA(operating)

![image](https://user-images.githubusercontent.com/79521972/167540915-71a95a48-d4d4-4cda-855f-f08e243bca90.png)

[https://www.slideserve.com/amity-riley/memory](https://www.slideserve.com/amity-riley/memory)

<br>

## SRAM (Static RAM)

![image](https://user-images.githubusercontent.com/79521972/167540941-f59e73da-19c6-4295-9b2f-9e3909e819a1.png)

![image](https://user-images.githubusercontent.com/79521972/167540953-ed400fa9-8c39-4130-9df4-a894e46b1d32.png)

![image](https://user-images.githubusercontent.com/79521972/167540964-17809813-f41c-4f9a-a76f-2b2e42a5989f.png)

[blackreas.tistory.com](blackreas.tistory.com)

<br>



## DRAM

- 주기적으로 읽어 주어야 함.
- Simple 1-Transistor cell 
- No direct power source requires periodic  Refresh 
- Share address line due to large capacity 
- Two new signals: RAS and CAS 
  - Row Address Strobe 
  - Column Address Strobe


<br>

## DRAM example

- Samsung 1Mx16 FPM DRAM 
  - Power: 3.3-5 V, 450-500 mW 
  - Access time: 50-60 ns

![image](https://user-images.githubusercontent.com/79521972/167541127-ce355567-1fa4-4d94-ae06-a70e867461fa.png)



<br>

## DRAM (Dynamic RAM)

![image](https://user-images.githubusercontent.com/79521972/167541215-233e612f-b504-4a99-b598-5808a0c531b5.png)



[http://www.cse.scu.edu/~tschwarz/coen180/LN/DRAM.html](http://www.cse.scu.edu/~tschwarz/coen180/LN/DRAM.html)



<br>

-  read/write/burst timing/SDRAM burst read timing

![image](https://user-images.githubusercontent.com/79521972/167541291-88cc457d-9023-4e3f-b40a-0ee23f34e9dc.png)

[http://www.dewassoc.com/](http://www.dewassoc.com/)

DRAM도 memory access를 할 때 연속된 메모리를 access 할 때 훨씬 더 빠르다(burst timing을 사용할 수 있어서)

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
  - Wear leveling: remap data to less used blocks

<br>

- Comparison on various performance metric 
  - NAND: fast writing 
  - NOR: byte access

![image](https://user-images.githubusercontent.com/79521972/167541689-c645bed1-e1a1-436b-adc6-57fd77c9a055.png)

