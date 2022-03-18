---
layout: single
title: "[System Programming] Unix/Linux 소개"
categories: ['System', 'System Programming']
tag: ['System Programming', 'Unix']
toc: true
toc_sticky: true
---

# Unix/Linux

<br>

## 학습 목표 및 내용

- 학습 목표
  - 유닉스/리눅스 시스템의 체계적 이해
  - 시스템 프로그래밍 능력 향상
- 학습 내용
  - 리눅스 시스템 프로그래밍
    - 시스템 호출을 이용한 `C 프로그래밍`
- 주요 프로그래밍 주제
  - 파일
  - 프로세스
  - 메모리
  - 프로세스 사이의 통신(IPC)

<br>

## 컴퓨터의 정의

- **프로그램**(명령어들의 리스트)에 따라 데이터를 처리하는 기계

계산기: 특수 목적 컴퓨터(정해진 기능만을 수행, 기능 변경 불가능)

노트북, 데스크 탑: 범용 컴퓨터(프로그램이라는 개념 도입, 수행 기능 변경 가능)

<br>

## 프로그램

- 작업 지시서(instructions이 나열된 것)
- 컴퓨터에게 해야 할 작업의 내용을 알려주는 문서
- A sequence of steps(instructions)

- 각 step에서는 arithmetic 혹은 logical operation이 수행된다.

<br>

## von Neumann Architecture(매우 중요)

- Stored Program concept
  - program과 data를 저장하는 main memory
    - Instructions and data are stored in a single read-write memory.
  - 메모리의 내용은 location에 의해 <mark>addresable</mark> 하다 without regard to the type of data contained there.
  - Execution occurs in **sequential fashion** unless **explicitly** modified 
    - Sequence, Loop, Selection
    - 위 세가지로 어떤 프로그램이든 설명 가능하다.
    - Loop와 selection이 아닌 경우 sequencetial하게 실행됨
    - Loop와 selection이 explicitly modified 된 것임

- Structure of von Nuemann machine

