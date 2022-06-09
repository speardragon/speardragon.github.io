---
layout: single
title: "[System Programming] 9-3장. 시스템 부팅"
categories: ['System', 'System Programming']
tag: ['booting']
---

<br>

## 시스템 부팅

- 부팅: 커널 이미지를 로딩, 커널 시작 
- 시스템 부팅은 fork/exec 시스템 호출을 통해 이루어진다. 
- 커널은 우선 swapper 프로세스를 생성함

![image](https://user-images.githubusercontent.com/79521972/168194104-4aa3d82d-2f61-4e80-9196-c577e1567260.png)

login process의 역할은 로그인을 할 수 있도록 -> 로그인이 끝나면 shell 프로세스 실행

<br>

## 시스템 부팅(시험)

- The first process(sched ) with pid =0 is created during boot time, and fork/exec twice. 
  - 0 sched 
  - 1 init 
  - 2 pagedaemon 
- Process ID 0 : swapper (scheduler process) 
  - system process 
  - part of the kernel 
  - no program on disk corresponds to this process 
    - code 자체가 kernel의 일부분이기 때문에 별도의 swapper 실행파일이 존재하지 않는 것이다.
  - 커널 내부에서 만들어진 프로세스로 프로세스 스케줄링을 한다
  
- Process ID 1: init process (초기화 프로세스) 
  - invoked by the kernel (/etc/init or /sbin/init) 
  - /etc/inittab 파일에 기술된 대로 시스템을 초기화 
    - reads the system initialization files (/etc/rc*)  - 초기화 할 내용
    - brings the system to a certain state (multi-user) 
    - creates processes based upon script files getty, login process, mounting file systems, start daemon processes 
- Process ID 2: pagedaemon 
  - supports the paging of the virtual memory system 
  - kernel process



<br>

## Daemons

- A daemon is a process that runs in the background, not connecting to any controlling terminal. 
- Daemons are normally started at boot time, are run as root or some other special user (such as apache or postfix), and handle system-level tasks 
- A daemon has two general requirements: 
  - it must run as a child of init, and 
  - it must not be connected to a terminal
    - background에서 실행 되어야 하기 때문

<br>

## 시스템 부팅

- getty 프로세스 (/etc/getty) 
  - 터미널 디바이스 오픈 
  - 로그인 프롬프트를 내고 키보드 입력(사용자 ID)을 감지한다. 
  - **exec** /bin/login, after entering a response to the login prompt. 
- login 프로세스 
  - 사용자의 로그인 아이디 및 패스워드를 검사 (/etc/passwd) 
  - 성공 시: exec /bin/sh or /bin/csh (쉘 프로그램) 
  - set the evnironment variables like HOME, LOGNAME, PATH... 
- shell 프로세스 
  - 시작 파일을 실행한 후에 쉘 프롬프트를 내고 사용자로부터 명령어를 기다린다

<br>

## BSD Terminal Logins

- The system administrator creates a file, usually /etc/ttys, that has one line per terminal device. 
- Each line specifies the name of the device and other parameters that are passed to the getty program 
- The init process reads the file /etc/ttys and, for every terminal device that allows a login, does a fork followed by an exec of the program getty 
- Linux Terminal Logins 
  - The Linux login procedure is very similar to the BSD procedure

<br>

## 터미널 로그인 과정

1. . init forks once per terminal. 
2. each child of init execs getty.

> /etc/ttys: 1 line per terminal device Each line specifies the name of the device and other parameters that has passed to getty program

![image](https://user-images.githubusercontent.com/79521972/168194595-ea636df1-822a-4997-9c43-860cf8f016be.png)

- Processes invoked by init to allow terminal logins. 
- All the processes in this slide have a real user ID 0, and effective user ID 0 – superuser privileges

<br>

## 로그인 후 상태

3. **getty** opens for terminals for reading and writing and then waits for us to enter our user name.

4. When we enter our user name, getty execs login.
   process ID 1



![image](https://user-images.githubusercontent.com/79521972/168476445-08da7261-8817-42a4-a53b-3161ec391899.png)

State of processes after login has been invoked.



<br>

## Terminal Logins

5.  login reads password and authenticates.
6. If we log in correctly, login changes to our home directory, changes ownership of our terminal device, and initializes environment variables.
7.  login execs our login shell, execl("/bin/bash", "-bash", 0);



![image](https://user-images.githubusercontent.com/79521972/168476488-baed4cf4-e6dc-4ec9-8c14-d8b096055292.png)

Processes after everything is set.

<br>

## Network Logins

기본적인 방법은 대동소이 하다.

- inetd 
  - waits for most network connections 
  - sometimes called the Internet superserver 
- As part of the system start-up, init (fork/exec) invokes a shell that executes the shell script /etc/rc. 
  - One of the daemons that is started by this shell script is inetd. 
  - Once the shell script terminates, the parent process of inetd becomes init; 
  - inetd waits for TCP/IP connection requests to arrive at the host. 
  - When a connection request arrives, inetd does a fork and exec of the appropriate program (Telnet)

<br>

## Network Logins (Telnet)

- Let‘s assume that a TCP connection request arrives for the TELNET server. 
  - TELNET is a remote login application that uses the TCP protocol. 
  - A user on another host or on the same host initiates the login by starting the TELNET client: 
    - telnet hostname 
  - The client opens a TCP connection to hostname, and the program that‘s started on hostname is called the TELNET server. 
  - The client and the server then exchange data across the TCP connection using the TELNET application protocol. 
  - What has happened is that the user who started the client program is now logged in to the server‘s host.

<br>

## TELENT 서버

The sequence of processes involved in executing the TELNET server, called telnetd

![image](https://user-images.githubusercontent.com/79521972/168195574-61d1b7ea-3095-470d-ba5c-52a3a3dac2ea.png)



<br>

## Process Groups (review)

session part를 위한 remind

- Each process is a member of a process group, which is a collection of one or more processes generally associated with each other for the purposes of **job control** 
- The primary attribute of a process group is that signals may be sent to all processes in the group: a single action can terminate, stop, or continue all processes in the same process group 
- Each process group is identified by a process group ID (pgid), and has a process group leader 
- The process group ID is equal to the pid of the process group leader. 
- Process groups exist so long as they have one remaining member. 
- Even if the process group leader terminates, the process group continues to exist

<br>

## Sessions

- When a new user first logs into a machine, the login process creates **a new session** that consists of a single process, the user‘s **login shell**. 
- The login shell functions as the session leader. The pid of the session leader is used as the session ID. 
- A session is a collection of one or more process groups. 
- Sessions arrange a logged-in user‘s activities, and associate that user with a controlling terminal, which is a specific tty device that handles the user‘s terminal I/O. 
- Consequently, sessions are largely the business of shells

<br>

## Sessions and Process Groups

- On a given system, there are many sessions: 
  - one for each user login session, and others for processes not tied to user login sessions, such as daemons. 
  - 로그인을 위해서 만들어지는 것이 session
  
- Daemons tend to create their own sessions to avoid the issues of association with other sessions that may exit. 
- pid_t setsid(void);



<br>

## Sessions and Process Groups

- Session 
  - login 하는 사용자마다 하나씩 만들어짐
  
  - A collection of one or more process groups. 
    - login shell process는 자동으로 생성 
    - login shell은 session과 같이 만들어지고 계속해서 살아 있지만 다른 process group은 만들어졌다 사라졌다 한다.
    
  - $ proc1 | proc2 & 
  
  - $ proc3 | proc4 | proc5
  

![image](https://user-images.githubusercontent.com/79521972/168476690-f1899c2a-eefb-45fd-8594-e7a8d6abca0c.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/168195937-a7de9567-d33e-4505-9ea0-f9bb605cce00.png)



<br>

- Session example

![image](https://user-images.githubusercontent.com/79521972/168196012-91b9d550-98db-40cc-9b4d-ff7ef680624e.png)

session은 모두 같다.

shell은 별도의 프로세스 그룹

다른 프로세스 그룹의 parent는 shell.

<br>

## Sessions

```c
#include <unistd.h>

pid_t setsid(void);
		Returns: process group ID if OK, -1 on error 
```

- create a new session. 
  - The calling process becomes the leader of the new session. 
  - The calling process becomes the process group leader of the new process group.

<br>

```c
#include <unistd.h>

pid_t getsid(pid_t pid);
	 Returns: session leader's process group ID if OK, 1 on error 
```

- returns the session ID of the process with pid. 
  - getsid(0) returns the session ID of the calling process.

<br>

## Controlling terminal

- controlling terminal 
  - **A session can have a single controlling terminal**. 
    - Controlling terminal is usually **the terminal device**. 
    - /dev/tty 
  - A session may have a single foreground process group and one or more background process groups. 
  - The session leader that established the connection to the controlling terminal is called **controlling process**. 
  - **interrupt or quit signal** are sent to all processes in the foreground process group. 
  - **hang-up signal** is sent to the controlling process

- Process groups and sessions showing controlling terminal

![image](https://user-images.githubusercontent.com/79521972/168196311-ba1e3f29-57eb-4919-a50c-fb9ac5f373fe.png)

<br>

## Interaction with the terminal driver(생략)

- A special terminal character affects the foreground job: 
  - the suspend key (typically Control-Z). 
- Entering this character causes the terminal driver to send the SIGTSTP signal to all processes in the foreground process group. 
- The jobs in any background process groups aren‘t affected. 
- The terminal driver looks for three special characters, which generate signals to the foreground process group. 
  - The interrupt character (typically DELETE or Control-C) generates SIGINT. 
  - The quit character (typically Control-backslash) generates SIGQUIT. 
  - The suspend character (typically Control-Z) generates SIGTSTP.

<br>

- Another job control condition that must be handled by the terminal driver. 
  - Since we can have a foreground job and one or more background jobs, which of these receives the characters that we enter at the terminal? 
  - Only the foreground job receives terminal input. 
  - It is not an error for a background job to try to read from the terminal,
  - but the terminal driver detects this and sends a special signal to the background job: SIGTTIN. 
  - This signal normally stops the background job; by using the shell, we are notified of this event and can bring the job into the foreground so that it can read from the terminal. 



<br>

## Login and session summary

![image](https://user-images.githubusercontent.com/79521972/168196621-4d8dbf0b-7266-4a06-bd44-c26d2874c9c7.png)

getty를 통해 login을 하면 shell이 fork/exec을 한다.

<br>

## 핵심 개념

- 프로세스는 실행 중인 프로그램이다. 
- fork() 시스템 호출은 부모 프로세스를 똑같이 복제하여 새로운 자식 프로세스를 생성한다. 
- exec() 시스템 호출은 프로세스 내의 프로그램을 새로운 프로그램으로 대치하여 새로운 프로그램을 실행시킨다. 
- 시스템 부팅은 fork/exec 시스템 호출을 통해 이루어진다. 
- 시그널은 예기치 않은 사건이 발생할 때 이를 알리는 소프트웨어 인터럽트이다.