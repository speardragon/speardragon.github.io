---
layout: single
title: "[System Programming] 11-1장. 시그널"
categories: ['System', 'System Programming']
tag: ['Signal']
---

<br>

# 11.1 시그널

## Introduction

- Signals 
  - **Software interrupts** : 소프트웨어 프로그래밍에서 굉장히 중요한 이벤트
  - provides a way of handling asynchronous events 
    - 프로세스가 실행 중간에 예기치 않은 사건이 발생할 때 이를 알리는 수단 
    - E.g. A user types the interrupt key to stop a program. 
- Signal name 
  - Begins with "SIG". 
    - E.g. SIGABRT, SIGTERM, SIGALRM, … 
  - Is defined by positive integer constants in   
    - E.g. #define SIGHUP 1 
    - Depends on architecture and OS.

<br>

CPU 내부에서 발생하는 interrupt를 software interrupt라고 한다.

<br>

- Examples of signal generation 
  - When user press "Ctrl-C" on the terminal. (키보드로부터 종료 요청) 
    - Generates SIGINT signal. 
  - When user press "Ctrl-Z" on the terminal. (키보드로부터 정지 요청) 
    - Generates SIGSTP signal. 
  - When executes an invalid memory references. 
    - Generates SIGSEGV signal. (SEGmentation Violation) 
  - When superuser want to kill a process. 
    - Generates SIGKILL signal. 
  - When a process writes to a pipe after the reader has terminated. 
    - Generates SIGPIPE signal.

