

## 1. 스토리지와 파일구조

### DBMS Storage

#### DBMS Storage

- DBMS는 데이터를 “hard” disk에 저장 
- Disk access가 DBMS의 성능에 중요한 문제 
  - READ : disk -> main memory 데이터 전송 
  - WRITE : memory -> disk 데이터 전송 
  - higher cost than memory access 
- Main memory에 모두 저장 못 하는 이유 
  - 비용문제 
  - 메모리에 저장된 데이터는 volatile 
- 효과적인 memory – disk 데이터 전송을 위한 Buffer management 가 필요

![image](https://user-images.githubusercontent.com/79521972/176451326-646b2175-eb1d-44c9-b41d-dd39f59d78b8.png)

<br>

### DBMS Storage

#### Disk space management

- DBMS에서 가장 낮은 layer에서 disk의 space를 관리 
- 상위 component에서 다음과 같은 request를 요청 
  - allocate & de-allocate a page 
  - read & write a page 
- 성능을 위해 page들을 최대한 sequential 하도록 allocation 한다 
  - seek / rotation delay을 줄이기 위해



<br>

#### Buffer Management

![image](https://user-images.githubusercontent.com/79521972/176451624-6495b693-c618-495d-a917-20ba93bc6ebb.png)



- Page buffering process 
  - Pool에서 page 을 저장할 frame을 선택 
    - free frame이 있으면 선택 
    - 없는 경우, replacement policy (LRU)에 의해 오래된 frame unpin 후에 frame 결정 
  - page을 disk에서 읽어 frame에 저장 
  - pin : pin_count++, page를 사용하는 사용자가 있음을 표시하기 위해 
    - pin_count가 0 인 경우만 replacement 대상 
  - 주소를 반환 
- Page 사용자는 사용이 끝나면 page에 대한 unpin을 불러주어야 한다 
  - 만약 업데이트 경우 dirty flag set 
- 읽는 pages list가 예측되면 성능을 향상 위해 prefetch 기능 제공



<br>

### Format in File

#### Record formats: Fixed Length

![image](https://user-images.githubusercontent.com/79521972/176451937-2dc480b5-96b5-4875-a916-6980e5e8d7f3.png)



- Field 타입과 size에 대한 정보가 system catalog에 저장 
- 필요한 Field access할 때 pointer 연산으로 빠르게 주소 반환

<br>

#### Page format for fixed length records

![image](https://user-images.githubusercontent.com/79521972/176452121-668d740f-cc14-4456-ba4d-e878cdf1f7ad.png)



<br>

### Format in File

#### Record formats: Variable Length

![image](https://user-images.githubusercontent.com/79521972/176452258-a6e5c167-6c94-4a74-bf7d-a6ca18ea67d8.png)

<br>

#### Page format for variable length records

![image](https://user-images.githubusercontent.com/79521972/176452422-0beb657b-2474-4d5e-8c1c-715de3379f91.png)



<br>

## 2. 인덱스 개념

### 인덱스

#### Indexes (indices)

- 특정 기준에 맞는 rows를 빠르게 찾기 위해 사용 
- Index trade-off 
  - Search 성능 향상 
  - 인덱스를 관리하는 비용 발생 
  - DML성능 저하 
  - 인덱스를 저장하기 위한 space 사용 증가 
- Search Key 
  - index에서 검색을 위해 사용되는 column 또는 column의 집합 
  - 한 테이블에서 다른 search key를 가지는 다수의 index가 생성 가능

<br>

#### 인덱스를 만들 때 고려 사항

- 인덱스가 언제 사용될 것인가? 
  - 정확하게 key와 일치하는 row들을 찾을 때 사용할 것인가? 
  - Range query에 사용하나? (values between, >, <= 등) 
  - 테이블에 모든 row 들을 조회할 때 사용할 것인가? 
- 테이블이 얼마나 자주 업데이트 되는가? 
  - DML은 index 관리를 위한 update가 발생 
- 인덱스의 key가 super key인가 또는 primary key인가? 
- 하나의 key로 여러 row 들이 가능한가? Unique or not

<br>

#### Tree (Ordered) Index vs. Hash Index

- Tree Index는 search key의 순서로 index entry가 정렬 
  - range query나 order by operation에 유리 
  - B+ Tree가 주로 사용 
- Hash Index 
  - index entry의 위치를 hash function을 이용하여 bucket의 위치를 지정하여 저장 
  - search 에 효율성 : O(1) 
  - DML이 발생할 때 index 관리에 유리

<br>

### 인덱스

#### Index on SQL

- DBMS 다음의 경우에 index 를 생성한다 

- Primary key 

- unique constraint 

- Create index statement in SQL 
  CREATE [UNIQUE] INDEX [using ] 
                ON (index_colomn_name, ...) 

  index_type : HASH, BTREE



<br>

## 3. B+ Tree 인덱스

### B+Tree

#### B+ Tree

- DBMS에 index로 사용되는 tree structure 
- 같은 레벨의 모든 leaf모드에 key와 실제 data pointer 저장 
- Components 
  - Root Node (적어도 두 개의 children을 가질 때) 
  - Non-leaf Node 
  - Leaf Node 
- 차수 n (Order n) 는 노드 사이즈을 결정한다 
  - size of Node = n + 1 pointers + n key

![image](https://user-images.githubusercontent.com/79521972/176454786-30715088-0cad-44ec-8af0-58058126db13.png)



<br>

#### Insert into B+ Tree

- Simple case : Leaf Node에 공간이 있는 경우
  - 예제) Insert key = 32

![image](https://user-images.githubusercontent.com/79521972/176454979-04133c70-5d27-4ec4-bcc8-5f98e70745f4.png)



![image](https://user-images.githubusercontent.com/79521972/176455042-077cc99f-8a02-40c1-bfc4-f80bbaef5c13.png)



- Leaf overflow case
  - 예제) Insert key = 7

![image](https://user-images.githubusercontent.com/79521972/176455165-784b0695-4a77-4f09-89a9-b3ed6d6a5d6b.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176455265-09d64c0b-eabe-48f4-ace8-eb5dc153aac9.png)



<br>

- Non-leaf overflow case
  - 예제) Insert key = 160

![image](https://user-images.githubusercontent.com/79521972/176455498-114ea0c5-3dce-44d8-9ee9-8fc170bd84b5.png)



![image](https://user-images.githubusercontent.com/79521972/176455630-1ae52d81-fcf1-489e-8c5b-b04362910cfb.png)



<br>

- new root case
  - 예제) Insert key = 45

![image](https://user-images.githubusercontent.com/79521972/176455776-ccac3283-f4c2-4266-bd72-69031b32a0f9.png)

![image](https://user-images.githubusercontent.com/79521972/176455829-62f38406-eae4-4106-99be-51aba68e03e8.png)

<br>

#### Delete from B+Tree

- Leaf node : Coalesce with sibling
  - 예제) delete key = 50

![image](https://user-images.githubusercontent.com/79521972/176456180-54bc920a-03ef-41cc-b3c4-27d4c7386263.png)

![image](https://user-images.githubusercontent.com/79521972/176456256-043323b5-2e0d-41fb-8e38-dfc663d8fb54.png)

<br>

- Redistribute key
  - 예제) delete key = 50

![image](https://user-images.githubusercontent.com/79521972/176456404-dbd97501-c2ec-4c85-81ad-bc876a2f9748.png)

- Non-leaf coalesce
  - 예제) delete key = 37

![image](https://user-images.githubusercontent.com/79521972/176456550-4434520b-2b53-47c8-8c00-7f7b86d569ef.png)



![image](https://user-images.githubusercontent.com/79521972/176456772-416a933f-4991-4c67-9381-b2ebc6bd684f.png)



<br>

## 4. 해쉬 인덱스

### 해쉬인덱스

#### Hash Index

- key에 대한 hash function을 통해 데이터 위치를 찾는 index 
- Index entry들이 bucket이라는 단위로 저장된다 
- Bucket을 할당하는 방법 
  - static hashing 
  - Bucket을 고정 된 수로 할당 
  - dynamic hashing 
  - 데이터 양에 따라 가변적으로 bucket 을 늘려나간다

![image](https://user-images.githubusercontent.com/79521972/176456998-d78b622f-b160-4741-84c7-84a4ac9e6a82.png)



<br>

#### Static Hashing

- Hash function의 결과에 bucket수로 mod 연산하여 위치 결정 
- Bucket수가 미리 정해져 있기 때문에 overflow 발생 가능 
  - Overflow chaining – linked list형태로 같은 hash key을 갖는 bucket을 연결한다

![image](https://user-images.githubusercontent.com/79521972/176457165-d88a68f9-e7bf-4546-a586-2b064d5c84de.png)



<br>

#### Dynamic Hashing - Extensible Hasing

- Static hashing과 달리 bucket수를 동적으로 변경하면서 hashing하는 방법 
- Hashing function의 결과를 binary로 표현하여 bucket의 할당에 binary의 prefix을 사용 
  - if prefix = 0, 2^0=1 bucket 
  - if prefix = 1, 2 bucket , prefix =3, 2^3 = 8 
- Bucket이 overflow가 발생하면 hash function의 prefix을 늘려가며 bucket의 개수를 늘 린다

![image](https://user-images.githubusercontent.com/79521972/176457341-8d273d57-00f9-4b4d-a9ea-d32333d21214.png)



<br>

- 예제) h(key) 4 bit, bucket당 2개의 key

![image](https://user-images.githubusercontent.com/79521972/176457655-442c15c3-4683-4012-a859-ed75c279abdf.png)

![image](https://user-images.githubusercontent.com/79521972/176457735-f71bf3b2-5f50-4d06-b59e-e15317b061db.png)





