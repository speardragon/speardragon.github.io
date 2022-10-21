---
layout: single
title: "[OS] Process Concept"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---



일부 내용은 시험에서 제외됨 (시험기간 직전에 알려줌)



# Chapter 3: Process Concept

- Process Concept 
- Process Scheduling 
- Operation on Processes 
- Inter-process Communication
- Examples of IPC Systems 
- Communication in Client-Server Systems



<br>

## Process Concept

- An operating system executes a variety of programs: 
  - Batch system – jobs 
  - Time-shared systems – user programs or tasks 
- Textbook uses the terms **job** and **process** almost interchangeably. 
- Process – a program in execution; process execution must progress in sequential fashion.  
- A process includes: 
  - **Resources**: timers, pending signals, open files, network connections, hardware, and IPC mechanisms 
  - **state, and a virtualized processor**



<br>

## The Multiple Parts (section) of a Process

- **Each process has following memory sections** 
  - **Text section** 
    - The program code(기계어로 번역된) & read-only data such as constant variables 
    - Current activity including **program counter**, processor registers 
  - **Stack section** containing temporary data 
    - Function parameters, return addresses, local variables 
  - **Data section** containing global variables 
    - 함수 밖에서 선언된 변수, static 변수
  - **Heap section** containing memory dynamically allocated during run time



<br>

## Process in Memory

![image-20220907234751229](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234751229.png)

- program definition에서 하나의 process가 만들어진 모습 - process 주소 공간
  - process 마다 만들어 지는 것! (protection 가능)

- text(code)는 program definition이 들어있는데 이를 그대로 탑재 - read only
  - 그러나 모두 다 탑재하는 것은 비효율적이기 때문에 여러 process가 공유한다.
- data: 함수 밖에서 선언한 변수가 저장되는 공간 or static 변수
- stack: local 함수 변수
- heap: 동적 메모리 할당



유일하게 공유되는 부분은 text 부분!

다른 프로세스의 정보를 읽거나 수정하기 위해서는 커뮤니케이션이 이루어져야 함.(MPP or Shared memory)

<br>

## Process Concept

- Program is **passive** entity stored on disk (**executable file**), process is **active** 
  - Program becomes process when executable file loaded into memory 
- Execution of program started via GUI mouse clicks, command line entry of its name, etc 
- One program can be several processes 
  - Consider multiple users executing the same program



<br>

## Process State

- As a process executes, it changes state 
  - new: The process is being created. 
  - running: Instructions are being executed. 
  - waiting: The process is waiting for some event to occur. 
    - I/O completion, Signal 
  - ready: The process is waiting to be assigned to a process. 
  - terminated: The process has finished execution.



<br>

## Diagram of Process State

<mark>매우 중요한 그림과 설명</mark>

![image-20220907234908196](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234908196.png)

- new: process 주소 공간을 만듬(할당)
  - PCB도 만듦.

- ready: 탑재가 종료되면 admitted 되어 CPU를 할당 받아야 하는데 다른 실행되어야 하는 프로그램들도 있기 때문에 줄(queue)을 서야 한다.
  - 갯수 제한이 없음
  - running으로 가는 걸 결정하는 것: short term scheduler

- running: **CPU scheduling** or priority
  - 오로지 하나의 process만 가능(core가 하나이기 때문에)
  - 프로세스가 interrupt가 걸린 경우 ready queue로 이동하게 됨

- waiting: I/O 혹은 event wait을 하는 경우, waiting을 하고나서 바로 실행 불가(다른 프로그램이 기다리고 있기 때문),
  - 갯수 제한이 없음
  - resume을 기다림
  - event: read, printf, sleep, recv

- terminated: 모든 자원 반납(process 주소공간, 무수히 많은 자원)
  - exit() 호출
  - 마지막 문장 실행

<br>

## Process Control Block (PCB)

![image-20221002134206650](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20221002134206650.png)

Information associated with each process. 

- new state에서 만들어짐

- Process state – running, waiting, etc 
- Program counter – location of instruction to next execute 
- CPU registers – contents of all process-centric registers 
- CPU scheduling information 
  - Priority, pointers to scheduling queues 
- Memory-management information 
  - memory allocated to the process 
  - Value of base, limit registers, page table 
- Accounting information - CPU used, clock time elapsed since start, time limits 
- I/O status information 
  - I/O devices allocated to process, list of open files


- 저장을 하는 이유: process가 running 상태에 있다가 다른 상태에 가고나서 다시 돌아올 때 실행을 잠시 중단 했던 시점의 snapshot 정보(original)를 복원하기 위한 목적

<br>

