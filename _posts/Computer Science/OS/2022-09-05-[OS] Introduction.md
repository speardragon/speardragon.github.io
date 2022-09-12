---
layout: single
title: "[OS] Introduction"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---



## What is an Operating System?

- A program that acts as an intermediary between a user of a computer and the computer hardware
- Operating system goals:
  - Execute user programs and make solving user problems **easier**
  - Provide an execution environment in which a user can execute programs
    - Make the computer system convenient to use (convenience)
    - Use the computer hardware in an efficient manner (efficiency)



<br>

## Computer System Structure

- Computer system can be divided into four components: 
  - Hardware – provides basic computing resources 
    - CPU, memory, I/O devices 

  - Operating system 
    - Controls and coordinates use of hardware among various applications and users 

  - Application programs – define the ways in which the system resources are used to solve the computing problems of the users 
    - Word processors, compilers, web browsers, database systems, video games 

  - Users 
    - People, machines, other computers


​	

<br>

## Four Components of a Computer System

![image-20220905144132620](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905144132620.png)

<br>

## What Operating System Do

- Depends on the point of view 
- Users want convenience, **ease of use** and **good performance** 
  - Don’t care about **resource utilization** 
- But shared computer such as **mainframe** or **minicomputer** must keep all users happy 
  - 한 사용자만 만족시키는 것이 아니라 다른 많은 모든 사람에게 만족할만하도록
- Users of dedicate systems such as **workstations** have dedicated resources but frequently use shared resources from **servers** 
- Handheld computers are resource poor, optimized for usability and battery life 
- Some computers have little or no user interface, such as embedded computers in devices and automobiles



<br>

## Operating System Definition

- OS is a **resource allocator** 
  - Manages all resources 
  - Decides between conflicting requests for efficient and fair resource use 
- OS is a **control program** 
  - Controls execution of programs to prevent errors and improper use of the computer 
- “The one program running at all times on the computer” is the **kernel**. 
- Everything else is either 
  - a system program (ships with the operating system) , or 
  - an application program.



<br>

## Computer System Organization

- Computer-system operation
  - One or more CPUs, device controllers connect through common bus providing access to shared memory 
  - Concurrent execution of CPUs and devices competing for memory cycles (memory controller)

![image-20220905150720928](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905150720928.png)

<br>

## Computer-System Operation

- I/O devices and the CPU can execute concurrently 
- Each device controller is in charge of a particular device type 
- Each device controller has a local buffer 
- CPU moves data from/to main memory to/from local buffers 
- I/O is from the device to local buffer of controller 
- Device controller informs CPU that it has **finished** its operation by causing an **interrupt**



<br>

## Computer Startup

- **bootstrap program** is loaded at power-up or reboot 
  - bootstrap program: OS를 탑재시키는 프로그램(ROM에 저장되는 firmware)
  - Typically stored in ROM or EPROM, generally known as **firmware** 
  - Initializes all aspects of system 
  - Loads operating system kernel and starts execution 
- After OS starts, it waits for the event. 
  - OS는 능동적인 애가 아니라 외부에서 어떠한 서비스 요청이 들어왔을 때 반응하기에 이를 기다린다.
  - Event driven system 
  - Occurrence of Event is signaled by (H/W or S/W) interrupt 
    - power failed -> hardware interrupt



<br>

## Common Functions of Interrupts

- Interrupt transfers control to the interrupt service routine, through the **interrupt vector**, which contains the addresses of all the interrupt service routines (ISR) 
- Interrupt architecture must save the address of the interrupted instruction - ISR 실행 후 리턴을 위해
  - 반드시 interrupt 됐을 당시 address를 save를 해 놔야 처리 후 다시 돌아갈 수 있음

![image-20220905150851764](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905150851764.png)



- A **trap** or **exception** is a software-generated interrupt caused either by an error or a user request 
- An operating system is **interrupt driven**

- system call - OS API
  - 사용이 kernel 모드에서 일어남
