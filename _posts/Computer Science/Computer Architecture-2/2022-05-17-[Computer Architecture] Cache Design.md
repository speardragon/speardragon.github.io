---
layout: single
title: "[Computer Architecture] Cache Design"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Cache Design']
---

<br>

way는 한 cache(set) address에 들어갈 수 있는 장소가 몇 개가 있느냐?를 나타내는 숫자



애초에 tag를 비교하는 것은 시간이 오래걸려서 fully associative 같은 경우 Tag를 모두 비교하는 것은 매우 비효율적이기 때문에 이를 parallel 하게 모두 비교하자니 hardware 를 너무 무겁게 사용하게 된다.

그래서 이를 위한 특별한 메모리를 사용하는데 그것이 바로 Content addressable Memory(CAM) 이라고 한다.

<br>
direct mapped (b=2)

- block size가 2인 direct mapped
- 한 address를 가져올 때 그것과 인접한 address도 하나 같이 가져오는데 이 둘의 인접해 있을 것이기 때문에 **같은 tag**를 가지고 있을 것이다.

- 그렇기 때문에 별도의 tag 두 개가 필요한 것이 아니라 같은 tag에 두 개의 address가 들어가게 되는 것이다.





---

Q) block size가 크면 pollution data가 들어올 수도 있는데 왜 이렇게 하냐

A) Spatial Locality, 즉, 그 data를 가져왔다는 것은 그 근처의 data를 가져올 가능성이 크기 때문이다.



Q) 교수님 질문있습니다 혹시 2-way와 4-way일때는 tag를 비교하는데 8-way일때만 cam을 이용하는것 인가요 ?

A) b

---

fully associative는 delay가 길어서 cache에서는 사용못하지만 virtual memory에서는 사용한다.

<br>

## Spatial Locality?

- Increase block size: 
  - Block size, b = 4 words 
  - C = 8 words 
  - Direct mapped (1 block per set) 
  - Number of blocks, B = 2 (C/b = 8/4 = 2)

