---
layout: single
title: "[OS] Synchronization Examples"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---

# Chapter 7: Synchronization Examples

- Classic Problems of Synchronization 
  - the bounded-buffer synchronization problem 
  - the readers-writers synchronization problem 
  - The dining-philosophers synchronization problems 
- Synchronization within the Kernel 
- the tools used by Linux and Windows to solve synchronization problems. 
- POSIX Synchronization 
- Synchronization in Java  
- Alternative Approaches



<br>

## Classical Problems of Synchronization

- Classical problems used to test newly-proposed synchronization  schemes 
  - Bounded-Buffer Problem 
  - Readers and Writers Problem 
  - Dining-Philosophers Problem



<br>

## Bounded-Buffer Problem

- n buffers, each can hold one item 
- Semaphore mutex initialized to the value 1 
- Semaphore full initialized to the value 0 
- Semaphore empty initialized to the value n



<br>

## Bounded Buffer Problem (Cont.)

- The structure of the producer process

![image-20221002214616833](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002214616833.png)



<br>

## Bounded Buffer Problem (Cont.)

- The structure of the consumer process

![image-20221002214648618](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002214648618.png)



<br>

## Readers-Writers Problem

- A data set is shared among a number of concurrent processes 
  - Readers – only read the data set; they do not perform any updates 
  - Writers – can both read and write 
- Determine need for mutex based on nature of processes 
  - Readers : allow multiple readers can read data simultaneously 
  - Writers : Only one single writer can access the shared data at the same time 
    - restrict to 1 at a time (mutex) 
    - Also writers should have exclusive access to data 
      - Should not have reader reading while writer is writing 
      - Readers in CS should block writers from entering CS 
      - Writers in CS should block readers and other writers from entering  CS 
- Several variations of how readers and writers are considered – all involve some  form of priorities



<br>

## Readers-Writers Problem (Cont.)

- Shared Data 

  - Data set 

  - Semaphore rw_mutex initialized to 1   // rw_mutex functions as a mutex semaphore  

    ​																	// for writers. used by first or last reader 

    ​																	// that enters or exists cs 

  - Semaphore mutex initialized to 1         // mutex is used to ensure mutex when  

    ​																	// read_count is updated 

  - Integer read_count initialized to 0



- The structure of a writer process

![image-20221002215006472](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215006472.png)

<br>

## Readers-Writers Problem (Cont.)

- The structure of a reader process

![image-20221002215037719](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215037719.png)



<br>

## Readers-Writers Problem Variations

- The solution in previous slide can result in a situation where a writer process  never writes. It is referred to as the “First reader-writer” problem. 
- First variation - Reader’s priority 
  - no reader kept waiting unless writer has permission to use shared object 
  - No reader should wait simply because a writer is ready 
  - Readers obtain access to CS when needed 
  - Only block if writer has access to CS 
  - Writers may starve  
- Second variation - Writer’s priority 
  - once writer is ready, it performs the write ASAP 
  - If a writer is waiting to access a object, no new readers may start reading 
  - Readers may starve 
- Both may have starvation leading to even more variations 
- Problem is solved on some systems by kernel providing reader-writer locks



<br>

## Dining-Philosophers Problem

![image-20221002215159971](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215159971.png)

- Philosophers spend their lives alternating thinking and eating 
- Don’t interact with their neighbors, occasionally try to pick up 2  chopsticks (one at a time) to eat from bowl 
  - Need both to eat, then release both when done 
- In the case of 5 philosophers 
  - Shared data  
    - Bowl of rice (data set) 
    - Semaphore chopstick [5] initialized to 1



<br>

## Monitor with condition variables

![image-20221002215244131](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215244131.png)

<br>

## Dining-Philosophers Problem Algorithm

- Semaphore Solution 

- The structure of Philosopher i :

  ```c
  while (true){ 
      wait (chopstick[i] );
      wait (chopStick[ (i + 1) % 5] );
      /* eat for awhile */
      signal (chopstick[i] );
      signal (chopstick[ (i + 1) % 5] );
      /* think for awhile */
  }
  ```

- What is the problem with this algorithm?



<br>

## Monitor Solution to Dining Philoshophers

```c
monitor DiningPhilosophers
{ 
    enum { THINKING; HUNGRY, EATING) state [5] ;
          condition self [5];
          void pickup (int i) { 
              state[i] = HUNGRY;
              test(i);
              if (state[i] != EATING) self[i].wait;
          }
          void putdown (int i) { 
              state[i] = THINKING;
              // test left and right neighbors
              test((i + 4) % 5);
              test((i + 1) % 5);
          }
          void test (int i) { 
              if ((state[(i + 4) % 5] != EATING) &&
                  (state[i] == HUNGRY) &&
                  (state[(i + 1) % 5] != EATING) ) { 
                  state[i] = EATING ;
                  self[i].signal () ;
              }
          }
          initialization_code() { 
              for (int i = 0; i < 5; i++)
                  state[i] = THINKING;
          }
}
```