- library 함수(printf와 같은)는 User mode 에서만 사용
- trap: OS 함수를 호출하기 위해서 발생되는 (software) interrupt
  - trap마다 번호가 있어 해당하는 trap handler 실행 후 리턴
- exception - divide by zero: 프로그램에서 각종 error가 발생할 때의 software interrupt

<br>

## Interrupt Handling

- The operating system preserves the state of the CPU by storing registers and the **program counter** (ISR 실행후 리턴시 원래 snapshot 복구) 
  - running snapshot
- Determines which type of interrupt has occurred: 
  - **polling** 
    - CPU에 interrupt가 걸렸다는 사실만 알려지고 누가 건지는 모름
    - 초창기에 사용
  - **vectored** interrupt system 
    - 어느 device가 interrupt를 걸면 CPU가 OK라는 응답을 보내고, 다시 device는 ok 사인에 대하여 interrupt vector를 memory에 write하는 것처럼 data bus에 실어 보낸다.
- Separate segments of code determine what action should be taken for each type of interrupt



<br>

## Interrupt Timeline

![image-20220905150948572](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905150948572.png)

I/O device가 IO를 하고 있는 동안에도 CPU 역시 일을 하고 있음.(concurrent)

transfer done 단계에서 interrupt가 있었을 것이고 CPU에서는 ISR이 실행되는 구간이 있음

<br>

## Storage Structure

- Main memory – only large storage media that the CPU can access directly 
  - **Random access** 
  - Typically **volatile** (power가 꺼지면 content가 날라감)
- Secondary storage(HDD, SDD) – extension of main memory that provides large **nonvolatile** storage capacity 
- Hard disks – rigid metal or glass platters covered with magnetic recording material 
  - Disk surface is logically divided into **tracks**, which are subdivided into **sectors** 
  - The **disk controller** determines the logical interaction between the device and the computer 
- **Solid-state disks** – faster than hard disks, nonvolatile  
  - Various technologies 
  - Becoming more popular



<br>

## Storage Hierarchy

- Storage systems organized in hierarchy 
  - Speed 
  - Cost 
  - Volatility 
- **Caching** – copying information into faster storage system; main memory can be viewed as a cache for secondary storage 
- **Device Driver** for each device controller to manage I/O 
  - Provides uniform interface between controller and kernel



<br>

## Storage-Device Hierarchy

![image-20220905151518709](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905151518709.png)

program을 실행시키기 위해서는 무조건 main memory에 탑재를 시켜야 함.

cache는 각 계층 사이마다 있을 수 있음

- 또한 여러 개가 이어져서 사용되기도 함

<br>

## Caching

- Important principle, performed **at many levels** in a computer (in hardware, operating system, software) 
- Information in use **copied** from slower to faster storage temporarily 
- Faster storage (cache) checked first to determine if information is there 
  - If it is, information used directly from the cache (fast)  
  - If not, data copied to cache and used there 
- Cache smaller than storage being cached 
  - **Cache management** is important design problem 
  - Cache size and replacement policy(무엇을 버릴지)



<br>

## I/O Structure

-  Computer-system consists of one or more CPUs and multiple device controllers connected through a common bus 
- A device controller includes local buffer storage and a set of special purpose registers 
- Device controller is responsible for moving data between the peripheral device it controls and its local buffer storage  
- OS have a **device driver** for each device controller 
  - 각 device driver(disk, keyboard...) 마다 대화가 가능한 device controller가 있고 끼리끼리만 대화 가능
  - device driver understands the device controller and provides OS with uniform interface to the device



- To start an **Interrupt driven I/O** operation, 
  - device driver loads the registers within device controller (command) 
  - device controller understands the commands by examining the contents of registers 
  - device controller transfers the data between device and local buffer 
  - Once data transfer is complete, device controller informs the device driver using interrupt (data transfer 종료 통보) 
  - Device driver returns control to OS, returning data or pointer to the data, status info.



<br>

## Direct Memory Access Structure

