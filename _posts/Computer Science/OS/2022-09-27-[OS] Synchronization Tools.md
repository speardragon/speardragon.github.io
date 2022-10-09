---
layout: single
title: "[OS] Synchronization Tools"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---

# Chapter 6: Synchronization Tools

- Background 
- The Critical-Section Problem 
- Peterson’s Solution 
- Synchronization Hardware 
- Mutex Locks 
- Semaphores 
- Classical Problems of Synchronization 
- Monitors



<br>

## Objectives

- To present the concept of process synchronization.
- To introduce the critical-section problem, whose solutions can be used to ensure the consistency of shared data
- To present both software and hardware solutions of the critical-section problem
- To examine several classical process-synchronization problems
- To explore several tools that are used to solve process synchronization problems



<br>

## Background

- Processes can execute concurrently or in parallel 
  - CPU scheduler switches between processes to provide concurrent  execution 
  - May be interrupted at any time, partially completing execution at any  point in its instruction stream 
    - Processing core may be assigned to execute instructions of another process (concurrent execution) 
  - Two instruction streams (different processes) execute  simultaneously on separating processing cores (parallel execution) 
- Concurrent access to shared data may result in data inconsistency 
- Maintaining data consistency requires mechanisms to ensure the orderly  execution of cooperating processes



<br>

## Cooperating Processes

- Independent process cannot affect or be affected by the execution of  another process. 
- Cooperating process can affect or be affected by the execution of another  process 
  - Directly share a logical address space (code and data) 
  - Share data through shared memory or message passing  
- Advantages of process cooperation 
  - Information sharing  
  - Computation speed-up 
  - Modularity 
  - Convenience



<br>

## Producer-Consumer Problem

- Illustration of the problem: 
  - Suppose that we wanted to provide a solution to the consumer-producer problem that fills all the buffers.  
  - We can do so by having an integer counter that keeps track of the  number of full buffers.  
  - Initially, counter is set to 0. It is incremented by the producer after it  produces a new buffer and is decremented by the consumer after it  consumes a buffer. 
- Paradigm for cooperating processes, producer process produces  information that is consumed by a consumer process. 
  - unbounded-buffer places no practical limit on the size of the buffer. 
  - bounded-buffer assumes that there is a fixed buffer size.



<br>

## Ch 3 Bounded-Buffer Shared-Memory Solution

- Shared data

  - ```c
    #define BUFFER_SIZE 10
    typedef struct {
        . . .
    } item;
    item buffer[BUFFER_SIZE];
    int in = 0;
    int out = 0;
    ```

- if in=out, buffer is empty 

- if (in+1) mod n = out, buffer is full 

- Solution is correct, but can only use BUFFER_SIZE-1 elements



<br>

## Bounded-Buffer - Producer

```c
while (true) { 
    /* produce an item in next produced */ 
    while (((in + 1) % BUFFER_SIZE) == out) 
        ; /* do nothing */ 
    buffer[in] = next_produced; 
    in = (in + 1) % BUFFER_SIZE; 
}
```



<br>

## Bounded Buffer - Consumer

```c
item next_consumed; 
while (true) {
    while (in == out) 
        ; /* do nothing */
    next_consumed = buffer[out]; 
    out = (out + 1) % BUFFER_SIZE;
    /* consume the item in next consumed */ 
} 

```



<br>

## Producer (counter)

```c
while (true) {
    /* produce an item in next produced */ 
    
    while (counter == BUFFER_SIZE) ; 
    	/* do nothing */ 
    buffer[in] = next_produced; 
    in = (in + 1) % BUFFER_SIZE; 
    counter++; 
} 
```



<br>

## Consumer (counter)

```c
while (true) {
    while (counter == 0) 
        ; /* do nothing */ 
    next_consumed = buffer[out]; 
    out = (out + 1) % BUFFER_SIZE; 
    counter--; 
    /* consume the item in next consumed */ 
} 
```



<br>

## Race Condition

