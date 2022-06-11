---
layout: single
title: "[Computer Architecture] I/O - 2"
categories: ['Computer Science', 'Computer Architecture']
tag: ['I/O']
---





## Interrupts

CPU가 있으면 CPU가 직접 check 즉, polling하지 않고 IO 자체가 준비가 되면 IO가 interrupt를 걸어서 준비가 됐음을 알린다. 

priority를 줄 수도 있음(우선순위를 먼저 처리)

- When a device is ready or error occurs 
  - Controller interrupts CPU 
- Interrupt is like an **exception** 
  - But not synchronized to instruction execution 
  - Can invoke handler between instructions 
  - Cause information often identifies the interrupting device 
- **Priority interrupts** 
  - Devices needing more urgent attention get higher priority 
  - Can interrupt handler for a lower priority interrupt 
- <mark>Special hardware needed </mark>
  - **Interrupt controller**
  - 현재 모인 interrupt를 처리해서 CPU에 달린 하나의 interrupt pin으로  보내준다.

<br>

## Interrupt controller

- Example: ARM microcontroller (Cortex M0)

![image](https://user-images.githubusercontent.com/79521972/172031965-e7813976-5134-4b85-acb5-9d28537b3cfb.png)

interrupt source들 모아서 CPU에게 알려주고 우선순위, 대기열 등에 의거하여 interrupt를 control 해주는 interrupt controller이 있음

<br>

## I/O Data Transfer

- Polling and interrupt-driven I/O 
  - CPU transfers data between memory and I/O data registers 
  - Time consuming for high-speed devices 

- CPU가 harddisk에서 데이터는 lw하고 다시 메모리에 저장하는 행동은 너무 시간 소모가 심함.

- <span style="color:red">Direct memory access (DMA) </span>
  - CPU가 관여하지 않고
  - OS provides starting address in memory 
  - I/O controller transfers to/from memory autonomously 
  - Controller interrupts on completion or error


<br>

- CPU는 다음과 기능이 있다. (셋 중 어느 기능이라도 없으면 CPU가 아님)

  - Arithmetic and Logical processing

  - Data move(lw/sw)

  - flow control

- DMA는 위 기능 중에서 다른건 다 못하고 Data move만 가능하도록 만든 것이다.

- IO에서 메모리, 메모리에서 IO

 

CPU 는 DMA controller 에게 다음과 같은 정보를 보냅니다. 

-Read/Write

-Device address

-Starting address of memory block for data

-Amount of data to be transferred 

<br>

## Direct Memory Access (DMA)

- For high-bandwidth devices (like disks) interrupt-driven I/O would consume a lot of processor cycles 
  - disk와 같은 높은 bandwidth를 가진 기기(interrupt-driven IO)는 수많은 processor cycle을 소비할 것이다. 

- With DMA, the DMA controller has the ability to transfer large blocks of data **directly** to/from the memory **without involving the processor**
  - DMA controller는 CPU 없이 memory와 직접적으로 거대 data block을 옮기는 능력이 있다


1. The processor **initiates the DMA transfer** by supplying the I/O device address, the operation to be performed, the memory address destination/source, the number of bytes to transfer
   - CPU는 IO device adress를 공급함으로써 DMA 전송을 시작한다.
2. The **DMA controller manages the entire transfer** (possibly  thousand of bytes in length), arbitrating for the bus 
   - DMA controller는 전체의 전송을 관리한다.(가능한 한 1000 bytes 크기) ,bus를 커버하면서
3. When the DMA transfer is complete, the DMA controller **interrupts the processor** to let it know that the transfer is complete 
   - DMA 전송이 끝나면 DMA controller가 CPU에게 interrupt를 걸어서 다 끝났음을 알린다.

- **There may be multiple DMA devices in one system** 
  - Processor and DMA controllers contend(다투다, 경쟁하다) for bus cycles and for memory

처음에는 slave처럼 작동하다가 CPU가 bus를 끊으면 마치 master처럼 작동

<br>

## DMA Data transfer (Example)

- DMA 없이

![image](https://user-images.githubusercontent.com/79521972/171561167-8292ebb2-1252-495a-8767-5a5d8793888e.png)

1. CPU transfer (read/write) data between memory and I/O
2. I/O operation is **slow** -> CPU overhead

![image](https://user-images.githubusercontent.com/79521972/171561248-ac76fd20-5533-493a-88bc-5ad9ee1901a9.png)

1. CPU writes command(r/w), device address, memory address, and count
2. Bus Request and Bus Grant
3. DMA controls data transfer (CPU can still use cache and do other calculation)
4. Once it's done, send interrupt to CPU

블락 단위로 보낼 수 있는 버퍼가 존재



- DMA가 Bus를 통해서 CPU에게 Request를 보냄

- CPU가 허락함(Bus Grant)

- 그러면 CPU와 memory의 연결을 끊어 버림

- 그러면 DMA가 master가 돼서 memory를 직접 주거니 받거니 한다.

- disk가 CPU를 거치지 않고 directly memory에 access 할 수 있게 됐다.
- 전송을 마치면 interrupt를 보냄 -> 다시 CPU가 bus를 잡음



<br>

## DMA data Transfer

- If DMA writes to a memory block that is cached 
  - Cached copy becomes **stale** (탁해진다, 신선하지 않다)
- If write-back cache has dirty block, and DMA reads memory block 
  - Reads stale data 
- Need to ensure **cache coherence** 
  - memory에 있는 데이터가 cache에도 있을 경우에는 memory에서 가져오거나 memory에 write해 봤자 의미가 없다.
    - cache에는 update 됐는데 memory에는 아직 update되지 않았다면 이 memory 내용을 가져오는 건 좋지 않음 -> cache coherence
    - write 할 때도 memory에만 update하면 안되고 cache에도 update 해주어야 함
  - OS forces write-backs from cache if they will be used  for DMA (called a **cache flush**) 
  - Or use **non-cacheable memory** locations for I/O
    - cache를 통하지 않고 바로오도록 하는 공간을 따로 마련

<br>

## DMA/VM Interaction

- OS uses virtual addresses for memory 
  - DMA blocks may not be contiguous in physical memory 
- Should DMA use **virtual addresses**? 
  - DMA controller will have to translate virtual address to physical  address (i.e. need TLB structure) 
- If DMA uses **physical addresses** 
  - Must constrain all the DMA transfers to stay within a page. (if not,  then it won’t necessarily be contiguous in memory) 
  - May need to break transfers into page-sized chunks 
  - Or chain multiple transfers 
  - Or allocate contiguous physical pages for DMA

-  Virtual 을 사용할 수도 physical을 사용할 수도 있음.

- Whichever is used, the OS must cooperate by not  remapping pages while a DMA transfer involving that  page is in progress

<br>

## I/O system Performance

- I/O **performance** depends on 
  - Hardware: CPU, memory, controllers, buses 
  - Software: operating system, database management system,  application 
  - Workload: request rates and patterns 
- I/O system design can trade-off between **response time and throughput** 
  - Measurements of throughput often done with constrained  response-time



<br>

## I/O vs. CPU Performance

- Amdahl’s Law 
  - **Don’t neglect I/O performance** as parallelism increases computer  performance 

- Example 
  - Benchmark takes 90s CPU time, 10s I/O time 
    - % IO time = 10/100 = 10%
  - Double the number of CPUs / 2-years 
    - I/O unchanged
  - CPU time은 줄여도 IO time은 줄일 수 없기 때문에 IO time이 차지하는 비율이 점점 커진다.
  - 따라서 speed up을 할 수 없는 부분인 I/O가 중요하다.

![image](https://user-images.githubusercontent.com/79521972/172289474-a58b7cdc-9884-4fdd-a6e8-73b92fa9b753.png)



<br>

## I/O System Design

- Satisfying latency requirements 

  - For time-critical operations 

- Maximizing throughput 

  - Find “weakest link” (lowest-bandwidth component) in  the I/O system 

    - Processor and memory system ? 
    - Underlying interconnection (i.e. bus?) 
    - I/O controllers ? 

    - I/O device themselves ? 

- Reconfigure the weakest link to meet the bandwidth  and/or latency requirements  

- Balance remaining components in the system

<br>

## Concluding Remarks

- I/O performance measures 
  - Throughput, response time 
  - Dependability and cost also important 
- Buses used to connect CPU, memory, I/O controllers 
  - Polling, interrupts, DMA 
- I/O benchmarks 
  - TPC, SPECSFS, SPECWeb 
- RAID 
  - Improves performance and dependabilit



disk controller 안에 dma가 들어있다.