CPU의 간섭없이 device to/from memory read/write하는 방법

- Used for high-speed I/O devices able to transmit information at close to memory speeds 
- Device controller transfers blocks of data from buffer storage directly to main memory without CPU intervention 
- Only one interrupt is generated per block, rather than the one interrupt per byte



<br>

## How a Modern Computer Works

![image-20220905151927609](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905151927609.png)

폰노이만 구조를 해치지 않으면서 좀 더 효율적으로 read/write

<br>

## Von Neumann Architecture

- Stored Program concept 
  - Main memory storing programs and data 
  - Contents of memory are addressable by location 
  - Execution occurs sequentially (unless explicitly specified)



어떤 프로그램도 아래 세 가지를 벗어나지 않음

1. sequence
2. iteration
3. selection

<br>

## The Von Neumann Architecture

![image-20220905152032735](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152032735.png)

내부에서 고정적으로 사용되는 memory -> CPU regiseter

<br>

## Instruction Cycle

- Two steps: 
  - Fetch 
  - Execute

![image-20220905152102610](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152102610.png)

<br>
![image-20220905152112450](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152112450.png)



<br>

## Computer-System Architecture

- **Single-processor systems** 
  - Most systems use a single general-purpose processor 
  - Most systems have special-purpose processors as well (device specific) 
- **Multiprocessor systems** growing in use and importance 
  - Also known as <span style="color:red">parallel systems(multi-core)</span>, tightly-coupled systems 
  - Tightly-coupled system – processors share memory and a clock; communication usually takes place through the shared memory in close area(제한된 영역 내에서 공유되는) 
  - Advantages include:  
    - **Increased throughput** (Speedup) 
    - **Economy of** scale – cost less than multiple single-processor systems 
    - **Increased reliability** – graceful degradation or fault tolerance (duplication) 
  - Two types: 
    - **Asymmetric Multiprocessing** 
    - **Symmetric Multiprocessing**

- Symmetric multiprocessing (SMP) 
  - Each processor runs an **identical** copy of the operating system. 
  - Many processes can run at once without performance deterioration. 
  - Load balancing is important 
    - 예를 들어 채점을 두 명이서 나눠서 하는데 한 사람은 만 명에 대한 걸 하고 한 사람은 10명에 대한 채점을 한다면 밸런싱이 맞지 않아 비효율이 발생할 것이기 때문이다.  
  - Most modern operating systems support SMP 
- Asymmetric multiprocessing 
  - Each processor is assigned a **specific task**; **master** processor schedules and allocates work to **slave** processors. 
  - More common in extremely large systems

<br>

## Symmetric Multiprocessing Architecture

![image-20220905152305462](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152305462.png)

master-slave 관계가 아닌 동등한 관계

<br>

## A Dual-Core Design

- **Memory access model**: **UMA**(uniform memory access ) and **NUMA** architecture variations 
  - Multiprocessing increases computing power by adding CPUs 
  - Adding CPUs also increases amount of memory addressable 
- Multi-chip and **multicore** 
- Blade server systems 
  - Chassis containing multiple independent multiprocessor systems

![image-20220905152350693](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152350693.png)



<br>

## Clustered Systems

- 좀 더 독립성이 부여된 시스템들끼리 모여 있는 경우
- Another type of multiprocessor system 
- multiple **independent** nodes working together 
  - Each node may be a single-processor system or multi-core system 
  - **Loosely-coupled system**, closely linked via LAN (Ethernet) or InfiniBand  
  - Usually sharing storage via a **storage-area network (SAN)** 
  - Provides a **high-availability(HA)** service which survives failures 
    - **Asymmetric clustering** has one machine in hot-standby mode 
    - Sy**mmetric clustering** has multiple nodes running applications, monitoring each other 
  - Some clusters are for **high-performance computing (HPC)** 
    - Applications must be written to use parallelization 
  - Other forms of Clusters 
    - Parallel cluster or clustering over WAN 
    - Some have **distributed lock manager (DLM)** to avoid conflicting operations



