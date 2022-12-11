---
layout: single
title: "[OS] Deadlocks"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---

[toc]



# Chapter 8: Deadlocks

- System Model 
- Deadlock Characterization 
- Methods for Handling Deadlocks 
- Deadlock Prevention 
- Deadlock Avoidance 
- Deadlock Detection  
- Recovery from Deadlock  
- Combined Approach to Deadlock Handling



<br>

## Chapter Objectives

- Illustrate how deadlock can occur when **mutex locks** are used 
- Define the **four necessary conditions** that characterize deadlock 
- Identify a deadlock situation in a **resource allocation graph** 
- Evaluate the four different approaches for **preventing deadlocks** 
- Apply the banker’s algorithm for **deadlock avoidance** 
- Apply the **deadlock detection** algorithm 
- Evaluate approaches for recovering from deadlock



<br>

## Bridge Crossing Example

![image-20221002222310399](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002222310399.png)



- Traffic only in one direction. 
- Each section of a bridge can be viewed as a resource.  
  - 다리 - resource

- If a deadlock occurs, it can be resolved if one car backs up  (preempt resources and rollback). 
- Several cars may have to be backed up if a deadlock occurs. 
- Starvation is possible.



<br>

## System Model

- System consists of resources 

- Resource types R1 , R2 , . . ., Rm 

  CPU cycles, memory space, I/O devices 

- Each resource type Ri has Wi instances(CPU).

- 공유가 안되는 resource에서 문제가 됨. 

- If a resource is non-sharable, Each process utilizes a resource as follows: 

  - request resource 
  - use  
  - Release 

- OS manages resources 

  - Maintains info. regarding availability of resource, which process using resource etc.



<br>

## Deadlock with Semaphores

- A set of blocked processes each holding a resource and waiting to acquire a  resource held by another process in the set. 
  - Example  
    - System has 2 tape drives. 
    - P1 and P2 each hold one tape drive and each needs another one. 
- Data: 
  - A semaphore S1 initialized to 1 
  - A semaphore S2 initialized to 1 
- Two processes P1 and P2 
- P1:  
  - wait(s1) 
  - wait(s2) 
- P2:  
  - wait(s2) 
  - wait(s1)



<br>

## Deadlock in multithreaded application

```
pthread_mutex_t first_mutex;
pthread_mutex_t second_mutex;

pthread_mutex_init(&first_mutex, NULL);
pthread_mutex_init(&second_mutex, NULL);
```



mutex_lock은 semaphore와 다르게 block이 안됨.

<br>

## Deadlock in multithreaded application

```c
void *do_work_one(void *param){ /* thread one */
    pthread_mutex_lock(&first_mutex); /* POSIX mutex lock */
    pthread_mutex_lock(&second_mutex);
    /* do some work */
    pthread_mutex_unlock(&second_mutex);
    pthread_mutex_unlock(&first_mutex);
    pthread_exit(0);
}
void *do_work_two(void *param){ /* thread two */
    pthread_mutex_lock(&second_mutex);
    pthread_mutex_lock(&first_mutex);
    /* do some work */
    pthread_mutex_unlock(&first_mutex);
    pthread_mutex_unlock(&second_mutex);
    pthread_exit(0);
}
```

- deadlock 발생
  - 다른 상대방이 갖고 있는 mutex_lock을 가질 수 있는 경우는 없다.

<br>

## Livelock in multithreaded application

- similar to deadlock, but the threads are unable to proceed **for different reasons** 
- **Deadlock** occurs when every thread in a set is blocked waiting for  an event that can be caused only by another thread in the set 
  - event를 발생하기를 기다리는데 event가 실행되지 않을 때
  - Deadlocks can occur via system calls, locking, etc. 
  
- **Livelock** 
  - when threads retry failing operations at the same time  
    - 의미없는 operation을 반복하는 상태
  - **pthread_mutex_trylock() function** attempts to acquire a mutex lock without blocking 
  - Livelock can be avoided by having each retry **failing operations** at random times 
    - random 시간 만큼 delay를 주고 다시 시도
  - Ethernet collision detection 



<br>

## Livelock in multithreaded application