- counter++ could be implemented in machine language as: 

  register1 = counter 

  register1 = register1 + 1 

  counter = register1 

- counter-- could be implemented in machine language as:

  register2 = counter 

  register2 = register2 - 1 

  counter = register2



<br>

## Race Condition

- If both the producer and consumer attempt to update the buffer  concurrently, the assembly language statements may get interleaved. 

- Interleaving depends upon how the producer and consumer processes  are scheduled. 

- Assume **counter** is initially 5. One interleaving of statements is: 

  producer: register1 = counter (register1 = 5) 
  producer: register1 = register1 + 1 (register1 = 6) 
  consumer: register2 = counter (register2 = 5) 
  consumer: register2 = register2 – 1 (register2 = 4) 
  producer: counter = register1 (counter = 6) 
  consumer: counter = register2 (counter = 4) 

- The value of count may be either 4 or 6, where the correct result  should be 5.



<br>

- More than one process accessing a shared variable 

  - Timing factors (interrupts) can cause incorrect results 
    - Intermediate results are used 

- The statements: 

  - counter := counter + 1; 

  - counter := counter - 1; 

    must be executed atomically. 

- Atomic operation means an operation that completes in its entirety  without interruption. 

- **Race condition**: The situation where several processes access – and  manipulate shared data concurrently. The final value of the shared data  depends upon which process finishes last. 

- To prevent race conditions, concurrent processes must be  **synchronized**.



<br>

## Critical Section Problem

- Consider system of n processes {p0 , p1 , … pn-1 } 
- n processes all competing to use some shared data 
- Each process has critical section segment of code 
  - Process may be changing common variables, updating table, writing file,  etc 
  - When one process in critical section, no other may be in its critical section 
- Critical section problem is to design protocol to solve this 
  - ensure that when one process is executing in its critical section, no other  process is allowed to execute in its critical section.
- Each process must ask permission to enter critical section in **entry section**,  may follow critical section with **exit section**, then **remainder section** 
- Especially challenging with preemptive kernels



<br>

## Critical Section

- Structure of process P<sub>i</sub>

![image-20221002203915957](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002203915957.png)





<br>

## Solution to Critical-Section Problem

1. Mutual Exclusion. If process Pi is executing in its critical section, then no  other processes can be executing in their critical sections. 

2. Progress.  

   1. A process outside of its CS can not block another process from entering  its own CS  
   2. If no process is executing in its critical section and there exist some  processes that wish to enter their critical section, then the selection of the  processes that will enter the critical section next cannot be postponed  indefinitely. 

3. **Bounded Waiting**. A bound must exist on the number of times that other  processes are allowed to enter their critical sections after a process has made  a request to enter its critical section and before that request is granted. 

   - Assume that each process executes at a nonzero speed

   - No assumption concerning relative speed of the n processes.



<br>

## Critical-Section Handling in OS

- Two approaches depending on if kernel is preemptive or non-preemptive  
  - Preemptive – allows preemption of process when running in  kernel mode 
  - Non-preemptive – runs until exits kernel mode, blocks, or  voluntarily yields CPU 
    - Essentially free of race conditions in kernel mode



<br>

## Algorithm for Process Pi

![image-20221002204108801](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002204108801.png)



<br>

## Initial Attempt 1 to Solve Problem

- Only 2 processes, Pi and Pj 
- turn - i => Pi can enter its critical section 
- initially turn = i

![image-20221002204145030](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002204145030.png)



- Satisfies mutual exclusion, but not progress (strict alternation)



<br>

## Initial Attempt 2 to Solve Problem

- initially flag [i] = flag [j] = false. • flag [i] = true 
- Pi ready to enter its critical section

![image-20221002204223532](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002204223532.png)

- Satisfies mutual exclusion, but not progress requirement. 
- If switch 2 statements in entry section, then no mutual exclusion



<br>

1) MTX requirement is met

2) Progress Requirement is not met 

   T0: Pi sets flag[i] = true

   T1: Pj sets flag[j] = true 

   both processes are looping forever 

