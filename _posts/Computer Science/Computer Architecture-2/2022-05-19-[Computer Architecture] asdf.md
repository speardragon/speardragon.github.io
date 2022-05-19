---
layout: single
title: "[Computer Architecture] Cache"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Cache Design']
---

<br>

## Cache Summary

- What data is held in the cache? 
  - Recently used data (temporal locality) 
  - Nearby data (spatial locality) 
- How is data found? 
  - Set is determined by address of data 
  - Word within block also determined by address 
  - In associative caches, data could be in one of several  ways 
- What data is replaced? 
  - Least-recently used way in the set



<br>

## Miss Rate Trends

![image](https://user-images.githubusercontent.com/79521972/169209647-63f7804f-a8a7-437c-8c36-940f47cb1dd0.png)

- Compulsory는 처음에 일어났다 그 이후는 거의 안생긴다.

- conflict도 cache size에 따라 줄어든다.

<br>

![image](https://user-images.githubusercontent.com/79521972/169209395-553f5456-44b8-4261-a4af-90060080a05f.png)

- Bigger blocks reduce compulsory misses 
- Bigger blocks increase conflict misses
- block을 무작정 키우면 set이 줄어듦 



<br>

## Cache Design Trade-offs

![image](https://user-images.githubusercontent.com/79521972/169210470-fd095529-e6c2-42b0-ad91-9f2f7ee9adb7.png)



<br>

## Multilevel Caches

- Larger caches have lower miss rates, longer access times 
- Expand memory hierarchy to multiple levels of  caches 
- Level 1 (Primary): attached to CPU 
  - **small and fast** (e.g. 16 KB, 1 cycle)
  - CPU와 가까이 있기 때문에 빨라야 함 
- Level 2: services misses from L1 cache 
  - larger and slower, but still faster than mail memory (e.g.  256 KB, 2-6 cycles) 
  - memory까지 안가기 위해서 커야함
- Most modern PCs have L1, L2, and L3 cache 
- (see Google.com for example CPUs)



<br>

## Multilevel Cache Example

- Given 
  - CPU base CPI = 1, clock rate = 4GHz 
  - Miss rate/instruction = 2% 
  - Main memory access time = 100ns 
- With just primary cache 
  - Miss penalty = 100ns/0.25ns = 400 cycles 
  - Effective CPI = 1 + 0.02 × 400 = 9 
  - 9 clock에 하나씩 instruction이 들어오는 것이므로 굉장히 성능이 안 좋다.
- Now add L-2 cache 
  - Access time = 5ns 
  - Global miss rate to main memory = 0.5% 
  - Primary miss with L-2 hit: Miss Penalty = 5ns/0.25ns = 20  cycles 
  - Primary miss with L-2 miss: Extra penalty = 400 cycles 
  - CPI = 1 + 0.02 × 20 + 0.005 × 400 = 3.4 
- Performance ratio = 9/3.4 = 2.6



![image](https://user-images.githubusercontent.com/79521972/169211405-140fe7a3-68dd-4ec4-bdcd-09f1e6ae671d.png)

위가 single cache, 아래가 multilevel cache



<br>

## Multilevel Cache Considerations

- Primary cache 
  - Focus on minimal hit time 
- L-2 cache 
  - Focus on low miss rate to avoid main memory  access 
  - Hit time has less overall impact 
- Results 
  - L-1 cache usually smaller than a single cache 
  - L-1 block size smaller than L-2 block size

<br>

## Intel Pentium III Die

![image](https://user-images.githubusercontent.com/79521972/169212056-803f05a0-19ac-4961-ac31-a5bb6fb86fe9.png)



<br>

## Intel Pentium III Die

![image](https://user-images.githubusercontent.com/79521972/169212103-e4cfe35a-de90-4b21-97e5-3fefd22c58e6.png)

Memory Controller -> DRAM



<br>

## Software optimization

- Assuming the arrays do not fit in the cache,  exchanging nesting of the loops can  improve spatial locality. 
- Data stored in row-major ordering (in C):

```c
/* before */
for(j = 0; j < 100; j++)
    for(i = 0; i < 5000; i++)
        x[i][j] = 2 * x[i][j];
```

```c
/* after */
for(i = 0; i < 5000; i++)
    for(j = 0; j < 100; j++)
        x[i][j] = 2 * x[i][j];
```

after와 같이 되어있어야 cache를 잘 사용하는 것

<br>

## Software optimization via Blocking

- Goal: maximize accesses to data before it is  replaced 
  - Consider the loops of DGEMM (double precision  general matrix multiplication): X = YZ

```c
for ( i = 0; i < n, ++i)
    for ( j = 0; j < n; ++j)
    {
        r = 0;
        for( k = 0; k < n; k++ )
            r += Y[i][k] * Z[k][j];
        X[i][j] = r;
    }
```

- n by n이 size가 작으면 상관 없는데 크면 문제가 된다.



The two inner loops read all N by N elements of Z,  read the same N elements in a row of Y repeatedly,  and write one row of N elements of X.

<br>

## Software optimization via Blocking

- X, Y and Z arrays

![image](https://user-images.githubusercontent.com/79521972/169212361-42d4b57a-b23d-4f9b-b839-762f666d6632.png)



<br>

## Software optimization via Blocking

```c
/* after */
for (jj=0, jj < n, jj = jj+B)
for (kk=0, kk < n, kk = kk+B)
    
for ( i = 0; i < n, ++i)
	for ( j = jj; j < min(jj+B, n); ++j)
	{
		r = 0;
		for( k = kk; k < min(kk+B, n); k++ )
			r += Y[i][k] * Z[k][j];
		X[i][j] = X[i][j] + r;
	}
```

적당한 block을 나누면 충분히 cache에 올라올 수 있기 때문에 그것에 대해서 계산을 진행하고 Block pointer를 옮겨가면서 계산을 쭉 이어 나간다.



The two inner loops now compute in steps of size B  rather than the full length of X and Z. (assume X is  initialized to zero.)

<br>

## Example (n=4, B=2)

![image](https://user-images.githubusercontent.com/79521972/169214785-235dacf1-3a5b-4fab-8696-a50a01e11236.png)

두번째 퍼즐 그림 잘못됨 -> 수정할 것

<br>

## Software optimization via Blocking

![image](https://user-images.githubusercontent.com/79521972/169215116-1b09e58b-2c96-4626-a68d-f5bbde0e7379.png)



- GFLOPS - 초당 몇 기가의 floating point 계산을 할 수 있는지
- blocked는 새로 block이 올라올 때만 바꾸면 되기 때문에 거의 감소하지 않는다.



<br>

## Cache Coherence Problem

![image](https://user-images.githubusercontent.com/79521972/169216130-30c73ac9-6fd9-4b9b-8123-f0a6f079f1cc.png)

- Suppose two CPU cores share a physical  address space –
  - Write-through caches

![image](https://user-images.githubusercontent.com/79521972/169215568-73bc85e1-bed9-4e1e-b928-eb3db90b55f5.png)

- 처음에는 memory에 0이 들어있었다.
- cache A가 이를 read 하면 A cache에는 0이 써질 것이다.
- cache B가 또 read하면 B에도 0이 써질 것이다.
- CPU A가 메모리 X에 1을 write한다.
- 그러면 cache와 memory에 값들이 전부 다 다른 문제가 발생한다. -> cache coherence

<br>

## Cache Coherence Protocols

- Operations performed by caches in multiprocessors to ensure coherence 
  - Migration of data to local caches 
    - Reduces bandwidth for shared memory 
  - Replication of read-shared data 
    - Reduces contention for access 
- Snooping protocols (snoop: 돌아다니면서 몰래 살펴보는)
  - Each cache monitors bus reads/writes 
  - bus를 모니터
- Directory-based protocols 
  - Caches and memory record sharing status of blocks in a directory



<br>

## Invalidating Snooping Protocols

- Cache gets exclusive access to a block when  it is to be written 
  - Broadcasts an invalidate message on the bus 
  - Subsequent read in another cache misses 
    - Owning cache supplies updated value



![image](https://user-images.githubusercontent.com/79521972/169217015-a73b5ae4-c9b9-48b6-8724-508174b5d734.png)



<br>

---

program마다 사용할 수 있는 address space가 있음

<img src="https://user-images.githubusercontent.com/79521972/169217938-dd532ae6-0848-4179-aa81-fe2b4d1aec62.png" alt="image" style="zoom:67%;" />

program마다 사용할 수 있는 용량이 100GB라고 하면 실제(physical) 메모리에는 10GB만 있다고 해 보자

프로그램은 100GB를 사용할 수 있지만 실제로 bus를 통해 가는 것는 PM(Physical memory)에서 간다.

- 근데 어떻게 10GB에서 오는데 100GB를 사용?

VM에서 일부분은 PM에서 떼 온 것이겠지만 나머지 부분은 hard disk에 있다. 그래서 PM에 없다면 100만배를 기다려서 harddisk에 갔다와야 하기 때문에 갔다오는 도중에 다른 program을 실행시키도록 한다.



performance 즉, miss rate을 제일 신경써야 한다.

가져오는 단위를 cache에서는 block 이었고 hard disk는 page이다.



- To reduce miss-rate
  1. Block size를 늘린다.(Block 대신 더 큰 Page 단위)
  2. associativity를 늘린다. (-> fully associativity; 늘릴수 있는 최대의 associativity)
  3. capacity size를 키운다.



- address가 어디 들어있는지를 알려주기 위해 각각의 VM마다 Page Table(PT)를 준다.





<br>

# Virtual memory

## Virtual Memory

- Gives the illusion of bigger memory 
- Main memory (DRAM) acts as cache for hard  disk

![image](https://user-images.githubusercontent.com/79521972/169217291-052e2f4c-4d36-4a58-9819-185138d771de.png)



<br>

## Memory Hierarchy

![image](https://user-images.githubusercontent.com/79521972/169217340-a8c9cecc-3038-4956-91cd-4484451f9862.png)