```c
void *do_work_one(void *param){ /* thread one */
    int done = 0; 

    while (!done) {
        pthread_mutex_lock(&first_mutex); 
        if (pthread_mutex_trylock(&second_mutex)) {
            /* do some work */
            pthread_mutex_unlock(&second_mutex);
            pthread_mutex_unlock(&first_mutex);
            done = 1;
        }
        else
            pthread_mutex_unlock(&first_mutex);
    }
    pthread_exit(0);
}

void *do_work_two(void *param){ /* thread two */
    int done = 0; 

    while (!done) {
        pthread_mutex_lock(&second_mutex); 
        if (pthread_mutex_trylock(&first_mutex)) {
            /* do some work */
            pthread_mutex_unlock(&first_mutex);
            pthread_mutex_unlock(&second_mutex);
            done = 1;
        } 
        else
            pthread_mutex_unlock(&second_mutex);
    }
    pthread_exit(0);
}
```

if (pthread_mutex_trylock(mutex)) 를 해도 이미 first와 second 모두 lock을 얻은 상태이기 때문에 false일 것이다.

그래서 else문의 pthread_mutex_unlock을 하여 다시 돌아가서 똑같이 반복한다.



livelock, deadlock, deadlock with semaphore 셋 간의 차이점

- deadlock with semaphore 
  - wait(S1)을 하면 kernel로 내려오는데 semaphore 값이 1이니까 바로 return이 됨
  - return 되면서 **semaphore가** 0->1로 바뀌고
  - 그 이후에 wait(S2)
  - 그러면 못 돌아옴(block)
- deadlock in multithreaded
  - mutex_lock
  - 첫번째 문장으로 first_mutex를 얻고 다른 함수의 첫번째 문장으로 second_mutex를 얻음
  - 근데 두 함수 어떤 것이든 두 번째 문장은 실행이 안된다. (1에서 0으로 바뀌었을테니까)
  - 커널 안에서 빙글빙글 돌고 있음 (wait 상태를 간 게 아님)
  - 다른 lock을 얻으려고 하는데 못얻고 있으니까 계속 얻으려고 시고하고 있는 거임.
- livelock
  - 두 thread가 각각 
  - pthread_mutex_trylock: 바로 user에게 lock을 얻었는지 못얻었는지의 결과를 return해줌
    - 그래서 if 조건문에 딱 걸리기 때문에 획득했는지 안했는지 딱 알 수 있음.
  - 만약 못얻고 else에 걸리면 첫번째 문장으로 획득했던 lock을 회수하고 다시 시도한다.
  - 이 과정을 반복하면 deadlock이 발생할 확률이 현저히 줄어든다.
  - sleep(rand(100)) 과 같이 random time을 기다리게 한다.



<br>

## Deadlock Characterization

Deadlock can arise if four conditions hold simultaneously.(4개의 필요조건)

4가지가 동시에 일어나야 deadlock 발생

- **Mutual exclusion**: at least one resource must be held in a non-sharable mode  
  - <u>only one process at a time can use a resource.</u> 
  - 오로지 하나의 프로세스만이 하나의 자원을 사용할 수 있다.
  - If a process wants resource held by another process, it must suspend 
  - Wait for process holding resource to release it 
  
- **Hold and wait**: a process holding at least one resource is waiting to acquire additional resources held by other processes. 
  - 공유가 불가능한 resource를 적어도 한 개이상 보유하고 있는 프로세스 and 다른 resource를 요구할 때
  - 자원을 최소 한 개를 갖고 있으면서 다른 자원을 기다리고 있는 상태
  
- **No preemption**: a resource can be released only voluntarily by the process  holding it, after that process has completed its task. 
  - preemption이 불가능한 resource가 있어야 함.
  - 프로세스는 작업을 완료한 후에만 소유하고 있던 자원을 반납할 수 있다.
  
- **Circular wait**: there exists a set {P0 , P1 , …, P0 } of waiting processes such  that P0 is waiting for a resource that is held by  P1 , P1 is waiting for a resource that is held by P2 , …, Pn–1 is waiting for a  resource that is held by Pn , and P0 is waiting for a resource that is held by  P0 .
  - circular하게 다른 프로세스의 resource를 연쇄적으로 기다리고 있는 상태이다.
  - 프로세스가 다른 프로세스의 자원을 요구하는 형태가 원형을 이루고 있는 상태




<br>

## Resource-Allocation Graph

A set of vertices V and a set of edges E.

- V is partitioned into two types: 
  - P = {P1 , P2 , …, Pn }, the set consisting of all the processes in the  system. 
  - R = {R1 , R2 , …, Rm}, the set consisting of all resource types in the  system. 