<br>

## Clustered Systems

![image-20220905152503816](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152503816.png)

<br>

## Distributed Systems

- Distribute the computation among several physical processors. 
- **Loosely coupled system** – each processor has its own local memory; processors communicate with one another through various communications lines. 
- Advantages of distributed systems. 
  - Resources Sharing 
  - Computation speed up – load sharing 
  - Reliability 
  - Communications 
- Requires networking infrastructure. 
- Local area networks (LAN) or Wide area networks (WAN) 
- May be either client-server or peer-to-peer systems.



---

(생략)

- Network Operating System 
  - provides file sharing 
  - provides communication scheme 
  - runs independently from other computers on the network 
- Distributed Operating System 
  - less autonomy between computers 
  - gives the impression there is a single operating system controlling the network.

---

<br>

## Operating System Structure

- **Multiprogramming** (**Batch system**) needed for efficiency (non-preemptive multiprogramming)
  - Single user cannot keep CPU and I/O devices busy at all times 
  - Multiprogramming organizes jobs (code and data) so CPU always has one to execute 
  - A subset of total jobs in system is kept in memory (job pool)  
  - One job selected and run via **job scheduling** 
    - job scheduling: 누가 먼저 스케쥴링 될 것인지
  - When it has to wait (for I/O for example), OS switches to another job 
- **Timesharing** (**multitasking**) is logical extension in which CPU switches jobs so frequently that users can interact with each job while it is running, creating **interactive computing** 
  - **Response time** should be < 1 second 
  - Each user has at least one program executing in memory => **process** 
  - If several jobs ready to run at the same time => **CPU scheduling** 
  - If processes don’t fit in memory, **swapping** moves them in and out to run 
  - **Virtual memory** allows execution of processes not completely in memory



![image-20220905152750658](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152750658.png)

- Non-multiprogramming: 
  - 한 프로그램이 끝나고 다른 프로그램이 실행.
  - CPU가 일을 하지 않고 놀고 있는 구간이 존재(CPU Time의 낭비)
- Non-preemptive multiprogramming
  - CPU가 놀고 있는 구간을 활용하여 다른 프로그램을 실행시킴
  - CPU가 쉬지 않고 일을 하여 시스템의 효율이 올라감
  - CPU와 I/O device가 동시에 활용되는 경우가 생길 수 있음
  - I/O를 하지 않는 동안에 CPU를 굉장히 오래 붙잡고 있어도 크게 문제가 되지 않는다.
    - non preemptive - 강제로 빼앗지 않는
  - I/O를 하기 위해서 자발적으로 내놓지 않으면 다른 프로그램이 실행될 수 없음
- Preemptive multiprogramming - Time Quantum 부여
  - 어떤 프로그램이 CPU에서 실행이 되는데 CPU에서 실행될 수 있는 최대 시간이 바로 time quantum이다.
  - time quantum 시간 동안만 CPU를 사용하는 것을 허락한다.
  - I/O를 하지 않아도 CPU를 빼앗아서 동시에 프로그램을 실행시킬 수 있음
  - 만약 time quantum이 굉장히 커지게 되면 Non-preemptive 에 가까워 짐.
  - 만약 time quantum이 굉장히 작아지게 되면 interaction은 굉장히 올라가는 대신에 시스템 성능은 오히려 Non-preemptive 보다 안 좋아 지게 된다.
    - 그래서 적정한 값을 정하는 것이 중요함

<br>

## Memory Layout for Multiprogrammed System

![image-20220905152816091](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905152816091.png)

- 누구를 먼저 탑재시킬 지를 정하는 것이 job scheduling
- 탑재된 job들 중에서 누가 먼저 CPU를 차지할 지를 결정하는 것이 CPU scheduling
- swapping: 탑재된 job들 중에서 별로 시급하지 않은 job의 경우 hard disk job pool에 보내고(swap out) 더 중요한 job을 job pool(swap in)로부터 불러 들인다.