## CPU Switch From Process to Process

![image-20220907235102206](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235102206.png)

- P0가 실행되다가 interrupt나 system call을 실행한 경우 PCB0에 해당 프로세스와 관련된 정보를 모두 저장한다.
- ready state에 있던 process 중 P1이 실행을 시작한다.
  - 시작하면서 PCB1에 저장되었던 process 정보를 복원한다.




- idle -> suspend
- executing -> resume
- suspend 되는 시점에서의 정보 -> running snapshot을 PCB에 저장
- 이 중 상당부분은 context switching overhead 때문에 사용되지 못함.
  - 이 overhead를 해결하기 위해 나온 개념이 thread

<br>

## Threads

- So far, process has a single **thread of execution** 
- Consider having multiple program counters per process  
  - Multiple locations can execute at once 
    - **Multiple threads of control** -> **threads** 
- Must then have storage for thread details, multiple program counters in PCB 
- See next chapter
- 어떤 프로그램에 프로그램 궤적이 여러 개가 있다면??
- 이 각각의 궤적을 thread라고 부른다!!

<br>

## Process Representation in Linux

- Represented by the C structure

```c
// task_struct
pid t pid; /* process identifier */
long state; /* state of the process */
unsigned int time slice /* scheduling information */
struct task struct *parent; /* this process’s parent */
struct list head children; /* this process’s children */
struct files struct *files; /* list of open files */
struct mm struct *mm; /* address space of this pro */
```

![image-20220907235228970](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235228970.png)



<br>

## Process Scheduling

- Maximize CPU use, quickly switch processes onto CPU for time sharing 
- **Process scheduler** selects among available processes for next execution on CPU 
- Maintains scheduling queues of processes 
  - **Job queue** – set of all processes in the system 
    - HDD에서 main memory로 탑재를 기다리는 프로그램들의 줄
  - **Ready queue** – set of all processes residing in main memory, ready and waiting to execute 
  - **Device queues** – set of processes waiting for an I/O device 
  - Processes migrate among the various queues



<br>

## Ready Queue And Various I/O Device Queues

![image-20220907235341626](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235341626.png)



<br>

## Representation of Process Scheduling

![image-20220907235401030](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235401030.png)



<br>

## Schedulers

- **Long-term scheduler** (or job scheduler) 
  - selects which processes should be brought into the ready queue. 
  - Long-term scheduler is invoked **infrequently** (seconds, minutes) => (may be slow) 
  - The long-term scheduler controls the **degree of multiprogramming**
    - If too many jobs with a lot of I/O, 
      - CPU utilization may be low, 
      - processes mainly blocked 
    - If too many jobs which are mainly computation 
      - Low utilization of I/O devices 
    - 그래서 I/O와 CPU 사용을 적절히 융화하는 것이 long tern scheduler의 일
  - Processes can be described as either: 
    - I/O-bound process(I/O를 많이 하는 process) – spends more time doing I/O than computations, many short CPU bursts.
    - CPU-bound process(CPU 사용을 많이 하는 process) – spends more time doing computations; few very long CPU bursts. 
  - Long-term scheduler strives for good **process mix**
  
- **Short-term** scheduler (or CPU scheduler) 
  - selects which **ready process** should be executed next and allocates CPU 
  - Sometimes the only scheduler in a system 
  - Short-term scheduler is invoked **very frequently** (milliseconds) 
    - => (must be fast).
    - multi-programming 환경이기 때문에 매우 빈번하게 실행됨.

<br>

## Addition of Medium Term Scheduling

![image-20220907235624346](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235624346.png)

- swap 하는 기능이 추가됨
  
- 부분적으로 실행되는 process를 하드디스크로 보내버림
  
- **Medium-term scheduler** <u>can be added if degree of multiple programming needs to decrease</u> 
  - degree of multiple programming이 높으면 throughput이 좋아지지만 너무 많아지면 오히려 낮아지는데 context switching overhead 가 너무 자주 일어나기 때문이다
    - So, Sometimes system load needs to be readjusted 
  - **Remove process from memory, store on disk, bring back in from disk to continue execution: swapping**
  
- Some systems do not have

- 시스템 load가 줄어들 때까지 메모리에서 프로세스를 harddisk에 저장을 하고 죽인다. 

<br>

## Multitasking in Mobile Systems

- Some mobile systems (e.g., early version of iOS) allow only one process to run, **others suspended** 
- Due to screen real estate, user interface limits iOS provides for a
  - Single **foreground** process- controlled via user interface(ui) 
  - Multiple **background** processes– in memory, running, but not on the display, and with limits 
  - Limits include single, short task, receiving notification of events, specific long-running tasks like audio playback 