- 2 types of edges 
  - request edge – directed edge P1 -> Rj 
    - Indicates process waiting for instances of a resource type 

  - assignment edge – directed edge Rj -> Pi 
    - Indicates an instance of resource held by process



<br>

## Resource-Allocation Graph (Cont.)

- Process![image-20221002223045837](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223045837.png)

- Resource Type with 4 instances ![image-20221002223100966](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223100966.png)



- P<sub>i</sub> requests instance of R<sub>j</sub> ![image-20221002223131353](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223131353.png)



- P<sub>i</sub> is holding an instance of R<sub>j</sub> ![image-20221002223152827](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223152827.png)



<br>

## Example of a Resource Allocation Graph

- One instance of R1 
- Two instances of R2 
- One instance of R3 
- Three instance of R4 
- P1 holds one instance of R2 and is  waiting for an instance of R1 
- P2 holds one instance of R1, one  instance of R2, and is waiting for an  instance of R3
- P3 is holds one instance of R3

![image-20221002223234227](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223234227.png)

- 2 process P1, P2 
-  resources f1, f2 
- P1: request(f1)                P2: request(f2) 
- request(f2)                       request(f1)
  - P1이 쭉 실행된 후 release를 한다면 deadlock은 발생하지 않을 것이다.
  - 그러나 P1에서 첫문장을 실행하고 P2에서 첫문장을 실행한 후에
    - 각자 그 다음 문장을 실행하려 하면 resource allocation graph에서 cycle이 형성 된다.


![image-20221002223321330](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223321330.png)

Cycle in resource allocation graph sufficient for deadlock if each resource type in cycle **consists of a single entity**

- cycle이 존재하면 deadlock을 발생시킨다는 충분조건은 resource type가 single entity를 구성하는 것
- cycle이 없으면 deadlock도 없다.
- 

<br>

## Resource Allocation Graph With A Deadlock

![image-20221002223354398](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223354398.png)

- P1이 R1을 요청하나 획득할 가능성은 없음
  - P2가 가지고 있기 때문에 request edge가 assignment edge로 바뀔 수가 없음.
- P2 역시 P3가 갖고 있는 것을 요구 하고 있기 때문에 가능성 없음
- P3도 P1이 갖고 있는 것을 기다리고 있음.
- 따라서 이는 circular wait
- 공유도 안되고, preemption도 안되고 hold and wait 조건도 만족하고
- R2는 single resource가 아니기 때문에 cycle이 있다고 무조건 deadlock은 아니다.

<br>

## Resource Allocation Graph With A Cycle But No Deadlock

![image-20221002223421800](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223421800.png)

- R1과 R2가 single resource가 아니기 때문에 필요조건은 만족했지만 충분조건은 만족하지 못했다.

- P1은 P2나 P3 둘 중 어느 것이 release하는 resource를 기다리고 있음.
- 여기서 P2와 P4는 hold만 하고 있기 때문에(not hold and wait) 자원을 놓았을 때 cycle이 깨지기 때문에 deadlock이 아니다

<br>

## Basic Facts

- If graph contains no cycles => no deadlock. 
- If graph contains a cycle => 
  - if only one instance per resource type, then **deadlock**. (충분조건)
    - 하나면 무조건 데드락
  - if several instances per resource type, possibility of deadlock. (필요조건)
    - 일수도 아닐수도

- 즉 cycle이 있는데 single resource type이면 무조건 데드락이다.

<br>

## Methods for Handling Deadlocks

- Ensure that the system will **never** enter a deadlock state. : overhead가 있음(사전에 발생가능성을 차단해야 하기 때문에) 
  - : Deadlock 현상 자체를 미연에 방지하는 방법
    - Deadlock prevention 
    - Deadlock avoidance 
  
- Allow the system to enter a deadlock state and then recover. 
  - Deadlock 상태를 허용하면서 그것을 복구하는 방법
    - Deadlock detection and recovery 

  - 사전에 아무 조치를 취하지 않고 주기적으로 체크만 함.

- Ignore the problem and pretend that deadlocks never occur in the  system; used by most operating systems, including UNIX.
  - Deadlock 문제 자체를 무시해 버리는 방법
  - deadlock과 관련해서 OS가 해줄 수 있는 일이 아무것도 없으니까 그냥 무시
    - 그냥 kill 하거나 rebooting

  - OS가 제공하지 않으면 application 내에 해결책 구현이 필요할 수도 있음.





