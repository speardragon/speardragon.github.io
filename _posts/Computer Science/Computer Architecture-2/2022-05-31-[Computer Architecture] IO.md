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

## I/O (Input/Output)

- CPU입장에서 Mem빼고는 다 I/O
  - hard disk는 io와 controller에 연결 되어 있고 storage


![image](https://user-images.githubusercontent.com/79521972/171527681-72b72e86-9dfe-4624-a02f-1d79a89bde2f.png)



- 입출력 모듈는 왜 필요할까요? 
  - 다양한 장치가 존재하는데 각자의 속도는 다 다릅니다. 출력 데이터 포맷도 다 다르고 그래서 **일반화** 할 수 는 없을까 해서 만들어진 것이 '**I/O module**' 입니다. (CPU와 RAM보다 속도도 느리기 때문에 버퍼링 할 수 있는 게 필요 했습니다.)

https://com24everyday.tistory.com/173

- 빠른 것들은 그냥 붙이고 느린 것들은 bridge를 달아서 거기에 매다는 방식도 존재함
  - hierarchical bus



![image](https://user-images.githubusercontent.com/79521972/171528915-6c33181b-32b4-4ecc-b855-ad443d6f39d8.png)

- data를 저장하는 register와 status/control register가 존재한다.
  - 그런데 이런 것들 하나하나를 control 하는 것은 쉬운 일이 아니다.(chip을 설계한 사람이 아닌 이상)
  - 그래서 드라이버를 설치하는 것
- 하나하나 register와 logic마다 IO address가 존재한다.(I/O port)

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



 







# Chapter 6. Storage and Other I/O Topics

## Introduction

CPU입장에서 Memory가 아닌 것은 전부 I/O이다.

- I/O devices can be **characterized** by 
  - Behaviour: input, output, storage (어떤 행동?)
  - Partner: human or machine (누가 컨트롤?)
  - Data rate: bytes/sec, transfers/sec (얼마나 빠르게 전송?)
- I/O bus connections

![image](https://user-images.githubusercontent.com/79521972/171084746-56a2b456-f13d-4c86-b0e4-6121c4fb581c.png)

- 프로세서와 메모리는 프로세서가 원하면 메모리에서 데이터를 가져오거나 데이터를 쓰는 것이 가능한데
- IO 자체가 주체가 되어 데이터를 주고 받을 수도 있는데 
  - I/O들이 CPU에게 Interrupt를 걸기위한 interrupt 핀이 존재함 (그래야 I/O가 다 마쳤을 때 알리기 위함)
  - 키보드와 같은 것은 

- IO device들은 direct로 연결된 것이 아니라 IO controller를 중간에 거친다.
  - 그래서 CPU는 IO device가 어떻게 생겼는지 알 필요가 없고 
  - IO controller에 내장된 (두 종류의 register[control, cammand],  data, logic) 것들에 접근하여 데이터를 주고 받는 것이다.


<br>

## I/O System Characteristics

- CPU 부분에서는 performance가 중요했음

- memory 부분에서는 performance (throughput) 과 miss rate, capacitancy  과 같은 것들이 최대 관심사였다.
- IO는 물론 performance(**throughput**)도 중요하지만 IO 종류가 다양하다 보니까 **Latency가** 중요한 경우도 많다.
  - 또한 Dependability도 중요함 

- **Dependability** (reliability; 신뢰성) is important 
  - disk가 날라가면 엄청 큰일나기 때문에 speed 도 중요하지만 dependability가 매우 중요한 것
  - Particularly for storage devices 
- **Performance** measures 
  - Latency (response time) 
    - IO에서는 키보드와 같은 것은 치자마자 입력이 되는 것이 중요하기 때문에 Latency가 중요함
    - 키보드가 모았다가 보내는 것은 의미가 x
  - Throughput (bandwidth) 
    - gigabit internet 같은 것은 throughput이 중요함
  - IO종류에 따라 latency가 중요할 때도 Throughput이 중요할 때도 있다
  - Desktops & embedded systems (얘네는 아래가 중요함)
    - Mainly interested in **response time** & **diversity of devices** 
  - Servers (서버는 아래가 중요함)
    - Mainly interested in **throughput** & **expandability of devices**



<br>

## Dependability

- system이 얼마나 안전하냐 (reliability)

![image](https://user-images.githubusercontent.com/79521972/171087780-a64dbcf1-426a-41c7-afa7-c8401a8f540d.png)

system이 실행되다가 fail이 발생하면 interrupt가 발생하고 restoration 되면 다시 실행하는

- Fault: failure of a component
  - May or may not lead to **system failure**
- system에서 fail과 restore은 지정된 시간에 따라 발생하는 것이 아니기 때문에 평균을 구한다.

실제로 사용할 수 있는 구간은 fail이 발생하고 restore가 되면 그 restore된 시점부터 다음 fail이 발생하기 전까지이다.

<br>

## Dependability Measures

- Reliability – measured by the **mean time to failure** (**MTTF**). Service  interruption is measured by **mean time to repair** (**MTTR**) 
  - MTTF는 available한 상태

  - MTTR는 service interrupt 구간(사용하지 못하는)

- **Availability** – a measure of service accomplishment 
  - Availability = MTTF/(MTTF + MTTR) 
  - availablity를 높이기 위해선 분자를 높이던지 분모를 낮추던지
  - 고장이 덜나게 하려면(MTTF를 높이려면) -> Fault avoidance

- To increase MTTF, either improve the quality of the components or  design the system to continue operating in the presence of faulty  components 

- 1. Fault avoidance: preventing fault occurrence by construction 

- 2. Fault tolerance: using redundancy to correct or bypass faulty  components (hardware) 

     - l Fault detection versus fault correction

     - l Permanent faults versus transient faults



<br>

## Disk Storage

special IO

- Nonvolatile, rotating magnetic storage
  - 비휘발성, 자기를 띤, 겹친 디스크가 회전,
  - track의 sector마다에 데이터가 쓰여짐.


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
  - **Sector**: a physical spot on a formatted disk that holds information
  - **Block**: a group of sectors that the operating system can address 
    (1, 2, 4, 8 or even 16 sectors)
    - OS가 관리할 때는 block이라는 용어를 사용함.

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
- Disk drives include **caches**
  - Prefetch sectors in anticipation of access
  - Avoid (or reduce) seek and rotational delay



<br>

## Flash Storage(SSD)

- Non-volatile semiconductor storage
- 100× – 1000× **faster** than disk
- Smaller, lower power, more robust
- But more $/GB (between disk and DRAM)
- Flash memory bits **wear out** (unlike disks and DRAMs), but **wear leveling** can make it unlikely that the write limits of the flash will be exceeded
  - wear out - 10만번 정도 쓰면 자주쓰는 부분은 다 떨어짐, 마모 된다.
  - FTL이라는 펌웨어가 들어있음 -> 골고루 쓸 수 있도록 관리해 주는

<br>



## RAID: Disk arrays

<mark>dependability, throughput 을 위해 사용하는 방법</mark>

- **Redundant Array of Inexpensive Disks** (RAID) 
  - 말 그대로 disk를 여러개 쓰는 (disk array)
  - reliability 측면에서 하나 있는 거 보다는 더 떨어짐
    - disk가 많아질 수록 고장날 확률이 올라가기 때문에
  - 그치만 동시에 여러 개의 데이터를 준비시킬 수 있음
- Arrays of small and inexpensive disks 
  - **Increase** potential **throughput** by having many disk  drives 
    - Data is spread over multiple disk 
    - Multiple accesses are made to several disks at a time 
- Reliability is lower than a single disk 
- But availability can be improved by adding redundant disks (RAID) 
  - Lost information can be reconstructed from redundant  information 
  - MTTR: mean time to repair is in the order of hours 
  - MTTF: mean time to failure of disks is tens of years 



<br>

## RAID 0 (no redundancy)

- 원래 하나의 disk에 다 있던 block을 두 개의 disk에 interleaved 시킴

- Multiple smaller disks as opposed to one big disk 
  - Spreading over multiple disks (**striping**) means that **multiple blocks can be  accessed in parallel** increasing the performance  
    - A 4 disk system gives four times the  **throughput** of a 1 disk system 
  - Same cost as one big disk – assuming 4 small disks cost the same as one big disk 
- No redundancy, so what if one disk fails? 
  - Failure of one or more disks is more likely  as the number of disks in the system increases
- reliability가 떨어짐 - 하나 있을 때보다 두 개 있을 때 고장날 확률이 더 높기 때문에

![image](https://user-images.githubusercontent.com/79521972/171555616-d3cebbf8-5de1-4a6c-8a2a-e6258a4260d2.png)



<br>

## RAID 1 and 2

RAID 0 에서 reliability가 낮던 단점을 보완

- RAID 1: **Mirroring** 
  - N + N disks, **replicate** data 
    - Write data to both data disk and mirror disk 
    - On disk failure, read from mirror 
      - 하나가 고장나면 다른 곳에서 가져오면 됨
  - 용량이 두 배가 되기 때문에 비효율 

![image](https://user-images.githubusercontent.com/79521972/172031135-fcc5128b-9956-4847-915a-8000960ac41f.png)

- RAID 2: Error correcting code (ECC) 
  - N + E disks (e.g., 10 + 4) 
  - Split data at bit level across N disks 
  - Generate E-bit **ECC** 
    - Error correction code
    - 어느 부분에서 error가 났는지 판단 후 복구
  - Too complex, **not used in practice**

![image](https://user-images.githubusercontent.com/79521972/171555775-17bea934-2a8e-401b-9a6e-43553b19aa28.png)



<br>

## Striping

- Striping in Storage
- A technique of **segmenting logically sequential data**, such as a file, so that consecutive segments are stored on different physical storage devices.
  - consecutive segment가 다른 물리적 storage에 저장되어있다.
- By spreading segments across multiple devices which can be accessed concurrently, total data throughput is increased

![image](https://user-images.githubusercontent.com/79521972/172031193-c9040884-e140-44ba-8e69-72e053d47753.png)

<br>

## RAID 3: Byte-level striping with Parity

- N + 1 disks (+1은 parity check를 위한 disk)
  - Data striped(subdivided) across N  disks at byte level 
  - Redundant disk stores parity 
  - Read access: read all disks  
  - Write access: generate new parity and update all disks 
  - On failure: **use parity to reconstruct missing data** 
- Requires hardware support for efficient use 
- 6개가 하나의 블락
  - 한 블락을 access 하려면 저 6개를 다 access해야함

- **Not widely used**

![image](https://user-images.githubusercontent.com/79521972/172031212-a77b85b5-1c81-4e35-95e0-079945055f0c.png)



<br>

## RAID 4: Block-level striping

- N + 1 disks 
  - Data striped across N disks at block level 
    - 한 block이 한 disk에 담김
  - Redundant disk stores parity for a  group of blocks 
  - Read access: Read only the disk  holding the required block 
  - Write access: Just read disk  containing modified block, and parity  disk 
    - Calculate new parity, update data  disk and parity disk 
  - On failure: Use parity to reconstruct missing data 
    - parity를 계속 체크해야하는 비효율
- **Not widely used**

![image](https://user-images.githubusercontent.com/79521972/171556164-09fe90aa-c0ea-4a32-b607-5ff3a82697af.png)

RAID 4 setup with dedicated parity disk  with each color representing the group of  blocks in the respective parity block (a stripe)

<br>

## RAID 3 vs RAID 4

![image](https://user-images.githubusercontent.com/79521972/171556213-316133f9-0bd5-4b66-a251-1bfaf4d01cfb.png)

RAID 4에서는 block 단위로 하기 때문에 disk하나하나 access 하지 않아도 됨(reliablity는 줄어들긴 해도)

<br>

## RAID 5: Distributed Parity

- N + 1 disks
  - Like RAID 4, but parity blocks distributed across disks
    - Avoids parity disk being a bottleneck
- **Widely used**

![image](https://user-images.githubusercontent.com/79521972/172031260-5d191a65-11d3-4d09-a512-8e0f0f6fa5f9.png)

**parity block을 분산 시켜서 배치한 방식**

- 한 disk에만 traffic이 몰릴 것을 대비

<br>

## RAID 6: P + Q Redundancy

redundancy를 두 개 쓰자는 아이디어

- N + 2 disks 
  - Like RAID 5, but two lots of parity 
  - Greater fault tolerance through more redundancy 
- Multiple RAID 
  - More advanced systems give similar fault tolerance with better  performance

![image](https://user-images.githubusercontent.com/79521972/171556412-8d8e5283-7d02-483c-8406-1caf7fad1263.png)

- Allows up to two drives failure

![image](https://user-images.githubusercontent.com/79521972/171556330-b0076afb-dd2e-46c8-b38a-82a146667d5d.png)

복잡하고 cost가 올라가지만 두 개가 고장나도 복구가 가능하다.

<br>

## RAID Summary

- RAID can improve **performance** and **availability** 
  - High availability requires **hot swapping** (replacement of addition  of components to a computer system without stopping, shutting  down, or rebooting the system) 
  - interleaved된 구조로 인해서 performance 증가
  - 고장이 나도 parity라던가 redundancy로 복구가 가능함 -> dependability(availability)
- Assumes independent disk failures 
  - Too bad if the building burns down! 
- See “Hard Disk Performance, Quality and Reliability” 
  - http://www.pcguide.com/ref/hdd/perf/index.htm



<br>

## Review: components of a computer

![image](https://user-images.githubusercontent.com/79521972/171556562-855855af-65bc-459c-a508-e71d4eecd413.png)

- Important metrics for an I/O system (중요한 기준)
  - **Performance** 
  - Expandability 
  - **Dependability** 
  - **Cost**, size, weight 
  - Security
- IO 종류에 따라 중요하게 여겨지는 metric(기준)이 달라질 수 있다.



<br>

## A Typical I/O system

![image](https://user-images.githubusercontent.com/79521972/171556688-9a117fca-b523-4f6f-bf94-64dcbd087406.png)

IO bus를 기준으로 윗 부분이 master, 아랫 부분이 slave로 보고 

IO device마다 속도가 다름

사람이 CPU에게 요청할 수 있는데 이는 개별적으로 CPU에게 요청하는 것이 아니기 때문에 Interrupt라고 한다.

<br>

## Input and output Devices

- I/O devices are **incredibly diverse** with respect to 
  - Behavior – input, output or storage 
  - Partner – human or machine 
  - Data rate – the peak rate at which data can be transferred  between the I/O device and the main memory or processor

![image](https://user-images.githubusercontent.com/79521972/171556802-24468b73-eec9-46fb-b07e-0ec9e3b5c9ba.png)

<br>

## I/O Performance Measures

- I/O bandwidth (**throughput**) – amount of information that can be  input (output) and communicated across an interconnect (e.g., a  bus) to the processor/memory (I/O device) per unit time 
- How much data can we move through the system in a certain  time? 
- How many I/O operations can we do per unit time? 
- I/O response time (**latency**) – the total elapsed time to accomplish  an input or output operation

<br>

## I/O system interconnection issues

- A **bus** is a shared communication link (a single set of wires used to connect multiple subsystems) that needs to support a range of devices with widely varying latencies and data transfer rates 
  - 키보드와 같은 IO device가 CPU에게 데이터를 보내기 위해서 IO controller의 serial interface를 통한다.
  - **Advantages** 
    - **Versatile**(변하기 쉬움) – new devices can be added easily and can be  moved between computer systems that use the same bus  standard 
    - **Low cost** – a single set of wires is shared in multiple ways 
  - **Disadvantages** 
    - Creates a communication bottleneck 
      - bus bandwidth limits the maximum I/O throughput 
  
- The maximum bus speed is largely limited by 
  - The **length** of the bus 
  - The **number** of devices on the bus

위는 진짜 간단한 형태의 bus로 이 방식으로 구성하면 제한이 너무 크다.

<br>

## Types of Buses

- **Processor-memory bus** (“Front Side Bus”, proprietary) 
  - Short and **high speed** 
  - Matched to the memory system to maximize the memoryprocessor bandwidth 
  - Optimized for cache block transfers 
- **I/O bus** (industry standard, e.g., SCSI, USB, Firewire) 
  - Usually is lengthy and **slower** 
  - Needs to accommodate a wide range of I/O devices 
  - Use either the processor-memory bus or a backplane bus to  connect to memory 
- Backplane bus (industry standard, e.g., ATA, PCIexpress) 
  - Allow processor, memory and I/O devices to coexist on a single bus (built into backplane of computer) 
  - Used as an intermediary bus connecting I/O busses to the  processor-memory bus

<br>

## I/O Transactions(read/write)

- An I/O transaction is a sequence of operations over the interconnect that includes a **request** and may include a **response** either of which may carry data.  
- A transaction is initiated by a single request and may take many individual bus operations. An I/O transaction typically includes two  parts 
  - Sending the **address** 
  - Receiving or sending the **data**

- CPU에서 mem을 읽을 때 address가 mem으로 가면 어느 정도 후에 mem이 data를 실어주고 양방향으로 왔다갔다 하기도 한다.

  - 이 과정에서 메모리(slave)는 가만히 있고 CPU(master)가 능동적으로 작동한다.

  - 메모리는 수동적

- IO도 마찬가지로 address를 보내주고 데이터르 받아오는데 가만히 있는 것이 아니라 능동적으로

<br>

## Synchronous and Asynchronous Buses

- Synchronous bus (e.g., **fast** processor-memory buses) 
  - Includes a clock in the control lines and has a fixed protocol for  communication that is relative to the clock 
  - Advantage: involves very little logic and can run very fast 
  - Disadvantages: 
    - Every device communicating on the bus must use same clock rate 
    - To avoid clock skew, they cannot be long if they are fast 
- Asynchronous bus (e.g., **relatively slow** I/O buses) 
  - It is not clocked, so requires a **handshaking protocol** and additional  control lines 
    - (ReadReq, Ack, DataRdy) 
  - handshaking: 준비 됐어? -> 준비 됐다 -> 왔다 갔다
- Advantages: 
  - Can accommodate a wide range of devices and device speeds 
  - Can be lengthened without worrying about clock skew or  synchronization problems 
- Disadvantage: slow(er)



<br>

## Asynchronous Bus Handshaking Protocol

- Output (read) data from memory to an I/O device

![image](https://user-images.githubusercontent.com/79521972/171557549-81be5214-315d-4740-b61a-8824aee3a6b4.png)

![image](https://user-images.githubusercontent.com/79521972/172282066-50571ad6-7c93-48e5-86d4-2158f1d1d61e.png)

<br>

## Bus Types

- **Processor-Memory buses** 
  - Short, **high speed** 
  - Design is matched to memory organization 
- **I/O buses** 
  - **Longer**, allowing multiple connections 
  - 조금 느려도 low power?
  - Specified by standards for interoperability 
  - Connect to processor-memory bus through a bridg



<br>

## Bus Signals and Synchronization

- **Data lines** 
  - Carry address and data 
  - Multiplexed or separate 
- **Control lines** 
  - Indicate data type, synchronize transactions 
- **Synchronous** 
  - Uses a bus **clock** 
- **Asynchronous** 
  - Uses request/acknowledge control lines for handshaking



<br>

## I/O Bus Examples

![image](https://user-images.githubusercontent.com/79521972/171557774-0860d316-3c4f-44a2-b5e8-1eea95106d74.png)



<br>

## Typical x86 PC I/O System

![image](https://user-images.githubusercontent.com/79521972/171557842-f9eb3ec4-0abd-4dc5-b5f5-7c0899b8fb5a.png)

 north bridge를 통해서 high speed IO가 연결 되고

south bridge를 통해서 low speed IO가 연결된다.

<br>

## AMBA(Advanced Microcontroller Bus Architecture)

ARM

![image](https://user-images.githubusercontent.com/79521972/171557954-3f233873-1b5b-4371-9959-3eccb45d5118.png)



지금은 AXI로 변하였음 - network

- master가 여러개가 붙어 있음(core와 같은 것들)

<br>

## I/O Management

-  I/O is mediated by the **OS** (대부분 OS가 handling)
  - Multiple programs share I/O resources 
    - Need **protection and scheduling** 
  - I/O causes asynchronous **interrupts**
    - IO controller가 요청 
    - Same mechanism as exceptions 
  - **I/O programming is fiddly** (어렵다)
    - Low-level control of an I/O is complex and detailed 
    - OS provides abstractions to programs
    - IO controller가 수십개 수백개가 되는 경우가 있는데 이것들에 대해 잘 알아야 프로그래밍을 잘 짤 수 있다.
  - 그래서 IO 설계 logic은 설계자 외에 모른다.
    - 그래서 보통 드라이버 소스를 만들어서 제공해 주고 사용자가 실행하여 사용한다.



<br>

## I/O Commands

- I/O devices are managed by I/O controller hardware 
  - Transfers data to/from device 
  - Synchronizes operations with software 
- I/O registers (**I/O port addresses**) 
  - **Command registers** (control)
    - Cause device to do something 
  - **Status registers** 
    - Indicate what the device is doing and occurrence of errors 
  - **Data registers** 
    - Write: transfer data to a device 
    - Read: transfer data from a device



<br>

## I/O Register Mapping

몇 번지부터 몇 번지 까지 I/O를 쓸 것인가

- **Memory mapped I/O** 
  - Registers are addressed in **same space as memory**: 
    - the same load/store instruction used (lw/sw 사용)
  - Address decoder distinguishes between them 
  - OS uses address translation mechanism to make them only  accessible to kernel 
  - RISC
- **Isolated I/O (or separated I/O or I/O mapped)** 
  - Memory and I/O address spaces separated (분리되어 있음)
  - **Special instructions** to access I/O registers (e.g. in, out) 
  - Separated control signal (e.g. MEM/IO)  
  - Can only be executed in kernel mode 
  - Example: x86



<br>

## Memory-mapped I/O hardware

- Address Decoder: 
  - Looks at address to determine which device/memory  communicates with the processor 
- I/O Registers: 
  - Hold values written to the I/O devices 
- ReadData Multiplexer: 
  - Selects between memory and I/O devices as source  of data sent to the processor

![image](https://user-images.githubusercontent.com/79521972/172283407-19e5524d-9fd6-490e-beab-fd1f60b67e4b.png)

<br>

## Memory-mapped I/O code

- Suppose I/O Device 1 is assigned the address 0xFFFFFFF4 
  - Write the value 42 to I/O Device 1 
  - Read value from I/O Device 1 and place in $t3


![image](https://user-images.githubusercontent.com/79521972/171559093-7165c37c-73cc-475a-9fb7-d97b215610e9.png)



memory의 address에 따라 진짜 memory에 접근 하는 것일 수도 있고 IO에 접근하는 것일 수도 있다.



- 이와 다르게 Isolated의 경우 `out` or `in` 과 같은 명령어가 별도로 존재함.



<br>

## Communication of I/O devices and Processor

CPU와 IO간 어떤 방식으로 통신하는가?

IO는 memory와 다르게 IO device마다 속도가 다르기 때문에 memory처럼 일관적인 속도로 주고 받을 수 없다.

- **Polling** 
  - Periodically check I/O status register (through the OS) 
    - 주기적으로 **IO status register를 check**한다.(IO가 준비가 됐나... 하고 계속 체크하는 것)
    - If device ready, do operation -> device가 준비되면 작동 하고
    - If error, take action -> error이면 행동을 취한다.
  - Common in small or low-performance real-time embedded  systems 
    - Predictable timing 
    - Low hardware cost 
  - In other systems, wastes CPU time 
  - CPU가 하루종일 그것에만 **낭비**하면 손해임 그래서 나온게 Interrupt-driven I/O
    - 물론 목적이 하나인 임베디드라면 상관이 없겠지만... 범용 컴퓨터에선 중요함
- **Interrupt-driven I/O** (<span style="color:red">중요</span>)
  - I/O device **issues** and **interrupt** to ask service
  - more efficient - CPU가 할일이 많고 Interrupt 걸 수 있는 I/O가 많을 때 효율적이다.
    - 하지만 냉장고와 같은 거는 할 일이 없기 때문에 굳이 이런 기능이 없어도 되는 것이다.



<br>

## Polling example

- For input device

![image](https://user-images.githubusercontent.com/79521972/171559431-3c51814d-6480-4be9-8357-edaae427b1e6.png)

준비(loop)하다가 input

LSB가 준비 됐는지를 알려주기 때문에 그것을 계속 확인해서 1이면 즉, 준비 됐으면 s2에 read한다.

- For output device

![image](https://user-images.githubusercontent.com/79521972/171559463-9ccc4167-fe70-4c0a-b077-13f77d793000.png)

준비(loop)하다가 output

<br>

## Example (PIC 32 microcontroller)

![image](https://user-images.githubusercontent.com/79521972/172031784-e523a37b-faf3-4132-93ed-beb3b48ad933.png)

 Bus Matrix는 interconnection network로 되어 있으니까 여러개가 한 꺼번에도 가능

- 동시에 access 가능



<br>

## PIC32 MX675F521H block diagram

MIPS controller

![image](https://user-images.githubusercontent.com/79521972/172031814-c4438a39-a892-4552-a3ae-6567a29f327c.png)



<br>

## Example (keyborad control)

- Example (note that this is not MIPS)

![image](https://user-images.githubusercontent.com/79521972/172284468-e3c595a5-b361-4ed0-a6d0-0bb6a42f4e92.png)

<br>

## Example (PIC32 GPIO; Gernal Purpose IO)

여러 기능의 IO가 있어서 여러 개를 control 가능하다. -> 일부는 output으로 일부는 input으로

- (ex) Read four switches and turn on the  corresponding bottom four LEDs. 
  - TRIS(control) register determines input(1) and output(0) 
  - PORT(data reg) register: value from input or driven to output 
  - Examine pins RD[11:8], and write it back to RD[3:0].

```c
// C Code
#include <p3xxxx.h>
int main(void) {
    int switches;
    //1111 111100000000
    TRISD = 0xFF00; // RD[7:0] outputs ; control register 0이면 output,1이면 input
    // RD[11:8] inputs
    while (1) {
        // read & mask switches, RD[11:8]
        switches = (PORTD >> 8) & 0xF; // read(1111)한 결과를 읽기 위해
        PORTD = switches; // display on LEDs ; write
    }
}
```



<br>