3) Bounded-waiting Requirement ?



<br>

## Peterson's Solution

- Good algorithmic description of solving the problem 
- Two process solution 
- Assume that the LOAD and STORE machine-language instructions  are atomic; that is, cannot be interrupted 
- The two processes share two variables: 
  - int turn;  
  - Boolean flag[2] 
  - initially flag [0] = flag [1] = false 
- The variable turn indicates whose turn it is to enter the critical  section 
- The flag array is used to indicate if a process is ready to enter the  critical section. flag[i] = true implies that process Pi is ready ready enter its critical section!



<br>

## Algorithm for Process Pi

![image-20221002204443244](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002204443244.png)

- Combined shared variables of initial attempts 1 and 2. 
- Meets all three requirements; solves the critical-section problem for two  processes. 
- Problem: does not generalize well 
  - Very difficult to expand to more than 2 processes



<br>

## Peterson's Solution (Cont.)

- to enter the CS, process Pi first sets flag[i] to be true and then sets turn to  value j , thereby asserting that if the other process wishes to enter CS it  can do so. 

- Provable that the three CS requirement are met: 

  1. Mutual exclusion is preserved 
     Pi enters CS only if: 
     either `flag[j] = false or turn = i `

  2. Progress requirement is satisfied 

  3. Bounded-waiting requirement is met 

     Pi will enter CS (progress) after at most one entry by Pj



<br>

## Multiple-Process Solution: Bakery Algorithm

Critical section for n processes

- Before entering its critical section, process receives a number.  Holder of the smallest number enters the critical section. 
- If processes Pi and Pj receive the same number, if i < j, then Pi is  served first; else Pj is served next . 
- The numbering scheme always generates numbers in increasing  order of enumeration; i.e., 1,2,3,3,3,3,4,5...



<br>

## Bakery Algorithm (Cont.)

- Notation <= lexicographical order (ticket #, process id #) 

  - (a,b) < (c,d) if a < c or if a = c and b < d 
  - max (a<sub>0</sub> ,…, a<sub>n-1</sub> ) is a number, k, such that k >= a<sub>i</sub> for i - 0,…, n – 1 

- Shared data 

  **var** choosing: **array** [0..n – 1] **of** boolean; 
  		number: **array** [0..n – 1] **of** integer, 

  Data structures are initialized to false and 0 respectively



![image-20221002204837783](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002204837783.png)



<br>

1) MTX requirement is met 
   consider Pi in its CS and Pk trying to enter CS. 
   when Pk executes the second while statement for j==1, it finds  that 
   - number[j] ≠ 0 
   - (number[j], j) < (number[i], i) 
2) Progress Requirement is met
3) Bounded-waiting Requirement - ensures fairness - process enters CS on FCFS basis



<br>

## Synchronization Hardware

- Many systems provide hardware support for implementing the critical  section code. 
- All solutions below based on idea of locking 
  - Protecting critical regions via locks 
- Uniprocessors – could disable interrupts 
  - Currently running code would execute without preemption 
  - Generally too inefficient on multiprocessor systems 
    - Operating systems using this not broadly scalable 
- Modern machines provide special atomic hardware instructions 
  - Atomic = non-interruptable 
  - Either test memory word and set value 
  - Or swap contents of two memory words



<br>

## Solution to Critical-section Problem Using Locks

![image-20221002205101587](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205101587.png)





<br>

## test_and_set Instruction

- Test and modify the content of a word atomically.

  ![image-20221002205126651](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205126651.png)

- Executed atomically (Non-interruptible) 
- Returns the original value of passed parameter (lock) 
- Set the new value of passed parameter to “TRUE”. 
- If multiple processes attempting to execute instruction, first successful  process will have value false returned to it



<br>

## Solution using test_and_set()

- Shared Boolean variable lock, initialized to FALSE 

- Solution:

  ![image-20221002205221103](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205221103.png)

- It works fine 
- Problem:  
  - fairness (starvation) – no bounded waiting 
  - Waste CPU cycle 
    - CPU cycles to check test&set routine