![image](https://user-images.githubusercontent.com/79521972/156915056-0fb8ff72-1f55-47bf-ae60-3f384f5bfc77.png)

- main memory는data와 instructions을 모두 저장한다.
- ALU operating on binary data.
- Control unit interpreting instructions from memory and executing
  - decode 과정
- control unit에 의해 작동되는 Input/output equipment

ALU와 control unit이 합쳐진 것이 CPU이다.

<br>

##  Instruction Cycle

![image](https://user-images.githubusercontent.com/79521972/156915411-88b95d86-98a9-4a07-affe-ddb876a956fa.png)

- Two steps이 존재:
  - Fetch: 다음에 실행할 instruction을 메인 메모리에 가져오는 과정.
  - Execute:메인 메모리에 가져온 instruction의 실제 실행을 하는 과정.

첫 번째 instruction cycle이 실행 되면 fetch와 execute 과정이 이루어 지는 것.

<br>

CPU의 핀 중 하나는 Interrupt 를 걸 수 있는데 이 핀이 cycle 중간에 active high가 되더라도 그 즉시 check하는 것이 아니라 한 사이클이 끝나고 다음 사이클을 다시 시작하기 직전에 잠깐 확인한다.

이렇게 똑같은 Instruction Cycle 구조를 갖고 있음에도 불구하고 여러 프로그램의 각자behavior가 다 다른 이유는 프로그램 각자의 instruction 순서가 다르기 배치 되어있기 때문.

### Fetch&Execution Cycle

- Program Counter(PC, CPU register) holds address of next instruction to fetch
  - PC가 다음에 실행(fetch)할 instruction의 주소를 갖고있다.
- Processor fetches instruction from **memory location** pointed to by PC
- increment PC
  - Unless told otherwise

- Processor interprets instruction and performs required **actions**.(execution)
  - 어떤 종류의 instruction인지 interpret -> decode(e.g. add, subtract, multiply)

<br>

![image](https://user-images.githubusercontent.com/79521972/156916664-a80540f7-e43c-45c6-a09b-61c15f72bfa7.png)



<br>

Instruction Cycle의 좀 더 자세한 그림은 다음과 같다.

![image-20220306181937439](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220306181937439.png)

왼쪽 3개의 과정까지가 fetch이고 나머지가 execution 과정이다.

<br>

## System software

시스템 소프트웨어를 작성하는 것이 시스템 프로그래밍(우리가 배우는 것)이기에 시스템 소프트웨어가 무엇인지 알아보자.

software designed to provide <mark>a platform</mark> to other software. 

- 소프트웨어 중 다른 소프트웨어에 영향을 주지 않고 uninstalled 할 수 있는 소프트 웨어는 시스템 소프트웨어가 아니다.(wikipedia)

즉, 시스템 소프트웨어는 다른 소프트웨어에게 플랫폼을 제공하기 위해 디자인 되었기 때문에 다른 소프트웨어와 종속 관계인 것이다.

system application에 사용되는 OS 프로그램들과 서비스들로 구성된 플랫폼

refers to the files and programs that make up your computer’s operating system. 

-  System files include libraries of functions, system services, drivers for printers and other hardware, system preferences, and other configuration files. 
  - The programs that are part of the system software include assemblers, compilers, file management tools, system utilities, and debuggers (techterms.com)

<br>

## system software 인 것과 아닌 것?

system software의 예:

- OS Kernals
- Drivers
- Bare Metal Hypervisors
  - bare metal hypervisor : 범용 하드웨어
- Compilers(that produce native binaries) and Debuggers



system software가 아닌 것의 예:

- GUI Chat application(Slack, Discord, etc)
- Web-based JavaScript Application
- Web service API
  - 다른 소프트웨어에게 서비스를 제공하고 추상화를 위해 하드웨어와 상호작용하지 않는다.

<br>

## System programming

- programming computer system software의 acticity

- system programming의 구별되는 특징 

  - application program가 유저에게 직접적으로 서비스를 제공하는 소프트웨어를 생산하는 것을 목표를 하는데
    - 예를 들어, ms word는 사용자가 다른 소프트웨어가 아닌 사람(Human)이기 때문에 시스템 소프트웨어가 아니다.

  - 반면에 system programming은 <mark>다른 소프트웨어에게</mark> 서비스를 제공하는 플랫폼을 생산하는 것을 목표로 한다.

- 또한 system programming은 프로그래머가 반드시 hardware와 os에 대해서 매우 잘 알아야 한다는 점에서 구분된다.
  - System software lives at a low level, interfacing directly with the kernel and core system libraries
- system programming의 가장 중요한 핵심 파트는 매우 빠른 속도가 요구된다는 것이다.
  - 이는 성능, 즉 performance와 직결된 문제로 다른 소프트웨어에 플랫폼 역할을하기 때문에 만약 당신의 application의 중심부(즉, system software platform)가 느리다면, 전체 application의 performance도 매우 느려질 것이다. 

- 따라서 목표가 available resources의 효율적인 사용인 것이다.

<br>

## Run-Time Environments(RTE)

프로그램은 항상 RTE와 같은 또다른 프로그램과 연관되어 만들어진다.

RTE의 종류:

- OS: Unix/Linux, Window OS
- Java Virtual Machine(JVM)
  - JVM은 OS는 아니지만 다른 소프트웨어에게 플랫폼을 제공하기 때문에 RTE의 종류 중 하나인 것이다.
- Cloud OS



<br>

## What is an operating system?(OS)

application 짜는 사람들이 하드웨어를 직접 제어하도록 짜야 된다면 너무 힘들 것이기 때문에 application과 hardware 중간 다리 역할을 하는 매개체가 있어야 하는데 이 매개체를 바로 OS(operation syste), 우리 말로 운영체제라고 한다.

hardware 자원에 access 하는 application software를 제공하는 special layer of software

- Convenient abstraction of complex hardware devices 
- Protected access to shared resources 
- Security and authentication 
- Communication amongst logical entities

위 네 가지가 OS에서 중요한 부분이다.

<br>

![image](https://user-images.githubusercontent.com/79521972/156927317-a1487246-be7a-4e05-b86a-4e58964b2db3.png)

응용 프로그램이 하드웨어를 직접 access해야 하는데 그것이 매우 불편하기 때문에 OS가 hardware를 추상화 하여 user program이 hardware 제어를 손쉽게 할 수 있도록 도와 준다.

이때 이 둘(하드웨어, 유저 프로그램) 간의 interface가 정의되는데

- User interface: 사용자 관점에서 정의된 interface
- System call: 하드웨어 관점에서 정의된 interface

가 있다.

보라색으로 되어 있는 것들: system call에 의해서 사용자에게 제공되는 서비스들을 구현하는 알고리즘의 집합(OS kernal)이다.

<br>

## What does an OS do?

- Provide abstractions to apps

  -  file system
    - 각각의 파일을 저장할 하드 디스크에 대한 access가 필요한데 파일을 사용하는 입장에서 하드디스크를 어떻게 access를 해야하는지 알 필요까지는 없다.(OS에서 파일에 대한 추상화를 지원하기 때문에)

  - process, theads
    - Process, Thread: Ms word 아이콘을 두 번 누르면 창이 두 개가뜸 -> 프로세스가 두개가 실행 된 것즉, instance두 개가 만들어진 것이라고도 한다.

  - VM, containers

- Manage resources
  - Memory, CPU, storage...

- Achieves the above by implementing specific algorithms
  - Scheduling
  - Concurrency
  - Transactions
  - Security

<br>

## Virtual Machines

- Software emulation(한 컴퓨터가 다른 컴퓨터처럼 똑같이 작동하도록 하는 기법, 즉 복제) of an abstract machine
  - 프로그램들에게 마치 그들이 machine을 소유한 것과 같은 환상(illusion)을 준다.
  - Make it look like 하드웨어가 내가 원하는 feature들을 가지고 있다고 

- Two types of "VMs"

  - Process VM: **a single program**의 실행을 지원(support)
    - 주로 OS에 의해 제공되는 기능

  - System VM: **전제 운영체제와 그것의 applications**의 실행을 지원(support)
    - Hypervisor에 의해 제공됨

### Porcess VMs

특징:

- Programming simplicity
  - 각 process는 모든 memory/CPU time을 소유한다고 생각한다.
  - 모든 devices를 소유한다고 생각함
  - 다른 device는 same high level interface를 가진 것처럼 보임
  - 다른 interface는 raw hardware보다 더 강력
- Fault Isolation(결함 고립)
  - 다른 precess들에게 직접적으로 영향을 줄 수 없다.
  - 버그가 전체 머신을 망칠 수 없다.

- Protection and Portability
  - Java interface 는 안전하고 안정되어 있다 (많은 플랫폼들을 걸쳐서)

### System VMs: Layers of OSs

![image](https://user-images.githubusercontent.com/79521972/156927771-79adade9-7026-4d36-8994-51d1f1e8e321.png)

- Useful of OS development
  - OS가 충돌이 날때, 한 VM에게 제약을 가한다.
  - 다른 OS들 위의 프로그램들을 테스트 하는 것을 돕는다.

<br>

## Process Life Cycle

- OS가 process의 life cycle을 관리/감독한다.
  - 3 main steps: <mark>**Creating the process, managing the process and terminating the process.** </mark>
- The OS identifies which program (= executable file) is to be executed to create the process.
  - Executable files are generated with a compiler given the text of the program in a language such as C or C++
  - This is a file that contains instructions to be executed on the CPU to activate the process. The format of executable files is specified by the OS. 
- OS는 prcess를 만들라고 user에게 요청받는다.
  - 이는 예를 들어 shell(명령 프롬포트)을 통해 가능하다.
  - Shell: 소프트웨어 중 하나로 user를 위한 interface를 제공한다.
    - Command Line Interface(CLU) or a Graphical User Interface(GUI).



### Creating the process

- OS가 process instance를 executable로 부터 생성한다.
  - This instance is identified by its process ID, which is unique among all running processes.
  - All information about running processes is maintained by the OS in a process table.
- OS는 또한 computer의 main memory에 새로운 process에 대한 메모리도 할당함.
- The OS loads the instructions from the program executable file into main memory

### Managing the process

- The OS identifies the starting function in the program (e.g., "main" in C and C++) and invokes it (calls the function). 
  - The main function receives arguments passed from the OS. These are called the command line arguments. 
- From that point on, the instructions of the main function are executed as specified in the program.

### Terminating the process

- process가 special system call인 `exit`을 실행시 process가 종료됨
- Memory and all other resources allocated to the resource are freed, the process ID is freed and the process entry is removed from the OS process table.(freed: 해방)



## Why Linux?

1991년 : 핀란드 대학원생 Linus Tovalds가 만듬

리눅스 기능상 장점:

- 멀티 태스킹
-  다중 사용자 접근
- POSIX 1003.1 standard 지원
- 다양한 파일 시스템 지원
- 다양한 네트워크 프로토콜 지원
- 다양한 아키텍처 지원
- 멀티 프로세서 지원

<br>
유닉스/리눅스 기반 운영체제:

1. 안드로이드OS
2. iOS
3. Mac OS x
4. Linux
5. BSD 유닉스(버클리)

6. 시스템 V
7. Sun 솔라리스
8. ...

<br>

##  유닉스의 특징

- 다중 사용자, 다중 프로세스 운영체제(OS)
- 쉘 프로그래밍(Shell Programming)
- 훌륭한 네트워킹

<br>

## Unix/Linux의 구성

![image](https://user-images.githubusercontent.com/79521972/156928254-1d38b3ad-4fd7-4e86-a9fd-7991640879d6.png)

- Application program
- Library
- System call
  - Application과 operating system과의 interface
  - System call이 호출되면 kernal code가 수행됨
- Kernal
  - system resource를 효율적으로 사용하도록 관리
  - process/memory/file/IO management

<br>

## 유닉스 운영체제 구조

- 운영체제
  - 컴퓨터의 하드웨어 자원을 운영 관리하고 프로그램을 실행할 수 있는 환경을 제공.
- 커널(Kernal)
  - 운영체제의 핵심으로 하드웨어 운영 및 관리
- 시스템 호출(system call)
  - 커널이 제공하는 서비스에 대한 프로그래밍 인터페이스 역할
- 쉘(Shell)
  - 사용자와 운영체제 사이의 인터페이스
  - 사용자로부터 명령어를 입력받아 해석하고 수행해 주는 명령어 해석기

<br>

## 커널의 역할

- 하드웨어를 운영 관리하여 프로세스, 파일, 메모리, 통신, 주변장치(키보드, 마우스) 등을 관리하는 서비스 제공

<br>

**프로세스 관리(Process management)**

- 여러 프로그램이 실행될 수 있도록 프로세스들을 CPU 스케줄링하여 동시에

**파일 관리(File management)**

- 디스크와 같은 저장장치 상에 파일 시스템을 구성하여 파일을 관리한다. 

**메모리 관리(Memory management)**

- main memory가 효과적으로 사용될 수 있도록 관리한다.

**통신 관리(Communication management)**

- 네트워크를 통해 주고받을 수 있도록 관리한다.

**주변장치 관리(Device management)**

- 모니터, 키보드, 마우스와 같은 장치를 사용할 수 있도록 관리한다.

<br>

## 시스템 호출

![image](https://user-images.githubusercontent.com/79521972/156928484-5305d3c1-1387-4f42-9c3e-1a6527282c19.png)

OS 시스템과 사용자 프로세스 간의 inteface

<br>

- system interface 역할
  - Application programs talk to the operating system(system call을 통해서)
  - 프로그래머들의 기능적 interface to the UNIX kernal

![image](https://user-images.githubusercontent.com/79521972/156928543-a9926ca2-7961-4b06-93ff-31b75a0551aa.png)

<br>

## Kernal mode & User mode

![image](https://user-images.githubusercontent.com/79521972/156928553-66b7c6d2-f40e-4763-b981-34e23495331b.png)





## 시스템 호출과 라이브러리 함수

![image](https://user-images.githubusercontent.com/79521972/156928564-2fc2dc73-f084-4403-a7c2-d8ae5cca3663.png)

![image](https://user-images.githubusercontent.com/79521972/156928579-056d35a8-4656-4287-924c-0d1b16803287.png)

![image](https://user-images.githubusercontent.com/79521972/156928595-1a0cb0b6-11ee-4fda-84c3-8fcf14b1abf8.png)

### Kernal mode

- privileged mode
  - 무엇이 privileged인가?(특권을 가진)
  - no restriction is imposed on the kernal of the system
- may use all the instructions of the processor
- 메모리의 전체를 조작
- 주변기기(peripheral) 컨트롤러와 직접적으로 소통



### User mode

- normal execution mode for a process
  - has no privileges
  - certain instructions are forbidden
  - only allowed to zones allocated to it
  - cannot interact with the physical machine
- process carries out operations in its own environment, without interfering with other processes
- process may be interrupted at any moment(언제든 프로세스 중지 가능)

<br>

## Calling a regular function & Invoking a system call

- 우리가 프로그램을 작성할때 반드시 regular function을 호출하는 것과 system call을 invoke하는 것의 차이를 이해해야 한다.
  -  둘이 비슷해 보이지만(같은 일을 하기때문에) system call 호출은 완전히 다르다.

- 만약 process가 system call을 부르면 프로세스는 OS가 그 service를 완수할 때까지 interrupted 상태를 유지한다.
  - 즉, system call이 불리게 되는 매 순간마다 process는 context switch가 착수되는 것이다.

- 프로그램과 OS간의 interface는 `system call의 집합`으로 정의된다



What is context switch??

<br>

## 그래서 이 수업에서 배우는 것은?

시스템 호출 요약

- 파일 관련 시스템 호출
  - open(), close(), read(), write(), dup(), seek(), …

- 프로세스 관련 시스템 호출
  - fork(), exec(), exit(), wait(), getpid(), getppid(), …

- 시그널 관련 시스템 호출
  - signal(), alarm(), kill(), sleep(), …
- IPC 관련 시스템 호출
  - pipe(), socket()

<br>

## 시스템 호출과 라이브러리 함수 정리

- 시스템 호출(System Calls)
  - Unix kernal에게 서비스를 요청하는 호출
  - Unix man의 Section 2에 설명되어 있음
  - C 함수 처럼 호출될 수 있음
    - C 함수 처럼 호출될 수 있으나 context switch 일어난 다는 것이 C 라이브러리 함수와의 차이점이다.
- C 라이브러리 함수(Libraray Functions)
  - C 라이브러리 함수는 보통 system call을 포장해 놓은 함수이다.
  - 보통 내부에서 system call을 함.

<br>

## Linux 장점

- 풍부하고 다양한 하드웨어를 효과적으로 지원
  - 대부분의 하드웨어를 지원하는 추세(PC, Work station, server...)
- 놀라운 성능 및 안정성
- 인터넷에 맞는 강력한 network 구축
- 다양한 응용 프로그램 개발이 됨
- 무료 배포판으로 Ununtu등이 쓰임

