<br>

## Deadlock Prevention

- **Invalidate one of the four necessary conditions for deadlock**: 
  - A set of methods for ensuring that one of 4 necessary conditions for  deadlock cannot hold 
- Restrain the ways request can be made. 
  - Result in low device utilization and reduced system throughput



1) Mutual Exclusion 부정
   - not required for sharable resources 
     - (e.g., read-only files) 
   - must hold for non-sharable resources.  
   - Depends on resources  
   - Not practical - 의미가 없음(실현 불가능0)
   - 애초에 자원에 동시 접근이 불가능하기 때문에 동시엥 접근을 하게 하는 것은 불가능



<br>

2. Hold and Wait 부정  - must guarantee that whenever a process requests a resource, it does not hold any other resources. 

   - 프로세스가 자원을 요청할 때는 항상 어느 자원도 소유하고 있지 않도록 해야 한다.
     - 프로세스가 실행되기 전에 필요한 자원들을 모두 요청하고 할당 받는다.

   - One shot allocation 
     - Require process to request and be allocated all its resources  before it begins execution,  
     - 한꺼번에 요구 -> 자원의 낭비가 심함(사용하지 않는 것도 미리 받아놓기 떄문)
   - or allow process to request resources only when the process has none. 
     - Release resources held before requesting more 
   - Prob. 
     - Ability to acquire resources simultaneously may be limited 
     - Low throughput : processes may be suspended waiting for jobs  holding resources they don’t need 
     - Low resource utilization :  
     - starvation possible : Not all the required resources may  become available at once

<br>



3) No Preemption 부정
   - 갖고 있는 자원을 프로세스가 스스로 반납하기 전까지는 회수 되지 않음
     - 프로세스가 하나 이상의 자원을 소유하고 있고 즉시 할당 받을 수 없는 다른 자원을 요청하는 경우 소유하고 있는 자원을 모두 반납하게 하는 것
   - 절대로 preemption을 못하게 되면 deadlock 확률이 증가하기 때문에 preemption을 가능하게 하면 deadlock 확률이 감소한다.
   - If a process that is holding some resources requests another  resource that cannot be immediately allocated to it, then all resources currently being held are released 
   - Take away resources from a process suspended on requests 
     - (If  a process that is holding some resources requests another  resource that cannot be immediately allocated to it, then all  resources currently being held are released) 
   - Preempted resources are added to the list of resources for  which the process is waiting. 
   - Process will be restarted only when it can regain its old  resources, as well as the new ones that it is requesting. 
   - Starvation



4) Circular Wait  
   - 자원들의 total ordering을 반환하는 F() 함수를 사용
   - Ensure that there is no cycle of suspended processes 
   - impose a total ordering of all resource types,  
     - Disk(3), tape(5) ,,,, 
     - 낮은 번호의 resource request는 거부
   - require that each process requests resources in an increasing order of  enumeration. 
     - Processes can only request a resource whose associated value is  greater than value of any resources it holds 
   - P1은 R1을 요구하기 위해서 R2를 release하고 해야 함!
   - If want resource less than hold 
     - Release resources > new request 
     - Make new request in ascending order 
   - Circular wait (P1 has R2, wait for R1 ; P2 has R1, wait for R2) 
     - Impossible situation

![image-20221002223908168](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223908168.png)





<br>

## Circular Wait (Deadlock Example)

- Invalidating the circular wait  condition is most common. 
- Simply assign each resource  (i.e., mutex locks) a unique  number. 
- Resources must be acquired in  order. 
- If 
  first_mutex = 1 
  second_mutex = 5 
- code for thread_two could not  be written:

![image-20221002223951090](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223951090.png)

<br>

## Deadlock Example with Lock Ordering

![image-20221002224013927](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224013927.png)



- Transactions 1 and 2 execute concurrently. Transaction 1 transfers $25  from account A to account B, and Transaction 2 transfers $50 from  account B to account A
- Java: System.identifyHashCode(Object) – returns the hash code value  for ordering lock acquisition

- lock 번호를 얻는데 도움이 되는 hash code

<br>

## Deadlock Avoidance

- Deadlock prevention prevent deadlocks by restraining how requests  can be made 
- Lead to low device utilization and reduced system throughput