![image](https://user-images.githubusercontent.com/79521972/168721989-b4c21b9b-1cbc-4ce7-88c7-1a6ebaeaadbb.png)



<br>

## Cache with Larger Block Size

![image](https://user-images.githubusercontent.com/79521972/168722037-425c9329-baa6-4af5-af77-5c9e59f0b5e7.png)

hit인 경우를 판단하여 Hit일 때 memory address의 값을 읽어 data로 가져온다.

<br>

## Direct Mapped Cache Performance

![image](https://user-images.githubusercontent.com/79521972/168724373-ecab6642-edac-4fa4-8a4c-60f5a0206e28.png)

0x4: 000<span style="color:red">0</span>**01**00 

0xC: 000<span style="color:red">0</span>**11**00

0x8: 000<span style="color:red">0</span>**10**00

- 빨간색 ->  set number (위 그림에서 set이 두 개이기 때문에 1 bit가 필요하다.)
- bold 체 -> block offset  (위 그림에서 block의 갯수가 4개이기 때문에 2bit가 필요하다.)

한 번 가져올 때 miss가 나는데 (compulsory miss) 그걸 가져올 때 근처에 있는 것을 가져오기 때문에 그 이후에 그 주소를 가져올 때는 compulsory miss가 발생하지 않는다.



Q) 근처에 address를 가져오는데 근처의 기준이 뭐지? tag? 아니면 그 이후꺼를 쭉?

A)

<br>

## Block size Consideration

- Larger blocks should reduce miss rate 
  - Due to spatial locality 
- But, in a fixed-sized cache 
  - Larger blocks => fewer of them 
    - More competition => increased miss rate 
  - Larger blocks => pollution 
- Larger miss penalty 
  - Can override benefit of reduced miss rate 
  - Early restart and critical-word-first can hel



<br>

## Types of Misses

- **Compulsory**: first time data accessed 
- **Capacity**: cache too small to hold all data of  interest 
  - 전제적으로 cache size가 작아서 할 수 없이 생기는 miss
- **Conflict**: data of interest maps to same  location in cache
  - n-way를 조금 늘리면 괜찮아지는 miss (용량을 늘리고 tag로 확인)

**Miss penalty**: time it takes to retrieve a block from  lower level of hierarchy



<br>

## Cache Organization Recap

- Capacity: C  
- Block size: b 
- Number of blocks in cache: **B = C/b** 
- Number of blocks in a set: N 
- Number of sets: S = B/N

![image](https://user-images.githubusercontent.com/79521972/168725294-cccc1950-a96f-417b-aaac-3007e4a79145.png)

<br>

# Chapter 8: Memory Systems

## Cache Replacement Policy



<br>

## Replacement Policy

- Cache is too small to hold all data of interest at  once 
- If cache full: program accesses data X & evicts  data Y 
- **Capacity miss** when access Y again 
- How to choose Y to minimize chance of needing  it again?  
  - **Least recently used (LRU) replacement**: the least  recently used block in a set evicted

- cache에 데이터가 다시 들어오게 되는 경우 기존에 있던 것을 버려야 할텐데 무슨 기준으로 버려야 되는가?
  - LRU(Least recently used) replacement

<br>

- Direct mapped: no choice 
- Set associative: 
  - Prefer non-valid entry, if there is one 
  - Otherwise, choose among entries in the set Conflict: data of  interest maps to same location in cache 
- Least-recently used (LRU) 
  - Choose the one unused for the longest time 
    - Simple for 2-way, manageable for 4-way, **too hard beyond that** 
    - 2-way나 4-way는 managable 한데 엄청 큰 건 힘듦
- Random 
  - Gives approximately the same performance as LRU for  high associativity
  - 생각보다 성능이 높음(n이 클 때 거의 LRU와 비슷한 성능)


<br>

## LRU Replacement

![image](https://user-images.githubusercontent.com/79521972/168725967-4a0475b6-c6f0-4cd8-a599-cc71aab047c6.png)



- 0x04가 들어오고 0x24가 들어왔기 때문에 더 먼저 쓰인 04번지를 밀어내고 새로 54 번지가 들어오도록 한다.
- 둘 중에 어떤게 더 최근에 쓰였는지 항상 tracking을 해야 함.



<br>

## Cache Misses

- ☞ The next few slides are excerpts from the book, COD. ) 
- On cache hit, CPU proceeds normally 
- **On cache miss** 
  - Stall the CPU pipeline 
    - access를 할 수 없기 때문에 
  - Fetch block from **next level** of hierarchy 
  - Instruction cache miss 
    - Restart instruction fetch 
  - Data cache miss 
    - Complete data access

<br>

## Write-Through

- On data-write hit, could just update the block in cache 
  - cache에는 write이 바로 될지라도 실질적은 memory에 써지지는 않는다.
  - But then cache and memory would be inconsistent 
  
- **Write through**: also update memory 
  - cache가 update 될 때마다 memory도 update 하도록 한다!
    - 시간이 엄청 걸릴 것이다 -> buffer로 해결
  - 대신 cache와 memory의 내용이 같게 유지할 수 있다.
- But, makes writes take longer 
  - e.g., if base CPI = 1, 10% of instructions are stores, write  to memory takes 100 cycles 
    - Effective CPI = 1 + 0.1×100 = 11 
- **Solution**: <mark>write buffer</mark>
  - Holds data waiting to be written to memory 
  - CPU continues immediately 
    - Only stalls on write if write buffer is already full



<br>

## Write-Back

- Alternative: On data-write hit, just update the block in cache 
  - Keep track of whether each block is dirty 
- When a dirty block is replaced 
  - Write it back to memory 
  - Can use a write buffer to allow replacing block to be read first

또 다른 방식은 Write-back 방식이다. 이 방식은 블록이 교체될 때만 메모리의 데이터를 업데이트한다. 데이터가 변경됐는지 확인하기 위해 캐시 블록마다 `dirty` 비트를 추가해야 하며, 데이터가 변경되었다면 `1`로 바꿔준다. 이후 해당 블록이 교체될 때 `dirty` 비트가 `1`이라면 메모리의 데이터를 변경하는 것이다.


<br>

## Write Miss - Write Allocation

- What should happen on a Write Miss? 
- For, Write-through 
  - Write allocate (or fetch-on-write): fetch the block  followed by write-hit operation 
  - Write around (or write-no-allocate): not loaded to  cache, and is written directly to memory 
    - Since programs often write a whole block before reading it (e.g.  initialization) 
- For, Write-back 
  - Usually fetch the block



데이터를 변경할 주소가 캐싱된 상태가 아니라면(Write miss) Write-allocate 방식을 사용한다. 당연한 얘기지만, 미스가 발생하면 해당 데이터를 캐싱하는 것이다. write-allocate를 하지 않는다면 당장은 resource를 아낄 수 있겠지만 캐시의 목적을 달성하지는 못할 것이다.

<br>

## Example: Intrinsity FastMATH(시험)

- Embedded MIPS processor 
  - 12-stage pipeline 
  - Instruction and data access on each cycle 
- Split cache: separate I-cache and D-cache 
  - Each 16-KB: 256 blocks * 16 Words/block 
  - D-cache: write-through or write-back 
- SPEC2000 miss rate (benchmark program)
  - I-cache: 0.4% 
  - D-cache: 11.4% 
  - Weighted average: 3.2%


<br>

## Example: Intrinsity FastMATH

![image](https://user-images.githubusercontent.com/79521972/168726950-e68112ea-869e-40fd-aca8-359af5b6aae4.png)

Virtual memory와 합쳐진 processor 구조가 중요함 (추후에 배울 것)

<br>

## Main Memory Supporting Caches

- Use DRAMs for main memory 
  - Fixed width (e.g., 1 word) 
  - Connected by fixed-width clocked bus 
    - **Bus clock** is typically slower than CPU clock 
- Example cache block read 
  - Assume, 1 bus cycle for **address transfer** 
  - 15 bus cycles per **DRAM access** 
  - 1 bus cycle per **data transfer** 
- For 4-word block, 1-word-wide DRAM 
  - Miss penalty = 1 + 4×15 + 4×1 = 65 bus cycles 
  - Bandwidth = 16 bytes / 65 cycles = 0.25 B/cycle



- memory에서 가져오기 위해 address를 전달하는데 한 clock 걸린다고 가정( bus를 통해 전달)

- DRAM이니까 cell에 가서 메모리를 가져와서 적재하는데 15 bus cycle이 걸림.

- 전달하는데 1 bus cycle

그렇다면 4word(16 byte)를 가져오는데 몇 clock이 걸릴까?



<br>

## Increasing Memory Bandwidth

![image](https://user-images.githubusercontent.com/79521972/168727455-cc3e1f01-0465-40cd-b158-a4b54eae54b2.png)

![image](https://user-images.githubusercontent.com/79521972/168727464-fac87e10-1db3-4726-bd1e-4002576a8a2b.png)

- a->b : bus가 32에서 128로 변화했다하면

  - 4-word wide memory 
    - Miss penalty = 1 + 15 + 1 = 17 bus cycles 
    - Bandwidth = 16 bytes / 17 cycles = 0.94 B/cycle 
    
    - address가 bus를 통해 오면, 상위 address는 똑같기 때문에 memory bank에 동시에 찾아간다. 
    
    - 찾아 가는데는 15clock, 하나씩 보내는데 4clock
    
  - 4-bank interleaved memory 
    - Miss penalty = 1 + 15 + 4×1 = 20 bus cycles 
    - Bandwidth = 16 bytes / 20 cycles = 0.8 B/cycle
  
- b의 bus는 128 bit, c의 bus는 32bit

- c를 쓰는 것이 좋다.

메모리에 접근하는것은
캐시 하나 존재 > 메모리가 버스로 연결 > 메모리 구조이다.
이때, bandwidth를 늘리면 한번에 이동가능한 메모리양이 늘어난다. bus를 늘리기 위해서는 구조비용이 많이 들게 된다. 따라서 비용없이 늘리기 위해서는 bank라고 하는 개념이 도입된다.

사실 이해 잘 안 됨



https://parksb.github.io/article/29.html

https://velog.io/@blacklandbird/%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B5%AC%EC%A1%B0-8