- Android runs foreground and background, with fewer limits 
  - Background process uses a **service** to perform tasks 
  - Service can keep running even if background process is suspended 
  - Service has no user interface, small memory use



<br>

## Context Switch

- When CPU switches to another process, the system must 1) **save the state of the old process** and 2) load the **saved state** for the new process via a **context switch** & 3) restart new process 
- **Context** of a process represented in the PCB 
- Context-switch time is overhead; the system does no useful work while switching 
  - The more complex the OS and the PCB -> the longer the context switch 
- Time dependent on hardware support 
  - Some hardware provides multiple sets of registers per CPU -> **multiple contexts loaded at once**



<br>

## Operations on Processes(매우 중요)

- System must provide mechanisms for
  - process **creation**, 
  - Process **termination**, 
  - and so on as detailed next



<br>

## Process Creation

- **Parent** process create **children** processes, which, in turn create other processes, forming a tree of processes 
- Generally, process identified and managed via a **process identifier (pid)** 
- Resource sharing options - design issue
  - Parent and children share **all** resources. 
    - 부모 자식 간은 모든 자원을 공유한다.
  - Children share **subset** of parent’s resources. 
  - Parent and child share **no** resources. 
- Execution options 
  - Parent and children execute concurrently. 
  - Parent waits until children terminate

- 모든 프로세스는 init process의 자손이다.

<br>

## A tree of Processes in Linux

![image-20220908000144448](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000144448.png)



<br>

## Process Creation (Cont.)

- Address space 
  - Child duplicate of parent. 
  - Child has a program(new program) loaded into it. 

- UNIX examples 
  - **fork** system call creates new process 
  - **exec** system call used after a **fork** to `replace the process’ memory space with a new program.` 
    - 프로세스의 메모리 공간을 새로운 프로그램으로 대치한다.
  - m = fork()
    - m값은 parent도 받고 child도 받는데 그 값은 다르다.
      - child한테는 0을, parent 한테는 child의 pid


```
if (fork() != 0) /* parent code */ 
	{ code to be executed by parent} 
else 
	{ exec(code image) }
```

![image-20220908000403071](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000403071.png)



<br>

## C Program Forking Separate Process

```c
#include <sys/types.h>
#include <studio.h>
#include <unistd.h>
int main() {
    pid_t pid;
    
    /* fork another process */
    pid = fork(); // thread control이 두 개가 됨
    
    if (pid < 0) { /* error occurred */
        fprintf(stderr, "Fork Failed");
        return 1;
    }
    else if (pid == 0) { /* child process */
        execlp("/bin/ls", "ls", NULL);
    }
    else { /* parent process */
        /* parent will wait for the child */
        wait (NULL);
        printf ("Child Complete");
    }
    return 0;
}
```



<br>

## Creating a Separate Process via Windows API

![image-20220908000502407](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000502407.png)

<br>

## Process Creation (Cont.)

- **Shell** : process which is command interpreter 
  - Get command  
  - Execute command (fork a child, wait for a child to finish) 
  - Go back to get command

```c
While (true) {
     display prompt
     read command (command, params)
     if (fork() != 0) then{ wait(&status); exit() }
     else { execve(command, parmas) }
```

shell이든 일반적인 프로그램에서 fork를 하든 동작원리는 같다.

<br>

## Process Termination

- Process executes **last statement** and then asks the operating system to delete it using the **exit()** system call. 
  - Returns status data from child to parent (via **wait()**) 
  - Process’ resources are **deallocated** by operating system 
- Parent may terminate the execution of children processes using the **abort()** (비정상 종료)system call. Some reasons for doing so: 
  - abort()의 인자로 강제 종료 시킬 프로세스를 적는다.
  - 다른 프로세스가 나를 종료시키려 할 때 쓰는 것이 abort
  - Child has **exceeded** allocated resources 
  - Task assigned to child is **no longer required** 
  - The parent is **exiting** 
    - operating systems does not allow a child to continue if its parent terminates
      - <span style="color:red">부모 프로세스가 종료되면 그 아래 자손 프로세스들은 모두 종료된다.</span>
  
- Some operating systems do not allow child to exists if its parent has terminated. If a process terminates, then all its children must also be terminated. 
  - **cascading termination**. All children, grandchildren, etc. are terminated. 
    - 모든 자손이 종료
  - The termination is initiated by the operating system. 
- The parent process may wait for termination of a child process by using the wait() system call. The call returns status information and the pid of the terminated process 
  - pid = wait(&status); 
    - status: child process가 종료된 이유
    - pid: 종료되어진 child process의 pid