- System will not grant request which could potentially lead to deadlock 
  - 자유 형식으로 요청을 할 수 있지만 deadlock으로 발전할 가능성이 있는 request를 거부한다.

- Requires that the system has some additional a priori information(사전 정보) available.

  - Simplest and most useful model requires that each process declare the **maximum number** of resources of each type that it may need. 

  - The deadlock-avoidance algorithm dynamically examines the resource-allocation state to ensure that there can never be a circular-wait condition. 

  - Resource-allocation state is defined by the number of available and  allocated resources, and the maximum demands of the processes.



<br>

## Safe State

- When a process requests an available resource, system must  decide if immediate allocation leaves the system in a safe state. 
- System is in safe state if there exists a safe sequence of all  processes.  
- Sequence {P1, P2, ..., Pn} is safe if for each Pi , the resources  that Pi can still request can be satisfied by currently available  resources + resources held by all the Pj , with j<I.
  - If Pi resource needs are not immediately available, then Pi can wait until all Pj have finished. 
  - When Pj is finished, Pi can obtain needed resources,  execute, return allocated resources, and terminate.  
  - When Pi terminates, Pi+1 can obtain its needed resources,  and so on.



<br>

## Basic Facts

- If a system is in safe state => no deadlocks. 
- If a system is in unsafe state => possibility of deadlock. 
- **Avoidance** => ensure that a system will never enter an unsafe  state.
  - 항상 safe 상태에 있도록




<br>

## Safe, unsafe, deadlock state spaces

![image-20221002224304739](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224304739.png)

unsafe: 현재는 deadlock이 아닌데 진전이 되면 deadlock이 걸릴 것이 농후해 보이는 상태

safe: 아예 deadlock이 걸리지 않는 상태

<br>

## Safe, unsafe state

![image-20221002224329818](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224329818.png)s

P1은 2개만 있으면 되는데 현재 available이 3이므로 P1이 전부 요구해도 들어줄 수 있지만 나머지는 들어줄 수 없는 경우가 발생할 수 있다.

<br>

![image-20221002224344691](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224344691.png)

- if P2 requests and is allocated 1 more tape drive 
- Safe sequence exist? => **no**!, so the request should not be allowed.
  - deadlock avoidance!

<br>

## Avoidance algorithms

- Single instance of a resource type 
  - Use a resource-allocation graph 
- Multiple instances of a resource type 
  - Use the banker’s algorithm



<br>

## Resource-Allocation Graph Algorithm

- If we have a resource-allocation system with only one instance of each  resource type,  
  - Request and assignment edge 
  - **Claim edge** Pi -> Rj indicated that process PI may request  resource Rj at some time in the future; represented by a dashed  line. 
  - Claim edge converts to request edge when a process requests a  resource. 
  - When a resource is released by a process, assignment edge  reconverts to a claim edge. 
  - Resources must be claimed a priori in the system. 
    - Before process Pi starts execution, all its claim edges must  already appear in resource-allocation graph 
    - Request (Pi -> Rj ) can be granted only if converting the  request edge to an assignment edge does not result in the  formation of a cycle 
  - O(n 2 ) where n is the # of processes



<br>

## Resource-Allocation Graph For Deadlock Avoidance

![image-20221002224551024](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224551024.png)

실선: assignment edge, request edge

점선: Request and assignment edge 

<br>

## Unsafe State In A Resource-Allocation Graph

![image-20221002224618692](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224618692.png)



<br>

## Resource-Allocation Graph Algorithm

- Suppose that process P<sub>i</sub> requests a resource R<sub>j</sub> 
- The request can be granted only if converting the request edge to an  assignment edge does not result in the formation of a cycle in the  resource allocation graph



<br>

## Banker's Algorithm

- Multiple instances. 
  - multiple resource type - deadlock avoidance

- Each process must a priori claim maximum use. 
- When a process requests a resource it may have to wait.  
- When a process gets all its resources it must return them in a  finite amount of time. 
- O(m x n<sup>2</sup>)
  - n: 프로세스 갯수
  - m: message type의 갯수



<br>

## Data Structures for the Banker's Algorithm

Let n = number of processes, and m = number of resources types

- **Available**: Vector of length m. If available [j] = k, there are k instances of resource type Rj available. 

- **Max**: n x m matrix. If Max [i,j] = k, then process Pi may request  at most k instances of resource type Rj . 

