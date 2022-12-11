---
layout: single
title: "[OS] Synchronization Examples"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---

[toc]



# Chapter 7: Synchronization Examples

- Classic Problems of Synchronization 
  - the bounded-buffer synchronization problem 
  - the readers-writers synchronization problem 
  - The dining-philosophers synchronization problems 
- Synchronization within the **Kernel** 
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

- **n** buffers, each can hold one item 
  - producer와 consumer간의 shared buffer
  - in, out index로 조절 - full, empty 판단
    - 혹은 count 변수로




- Semaphore **mutex** initialized to the value 1  - binary semaphore
  - critical section의 진입 -> 0으로 초기화하면 영원히 못들어감!
  - binary semaphore

- Semaphore **full** initialized to the value 0 
  - <mark>차있는 slot의 갯수</mark>
  - count semaphore -> 0~n

- Semaphore **empty** initialized to the value n
  - 비어있는 slot의 갯수
  - count semaphore -> 0~n


- semaphore
  - 개발자, hardware의 도움이 아닌 OS의 도움!

<br>

## Bounded Buffer Problem (Cont.)

- The structure of the producer process

![image-20221002214616833](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002214616833.png)

- common buffer에 삽입하는 부분이 CS임 (add next ...)

- produce가 CS에 들어가면 consumer는 못 들어감
- wait(empty) - 빈 slot이 없으면 대기(block)
  - empty의 값이 0에서 1로 바뀔 때까지
  - empty의 값이 있기를 기다림 -> 빈 slot이 있기를 기다림
- wait(mutex)

<br>

## Bounded Buffer Problem (Cont.)

- The structure of the consumer process

![image-20221002214648618](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002214648618.png)

- 꺼내오려는데 꺼내올 것이 없으면 대기해야 하기 때문에 wait(full) 값이 0이면 wait한다.
- full의 값이 있기를 기다림

<br>

## Readers-Writers Problem

- A data set is shared among a number of concurrent processes 
  - **Readers** – only read the data set; they do not perform any updates 
    - 읽기만 하는 process
    - 여러개가 동시에 있어도 OK
  - **Writers** – can both read and write 
    - 읽기도 하고 쓰기도 하는 프로세스
    - 오직 한 process만 가능
  - readers와 writers가 동시에 있는 것도 불가능
    - 즉 writer가 있으면 무조건 걔만 딱 하나 있어야 함.
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

  - Semaphore **rw_mutex** initialized to 1   // rw_mutex functions as a mutex semaphore  

    ​																	// for writers. used by first or last reader 

    ​																	// that enters or exists cs 

    - 1) writers끼리 경쟁
    - 2. reader vs. writer
    - writer가 경쟁하는 것은 first reader
      - 첫번째 reader가 들어가면 그 다음 reader는 계속 들어가지기 때문에
    - 진입제어 용
  
  - Semaphore **mutex** initialized to 1         // mutex is used to ensure mutex when  
  
    ​																	// read_count is updated 
  
    - read_count가 바뀔 때 atomic 함을 보장해 주기 위해
  
  - Integer **read_count** initialized to 0
  
    - critical section에 진입을 시도하는 reader process의 갯수
    - read_count 값을 바꾸는 과정이 atomic하지 않는다.
    - 그래서 mutex로 감싸서 atomic operation을 보장한다.



- The structure of a writer process

![image-20221002215006472](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215006472.png)

- rw_mutex 값을 1로 초기화
  - critical section을 빠져나오면서 signal(rw_mutex)를 통해 rw_mutex 값이 0에서 1로 바뀌어야지 다른 것들이 들어갈 수 있게 됨.
- queue를 통해 FCFS 보장

<br>

## Readers-Writers Problem (Cont.)

- The structure of a reader process

![image-20221002215037719](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215037719.png)

- reader가 하나 들어가면 read_count값을 하나 증가시킨다.
- mutex는 read_count 변수를 update할 때 atomic operation를 보장하기 위해서이므로 wait(mutex) 후에read_count 값을 바꿀 수 있다.
- read_count == 0 
  - CS에 존재하는 reader가 없다.
  - CS를 빠져 나오는 reader가 0
  - signal(rw_mutex) -> 0
  - 이제 writer가 들어갈 수 있음
- read_count 가 2를 넘어서면 rw_mutex 값을 바꾸지 않기 때문에 계속 들어갈 수 있음

<br>

## Readers-Writers Problem Variations

- The solution in previous slide can result in a situation where a writer process never writes. It is referred to as the “**First reader-writer**” problem. 
- First variation - **Reader’s priority**(reader에게 유리함) 
  - 만약 최초의 CS 진입 경쟁을 할 때 reader가 이겨서 reader가 들어갔다면 그 후에 reader는 계속 들어갈 수 있지만 writer는 못들어감(rw_mutex 값은 read_count가 ==0 일 때만 변경 되기 때문에 )
  - <u>no reader kept waiting unless writer has permission to use shared object</u> 
  - No reader should wait simply because a writer is ready 
  - Readers obtain access to CS when needed 
  - Only block if writer has access to CS 
  - Writers may starve
    - -> 근데 writer에게 유리하게 코드를 바꿀 수 있음  
  