<br>

## compare_and_swap Instruction

Definition:

![image-20221002205311507](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205311507.png)

- Executed atomically 
- Returns the original value of passed parameter “value” 
- Set the variable “value” the value of the passed parameter “new_value” but  only if “value” ==“expected”. That is, the swap takes place only under this  condition.



<br>

## Solution using compare_and_swap

- Shared integer “lock” initialized to 0;  
- Solution:

![image-20221002205409806](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205409806.png)

- Problem:  
  - fairness (starvation) – no bounded waiting 
  - Waste CPU cycle



<br>

## Bounded-Waiting Mutual Exclusion with Test_and_Set

![image-20221002205506699](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205506699.png)



- Shared data (initialized to false):  
- boolean lock; 
- boolean waiting[n]; 
- Satisfies all the CS requirements 

1. MTX 

   - Pi can enter CS only if either  waiting[i] == false or key == false

   - Value of key can become false only if  testAndSet is ececuted 
   - waiting[i] can become false only if  another process leaves CS 
   - -> Only one waiting[i] can become  false

2. Progress Requirement

   ![image-20221002205639849](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205639849.png)

- a process exiting CS either sets lock to false, or sets  waiting[j] to false to allow a process that is waiting to enter  CS to proceed

3. Bounded-waiting Requirement

   ![image-20221002205716160](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205716160.png)

- any process waiting to enter CS will do so within n-1 times



<br>

## Mutex Locks

- Previous solutions are complicated and generally inaccessible to  application programmers 
- OS designers build software tools to solve critical section problem 
- Simplest is mutex lock 
- Product critical regions with it by first acquire() a lock then release() it 
  - Boolean variable indicating if lock is available or not 
- Calls to acquire() and release() must be atomic 
  - Usually implemented via hardware atomic instructions 
- But this solution requires **busy waiting** 
  - This lock therefore called a **spinlock** 
  - Good locking mechanism used in multiprocessor system when lock is held for a short duration (duration of less than 2 context switches) 



<br>

## acquire() and release()

![image-20221002205926375](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002205926375.png)



<br>

## Semaphore

- Synchronization tool that provides more sophisticated ways (than Mutex locks) for  process to synchronize their activities. 

- Each process must perform operations on semaphore before entering its own CS 

  - Spinlock : no context switch required 
  - When locks are expected to be held for short times, spinlocks are useful  

- Semaphore S – integer variable 

- Can only be accessed via two indivisible (atomic) operations 

  - **wait()** and **signal()** 
    - Originally called **P()** and **V()** 

- Definition of the **wait() operation**

  ```c
  wait(S) { 
      while (S <= 0); // busy wait
      S--;
  }
  ```

- Definition of the signal() operation

  ```c
  signal(S) { 
      S++;
  }
  ```

  

<br>

## Semaphore Usage

- Counting semaphore – integer value can range over an unrestricted domain 

- Binary semaphore – integer value can range only between 0 and 1 

  - Same as a mutex lock 

- Can solve various synchronization problems 

  - semaphore “mutex” initialized to 1

    ![image-20221002210231403](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002210231403.png)

- Consider P<sub>1</sub> and P<sub>2</sub> that require S<sub>1</sub> to happen before S<sub>2</sub>

  Create a semaphore “synch” initialized to 0 

  ![image-20221002210337294](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002210337294.png)

- Can implement a counting semaphore S as a binary semaphore



<br>

## Semaphore Implementation

- Must guarantee that no two processes can execute the wait() and  signal() on the same semaphore at the same time 
- Thus, the implementation becomes the critical section problem where  the wait and signal code are placed in the critical section 
  - Could now have busy waiting in critical section implementation 
    - But implementation code is short 
    - Little busy waiting if critical section rarely occupied 
- Note that applications may spend lots of time in critical sections and  therefore this is not a good solution



<br>

## Semaphore Implementation with no Busy waiting