![image](https://user-images.githubusercontent.com/79521972/169321229-8f21f8f9-6c32-4bd2-9bed-41c8b17cc1f0.png)



<br>

## 주요 시그널 /usr/include/signal.h

![image](https://user-images.githubusercontent.com/79521972/169321381-4b746527-f91f-4921-b740-d721b93bb23c.png)

## 주요 시그널

![image](https://user-images.githubusercontent.com/79521972/169321457-34c73c6c-c715-4eb0-8460-2905d138bfd5.png)



<br>

## 시그널 생성 이유

- **터미널에서 생성된 시그널** 
  - 키보드로부터 인터럽트 CTRL-C -> SIGINT 
  - 키보드로부터 정지 CTRL-Z -> SIGSTP 
- **하드웨어 예외가 생성하는 시그널** 
  - 0으로 나누기  -> SIGFPE 
  - 유효하지 않는 메모리 참조 -> SIGSEGV 
  - 부동소수점 오류 -> SIGFPE 
  - 정전 -> SIGPWR 
  - Ilegal instruction 실행, 
  - 접근 불가 메모리 접근 -> SIGSEGV 
- **kill() 시스템 호출** 
  - 프로세스(그룹)에 시그널 보내는 시스템 호출 
  - 프로세스의 소유자이거나 슈퍼유저이어야 한다.



<br>

## 시그널 종류

- 소프트웨어 이벤트 발생 (software interrupt)
  - 타이머 만료 
  - SIGALRM: 알람 시계 울림 
  - SIGPIPE: 끊어진 파이프 
  - SIGCHLD: 자식 프로세스가 끝났을 때 부모에 전달되는 시그널 
  -  SIGCHLD: 자식 프로세스 종료  
  - CPU time slice expire
    - 어떤 프로세스가 CPU를 10초 동안 쓴다고 가정, (IO는 안하고) -> CPU 활용 측면에서 좋을 진 몰라도 멀티 유저 프로세스 측면에서는 좋지 않은 것이다. 이 때 CPU를 사용할 수 있는 최대 시간이 정해진 것이다.



<br>

## Introduction

- Disposition of the signal(called action). 
  - **Ignore the signal** 
    - **SIGKILL** and **SIGSTOP** cannot be ignored. 
  - **Catch the signal** 
    - We should tell the kernel to call a **signal handler function** whenever the signal occurs. 
  - **Execute the default action** 
    - The default action for most signals is **to terminate**.



<br>

## 시그널 기본 처리

- **시그널 무시(ignore)** 
- 프로세스 종료 
  - 비정상 종료, exit()에 의한 정상종료와 다름 
- 코어 덤프 파일 생성 및 프로세스 종료 
  - 프로세스의 가상 메모리 이미지 포함, 
  - 프로세스 종료 시점의 상태 점검을 위해 디버거가 사용 
- 프로세스 중지(suspend) 
- Stopped 프로세스 계속(resume)

<br>

## Signals

- Signals for terminating processes 
  - SIGHUP 
    - This signal is sent to the controlling process(session leader) whenever the session‘s terminal disconnects. 
    - The kernel also sends this signal to each process in the foreground process group when the session leader terminates. 
    - The default action is to terminate, which makes sense—the signal suggests that the user has logged out.

- Signals for terminating processes(cont.) 
  - SIGINT 
    - This signal is sent to all processes in the foreground process group when the user enters the interrupt character (usually Ctrl-C). 
    - It is often used to terminate a runaway program. 
    - The default behavior is to terminate; however, processes can elect to catch and handle this signal, and generally do so to clean up before terminating
  - SIGQUIT 
    - Is similar to SIGINT, but generates a core file. 
    - The kernel raises this signal for all processes in the foreground process group when the user provides the terminal quit character (usually Ctrl-\). 
    - The default action is to terminate the processes, and generate a core dump (termination with core)
  

![image](https://user-images.githubusercontent.com/79521972/169322735-523461a6-b9ee-4bb3-b6b3-ec874c1f2146.png)



- Signals for terminating processes(cont.) 
  - SIGABRT 
    - abort( ) function sends this signal to the process that invokes it. 
    - abnormal termination 
    - process then terminates and generates a core file. 
    - In Linux, assertions such as assert( ) call abort( ) when the conditional fails. 
  - SIGKILL 
    - This signal is sent from the kill( ) system call 
    - The default action is to terminate the process 
    - cannot be caught or ignored. 
  - SIGTERM 
    - default signal sent out by the kill command. 
    - Can be caught 
    - Terminate the process

- Signals for terminating processes(cont.) 
  - SIGCHLD(or SIGCLD) 
    - When a process terminates, it is sent to parent. 
    - Ignore 
    - The parent must catch using wait(). 
    - A handler for this signal generally calls wait( ) to determine the child‘s pid and exit code.

- Signals for suspending or resuming. 
  - SIGCONT 
    - The kernel sends this signal to a process when the process is resumed after being stopped 
    - By default, this signal is ignored, but processes can catch it if they want to perform an action after being continued. 
    - This signal is commonly used by terminals or editors, which wish to refresh the screen 
  - SIGSTOP 
    - This signal is sent only by kill( ).  
    - It unconditionally stops a process, and 
    - cannot be caught or ignored.

- Signals for suspending or resuming(cont). 
  - SIGTSTP 
    - The kernel sends this signal to all processes in the foreground process group when the user provides the suspend character (usually Ctrl-Z). 
    - Suspend 
  - SIGTTIN 
    - This signal is sent to a process that is in the background when it attempts to read from its controlling terminal. 
    - The default action is to stop the process. 
  - SIGTTOU 
    - This signal is sent to a process that is in the background when it attempts to write to its controlling terminal. 
    - The default action is to stop the process

- Signals triggered by a physical circumstance 
  - SIGILL 
    - illegal hardware instruction 
    - Terminate 
  - SIGTRAP 
    - An implementation-defined hardware fault. 
    - use this signal to transfer control to a debugger when a breakpoint instruction is executed. 
    - terminate with core

- SIGBUS 
  - kernel raises signal when the process incurs a hardware fault other than memory protection, which generates a SIGSEGV. 
  - On traditional Unix systems, this signal represented various irrecoverable errors, such as unaligned memory access. 
  - Linux kernel, however, fixes most of these errors automatically, without generating the signal. 
  - kernel raises this signal when a process improperly accesses a region of memory created via mmap
  - Unless this signal is caught, the kernel will terminate the process, and generate a core dump

- Signals triggered by a physical circumstance(cont.) 
  - SIGFPE 
    - Despite its name, this signal represents any arithmetic exception, and not solely those related to floating-point operations. 
    - Exceptions include overflows, underflows, and division by zero. 
    - The default action is to terminate the process and generate a core file, but processes may catch and handle this signal if they want. 
  - SIGSEGV 
    - This signal, whose name derives from segmentation violation, is sent to a process when it attempts an invalid memory access. 
    - This includes accessing unmapped memory, reading from memory that is not read-enabled, executing code in memory that is not execute-enabled, or writing to memory that is not write-enabled. 
    - Processes may catch and handle this signal, but the default action is to terminate the process and generate a core dump.

- Signals available for use by the programmer 
  - SIGUSR1, SIGUSR2 
    - User-defined signals, for use in application programs 
    - the kernel never raise them. 
    - default action is to terminate the process 
    - Signal generated when a pipe is closed 
  - SIGPIPE 
    - If a process writes to a pipe, but the reader has terminated, the kernel raises this signal. 
    - The default action is to terminate the process

<br>

## Other Signals

- SIGALRM 
  - The alarm( ) and settimer( ) (with the ITIMER_REAL flag) functions send this signal to the process that invoked them when an alarm expires. 
- SIGPROF 
  - The settimer( ) function, when used with the ITIMER_PROF flag, generates this signal when a profiling timer expires. 
  - The default action is to terminate the process. 
- SIGPWR 
  - This signal is system-dependent. On Linux, it represents a low-battery condition (such as in an uninterruptible power supply, or UPS). 
  - A UPS monitoring daemon sends this signal to init, which then responds by cleaning up and shutting down the system—hopefully before the power goes out!

- SIGSYS 
  - kernel sends this signal to a process when it attempts to invoke an invalid system call. 
  - This can happen if a binary is built on a newer version of the operating system (with newer versions of system calls), but then runs on an older version. 
  - Properly built binaries that make their system calls through glibc should never receive this signal. 
- SIGTRAP 
  - The kernel sends this signal to a process when it crosses a break point. 
  - Generally, debuggers catch this signal, and other processes ignore it.

- SIGURG 
  - The kernel sends this signal to a process when out-of-band (OOB) data has arrived on a socket. 
- SIGWINCH 
  - The kernel raises this signal for all processes in the foreground process group when the size of their terminal window changes. 
  - By default, processes ignore this signal 
  - A good example of a program that catches this signal is top—try resizing its window while it is running and watch how it responds. 
- SIGXFSZ 
  - The kernel raises this signal when a process exceeds its file size limit. 
  - The default action is to terminate the process, but if this signal is caught or ignored, the system call that would have resulted in the file size limit being exceeded returns -1, and sets errno to EFBIG

<br>

## alarm()

```c
#include <unistd.h>
unsigned int alarm(unsigned int sec)
//sec초 후에 프로세스에 SIGALRM 시그널이 발생되도록 설정한다.
```

- Set a timer that will expire at a specified time in the future. 
  - sec초 후에 프로세스에 SIGALRM 시그널이 발생한다. 
  - 이 시그널을 받으면 ―자명종 시계" 메시지를 출력하고 프로그램은 종료된다

![image](https://user-images.githubusercontent.com/79521972/169324027-7b9bd600-086b-4078-83aa-f1b5401dd06f.png)

- alarm(0) 
  - 이전에 설정된 알람은 취소된다. 

- Default action is to terminate the process, but most processes catch this signal. 
  - default action은 프로세스를 종료하지만 대부분 프로세스는 user defined handler 실행

- 한 프로세스 당 오직 하나의 알람만 설정할 수 있다. 
- There is only one alarm clock per process. 
  - 이전에 설정된 알람이 있으면 취소되고 남은 시간(초)을 반환한다. 
  - 이전에 설정된 알람이 없다면 0을 반환한다. 
  - If, when we call alarm, a previously registered alarm clock for the process has not yet expired, the number of seconds left is returned. The previously registered alarm clock is replaced by the new one.



<br>

## alarm.c

```c
#include <stdio.h>
/* 알람 시그널을 보여주는 프로그램 */
int main( )
{ 
    int sec = 0;
    alarm(5);
    printf("무한 루프 \n");
    while (1) { 
        sleep(1); 
        printf(―%d초 경과 \n―, ++sec);
    }
    printf("실행되지 않음 \n"); 
} 
```

alarm(5)이 호출되고 5초가 지나면 아래 while loop를 돈 후에 자명종 시계를 호출하고 종료된다.

```shell
$ alarm
무한 루프
1초 경과
2초 경과
3초 경과
4초 경과
5초 경과
자명종 시계
```

<br>

# 11.2 시그널 처리

## 시그널 처리기

- 시그널에 대한 처리함수 지정 
  - signal() 시스템 호출 
  - "이 시그널이 발생하면 이렇게 처리하라"

- signal() 시스템 호출
  - 사용자가 정의한 시그널 함수 등록


```c
#include <signal.h>
signal(int signo, void (*func)( )))
//signo에 대한 처리 함수를 func으로 지정하고 기존의 처리함수를 리턴한다
```

- 시그널 처리 함수 func 
  - **SIG_IGN** : 시그널 무시 
    - -> all signals can be ignored, except SIGKILL and SIGSTOP 
  - **SIG_DFL** : 기본 처리 
  - **사용자 정의 함수 이름** -> most are to terminate process

<br>

## 예제: almhandler.c (SIGALRM)

```c
#include <stdio.h>
#include <signal.h>
void alarmHandler();
/* 알람 시그널을 처리한다. */
int main( )
{ 
    int sec = 0;
    signal(SIGALRM,alarmHandler); //사용자 정의 핸들러 함수 등록
    alarm(5); /* 알람 시간 설정 */
    printf("무한 루프 \n");
    while (1) {
        sleep(1);
        printf(―%d초 경과 \n―, ++sec);
    }
    printf("실행되지 않음 \n");
}
void alarmHandler()
{
    printf("일어나세요\n");
    exit(0);
}
```

```shell
$ alarmhandler
무한 루프
1
1초 경과
2초 경과
3초 경과
4초 경과
5초 경과
일어나세요
```

<br>

## 예제: sigint1.c (SIGINT) 

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
void intHandler();
/* 인터럽트 시그널을 처리한다. */
int main( )
{
    if (signal(SIGINT,intHandler) == SIG_ERR) {
        fprintf (stderr, "Cannot handle SIGINT!\n"); //redundant code (예외 처리)
        exit (EXIT_FAILURE);
    }
    while (1)
        pause();
    printf("실행되지 않음 \n");
}
void intHandler(int signo)
{
    printf("인터럽트 시그널 처리\n");
    printf(―시그널 번호: %d\n―, signo);
    exit(0);
}
```

```shell
$ sigint1
^c인터럽트 시그널 처리
시그널 번호: 2
```

사용자가 CTRL-C를 누르면 SIGINT signal 발생 -> user defined handler 함수 intHandler 호출

<br>

```c
#include <signal.h>
pause()
 시그널을 받을 때까지 해당 프로세스를 잠들게 만든다.
```

<br>

## 시그널 처리기

- a process can catch neither **SIGKILL nor SIGSTOP**, so setting up a handler for either of these two signals makes no sense 
  - SIGKILL이나 SIGSTOP은 catch할 수 없는 signa이기 때문에 해당 signal에 대한 handler를 만드는 것은 의미가 없다.(make no sense)

- The handler function **must return void**, which makes sense because (unlike with normal functions) <mark>there is no standard place in the program for this function to return</mark>

<br>

## signal()

- Example
  - It possible to use **one signal handler for several signals**
  - 한 signal handler에서 여러 signal 처리 ( signo을 통해서)

```c
#include <signal.h>
void myhandler(int signo)
{
    switch (signo) {
        case SIGQUIT : printf("SIGQUIT(%d) is caught\n",SIGQUIT);
            break;
        case SIGTSTP : printf("SIGTSTP(%d) is caught\n",SIGTSTP);
            break;
        case SIGTERM : printf("SIGTERM(%d) is caught\n",SIGTERM);
            break;
        case SIGUSR1 : printf("SIGUSR1(%d) is caught\n",SIGUSR1);
            break;
        default: printf("other signal\n");
    }
    return; //원래는 종료를 시키는 것이 일반적, not return
}
int main(void)
{
    signal(SIGQUIT, myhandler);
    signal(SIGTSTP, SIG_DFL); //use default handler
    signal(SIGTERM, myhandler);
    signal(SIGUSR1, myhandler);
    for (;;)
        pause();
}

```

![image](https://user-images.githubusercontent.com/79521972/169325455-43f246d0-9fbc-42c0-a062-d5f586fee2bc.png)



- 실행

```shell
$ ./a.out
^\ 					//SIGQUIT; 프로세스 실행 중지
SIGQUIT(3) is caught
^Z 					//SIGSTP
[1]+ Stopped ./a.out
$ ps
 PID TTY TIME CMD
15554 pts/2 00:00:00 bash
15587 pts/2 00:00:00 a.out
15588 pts/2 00:00:00 ps
$ kill 15587 		//signo를 명시하지 않으면 SIGTERM 시그널을 보내 해당 프로세스를 강제 종료시킴.
SIGTERM(15) is caught
$ kill -USR1 15587
SIGUSR1(10) is caught
$
```

-USR: 사용자 정의 signal

<br>

## Waiting for a Signal, Any Signal

```c
#include <unistd.h>
int pause (void);
```

- pause( ) system call puts a process to sleep **until it receives a signal** that either is handled or terminates the process 
- pause( ) returns only if a caught signal is received, in which case the signal is handled, and pause( ) returns -1, and sets errno to EINTR (invalid signo). 
- If the kernel raises an **ignored** signal, the process **does not wake up** (never). 
- It performs only two actions. 
  - First, it puts the process in the interruptible sleep state. 
  - Next, it calls schedule( ) to invoke the Linux process scheduler to find another process to run. 
  - As the process is not actually waiting for anything, the kernel will not wake it up unless it receives a signal. 

<br>

## Ex1

```c
// register the same handler for SIGTERM and SIGINT.
// also reset the behavior for SIGPROF to the default (which is to terminate the process)
//and ignore SIGHUP (which would otherwise terminate the process):
static void signal_handler (int signo) {
    if (signo == SIGINT)
        printf ("Caught SIGINT!\n");
    else if (signo == SIGTERM)
        printf ("Caught SIGTERM!\n");
    else { /* this should never happen */
        fprintf (stderr, "Unexpected signal!\n");
        exit (EXIT_FAILURE);
    }
    exit (EXIT_SUCCESS);
}
int main (void) {
    /* Register signal_handler as our signal handler for SIGINT.*/
    if (signal (SIGINT, signal_handler) == SIG_ERR) {
        fprintf (stderr, "Cannot handle SIGINT!\n");
        exit (EXIT_FAILURE);
    }
    /*Register signal_handler as our signal handler for SIGTERM */
    if (signal (SIGTERM, signal_handler) == SIG_ERR) {
        fprintf (stderr, "Cannot handle SIGTERM!\n");
        exit (EXIT_FAILURE);
    }
    /* Reset SIGPROF's behavior to the default. */
    if (signal (SIGPROF, SIG_DFL) == SIG_ERR) {
        fprintf (stderr, "Cannot reset SIGPROF!\n");
        exit (EXIT_FAILURE);
    }
    /* Ignore SIGHUP. */
    if (signal (SIGHUP, SIG_IGN) == SIG_ERR) {
        fprintf (stderr, "Cannot ignore SIGHUP!\n");
        exit (EXIT_FAILURE);
    }
    for (;;)
        pause ( );
    Return 0;
}
```

signal handler를 user defined handler로 지정하지 않은 경우(SIG_DFL, SIG_IGN)는 사용자 정의 handler가 실행되지 않고 각자 지정된 대로 처리된다.

<br>

## Ex2

```c
include <signal.h> /* sigusr.c */
    static void sig_usr(int signo) { /* argument is signal number */
    if (signo == SIGUSR1)
        printf("received SIGUSR1\n");
    else if (signo == SIGUSR2)
        printf("received SIGUSR2\n");
    else
        perror("received signal %d\n", signo);
    return;
}
int main(void) {
    if (signal(SIGUSR1, sig_usr) == SIG_ERR)
        perror("can't catch SIGUSR1");
    if (signal(SIGUSR2, sig_usr) == SIG_ERR)
        perror("can't catch SIGUSR2");
    for ( ; ; )
        pause( );
}
```



```shell
$ a.out & // start process in background
[1] 4720 job number & process ID
$ kill –USR1 4720 // send SIGUSR1
received SIGUSR1
$ kill –USR2 4720 // send SIGUSR2
received SIGUSR2
$ kill 4720 // send SIGTERM
[1] + Terminated a.out &
```

<br>

## sigaction() 함수

- signal()보다 **정교하게** 시그널 처리기를 등록하기 위한 함수 
  - sigaction 구조체를 사용하여 정교한 시그널 처리 액션을 등록

```c
#include <signal.h>
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
// signum 시그널(SIGKILL과 SIGSTOP 제외)이 수신되었을 때, 프로세스가 취할 액션을 변경하는 데 사용된다. // 이 시그널에 대한 새로운 액션은 act가 되며, 기존의 액션은 oldact에 저장된다. 성공하면 0을 실패하면 –1를 반환한다.
```

```c
struct sigaction {
    void (*sa_handler)(int); // 시그널 처리기
    void (*sa_sigaction)(int, siginfo_t *, void *);
    sigset_t sa_mask; // 시그널을 처리하는 동안 차단할 시그널 집합
    int sa_flags;
}
```

- `void (*sa_handler)(int); // 시그널 처리기`
  - SIG_IGN : 시그널 무시 
    - -> all signals can be ignored, except SIGKILL and SIGSTOP
  - SIG_DFL : 기본 처리 
  - 사용자 정의 함수 이름

- `void (*sa_sigaction)(int, siginfo_t *, void *); `
  - sa_flags의 SA_SIGINFO 일 때 sa_handler() 대신에 호출되는 처리기

- `sigset_t sa_mask; // 시그널을 처리하는 동안 차단할 시그널 집합`

- `int sa_flags : 시그널 처리 절차 `
  - SA_SIGINFO -> sa_flag가 sa_handler 대신에 sa_sigaction을 사용함 
  - SA_NOCLDSTOP -> <mark>signum이 SIGCHILD일 경우, 자식 프로세스가 종료/중단 되었 을 때 부모 프로세스에게 SIGCHILD를 전달 하지 않음 </mark>
  - SA_ONESHOT -> 시그널을 수신하면 설정된 액션을 하고 SIG_DFL로 재설정됨 
  - SA_NOMASK -> 시그널을 처리하는 동안에 전달되는 시그널은 차단되지 않음

<br>

## 시그널 집합 핸들링 함수

- \#include  \<signal.h>
- int sigemptyset(sigset_t *set); 
  - 시그널 집합 set을 공집합으로 초기화 
- int sigfillset(sigset_t *set); 
  - 시그널 집합 set을 모든 시그널을 포함하도록 초기화 
- int sigaddset(sigset_t *set, int signum); 
  - 시그널 집합 set에 시그널 signum을 추가 
- int sigdelset(sigset_t *set, int signum); 
  - 시그널 집합 set에서 시그널 signum을 삭제 
- int sigismember(sigset_t *set, int signum); 
  - 시그널 signum이 집합 set의 원소인지 여부 반환

<br>

## sigint2.c

```c
#include <stdio.h>
#include <signal.h>
struct sigaction newact;
struct sigaction oldact;
void sigint_handler(int signo);

int main( void)
{
    newact.sa_handler = sigint_handler; // 시그널 처리기 지정
    sigfillset(&newact.sa_mask); // 모든 시그널을 차단하도록 마스크(초기화)

    // SIGINT의 처리 액션을 새로 지정, oldact에 기존 처리 액션을 저장
    sigaction(SIGINT, &newact, &oldact); 
    while(1)
    {
        printf( "Ctrl-C를 눌러 보세요 !\n"); 
        sleep(1); 
    }
}
/* SIGINT 처리 함수 */ 
void sigint_handler(int signo) 
{ 
    printf("%d 번 시그널 처리!\n", signo);
    printf("또 누르면 종료됩니다.\n");
    sigaction(SIGINT, &oldact, NULL); // 기존 처리 액션으로 변경
}
```

```shell
$ sigint2
Ctrl-C를 눌러보세요!
Ctrl-C를 눌러보세요!
^c
2번 시그널 처리!
또 누르면 종료됩니다.
Ctrl-C를 눌러보세요!
^c
```

사용자가 Ctrl-C를 누르게 되면 종료를 하게 된다.



<br>

## Execution and Inheritance

- When a process is first executed, **all signals** are set to their **default actions**, unless the parent process is ignoring them; in this case, the newly created process will also **ignore** those signals. 
  - Put another way, any signal caught by the parent is reset to the **default action** in the new process, and all other signals remain the same. 
    - 사용자 정의도 defaul로 세팅된다.
    - This makes sense because a freshly executed process **does not share the address space of its parent**, and thus any registered signal handlers may not exist. 
- When a process calls fork( ), the child **inherits** the exact same signal semantics as the parent. 
  - This also makes sense, as the child and parent share an address space, and thus the parent‘s signal handlers exist in the child

- when the shell executes a process **"in the background"** (or when another background process executes another process), the newly executed process **should ignore the interrupt and quit character** 
  - Catches the signals in foreground process 
  
- Thus, before a shell executes a background process, it should set SIGINT and SIGQUIT to SIG_IGN. 
  - SIGQUIT나 SIGINT는 키보드로 받는 입력을 받을 때 이기 때문에 background에서 받을 수 없는 것이다. 

- It is therefore common for programs that handle these signals to first check to make sure they are not ignored

<br>