- **Allocation**: n x m matrix. If Allocation[i,j] = k then Pi is currently  allocated k instances of Rj. 

- **Need**: n x m matrix. If Need[i,j] = k, then Pi may need k more  instances of Rj to complete its task. 

  Need [i,j] = Max[i,j] – Allocation [i,j].



<br>

## Safety Algorithm

1. Let Work and Finish be vectors of length m and n, respectively.  
   Initialize: Work := Available and Finish [i] = false for i = 1,2, …, n. 

2. Find an i such that both:  

   (a) Finish [i] = false 

   (b) Needi ≤ Work 

   If no such i exists, go to step 4. 

3. Work := Work + Allocation<sub>i</sub> 
   Finish[i] := true 
   go to step 2. 

4. If Finish [i] = true for all i, then the system is in a safe state









<br>

## Resource-Request Algorithm for Process Pi

Request = request vector for process Pi . If Request<sub>i</sub> [j] = k then  process P<sub>i</sub> wants k instances of resource type R<sub>j</sub> 

1. If Request<sub>i</sub> ≤ Need<sub>i</sub> go to step Otherwise, raise error condition,  since process has exceeded its maximum claim 

2. If Request<sub>i</sub> ≤ Available, go to step 3. Otherwise Pi must wait,  since resources are not available 

3. Pretend to allocate requested resources to Pi by modifying the  state as follows: 

   ​				Available = Available – Request; 

   ​				Allocation<sub>i</sub> = Allocation<sub>i</sub> + Request<sub>i</sub> ; 

   ​				Need<sub>i</sub> = Need<sub>i</sub> – Request<sub>i</sub> ; 

   - If safe => the resources are allocated to P<sub>i</sub> 
   - If unsafe => P<sub>i</sub> must wait, and the old resource-allocation state  is restored



<br>

## Example of Banker's Algorithm - 시험

- 5 processes P<sub>0</sub> through P<sub>4</sub> ;  
- 3 resource types  
  - A (10 instances), B (5instances), and C (7 instances). 
- Snapshot at time T<sub>0</sub> :

![image-20221002225224194](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225224194.png)

Need: 앞으로 더 요청할 가능성이 있는 숫자

<br>

## Example (Cont.)

Is this safe?

- The content of the matrix. Need is defined to be Max – Allocation.

![image-20221002225317137](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225317137.png)

- The system is in a safe state since the sequence < P<sub>1</sub> , P<sub>3</sub> , P<sub>4</sub> , P<sub>2</sub> , P<sub>0</sub>>  satisfies safety criteria.  
  - (3,3,2) -> P<sub>1</sub> (5,3,2) -> P<sub>3</sub> (7,4,3) -> P<sub>4</sub> (7,4,5) -> P<sub>2</sub> (10,4,7) -> P<sub>0</sub> (10,5,7)



<br>

## Example (Cont.): P1 request (1,0,2) - 시험

