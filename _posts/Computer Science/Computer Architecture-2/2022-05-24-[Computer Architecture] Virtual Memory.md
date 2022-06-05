---
layout: single
title: "[Computer Architecture] Virtual Memory"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Virtual Memory']
---

<br>

program마다 사용할 수 있는 address space가 있음

<img src="https://user-images.githubusercontent.com/79521972/169217938-dd532ae6-0848-4179-aa81-fe2b4d1aec62.png" alt="image" style="zoom:67%;" />

- CPU에는 여러 프로그램들이 존재하고 각 프로그램 마다는 주소가 존재한다.

- HDD에는 각 프로그램이 사용할 수 있는 virtual memory 공간이 할당되어 있고

- program마다 사용할 수 있는 용량이 100GB라고 하고 실제(physical) 메모리에는 10GB만 있다고 해 보자

- 프로그램은 100GB를 사용할 수 있지만 실제로 bus를 통해 가는 것는 PM(Physical memory)에서 간다.
  - 근데 어떻게 10GB에서 오는데 100GB를 사용?


- VM에서 일부분은 PM에 존재하지만 VM의 나머지 부분은 hard disk에 있다. 그래서 PM에 없다면 100만배를 기다려서 harddisk에 갔다와야 하기 때문에 잠시 stop 해 놓고 갔다오는 도중에 다른 program을 실행시키도록 한다.
- 이 때의 miss rate을 최소화하기 위해서 fully associativity 방식을 사용해야 한다.

  - 이를 통해 프로그램 할당된 여러 address 들이 모두 같은 0번지를 가지겠지만 이것이 실제 PM에 mapping 될 때는 다른 곳에 위치하게 되는 것이다.
  - 근데 fully associative는 어디에 위치했는지를 아는 것이 중요하다.

    - 어디 들어있다고 기록을 하는 장치(table)가 PM에 따로 존재 -> Page Table


<br>

- performance 즉, miss rate을 제일 신경써야 한다.

- 가져오는 단위가 cache에서는 **block**이었는데 hard disk는 **page**이다.



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