정상적인 방법으로 종료하지 못하는 프로세스

- If no parent waiting (did not invoke wait()) process is a **zombie** 
  - 만약 child process가 exit() 시스템콜로 종료했는데 parent process가 wait()으로 child process의 종료를 기다리지 않는 경우
  - 이 때 종료되지 않은 child process를 zombie process라고 함.
  - <span style="color:red">만약 child process가 exit() 시스템 콜을 호출하여 종료했는데 parent process가 이를 wait()으로 child process의 종료를 기다리지 않으면 이때 이 child process는 zombie process가 된다.</span>
  
- If parent terminated without invoking wait , process is an **orphan(고아)**
  - parent가 wait()을 호출하지 않고 그냥 종료해 버린 경우
  - wait()을 하지 않으면 parent가 자식이 종료되지 않았는데 그냥 종료되어 버릴 수 있는 것!
  - <span style="color:red">parnet process가 child process가 아직 종료되지 않았는데 wait() 호출하지도 않고 그냥 종료되면 이때 child process는 orphan process가 된다.</span>

**정리**

- 부모 프로세스가 자식 프로세스보다 먼저 종료되면 orphan process
- 자식 프로세스가 종료되었지만 부모 프로세스가 자식 프로세스의 종료를 회수하지 않는 경우 자식 프로세스는 zombie process



<br>

## Multiprocess Architecture - Chrome Browser

- Many web browsers ran as **single process** (some still do) 
  - If one web site causes trouble, entire browser can hang or crash 
    - 한 사이트에 문제가 생긴 것이 다른 모든 사이트에 영향을 줌
- Google Chrome Browser is multiprocess with **3 categories** 
  - **Browser process** manages user interface, disk and network I/O 
  - **Renderer process** renders web pages, deals with HTML, Javascript, new one for each website opened 
    - Runs in sandbox restricting disk and network I/O, minimizing effect of security exploits 
    - Sandbox: a virtual container in which untrusted programs can be safely run 
  - **Plug-in process** for each type of plug-in (ex: QuickTime….)

![image-20220908000853304](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000853304.png)

<br>

## Interprocess Communication(IPC)

- Processes within a system may be **independent** or **cooperating** 
- **Independent** process cannot affect or be affected by the execution of another process 
- **Cooperating process** can affect or be affected by the execution of another process , including **sharing data** 
  - 공유데이터처럼 영향을 끼치거나 받는 프로세스

- **Reasons** for cooperating processes: 
  - Information sharing 
  - Computation speedup 
  - Modularity 
  - Convenience 
- Cooperating processes need **interprocess communication (IPC)** 
- **Two models** of IPC 
  - Shared memory 
  - Message passing



<br>

## Communications Models

![image-20220908001020013](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908001020013.png)

- shared memory

  - process가 공유하는 메모리가 있음

  - shared memory가 만들어질 때는 OS의 도움을 받지만 그 이후는 OS의 도움을 받지 않는다.

- MPP
  - **메세지를 보낼 때마다 OS의 도움을 받는다.**
  - kernel에 메세지를 보내고 이를 읽고의 과정
  - send

<br>

## Producer-Consumer Problem (SM; shared memory)

**example**

- Paradigm for cooperating processes, **producer process** produces information that is consumed by a **consumer process**. 
  - <u>unbounded-buffer</u> places no practical limit on the size of the buffer. 
    - consumer may have to wait for new items, but the producer can always produce new items 
  - <u>bounded-buffer</u> assumes that there is a fixed buffer size. 
    - consumer must wait if the buffer is empty 
    - producer must wait if the buffer is full



<br>

## Bounded-Buffer - Shared-Memory Solution

- Shared data

```c
#define BUFFER_SIZE 10
typedef struct {
    . . .
} item;
item buffer[BUFFER_SIZE];

// 배열 slot의 index
int in = 0;
int out = 0;
```

- Solution is correct, but can only use BUFFER_SIZE-1 elements
  - 버퍼에 저장할 공간이 실제로 하나가 남아있다 하더라도 이에 저장하게 되면 empty와 full을 구분할 수 없기 때문에 실제 버퍼 사이즈보다 하나 적게 사용가능하다.




<br>

## Bounded-Buffer - Producer

```c
item next_produced;
while (true) {
    /* produce an item in next produced */
    while (((in + 1) % BUFFER_SIZE) == out) // is buffer full?
        ; /* do nothing */
    buffer[in] = next_produced;
    in = (in + 1) % BUFFER_SIZE;
} 
```

