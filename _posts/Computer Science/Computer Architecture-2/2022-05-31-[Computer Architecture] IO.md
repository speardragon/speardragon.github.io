---
layout: single
title: "[Computer Architecture] I/O"
categories: ['Computer Science', 'Computer Architecture']
tag: ['I/O']
---

<br>

# Review)

![image](https://user-images.githubusercontent.com/79521972/171526044-95eef031-8793-4f80-a2b5-acd929f8b70d.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/171527681-72b72e86-9dfe-4624-a02f-1d79a89bde2f.png)



- 입출력 모듈는 왜 필요할까요? 
  - 다양한 장치가 존재하는데 각자의 속도는 다 다릅니다. 출력 데이터 포맷도 다 다르고 그래서 **일반화** 할 수 는 없을까 해서 만들어진 것이 '**I/O module**' 입니다. (CPU와 RAM보다 속도도 느리기 때문에 버퍼링 할 수 있는 게 필요 했습니다.)

https://com24everyday.tistory.com/173



![image](https://user-images.githubusercontent.com/79521972/171528915-6c33181b-32b4-4ecc-b855-ad443d6f39d8.png)

하나하나 register와 logic마다 IO address가 존재한다.(I/O port)

- memory와 별도로 I/O가 존재하는 경우 -> **Isolated I/O**
  - I/O 명령어가 따로 존재
- memory의 한 부분을 I/O로 사용하는 것 -> **memory-mapped I/O**
  - I/O를 할 때에도 그냥 기존의 lw, sw를 사용

<br>

I/O와 CPU가 통신하는 방법?

**Input Output Techniques**

1. **Programmed I/O** : CPU must wait (polling)

2. **Interrupted-driven I/O**

3. **Direct memory acess(DMA)** : CPU한테 데이터를 주고 받겠다는 허락을 한 번 받기만 하면 그 이후부터는 CPU 에 개입 없이 다른 게 직접 수행

![image](https://user-images.githubusercontent.com/79521972/171528894-43da1788-f8e0-49f7-ac73-6afa7faa05a1.png)

시스템 버스에는 CPU와 Memory가 붙고 아래쪽에 실제 디바이스가 위치한다.





![image](https://user-images.githubusercontent.com/79521972/171528871-6d623e62-e08a-4ffb-a764-5747792b9ba7.png)



 



direct memory access(DMA)

# Chapter 6. Storage and Other I/O Topics

## Introduction

CPU랑 Memory가 아닌 것은 전부 I/O이다.

- I/O devices can be **characterized** by 
  - Behaviour: input, output, storage (어떤 행동?)
  - Partner: human or machine (누가 컨트롤?)
  - Data rate: bytes/sec, transfers/sec (얼마나 빠르게 전송?)
- I/O bus connections

![image](https://user-images.githubusercontent.com/79521972/171084746-56a2b456-f13d-4c86-b0e4-6121c4fb581c.png)

- I/O들이 CPU에게 Interrupt를 걸기위한 interrupt 핀이 존재함 (그래야 I/O가 다 마쳤을 때 알리기 위함)

<br>

## I/O System Characteristics

- **Dependability** (reliability; 신뢰성) is important 
  - disk가 날라가면 큰일나기 때문에 speed 도 중요하지만 dependability가 매우 종요한 것
  - Particularly for storage devices 
- **Performance** measures 
  - Latency (response time) 
  - Throughput (bandwidth) 
  - Desktops & embedded systems (얘네는 아래가 중요함)
    - Mainly interested in **response time** & **diversity of devices** 
  - Servers (서버는 아래가 중요함)
    - Mainly interested in **throughput** & **expandability of devices**



<br>

## Dependability

system이 얼마나 안전하냐 (reliability)

![image](https://user-images.githubusercontent.com/79521972/171087780-a64dbcf1-426a-41c7-afa7-c8401a8f540d.png)

system이 실행되다가 fail이 발생하면 interrupt가 발생하고 restoration 되면 다시 실행하는

- Fault: failure of a component
  - May or may not lead to system failure

실제로 사용할 수 있는 구간은 fail이 발생하고 restore가 되면 그 restore된 시점부터 다음 fail이 발생하기 전까지이다.

<br>

## Dependability Measures

- Reliability – measured by the **mean time to failure** (**MTTF**). Service  interruption is measured by **mean time to repair** (**MTTR**) 

- **Availability** – a measure of service accomplishment 
  - Availability = MTTF/(MTTF + MTTR) 

- To increase MTTF, either improve the quality of the components or  design the system to continue operating in the presence of faulty  components 

- 1. Fault avoidance: preventing fault occurrence by construction 

- 2. Fault tolerance: using redundancy to correct or bypass faulty  components (hardware) 

     - l Fault detection versus fault correction

     - l Permanent faults versus transient faults



<br>

## Disk Storage

- Nonvolatile, rotating magnetic storage

![image](https://user-images.githubusercontent.com/79521972/171547241-733fa6cf-ffab-46ec-95e6-ad1f1b56da9c.png)

<br>

## Disk Sectors and Access

- Each sector records
  - Sector ID
  - Data (512 bytes, 4096 bytes proposed)
  - Error correcting code (ECC): used to hide defects and errors
  - Synchronization fields and gaps
- Access to a sector involves
  - Queuing delay if other accesses are pending
  - Seek: move the heads
  - Rotational latency
  - Data transfer
  - Controller overhead
- Blocks vs. sectors
  - Sector: a physical spot on a formatted disk that holds information
  - Block: a group of sectors that the operating system can address 
    (1, 2, 4, 8 or even 16 sectors)

<br>

## Disk Access Example

- Given

  - 512B sector, 15,000rpm, 4ms average seek time, 100MB/s transfer rate, 0.2ms controller overhead, idle disk

- Average read time

  - 4ms seek time

    ½ / (15,000/60) = 2ms rotational latency

    512 / 100MB/s = 0.005ms transfer time

    0.2ms controller delay
    = 6.2ms

- If actual average seek time is 1ms

  - Average read time = 3.2ms

<br>

## Disk Performance Issues

- Manufacturers quote average seek time
  - Based on all possible seeks
  - Locality and OS scheduling lead to smaller actual average seek times
- Smart disk controller allocate physical sectors on disk
  - Present logical sector interface to host
  - SCSI, ATA, SATA
- Disk drives include caches
  - Prefetch sectors in anticipation of access
  - Avoid (or reduce) seek and rotational delay



<br>

## Flash Storage

- Non-volatile semiconductor storage
- 100× – 1000× faster than disk
- Smaller, lower power, more robust
- But more $/GB (between disk and DRAM)
- Flash memory bits wear out (unlike disks and DRAMs), but wear leveling can make it unlikely that the write limits of the flash will be exceeded



<br>

## Flash Types

- NOR flash: bit cell like a NOR gate
  - Random read/write access
  - Used for instruction memory in embedded systems
- NAND flash: bit cell like a NAND gate
  - Denser (bits/area), but block-at-a-time access
  - Cheaper per GB
  - Used for USB keys, media storage, …
- Flash bits wear out after 10,000 ~ 1,000,000 
  accesses
  - Not suitable for direct RAM or disk replacement
  - Wear leveling: remap data to less used blocks



<br>