<br>

## Solution to Dining Philosophers (Cont.)

- Each philosopher “i” invokes the operations pickup() and  putdown() in the following sequence:

![image-20221002215440221](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215440221.png)

- No deadlock, but starvation is possible



<br>

## A monitor to allocate single resource

```c
Monitor ResourceAllocation {
    boolean busy; 
    condition x; 
    void acquire (int time){
        if (busy)
            x.wait (time);
        busy = true;
    }
    void release() {
        busy = false;
        x.signal();
    }
    void init() {
        busy = false;
    }
}
```

```c
ResourceAlocation R;

R.acquire(t);
	access the resource
R.release();
```



<br>

## Synchronization Examples

- Solaris 
- Windows 
- Linux 
- Pthread 
- Java



<br>

## Solaris Synchronization

- Implements a variety of locks to support multitasking, multithreading  (including real-time threads), and multiprocessing 
- Uses adaptive mutexes for efficiency when protecting data from short  code segments 
  - Starts as a standard semaphore spin-lock 
  - If lock held, and by a thread running on another CPU, spins 
  - If lock held by non-run-state thread, block and sleep waiting for signal of  lock being released 
- Uses condition variables and readers-writers locks when longer  sections of code need access to data 
- Uses turnstiles to order the list of threads waiting to acquire either an  adaptive mutex or reader-writer lock 
- Turnstiles are per-lock-holding-thread, not per-object 
- Priority-inheritance per-turnstile gives the running thread the highest of the priorities of the threads in its turnstile 
- Uses readers-writers locks when longer sections of code need access  to data



<br>

## Kernel Synchronization - Windows

- Uses interrupt masks to protect access to global resources on  uniprocessor systems 
- Uses spinlocks on multiprocessor systems 
  - Spinlocking-thread will never be preempted 
- Also provides dispatcher objects 
  - The kernel defines a set of object types called kernel dispatcher  objects, or just dispatcher objects.  
  - Dispatcher objects include timer objects, event objects,  semaphore objects, mutex objects, and thread objects. 
  - Events 
    - An event acts much like a condition variable 
  - Timers notify one or more thread when time expired 
  - Dispatcher objects either signaled-state (object available) or  non-signaled state (thread will block)



<br>

- Every kernel-defined dispatcher object type has a state that is  either set to Signaled or set to Not-Signaled. 
- Mutex dispatcher object

![image-20221002220101063](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220101063.png)



<br>

## Linux Synchronization

- Linux: 
  - Prior to kernel Version 2.6, Linux was a non-preemptive kernel 
    - disables interrupts to implement short critical sections 
  - Version 2.6 and later, fully preemptive 
    - A task can be preempted when it is running in kernel 
- Linux provides: 
  - atomic integers 
  - spinlocks 
  - semaphores 
  - reader-writer versions of both (spinlock, semaphores)

- On single-CPU system, spinlocks replaced by enabling and  disabling kernel preemption



<br>

- atomic variables 

- atomic_t is the type for atomic integer 

- Simplest synchronization tool within Linux kernel 

- All math operations using atomic integers are performed without interruption 

- Atomic operations do not need the overhead of locking mechanism  

  

- atomic_t counter; 

- int value

![image-20221002220244235](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220244235.png)



<br>

## mutex_lock, spin_lock

- Mutex lock is used to protect critical sections within kernel 

  mutex_lock() 

  critical section // sleep is possible 

  mutex_unlock() 

- Reader-writer version 

- Spin lock & semaphore are used for locking in the kernel 

  - On SMP machine, spinlock is a fundamental locking mechanism 
  - On single-CPU system, spinlocks is not inappropriate 
    - enabling and disabling kernel preemption

![image-20221002220430057](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220430057.png)



<br>

## Pthread (POSIX) Synchronization

- Pthreads API is OS-independent user level threads 
- Widely used on UNIX, Linux, and macOS 
- It provides: 
  - mutex locks 
  - Semaphore (named, unnamed) 
  - condition variable 
- Non-portable extensions include: 
  - read-write locks 
  - spinlocks



<br>

## POSIX Mutex Locks

- Fundamental synchronization tool used with Pthreads  
- Creating and initializing the lock

![image-20221002220547561](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220547561.png)

- Acquiring and releasing the lock

![image-20221002220601230](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220601230.png)

<br>

## POSIX Semaphores

- POSIX provides two versions – **named** and **unnamed**. 
- Named semaphores can be used by unrelated processes, unnamed  cannot.



<br>

## POSIX Named Semaphores

- Creating an initializing the semaphore:

![image-20221002220656274](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220656274.png)

- Another process can access the semaphore by referring to its name SEM. 
- Acquiring and releasing the semaphore:

![image-20221002220712037](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220712037.png)



<br>

## POSIX Unnamed Semaphores

- Creating an initializing the semaphore:

![image-20221002220736432](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220736432.png)

- Acquiring and releasing the semaphore:

![image-20221002220749296](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220749296.png)

