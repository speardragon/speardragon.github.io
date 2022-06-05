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

CPU와 Mem빼고는 다 I/O

- hard disk는 io와 controller에 연결 되어 있고storage

![image](https://user-images.githubusercontent.com/79521972/171527681-72b72e86-9dfe-4624-a02f-1d79a89bde2f.png)



- 입출력 모듈는 왜 필요할까요? 
  - 다양한 장치가 존재하는데 각자의 속도는 다 다릅니다. 출력 데이터 포맷도 다 다르고 그래서 **일반화** 할 수 는 없을까 해서 만들어진 것이 '**I/O module**' 입니다. (CPU와 RAM보다 속도도 느리기 때문에 버퍼링 할 수 있는 게 필요 했습니다.)

https://com24everyday.tistory.com/173



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

CPU 부분에서는 performance가 중요했음

memory 부분에서는 miss rate, capacitancy  과 같은 것들이 중요하게 작용함

- **Dependability** (reliability; 신뢰성) is important 
  - disk가 날라가면 큰일나기 때문에 speed 도 중요하지만 dependability가 매우 중요한 것
  - Particularly for storage devices 
- **Performance** measures 
  - Latency (response time) 
    - IO에서는 키보드와 같은 것은 치자마자 입력이 되는 것이 중요하기 때문에 Latency가 중요함
  - Throughput (bandwidth) 
    - gigabit internet 같은 것은 throughput이 중요함
  - IO종류에 따라 latency가 중요할 때도 Throughput이 중요할 때도 있다
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
- system에서 fail과 restore은 지정된 시간에 따라 발생하는 것이 아니기 때문에 평균을 구한다.

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
- Disk drives include caches
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
    - A 4 disk system gives four times the  throughput of a 1 disk system 
  - Same cost as one big disk – assuming 4 small disks cost the same as one big disk 
- No redundancy, so what if one disk  fails? 
  - Failure of one or more disks is more likely  as the number of disks in the system increases
- reliability가 떨어짐 - 하나 있을 때보다 두 개 있을 때 고장날 확률이 더 높기 때문에

![image](https://user-images.githubusercontent.com/79521972/171555616-d3cebbf8-5de1-4a6c-8a2a-e6258a4260d2.png)

Q) 하나 있을 때는 그거 

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

- N + 1 disks 
  - Data striped(subdivided) across N  disks at byte level 
  - Redundant disk stores parity 
  - Read access: read all disks  
  - Write access: generate new parity and update all disks 
  - On failure: use parity to reconstruct missing data 
- Requires hardware support for efficient use 
- 6개가 하나의 블락
  - 한 블락을 access 하려면 저 6개를 다 access해야함

- **Not widely used**

![image](https://user-images.githubusercontent.com/79521972/172031212-a77b85b5-1c81-4e35-95e0-079945055f0c.png)



<br>

## RAID 4: Block-level striping

- N + 1 disks 
  - Data striped across N disks at block  level 
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



<br>

## RAID 5: Distributed Parity

- N + 1 disks
  - Like RAID 4, but parity blocks distributed across disks
    - Avoids parity disk being a bottleneck
- **Widely used**

![image](https://user-images.githubusercontent.com/79521972/172031260-5d191a65-11d3-4d09-a512-8e0f0f6fa5f9.png)

parity block을 분산 시켜서 배치한 방식

<br>

## RAID 6: P + Q Redundancy

parity를 두 개 쓰자는 아이디어

- N + 2 disks 
  - Like RAID 5, but two lots of parity 
  - Greater fault tolerance through more redundancy 
- Multiple RAID 
  - More advanced systems give similar fault tolerance with better  performance

![image](https://user-images.githubusercontent.com/79521972/171556412-8d8e5283-7d02-483c-8406-1caf7fad1263.png)

- Allows up to two drives failure

![image](https://user-images.githubusercontent.com/79521972/171556330-b0076afb-dd2e-46c8-b38a-82a146667d5d.png)



<br>

## RAID Summary

- RAID can improve **performance** and **availability** 
  - High availability requires **hot swapping** (replacement of addition  of components to a computer system without stopping, shutting  down, or rebooting the system) 
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

<br>

## Input and output Devices

- I/O devices are **incredibly diverse** with respect to 
  - Behavior – input, output or storage 
  - Partner – human or machine 
  - Data rate – the peak rate at which data can be transferred  between the I/O device and the main memory or processor

![image](https://user-images.githubusercontent.com/79521972/171556802-24468b73-eec9-46fb-b07e-0ec9e3b5c9ba.png)

<br>

## I/O Performance Measures

- I/O bandwidth (throughput) – amount of information that can be  input (output) and communicated across an interconnect (e.g., a  bus) to the processor/memory (I/O device) per unit time 
- How much data can we move through the system in a certain  time? 
- How many I/O operations can we do per unit time? 
- I/O response time (latency) – the total elapsed time to accomplish  an input or output operation

<br>

## I/O system interconnection issues

- A **bus** is a shared communication link (a single set of wires used to  connect multiple subsystems) that needs to support a range of  devices with widely varying latencies and data transfer rates 
  - Advantages 
    - **Versatile**(변하기 쉬움) – new devices can be added easily and can be  moved between computer systems that use the same bus  standard 
    - **Low cost** – a single set of wires is shared in multiple ways 
  - Disadvantages 
    - Creates a communication bottleneck – bus bandwidth limits  the maximum I/O throughput 
- The maximum bus speed is largely limited by 
  - The **length** of the bus 
  - The **number** of devices on the bus



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
- **Backplane bus** (industry standard, e.g., ATA, PCIexpress) 
  - Allow processor, memory and I/O devices to coexist on a single bus (built into backplane of computer) 
  - Used as an intermediary bus connecting I/O busses to the  processor-memory bus

<br>

## I/O Transactions(read/write)

- An I/O transaction is a sequence of operations over the  interconnect that includes a request and may include a response either of which may carry data.  
- A transaction is initiated by a single request and may take many individual bus operations. An I/O transaction typically includes two  parts 
  - Sending the **address** 
  - Receiving or sending the **data**

- CPU에서 mem을 읽을 때 address가 mem으로 가면 data가 양방향으로 왔다갔다 함.

  - 이 과정에서 메모리(slave)는 가만히 있고 CPU(master)가 능동적으로 작동한다.

  - 메모리는 수동적

- IO는 가만히 있는 것이 아니라 능동적으로

<br>

## Synchronous and Asynchronous Buses

- Synchronous bus (e.g., fast processor-memory buses) 
  - Includes a clock in the control lines and has a fixed protocol for  communication that is relative to the clock 
  - Advantage: involves very little logic and can run very fast 
  - Disadvantages: 
    - Every device communicating on the bus must use same clock rate 
    - To avoid clock skew, they cannot be long if they are fast 
- Asynchronous bus (e.g., relatively slow I/O buses) 
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



<br>

## Bus Types

- Processor-Memory buses 
  - Short, high speed 
  - Design is matched to memory organization 
- I/O buses 
  - Longer, allowing multiple connections 
  - Specified by standards for interoperability 
  - Connect to processor-memory bus through a bridg



<br>

## Bus Signals and Synchronization

- Data lines 
  - Carry address and data 
  - Multiplexed or separate 
- Control lines 
  - Indicate data type, synchronize transactions 
- Synchronous 
  - Uses a bus clock 
- Asynchronous 
  - Uses request/acknowledge control lines for handshaking



<br>

## I/O Bus Examples

![image](https://user-images.githubusercontent.com/79521972/171557774-0860d316-3c4f-44a2-b5e8-1eea95106d74.png)



<br>

## Typical x86 PC I/O System

![image](https://user-images.githubusercontent.com/79521972/171557842-f9eb3ec4-0abd-4dc5-b5f5-7c0899b8fb5a.png)

bridge를 통해서 

<br>

## AMBA(Advanced Microcontroller Bus Architecture)

ARM

![image](https://user-images.githubusercontent.com/79521972/171557954-3f233873-1b5b-4371-9959-3eccb45d5118.png)



지금은 AXI로 변하였음 - network

- master가 여러개가 붙어 있음(core와 같은 것들)

<br>

## I/O Management

-  I/O is mediated by the **OS** 
  - Multiple programs share I/O resources 
    - Need protection and scheduling 
  - I/O causes asynchronous interrupts 
    - Same mechanism as exceptions 
  - **I/O programming is fiddly** (어렵다)
    - Low-level control of an I/O is complex and detailed 
    - OS provides abstractions to programs
  - IO 설계 logic은 설계자 외에 모른다.
    - 드라이버 실행



<br>

## I/O Commands

- I/O devices are managed by I/O controller  hardware 
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
  - Registers are addressed in **same space as memory**: the same load/store instruction used 
  - Address decoder distinguishes between them 
  - OS uses address translation mechanism to make them only  accessible to kernel 
  - RISC
- **Isolated I/O (or separated I/O or I/O mapped)** 
  - Memory and I/O address spaces separated 
  - Special instructions to access I/O registers (e.g. in, out) 
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



<br>

## Memory-mapped I/O code

- Suppose I/O Device 1 is assigned the address 0xFFFFFFF4 
  - Write the value 42 to I/O Device 1 
  - Read value from I/O Device 1 and place in $t3


![image](https://user-images.githubusercontent.com/79521972/171559093-7165c37c-73cc-475a-9fb7-d97b215610e9.png)



Isolated의 경우 `out` or `in` 과 같은 명령어가 별도로 존재함.



<br>

## Communication of I/O devices and Processor

CPU와 IO간 어떤 방식으로 통신하는가?

- **Polling** 
  - Periodically check I/O status register (through the OS) 
    - 주기적으로 IO status register를 check한다.
    - If device ready, do operation -> device가 준비되면 작동 하고
    - If error, take action -> error이면 행동을 취한다.
  - Common in small or low-performance real-time embedded  systems 
    - Predictable timing 
    - Low hardware cost 
  - In other systems, wastes CPU time 
  - CPU가 하루종일 그것에만 낭비하면 손해임 그래서 나온게 Interrupt-driven I/O
- **Interrupt-driven I/O** (<span style="color:red">중요</span>)
  - I/O device issues and interrupt to ask service
  - more efficient - CPU가 할일이 많고 Interrupt 걸 수 있는 I/O가 많을 때 효율적이다.
    - 하지만 냉장고와 같은 거는 할 일이 없기 때문에 굳이 이런 기능이 없어도 되는 것이다.



<br>

## Polling example

- For input device
  - polling

![image](https://user-images.githubusercontent.com/79521972/171559431-3c51814d-6480-4be9-8357-edaae427b1e6.png)

준비(loop)하다가 input

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

## Example (PIC32 GPIO; Gernal Purpose IO)

여러 기능의 IO가 있어서 여러 개를 control 가능하다.

- (ex) Read four switches and turn on the  corresponding bottom four LEDs. 
  - TRIS(control) register determines input(1) and output(0) 
  - PORT(data reg) register: value from input or driven to output 
  - Examine pins RD[11:8], and write it back to RD[3:0].

```c
// C Code
#include <p3xxxx.h>
int main(void) {
    int switches;
    TRISD = 0xFF00; // RD[7:0] outputs 
    // RD[11:8] inputs
    while (1) {
        // read & mask switches, RD[11:8]
        switches = (PORTD >> 8) & 0xF; // read 
        PORTD = switches; // display on LEDs ; write
    }
}
```



<br>

## Interrupts

CPU가 있으면 polling하지 않고 준비가 되면 IO가 interrupt를 걸어서 준비가 됐음을 안다.

- When a device is ready or error occurs 
  - Controller interrupts CPU 
- Interrupt is like an **exception** 
  - But not synchronized to instruction execution 
  - Can invoke handler between instructions 
  - Cause information often identifies the interrupting device 
- Priority interrupts 
  - Devices needing more urgent attention get higher priority 
  - Can interrupt handler for a lower priority interrupt 
- Special hardware needed 
  - **Interrupt controller**

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

- <span style="color:red">Direct memory access (DMA) </span>
  - OS provides starting address in memory 
  - I/O controller transfers to/from memory autonomously 
  - Controller interrupts on completion or error


<br>

- CPU는 다음과 기능이 있다. (셋 중 어느 기능이라도 없으면 CPU가 아님)

  - Arithmetic and Logical processing

  - Data move(lw/sw)

  - flow control

- DMA는 위 기능 중에서 다른건 다 못하고 Data move만 가능하도록 만든 것이다.

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



<br>

## DMA Data transfer (Example)

![image](https://user-images.githubusercontent.com/79521972/171561167-8292ebb2-1252-495a-8767-5a5d8793888e.png)

1. CPU transfer (read/write) data between memory and I/O
2. I/O operation is slow -> CPU overhead

![image](https://user-images.githubusercontent.com/79521972/171561248-ac76fd20-5533-493a-88bc-5ad9ee1901a9.png)

1. CPU writes command(r/w), device address, memory address, and count
2. Bus Request and Bus Grant
3. DMA controls data transfer (CPU can still use cache and do other calculation)
4. Once it's done, send interrupt to CPU



DMA가 Bus를 통해서 CPU에게 Request를 보냄

CPU가 허락함(Bus Grant)

그러면 CPU와 memory의 연결을 끊어 버림

그러면 DMA가 memory를 직접 주거니 받거니 한다.

- disk가 CPU를 거치지 않고 directly memory에 access 할 수 있게 됐다.



<br>

## DMA data Transfer

- If DMA writes to a memory block that is cached 
  - Cached copy becomes **stale** (탁해진다, 신선하지 않다)
- If write-back cache has dirty block, and DMA reads memory block 
  - Reads stale data 
- Need to ensure **cache coherence** 
  - memory에 있는 데이터가 cache에도 있을 경우에는 memory에서 가져오거나 memory에 write해 봤자 의미가 없다.
  - OS forces write-backs from cache if they will be used  for DMA (called a **cache flush**) 
  - Or use **non-cacheable memory** locations for I/O
