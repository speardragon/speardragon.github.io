---
layout: single
title: "[OS] OS Structure"
categories: ['OS']
tag: ['운영체제', 'OS']
toc: true
toc_sticky: true
---



1, 2 장은 중요도가 떨어짐(많은 시간을 투자해서 이해할 필요 X)

대충 보면서 이해하고 나중에 관련 내용이 나왔을 때 돌아와서 보기



# Chapter 2: Operating-System Structures

- **the services an OS provides to users, processes, and other systems** 
  - Operating System Services 
  - User Operating System Interface 
  - System Calls 
  - System Programs 
- the various ways of structuring an operating system 
  - Operating System Design and Implementation 
  - Operating System Structure (어떻게 구현하는 지)
- how operating systems are installed and customized and how they boot 
  - Operating System Debugging 
  - Operating System Generation 
  - System Boot

<br>

- 리눅스는 monolothic 구조를 갖고 있음
  - OS는 성능이 최대 관건 vs. OS는 모듈화가 관건

- OS는 **실행환경**이다.
  - cloud OS 의 시대
  - JVM은 실행환경이지만 OS위에 있다.
  - 쿠버네티스, docker -> 전망이 있음



<br>

## Operating System Services

- Operating systems provide an **environment** for execution of programs and **services** to programs and users 

- One set of operating-system **services** provides functions that are helpful to the user: 

  - **User interface** - Almost all operating systems have a user interface (UI). 
    - Varies between **Command-Line Interface (CLI)** – text command, 
    - **Graphics User Interface (GUI)**, 
    - **Batch** - file including several commands is executed – **shell script** 
      - 여러 CLI가 나열된 파일
  - **Program execution** - The system must be able to **load a program** into **memory** and to run that program, end execution, either 'normally' or 'abnormally' (indicating error) 
    - c 소스 파일에서 object 파일로 만드는 것은 compiler의 역할
    - 여러 개의 object 파일을 하나로 모아서 실행파일로 만드는 역할은 linker
    - main memory에 탑재 시키는 것이 loader
  - **I/O operations** - A running program may require I/O, which may involve a file or an I/O device

  - **File-system manipulation** - The file system is of particular interest. Programs need to read and write files and directories, create and delete them, search them, list file Information, permission management. 
  - **Communications** – Processes may exchange information, on the same computer or between computers over a network 
    - Communications may be via **shared memory** or through **message passing** (packets moved by the OS) 
  - **Error detection** – OS needs to be constantly aware of possible errors 
    - May occur in the CPU and memory hardware, in I/O devices, in user program 
    - For each type of error, OS should take the **appropriate action** to ensure correct and consistent computing 
    - Debugging facilities can greatly enhance the user’s and programmer ’s abilities to efficiently use the system

- Another set of OS **functions** exists for ensuring the **efficient** operation of the system itself via resource sharing 
  - **Resource allocation** - When multiple users or multiple jobs running concurrently, resources must be allocated to each of them 
    - Many types of resources - CPU cycles, main memory, file storage, I/O devices. 
  - **Accounting** - To keep track of which users use how much and what kinds of computer resources (얼마나 사용하는 지 모니터링)
  - **Protection and security** - The owners of information stored in a multiuser or networked computer system may want to control use of that information, concurrent processes should not interfere with each other 
    - **Protection** involves ensuring that all access to system resources is controlled 
    - **Security** of the system from outsiders requires user authentication, extends to defending external I/O devices from invalid access attempts

<br>

## A View of Operating System Services

![image-20220907225450734](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907225450734.png)

사용자가 하드웨어를 직접 접근할 일은 없음 -> OS가 하드웨어를 추상화 하였기 때문에 



<br>

## User Operating System Interface - CLI

- CLI or **command interpreter** allows direct command entry 
  - Sometimes implemented in kernel, sometimes by systems program (Window/Unix) 
  - Sometimes multiple flavors implemented – **shells** 
    - Bourne shell, C shell, Bourne-Again shell, Korn shell 
  - Primarily fetches a command from user and executes it
  - Sometimes commands built-in, sometimes just names of programs 
    - If the latter, adding new features doesn’t require shell modification 
  - ls와 같은 명령 또한 프로그램이다.(시스템 프로그램)



<br>

## Bourne Shell Command Interpreter

![image-20220907225743599](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907225743599.png)



<br>

## User Operating System Interface - GUI