- in == out은 buffer가 비어있다는 것을 의미함.
- in과 out은 적어도 두 칸 이상 차이가 나야지 buffer에 내용을 넣을 수 있음.

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

- in: 새로운 데이터를 쓸 위치
- out: consumer에서 데이터를 가져올(꺼내올) 위치



<br>

## Interprocess Communication - Shared Memory

- An area of memory shared among the processes that wish to communicate 
- The communication is under the `control of the user processes` **not the operating system** 
  - OS가 아니라 user process의 control 하에 통신이 이루어짐

- Major issue is to provide mechanism that will allow the user processes **synchronize** their actions when they access shared memory.
- IPC using SM requires communicating processes to establish a region of shared memory 
- shared memory resides in the address space of the process creating the shared memory segment 
- Other processes that wish to communicate using this SM segment **must attach it to their address space**



<br>

## Interprocess Communication – Message Passing

- Mechanism for processes to **communicate** and to **synchronize** their actions. 
  - processes communicate with each other without resorting(의지) to shared variables. 
  - Useful in a distributed environment 
- Message passing facility provides two operations: 
  - send(message) - message size fixed or variable 
  - receive(message) 
- The message size is either fixed or variable 
  - Using **fixed-sized message**, implementation of OS is simple, application programming is difficult 
  - Using **variable-sized message**, implementation of OS is complex, application programming is simple

- send(), receie()

<br>

## Message Passing (Cont.)

- If P and Q wish to communicate, they need to: 
  - establish a **communication link** between them 
  - exchange messages via send/receive 
- Implementation issues: 
  - How are links established?  
  - Can a link be associated with more than two processes? 
  - How many links can there be between every pair of communicating processes?
  - What is the capacity of a link? 
  - Is the size of a message that the link can accommodate fixed or variable? 
  - Is a link unidirectional or bi-directional?



<br>

- Implementation of communication link 
  - Physical: 
    - Shared memory 
    - Hardware bus 
    - Network
  - Logical: 
    - Direct or indirect (**Naming**) 
      - 수신자를 명확히 할 것인가 불명확히 할 것인가
    - Symmetric or asymmetric communication (**Symmetry**) 
    - Synchronous or asynchronous (**Synchronization**) 
      - async - 상대방의 수신여부와 상관없이 막 보내는
    - Automatic or explicit buffering (**Buffering**)



<br>

## Naming Direct Communication

- Processes must name each other **explicitly**: 
  - **send** (P, message) - send a message to process P 
  - **receive** (Q, message) - receive a message from process Q 
- Properties of communication link 
  - Links are established **automatically**. 
  - A link is associated with exactly **one pair** of communicating processes. 
  - Between each pair there exists exactly one link. 
  - The link may be **uni**directional, but is usually **bi**-directional. 

![image-20220909204018656](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909204018656.png)

- Shared memory와 달리 message passing은 OS가 synchronize 문제를 책임지고 해결해야 함.

<br>

## Direct Communication

- Exhibits **symmetry** in addressing 
  - **Both** the sender and the receiver processes **must name** the other to communicate 
- A variant (**Asymmetry**) 
  - Only the sender names the recipient; recipient is not required to name the sender 
  - **send** (P, message) : send a message to process P 
  - **receive**(id, message) : receive a message from any process; id is set to the name of the process with which communication has taken place
    - 어떤 프로세스가 보냈는지 해당 프로세스의 id가 담긴다.



<br>

## Naming Indirect Communication

- Messages are directed and received from mailboxes (also referred to as ports). 
  - Send(A, msg), receive(A, msg) 
    - A는 pid가 아닌 mailbox의 ID
  - **Each mailbox has a unique id**. 
  - A process can communicate with some other process via a number of different mail boxes 
  - Processes can communicate **only if they share a mailbox**. 
- Properties of communication link 
  - Link established only if processes share a common mailbox 
  - A link may be associated with more than two processes. 
  - Each pair of processes may share several communication links. 
  - Link may be unidirectional or bi-directional.

프로세스가 메시지를 보내는데, 메시지를 받을 수신자를 따로 명시하지 않는다.

<br>

## Indirect Communication (Continued)

- Operations 
  - create a new mailbox (port) 
  - send and receive messages through mailbox 
  - destroy a mailbox 
- Primitives are defined as: 
  - **send**(A, message) – send a message to mailbox A
  - **receive**(A, message) – receive a message from mailbox A



- Mailbox owned **by process** 
  - Mailbox is part of address space of the process 
  - Owner can only receive messages, user can only send messages 
