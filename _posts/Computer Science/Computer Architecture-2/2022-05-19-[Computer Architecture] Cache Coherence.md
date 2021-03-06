---
layout: single
title: "[Computer Architecture] Cache Coherence"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Cache Design']
---

<br>

## Cache Summary

- What data is held in the cache? 
  - **Recently used data** (temporal locality) 
  - **Nearby data** (spatial locality) 
- How is data found? 
  - Set is determined by address of data 
  - Word within block also determined by address 
  - In associative caches, data could be in one of several  ways 
- What data is replaced? 
  - Least-recently used(LRU) way in the set



<br>

## Miss Rate Trends

![image](https://user-images.githubusercontent.com/79521972/169209647-63f7804f-a8a7-437c-8c36-940f47cb1dd0.png)

- Compulsory는 처음에 일어났다가 그 이후는 거의 안생긴다.

- conflict도 cache size에 따라 줄어든다.

<br>

![image](https://user-images.githubusercontent.com/79521972/169209395-553f5456-44b8-4261-a4af-90060080a05f.png)

- Bigger blocks reduce compulsory misses 
- Bigger blocks increase conflict misses
- block을 무작정 키우면 set의 갯수가 줄어들 것이다.
  - 그래서 결국 conflict miss가 늘어날 수 밖에 없음.




<br>

## Cache Design Trade-offs

![image](https://user-images.githubusercontent.com/79521972/169210470-fd095529-e6c2-42b0-ad91-9f2f7ee9adb7.png)

각각의 miss rate을 줄이기 위한 design change

- capacity miss
  - Increase cache size
  - 단점: may increase access time
- conflict miss
  - Increase associativity
  - 단점: may increase access time
- compulsory miss
  - Increase block size
  - 단점: increases miss penalty. For very large block size, may increase miss rate due to pollution

<br>

## Multilevel Caches

- 여러 개의 cache를 두는데 CPU와 가까운 cache는 최대한 빨라야 함.
- 그치만 miss rate도 줄여야 하기 때문에 L2 cache를 두어서 memory까지 안 가도록 함.

- Larger caches have lower miss rates, longer access times 
- Expand memory hierarchy to multiple levels of  caches 
- **Level 1** (Primary): attached to CPU 
  - **small and fast** (e.g. 16 KB, 1 cycle)
  - CPU와 가까이 있기 때문에 빨라야 함 
- **Level 2**: services misses from L1 cache 
  - larger and slower, but still faster than mail memory (e.g.  256 KB, 2-6 cycles) 
  - memory까지 안가기 위해서 커야함
- Most modern PCs have L1, L2, and L3 cache 
- (see Google.com for example CPUs)



<br>

## Multilevel Cache Example(시험)

- Given 
  - CPU base CPI = 1, clock rate = 4GHz 
  - Miss rate/instruction = 2% 
  - Main memory access time = 100ns 
- With just primary cache 
  - Miss penalty = 100ns/0.25ns = 400 cycles 
  - Effective CPI = 1 + 0.02 × 400 = 9 
    - hit이면 CPI=1, miss이면 miss rate에 해당하는 CPI( 0.02 × 400)
  - 9 clock에 하나씩 instruction이 들어오는 것이므로 굉장히 성능이 안 좋다.
  - ![image](https://user-images.githubusercontent.com/79521972/169930960-e39c204e-0d1f-48eb-8f95-b9cd8736885b.png)
- Now add L-2 cache 
  - Access time = 5ns 
  - Global miss rate to main memory = 0.5% 
  - Primary miss with L-2 hit: Miss Penalty = 5ns/0.25ns = 20  cycles 
  - Primary miss with L-2 miss: Extra penalty = 400 cycles 
  - CPI = 1 + 0.02 × 20 + 0.005 × 400 = 3.4 
  - ![image](https://user-images.githubusercontent.com/79521972/169931079-5e2cc71c-eebf-4604-865f-bd77182cca60.png)
- Performance ratio = 9/3.4 = 2.6



<br>

## Multilevel Cache Considerations

- Primary cache 
  - Focus on minimal hit time 
- L-2 cache 
  - Focus on low miss rate to avoid main memory access 
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

Memory Controller <-> DRAM



<br>

## Software optimization

- Assuming the arrays do not fit in the cache,  exchanging nesting of the loops can improve spatial locality. 
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

after와 같이 되어있어야 데이터가 저장되어 있는 순서대로 access하기 때문에 cache를 잘 사용하는 것

before 방식처럼 사용하게 되면 access를 세로로 하기 때문에 데이터 access를 계속 jump를 해 가면서 하기 때문에 locality를 완전히 무시하게 된다.

![image](https://user-images.githubusercontent.com/79521972/169933003-b950b3c7-7320-4a2b-b1c3-261e2c6927ba.png)

<br>

## Software optimization via Blocking

- **Goal**: maximize accesses to data before it is replaced 
  - Consider the loops of DGEMM (double precision general matrix multiplication): X = YZ

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

column을 가져와야 하는 j는 부담이 너무 크다.

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

그래서 한 번에 쭉 다 계산하는 것이 아니라 적당한 block을 나누면 충분히 cache에 올라올 수 있기 때문에 그것에 대해서 계산을 진행하고 Block pointer를 옮겨가면서 Block 마다 matrix 계산을 쭉 이어 나간다.



The two inner loops now compute in steps of size B  rather than the full length of X and Z. (assume X is  initialized to zero.)

<br>

## Example (n=4, B=2)

![image](https://user-images.githubusercontent.com/79521972/169214785-235dacf1-3a5b-4fab-8696-a50a01e11236.png)

두번째 퍼즐 그림 잘못됨 -> 수정할 것

<br>

## Software optimization via Blocking

![image](https://user-images.githubusercontent.com/79521972/169215116-1b09e58b-2c96-4626-a68d-f5bbde0e7379.png)



- GFLOPS - 초당 몇 기가의 floating point 계산을 할 수 있는지
- blocked는 새로 block이 올라올 때만 바꾸면 되기 때문에 초당 연산량이 거의 감소하지 않는다.



<br>

## Cache Coherence Problem

![image](https://user-images.githubusercontent.com/79521972/169216130-30c73ac9-6fd9-4b9b-8123-f0a6f079f1cc.png)

- Shared memory
- Suppose two CPU cores share a physical  address space –
  - Write-through caches

![image](https://user-images.githubusercontent.com/79521972/169215568-73bc85e1-bed9-4e1e-b928-eb3db90b55f5.png)

- 처음에는 memory에 0이 들어있었다.
- cache A가 이를 read 하면 A cache에는 0이 써질 것이다.
- cache B가 또 read하면 B에도 0이 써질 것이다.
- CPU A가 데이터 0을 1로 바꾸면 그 즉시 메모리 X에도 반영되어 write한다.
- 그러면 cache와 memory에 값들이 전부 다 다른 문제가 발생한다. -> cache coherence

두 CPU의 각 cache(즉, 클라이언트) 에서 접근한다고 했을 때 캐시의 갱신으로 인한 데이터 불일치가 생기는데 예를 들어, 쉽게 말해서 변수 x가 있는데 이 값은 0이다. 그런데 A에서 이 값을 1로 바꾸었고 B에서 x값을 읽는 상황을 가정해 보면, CPU B는 A가 수정한 값 1을 읽는 것이 아니라 현재 자신의 로컬 캐시인 0을 읽을 수 밖에 없다. 따라서 캐시 1, 2는 같은 X라는 변수에 대해 다른 값을 가지게 되므로 데이터 불일치 문제가 발생한다.

<br>

## Cache Coherence Protocols

- Operations performed by caches in multiprocessors to ensure coherence 
  - Migration of data to local caches 
    - Reduces bandwidth for shared memory 
  - Replication of read-shared data 
    - Reduces contention for access 
- **Snooping protocols** (snoop: 돌아다니면서 몰래 살펴보는)
  - Each cache monitors bus reads/writes 
  - bus를 통해 누가 쓰고 있는지 아닌지를 모니터한다.
- Directory-based protocols 
  - 어느 cache가 사용했는지 전부 기록하는 방식
  - Caches and memory record sharing status of blocks in a directory



<br>

## Invalidating Snooping Protocols

- Cache gets exclusive access to a block when it is to be written 
  - Broadcasts an invalidate message **on the bus** 
  - Subsequent read in another cache misses 
    - Owning cache supplies updated value

![image](https://user-images.githubusercontent.com/79521972/169217015-a73b5ae4-c9b9-48b6-8724-508174b5d734.png)

A가 memory X에 1을 write하는 경우 다른 CPU는 bus를 사용하지 못하도록 bus activity에 대해서 **Invalidate for X**를 알려주는 것이다.

그렇기 때문에 이 때 B의 cache에는 데이터가 없을 것이고 그 후에 memory를 read하게 되면 memory 대신 A가 write한 값을 B에게 주게 된다.



Q) 어떻게 A가 B한테 주는 거지? monitor를 통해 x는 invalidate하다고 되었기 때문에 A가 주는 건가?

<br>

---