<br>

## Operating-System Operations

- **Interrupt driven** (hardware and software) 
  - Hardware interrupt by one of the devices (외부 디바이스에 의한)
  - Software interrupt (**exception** or **trap**): 
    - exception: Software error (e.g., division by zero) 
    - trap: Request for operating system service 
  - Other process problems include infinite loop, processes modifying each other or the operating system 
    - 정상적으로 실행되는 것처럼 보이지만 무한 루프를 돌고 있는 경우(interrupt 검출이 되지 않음)
    - 이를 잡아낼 수 있는 tool이 있어야 함(아래와 같이)
- Basic tools to Ensure correct operation of computer system 
  - Dual mode - user가 kernel의 권한을 행사하지 못하도록 하는 것
  - Privileged instruction 
  - Memory protection 
    - 프로그램이 OS 영역을 침범하거나 다른 프로그램을 침범하는 것을 catch하여 interrupt를 걸어 OS에 통보
  - Timer interrupt – infinite loops
    - 일정 시간이 경과하면 걸리는 interrupt
  
- **Dual-mode** operation allows OS to protect itself and other system components
  - **User mode** and **kernel mode** 
  - **Mode bit** provided by hardware (mode를 판단하기 위한 bit)
    - Provides ability to distinguish when system is running user code or kernel code 
    - Some instructions designated as **privileged**, only executable in kernel mode 
      - 일부는 user mode에서 실행되지 않음(시스템에 심각한 문제를 야기하는 것을 방지하기 위함)
    - System call changes mode to kernel, return from call resets it to user 
- Increasingly CPUs support multi-mode operations 
  - i.e. **virtual machine manager** (**VMM**) mode for guest **VMs**



<br>

## Transition from User to Kernel Mode

- Timer to prevent <u>infinite loop / process hogging resources</u> 
  - Timer is set to interrupt the computer after some time period 
  - Keep a counter that is decremented by the physical clock. 
  - Operating system set the counter (privileged instruction) 
  - When counter zero generate an interrupt -> 프로그램이 무한 루프를 돌고있다고 판단
    - OS 자원을 낭비, CPU time 낭비
  - Set up before scheduling process to regain control or **terminate** program that exceeds allotted time
    - 종료 시켜 컴퓨터 자원 회수

![image-20220905153023964](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905153023964.png)



<br>

## Process Management

program definition이 만들어내는 instance가 process(msword 창을 두 개 띄우는 경우 '하나의 program definition에서 두 개의 process가 생겼다'라고 한다.)

- <u>A process is a program in execution</u>. It is a unit of work within the system. Program is a **passive entity**, process is an **active entity**. 
  - 프로세스는 실제로 실행되고 있는 상태이기 때문에 active entity인 것.

- Process needs resources to accomplish its task 
  - CPU, memory, I/O, files 
  - Initialization data 
- Process termination requires reclaim of any reusable resources 
- `Single-threaded process` has one **program counter** specifying location of next instruction to execute 
  - 하나의 process에 하나의 program counter
  - program counter가 갖는 값의 궤적이 thread
  - Process executes instructions sequentially, one at a time, until completion 
- `Multi-threaded process` has one program counter per thread 
- Typically system has many processes, some user, some operating system running concurrently on one or more CPUs 
  - Concurrency by multiplexing the CPUs among the processes / threads
  - CPU가 여러개인 경우 -> parallel processing
  - CPU가 하나인 경우 -> pseudo parallel processing
    - CPU 하나를 공유하면서 차지해야 하기 때문



<br>

## Process Management Activities

The operating system is responsible for the following activities in connection with process management:

- Creating and deleting both user and system processes 
- Suspending and resuming processes 
  - 프로세스가 계속 실행되는 것이 아니라 I/O를 해야 하기 때문에 I/O가 한 번 실행되고(suspend) 종료될 때(resume)까지 CPU에 의해 실행이 될 수 없다.