- Mailbox owned **by kernel** 
  - Mailbox is not attached(종속) to any particular process 
  - Ownership can be passed to other processes 
  - Operations of mailbox 
    - create a new mailbox (owner) 
    - send (owner) and receive (user) messages through mailbox 
    - destroy a mailbox



- Mailbox sharing 
  - P1 , P2 , and P3 share mailbox A. 
  - P1 , sends; P2 and P3 receive. 
  - **Who gets the message?** 
- Solutions 
  - Allow a link to be associated with at most two processes. 
  - Allow only one process at a time to execute a receive operation. 
  - Allow the system to select arbitrarily the receiver. Sender is notified who the receiver was.

<br>

## Synchronization

- Message passing may be either blocking or non-blocking. 
- **Blocking** is considered **synchronous** 
  - Blocking **send** has the sender block(waiting) until the message is received 
    - 내가 원하는 프로세스가 메시지를 받을때까지 Block 되어 있는 것
  - Blocking **receive** has the receiver block until a message is available 
    - 어떠한 메시지가 도착 할 때까지 Block 되어 있는 것
- **Non-blocking** is considered **asynchronous** 
  - Non-blocking send has the sender send the message and continue 
  - Non-blocking receive has the receiver receive a valid message or null message 
    - 메세지가 안 왔으면 그냥 널메세지를 받았다고 하고 리턴
    - 제대로 수신이 되었는 지를 사용자가 확인해야 함.
- send and receive primitives may be either blocking or non-blocking. 
  - Blocking send 
  - Nonblocking send 
  - Blocking receive 
  - Nonblocking receive 
- When both the send and receive are blocking -> rendezvous scheme(랑데뷰 sheme)
  - end, receive 는 꼭 먼저 호출한 것이 끝나야 그 다음 것을 send or receive 할 수 있다.




- Producer-consumer becomes trivial

message next produced;

```c
while (true) {
 /* produce an item in next produced */
 send(next produced);
} 
```



message next consumed;

```c
while (true) {
 receive(next consumed);
 /* consume the item in next consumed */
}
```



non-blocking의 경우 위 코드로 동작하지 않음 (기다리는 기능이 있기 때문)



<br>

## Buffering

- Queue of messages attached to the link; implemented in one of three ways.

1. Zero capacity – 0 messages Sender must wait for receiver (rendezvous, synchronous). 

2. Bounded capacity – finite length of n messages Sender must wait if link full. (Asynchronous) 

3. Unbounded capacity – infinite length Sender never waits.



| Process P       | Process Q       |
| --------------- | --------------- |
| Send(Q, msg)    | receive(P, msg) |
| Receive(Q, msg) | send(P, ACK)    |



<br>

## Examples of IPC Systems - POSIX

- POSIX provides both of Message Passing and Shared Memory 
- POSIX Shared Memory 
- POSIX SM is organized using **memory-mapped file**, which associate the region of SM with a file 
  - Process first creates shared memory segment 
    - `shm_fd = shm_open(name, O CREAT | O RDRW, 0666);` 
  - Also used to open an existing segment to share it 
  - Set the size of the object 
    - `ftruncate(shm_fd, 4096);`
  - **Establish a memory-mapped file containg** shared memory segment using `mmap( ) `
  - Now the process could write to the shared memory 
    - `sprintf(shared memory, "Writing to shared memory");`



<br>

## IPC POSIX Producer

![image-20220909205059261](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909205059261.png)





<br>

## IPC POSIX Consumer

![image-20220909205123387](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909205123387.png)



<br>

## An example of IPC: Mach

MPP 예

- Mach developed at CMU 

- Task : similar to multi-threaded-process 

- Most communication (system call or intertask information) is done by message using mailbox 

  - Even system calls are messages 
  - Each task gets two mailboxes at creation- Kernel and Notify 
    - Kernel mailbox: kernel communicate with task through this 
    - Notify mailbox : kernel sends notification of event occurrence 
  - Only **three system calls** needed for message transfer 
    - `msg_send(), msg_receive(), `
    - `msg_rpc()`: sends a message and waits for exactly one return message from the sender 
  - Mailboxes needed for commuication, created via 
    - port_allocate()

  - Send and receive are flexible, for example four options if mailbox full: 
    - Wait indefinitely 
    - Wait at most n milliseconds 
    - Return immediately 
    - Temporarily cache a message



<br>

## Examples of IPC Systems - Windows XP(생략)

- Message-passing centric via **local procedure call (LPC)** facility 
  - Only works between processes on the same system 
  - Uses ports (like mailboxes) to establish and maintain communication channels 
  - Communication works as follows: 
    - The client opens a handle to the subsystem’s **connection port** object. 
    - The client sends a connection request. 
    - The server creates two **private communication ports** and returns the handle to one of them to the client. 
    - The client and server use the corresponding port handle to send messages or callbacks and to listen for replies.