- With each semaphore there is an associated waiting queue 
- Each entry in a waiting queue has two data items: 
  - value (of type integer) 
  - pointer to next record in the list 
- Two operations: 
  - **block** – place the process invoking the operation on the  appropriate waiting queue 
  - **wakeup** – remove one of processes in the waiting queue  and place it in the ready queue

```c
typedef struct{
    int value;
    struct process *list; /* list of process(waitlist) /*
} semaphore;
```



<br>

## Implementation with no Busy waiting (Cont.)

```c
wait(semaphore *S) { 
    S->value--; 
    if (S->value < 0) {
        add this process to S->list; 
        block(); 
    } 
}
signal(semaphore *S) { 
    S->value++; 
    if (S->value <= 0) {
        remove a process P from S->list; 
        wakeup(P); /* place P onto ready queue */
    } 
}
```



<br>

## Semaphore as General Synchronization Tool

- Execute B in Pj only after A executed in Pi 
- Use semaphore flag initialized to 0 
- Code:

![image-20221002210808139](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002210808139.png)



<br>

## Two Types of Semaphores

- Counting semaphore - integer value can range over an  unrestricted domain. 
- Binary semaphore – integer value can range only between 0  and 1; can be simpler to implement. 
- Can implement a counting semaphore S as a binary semaphore.



<br>

## Implementing S as a Binary Semaphore

- Data structures:

  var S1: binary-semaphore; 

  S2: binary-semaphore; 

  C: integer;

- Initialization:

  S1 = 1 

  S2 = 0 

  C = initial value of semaphore S



<br>

## Implementing S (Cont.)

- wait operation

![image-20221002211023675](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211023675.png)

- signal operation

![image-20221002211037875](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211037875.png)



<br>

## Problems with Semaphores

- Incorrect use of semaphore operations: 
  - signal (mutex) …. wait (mutex)
  - wait (mutex) … wait (mutex) 
  - Omitting of wait (mutex) or signal (mutex) (or both) 
- Deadlock and starvation are possible.



<br>

## Monitors

- A high-level abstraction that provides a convenient and effective mechanism for  process synchronization 
- Abstract data type, internal variables only accessible by code within the procedure 
  - Ensure mutex at higher level, within monitor 
    - Only one process at a time can be executing within monitor 
  - Encapsulates data, procedures to manipulate data into one module 
- Only one process may be active within the monitor at a time 
- But not powerful enough to model some synchronization schemes

![image-20221002211222031](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211222031.png)



<br>

- A high-level abstraction that provides a convenient and effective  mechanism for process synchronization 
- Abstract data type, internal variables only accessible by code within the  procedure 
- Only one process may be active within the monitor at a time 
- But not powerful enough to model some synchronization schemes

![image-20221002211258316](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211258316.png)



<br>

## Monitors (continued)

- Initialization code is executed when monitor is declared 
- Monitor procedures can only access variables declared within  monitor, procedures 
  - Variables within monitor can not be accesses outside of monitor 
  - Only access to monitor is via calls to its procedures labeled entry 



<br>

## Schematic view of a monitor

![image-20221002211359527](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211359527.png)

<br>

## Monitors (continued)

- To allow a process to wait within the monitor, a condition variable must  be declared, as

  ​		condition x, y; 

  - Associated with each condition is a queue 

- Two operations are allowed on a condition variable: 

- Processes placed onto / removed from queue via wait and signal 

  - x.wait() – means that the process that invokes this operation is  suspended until another process invokes x.signal()  
  - x.signal() – resumes exactly one suspended process (if any) that invoked x.wait() 
    - If no x.wait() on the variable, then it has no effect on the  variable 
    - If no process is suspended, then the signal operation has no  effect.



<br>

## Monitor with condition variables

![image-20221002211530941](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211530941.png)



<br>

## Monitors (continued)