- Providing mechanisms for process synchronization 
  - 동일한 계좌에 대한 동기화

- Providing mechanisms for process communication (IPC- inter process ?)
- Providing mechanisms for deadlock handling
  - 동기화를 하다가 잘못되면 두 프로세스 모두가 실행이 되지 않는 경우(둘 중 어느 프로세스도 이도저도 못하는 상황) 가 발생하는데 
  - 검출하여 해결하는 것이 process management의 역할




<br>

## Memory Management

- To execute a program all (**or part**) of the instructions must be in memory 
- All (**or part**) of the data **that is needed by the program** must be in memory. 
- Memory management determines what is in memory and when 
  - Optimizing CPU utilization and computer response to users 
- Memory management activities 
  - Keeping track of which parts of memory are currently being used and by whom 
  - Deciding which processes (or parts thereof) and data to move into and out of memory 
  - Allocating and deallocating memory space as needed



<br>

## Storage Management

secondary memory

- OS provides uniform, logical view of information storage (3차원적 주소를 1차원으로 생각할 수 있도록 해 주는 역할)
  - Abstracts physical properties to logical storage unit - **file** 
  - Each medium is controlled by device (i.e., disk drive, tape drive) 
    - Varying properties include access speed, capacity, data-transfer rate, access method (sequential or random) 
- File-System management 
  - Files usually organized into directories 
  - Access control on most systems to determine who can access what 
  - OS activities include 
    - Creating and deleting files and directories 
    - Primitives to manipulate files and directories 
    - Mapping files onto secondary storage 
    - Backup files onto stable (non-volatile) storage media



<br>

## Mass-Storage Management(생략)

- Usually disks used to store data that does not fit in main memory or data that must be kept for a “long” period of time 
- Proper management is of central importance 
- Entire speed of computer operation hinges on disk subsystem and its algorithms 
- OS activities 
  - Free-space management 
  - Storage allocation 
  - Disk scheduling 
- Some storage need not be fast 
  - Tertiary storage includes optical storage, magnetic tape 
  - Still must be managed – by OS or applications 
  - Varies between WORM (write-once, read-many-times) and RW (read-write)



<br>

## Performance of Various Levels of Storage

![image-20220905153417612](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905153417612.png)



<br>

## Migration of data "A" from Disk to Register

- Multitasking environments must be careful to use most recent value, no matter where it is stored in the storage hierarchy

![image-20220905153448230](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905153448230.png)

- Multiprocessor environment must provide **cache coherency** in hardware such that all CPUs have the most recent value in their cache 
  - multiprocessing 환경에서 더 어려운 과제 (하지만 중요한 OS의 필수 역할)

- Distributed environment situation even more complex 
  - Several copies of a datum can exist 
  - Various solutions covered in Chapter 17



<br>

## I/O Subsystem

- One purpose of OS is to **hide peculiarities of hardware** devices from the user 
- I/O subsystem responsible for 
  - Memory management of I/O including **buffering** (storing data temporarily while it is being transferred), **caching** (storing parts of data in faster storage for performance), **spooling** (the overlapping of output of one job with input of other jobs) 
  - General device-driver interface 
  - Drivers for specific hardware devices



<br>

## Protection and Security

- **Protection** – any mechanism for controlling access of processes or users to resources defined by the OS 
- **Security** – defense of the system against internal and external **attacks** 
  - Huge range, including denial-of-service, worms, viruses, identity theft, theft of service 
- Systems generally first distinguish among users, to determine who can do what 
  - User identities (**user IDs**, security IDs) include name and associated number, one per user 
  - User ID then associated with all files, processes of that user to determine access control 
  - Group identifier (**group ID**) allows set of users to be defined and controls managed, then also associated with each process, file 
  - **Privilege escalation**(단계적 확대) allows user to change to effective ID with more rights

​	

<br>

## Computing Environments - Traditional Computing