<br>

## Local Procedure Calls in Windows XP(생략)

![image-20220909205521746](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909205521746.png)





<br>

분산 어플리케이션의 예 client server model

## Communications in Client-Server Systems

- Sockets – low level form of communication between distributed processes 
- Remote Procedure Calls (RPC)
- Pipes 
- Remote Method Invocation (Java)



<br>

## Sockets

- A socket is defined as an **endpoint** for communication. 
- Concatenation of IP address and port number 
- The socket 161.25.19.8:1625 refers to port 1625 on host 161.25.19.8 
- Communication consists between a **pair of sockets.** 
- Three types of sockets 
  - **Connection-oriented (TCP)** 
  - **Connectionless (UDP)** 
  - MulticastSocket class– data can be sent to multiple recipients 
- All ports below 1024 are well known, used for standard services 
- Special IP address 127.0.0.1 (**loopback**) to refer to system on which process is running



<br>

## Socket Communication

![image-20220909205747581](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909205747581.png)



- Communication using socket is considered a **low-level form** of communication between distributed processes 
- Socket allows only an **unstructured stream of bytes** to be exchanged 
- It is the responsibility of server/client to impose a structure on the data



<br>

## Date Client

```java
public class DateClient {
    public static void main(String[] args) {
        try { // this could be changed to an IP name or address other than the localhost
            Socket sock = new Socket("127.0.0.1",6013);
            InputStream in = sock.getInputStream();
            BufferedReader bin = new BufferedReader(new InputStreamReader(in));
            String line;
            while( (line = bin.readLine()) != null)
                System.out.println(line);
            sock.close();
        }
        catch (IOException ioe) {
            System.err.println(ioe);
        }
    }
}
```



<br>

## Date Server

```java
public class DateServer{
    public static void main(String[] args) {
        try {
            ServerSocket sock = new ServerSocket(6013); // now listen for connections
            while (true) {
                Socket client = sock.accept();
                // we have a connection
                PrintWriter pout = new PrintWriter(client.getOutputStream(), true);
                // write the Date to the socket
                pout.println(new java.util.Date().toString());
                client.close();
            }
        }
        catch (IOException ioe) {
            System.err.println(ioe);
        }
    }
}
```



<br>

## Remote Procedure Calls(RPC)

- Remote procedure call (RPC) abstracts procedure calls between processes on networked systems. 
  - Again uses ports for service differentiation 
  - Messages exchanged are well structured(unlikely port) and are not no longer just packets of data 
  - Each message is addressed to an **RPC daemon** listening to a **port** on the remote system 
    - RPC daemon이 계속해서 port를 들여다 봐서 메세지가 왔는지를 확인한다.
  - Each message contains an **id**. 
    - Specifying the function to execute, and parameters 
  - The function is executed as requested, and any out is sent back in a separate message 
    - RPC message를 보낸 process에게 다시 응답.
- **Port** – used to service differentiation

- **Stubs** – client-side proxy for the actual procedure on the server. 
- RPC마다 하나씩 존재
  - Separate stub for each separate remote procedure 
  - The client-side stub locates the server and **marshalls**(전송 format으로 바꿔주는 역할) the parameters. 
  - The server-side stub receives this message, **unpacks the marshalled** parameters, and performs the procedure on the server. 
  - On Windows, stub code compile from specification written in **Microsoft Interface Definition Language (MIDL)**

- Data representation handled via **External Data Representation (XDL)** format to account for different architectures 

  - **Big-endian** and **little-endian** (marshalling, unmarshalling) 

- Semantics of RPC 

  - Remote communication has more failure scenarios than local 

  - Messages can be delivered **exactly once** rather than **at most once** 
    - 정확히 한번을 보낼 수 있다.

- OS typically provides a rendezvous (or **matchmaker**) service to connect client and server



<br>

## Execution of RPC

![image-20220909210100852](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210100852.png)

- client가 server 쪽의 함수를 호출하려고 하는 경우
  - procedure X에게 RPC를 보내달라고 요구
  - kernel에서 matchmaker에게 메세지를 보냄
    - X라는 procedure의 port번호를 알려줘!
    - from: client | to: server | port: matchmaker
  - matchmaker가 port P(RPC X)를 찾아서 kernel(사용자)에게 다시 보냄
    - from: server | to: client | port: kernel
  - RPC의 포트번호를 알았으니 kernel은 RPC에게 메세지를 보낸다.
    - from: client | to: server | port: port P
  - RPC daemon이 port를 listen하고 있다가 메세지가 오면 이를 받는다.
  - daemon process가 X 함수를 실행한 결과를 message에 담아서 보내게 된다.
    - from: RPC | to: client | port: kernel







