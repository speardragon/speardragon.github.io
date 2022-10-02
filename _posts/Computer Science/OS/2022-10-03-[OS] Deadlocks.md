---
layout: single
title: "[OS] Deadlocks"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---

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

- Illustrate how deadlock can occur when mutex locks are used 
- Define the four necessary conditions that characterize deadlock 
- Identify a deadlock situation in a resource allocation graph 
- Evaluate the four different approaches for preventing deadlocks 
- Apply the banker’s algorithm for deadlock avoidance 
- Apply the deadlock detection algorithm 
- Evaluate approaches for recovering from deadlock



<br>

## Bridge Crossing Example

![image-20221002222310399](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002222310399.png)



- Traffic only in one direction. 
- Each section of a bridge can be viewed as a resource.  
- If a deadlock occurs, it can be resolved if one car backs up  (preempt resources and rollback). 
- Several cars may have to be backed up if a deadlock occurs. 
- Starvation is possible.



<br>

## System Model

- System consists of resources 

- Resource types R1 , R2 , . . ., Rm 

  CPU cycles, memory space, I/O devices 

- Each resource type Ri has Wi instances. 

- If a resource is non-sharable, Each process utilizes a resource as follows: 

  - request resource 
  - use  
  - Release 

- OS manages resources 

  - Maintains info. regarding availability of resource, which process  using resource etc.



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



<br>

## Deadlock in multithreaded application

```c
void *do_work_one(void *param){ /* thread one */
    pthread_mutex_lock(&first_mutex); /* POSIX mutex lock */
    pthread_mutex_lock(&second_mutex);
    /* do some work */
    pthread_mutex_unlock(&second_mutex);
    pthread_mutex_unlock(&first_mutex);
    pthread_exit(0)l
}
void *do_work_two(void *param){ /* thread two */
    pthread_mutex_lock(&second_mutex);
    pthread_mutex_lock(&first_mutex);
    /* do some work */
    pthread_mutex_unlock(&first_mutex);
    pthread_mutex_unlock(&second_mutex);
    pthread_exit(0)l
}
```



<br>

## Livelock in multithreaded application

- similar to deadlock, but the threads are unable to proceed for  different reasons 
- Deadlock occurs when every thread in a set is blocked waiting for  an event that can be caused only by another thread in the set 
  - Deadlocks can occur via system calls, locking, etc. 
- Livelock 
  - when threads retry failing operations at the same time  
  - pthread_mutex_trylock() function attempts to acquire a  mutex lock without blocking 
  - Livelock can be avoided by having each retry failing  operations at random times 
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
    pthread_exit(0)l
}