- Stand-alone general purpose machines (network가 없던 시절)
- But blurred as most systems interconnect with others (i.e., the Internet) 
  - Office environment 
    - PCs connected to a network, terminals attached to mainframe or minicomputers providing batch and timesharing 
    - Now portals allowing networked and remote systems access to same resources 
  - Home networks 
    - Used to be single system, then modems 
- **Portals** provide web access to internal systems 
- **Network computers** (**thin clients**) are like Web terminals 
  - memory가 굉장히 적은 컴퓨터 (단순한) 

- Mobile computers interconnect via **wireless networks** 
- Networking becoming ubiquitous – even home systems use **firewalls** to protect home computers from Internet attacks



<br>

## Computing Environments - Mobile Computing

- Handheld systems 
  - smartphones, tablets, etc 
- What is the functional difference between them and a “traditional” laptop? 
  - Reduced feature set OS, limited I/O, limited CPU, memory, power 
  - Extra feature – more OS features (GPS, gyroscope) 
- Allows new types of apps like **augmented reality**(AR)
- Use IEEE 802.11 wireless, or cellular data networks for connectivity 
- Leaders are Apple iOS and Google Android



<br>

## Computing Environments – Distributed Systems

- Distributed computiing 
  - Collection of separate, possibly heterogeneous, systems networked together 
    - **Network** is a communications path, **TCP/IP** most common 
      - **Local Area Network** (**LAN**) 
      - **Wide Area Network** (**WAN**) 
      - **Metropolitan Area Network** (**MAN**) 
      - **Personal Area Network** (**PAN**) 
  - **Network Operating System** provides features between systems across network 
    - Communication scheme allows systems to exchange messages 
    - Illusion of a single system

<br>

## Computing Environments – Client-Server Computing

- Client-Server Computing 
  - Dumb terminals supplanted by smart PCs 
  - Many systems now **servers**, responding to requests generated by **clients** 
    - **Compute-server system** provides an interface to client to request services (i.e., database) 
    - **File-server system** provides interface for clients to store and retrieve files 

![image-20220905153920835](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905153920835.png)

<br>

## Computing Environments - Peer-to-Peer Computing

![image-20220905153946104](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905153946104.png)



- Another model of distributed system 
- **P2P** does not distinguish clients and servers 
  - Instead all nodes are considered peers 
  - May each act as client, server or both 
  - Node must join P2P network 
    - Registers its service with central lookup service on network, or 
    - Broadcast request for service and respond to requests for service via **discovery protocol** 
  - Examples include Napster and Gnutella, **Voice over IP** (**VoIP**) such as Skype 

<br>

## Computing Environments - Virtualization

- Allows operating systems to run applications as applications within other OS 
  - Vast and growing industry 
- **Emulation** used when source CPU type different from target type (i.e. PowerPC to Intel x86) 
  - Generally slowest method 
  - When computer language not compiled to native code – **Interpretation** 
  - Every machine-level instruction that runs natively on source system must be translated to equivalent functions of target system 
- **Virtualization** – OS natively compiled for CPU, running **guest** OSes also natively compiled 
  - Consider VMware running WinXP guests, each running applications, all on native WinXP **host** OS 
  - **VMM** (virtual machine Manager) provides virtualization services

- Use cases involve laptops and desktops running multiple OSes for exploration or compatibility 
  - Apple laptop running Mac OS X host, Windows as a guest 
  - Developing apps for multiple OSes without having multiple systems 
  - QA testing applications without having multiple systems 
  - Executing and managing compute environments within data centers 
- VMM can run natively, in which case they are also the host 
  - There is no general purpose host then (VMware ESX and Citrix XenServer)

<br>

## Virtual Machines

- A **virtual machine** takes the layered approach to its logical conclusion. It treats hardware and the operating system kernel as though they were all hardware. 
- A virtual machine provides an interface identical to the underlying bare hardware. 
- The operating system **host** creates the illusion that a process has its own processor and (virtual memory). 
- Each **guest** provided with a (virtual) copy of underlying computer. 



<br>

