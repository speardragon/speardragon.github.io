---
layout: single
title: "[Computer Architecture] Virtual Memory"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Virtual Memory']
---

<br>

program마다 사용할 수 있는 address space가 있음

<img src="https://user-images.githubusercontent.com/79521972/169217938-dd532ae6-0848-4179-aa81-fe2b4d1aec62.png" alt="image" style="zoom:67%;" />

- program마다 사용할 수 있는 용량이 100GB라고 하면 실제(physical) 메모리에는 10GB만 있다고 해 보자

- 프로그램은 100GB를 사용할 수 있지만 실제로 bus를 통해 가는 것는 PM(Physical memory)에서 간다.
  - 근데 어떻게 10GB에서 오는데 100GB를 사용?


- VM에서 일부분은 PM에 존재하지만 VM의 나머지 부분은 hard disk에 있다. 그래서 PM에 없다면 100만배를 기다려서 harddisk에 갔다와야 하기 때문에 잠시 stop 해 놓고 갔다오는 도중에 다른 program을 실행시키도록 한다.

<br>

performance 즉, miss rate을 제일 신경써야 한다.

가져오는 단위가 cache에서는 **block**이었는데 hard disk는 **page**이다.



- To reduce miss-rate
  1. Block size를 늘린다.(Block보다 더 큰 Page 단위)
  2. associativity를 늘린다. (-> fully associativity; 늘릴수 있는 최대의 associativity)
     - VM에서는 무조건 fully associativity
  3. capacity size를 키운다.



- address가 어디 들어있는지를 알려주기 위해 각각의 VM마다 Page Table(PT)를 준다.
  - 내가 쓰고 있는 page는 다른 사람들 보고 access하지 못하게 할 수 있다.

<br>

miss rate을 최소화하기 위해서 fully associative를 사용 -> 아무데나 들어갈 수 있음

이 때 어디 들어왔는지 어떻게 아느냐? -> Page Table

프로그램마다 page table을 갖기 때문에 A 프로그램의 0번지와 B 프로그램의 0번지는 다른 곳에 위치해 있다.



<br>

# Virtual memory

## Virtual Memory

- Gives the illusion of bigger memory 
- Main memory (DRAM) acts as cache for hard  disk