- User-friendly **desktop** metaphor interface 
  - Usually mouse, keyboard, and monitor 
  - **Icons** represent files, programs, actions, etc 
  - Various mouse buttons over objects in the interface cause various actions (provide information, options, execute function, open directory (known as a **folder**) 
  - Invented at Xerox PARC (Palo Alto Research Center) 
- Many systems now include both CLI and GUI interfaces 
  - Microsoft Windows is GUI with CLI “command” shell 
  - Apple Mac OS X is “Aqua” GUI interface with UNIX kernel underneath and shells available 
  - Unix and Linux have CLI with optional GUI(optional) interfaces 
    - Commercial- CDE(Common Desktop Environment), X-window 
    - Open source - KDE, GNOME



<br>

## Touchscreen Interfaces

- Touchscreen devices require new interfaces 
  - **Mouse not possible** or not desired 
  - Actions and selection based on gestures 
  - Virtual keyboard for text entry 
- Voice commands.

![image-20220907230322554](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907230322554.png)



<br>

## The Mac OS X GUI

![image-20220907230350004](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907230350004.png)



<br>

## System Calls

- **Programming interface** to the services provided by the OS 
- Typically written in a high-level language (C or C++) 
- Mostly accessed by programs via a high-level **Application Programming Interface (API)** rather than direct system call use 
  - API -> system call 전환
- Three most common APIs are **Win32 API** for Windows, **POSIX API** for POSIX-based systems (including virtually all versions of UNIX, Linux, and Mac OS X), and **Java API** for the Java virtual machine (JVM) 
  - 3 개의 API가 존재한다.


>  Note that the system-call names used throughout this text are generic





<br>

## Example of System Calls

- System call sequence to copy the contents of one file to another file

![image-20220907231055458](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231055458.png)

시스템 콜은 커널에서 실행되는 커널 함수이다.

<br>

## Example of Standard API

![image-20220907231116093](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231116093.png)

read()

??/

<br>

## System Call Implementation

- Typically, a **number** associated with each system call 
  - **System-call interface** maintains a table indexed according to these numbers 
- The system call interface invokes the intended system call in OS kernel and returns status of the system call and any return values 
- The caller need know nothing about how the system call is implemented 
  - Just needs to obey API and understand what OS will do as a result call 
  - Most details of OS interface hidden from programmer by API 
    - Managed by run-time support library (set of functions built into libraries included with compiler)



<br>

## API - System Call - OS Relationship

![image-20220907231213382](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231213382.png)

open() 시스템 콜의 number가 i라 할 때,

<br>

## Standard C Library Example

- C program invoking printf() library call, which calls write() system call

![image-20220907231241893](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231241893.png)

printf() -> library call(API)

<br>

## System Call Parameter Passing

- Often, more information is required than simply identity of desired system call 
  - Exact type and amount of information vary according to OS and call 
- Three general methods used to pass parameters to the OS 
  - Simplest: pass the parameters in registers 
    - In some cases, may be more parameters than registers 
  - Parameters stored in a block, or table, in memory, and address of block passed as a parameter in a register 
    - This approach taken by Linux and Solaris 
  - Parameters placed, or **pushed**, onto the **stack** by the program and **popped** off the stack by the operating system 
  - Block and stack methods do not limit the number or length of parameters being passed



<br>

## Parameter Passing via Table

![image-20220907231734443](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231734443.png)


<br>

## Types of System Calls

- **Process control** 
  - create process, terminate process 
  - Normal termination, abort 
  - load, execute 
  - get process attributes, set process attributes 
  - wait for time : wait_time() 
  - wait event, signal event : wait_event(), signal_event() 
  - allocate and free memory 
  - Dump memory if error 
  - **Debugger** for determining **bugs**, **single step** execution 
  - **Locks** for managing access to shared data between processes 
    - acquire_lock(), release_lock()



<br>

## Examples of Windows and Unix System Calls

![image-20220907231830253](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231830253.png)



<br>

## Standard C Library Example

- C program invoking printf() library call, which calls write() system call

![image-20220907231850599](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231850599.png)



<br>

## Example: MS-DOS

- **Single-tasking** 
- Shell invoked when system booted 
- Simple method to run program 
  - No process created 
- Single memory space 
- Loads program into memory, overwriting all but the kernel 
- Program exit -> shell reloaded
  - overwriting 하는 경우 reload


![image-20220907231923235](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907231923235.png)



<br>

## Example: FreeBSD

- Unix variant 
- **Multitasking** 
  - 여러 프로그램이 동시에 실행

- User login -> invoke user’s choice of shell 
- Shell executes fork() system call to create process 
  - Executes exec() to load program into process 
  - Shell waits for process to terminate or continues with user commands 
- Process exits with: 
  - code = 0 – no error 
  - code > 0 – error code

![image-20220907232028166](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907232028166.png)

<br>


## Types of System Calls

- **File management** 
  - create file, delete file 
  - open, close file 
  - read, write, reposition 
  - get and set file attributes 
  - Same for directories 
- **Device management** 
  - physical device (disk drives), logical device (file) 
  - request device, release device (similar to open, close files) 
  - read, write, reposition 
  - get device attributes, set device attributes 
  - logically attach or detach devices

- **Information maintenance** 
  - get time or date, set time or date 
  - get system data, set system data 
  - get and set process, file, or device attributes 
- **Communications** 
  - create, delete **communication connection** 
  - send, receive messages if **message passing model** to **host name** or **process name** 
    - From **client** to **server** 
    - 받을 host 이름과 process name이 필요로 됨
  - **Shared-memory model** create and gain access to memory regions 
    - allows memory transfer speed, but may have **synch problem** 
    - shared_memory create, shared_memory attach 
  - transfer status information 
  - attach and detach remote devices
  
- Protection 
  - Control access to **resources** 
  - Get and set **permissions** 
  - **Allow** and **deny** user access

<br>

## System Programs

utility program이라고 하기도 함.

- System programs provide a convenient environment for program development and execution. They can be divided into: 
  - File manipulation 
  - Status information sometimes stored in a File modification 
  - Programming language support 
  - Program loading and execution 
  - Communications 
  - Background services 
  - Application programs 
- Most users’ **view** of the operation system is defined by system programs, not the actual system calls

- Provide a convenient environment for program development and execution 
  - Some of them are simply user interfaces to system calls; others are considerably more complex 
- **File management** - Create, delete, copy, rename, print, dump, list, and generally manipulate files and directories 
- **Status information** 
  - Some ask the system for info - date, time, amount of available memory, disk space, number of users 
  - Others provide detailed performance, logging, and debugging information 
  - Typically, these programs format and print the output to the terminal or other output devices 
  - Some systems implement a **registry** - used to store and retrieve configuration information

- **File modification** 
  - Text editors to create and modify files 
  - Special commands to search contents of files or perform transformations of the text 
- **Programming-language support** - Compilers, assemblers, debuggers and interpreters sometimes provided 
- **Program loading and execution**- Absolute **loaders**, relocatable loaders, linkage editors, and overlay-loaders, debugging systems for higher-level and machine language 
- **Communications** - Provide the mechanism for creating virtual connections among processes, users, and computer systems 
  - Allow users to send messages to one another’s screens, browse web pages, send electronic-mail messages, log in remotely, transfer files from one machine to another

- **Background Services** 
  - Launch system program-process at boot time 
    - Some for system startup, then terminate 
    - Some from system boot to shutdown 
  - Provide facilities like disk checking, process scheduling, error logging, printing 
  - Run in user context **not kernel context** 
  - Known as **services**, **subsystems**, **daemons** 
  - 모니터를 통해서 사용자와 interaction이 일어나지 않는다.
- **Application programs** 
  - Don’t pertain to system 
  - 사용자가 짠 프로그램이 아님
  - Word processor, web browser . .. 
  - Run by users 
  - **Not typically considered part of OS** 
  - Launched by command line, mouse click, finger poke

<br>

## Linker &amp; Loader

- Will be explained later(memory management part)



<br>

## Operating System Design and Implementation

- Design and Implementation of OS is not easy, but some approaches have been proven successful 
- Internal structure of different Operating Systems can vary widely 
- Start the design by defining goals and specifications 
  - **Affected** by **choice** of hardware, type of system 
    - Batch, time-sharing, multi-user, distributed, real-time … 
- **User** goals and **System** goals 
  - User goals – operating system should be convenient to use, easy to learn, reliable, safe, and fast 
    - May not be useful in the system design 
  - System goals – operating system should be easy to design, implement, and maintain, as well as flexible, reliable, error-free, and efficient 
  - No unique solution

- Important principle to separate the following for flexibility 
  - **Policy**: <u>What</u> will be done? 
  - **Mechanism**: <u>How</u> to do it? 
- Mechanisms determine how to do something, policies decide what will be done 
- The **separation** of policy from mechanism is a very important principle, it allows maximum flexibility if policy decisions are to be changed later (example – timer) 
  - 둘 간의 dependency를 줄일 수 있음

- Specifying and designing an OS is highly creative task of **software engineering**

<br>

## Implementation

- Much variation  
  - Early OSes in assembly language 
  - Then system programming languages like Algol, PL/1 
  - Now C, C++ 
- Actually usually a mix of languages 
  - Lowest levels in assembly (실행속도가 빠름)
  - Main body in C 
  - Systems programs in C, C++, scripting languages like PERL, Python, shell scripts 
- More high-level language easier to **port** to other hardware (port-한 시스템에서 작동되는 걸 다른 시스템에서도 작동되도록 해 주는)
  - Can be written faster, easy to understand, debug 
  - But slower. Require more memory (기술의 발전으로 gap이 그렇게 크진 않음.)
  - Linux is written in C, so it can be available on various CPUs 
- MS-DOS written in Intel 8088 assembly language 
  - It runs natively only on Intel X86 family. 
  - **Emulators** of X86 instruction set allow OS to run on other CPUs 
- **Emulation** can allow an OS to run on **non-native hardware** by duplicating functionalities between two systems



<br>

## Operating System Structure

- General-purpose OS is very large program 
  - Monolithic structure vs. Modular structure 
- Various ways to structure ones (위로 갈 수록 monolithic, 아래로 갈 수록 modular)
  - Simple structure – MS-DOS 
  - More complex -- UNIX 
  - Layered – an abstraction 
  - Microkernel -Mach



<br>

## Simple Structure -- MS-DOS

- MS-DOS – written to provide the most functionality in the least space 
  - Not divided into modules (monolithic)
  - application program이 직접 device drivers에 interface -> 굉장한 문제가 발생할 확률이 높다.
  - Although MS-DOS has some structure, its interfaces and levels of functionality are not well separated 
  - 8088 provides no dual mode, hardware protection
    - 되도록 적은 자원으로 OS 구현하려고 시도

![image-20220907233015240](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233015240.png)



<br>

## Non Simple Structure -- UNIX

- original UNIX – **limited** by hardware functionality, the original UNIX operating system had limited structuring. 
- The UNIX OS consists of two separable parts 
  - Systems programs 
  - The kernel 
    - Consists of everything **below** the **system-call interface** and **above the physical hardware** 
    - Provides the file system, CPU scheduling, memory management, and other operating-system functions; a large number of functions for one level 
    - Layered some extent, but basically **monolithic**



<br>

## Traditional UNIX System Structure

Beyond simple but not fully layered

![image-20220907233156659](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233156659.png)

OS kernel 부분이 그렇게 막 well-structured 되진 않음, 단지 사용자와 kernel사이가 잘 구분 되어 있음

<br>

## Layered Approach

- **Modular structure** 
- Information hiding 
  - lower layer의 구조를 알 필요 없음

- The operating system is **divided** into a number of layers (levels), each built on top of lower layers. The bottom layer (layer 0), is the hardware; the highest (layer N) is the user interface. 
- With modularity, layers are selected such that each uses functions (operations) and **services of only lower-level layers** 
- Simple construction, debugging 
- Overhead in each layer
  - monolithic - interaction이 하나의 계층에서만 이루어지지만 modular는 layer에 걸쳐서 이루어 지기 때문에 response 시간이 길다.
  - 성능은 그렇다고 하지만 엔지니어링 시간이나 유지보수 기간 등을 따져 보았을 때 modular가 압도적으로 좋다.


![image-20220907233427206](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233427206.png)

<br>

## Microkernel System Structure

- **seperation** between policy and mechanism 
  - Policy free mechanisms of building blocks(kernel 모듈) 
  - Modularized Kernel (커다란 OS의 터전)
- Moves as much from the kernel into user space 
- **Mach (CMU)** example of **microkernel** 
  - Mac OS X kernel (**Darwin**) partly based on Mach 
- Communication takes place between user modules using **message passing** 
- Benefits: 
  - Easier to extend a microkernel (얹기만 하면 됨)
  - Easier to **port** the operating system to new architectures 
  - More reliable (less code is running in kernel mode) 
  - More secure 
- Detriments: 
  - **Performance overhead** of user space to kernel space communication

​	

<br>

## Microkernel System Structure

![image-20220907233551710](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233551710.png)

- 일부 kernel의 내용이 밖에 나올 수 있음
- microkernel: 최소한의 기능만을 가진 kernel

<br>

## Modules 

기존의 modular approach에서 진보한 구조

- Many modern operating systems implement **loadable kernel modules** 
  - Kernel provides core services while other services are implemented (via modules) dynamically (**dynamic linking** rather than recompiling) 
    - core service가 아니기 때문에 처음부터 linking 되어 탑재 되어 있진 않다.
  - Uses object-oriented approach 
  - Each core component is separate 
  - Each talks to the others over **known interfaces** 
  - Each is loadable as needed within the kernel 
- Overall, similar to layers but with more flexible 
  - Linux, Solaris, etc

<br>

## Solaris Modular Approach

![image-20220907233817812](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233817812.png)



<br>

## Hybrid Systems

- Most modern operating systems are actually not one pure model 
  - **Hybrid** combines multiple approaches to address performance, security, usability needs 
  - Linux and Solaris kernels in kernel address space, **so monolithic**(because of performance), plus modular for **dynamic loading** of functionality(core가 아닌 경우에 대하여) 
  - **Windows** mostly monolithic, plus microkernel for different subsystem personalities 
- Apple Mac OS X hybrid, **layered**, Aqua UI plus Cocoa programming environment 
  - Below is kernel consisting of Mach microkernel and BSD Unix parts, plus I/O kit and dynamically loadable modules (called **kernel extensions**)



<br>

## Mac OS X Structure

![image-20220907233918782](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907233918782.png)



---

<mark>이 아래는 생략</mark>

---

<br>

## iOS

- Apple mobile OS for iPhone, iPad 
  - Structured on Mac OS X, added functionality 
  - Does not run OS X applications natively 
    - Also runs on different CPU architecture (ARM vs. Intel) 
  - Cocoa Touch Objective-C API for developing apps 
  - Media services layer for graphics, audio, video 
  - Core services provides cloud computing, databases 
  - Core operating system, based on Mac OS X kernel

![image-20220907234000505](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234000505.png)

<br>

## Android

- Developed by Open Handset Alliance (mostly Google) 
  - Open Source 
- Similar stack to IOS 
- Based on Linux kernel but modified 
  - Provides process, memory, device-driver management 
  - Adds power management 
- Runtime environment includes core set of libraries and Dalvik virtual machine 
  - Apps developed in Java plus Android API 
    - Java class files compiled to Java bytecode then translated to executable than runs in Dalvik VM 
- Libraries include frameworks for web browser (webkit), database (SQLite), multimedia, smaller libc



<br>

## Android Architecture

![image-20220907234053174](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234053174.png)



<br>

## Operating-System Debugging

- Debugging is finding and fixing errors, or bugs 
- Failure analysis 
  - If a process fails, OS generate log files containing error information and Failure of an application can generate core(memory) dump file capturing memory of the process 
    - Running program and core dump can be probed by debugger 
  - OS failure is called crash. Error info is saved on log files, and crash dump file containing kernel memory is saved 
- Beyond crashes, performance tuning can optimize system performance 
  - Sometimes using trace listings of activities, recorded for analysis 
  - Unix command top 
  - Windows Task Manager 
- Kernighan’s Law: “Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.”



<br>

## Performance Tuning

- Improve performance by removing bottlenecks 
- To identify bottleneks, we need monitoring system performance 
- OS must provide means of computing and displaying measures of system behavior 
- For example, “top” program or Windows Task Manager 
  - Display resources used 
  - CPU. Memory usuage, networking 통계

![image-20220907234204265](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234204265.png)

<br>

## DTrace

- DTrace tool in Solaris, FreeBSD, Mac OS X allows live instrumentation on production systems 
- Probes fire when code is executed within a provider, capturing state data and sending it to consumers of those probes
- Example of following XEventsQueued system call move from libc library to kernel and back

![image-20220907234230584](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234230584.png)



<br>

- DTrace code enables scheduler to probe and record amount of time of each process running with UserID 101 in running mode (on CPU) in nanoseconds

![image-20220907234305805](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234305805.png)

![image-20220907234310961](https://raw.githubusercontent.com/speardragon/save-image-repo/main/img/image-20220907234310961.png)

<br>

## Operating System Generation(Building an OS)

- Building OS 
  - Write OS source code, or obtain written source code 
  - Configure the OS for specific machine 
  - Compile the OS 
  - Install the OS 
  - Boot the computer and its OS 
- Operating systems are designed to run on any of a class of machines; the system must be configured for each specific computer site 
  - The computer system must be configured and generated for each computer site – need system generation 
- SYSGEN program obtains information concerning the specific configuration of the hardware system from OS distribution file 
  - Used to build system-specific compiled kernel or system-tuned



<br>

## System Boot

- When power initialized on system, execution starts at a fixed memory location 
  - Firmware ROM used to hold initial boot code 
- Operating system must be made available to hardware so hardware can start it 
  - Small piece of code – bootstrap program, stored in ROM or EEPROM locates the kernel, loads it into memory, and starts it 
    - Sometimes two-step process where small bootstrap loader (BIOS), stored in ROM, load a second boot loader at fixed location (boot block) of disk 
  - Kernel initializes hardware, Inspecting memory, CPU. 
  - root file system is mounted 
- Common open source bootstrap loader for unix, GRUB, allows selection of kernel from multiple disks, versions, kernel options