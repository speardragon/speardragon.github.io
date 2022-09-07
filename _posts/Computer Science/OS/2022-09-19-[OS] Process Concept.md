---
layout: single
title: "[OS] Process Concept"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---



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
- Textbook uses the terms job and process almost interchangeably. 
- Process – a program in execution; process execution must progress in sequential fashion.  
- A process includes: 
  - Resources: timers, pending signals, open files, network connections, hardware, and IPC mechanisms 
  - state, and a virtualized processor



<br>

## The Multiple Parts (section) of a Process

- **Each process has following memory sections** 
  - **Text section** 
    - The program code & read-only data such as constant variables 
    - Current activity including **program counter**, processor registers 
  - **Stack section** containing temporary data 
    - Function parameters, return addresses, local variables 
  - **Data section** containing global variables 
  - **Heap section** containing memory dynamically allocated during run time



<br>

## Process in Memory

![image-20220907234751229](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234751229.png)



<br>

## Process Concept

- Program is passive entity stored on disk (executable file), process is active 
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

![image-20220907234908196](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234908196.png)



<br>

## Process Control Block (PCB)

Information associated with each process. 

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
- - I/O devices allocated to process, list of open files



<br>

## CPU Switch From Process to Process

![image-20220907235102206](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235102206.png)



<br>

## Threads

- So far, process has a single thread of execution 
- Consider having multiple program counters per process  
  - Multiple locations can execute at once 
    - Multiple threads of control -> threads 
- Must then have storage for thread details, multiple program counters in PCB 
- See next chapter



<br>

## Process Representation in Linux

- Represented by the C structure