<br>

## POSIX Condition Variables

- Since POSIX is typically used in C/C++ and these languages do not  provide a monitor, POSIX condition variables are associated with a  POSIX mutex lock to provide mutual exclusion: Creating and initializing  the condition variable:

![image-20221002220813118](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220813118.png)



<br>

## POSIX Condition Variables

- Thread waiting for the condition a == b to become true:

![image-20221002220839484](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220839484.png)

- Thread signaling another thread waiting on the condition variable:

![image-20221002220851449](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220851449.png)



<br>

## Monitor with condition variables

![image-20221002220914354](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220914354.png)



<br>

## Java Synchronization

- Java provides rich set of synchronization features:  
  - Java monitors 
  - Reentrant locks 
  - Semaphores 
  - Condition variables



<br>

## Java Monitors

- Concurrency mechanism for thread synchronization 
- Every Java object has associated with it a single lock. 
- If a method is declared as synchronized, a calling thread must own  the lock for the object. (single thread can be active in a monitor) 
- If the lock is owned by another thread, the calling thread must wait for  the lock until it is released. (entry Q) 
- Locks are released when the owning thread exits the synchronized method.



<br>

## Bounded Buffer - Java Synchronization

![image-20221002221030098](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221030098.png)



<br>

## Java Synchronization

- A thread that tries to acquire an unavailable lock is placed in the  object’s entry set: (entry Queue)

![image-20221002221058784](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221058784.png)



<br>

- Similarly, each object also has a wait set. (condition Queue) 
- When a thread calls wait(): 
  1. It releases the lock for the object 
  2. The state of the thread is set to blocked 
  3. The thread is placed in the wait set for the object

![image-20221002221134732](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221134732.png)



<br>

- A thread typically calls wait() when it is waiting for a condition to  become true. 

- How does a thread get notified? 

- When a thread calls notify(): 

  		1. An arbitrary thread T is selected from the wait set 

  2. T is moved from the wait set to the entry set 

  3. Set the state of T from blocked to runnable. 

- T can now compete for the lock to check if the condition it was waiting  for is now true



<br>

## Bounded Buffer - Java Synchronization

![image-20221002221326154](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221326154.png)

![image-20221002221334049](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221334049.png)



<br>

## Java Reentrant Locks

- Similar to mutex locks 
- The finally clause ensures the lock will be released in case an  exception occurs in the try block.

![image-20221002221438670](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221438670.png)



<br>

## Java Semaphores

- Constructor:

![image-20221002221508302](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221508302.png)



- Usage:

![image-20221002221520120](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221520120.png)



<br>

## Java Condition Variables

- condition variables provide similar functionality to wait() & notify() 

- To provide mutual exclusion, a condition variable is associated with an  ReentrantLock. (java’s syncronized) 

- Creating a condition variable by first first creating a ReentrantLock:  and invoking its newCondition() method of ReentrantLock: 

  ![image-20221002221555261](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221555261.png)

- A thread waits by calling the await() method, and signals by calling  the signal() method of condition variable.



<br>

## Monitor with condition variables

![image-20221002221616890](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221616890.png)

<br>

## Java Condition Variables

- Example: Five threads numbered 0 .. 4 
- Shared variable turn indicating which thread’s turn it is. 
- Thread calls doWork() when it wishes to do some work.  
  - But it may only do work if it is their turn. 
- If not their turn, wait 
- If their turn, do some work for awhile …... 
- When completed, notify the thread whose turn is next. 
- Necessary data structures:

![image-20221002221700820](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221700820.png)



<br>

## Java Condition Variables

![image-20221002221735544](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221735544.png)



<br>

## Alternative Approaches

- Transactional Memory 
- OpenMP 
- Functional Programming Languages



<br>

## Transactional Memory

- Consider a function update() that must be called atomically. One option is  to use mutex locks:

![image-20221002221916655](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221916655.png)

- Synchronization mechanism such as semaphores, mutex locks has  deadlock and scalability problem, as the number of threads increases  
- A memory transaction is a sequence of read-write operations to  memory that are performed atomically. A transaction can be completed  by adding atomic{S} which ensure statements in S are executed  atomically:

![image-20221002221936000](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221936000.png)

- Software transaction memory 
  - Implemented in software 
- Hardware transaction memory 
- Growth of multicore systems and emphasis on concurrent and parallel  programming have prompted significant research in this area. 



<br>

## OpenMP

- OpenMP is a set of compiler directives and API that support parallel  progamming.
- #pragma omp parallel

![image-20221002222021494](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002222021494.png)

- The code contained within the #pragma omp critical directive is  treated as a critical section and performed atomically.



<br>

## Functional Programming Languages

- Imperative (procedure-oriented) language: C, C++, Java 
- Functional programming languages offer a different paradigm  than procedural languages in that they do not maintain state.  
- Variables are treated as immutable and cannot change state  once they have been assigned a value. 
- There is increasing interest in functional languages such as  Erlang and Scala for their approach in handling data races.

