![image](https://user-images.githubusercontent.com/79521972/169217291-052e2f4c-4d36-4a58-9819-185138d771de.png)

cache <-> Main memory : hardware 적으로 이동

CPU에는 각 여러개의 프로그램이 있을텐데 각각 cache의 메모리를 사용하는 것이다.(virtual mermoy)

main memory <-> Hard disk : OS가 관리

<br>

## Memory Hierarchy

![image](https://user-images.githubusercontent.com/79521972/169217340-a8c9cecc-3038-4956-91cd-4484451f9862.png)

stall 하기에 너무 긴 시간이 걸리는 경우는 page fault

<br>

## Hard disk

- Nonvolatile, rotating magnetic storage

![image](https://user-images.githubusercontent.com/79521972/169942344-593987fb-debe-4cfd-9f8f-cae6072f7116.png)

- **Takes milliseconds to seek correct location on disk**

<br>

## Disk Sectors and Access

- Tens of thousands of tracks per surface, and each  track is divided into sectors 
- Each sector records 
  - Sector ID 
  - Data (512 bytes, 4096 bytes proposed) 
  - Error correcting code (ECC) 
  - Used to hide defects and recording errors 
  - Synchronization fields and gaps 
- Access to a sector involves 
  - Queuing delay if other accesses are pending 
  - Seek: move the heads 
  - Rotational latency 
  - Data transfer 
  - Controller overhead

<br>

## Disk Access Example(시험)

- Given – 512B sector, 15,000rpm, 4ms average seek time,  100MB/s transfer rate, 0.2ms controller overhead, idle  disk 

- Average read time 

  - 4ms seek time 

    \+ ½ / (15,000/60) = 2ms rotational latency 
    \+ 512 / 100MB/s = 0.005ms transfer time 
    \+ 0.2ms controller delay 
    = 6.2ms 

- If actual average seek time is 1ms – Average read time = 3.2ms

<br>

## Disk Performance Issues

- Manufacturers quote average seek time 
  - Based on all possible seeks 
  - **Locality** and **OS scheduling** lead to smaller actual  average seek times 
- Smart disk controller allocate physical sectors on  disk 
  - Present logical sector interface to host 
  - SCSI, ATA, SATA 
- Disk drives include caches (RAM) 
  - Prefetch sectors in anticipation of access 
  - Avoid seek and rotational delay

<br>

## Virtual Memory

- Virtual addresses 
  - Programs use virtual addresses 
  - Entire virtual address space stored on a hard drive 
  - Subset of virtual address data in DRAM 
  - CPU **translates** virtual addresses into physical addresses  (DRAM addresses) 
    - translate by page table
  - Data not in DRAM fetched from hard drive 
- Memory Protection 
  - Each program has own virtual to physical mapping(PT_) 
  - Two programs can use **same virtual address** for **different data** 
  - Programs don’t need to be aware others are running 
  - One program (or virus) can’t corrupt memory used by  another



<br>

## Cache/Virtual Analogues

![image](https://user-images.githubusercontent.com/79521972/169942935-5960adc5-5e20-4265-ad37-406c0ab35ec1.png)

**Physical memory acts as cache for vitual memory**



<br>

## Virtual Memory Definitions

- **Page size**: amount of memory transferred  from hard disk to DRAM at once 
- **Address translation**: determining physical  address from virtual address 
- **Page table**: lookup table used to translate  virtual addresses to physical addresses



<br>

## Virtual and Physical Address

![image](https://user-images.githubusercontent.com/79521972/169943212-90371f0b-4842-4249-88e9-f9ecf22c2e13.png)



**Most accesses hit** in physical memory 

But programs have the large capacity of virtual memory

- 모든 프로그램은 각각의 Virtual address를 모두 사용한다.
- 그런데 이 중에서 Physical address에 있는 것은 일부분 만이 들어와 있다.

<br>

## Address Translation

![image](https://user-images.githubusercontent.com/79521972/169943344-c99e07cd-6410-40dd-a590-e1a02b461d9e.png)

VPN: Virtual Page Number

PPN: Physical Page Number



<br>

## Vitual Memory Example

- **System**
  - Virtual memory size: 2 GB = 2<sup>31</sup> bytes 
  - Physical memory size: 128 MB = 2<sup>27</sup> bytes 
  - Page size: 4 KB = 2<sup>12</sup> bytes

- **Organization**: 
  - Virtual address: 31 bits 
  - Physical address: 27 bits 
  - Page offset: 12 bits 
  - \# Virtual pages = 2<sup>31</sup>/2<sup>12</sup> = 2<sup>19 </sup>(VPN = 19 bits) 
  - \# Physical pages = 2<sup>27</sup>/2<sup>12</sup> = 2<sup>15</sup> (PPN = 15 bits)

![image](https://user-images.githubusercontent.com/79521972/169944025-1a8d131f-7c7a-4c69-bbb6-7efc7c5d5953.png)

<br>

## Virtual Memory Example

- 19-bit virtual page numbers (VPN) 
- 15-bit physical page numbers (PPN)

![image](https://user-images.githubusercontent.com/79521972/169943773-16a658b8-214d-40ec-b178-8d25e07ee333.png)





<br>

![image](https://user-images.githubusercontent.com/79521972/169943825-2b037ce7-c54b-469f-8e44-f2c3466fa774.png)

247C 중에서 2는 Virtual page number이고 (777F) 

47C 이 page offset(page에서 몇 번째냐)이다.



<br>

## How to perform translation?

- Page table 
  - Entry for each virtual page 
  - Entry fields: 
    - Valid bit: 1 if page in physical memory 
    - Physical page number: where the page is located



<br>

## Page Table Example

![image](https://user-images.githubusercontent.com/79521972/169944554-efbc8755-9b7f-42e0-9237-5a41272cdd8d.png)

VPN is index into page table



<br>

## Page Table Example 1

![image](https://user-images.githubusercontent.com/79521972/169944630-db8738d7-0f94-436a-af85-60bb61367eca.png)

What is the physical  address of virtual  address 0x5F20?

- VPN = 5 
- Entry 5 in page table  VPN 5 => **physical  page 1** 
- Physical address:  0x1F20

<br>

## Page Table Example 2

![image](https://user-images.githubusercontent.com/79521972/169944848-e7326e4e-4f51-4129-9dc5-8ec95d542b1d.png)

What is the physical  address of virtual  address 0x73E0?

- VPN = 7 
- Entry 7 is invalid 
- Virtual page must be  paged into physical  memory from disk



<br>

## Mapping Pages to Storage

![image](https://user-images.githubusercontent.com/79521972/169944968-833c24f9-4811-4104-b86f-dc84120124b3.png)

<br>

## Replacement and Writes

- To reduce page fault rate, prefer leastrecently used (LRU) replacement 
  - **Reference bit** (aka **use bit**) in PTE set to 1 on access to  page 
  - Periodically cleared to 0 by OS 
  - A page with reference bit = 0 has not been used  recently 
- Disk writes take millions of cycles 
  - Write through is impractical 
  - Use write-back 
    - 계속 memory만 write하다가 memory page가 쫓겨 나야 할 때 memory에 써지는 방식
  - Dirty bit in PTE set when page is written
    - 메모리가 쓰여졌는지 확인하는 bit



<br>

## Page Table Challenges

- **Page table is large** 
  - usually located in physical memory 
- Load/store requires 2 main memory accesses:  
  - one for translation (page table read) 
  - one to access data (after translation) 
- Cuts memory performance in half 
  - Unless we get clever…



<br>

## Translation Lookaside Buffer (TLB)

- **Small cache** of **most recent translations** 
- Reduces # of memory accesses for most loads/stores from 2 to 1

없으면 miss이기 때문에 PT가서 가져옴 

<br>

## TLB

- **Page table accesses**: high temporal locality 
  - Large page size, so consecutive loads/stores likely  to access same page 
- TLB 
  - Small: accessed in < 1 cycle 
  - Typically 16 ~ 512 entries 
  - **Fully associative**
  -  -\> 99 % hit rates typical 
  - Reduces # of memory accesses for most  loads/stores from 2 to 1



<br>

## Example 2-Entry TLB

![image](https://user-images.githubusercontent.com/79521972/169945461-bd53c7d5-8ba8-4a64-86d5-f3f09db93bfc.png)



<br>

## Fast Translation Using a TLB

☞ The next few slides are excerpts from the book, COD, and Prof. Mary  Jane Irwin’s lecture notes (PSU). 

![image](https://user-images.githubusercontent.com/79521972/169945533-cdc01d8a-70ea-4705-aa0a-d1bb593a01cb.png)

TLB miss -> Page Table에서 가져옴

Page Table miss -> Page fault -> Hard disk에서 가져옴

<br>

## Trasnlation Lookaside Buffers (TLBs)

- Just like any other cache, the TLB can be organized as fully associative, set-associative, or direct-mapped 
- TLB access time is typically smaller than cache access time  (because TLBs are much smaller than caches) 
- TLBs are typically not more than 512 entries even on high end  machines

![image](https://user-images.githubusercontent.com/79521972/169945603-2f5379f9-f35c-4de4-88ca-88b360f2326b.png)

<br>

## Translation Lookaside Buffers (TLBs)

![image](https://user-images.githubusercontent.com/79521972/169947452-5370d03e-2e15-428e-ae15-bad155a8640b.png)

- A TLB miss is it a **page fault** or merely a **TLB miss**? 
  - If the page is loaded into main memory, then the TLB miss can  be handled (in hardware or software) by loading the translation  information from the page table into the TLB 
    - Takes 10’s of cycles to find and load the translation info into the TLB 
  - If the page is not in main memory, then it’s a true page fault 
    - Takes 1,000,000’s of cycles to service a page fault 
- TLB misses are much more frequent than true page faults

<br>

## TLB Event combinations

![image](https://user-images.githubusercontent.com/79521972/169947717-8dcec477-7ce9-4ba6-b888-4f2c48b43de6.png)