- Second variation - **Writer’s priority** 
  - writer와 reader가 경쟁하면 무조건 writer에게 우선순위
  - <u>once writer is ready, it performs the write ASAP</u> 
    - writer님이 들어가고 싶어하십니다! -> reader들은 못들어감
  - If a writer is waiting to access a object, no new readers may start reading 
  - Readers may starve 

- Both may have starvation leading to even more variations 
- Problem is solved on some systems by **kernel** providing **reader-writer locks**



<br>

## Dining-Philosophers Problem

![image-20221002215159971](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215159971.png)

- Philosophers spend their lives alternating **thinking** and **eating** 
- Don’t interact with their neighbors, occasionally try to pick up 2  chopsticks (one at a time) to eat from bowl 
  - Need both to eat, then release both when done 
    - 젓가락 두 개를 모두 집어야 식사 가능 -> 식사 후에 젓가락 내려놓기 가능
    - 한 번에 하나의 젓가락만 집을 수 있음
- In the case of 5 philosophers 
  - Shared data  
    - Bowl of rice (data set) 
    - Semaphore chopstick [5] initialized to 1
      - binary semaphore



<br>

## Monitor with condition variables

![image-20221002215244131](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215244131.png)

- 2개 이상의 process가 monitor 안에서 실행되면 동기화 자체에 문제가 생긴다.

- initialization code: monitor가 만들어질 때 딱 한 번 실행됨
  - shared data 초기화

- 사용 중인 모니터에 접근하기를 원해서 대기 중인 프로세스들은 **entry queue**에서 대기한다.
  
- 해당 condition을 기다리는 프로세스들은 condition queue에서 대기한다.
  
- next queue의 역할?
  - 다른 프로세스가 모니터 내부에 있어서 잠시 대기하는 큐
  - 모니터 내부 작업 중에 잠시 프로세스가 대기하는 큐

- 제일 우선순위 높은 ; next queue, entry queue, 새로들어온 애
  - condition queue는 next queue나 entry queue와는 비교 대상이 아님
    - condition이 만족해야만 깨어날 수 있기 때문

<br>

## Dining-Philosophers Problem Algorithm

- Semaphore Solution 

- The structure of Philosopher i :

  ```c
  while (true){ 
      wait (chopstick[i] );
      wait (chopStick[ (i + 1) % 5] );
      
      /* eat for awhile - critical section*/
      
      signal (chopstick[i] );
      signal (chopstick[ (i + 1) % 5] );
      
      /* think for awhile */
      
  }
  ```

- What is the problem with this algorithm?

  - 모든 다섯명의 철학자가 첫 코드를 실행했다고 했을 때 -> 모두가 젓가락 하나씩을 집으려고 할 때
    - 그 다음 코드를 실행하지 못할 것이기 때문에 deadlock 발생 (코드가 진행이 안 됨)

  - deadlock이 발생하지 않도록 semaphore로 해결할 수도 있음

- 간단한 솔루션 -> semaphore 사용
  - 철학자는 그 semaphore에 wait()연산을 실행하여 젓가락을 집으려고 시도
  - signal()연산을 수행하여 자신의 젓가락을 내려 놓음
- 공유 데이터: 밥그릇 (데이터 셋)
  - 1로 초기화된 semaphore -> chopstick[5]
- 인접한 두 철학자가 동시에 식사하지 않음을 권장
  - 그러나 모두가 동시에 자신의 왼쪽 젓가락을 집는다면?? -> deadlock 발생!
  - 해제가 되려면 누군가는 오른쪽 젓가락을 집어야 되는데 아무도 해제가 될 상황이 없기 때문.



- 해결안 1)
  - 최대 4명의 철학자들만이 테이블에 동시에 앉을 수 있도록 함
- 해결안 2)
  - 한 철학자가 젓가락 두 개를 모두 집을 수 있을 때만 젓가락을 집도록 허용
- 해결안 3)
  - 비대칭 해결안 사용. 홀수 번호의 철학자는 왼쪽 젓가락부터, 짝수 번호의 철학자는 짝수 젓가락부터 집을 수 있다는 rule 적용


<br>

## Monitor Solution to Dining Philoshophers

