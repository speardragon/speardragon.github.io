---
layout: single
title: "[Computer Architecture] Cache"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Cache']
---

<br>

초반 도서관 비유 정리하기



where to put?

how many?

가 중요한 issue



- 들어갈 수 있는 장소가 딱 정해져 있으면 -> direct-mapped
  - 맨 마지막 숫자로 결정

- 아무데나 가도록 하자 -> fully associative



8번에 access하고 싶은데 이게 188번인지 208번인지를 모르기 때문에 앞에 18이라는 tag가 붙여져 있어서 이를 통해 찾는다.



direct-mapped와 fully associative의 절충안 -> n-way set-associative



프로그래밍을 할 때 locality를 고려하지 않고 짠다면 성능이 안 좋을 수 밖에 없다.

<br>

## Cache

![image](https://user-images.githubusercontent.com/79521972/167994879-da0b1ba9-0dad-4b6e-b4d5-132a3e81d492.png)

1.1 GHz(925ps)

- CPI = 1

<br>

![image](https://user-images.githubusercontent.com/79521972/167994940-01ab04d4-2153-4ea8-b17a-e4e537868399.png)

3.1GHz(325ps)

- CPI = 4.12

<br>
![image](https://user-images.githubusercontent.com/79521972/167994990-b20b40b5-72c1-4663-8ba8-c9fa8a6a39a1.png)

1.8GHz(550ps)

- CPI = 1.15

Data cache에 데이터가 없었다면 (즉, data miss) main memory가서 가져와야 한다.(50배 사이클 손해)

- 즉 memory 하나만 없어도 몇 십배의 손해를 보게 된다.(clock cycle 설계보다 훨씬 중요함.)

<br>

All three of our MIPS  designs assumed 1- clock for data and  instruction memory; 
however, typical RAMs are 10-50  times slower



<br>

## Cache

![image](https://user-images.githubusercontent.com/79521972/167995359-235000dc-a772-4895-923e-4a8316b3ac48.png)


<br>

## Cache Design Parameters

- **Cache size** (in bytes or words). A larger cache can hold more of the  program’s useful data but is more costly and likely to be slower. 
- **Block or cache-line size** (unit of data transfer between cache and  main). With a larger cache line, more data is brought in cache with  each miss. This can improve the hit rate but also may bring low-utility  data in.  
- **Placement policy**. Determining where an incoming cache line is stored.  More flexible policies imply higher hardware cost and may or may not  have performance benefits (due to more complex data location).  
- **Replacement policy**. Determining which of several existing cache  blocks (into which a new cache line can be mapped) should be  overwritten. Typical policies: choosing a random or the **least recently  used** (LRU) block. 
- **Write policy**. 
  - Determining if updates to cache words are immediately  forwarded to main (**write-through**) or 
  - modified blocks are copied back  to main if and when they must be replaced (**write-back** or copy-back).



<br>

## Desktop, Drawer, and File Cabinet Analogy

![image](https://user-images.githubusercontent.com/79521972/167995532-da6868a8-96e5-459e-9521-09b116a102a7.png)



- Items on a desktop (register) or in a dreawer (cache) are more readily accessible than those in a file cabinet (main memory)

도서관 예가 더 좋음

<br>

## Temporal and Statial Localities

![image](https://user-images.githubusercontent.com/79521972/167995919-c17e2606-7687-4420-a8d4-c5e8c5614782.png)



<br>

## Compulsory, Capacity, and Conflict Misses

- **Compulsory misses**: With on-demand fetching, first access to any  item is a miss. Some “compulsory” misses can be avoided by  prefetching. 
  - 처음에 무조건 한 번은 갖다 놔야 한다.
- **Capacity misses**: We have to oust some items to make room for  others. This leads to misses that are not incurred with an infinitely  large cache.  
  - cache 사이즈가 너무 작아서 생기는 miss
- Conflict misses: Occasionally, there is free room, or space  occupied by useless data, but the mapping/placement scheme  forces us to displace useful items to bring in other items. This  may lead to misses in future
  - cache가 size는 큰데, 들어올 자리가 정해져 있는



virtaul memory에서는 miss rate을 줄여야 함.



<br>

## Cache

- Highest level in memory hierarchy 
- Fast (typically ~1 cycle access time) 
- Ideally supplies most data to processor 
- Usually holds most recently accessed data

![image](https://user-images.githubusercontent.com/79521972/167996732-3d497316-8fac-462a-8c0f-881e48a3a8f2.png)





<br>

## Cache Design Questions

- What data is held in the cache? 
- How is data found? 
- What data is replaced?

We focus on data loads, but stores floow same principles.



<br>

## What data is held in the cache?

- Ideally, cache anticipates needed data and  puts it in cache 
- But impossible to predict future 
- Use past to predict future – temporal and  spatial locality: 
  - Temporal locality: copy newly accessed data  into cache 
  - Spatial locality: copy neighboring data into  cache too



<br>

## Cache Terminaology

- Capacity (C):  
  - nuqmber of data bytes in cache 
- Block size (b):   
  - bytes of dqata brought into cache at once 
- Number of blocks (B = C/b):  
  - number of blocks in cache: B = C/b 
- Degree of associativity (N):  
  - number of blocks in a set  
- Number of sets (S = B/N):  
  - each memory address maps to exactly one cache set 
  - 들어갈 수 있는 자리 -> set



<br>

## How is data found?

- Cache organized into S sets 
- Each memory address maps to exactly one set 
- Caches categorized by # of blocks in a set:
  - Direct mapped: 1 block per set 
  - N-way set associative: N blocks per set 
  - Fully associative: all cache blocks in 1 set 
- Examine each organization for a cache with: 
  - Capacity (C = 8 words) 
  - Block size (b = 1 word) 
  - So, number of blocks (B = 8)



<br>

## Example Cache Parameters

- C = 8 words (capacity) 
- b = 1 word (block size) 
- So, B = 8 (# of blocks)

Ridiculously small, but will illustrate organizations





<br>

## Direct Mapped Cache

![image](https://user-images.githubusercontent.com/79521972/167997451-5c4390b5-bb61-408d-ba8c-776906040fa1.png)

word address의 맨 마지막 3 bit를 보고 들어갈지 말 지를 결정한다.

중복 되는 것에 대해서는  

<br>

## Cache fields

- Many addresses map to a single set 
  - Need to keep track of data contained in each set 
  - LSB(least significant bits) of the address specify  **which set** 
  - Remaining MSB bits (called **tag**) indicate **which of  many possible addresses**

![image](https://user-images.githubusercontent.com/79521972/167997610-9f65fa3d-852f-4eb7-8269-14a159b037a5.png)

<br>

## Direct Mapped Cache Hardware

![image](https://user-images.githubusercontent.com/79521972/167998009-3c872bcb-83e8-4724-9f8c-7281487561de.png)



<br>

![image](https://user-images.githubusercontent.com/79521972/167998108-e2813027-6b17-4677-b71f-e5861f0a9d27.png)

```
# MIPS assembly code
		addi $t0, $0, 5
loop: 	beq $t0, $0, done
		lw $t1, 0x4($0)
		lw $t2, 0xC($0)
		lw $t3, 0x8($0)
		addi $t0, $t0, -1
		j loop
done:
```

0x4: 0100  -> 0**001**00

0x8: 1000  -> 0**010**00

0xC: 1100  -> 0**011**00

MMM HHH HHH HHH HHH



MIPS Rate - 3/15 =20 % =0.2

- Temporal Locality

- Compulsory Misses(처음에 어쩔 수 없이 생기는 miss)



<br>

## Direct Mapped Cache: Conflict

![image](https://user-images.githubusercontent.com/79521972/167998864-98de0a7d-4a18-4f6f-b27c-42a955376cc0.png)



4번지랑 24번지랑 계속해서 싸운다.

Miss Rate = 10/10 =100%

- Conflict Misses



<br>

## N-Way Set Associative Cache

![image](https://user-images.githubusercontent.com/79521972/167999041-54a0ce91-1b66-43de-b2e8-a9f61d478107.png)



set을 지정하는 2 bit으로 줄었다.(4개 밖에 없기 때문)

전체 set의 갯수는 줄어들지만 



<br>
![image](https://user-images.githubusercontent.com/79521972/167999135-f8bf0e11-24ec-411a-8a97-c9a267cd2877.png)

![image](https://user-images.githubusercontent.com/79521972/167999438-bd8fe7cd-3476-4341-8791-23cecc8e5dbe.png)

이제는 싸우지 않고 다른 곳에 저장



Miss Rate = 2/10 = 20%

- (2-way) Associativity reduces conflict misses





## Fully Associative Cache



![image](https://user-images.githubusercontent.com/79521972/167999847-be66138b-0f05-4a43-9ce5-284b29e2732c.png)

tag를 다 보면서 비교해야 하기 때문에 오히려 시간이 더 많이 걸리 수도 있기 때문에 너무 늘릴 수는 없다.



Reduces conflict misses 

Expensive to build



지금까지는 block size를 1로 생각해서 어디에 저장할 지를 중점으로 알아보았다.



---

Q) set과 associative의 차이

A)

![image](https://user-images.githubusercontent.com/79521972/167999041-54a0ce91-1b66-43de-b2e8-a9f61d478107.png)

몇 번지를 나타내는 것이 set

set 안에 저장할 수 있는 공간이 몇개 있는지를 나타내는 것이 n-Way

set = cache의 address라고 생각하면 된다.

