void *do_work_two(void *param){ /* thread one */
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
            pthread_mutex_unlock(&seconf_mutex);
    }
    pthread_exit(0)l
}
```



<br>

## Deadlock Characterization

Deadlock can arise if four conditions hold simultaneously.

- Mutual exclusion: at least one resource must be held in a non-sharable  mode  
- only one process at a time can use a resource. 
- If a process wants resource held by another process, it must suspend 
- Wait for process holding resource to release it 
- **Hold and wait**: a process holding at least one resource is waiting to  acquire additional resources held by other processes. 
- **No preemption**: a resource can be released only voluntarily by the process  holding it, after that process has completed its task. 
- **Circular wait**: there exists a set {P0 , P1 , …, P0 } of waiting processes such  that P0 is waiting for a resource that is held by  P1 , P1 is waiting for a resource that is held by P2 , …, Pn–1 is waiting for a  resource that is held by Pn , and P0 is waiting for a resource that is held by  P0 .



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

![image-20221002223321330](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223321330.png)

Cycle in resource allocation graph sufficient for deadlock if each  resource type in cycle consists of a single entity



<br>

## Resource Allocation Graph With A Deadlock

![image-20221002223354398](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223354398.png)

<br>

## Resource Allocation Graph With A Cycle But No Deadlock

![image-20221002223421800](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223421800.png)



<br>

## Basic Facts

- If graph contains no cycles => no deadlock. 
- If graph contains a cycle => 
  - if only one instance per resource type, then deadlock. 
  - if several instances per resource type, possibility of  deadlock.



<br>

## Methods for Handling Deadlocks

- Ensure that the system will never enter a deadlock state. 
  - Deadlock prevention 
  - Deadlock avoidance 
- Allow the system to enter a deadlock state and then recover. 
  - Deadlock detection and recovery 
- Ignore the problem and pretend that deadlocks never occur in the  system; used by most operating systems, including UNIX.



<br>

## Deadlock Prevention

- Invalidate one of the four necessary conditions for deadlock: 
  - A set of methods for ensuring that one of 4 necessary conditions for  deadlock cannot hold 
- Restrain the ways request can be made. 
  - Result in low device utilization and reduced system throughput



1) Mutual Exclusion  
   - not required for sharable resources 
     - (e.g., read-only files) 
   - must hold for non-sharable resources.  
   - Depends on resources  
   - Not practical



<br>

2) Hold and Wait - must guarantee that whenever a process requests a  resource, it does not hold any other resources. 
   - One shot allocation 
     - Require process to request and be allocated all its resources  before it begins execution,  
   - or allow process to request resources only when the process has  none. 
     - Release resources held before requesting more 
   - Prob. 
     - Ability to acquire resources simultaneously may be limited 
     - Low throughput : processes may be suspended waiting for jobs  holding resources they don’t need 
     - Low resource utilization :  
     - starvation possible : Not all the required resources may  become available at once

<br>



3) No Preemption
   - If a process that is holding some resources requests another  resource that cannot be immediately allocated to it, then all  resources currently being held are released 
   - Take away resources from a process suspended on requests (If  a process that is holding some resources requests another  resource that cannot be immediately allocated to it, then all  resources currently being held are released) 
   - Preempted resources are added to the list of resources for  which the process is waiting. 
   - Process will be restarted only when it can regain its old  resources, as well as the new ones that it is requesting. 
   - Starvation



4) Circular Wait  
   - Ensure that there is no cycle of suspended processes 
   - impose a total ordering of all resource types,  
     - Disk(3), tape(5) ,,,, 
   - require that each process requests resources in an increasing order of  enumeration. 
     - Processes can only request a resource whose associated value is  greater than value of any resources it holds 
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
- If first_mutex = 1 second_mutex = 5 code for thread_two could not  be written:

![image-20221002223951090](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002223951090.png)

<br>

## Deadlock Example with Lock Ordering

![image-20221002224013927](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224013927.png)



- Transactions 1 and 2 execute concurrently. Transaction 1 transfers $25  from account A to account B, and Transaction 2 transfers $50 from  account B to account A
- Java: System.identifyHashCode(Object) – returns the hash code value  for ordering lock acquisition



<br>

## Deadlock Avoidance

- Deadlock prevention prevent deadlocks by restraining how requests  can be made 
- Lead to low device utilization and reduced system throughput

- System will not grant request which could potentially lead to deadlock 

- Requires that the system has some additional a priori information  available.

  - Simplest and most useful model requires that each process declare  the maximum number of resources of each type that it may need. 

  - The deadlock-avoidance algorithm dynamically examines the  resource-allocation state to ensure that there can never be a  circular-wait condition. 

  - Resource-allocation state is defined by the number of available and  allocated resources, and the maximum demands of the processes.



<br>

## Safe State

- When a process requests an available resource, system must  decide if immediate allocation leaves the system in a safe state. 
- System is in safe state if there exists a safe sequence of all  processes.  
- Sequence  is safe if for each Pi , the resources  that Pi can still request can be satisfied by currently available  resources + resources held by all the Pj , with j<I.
  - If Pi resource needs are not immediately available, then Pi can wait until all Pj have finished. 
  - When Pj is finished, Pi can obtain needed resources,  execute, return allocated resources, and terminate.  
  - When Pi terminates, Pi+1 can obtain its needed resources,  and so on.



<br>

## Basic Facts

- If a system is in safe state => no deadlocks. 
- If a system is in unsafe state => possibility of deadlock. 
- Avoidance => ensure that a system will never enter an unsafe  state.



<br>

## Safe, unsafe, deadlock state spaces

![image-20221002224304739](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224304739.png)



<br>

## Safe, unsafe state

![image-20221002224329818](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224329818.png)



<br>

![image-20221002224344691](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224344691.png)

- if P2 requests and is allocated 1 more tape drive 
- Safe sequence exist? => no!, so the request should not be allowed.



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
  - Claim edge Pi -> Rj indicated that process PI may request  resource Rj at some time in the future; represented by a dashed  line. 
  - Claim edge converts to request edge when a process requests a  resource. 
  - When a resource is released by a process, assignment edge  reconverts to a claim edge. 
  - Resources must be claimed a priori in the system. 
    - Before process Pi starts execution, all its claim edges must  already appear in resource-allocation graph 
    - Request (Pi -> Rj ) can be granted only if converting the  request edge to an assignment edge does not result in the  formation of a cycle 
  - O(n 2 ) where n is the # of processes



<br>

## Resource-Allocation Graph For Deadlock Avoidance

![image-20221002224551024](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002224551024.png)



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
- Each process must a priori claim maximum use. 
- When a process requests a resource it may have to wait.  
- When a process gets all its resources it must return them in a  finite amount of time. 
- O(m x n2 )



<br>

## Data Structures for the Banker's Algorithm

Let n = number of processes, and m = number of resources types

- Available: Vector of length m. If available [j] = k, there are k instances of resource type Rj available. 

- Max: n x m matrix. If Max [i,j] = k, then process Pi may request  at most k instances of resource type Rj . 

- Allocation: n x m matrix. If Allocation[i,j] = k then Pi is currently  allocated k instances of Rj. 

- Need: n x m matrix. If Need[i,j] = k, then Pi may need k more  instances of Rj to complete its task. 

  Need [i,j] = Max[i,j] – Allocation [i,j].



<br>

## Safety Algorithm

1. Let Work and Finish be vectors of length m and n, respectively.  Initialize: 

   Work := Available 

   Finish [i] = false for i = 1,2, …, n. 

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

## Example of Banker's Algorithm

- 5 processes P<sub>0</sub> through P<sub>4</sub> ;  
- 3 resource types  
  - A (10 instances), B (5instances), and C (7 instances). 
- Snapshot at time T<sub>0</sub> :

![image-20221002225224194](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225224194.png)



<br>

## Example (Cont.)

- The content of the matrix. Need is defined to be Max – Allocation.

![image-20221002225317137](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225317137.png)

- The system is in a safe state since the sequence < P<sub>1</sub> , P<sub>3</sub> , P<sub>4</sub> , P<sub>2</sub> , P<sub>0</sub>>  satisfies safety criteria.  
  - (3,3,2) -> P<sub>1</sub> (5,3,2) -> P<sub>3</sub> (7,4,3) -> P<sub>4</sub> (7,4,5) -> P<sub>2</sub> (10,4,7) -> P<sub>0</sub> (10,5,7)



<br>

## Example (Cont.): P1 request (1,0,2)

- Check that Request by P1 ≤ Available (that is, (1,0,2) ≤ (3,3,2) ≤ true.

![image-20221002225801105](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002225801105.png)

- Executing safety algorithm shows that sequence < P<sub>1</sub> , P<sub>3</sub> , P<sub>4</sub> , P<sub>0</sub> , P<sub>2</sub>> satisfies safety requirement.  
- Can request for (3,3,0) by P<sub>4</sub> be granted? 
- Can request for (0,2,0) by P<sub>0</sub> be granted?



<br>

## Deadlock Detection

- Allow system to enter deadlock state  
- Detection algorithm 
- Recovery scheme



<br>

## Single Instance of Each Resource Type

- Maintain wait-for graph 
- Nodes are processes. – Pi -> Pj if Pi is waiting for Pj . 
- Periodically invoke an algorithm that searches for acycle in the  graph. 
- An algorithm to detect a cycle in a graph requires an order of n 2 operations, where n is the number of vertices in the graph.



<br>

## Resource-Allocation Graph And Wait-for Graph

![image-20221002230020650](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230020650.png)



<br>

## Several Instances of a Resource Type

- **Available**: A vector of length m indicates the number of available  resources of each type 
- **Allocation**: An n x m matrix defines the number of resources of each  type currently allocated to each process 
- **Request**: An n x m matrix indicates the current request of each  process. If Request [i][j] = k, then process Pi is requesting k more  instances of resource type Rj .



<br>

## Detection Algorithm

1. Let Work and Finish be vectors of length m and n, respectively  Initialize: 

   (a) Work = Available

   (b) For i = 1,2, …, n, if Allocationi ≠ 0, then  
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



<br>

## Example of Detection Algorithm

- Five processes P<sub>0</sub> through P<sub>4</sub> ; three resource types  A (7 instances), B (2 instances), and C (6 instances). 
- Snapshot at time T<sub>0</sub> :

![image-20221002230446061](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230446061.png)

- Sequence  < P<sub>0</sub> , P<sub>2</sub> , P<sub>3</sub> , P<sub>1</sub> , P<sub>4</sub>> will result in Finish[i] = true for all i.  
- banker’s alg: Allocation Max Available Need



<br>

## Example (Cont.)

- P<sub>2</sub> requests an additional instance of type C.

![image-20221002230736013](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002230736013.png)

- State of system? 
  - Can reclaim resources held by process P<sub>0 </sub>, but insufficient  resources to fulfill other processes; requests. 
  - Deadlock exists, consisting of processes P<sub>1</sub> , P<sub>2</sub> , P<sub>3</sub> , and P<sub>4</sub>



<br>

## Detection-Algorithm Usage

- When, and how often, to invoke depends on: 
  - How often a deadlock is likely to occur? 
  - How many processes will need to be rolled back? 
    - one for each disjoint cycle 
- If detection algorithm is invoked arbitrarily, there may be many  cycles in the resource graph and so we would not be able to tell  which of the many deadlocked processes “caused” the deadlock.



<br>

## Recovery from Deadlock: Process Termination

- Abort all deadlocked processes. 
- Abort one process at a time until the deadlock cycle is eliminated. 
- In which order should we choose to abort? 
  - Priority of the process. 
  - How long process has computed, and how much longer to  completion. 
  - Resources the process has used. 
  - Resources process needs to complete. 
  - How many processes will need to be terminated.  
  - Isprocess interactive or batch?



<br>

## Recovery from Deadlock: Resource Preemption

- Selecting a victim – minimize cost. 
- Rollback – return to some safe state, restart process fro that  state. 
- Starvation – same process may always be picked as victim,  include number of rollback in cost factor.



<br>

## Combined Approach to Deadlock Handling

- Combine the three basic approaches 

  - prevention 
  - avoidance 
  - detection 

  allowing the use of the optimal approach for each of resources in  the system. 

- Partition resources into hierarchically ordered classes. 

- Use most appropriate technique for handling deadlocks within  each class.

