---
layout: single
title: "[Computer Architecture] Parallel Processors"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Parallel Processors']
---



# Parallel Processors from Client to Cloud

## Introduction

여러개 processor에다가 나눠서 각각에서 실행시키도록 한다.

- Goal: connecting multiple computers to get higher **performance** 
  - Multiprocessors 
  - Scalability, availability, power efficiency 
- Task-level (process-level) parallelism 
  - High throughput for independent jobs 
- **Parallel processing program** 
  - **Single program run on multiple processors** 
- Multicore microprocessors 
  - Chips with multiple processors (cores)

<br>

## Hardware and Software

- Hardware 
  - Serial: e.g., Pentium 4 
  - Parallel: e.g., quad-core Xeon e5345 
- Software 
  - Sequential: e.g., matrix multiplication 
  - Concurrent: e.g., operating system 
- Sequential/concurrent software can run on serial/parallel  hardware 
  - Challenge: making effective use of parallel hardware



## Parallel Programming

여러개 multiprocessor를 사용해서 parallel programming

각자 할당된 부분만 실행함.

- Parallel software is the problem 
- Need to get significant performance improvement 
  - Otherwise, just use a faster uniprocessor, since it’s  easier! 
- Difficulties 
  - Partitioning : 나누는 것
  - Coordination : 나눠준 결과를 다 받는 것
  - Communications overhead : 통신

<br>

## Amdahl's Law

