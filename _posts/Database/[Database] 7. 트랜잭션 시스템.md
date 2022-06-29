

## 1. 트랜잭션

### 데이터 무결성

#### 데이터 무결성

- 데이터 무결성을 유지하기 위해 해결해야 할 문제들 
  - Atomic operation 
  - Concurrency control (병행 제어) 
  - 장애 후 Recovery (회복) 
- Consistent database state 
  - 데이터 무결성이 유지되어 데이터베이스안의 데이터간의 모순점이 없는 상태 
  - 일시적으로 inconsistency가 발생할 수 있으나 결국은 일관된 값으로 유지되어야 함 
    - 예) 은행 계좌 이제 
- 트랜잭션 
  - 데이터 무결성을 지킴으로써 데이터베이스를 일관된 상태 (consistent state)을 유지하기 위한 핵심 개념

<br>

### Transaction

#### Transaction

- 트랜잭션(Transaction)은 다음의 data processing의 집합을 포함한 atomic operation  (unit of work) 
  - 하나 이상의 read operation  
  - 하나 이상의 write operation 
  - 트랜잭션 안에서의 데이터 계산 operation

<br>

### ACID

#### ACID: 트랜잭션의 특성

- Atomicity 
  - 트랜잭션안의 모든 operation이 실행되거나, 모두 실패 
- Consistency 
  - 트랙잭션의 실행이 database의 무결성 유지 
- Isolation 
  - 동시에 실행되는 트랜잭션은 서로 독립적이다 (또는 순차적으로 실행) 
- Durability 
  - 성공적으로 수행된 Transaction은 영원히 반영되어야 함을 의미



<br>

### Transaction States

#### Transaction States

![image](https://user-images.githubusercontent.com/79521972/176445194-86604ecd-3cc5-402d-bb76-316d17bfeb4c.png)



<br>

## 2. 트랜잭션 스케쥴

### Transaction Schedules

#### 트랜잭션 스케쥴

- 데이터베이스의 consistent state 를 유지하기 위해 동시에 실행되는 트랜잭션들의 operation의 순서를 정하는 걸 말한다 

- 예제) two update transactions account_no 123456의 현재 balance : 1000 

  - T1 : update account set balance = balance + 50 where account_no = 123456; 

  - T2 : update account set balance = balance - 100 where account_no = 123456; 

    ​	두 트랜잭션은 balance를 읽고 값을 update하는 연산으로 이루어져 있다

<br>

![image](https://user-images.githubusercontent.com/79521972/176445451-3f5ddf9d-a9f1-4ebb-acbb-b012e6e04540.png)

<br>

#### Serial or Serializable schedules

- 정확한 결과를 만드는 두 스케쥴은 serial schem 
- 예제) two update transactions account_no 123456의 현재 balance : 1000 
  - T1 : update account set balance = balance + 50 where account_no = 123456; 
  - T2 : update account set balance = balance - 100 where account_no = 123456; 
    - 두 트랜잭션은 balance를 읽고 값을 update하는 연산으로 이루어져 있다

### Transaction Schedules