<br>

## Pipes

- Acts as a conduit allowing two processes to communicate 
- **Issues** 
  - Is communication unidirectional or bidirectional? 
  - In the case of two-way communication, is it half or full-duplex? 
  - Must there exist a relationship (i.e. parent-child) between the communicating processes? 
  - Can the pipes be used over a network? 
- Ordinary pipes – cannot be accessed from outside the process that created it. 
  - Typically, a parent process creates a pipe and uses it to communicate with a child process that it created. (same machime) 

- Named pipes – can be accessed **without a parent-child relationship**.



<br>

## Ordinary Pipes

- Ordinary Pipes allow communication in standard producer-consumer style 
- Producer writes to one end (the **write-end** of the pipe) 
- Consumer reads from the other end (the **read-end** of the pipe) 
- Ordinary pipes are therefore **unidirectional** 
- If two way communication is required, two pipes must be used 
- Require parent-child relationship between communicating processes 
  - Is used to communicate with a child process that it creates via fork() 
- Windows calls these **anonymous pipes** 
- See Unix and Windows code samples in textbook



<br>

## Ordinary Pipes

```c
Pipe ( int fd[] )
```

- Creates a pipe 
- fd[0] is the read-end of pipe, fd[1] is write-end

![image-20220909210445086](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210445086.png)



<br>

## Ordinary pipe in UNIX

![image-20220909210516531](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210516531.png)



<br>

![image-20220909210602247](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210602247.png)



<br>

## Named Pipes

- Named Pipes are more **powerful** than ordinary pipes 
- Communication is bidirectional, half-duplex 
  - For full-**diplex**, two **named** pipes are required 
  - Can be used in a single machine 
- No parent-child relationship is necessary between the communicating processes 
- Several processes can use the named pipe for communication 
-  Provided on both UNIX and Windows systems 
  - Named Pipe of Window provides full duplex, intra or inter machine communication 
- makefifo(), open(), read(), write(), close() for Unix 
- CreateNamedPipe(), ConnectNamed Pipe(), ReadFile(), WriteFile()



<br>

## Remote Method Invocation(RMI)

- Remote Method Invocation (RMI) is a Java mechanism similar to RPCs. 
- RMI allows a Java program on one machine to invoke a method on a remote object.

![image-20220909210741721](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210741721.png)



<br>

## RPC vs. RMI

- RPC support procedural programming; only remote procedures or functions are called 
- RMI is object-based; supports invocation of methods on remote objects 
- Parameters to remote procedures are ordinary data structures in RPC 
- Possible to pass objects as parameters to remote methods in RMI



<br>

## Marshalling Parameters

![image-20220909210827672](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210827672.png)





<br>

## Concurrency

- Concurrency in Clients 
  - Clients can be run on a machine either iteratively (running them one by one) or 
  - Concurrently (more than two clients can run at the same time) 
  - Iterative running means 
- Concurrency in Servers 
  - Iterative server 
  - Concurrent server



<br>

## Connectionless iterative server

![image-20220909210929495](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909210929495.png)



<br>

## Connection-oriented concurrent server

![image-20220909211000015](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211000015.png)

- TCP 사용

- child process는 well-known port가 필요없음
  - well-known port는 서버에 접속 하는 데만 필요하기 때문에

<br>

## Programs and processes

![image-20220909211018635](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211018635.png)

서로는 독립적인 process이기 때문에 정보를 공유하기 위해서는 IPC mechanism를 사용해야 함.

<br>

## A Program that prints its own processid

![image-20220909211132736](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211132736.png)



<br>

## A program with one parent and one child

![image-20220909211334384](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211334384.png)



<br>

## A program with two fork functions

![image-20220909211450970](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211450970.png)

fork()를 한 번 하면 두 갈래가 되고 또 한번 fork()를 하면 각 갈래에서 두 갈래씩 나오기 때문에 총 4갈래로 나눠짐.

<br>

## The output of the program in Figure 15.12

![image-20220909211623606](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211623606.png)



<br>

## A program that prints the processids of the parent and the child

![image-20220909211643635](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211643635.png)



<br>

## Example of a server program with parent and child processes

![image-20220909211707582](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211707582.png)



<br>

## TCP 서버 함수 (1/8)

- TCP 서버 함수

![image-20220909211740525](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211740525.png)



<br>

![image-20220909211811550](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220909211811550.png)