## Computing Environments - Virtualization

![image-20220905154215993](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905154215993.png)

virtual machine manager

- 가상화를 해 주고 관리하는

<br>

## VMware Architecture

![image-20220905154232295](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905154232295.png)



hardware위에 OS가 있고 그 위에 가상화 layer가 존재하는 

application 별로 가상화 종류가 다 다름

<br>

## The Java Virtual Machine

![image-20220905154248006](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220905154248006.png)



<br>

## Computing Environments – Cloud Computing

- Delivers computing, storage, even apps as a service across a network 
- <u>Logical extension of virtualization</u> because it uses virtualization as the base for it functionality. 
  - Amazon **EC2** has thousands of servers, millions of virtual machines, petabytes of storage available across the Internet, pay based on usage 
- Many types 
  - **Public cloud** – available via Internet to anyone willing to pay 
  - **Private cloud** – run by a company for the company’s own use 
  - **Hybrid cloud** – includes both public and private cloud components 
  - Software as a Service (**SaaS**) – one or more applications available via the Internet (i.e., word processor) 
  - Platform as a Service (**PaaS**) – software stack ready for application use via the Internet (i.e., a database server) 
    - 플랫폼 자체를 서비스로 제공
  - Infrastructure as a Service (**IaaS**) – servers or storage available over Internet (i.e., storage available for backup use)
    - Infrastructure를 대여해 주는 서비스
  
- Cloud computing environments composed of traditional OSes, plus VMMs, plus cloud management tools 
  - Internet connectivity requires **security** like firewalls 
  - Load balancers spread traffic across multiple applications

![image-20220913002622262](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220913002622262.png)

load balancer - 적절하게 load를 분배해야 최대 성능을 얻을 수 있음

<br>

## Computing Environments – Real-Time Embedded Systems

- Real-time embedded systems most prevalent form of computers 
  - Car engine, manufacturing robots, Multimedia systems, network of diverse devices 
  - **real-time OS** 
    - Reduced feature set OS, limited user interface 
- Various embedded systems (보통 임베디드 시스템에 있음, not 범용 컴퓨터)
  - Some general-purpose computers with standard OS with special-purpose applications 
  - Some hardware devices with a special-purpose embedded OS 
  - some hardware devices with ASIC that perform tasks without an OS 
- Real-time OS has well-defined fixed time constraints 
  - Processing **must** be done within constraint 
  - Correct operation only if constraints met



<br>

## Real-Time Systems

- Often used as a control device in a dedicated application such as controlling scientific experiments, medical imaging systems, industrial control systems, and some display systems. 
- Well-defined fixed-time constraints. 
  - time sharing systems : fast response 
  - Batch system: no time constraints 
- `Hard real-time system`
  - Secondary storage limited or absent, data stored in short-term memory, or read-only memory (ROM) 
  - Conflicts with time-sharing systems, not supported by general-purpose operating systems. 
  - 제한된 시간 안에 이벤트 처리가 100% 보장되는 시스템
- `Soft real-time system`
  - Limited utility in industrial control or robotics 
  - Useful in applications (multimedia, virtual reality) requiring advanced operating-system features.
  - ex. 전화 교환기
  - 제한된 시간 안에 이벤트 처리가 100% 보장되지는 않지만 대부분 처리는 되는 시스템

<br>

## Open-Source Operating Systems

- Operating systems made available in source-code format rather than just binary **closed-source** 
  - **Linux**, **Windows** 
- Counter to the **copy protection** and **Digital Rights Management** (**DRM**) movement 
- Started by **Free Software Foundation** (**FSF**), which has “copyleft” **GNU Public License** (**GPL**) 
- Examples include **GNU/Linux** and **BSD UNIX** (including core of **Mac OS X**), and many more 
- Can use VMM like VMware Player (Free on Windows), Virtualbox (open source and free on many platforms - http://www.virtualbox.com) 
  - Use to run guest operating systems for exploration 
- I recommend to read textbook.