![image](https://user-images.githubusercontent.com/79521972/172290826-4b6bf839-47d1-4963-9a90-1bdf5747d74a.png)



<br>

## Example: 



<br>

## Scaling

-  To get good speedup on a multiprocessor while keeping  the problem size fixed is harder than getting good  speedup by increasing the size of the problem. 
- Strong scaling – when speedup can be achieved on a  multiprocessor without increasing the size of the  problem 
- Weak scaling – when speedup is achieved on a  multiprocessor by increasing the size of the problem  proportionally to the increase in the number of  processors 
- Load balancing is another important factor. Just a single  processor with twice the load of the others cuts the  speedup almost in half



<br>

## Scaling Example

- Workload: sum of 10 scalars, and 10 × 10 matrix  sum 
  - Speed up from 10 to 100 processors 
- **Single** processor:  
  - Time = (10 + 100) × tadd 
- **10** processors 
  - Time = 10 × tadd + 100/10 × tadd = 20 × tadd 
  - Speedup = 110/20 = **5.5** (55% of potential) 
- **100** processors 
  - Time = 10 × tadd + 100/100 × tadd = 11 × tadd 
  - Speedup = 110/11 = **10** (10% of potential) 
- Assumes load can be balanced across  processors

- 걸리는 시간이 110 -> 20 -> 10으로 줄어들었다. 
  - 하지만 speedup이 10보다 커질 수는 없다.(bottleneck)

<br>

## Scaling Example (cont)

- What if matrix size is 100 × 100? 
- Single processor:  
- Time = (10 + 10000) × tadd 
- 10 processors 
- Time = 10 × tadd + 10000/10 × tadd = 1010 × tadd 
- Speedup = 10010/1010 = 9.9 (99% of potential) 
- 100 processors 
- Time = 10 × tadd + 10000/100 × tadd = 110 × tadd 
- Speedup = 10010/110 = 91 (91% of potential) 
- Assuming load balanced

- 데이터의 사이즈를 100x100으로 높이니까 91배까지 늘어날 수 있었다.
  - problem 사이즈를 가변적으로 변화시키는 -> weak scaling

<br>

## Strong vs Weak Scaling

- Strong scaling: problem size fixed 
  - As in example 
  - Goal is to run the same problem size faster.

- Weak scaling: problem size proportional to number of  processors 
  - Goal is to run larger problem in same amount of time. 
  - 10 processors, 10 × 10 matrix 
    - Time = (10 + 100/10) × tadd = 20 × tadd 
  - 100 processors, 32 × 32 matrix 
    - Time = (10 + 1000/100) × tadd = 20 × tadd 
  - Constant performance in this example 
- Different approaches depending on the applications

<br>

## Instruction and Data Streams

- An alternate classification

![image](https://user-images.githubusercontent.com/79521972/172291613-d89abad6-c05a-4837-a3c2-a256c74fd0ce.png)

- MIMD 중에 special한 MIMD
  - SPMD: Single Program Multiple Data 
    - A parallel program on a MIMD computer 
    - Conditional code for different processors
    - 다 똑같은 프로그램이 여러 processor에서 돌고 있다.
      - **프로그램은 같지만 실행되는 부분이 다르다**. -> parallel programming

<br>

## SIMD

- Operate elementwise on vectors of data 
  - E.g., MMX and SSE instructions in x86 
    - Multiple data elements in 128-bit wide registers 
- All processors execute the same  instruction at the same time 
  - Each with different data address, etc. 
- Simplifies synchronization 
- Reduced instruction control hardware 
- Works best for highly data-parallel  applications



<br>

## Vector Processors

SIMD의 특별한 타입

MIPS도 이 버전이 있음



- Highly pipelined function units 
- Stream data from/to vector registers to units 
  - Data collected from memory into registers 
  - Results stored from registers to memory 
- Example: Vector extension to MIPS 
  - 32 × 64-element registers (64-bit elements) 
  - Vector instructions 
    - lv, sv: load/store vector (전부 데이터를 벡터에 load, store)
    - addv.d: add vectors of double (vector가 한 번만 계산이 일어나는 것이 아니라 pipeline식으로 쭉 전체가 계산된다.)
    - addvs.d: add scalar to each element of vector of double 
- Significantly reduces instruction-fetch bandwidth



<br>

## Example

 ![image](https://user-images.githubusercontent.com/79521972/172292573-d956404d-5508-4a18-9c1b-6bd9ed5d5f6f.png)



- 2 + 512/8 * 9 = 600번에 가깝게 fetch



![image](https://user-images.githubusercontent.com/79521972/172292666-b981e25a-10d8-4f49-9207-c1202f047a5e.png)

- 6번만 하면 됨



<br>

## Multithreading and Multiprocessing

- Multithreading and Multiprocessing 
  - 하나의 프로세스에서 여러개의 
  - 여러 개의 프로세스
- Multithreading: the ability of a CPU (or a single core  in a multicore processor) to provide multiple **threads  of execution** concurrently, supported by OS 
- Multiprocessing: include multiple complete  processing units in one or more cores 
- As the two techniques are complementary, they are  combined in nearly all modern systems  architectures with multiple multithreading CPUs and  with CPUs with multiple multithreading cores.



<br>

## Hardware Multithreading (on a Chip)

- core 안에 여러 개의 thread를 동시에 실행시킬 수 있음
  - hareware support가 되어야 함(register file 복사품이 여러 개 있어야 함)
- Performing multiple threads of execution in parallel (to  increase the utilization of a single core by using threadlevel and instruction-level parallelism) 
  - Hardware must support Replicate register files, PC,  etc. 
  - And, Fast switching between threads 
- The threads share the resources of a single or multiple  cores, which include the computing units, the CPU  caches, and the TLB.

![image](https://user-images.githubusercontent.com/79521972/172293055-8c8faa26-7d80-473f-95e9-17998311ed43.png)

<br>



## Threading on a 4-way Super Scalar Processor

연산 unit이 여러개 들어있는 processor(동시에 fetch)

![image](https://user-images.githubusercontent.com/79521972/172293326-204aeef4-3c44-4b01-b6f8-54b863a0fdbf.png)



Coarse MT: 할 수 있는 만큼 다 실행하고 다른 thread 대기

Fine MT: clock level에서 실행

SMT: hazard 등의 이유로 비어 있는 공간에 다른 thread를 실행



<br>

## ''



## Multicore or Multiprocessor

Q1



<br>

## Shared memory

여러 core들이 같은 address의 memory를 공유





<br>

## Example: Sum Reduction

- Sum 100,000 numbers on 100 processor UMA 
  - Each processor has ID: 0 ≤ Pn ≤ 99 
  - Partition 1000 numbers per processor 
  - Initial summation on each processor
- 자기에게 할당된 계산을 각자 계산하고 합침

![image](https://user-images.githubusercontent.com/79521972/172293767-dfe3e39c-96bd-4a07-a506-c2c5d33555b2.png)

- Now need to add these partial sums 
- Reduction: divide and conquer 
- Half the processors add pairs, then quarter, … 
- Need to synchronize between reduction steps



<br>

## Process synchronization



<br>

## Message Passing Multiprocessors(MPP)

shared memory에서는 

network를 통해서 나눠져 있음

- Each processor has its own private address space 
- Q1 – Processors share data by explicitly sending and receiving  information (**message passing**) 
- Q2 – Coordination is built into message passing primitives (**message send and message receive**)

- 주고 받는 protocol 때문에 느려질 수 있음



<br>

## Example with 10 processors

![image](https://user-images.githubusercontent.com/79521972/172294289-be06f0e4-9dac-4c38-8d14-3571976452e8.png)





최종적으로 master processor(0번 processor)가 취함

<br>

## MPT and OpenMP in multi-core







<br>

## CPU info (example)



- socket -> cpu가 2개
- 실제로 cpu의 갯수 -> 56개 -> logical core의 개수를 말함