```c
monitor DiningPhilosophers
{ 
    enum { THINKING; HUNGRY, EATING) state [5] ;
          condition self [5];
          void pickup (int i) { // wait에 해당하는 부분
              state[i] = HUNGRY;
              test(i); // 왼쪽 오른쪽 젓가락 확보를 위한 작업
              if (state[i] != EATING) self[i].wait;
          }
          void putdown (int i) { //signal에 해당하는 부분
              state[i] = THINKING;
              
              // test left and right neighbors - 내 좌우 사람들 중 나때문에 못먹던 사람 깨우기
              test((i + 4) % 5);
              test((i + 1) % 5);
          }
          void test (int i) { // private 함수(내부용)
              if ((state[(i + 4) % 5] != EATING) && // 나의 왼쪽 철학자가 먹지 않고 있으면서
                  (state[i] == HUNGRY) && // 내가 HUNGRY 상태 이면서
                  (state[(i + 1) % 5] != EATING) )  // 나의 오른쪽 철학자가 먹지 않고 있을 때!
              { 
                  state[i] = EATING ;
                  self[i].signal () ;
              }
          }
          initialization_code() { // 생성자
              for (int i = 0; i < 5; i++)
                  state[i] = THINKING; // CS 진입을 시도하지 않은 상태로 초기화
          }
}
```

- monitor instance (앞에 class가 붙었으면 class instance였을 것임)
- EATING 상태 - CS에 진입한 상태
- HUNGRY 상태 - CS에 진입하려는 상태
  - 젓가락 확보를 위해 경쟁하고 있는
- test 함수
  - 내가 밥을 먹으면 내 옆의 철학자는 eating으로 바뀌지 않고 self.wait으로 condition queue에 들어가지기 때문에 즉, wait하고 있기 때문에 마지막에 self[i].signal을 해 주어 깨워준다.
  - 깨어나면 pickup을 넘어갈 수 있기 때문에 critical section인 EAT에 도달할 수 있는 것이다.
- pickup()
  - 진입을 시도하기 때문에 HUNGRY로 상태를 바꿈.
  - 상태가 EATING이 아니면 self[i].wait
    - condition 변수 - self[i].wait
  - 다른 사람이 self[i].signal을 해야 살아남

<br>

## Solution to Dining Philosophers (Cont.)

- Each philosopher “i” invokes the operations pickup() and  putdown() in the following sequence:

![image-20221002215440221](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002215440221.png)

- No deadlock, but starvation is possible
  - 도착 순서에 의해서 젓가락을 잡을 수 있는 것이 아니기 때문에 일찍 hungry해도 eating하지 못할 수 있다.

- mutex 보장
  - 옆사람하고 나하고 동시에 젓가락을 잡는 경우가 생기지 않음
- progress
  - CS밖에 있는 process 때문에 CS에 못들어가는 일이 발생하지 않음
- bounded waiting
  - 실패를 하더라도 어느정도 시간 안에 들어가는 것이 보장
- 위 세가지 조건 모두 만족! by OS

<br>

## A monitor to allocate single resource

```c
Monitor ResourceAllocation {
    boolean busy; 
    condition x; 
    void acquire (int time){
        if (busy) //resource가 하나밖에 없는데 이미 할당되어져 있는 경우
            x.wait(time);
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
	access the resource //자원 사용 가능
R.release();
```



<br>

## Synchronization Examples - 나 같으면 안 낼 듯...

- Solaris 
- Windows 
- Linux 
- Pthread 
- Java



<br>

## Solaris Synchronization - XXXX

- Implements a variety of locks to support multitasking, multithreading  (including real-time threads), and multiprocessing 
- Uses **adaptive mutexes** for efficiency when protecting data from short  code segments 
  - Starts as a standard semaphore spin-lock - 기본적으로는 spinlock
  - If lock held, and by a thread running on another CPU, spins 
  - If lock held by **non-run-state thread**, **block** and sleep **waiting** for signal of  lock being released 
    - 이게 금방 풀릴 것 같지 않으면 일반적인 semaphore로 동작!
- Uses **condition variables** and **readers-writers locks** when longer  sections of code need access to data 
- Uses **turnstiles** to order the list of threads waiting to acquire either an  adaptive mutex or reader-writer lock 
  - Turnstiles(회전문) are per-lock-holding-thread, not per-object 
  - 먼저 들어온 사람이 먼저 lock 획득

- Priority-inheritance per-turnstile gives the running thread the highest of the priorities of the threads in its turnstile 
  - 어떤 lock에 대해 소유권의 우선순위가 높은 프로세스가 있는데 현재 lock을 소유하고 있는 프로세스는 그것보다 우선순위가 낮을 때 이 우선순위가 낮은 프로세스가 release를 해야 우선순위가 높은 프로세스가 사용하므로 
    - 높은 우선순위의 프로세스의 우선순위를 낮은 우선순위의 프로세스에게 잠시 빌려준다!
      - 너 이거 빌려줄테니까 빨리 끝내라 !

- Uses readers-writers locks when longer sections of code need access  to data



<br>

## Kernel Synchronization - Windows -XXXX

