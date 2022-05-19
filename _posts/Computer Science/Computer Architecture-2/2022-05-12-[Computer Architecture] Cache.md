---
layout: single
title: "[Computer Architecture] Cache"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Cache']
---

<br>

# Preview) 

![image](https://user-images.githubusercontent.com/79521972/168110546-54765136-0fc9-4ae0-89a0-afd71247c351.png)

- 오른쪽에 있는 내가 공부를 하는데 책상에 책 몇 권만을 두고 공부를 한다.
  - 이 책들은 내가 손만 뻗으면 쉽게 available 하지만 많은 책을 두고 공부를 할 수 없다는 단점이 있다.
- 공부를 하다가 없는 책이 있다면 왼쪽에 있는 책꽂이로 가서 원하는 책을 가져온다.
  - 이 경우 조금의 시간이 더 걸린다.
- 만약 그 책꽂이에도 없다면 도서관 전체를 돌아 다니면서 찾아야 하고
- 그래도 없다면 인터넷을 통해 다른 서점에서 구하거나 해외에서 직구 하거나 해야 한다.
  - 굉장히 오랜시간이 걸린다.
- 이 굉장히 오랜 시간 걸릴 때 동안 숙제를 안 하고 있으면 굉장히 손해이기 때문에 그 책이 필요한 숙제는 잠시 접어두고 다른 숙제를 한다.

<br>

위의 비유에서 각각의 요소는 컴퓨터에서 무엇을 의미하는 지를 보자.

- 책상에 있는 책: register file 
- 책꽂이: cache(SRAM)
- 도서관: main memory(DRAM)
- 인터넷: HDD / SSD

- 나: CPU

<br>

memory(cache)에 있는 걸 register file로 가져오는 것은 프로그래머가 하는 일 

main memory에서 cache로 오는 것은 hardware 영역

main memory에 없는 걸 HDD에서 가져오는 것은 OS 담당

<br>

where to put?

how many?

가 이번 챕터에서 중요한 issue

<br>

- 들어갈 수 있는 장소가 딱 정해져 있으면 -> direct-mapped
  - 맨 마지막 숫자(digit)로 결정

- 아무데나 가도록 하는 것 -> fully associative

<br>

8번에 access하고 싶은데 이게 188번인지 208번인지를 모르기 때문에 앞에 18이라는 tag가 붙여져 있어서 이를 통해 찾는다.

<br>

direct-mapped와 fully associative의 절충안 -> n-way **set-associative**

<br>

프로그래밍을 할 때 locality를 고려하지 않고 짠다면 성능이 안 좋을 수 밖에 없다.

<br>

## Cache

![image](https://user-images.githubusercontent.com/79521972/167994879-da0b1ba9-0dad-4b6e-b4d5-132a3e81d492.png)

- 1.1 GHz(925ps)
  - CPI = 1


<br>

![image](https://user-images.githubusercontent.com/79521972/167994940-01ab04d4-2153-4ea8-b17a-e4e537868399.png)

- 3.1GHz(325ps)
  - CPI = 4.12


<br>
![image](https://user-images.githubusercontent.com/79521972/167994990-b20b40b5-72c1-4663-8ba8-c9fa8a6a39a1.png)

- 1.8GHz(550ps)
  - CPI = 1.15


- Pipelined에서 hazard를 해결하기 위해 굉장히 설계를 신중하게 했지만
- Data cache에 데이터가 없었다면 (즉, cache miss) main memory가서 가져와야 하는데 이는 50배 사이클 손해를 보게 된다.

- 즉 cache에 data 하나만 없어도 몇 십배의 손해를 보게 된다.(clock cycle 설계보다 훨씬 중요함.)

<br>

All three of our MIPS  designs assumed 1- clock for data and instruction memory; 
however, typical RAMs are 10-50  times slower



<br>

## Cache

![image](https://user-images.githubusercontent.com/79521972/167995359-235000dc-a772-4895-923e-4a8316b3ac48.png)

Line = Block

<br>

## Cache Design Parameters

- **Cache size** (in bytes or words). 
  - A larger cache can hold more of the  program’s useful data but is more costly and likely to be slower. 

- **Block or cache-line size** (unit of data transfer between cache and  main). 
  - 어디다 둘 거냐
  - With a larger cache line, more data is brought in cache with  each miss. 
  - This can improve the hit rate but also may bring low-utility  data in.  
  
- **Placement policy**. 
  - Determining where an incoming cache line is stored.  
  - More flexible policies imply higher hardware cost and may or may not  have performance benefits (due to more complex data location).

- **Replacement policy**. 
  - 어떤 걸 쫓아낼지
  - Determining which of several existing cache  blocks (into which a new cache line can be mapped) should be  overwritten.
  - Typical policies: choosing a random or the **least recently  used** (LRU) block. 

- **Write policy**.
  - 사실상 memory에 write을 해야 하는데 시간이 매우 많이 걸리기 때문에 cache를 잘 사용하여 write하도록 하자.
  - Determining if updates to cache words are immediately  forwarded to main (**write-through**) or 
    - 그때 그때 write 하는 것 -> 엄청난 시간 소요
  - modified blocks are copied back  to main if and when they must be replaced (**write-back** or copy-back).
    - 모아놨다가 몽땅 write 하는 것 -> 효율적임




<br>

## Desktop, Drawer, and File Cabinet Analogy

![image](https://user-images.githubusercontent.com/79521972/167995532-da6868a8-96e5-459e-9521-09b116a102a7.png)



- Items on a desktop (register) or in a dreawer (cache) are more readily accessible than those in a file cabinet (main memory)

도서관 예가 더 좋음

<br>

## Temporal and Statial Localities

![image](https://user-images.githubusercontent.com/79521972/167995919-c17e2606-7687-4420-a8d4-c5e8c5614782.png)



한 번 access 된 것은 계속해서 많이 사용되더라 + 한 번 access 되면 그 근처에 있는 것도 같이 access 되더라는 것을 경험적으로 알 수 있는 그래프

<br>

## Compulsory, Capacity, and Conflict Misses

- **Compulsory misses**: 
  - With on-demand fetching, first access to any  item is a miss. 
  - Some “compulsory” misses can be avoided by  prefetching. 
  - 시스템을 처음 켰을 때는 cache에 아무것도 없을 것이기 때문에 처음에 무조건 한 번은 갖다 놔야 한다.
  - invalid bit이기 때문에 생기는 misses
  
- **Capacity misses**: 
  - We have to oust some items to make room for  others. This leads to misses that are not incurred with an infinitely  large cache.  
  - cache 사이즈가 너무 작아서 생기는 misses

- **Conflict misses**: 
  - Occasionally, there is free room, or space  occupied by useless data, but the mapping/placement scheme  forces us to displace useful items to bring in other items. 
  - This  may lead to misses in future
  - cache size는 큰데, 들어올 자리는 한정되어 들어오는 data에 대해서 충돌이 일어나서 생기는 miss
    - 위 비유에서 188번과 208번 자리는 들어오는 곳이 똑같은 것과 같은 이유.

- main memory와 hard disk 간의 miss는 굉장히 큰 영향을 주기 때문에 이를 줄이기 위해선 무조건 fully associative를 사용해야 함

- 그런데 fully associative를 위해선 다 뒤져봐야 하기 때문에 이를 OS가 관리하도록 되어있다.(page table)

- virtual memory에서는 miss rate을 줄여야 함.



<br>

## Cache

- Highest level in memory hierarchy 
- Fast (typically ~1 cycle access time) 
- Ideally supplies most data to processor 
- Usually holds **most recently accessed data**

![image](https://user-images.githubusercontent.com/79521972/168116135-d27c922f-b891-407e-a52b-cbc50f235979.png)





<br>

## Cache Design Questions

- What data is held in the cache? 
- How is data found? 
- What data is replaced?(충돌이 나면 어떤 걸 replace 할 것인지)

We focus on data loads, but stores flow same principles.



<br>

## What data is held in the cache?

- Ideally, cache anticipates needed data and  puts it in cache 
- But impossible to predict future 
- Use past to predict future – temporal and  spatial locality: 
  - Temporal locality: copy newly accessed data  into cache 
  - Spatial locality: copy neighboring data into  cache too



<br>

## Cache Terminology

- **Capacity (C):**  
  - number of data bytes in cache (전체 cache size)
- **Block size (b):**   
  - bytes of data brought into cache at once (연속되어 있는 data를 한 번에 가져오는 크기)
- **Number of blocks (B = C/b):**  
  - number of blocks in cache: B = C/b 
- **Degree of associativity (N):**  
  - number of blocks in a set  
  - 메모리에 있는 data를 cache에 가져오는데 몇 자리에 넣을 수 있는지
- **Number of sets (S = B/N):**  
  - each memory address maps to exactly one cache set 
  - 



<br>

## How is data found?

- Cache organized into S sets 
- Each memory address maps to exactly one set 
- Caches categorized by # of blocks in a set:
  - **Direct mapped**: 1 block per set 
  - **N-way set associative**: N blocks per set 
  - **Fully associative**: all cache blocks in 1 set 
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

- 들어갈 수 있는 자리는 8자리이므로 3 bit로 표현을 하고 이를 set number라고 한다.

- word address의 맨 마지막 3 bit를 보고 들어갈지 말 지를 결정한다.
  - 뒤의 00(Byte offset)을 제외하고 마지막 3 bit

- 메모리가 들어갈 곳(번지)이 하나하나 다 정해져 있는 (associative) direct mapped and 1-way

- 그렇기 때문에 다른 메모리여도 같은 번지수를 가질 수 있기 때문에 같은 번지에 계속 접근하면 그 때마다 miss가 발생할 수 밖에 없다.

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

- set을 토대로 cache 에 access하여 그곳의 data를 바로 가져가는 것이 아니라 tag를 확인 비교해서 맞는 지를 확인하고 데이터 read를 하고 있는 중임을 나타내는 Validation bit와 AND 연산을 통해 Hit이 결정된다.
  - 만약 tag가 다르면 miss가 난 것.
- 초기에는 0이고 데이터가 load가 되었다라는 것을 알려주기 위한 validation bit

- Hit이면 data 부분을 쫙 가져가서 사용한다.

- cache memory의 size 

  - 1+27+32 =60 bit

  - total size = 60 x 8 = 480 


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

0x4: 0100  -> ...00**001**00

0x8: 1000  -> . ..00**010**00

0xC: 1100  -> ...00**011**00

- MMM HHH HHH HHH HHH
  - 처음에는 miss가 날 수 밖에 없음(compulsory misses)

MIPS Rate = 3/15 =20 % =0.2

- Temporal Locality

- **Compulsory Misses**(처음에 어쩔 수 없이 생기는 miss)



<br>

## Direct Mapped Cache: Conflict

![image](https://user-images.githubusercontent.com/79521972/167998864-98de0a7d-4a18-4f6f-b27c-42a955376cc0.png)



- 4번지와 24번지에 access하는 상황 -> 계속해서 싸운다.

  - 0x04: 00**001**00

  - 0x24: 01**001**00
    - 001(1)에 access 하려고 하기 때문에 miss 발생.
    - 4번지를 넣고 tag를 확인하려 하는데 tag가 다름
    - 24번지를 넣고 tag를 확인하려 하는데 tag가 다름
    - 위 과정이 무한 반복되어 계속해서 miss가 발생


- Miss Rate = 10/10 =100%
  - **Conflict Misses**


그럼 들어갈 수 있는 곳을 한 군데 말고 두 군데로 만들어보자라는 idea 도입 -> 2-way set associative 

<br>

## N-Way Set Associative Cache

![image](https://user-images.githubusercontent.com/79521972/167999041-54a0ce91-1b66-43de-b2e8-a9f61d478107.png)



- set이 지정하는 2 bit으로 줄었다. set의 갯수가 4개로 줄었기 때문에.(way가 두 개가 되면서)

- 전체 set의 갯수는 줄어들지만 어떤 set에서 들어갈 수 있는 자리가 2개가 마련되어 있는 것이다.
  - 두 경우를 모두 check하여 hit인 경우의 data를 뽑아온다.

<br>
![image](https://user-images.githubusercontent.com/79521972/167999135-f8bf0e11-24ec-411a-8a97-c9a267cd2877.png)

![image](https://user-images.githubusercontent.com/79521972/167999438-bd8fe7cd-3476-4341-8791-23cecc8e5dbe.png)

0x04: 000**001**00 

0x24: 001**001**00

- 이제는 싸우지 않고 다른 곳에 저장
- 처음만 miss고 나머지는 계속 Hit
- compulsory miss
- 2-way associativity로 conflict misses를 줄였다.



**Miss Rate** = 2/10 = 20%

- (2-way) Associativity reduces conflict misses



<br>

## Fully Associative Cache



![image](https://user-images.githubusercontent.com/79521972/167999847-be66138b-0f05-4a43-9ce5-284b29e2732c.png)

- 8-way
- 들어갈 수 있는 자리(set)을 쫙 일렬로 풀어서 아무데나 다 들어갈 수 있게 하면 몇 개가 중복이 되더라도 위처럼 계속 넣을 수 있지만 tag를 일일히 다 보면서 비교해야 하기 때문에 오히려 시간이 더 많이 걸리 수도 있기 때문에 너무 늘릴 수는 없다.
  - 그렇기 때문에 conflict miss는 확실히 줄인다. (2-way에서는 한계가 있었기 때문에)

- Reduces conflict misses 

- 가져올 때(read), Expensive to build (tag 비교하는 횟수가 너무 많아 그 만큼의 시간이 소모 된다.)



지금까지는 block size를 1로 생각해서 어디에 저장할 지를 중점으로 알아보았다.

---

Q) set과 associative의 차이

A)

![image](https://user-images.githubusercontent.com/79521972/167999041-54a0ce91-1b66-43de-b2e8-a9f61d478107.png)

- 몇 번지를 나타내는 것이 set

- set 안에 저장할 수 있는 공간이 몇 개 있는지를 나타내는 것이 n-Way

- set = cache의 address라고 생각하면 된다.

- hardware가 굉장히 커져서 delay가 길어지기 때문에 cache에서는 잘 사용하지 않는다.
- 하지만 VM에서는 한 번 갔다오는 시간이 엄청 걸리기 때문에 miss rate을 줄이는 것이 관건이기 때문에 VM에서는 fully associative를 사용한다.