```c
task_struct
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
- Process scheduler selects among available processes for next execution on CPU 
- Maintains scheduling queues of processes 
  - Job queue – set of all processes in the system 
  - Ready queue – set of all processes residing in main memory, ready and waiting to execute 
  - Device queues – set of processes waiting for an I/O device 
  - Processes migrate among the various queues



<br>

## Ready Queue And Various I/O Device Queues

![image-20220907235341626](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235341626.png)



<br>

## Representation of Process Scheduling

![image-20220907235401030](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235401030.png)



<br>

## Schedulers

- Long-term scheduler (or job scheduler) 
  - selects which processes should be brought into the ready queue. 
  - Long-term scheduler is invoked infrequently (seconds, minutes) => (may be slow) 
  - The long-term scheduler controls the degree of multiprogramming
    - If too many jobs with a lot of I/O, 
      - CPU utilization may be low, 
      - processes mainly blocked 
    - If too many jobs which are mainly computation 
      - Low utilization of I/O devices 
  - Processes can be described as either: 
    - I/O-bound process – spends more time doing I/O than computations, many short CPU bursts.
    - CPU-bound process – spends more time doing computations; few very long CPU bursts. 
  - Long-term scheduler strives for good **process mix**

- Short-term scheduler (or CPU scheduler) 
  - selects which ready process should be executed next and allocates CPU 
  - Sometimes the only scheduler in a system 
  - Short-term scheduler is invoked very frequently (milliseconds) 
    - => (must be fast).

<br>

## Addition of Medium Term Scheduling

![image-20220907235624346](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907235624346.png)

- Medium-term scheduler can be added if degree of multiple programming needs to decrease 
  - Sometimes system load needs to be readjusted 
  - Remove process from memory, store on disk, bring back in from disk to continue execution: swapping

- Some systems do not have



<br>

## Multitasking in Mobile Systems

- Some mobile systems (e.g., early version of iOS) allow only one process to run, others suspended 
- Due to screen real estate, user interface limits iOS provides for a
  - Single foreground process- controlled via user interface 
  - Multiple background processes– in memory, running, but not on the display, and with limits 
  - Limits include single, short task, receiving notification of events, specific long-running tasks like audio playback 
- Android runs foreground and background, with fewer limits 
  - Background process uses a service to perform tasks 
  - Service can keep running even if background process is suspended 
  - Service has no user interface, small memory use



<br>

## Context Switch

- When CPU switches to another process, the system must 1) save the state of the old process and 2) load the saved state for the new process via a context switch & 3) restart new process 
- Context of a process represented in the PCB 
- Context-switch time is overhead; the system does no useful work while switching 
  - The more complex the OS and the PCB -> the longer the context switch 
- Time dependent on hardware support 
  - Some hardware provides multiple sets of registers per CPU -> multiple contexts loaded at once



<br>

## Operations on Processes

- System must provide mechanisms for
  - process creation, 
  - Process termination, 
  - and so on as detailed next



<br>

## Process Creation

- Parent process create children processes, which, in turn create other processes, forming a tree of processes 
- Generally, process identified and managed via a process identifier (pid) 
- Resource sharing options 
  - Parent and children share all resources. 
  - Children share subset of parent’s resources. 
  - Parent and child share no resources. 
- Execution options 
  - Parent and children execute concurrently. 
  - Parent waits until children terminate



<br>

## A tree of Processes in Linux

![image-20220908000144448](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000144448.png)



<br>

## Process Creation (Cont.)

- Address space 
- Child duplicate of parent. 
- Child has a program loaded into it. 
- UNIX examples 
- fork system call creates new process 
- exec system call used after a fork to replace the process’ memory space with a new program. 

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
    pid = fork();
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

- Shell : process which is command interpreter 
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



<br>

## Process Termination

- Process executes last statement and then asks the operating system to delete it using the exit() system call. 
  - Returns status data from child to parent (via wait()) 
  - Process’ resources are deallocated by operating system 
- Parent may terminate the execution of children processes using the abort() system call. Some reasons for doing so: 
  - Child has exceeded allocated resources 
  - Task assigned to child is no longer required 
  - The parent is exiting 
    - operating systems does not allow a child to continue if its parent terminates

-  Some operating systems do not allow child to exists if its parent has terminated. If a process terminates, then all its children must also be terminated. 
  - cascading termination. All children, grandchildren, etc. are terminated. 
  - The termination is initiated by the operating system. 
- The parent process may wait for termination of a child process by using the wait()system call. The call returns status information and the pid of the terminated process 
  - pid = wait(&status); 
- If no parent waiting (did not invoke wait()) process is a zombie 
- If parent terminated without invoking wait , process is an orphan

<br>

## Multiprocess Architecture - Chrome Browser

- Many web browsers ran as single process (some still do) 
  - If one web site causes trouble, entire browser can hang or crash 
- Google Chrome Browser is multiprocess with 3 categories 
  - Browser process manages user interface, disk and network I/O 
  - Renderer process renders web pages, deals with HTML, Javascript, new one for each website opened 
    - Runs in sandbox restricting disk and network I/O, minimizing effect of security exploits 
    - Sandbox: a virtual container in which untrusted programs can be safely run 
  - Plug-in process for each type of plug-in (ex: QuickTime….)

![image-20220908000853304](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908000853304.png)

<br>

## Interprocess Communication

- Processes within a system may be independent or cooperating 
- Independent process cannot affect or be affected by the execution of another process 
- Cooperating process can affect or be affected by the execution of another process , including sharing data 
- Reasons for cooperating processes: 
  - Information sharing 
  - Computation speedup 
  - Modularity 
  - Convenience 
- Cooperating processes need interprocess communication (IPC) 
- Two models of IPC 
  - Shared memory 
  - Message passing



<br>

## Communications Models

![image-20220908001020013](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220908001020013.png)



<br>

## Producer-Consumer Problem (SM)

- Paradigm for cooperating processes, producer process produces information that is consumed by a consumer process. 
  - unbounded-buffer places no practical limit on the size of the buffer. 
    - consumer may have to wait for new items, but the producer can always produce new items 
  - bounded-buffer assumes that there is a fixed buffer size. 
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
int in = 0;
int out = 0;
```

- Solution is correct, but can only use BUFFER_SIZE-1 elements



<br>

## Bounded-Buffer - Producer

```c
item next_produced;
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

## 

36p~