- Check that Request by P1 ≤ Available (that is, (1,0,2) ≤ (3,3,2) => true.

![image-20221002225801105](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225801105.png)

- Executing safety algorithm shows that sequence < P<sub>1</sub> , P<sub>3</sub> , P<sub>4</sub> , P<sub>0</sub> , P<sub>2</sub>> satisfies safety requirement.  
- <mark>중요</mark>
- Can request for (3,3,0) by P<sub>4</sub> be granted? 
- Can request for (0,2,0) by P<sub>0</sub> be granted?
- 위 예제에 대해서 safe sequence를 찾을 수 있으면 해당 request는 받아들여지는 것이고 찾을 수 없다면 받아들일 수 없는 것이다.

초기에 2,3,0의 용량에 벗어나지 않는 Need를 가진 것은 P1밖에 없다.

<br>

## Deadlock Detection

- Allow system to enter deadlock state  
- Detection algorithm 
- Recovery scheme



<br>

## Single Instance of Each Resource Type

- Maintain **wait-for** graph 
- Nodes are processes. – Pi -> Pj if Pi is waiting for Pj . 
- Periodically invoke an algorithm that searches for acycle in the graph. 
- An algorithm to detect a cycle in a graph requires an order of n<sup>2</sup> operations, where n is the number of vertices in the graph.
  - O(n^2)
  - x: n
  - y: O(n^2)




<br>

## Resource-Allocation Graph And Wait-for Graph

![image-20221002230020650](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230020650.png)

resource를 제거하더라도 사실상 프로세스 끼리 요구하는 것이기 때문에 의미 전달은 된다.

<br>

## Several Instances of a Resource Type

- **Available**: A vector of length m indicates the number of available  resources of each type 
- **Allocation**: An n x m matrix defines the number of resources of each  type currently allocated to each process 
- **Request**: An n x m matrix indicates the current request of each  process. If Request [i][j] = k, then process Pi is requesting k more  instances of resource type Rj .



<br>

## Detection Algorithm

1. Let Work and Finish be vectors of length m and n, respectively  Initialize: 

   (a) Work = Available

   (b) For i = 1,2, …, n, if Allocation<sub>i</sub> ≠ 0, then  
   		Finish[i] = false; otherwise, Finish[i] = true 

2. Find an index i such that both: 
   (a) Finish[i] == false 
   (b) Requesti ≤ Work 

   If no such i exists, go to step 4

3. Work = Work + Allocation<sub>i</sub> 
   Finish[i] = true 
   go to step 2 
4. If Finish[i] == false, for some i, 1 ≤ i ≤ n, then the system is in deadlock state. Moreover, if Finish[i] == false, then Pi is deadlocked



Algorithm requires an order of O(m x n<sup>2</sup>) operations to detect  whether the system is in deadlocked state

  ![그림입니다. 원본 그림의 이름: CLP0000b6980004.bmp 원본 그림의 크기: 가로 909pixel, 세로 475pixel](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/tmp45F0.jpg)  

<br>

## Example of Detection Algorithm - 시험

- Five processes P<sub>0</sub> through P<sub>4</sub> ; three resource types  A (7 instances), B (2 instances), and C (6 instances). 
- Snapshot at time T<sub>0</sub> :

![image-20221002230446061](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230446061.png)

- Request: 현재 요청한 resource 갯수
- Need: 앞으로 요청할 수 있는 최대 갯수(이 정도는 반드시 필요하다.)
- Sequence  < P<sub>0</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>1</sub> , P<sub>4</sub>> will result in Finish[i] = true for all i.  
  - sequence가 존재하므로 이 상황은 deadlock이 발생하지 않은 상황임!

- banker’s alg:   <u>Allocation</u>  <u>Max</u>  <u>Available</u>  <u>Need</u>



<br>

---

## detection과 avoidance의 차이점을 명확히 알기





---

## Example (Cont.)

- 근데 P<sub>2</sub> requests an additional instance of type C.

![image-20221002230736013](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230736013.png)

- State of system? 
  - Can reclaim resources held by process P<sub>0 </sub>, but insufficient  resources to fulfill other processes; requests. 
  - **Deadlock exists**, consisting of processes P<sub>1</sub> , P<sub>2</sub> , P<sub>3</sub> , and P<sub>4</sub>

 

<br>

## Detection-Algorithm Usage

- When, and how often, to invoke depends on: 
  - How often a deadlock is likely to occur? 
  - How many processes will need to be rolled back? (초반 다리 얘기)
    - deadlock 이전 상태로 돌아가기 위해서 얼마 전으로 돌아가야 하는가?
    - one for each disjoint cycle 
- If detection algorithm is invoked arbitrarily, there may be many cycles in the resource graph and so we would not be able to tell which of the many deadlocked processes “caused” the deadlock.
  - 최초로 deadlock을 유발한 process를 찾기 어려워짐




<br>

## Recovery from Deadlock: Process Termination

- Abort **all** deadlocked processes. 
- Abort one process at a time until the deadlock cycle is eliminated. 
  - 한개씩 abort 시켜보는 방식

- In which order should we choose to abort? 
  - Priority of the process. 
  - How long process has computed, and how much longer to completion. 
  - Resources the process has used. 
  - Resources process needs to complete. 
  - How many processes will need to be terminated.  
  - Is process interactive or batch?



<br>

## Recovery from Deadlock: Resource Preemption

- Selecting a victim – minimize cost. 
- Rollback – return to some safe state, restart process fro that  state. 
- Starvation – same process may always be picked as victim,  include number of rollback in cost factor.



<br>

## Combined Approach to Deadlock Handling

- **Combine** the three basic approaches 

  - **prevention** 
  - **avoidance** 
  - **detection** 

  allowing the use of the optimal approach for each of resources in  the system. 

- Partition resources into hierarchically ordered classes. 

- Use most appropriate technique for handling deadlocks within  each class.