- Suppose that, when the x.signal is invoked by a process P, there is a  suspended process Q associated with condition x 
- If the suspended process Q is allowed to resume its execution, the  signaling process P must wait, otherwise, both P and Q will be active  simultaneously within the monitor 
- 2 possibilities 
  - Signal and wait – P either waits until Q leaves the monitor, or  waits for another condition 
  - Signal and continue - Q either waits until P leaves the monitor, or  waits for another condition 
  - Both have pros and cons – language implementer can decide 
  - Monitors implemented in Concurrent Pascal compromise  
    - P executing signal immediately leaves the monitor, Q is resumed 
  - Implemented in other languages including Mesa, C#, Java



<br>

## Monitor Implementation Using Semaphores

- Variables 

![image-20221002211654825](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211654825.png)

- Each procedure F will be replaced by

![image-20221002211711850](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211711850.png)

- Mutual exclusion within a monitor is ensured by mutex semaphore 
- Signaling process must wait until the resumed process either leaves or waits 
  - Next semaphore on which signaling processes may suspend themselves  
  - Next-count counts the number of processes suspended on next



<br>

## Monitor Implementation - Condition Variables

- For each condition variable x, we have: 

![image-20221002211754045](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211754045.png)

- The operation x.wait can be implemented as:

![image-20221002211807215](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211807215.png)



<br>

## Monitor Implementation (Cont.)

- The operation x.signal can be implemented as:

![image-20221002211833560](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002211833560.png)



<br>

## Monitor Implementation (Cont.)

- Resuming Processes within a Monitor 
  - If several processes queued on condition x, and x.signal()  executed, which should be resumed? 
  - FCFS frequently not adequate 
- Condition operation with priority (process resumption order) 
  - Conditional-wait construct: x.wait(c); 
  - c – integer expression evaluated when the wait operation is  executed. 
  - value of c (priority number) stored with the name of the process  that is suspended. 
  - when x.signal is executed, process with smallest associated  priority number is resumed next.



<br>

## Single Resource allocation

- Allocate a single resource among competing processes using  priority numbers that specify the maximum time a process plans  to use the resource

![image-20221002212138116](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002212138116.png)

- Where R is an instance of type ResourceAllocator



<br>

## A Monitor to Allocate Single Resource

![image-20221002212250416](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002212250416.png)



<br>

## Monitor Implementation (Cont.)

- Check to establish correctness of system:  
  - User processes must always make their calls on the monitor in a  correct sequence. 
  - Must ensure that an uncooperative process does not ignore the  mutual-exclusion gateway provided by the monitor, and try to  access the shared resource directly, without using the access  protocols.



<br>

## Liveness

- Deadlock – two or more processes are waiting indefinitely for an event that can  be caused by only one of the waiting processes. 
- Let S and Q be two semaphores initialized to 1

![image-20221002212403259](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002212403259.png)

- Starvation – indefinite blocking.  
  - A process may never be removed from the semaphore queue in which it  is suspended. 
- Priority Inversion – Scheduling problem when lower-priority process holds a  lock needed by higher-priority process 
  - Solved via priority-inheritance protocol



<br>

## Atomic Transactions

- System Model 
- Log-based Recovery 
- Checkpointing 
- Concurrent Atomic Transactions



<br>

## System Model

- Assures that operations happen as a single logical unit of work, in its  entirety, or not at all 
- Related to field of database systems 
- Challenge is assuring atomicity despite computer system failures 
- **Transaction** - collection of instructions or operations that performs  single logical function 
  - Here we are concerned with changes to stable storage – disk 
  - Transaction is series of **read** and **write** operations 
  - Terminated by **commit** (transaction successful) or abort (transaction failed) operation 
  - Aborted transaction must be **rolled back** to undo any changes it  performed



<br>

## Types of Storage Media

- Volatile storage – information stored here does not survive system  crashes 

  - Example: main memory, cache 

- Nonvolatile storage – Information usually survives crashes 

  - Example: disk and tape 

- Stable storage – Information never lost 

  - Not actually possible, so approximated via replication or RAID to  devices with independent failure modes 

  Goal is to assure transaction atomicity where failures cause loss of  information on volatile storag



<br>