![image](https://user-images.githubusercontent.com/79521972/176445693-94193f35-db73-45c3-b5a5-25235ec908f5.png)

#### Serial or Serializable schedules

- 정확한 결과를 만드는 두 schedule은 serial schedule 
  - 한 트랙잭션의 모든 operation이 끝난 후 다른 트랜잭션이 시작 
  - Serial schedule은 항상 consistent 결과 생성 
  - 순차적 실행으로 throughput 이 좋지 않음 
- Serializable schedules 
  - 스케쥴이 serial schedule과 equivalent 할 때 스케쥴을 serializable이라고 한다 
  - Serializable를 계산하기 위한 두 가지 개념 
    - conflict serializability 
    - view serializability

<br>

#### Conflict Serializability

-  Conflict Operations : 두 operation이 다음 조건을 만족할 때 conflicting이라 할 수 있다 
  - 두 개의 다른 트랜잭션에 속해 있고 
  - 같은 데이터에 대한 operation 이며 
  - 적어도 하나의 operation이 write인 경우 
- 예제 
  - Conflict : S1, S2, S3 
  - non-conflict : S4, S5 

![image](https://user-images.githubusercontent.com/79521972/176446161-a5d97af6-693e-4f89-a062-7361741ef7f0.png)

- Conflict serializable > non-conflict operation들의 순서 바꿈(swap)으로 serial schedule 로 변환이 되는 스케쥴

![image](https://user-images.githubusercontent.com/79521972/176446321-e5b70fde-29e1-4c5c-a3ef-9646f436c1d5.png)



<br>

#### view serializability

- View Equivalent 

  - 다음 조건들을 만족할 때 스케쥴 S1과 S2가 view equivalent하다고 한다 

    1. 스케쥴 안의 어떤 트랜잭션이 같은 초기값을 읽어야 한다 

    2. 만약 S1에서 T1이 T2가 write한 값을 읽는다면, S2에서도 T1이 T2가 write한 값을 값을 읽어야 한다 

    3. 마지막 write는 같은 트랜잭션에서 이루어져야 한다 

- View serializable 

  - serial 스케쥴과 view equivalent한 스케쥴 
  - confilct serializable 인 스케쥴을 포함

![image](https://user-images.githubusercontent.com/79521972/176446621-d594f439-705e-4c14-b588-086007da0d54.png)

![image](https://user-images.githubusercontent.com/79521972/176446689-0ace2a06-54b8-4442-8687-5e4c53cdac1f.png)

<br>

### Precedence Graph

#### Precedence graph

- 스케쥴의 conflict serializability을 테스트할 때 사용 
- 각 트랜잭션이 graph의 노드 
- 만약 트랜잭션간에 conflict이 있을 때 directed edge로 표시 
  - T2 가 write하기 전에 T1이 read할 때 
  - T2가 read하기 전에 T1이 write할 때 
  - T2가 write하기 전에 T1이 write할 때 
- 그래프가 cycle이 없다면 conflict serializable 스케쥴

![image](https://user-images.githubusercontent.com/79521972/176447011-a0509143-6f49-412a-a002-347da51d21e7.png)



<br>

### 예제

![image](https://user-images.githubusercontent.com/79521972/176447086-21091e11-b8e5-458b-91ee-96568e301109.png)



<br>

### Recoverability

#### 복구 가능한 스케쥴 (Recoverable Schedule)

- 스케쥴은 serializable해야 하고 복구 가능해야 한다 
- T2가 T1에 의해 바뀐 데이터를 사용할 경우 T1이 commit 되기 전에 T2는 commit하면 안된다 
  - 만약 T1이 abort되면 T2도 abort되야 한다 
- Cascading schedule 
  - T1 -> T2-> T3 -> T4 데이터가 이렇게 사용될 때 만약 T1이 실패하면 모든 트랜잭션 실패 
    - cascading rollback 
  - cascading schedule은 복구 불가능한 스케쥴을 만든다 
  - Cascadeless 스케쥴 : 다른 transaction들은 commit후에 변경된 데이터 read 
- Cascadeless 스케쥴은 recoverable 한 스케쥴이다

![image](https://user-images.githubusercontent.com/79521972/176447633-1a0865f6-35a8-4bfb-a9c7-6eab267d284f.png)

<br>

## 3. 회복(recovery) 시스템

### Recovery

#### Crash Recovery (회복)

- Recovery는 다음의 트랙잭션 특성과 관계가 있다 
  - Atomicity 
    - 트랙잭션의 operation들이 실행 중에 장애 발생 하면 실행된 operation들은 rollback이 필요 
  - Durability 
    - 장애 후에도 commit 된 트랙잭션의 데이터는 영원히 저장

![image](https://user-images.githubusercontent.com/79521972/176447856-f9c3a484-8fcc-4c20-bf3e-16ded3a7d680.png)

#### Logging &amp; Recovery

- Recovery Alogorithm은 장애 후 데이터베이스의 atomicity과 durability을 제공하는 기술 
- Log : 데이터베이스의 변경 내용을 저장하고 있는 파일

![image](https://user-images.githubusercontent.com/79521972/176448049-9a5edbab-7894-4429-aa26-df9759844bf3.png)



<br>

### Aries Recovery algorithm

- Aries = Algorithms for Recovery and Isolation Exploiting Semantics 
  - IBM fellow인 C.Mohan 1992년 발표 (link) 
  - Write-Ahead Log (WAL) 
    - 디스크에 변경된 내용을 적기 전에 반드시 로그가 안전한 storage에 저장 되야 한다 
    - 변경된 내용을 디스크에 log record을 먼저 기록 해야 한다 - (Atomicity 보장) 
    - Commit전에 모든 log record들이 저장되어야 한다 - (Durability 보장) 
- UNDO log 
  - Crash 후에 commit되지 않은 변경 사항들을 UNDO(원복) 할 때 사용 
- REDO log 
  - Crash 후에 commit 된 변경 사항들을 REDO (replay)할 때 사용

<br>

#### UNDO logging

-  모든 액션은 undo log record 생성 (old 값을 포함) 
- Disk 업데이트하기 전에 log record를 disk에 적는다 (WAL) 
- Commit 로그가 flush되기 전 트랜잭션의 모든 체인지는 Disk에 반영

#### REDO logging

- 모든 액션은 redo log record 생성 (new 값을 포함) 
- Disk 업데이트하기 전에 log record를 disk에 적는다 (WAL) 
- 모든 log를 commit할 때 flush한다 
- 체인지가 disk에 반영된 후에 END log  record를 write



<br>

#### Undo 예제

![image](https://user-images.githubusercontent.com/79521972/176448627-204194e0-c08e-4d5b-a862-53289d467f5d.png)



![image](https://user-images.githubusercontent.com/79521972/176448686-e8ca3d89-b53c-499b-811d-17323435d218.png)

![image](https://user-images.githubusercontent.com/79521972/176448732-f3051674-2a5b-455d-beb1-2a6b7eab3aaf.png)



![image](https://user-images.githubusercontent.com/79521972/176448765-476a3133-4e22-454a-bae3-3f1d7392de11.png)



![image](https://user-images.githubusercontent.com/79521972/176448800-b6f15b38-f27e-4a13-9afc-a7c5a8f80461.png)



![image](https://user-images.githubusercontent.com/79521972/176448836-c1253dbe-b3c0-4561-9b64-9c0486c89e68.png)

![image](https://user-images.githubusercontent.com/79521972/176448908-1ac9c293-b5e3-4cb2-a418-be88a8f49e24.png)



![image](https://user-images.githubusercontent.com/79521972/176448950-fa431898-5f91-4e69-9c31-024fddeb6d89.png)

<br>

### Recovery Rules

![image](https://user-images.githubusercontent.com/79521972/176449282-63bb7696-d4d5-4ad6-9c01-ae18a2c049a1.png)

![image](https://user-images.githubusercontent.com/79521972/176449361-ac741b9d-7b6f-46bd-a5f6-b964a1d8918e.png)

<br>

### Recovery

#### Checkpoint

- DBMS는 주기적으로 체크포인트를 생성하여 시스템 충돌 시 복구에 걸리는 시간을 최소화 
  - Checkpoint log 작성 
- Fuzzy checkpoint 
  - checkpoint하는 동안 긴 시간을 다른 operation을 block하는 걸 막기 위해 checkpoint 하는 동안 update는 허용하는 checkpoint 기술

![image](https://user-images.githubusercontent.com/79521972/176449617-8f6278e0-d97e-4733-a4ac-d4585feb60e3.png)

- 예제)

![image](https://user-images.githubusercontent.com/79521972/176449704-eb82cbb1-6ddb-4cee-ba77-b85166055ed7.png)

<br>

## 4. 병행 제어

### Lock Protocols

#### Locking protocol in DBMS

- 동시에 실행되는 트랜잭션 간의 serializable 스케쥴을 만들어 주기 위한 구현된 메카니즘 
- Lock를 획득하기 전에 데이터에 읽거나 쓰는 동작을 할 수 없게 하여 동시성 문제 해결 
- Lock-manager in DBMS 
  - 모든 lock에 대한 request를 처리 
  - lock에 대한 정보를 hash table에 저장하여 관리 
- Lock 의 종류 
  - shared lock (S) 
    - read-only lock 
    - read 트랜잭션간에 share 가능 
    - write을 위한 execlusive lock 인 경우 block

- Lock 의 종류 
  - Exclusive lock (X) 
    - read-write lock 
    - 대상이 되는 아이템을 exclusively hold  
    - read/write 모든 operation을 block  
- Lock은 transaction이 commit 될 때 release 된다



<br>

#### Deadlock

- 여러 개의 트랙잭션들이 서로 각각의 lock를 잡고 상대 트랙잭션이 잡은 lock을 잡음으로써 서로 무한정 기다리게 되는 상황 
- DBMS의 lock manager는 deadlock detect하여 dealock이 되는 transaction을 rollback한다

![image](https://user-images.githubusercontent.com/79521972/176450208-3b62ea0c-7cc2-4030-95c0-fcd73a64012c.png)



<br>

#### Two phase locking protocol

- Serializable 스케쥴을 만들기 위해 Lock를 acquire/release 하는 순서를 정하는 protocol 
- 트랜잭션에 필요한 모든 lock을 release하기 전에 모두 잡아야 한다 
  - Growth phase : lock를 acquire하는 단계 
  - Shrinking phase : lock를 release 하는 단계

![image](https://user-images.githubusercontent.com/79521972/176450355-92e09b28-313f-4487-b89d-3fc7a8064f50.png)



<br>

### MVCC

#### MVCC (Multi-version concurency control)

- Locking에서 오는 성능 문제 개선 
- 많은 DBMS에서 현재 사용하는 동시성 제어 기법 
- 데이터 업데이트가 있을 때 이전의 데이터를 덮어 씌우는게 아니라 새로운 버전의 데이터를 버전 space 에 저장하고 link 생성 
- 데이터를 조회할 때 트랜잭션의 시작 시점에 맞는 데이터 버전을 반환 
  - Version filtering w/ timestamp = Commit Sequence Number(CSN) 
  - Read operation은 lock이 필요 없음 
  - lock protocol를 사용하는 DBMS보다 좋은 성능을 보여줌 
- 버전에 대한 garbage collection이 필요



<br>

#### MVCC 동작 예제

![image](https://user-images.githubusercontent.com/79521972/176450618-b3acd996-467e-49d7-a3dc-e8272b6816ea.png)