- Uses interrupt masks to protect access to global resources on  **uniprocessor systems** 
- Uses **spinlocks** on multiprocessor systems 
  - Spinlocking-thread will never be preempted 
- Also provides **dispatcher objects** user-land which may act mutexes, semaphores, events, and timers
  - The kernel defines a set of object types called kernel dispatcher  objects, or just dispatcher objects.  
  - Dispatcher objects include timer objects, event objects,  semaphore objects, mutex objects, and thread objects. 
  - **Events** 
    - An event acts much like a condition variable 
  - Timers notify one or more thread when time expired 
  - Dispatcher objects either **signaled-state** (object available) or **non-signaled state** (thread will block)



<br>

- Every kernel-defined dispatcher object type has a state that is  either set to Signaled or set to Not-Signaled. 
- Mutex dispatcher object

![image-20221002220101063](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220101063.png)

어떤 thread가 lock을 획득하면 nonsignaled로 바뀌고 release하면 다시 signaled 상태로 바뀐다.

<br>

## Linux Synchronization-XXXX

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

- On single-CPU system, spinlocks replaced by **enabling and  disabling kernel preemption**



<br>

- atomic variables 

- **atomic_t** is the type for atomic integer (data type)

- Simplest synchronization tool within Linux kernel 

- All math operations using atomic integers are performed without interruption 

- Atomic operations do not need the overhead of locking mechanism  

  

- atomic_t counter; 

- int value

![image-20221002220244235](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220244235.png)



<br>

## mutex_lock, spin_lock

- Mutex lock is used to protect critical sections within kernel 

  ​	mutex_lock() 

  critical section // sleep is possible 

  ​	mutex_unlock() 

- Reader-writer version 

- Spin lock & semaphore are used for locking in the kernel 

  - On SMP machine, spinlock is a fundamental locking mechanism 
  - On single-CPU system, spinlocks is not inappropriate 
    - enabling and disabling kernel preemption

![image-20221002220430057](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002220430057.png)



<br>

## Pthread (POSIX) Synchronization

이거는 user level에서의 synchronization 문제 해결하는 tool

- Pthreads API is OS-independent **user level threads** 
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

- Since POSIX is typically used in C/C++ and these languages do not  provide a monitor, **POSIX condition variables** are associated with a **POSIX mutex lock** to provide mutual exclusion: Creating and initializing  the condition variable:

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

- 여러 operation들(method)이 있는데 어떤 프로세스가 해당 메소드를 실행할 때는 오로지 한 개씩만 실행이 가능하도록 제어해야 한다.
- Java에서는 named condition variables이 없고 오직 하나의 unnamed condition variable만이 있다.

<br>

## Java Synchronization - 여기부터 쭉 XXXX

- Java provides rich set of synchronization features:  
  - Java monitors 
  - Reentrant locks 
  - Semaphores 
  - Condition variables



<br>

## Java Monitors

- Concurrency mechanism for thread synchronization 
- Every Java object has associated with it a **single lock.** 
- If a method is declared as synchronized, a calling thread **must own the lock** for the object. (single thread can be active in a monitor) 
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

- When a thread calls notify(): signal()이랑 동일

  	1. An arbitrary thread T is selected from the wait set 

  2. T is moved from the wait set to the entry set 

  3. Set the state of T from blocked to runnable. 

- T can now compete for the lock to check if the condition it was waiting  for is now true



<br>

## Bounded Buffer - Java Synchronization

![image-20221002221326154](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221326154.png)

![image-20221002221334049](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221334049.png)

notify()를 바깥으로 뺀 이유는 try안에 있으면 wait() 실행 중에 error 발생시 catch로 넘어가서 nofity()를 실행하지 못하는 경우가 있을 수 있기 때문이다.

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

- A thread waits by calling the **await()** method, and signals by calling  the **signal()** method of condition variable.



<br>

## Monitor with condition variables

![image-20221002221616890](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221616890.png)

<br>

## Java Condition Variables

- Example: Five threads numbered 0 .. 4 
- Shared variable **turn** indicating which thread’s turn it is. 
- Thread calls **doWork()** when it wishes to do some work.  
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

- Consider a function update() that **must be called atomically**. One option is  to use mutex locks:

![image-20221002221916655](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002221916655.png)

- Synchronization mechanism such as semaphores, mutex locks has **deadlock** and scalability problem, as the number of threads increases      
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

- The code contained within the #pragma omp critical directive is  treated as a critical section and performed **atomically**.



<br>

## Functional Programming Languages

- Imperative (procedure-oriented) language: C, C++, Java 
- Functional programming languages offer a different paradigm  than procedural languages in that they do not maintain state.  
- Variables are treated as immutable and cannot change state  once they have been assigned a value. 
- There is increasing interest in functional languages such as  Erlang and Scala for their approach in handling data races.