![image](https://user-images.githubusercontent.com/79521972/171068710-f392610d-7c7d-4091-858a-38ab11127c4e.png)



![image](https://user-images.githubusercontent.com/79521972/171068801-d59a965d-d313-4adb-921e-ed7aa55ef556.png)

1. CPU가 page table에 가서 해당 page에 접근한다.
2. 이 때, Memory에 올라온 상태가 아니면(invaild), Interupt를 발생한다.
3. OS 내부의 ISR에서 이 인터럽트를 처리하러 Disk에서 page를 찾는다.
4. 찾은 Page를 Memory에 올려 Frame화 한다.
5. page table을 업데이트 한다.
6. CPU에게 다시 수행하라고 명령한다.

<br>

# Virtual memory

## Virtual Memory

- Gives the illusion of bigger memory 
- Main memory (DRAM) acts as cache for hard disk

![image](https://user-images.githubusercontent.com/79521972/169217291-052e2f4c-4d36-4a58-9819-185138d771de.png)

cache <-> Main memory : hardware 적인 이동

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

sector가 하나의 데이터를 저장하는 단위

<br>

## Disk Sectors and Access

- Tens of thousands of tracks per surface, and each track is divided into sectors 
- **Each sector** records 
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
  - **Locality** and **OS scheduling** lead to smaller actual average seek times 
    - 한 track의 sector에 access하면 다음에도 그 근처를 access할 확률이 높다.
- Smart disk controller allocate physical sectors on disk 
  - Present logical sector interface to host 
  - SCSI, ATA, SATA 
- Disk drives include **caches** (RAM) 
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
    - OS가 page fault 발생시킴(cache의 miss와 같은 것)
- Memory Protection 
  - Each program has own virtual to physical mapping(PT) 
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

- 모든 프로그램은 각자의 Virtual address 용량을 모두 사용한다.
- 그런데 이 중에서 Physical address에 있는 것은 일부분 만이 들어와 있다.

<br>

## Address Translation

![image](https://user-images.githubusercontent.com/79521972/169943344-c99e07cd-6410-40dd-a590-e1a02b461d9e.png)

VPN: Virtual Page Number

PPN: Physical Page Number

- Virtual address = virtual page number + page offset
- page size가 4k라고 하면 그 4k가 되는 page 중에서 어디인지를 알아내는 것이 page offset이다.
- Virtual address에서나 physical address에서의 page 는 똑같을 것이기 때문에 동일한 page offset이 존재한다.
- 따라서 VPN -> PPN 변환 과정이 translation의 전부이다.

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

virtual page number 7FFFF -> 111 1111 1111 1111 1111 -> 19bit



<br>

![image](https://user-images.githubusercontent.com/79521972/169943825-2b037ce7-c54b-469f-8e44-f2c3466fa774.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/170392568-acb68b86-4e57-47a6-96ec-16a12c834a39.png)

- 위에서 봤듯이 page offset은 12 bit이기 때문에 3byte이므로 0x247C의 주소에서 뒤의 47C는 page offset을 나타낸다.
  - 47C -> 0100 0111 1100
- 따라서 VPN는 0x2가 되고 이를 PPN으로 변환해야 한다.
  - 0x2는 VPN에서 00002를 뜻하고 이는 Physical memory의 7FFF에 있다.
  - 00002 (VPN) => 7FFF (PPN)
  - 그래서 Virtual address의 2부분을 7FFF로 바꿔주면 된다.
- 따라서 최종적으로 Physical address는  0x7FFF47C 가 된다.

위 그림에서 VM과 PM이 이어진 것은 누구를 통해서 이루어 지는가?(Virtial memory를 Physical memory의 어디에 두어야 하는가)

- fully associative 

<br>

## How to perform translation?

- Page table 
  - Entry for each virtual page 
  - Entry fields: 
    - **Valid bit**: 1 if page in physical memory 
    - **Physical page number**: where the page is located



<br>

## Page Table Example

![image](https://user-images.githubusercontent.com/79521972/170418303-da1d5e5a-f72a-491e-bde4-8fa6e4c0b110.png)

Virtual page number로 page table에 먼저 접근해서 있으면 해당 table에서 Physical page number와 page offset을 합쳐 physical address를 만들어 낼 수 있다.

<br>

## Page Table Example 1

![image](https://user-images.githubusercontent.com/79521972/169944630-db8738d7-0f94-436a-af85-60bb61367eca.png)

What is the physical address of virtual address 0x5F20?

![image](https://user-images.githubusercontent.com/79521972/170392702-794230a3-b792-4f72-b790-e8830d6bd2e6.png)

<br>

## Page Table Example 2

![image](https://user-images.githubusercontent.com/79521972/169944848-e7326e4e-4f51-4129-9dc5-8ec95d542b1d.png)

What is the physical  address of virtual address **0x73E0**?

- VPN = **7** 
- Entry 7 is invalid (V = 0) -> **page fault**
  - Virtual page must be paged into physical memory from disk (page fault)




<br>

## Mapping Pages to Storage

![image](https://user-images.githubusercontent.com/79521972/169944968-833c24f9-4811-4104-b86f-dc84120124b3.png)

<br>

## Replacement and Writes

hard disk 까지 가야하는 경우를 대비하여 miss rate을 줄여야 하기 때문에 performance가 중요하다.

- **To reduce page fault rate**, prefer least recently used (**LRU**) replacement 
  - **Reference bit** (aka **use bit**) in PTE(page table entry) set to 1 on access to  page 
  - Periodically cleared to 0 **by OS** 
    - 주기적으로 0으로 바꿔주기 때문에 자주 쓰는 주소는 계속 1로 되어있을 텐데 쓴 지 오래 된 것은 0으로 바뀌는 것이다. 
  - A page with reference bit = 0 has not been used recently 
- Disk writes take millions of cycles 
  - Write through is impractical - 매우 비효율적
    - memory에 써질 때마다 disk에 올리는 것 -> 미친짓
  - Use write-back 
    - 계속 memory만 write하다가 memory page가 쫓겨 나야 할 때 memory에 써지는 방식
    - 즉, 메모리의 한 부분이 새로운 내용으로 바뀌려고 할 때
  - Dirty bit in PTE set when page is written
    - 메모리가 쓰여졌는지(내용이 바뀌었는지) 확인하는 bit



<br>

## Page Table Challenges

- **Page table is large** 
  - usually located in physical memory 
- Load/store requires **2 main memory accesses**:  (중요한 부분)
  - one for translation (page table read) 
  - one to access data (after translation) 
  
  - 즉, 실제 메모리에 접근하기 전에 Page Table을 통해 어디에 있는지를 먼저 보는 것
- 근데 page table도 결국은 DRAM이기 때문에 느리다.

- 이 두 번 access 하는 것도 더 줄일 수 있을까? -> **locality 사용**
- Cuts memory performance in half 
  - Unless we get clever…



<br>

## Translation Lookaside Buffer (TLB)

- 일종의 page table cache

- **Small cache** of **most recent translations** 
- Reduces # of memory accesses for most loads/stores from 2 to 1

- TLB에 없으면 miss이기 때문에 PT가서 가져옴 
- TLB가 꽉차면 쫓겨낼 때 메모리에 write

<br>

따라서 CPU에서 바로 Page table을 보고 물리적 주소로 변환하는 것이 아니라 중간에 있는 TLB를 먼저 봐서 최근에 access했던 virtual page가 physical page의 어디에 위치해 있는지 바로 변환하는 것이다.(있다면)

- TLB가 hit면 바로 가져옴
- TLB가 miss면 Page table에 가서 만약 valid 이면 최근에 access만 안 되었을 뿐이지 있다는 뜻이므로 TLB로 옮겨서 Physical memory로 찾아가고
- page table에 가봤더니 valid가 0이면 physical memory에 데이터가 존재하지 않는다는 뜻이기 때문에 **page fault**이고 Hard disk로 가는데 이는 시간이 너무 많이 걸리기 때문에 그냥 이 프로그램은 stop 시키고 기다리고 있는 다른 프로그램을 실행시켜 준다.

<br>

- 정리하자면 원래는 CPU에서 접근하고자 하는 virtual address가 있는데 이는 Physical addess로 변환되어야 한다. 그런데 변환을 하려면 fully associative 구조에서 찾아야 하기 때문에 page table을 이용하여 가상 메모리 공간을 index로 지정한 곳을 보고 변환을 하는 것이다. 그런데 여기서 또 page table에서 변환을 했던 것을  TLB에 기록하여 page table을 들리지 않고 물리적 주소를 얻을 수 있게 된다.
  - 즉, CPU가 어떤 virtual address를 사용하려고 하는데 TLB를 먼저 봐서 최근에 virtual page에서 physical page로 변환 된 것이 있나 보고 있다면 바로 변환 되고 없다면 page table에 가서 해당 가상 주소에 해당하는 physical address가 있나 보고 있다면 그 주소로 변환이 되고 만약 없다면 hard disk에 가서 가져와야 하는 것이다.

<br>

## TLB

- **Page table accesses**: high temporal locality 
  - Large page size, so consecutive loads/stores likely  to access same page 
- TLB 
  - Small: accessed in < 1 cycle 
    - CPU와 거의 같은 cycle
  - Typically 16 ~ 512 entries 
  - **Fully associative**, N-way도 적은 비율로 사용하긴 함.
    - -\> 99 % hit rates typical
      - it means 거의 memory access를 한 번만 한다.
  - Reduces # of memory accesses for most loads/stores **from 2 to 1**

![image](https://user-images.githubusercontent.com/79521972/170393324-ea90b5bd-988a-49f5-bbf4-72cb83e7462b.png)

<br>

## Example 2-Entry TLB

![image](https://user-images.githubusercontent.com/79521972/169945461-bd53c7d5-8ba8-4a64-86d5-f3f09db93bfc.png)

- 2개 짜리 TLB

- TLB를 먼저 봐서 같은 것이 있다면 해당 Physical page number를 바로 가져와서 사용한다.

- cache랑 다른 점
  - cache에서는 Tag이던 게
  - TLB에서는 Virtual Page Number이다.


<br>

## Fast Translation Using a TLB

☞ The next few slides are excerpts(발췌) from the book, COD, and Prof. Mary  Jane Irwin’s lecture notes (PSU). 

![image](https://user-images.githubusercontent.com/79521972/169945533-cdc01d8a-70ea-4705-aa0a-d1bb593a01cb.png)

- TLB miss -> Page Table에서 가져옴
  - valid로 판단

- Page Table miss -> Page fault -> Hard disk에서 가져옴

<br>

## Trasnlation Lookaside Buffers (TLBs)

- Just like any other cache, the TLB can be organized as fully associative, set-associative, or direct-mapped 
- TLB access time is typically smaller than cache access time (because TLBs are much smaller than caches) 
  - TLBs are typically not more than 512 entries even on high end machines


![image](https://user-images.githubusercontent.com/79521972/169945603-2f5379f9-f35c-4de4-88ca-88b360f2326b.png)

<br>

## Translation Lookaside Buffers (TLBs)

![image](https://user-images.githubusercontent.com/79521972/169947452-5370d03e-2e15-428e-ae15-bad155a8640b.png)

- A TLB miss is it a **page fault** or merely a **TLB miss**? 
  - If the page is loaded into main memory, then the **TLB miss can be handled** (in hardware or software) by loading the translation information **from the page table** into the TLB .
    - Takes 10’s of cycles to find and load the translation info into the TLB 
  - **If the page is not in main memory, then it’s a true page fault** 
    - Takes 1,000,000’s of cycles to service a page fault 
- TLB misses are much more frequent than true page faults

진행과정

1. CPU에서 Virtual address가 나온다. 
2. TLB에 찾아가서 PA가 있는지 확인하여 hit면 cache에 있으면 cache를 통해 없으면 main memory를 통해 CPU로 전달된다.
3. 그러나 TLB miss이면 page table에 가서 hit면 TLB에 가져다 놓고 PA로 변환되어 cache or memory를 통해 CPU로 전달된다.

<br>
Q) TLB 에서 virtual page number를 확인하는데 fully associative랑 n-way set이랑 다른가?

A) 

