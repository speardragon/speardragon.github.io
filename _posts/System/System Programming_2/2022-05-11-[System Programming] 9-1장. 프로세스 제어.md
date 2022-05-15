---
layout: single
title: "[System Programming] 9-1장. 프로세스 제어"
categories: ['System', 'System Programming']
tag: ['Process control']
---

<br>

## set-user-id 실행파일 (Review)

- 특별한 실행권한 set-user-id(set user ID upon execution) 
  - set-user-id 설정된 실행파일을 실행하면 
  - 이 프로세스의 유효 사용자 ID는 그 실행파일의 소유자로 바뀜. 
    - 프로세스의 실제 사용자 ID (real user ID)는 그 프로세스를 실행한 원래 사용자의 사용자 ID로 설정된다. 
    - 예를 들어 chang이라는 사용자 ID로 로그인하여 어떤 프로그램을 실행 시키면 그 프로세스의 실제 사용자 ID는 chang이 된다. 
  - 이 프로세스는 실행되는 동안 그 파일의 소유자 권한을 갖게 됨. 
- 예 : /usr/bin/passwd 명령어 실행 파일 
  - set-user-id 실행권한이 설정된 실행파일이며 소유자는 root 
  - 일반 사용자가 쉘에서 이 파일을 실행하게 되면, 이 프로세스의 유효 사용자 ID는 root가 됨. 
  - /etc/passwd처럼 root만 수정할 수 있는 파일의 접근 및 수정 가능

<br>

## Set-user-ID and set-group-ID (Review)

- passwd program를 이용하여 password를 바꿀 경우 
  - passwd program을 실행하는 도중 잠정적으로 root의 권한을 부여받음. 
    - Real user ID of passwd program: user 새로운 프로세스 생성 시 부모의 사용자 ID와 그룹 ID를 그대로 물려 받음 
    - Effective user ID of passwd program: root

![image](https://user-images.githubusercontent.com/79521972/168040951-edbf54bf-3a4a-4c73-bede-202150cce55c.png)

<br>



# 9.1 프로세스 생성

## 프로세스 생성

- fork() 시스템 호출 
  - 부모 프로세스를 똑같이 복제하여 새로운 자식 프로세스를 생성 
  - 자기복제(自己複製)

```c
#include <sys/types.h>
#include <unistd.h>
pid_t fork(void);
```

- 새로운 자식 프로세스를 생성한다. 
- It is called once but returns twice. 
- 자식 프로세스에게는 0을 리턴하고 부모 프로세스에게는 자식 프로 세스 ID를 리턴한다



<br>

- The child is a copy of parent. 

  - Child gets a copy of the parent‘s data, heap, and stack. 

  - Parent and child often share the text segment.
  
  - data, heap, stack은 공유하지 않는다.
    - global 변수는 data 영역이기 때문에 이를 공유하지 않는 것
  

![image](https://user-images.githubusercontent.com/79521972/168041237-acd87a67-ab92-40f4-839f-7fc8a6819b9f.png)

부모 프로세스의 pid에는 자식 프로세스의 pid인 15066이 들어가고 자식 프로세스의 pid에는 0의 값이 들어가게 된다.

<br>

## 프로세스 주소공간

![image](https://user-images.githubusercontent.com/79521972/168041327-27db91ef-096d-4a90-9973-1afe1776f776.png)

bss: generally 0으로 초기화

<br>

## 프로세스 생성

- fork()는 한 번 호출되지만 두 번 리턴한다. 
  - 자식 프로세스에게는 0을 리턴하고 
  - 부모 프로세스에게는 자식 프로세스 ID를 리턴한다. 
  - 부모 프로세스와 자식 프로세스는 병행적(concurrently)으로 각각 fork() 이후 문장의 실행을 계속한다.
  - 즉, tread control이 두 개 이상이 된다.



<br>

## fork1.c

```c
#include <stdio.h>
#include <unistd.h>
/* 자식 프로세스를 생성한다. */
int main()
{
     int pid;
     printf("[%d] 프로세스 시작 \n", getpid());
     pid = fork();
     printf("[%d] 프로세스 : 리턴값 %d\n", getpid(), pid); //두 번 실행
}
```

```shell
$ fork1
[15065] 프로세스 시작
[15065] 프로세스 : 반환값 15066
[15066] 프로세스 : 반환값 0
```



<br>

## 부모 프로세스와 자식 프로세스 구분

- fork() 호출 후에 리턴값이 다르므로 이 리턴값을 이용하여 
- 부모 프로세스와 자식 프로세스를 구별하고 
- 서로 다른 일을 하도록 할 수 있다. 

```
pid = fork();
if ( pid == 0 )
{ 자식 프로세스의 실행 코드 }
else
{ 부모 프로세스의 실행 코드 }
```



<br>

## fork2.c

```c
#include <stdlib.h>
#include <stdio.h>
/* 부모 프로세스가 자식 프로세스를 생성하고 서로 다른 메시지를 프린트 */
int main() 
{
     int pid; 
     pid = fork();
     if (pid ==0) { // 자식 프로세스
         printf("[Child] : Hello, world pid=%d\n", getpid());
     }
     else { // 부모 프로세스
         printf("[Parent] : Hello, world pid=%d\n", getpid());
     }
}
```

```shell
$ fork2
[Parent] : Hello, world pid=15799
[Child] : Hello, world pid=15800
```

parent와 chile의 실행 순서는 중요하지 않음

<br>

## fork3.c: 두 개의 자식 프로세스 생성

```c
#include <stdlib.h>
#include <stdio.h>
/* 부모 프로세스가 두 개의 자식 프로세스를 생성한다. */
int main()
{ 
    int pid1, pid2;
    pid1 = fork();
    if (pid1 == 0) {
        printf("[Child 1] : Hello, world ! pid=%d\n", getpid());
        exit(0); //child process 종료
    }
    pid2 = fork();
    if (pid2 == 0) {
        printf("[Child 2] : Hello, world ! pid=%d\n", getpid());
        exit(0);
    }
}
```

```shell
$ fork3
[Child1] : Hello, world! pid=15741
[Child2] : Hello, world! pid=15742
```



<br>

## fork() 

13페이지

- Example

```c
#include "apue.h"

int glob = 6; /* external variable in initialized data */
char buf[] = "a write to stdout\n";

int main(void)
{
    int var; /* automatic variable on the stack */ //uninitialized data
    pid_t pid;
    
    var = 88;
    if (write(STDOUT_FILENO, buf, sizeof(buf)-1) != sizeof(buf)-1)
        err_sys("write error");
    printf("before fork\n"); /* we don't flush stdout */
    
    if ((pid = fork()) < 0) {
        err_sys("fork error");
    } else if (pid == 0) { /* child */
        glob++; /* modify variables */
        var++;
    } else {
        sleep(2); /* parent */
    }
    
    printf("pid = %d, glob = %d, var = %d\n", getpid(), glob, var);
    exit(0);
}
```

- 실행

```shell
$ ./a.out
a write to stdout
before fork
pid = 430, glob = 7, var = 89
pid = 429, glob = 6, var = 88
$ ./a.out > temp.out
$ cat temp.out
a write to stdout
before fork
pid = 432, glob = 7, var = 89
before fork
pid = 431, glob = 6, var = 88
$
```

> We never know if the child starts executing before the parent or vice versa. - It depends on the process scheduling algorithm.



<br>

## fork()

- Information **shared** by child and parent. 
  - real-uid(gid), effective-uid(gid) 
  - controlling terminal 
  - current working directory, root directory 
  - signal handlers : 이벤트 발생 통지 수단
  - environment 
  - resource limits 
- **differences** between child and parent. 
  - the return value from fork. 
  - PID and PPID 
  - child‘s resource utilizations are set to 0. 
  - pending signals - signal 그 자체는 공유 x

- Two main reasons for fork to fail 
  - if there are already too many processes in the system. 
  - if the total number of processes for this real user ID exceeds the system‘s limit. 
- Two uses for fork (왜 fork를 하는가?)
  - A process wants to duplicate itself. 
    - Parent and child can each execute different sections of code at the same time. 
    - Common for network servers. 
  - A process wants to execute a different program. 
    - Common for shells. 
    - Child does exec() right after it returns from fork().

<br>

## 쉘의 구조 (revisit) 

```c
While (true) {
    display prompt
    read command (command, params) // Get command or execution file
    if (fork() != 0) then { // Execute command or execution file
		wait(&status); // fork a child, wait for a child to finish
		exit()
	}
	else
		execve(command, parmas) // 자식 프로세스
	} // 입력된 명령어를 실행
```

```shell
$ ls
. .. /etc
/home /usr /dev
$ a.out
….
$
```



<br>

## SIGCHLD

- When a process terminates, the kernel sends the signal SIGCHLD to the parent. 
  - By default, this signal is ignored, and no action is taken by the parent. 

- Processes can elect to handle this signal, however, via the signal( ) or sigaction( ) system calls 
- The SIGCHLD signal may be generated and dispatched at any time, as a child‘s termination is **asynchronous** with respect to its parent. 
  - 따라서 발생 시점 예측 불가.

- But often, the parent wants to learn more about its child‘s termination, or even explicitly wait for the event‘s occurrence. 
- This is possible with the **wait** system calls



<br>

## wait() and waitpid()

- A process that calls wait() or waitpid() 
  - Block, if all of its children are still running. 
  - Return immediately if a child has terminated. 
  - Return immediately with an error, if it doesn't have any child processes.
    - fork를 하지 않고 wait -> 즉시 return

![image](https://user-images.githubusercontent.com/79521972/168042761-9c5f5129-89a4-41d8-9aa4-eca3704d3c21.png)



<br>

## 프로세스 기다리기: wait()

- 자식 프로세스 중의 하나가 끝날 때까지 기다린다. 
  - 끝난 자식 프로세스의 종료 코드가 status에 저장되며 
  - 끝난 자식 프로세스의 번호를 리턴한다. 오류시 -1 리턴

```c
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait(int *status);
pid_t waitpid(pid_t pid, int *statloc, int options);
```

![image](https://user-images.githubusercontent.com/79521972/168042879-498d3d6d-22ac-447b-b392-156d249516ff.png)

파란색 선은 block하고 있다는 뜻

<br>

## wait()

```c
#include <sys/wait.h>

pid_t wait(int *statloc);
 		Both return: process ID if OK, 0 (see later), or -1 on error 
```

- wait() 
  - If a child has already terminated and is a zombie, wait **returns immediately** with that child's status. 
  -  Otherwise, it blocks the caller until a child terminates. 
  - If the caller blocks and has multiple children, wait returns when one terminates. 
- statloc argument 
  - Pointer to integer 
  - Store termination status (in case of normal termination) in the location pointed to by staloc. 
    - And signal number (abnormal termination) 
      - 왜 비정상 종료가 되었는지를 나타내는 정보
    - And if core file was created



<br>

## forkwait.c

```c
#include <unistd.h> …
/* 부모 프로세스가 자식 프로세스를 생성하고 끝나기를 기다린다. */
int main()
{
    int pid, child, status;
    printf("[%d] 부모 프로세스 시작 \n", getpid( )); 
    pid = fork(); 
    if (pid == 0) { 
        printf("[%d] 자식 프로세스 시작 \n", getpid( )); 
        exit(1);
    }
    child = wait(&status); // 자식 프로세스가 끝나기를 기다린다.
    printf("[%d] 자식 프로세스 %d 종료 \n", getpid(), child);
    printf("\t종료 코드 %d\n", status>>8); //종료코드값은 4바이트 중 3번째 바이트임 -> 8 shift
}
```

```shell
$ forkwait
[15943] 부모 프로세스 시작
[15944] 자식 프로세스 시작
[15943] 자식 프로세스 15944 종료
종료코드 1
```

<br>

## macros for interpreting the termination status

매크로 함수

- If not NULL, the status pointer contains additional information about the child (how the process terminated) 
- #include  \<sys/wait.h>
- WIFEXITED (status); 
  - returns true if the child process terminated normally 
  - 정상적으로 종료한 경우에 참(TRUE) 값을 리턴 
  - WEXITSATUS(status) 
    - WIFEXITED()가 True이면 exit 함수의 인자에서 하위 8 비트 값을 리턴 
- WIFSIGNALED (status); 
  - returns **true** if a signal that it **did not catch** caused the child process‘ termination (abnormal) 
  - WTERMSIG(status) : if WIFSIGNALED is True, fetch the signal number that caused the termination.



- WIFSTOPPED (status); 
  - Child 프로세스가 실행이 일시 중단된 경우에 참 값을 리턴 
  - WSTOPSIG(status) 
    - if WIFSTOPPED is True, Child 프로세스의 실행을 일시 중단시킨 시그널 번호를 리턴 
- WIFCONTINUED (status); 
  - return true if the process was continued, and is currently being traced via the ptrace( ) system call\
  - 중요 x

<br>

- A process that calls wait or waitpid can 
  - block (if all of its children are still running), or 
    - wait() is called at any random point in time 
  - return immediately with the termination status of a child, (SIGCHLD-asynchronous notification) or 
    - 예를 들어 zombie process 같은 경우
  - return immediately with an error (**if it doesn‘t have any child processes**) errno value: 
  - **ECHILD** - The calling process does not have any children.

<br>

## wait(): macros for interpreting the termination status

- Example

```c
#include <sys/types.h>
#include <sys/wait.h>
void pr_exit(int status)
{
    if (WIFEXITED(status)) //case 1
        printf("normal termination, exit status = %d\n", WEXITSTATUS(status));
    else if (WIFSIGNALED(status)) // case 2
        printf("abnormal termination, signal number = %d\n", WTERMSIG(status));
    else if (WIFSTOPPED(status)) // case 3
        printf("child stopped, signal number = %d\n", WSTOPSIG(status));
}
```



<br>

```c
#include <sys/types.h>
#include <sys/wait.h>
int main(void)
{
    pid_t pid;
    int status;
    
    if ((pid = fork()) < 0)
        printf("fork error");
    else if (pid == 0) /* child */
        exit(7);
    
    if (wait(&status) != pid) /* wait for child */
        printf("wait error");
    pr_exit(status); /* and print its status */ // case 1 실행
    
    if ( (pid = fork()) < 0)
        printf("fork error");
    else if (pid == 0) /* child */
        abort(); /* generates SIGABRT */ // 비정상 종료
    
    if (wait(&status) != pid) /* wait for child */
        printf("wait error");
    pr_exit(status); /* and print its status */ //case 2 실행
    
    if ( (pid = fork()) < 0)
        printf("fork error");
    else if (pid == 0) /* child */
        status /= 0; /* divide by 0 generates SIGFPE */
    
    if (wait(&status) != pid) /* wait for child */
        printf("wait error");
    pr_exit(status); /* and print its status */ // case2 실행
}
```

- 실행

```shell
$ ./a.out
normal termination, exit status = 7
abnormal termination, signal number = 6
abnormal termination, signal number = 8
$
```

```
Each signal number is defined in <signal.h>
SIGABRT: 6
SIGFPE: 8
```



<br>

## wait.c

```c
#include <unistd.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/wait.h>
int main (void){
    int status;
    pid_t pid;
    
    if (!fork ( )) //child
        return 1;
    pid = wait (&status);
    
    if (pid == -1) //error
        perror ("wait");
    printf ("pid=%d\n", pid);
    
    if (WIFEXITED (status))
        printf ("Normal termination with exit status=%d\n", WEXITSTATUS (status));
    
    if (WIFSIGNALED (status))
        printf ("Killed by signal=%d%s\n", WTERMSIG (status), WCOREDUMP (status) ? "(dumped core)" : "");
    
	if (WIFSTOPPED (status))
		printf ("Stopped by signal=%d\n", WSTOPSIG (status));
    
	if (WIFCONTINUED (status))
		printf ("Continued\n");
    
	return 0;
}
```



```shell
$ ./wait
pid=8529
Normal termination with exit status=1
```

<br>

- If, instead of having the child return, we have it call abort( ), which sends itself the SIGABRT signal, we will instead see something resembling the following:

```shell
$ ./wait
pid=8678
Killed by signal=6
```



<br>

## Waiting for a Specific Process

```c
#include <sys/types.h>
#include <sys/wait.h>
pid_t waitpid (pid_t pid, int *status, int options);
	return: process ID if OK, 0 (see later), or -1 on error
```

- Why waitpid() is required? 
  - a process has multiple children, and does not wish to wait for all of them, but **rather for a specific child process** 
  - wait() returns on termination of any of the children. 
- One solution would be to make multiple invocations of wait( ), each time noting the return value 
  - wait() 여러번 호출 -> 번거로움

- If you know the pid of the process you want to wait for, you can use the waitpid( ) system call 
- The waitpid( ) call is a more **powerful** version of wait( ). Its additional parameters allow for fine-tuning 
  - Provides some controls with options argument

<br>

## pid_t waitpid(pid_t pid, int *statloc, int options);

- The pid parameter specifies exactly which process or processes to wait for. 
- pid == -1 
  - Wait for any child process. This is the same behavior as wait( ). 
- pid > 0
  - Wait for any child process whose pid is exactly the value provided. For example, passing 500 waits for the child process with pid 500 
- pid == 0 
  - Wait for any child process that belongs to the same process group as the calling process. 
- pid < -1 
  - Wait for any child process whose process group ID is equal to the **absolute value** of this value. 
    - For example, passing -500 waits for any process in process group 500.

<br>

### waitpid option parameter

- The status parameter works identically to the sole parameter to wait( ) 
- **부모 프로세스의 대기 방법** 
- **WNOHANG** 
  - Do not block, but return immediately if no matching child process has already terminated (or stopped or continued) 
    - if a child specified by pid is not terminated. 
- **WUNTRACED** 
  - Do not block if a child specified by pid has stopped. 
  - If set, WIFSTOPPED is set, even if the calling process is not tracing the child process. 
  - 실행을 중단한 자식 프로세스의 상태값을 리턴 
  - This flag allows for the implementation of more general job control, as in a shell.
- WCONTINUED 
  - If set, WIFCONTINUED is set even if the calling process is not tracing the child process. 
  - 수행중인 자식 프로세스의 상태값 리턴 
  - As with WUNTRACED, this flag is useful for implementing a shell 
- wait (&status); 
  - is identical to waitpid (-1, &status, 0);


<br>

## wait() and waitpid()

- Example

```c
#include "apue.h"
#include <sys/wait.h>
int main(void)
{
    pid_t pid;
    if ((pid = fork()) < 0) {
        err_sys("fork error");
    } else if (pid == 0) { /* first child */
        if ((pid = fork()) < 0)
            err_sys("fork error");
        else if (pid > 0)
            exit(0); /* parent from second fork == first child */
        /* We're the second child; our parent becomes init. */
        sleep(2);
        printf("second child, parent pid = %d\n", getppid());
        exit(0);
    }
    if (waitpid(pid, NULL, 0) != pid) /* wait for first child */
        err_sys("waitpid error");
    /*
 * We're the parent (the original process); we continue executing,
 * knowing that we're not the parent of the second child.
 */
    exit(0);
}
```

```shell
$ ./a.out
$ second child, parent pid = 1
```



<br>

## PID 1742인 자식프로세스를 wait (waitpid) 자식이 종료 안된 경우 즉시 리턴

```c
int status;
pid_t pid;
pid = waitpid (1742, &status, WNOHANG); // 바로 리턴 (종료를 하지 않더라도)
if (pid == -1)
    perror ("waitpid");
else {
    printf ("pid=%d\n", pid);
    if (WIFEXITED (status)) // status check macro function
        printf ("Normal termination with exit status=%d\n", WEXITSTATUS (status));
    if (WIFSIGNALED (status))
        printf ("Killed by signal=%d%s\n",WTERMSIG (status), WCOREDUMP (status) ? " (dumped core)" : "");
}
```



<br>

## Zombies

- 부모 프로세스가 wait()이나 waitpid()를 호출하기 이전에 자식 프로세스가 이미 종료가 돼버린 프로세스를 zombie process라고 한다.

- a process that has terminated, but that has not yet been waited upon by its parent is called a "zombie"
  - 좀비 프로세스는 프로세스 테이블에만 존재 
- Zombie processes continue to consume system resources 
- wait() returns immediately with that child‘s status 
- These resources remain so that parent processes that want to check up on the status of their children can obtain information relating to the life and termination of those processes. 
- Once the parent does so, the kernel cleans up the process for good and the zombie ceases(stop) to exist.

<br>

## waitpid.c

```c
#include <sys/types.h>
…
/* 부모 프로세스가 자식 프로세스를 생성하고 끝나기를 기다린다. */
int main()
{
    int pid1, pid2, child, status;

    printf("[%d] 부모 프로세스 시작 \n", getpid( ));
    pid1 = fork();
    if (pid1 == 0) {
        printf("[%d] 자식 프로세스[1] 시작 \n", getpid( ));
        sleep(1);
        printf("[%d] 자식 프로세스[1] 종료 \n", getpid( ));
        exit(1);
    }

    pid2 = fork();
    if (pid2 == 0) {
        printf("[%d] 자식 프로세스 [2] 시작 \n", getpid( ));
        sleep(2);
        printf("[%d] 자식 프로세스 [2] 종료 \n", getpid( ));
        exit(2);
    }
    // 자식 프로세스 #1의 종료를 기다린다.
    child = waitpid(pid1, &status, 0); //option 0: 자식프로세스가 종료할때까지 wait
    printf("[%d] 자식 프로세스 [1] %d 종료 \n", getpid( ), child);
    printf("\t종료 코드 %d\n", status>>8);
}
```



```shell
$ waitpid
[16840] 부모 프로세스 시작
[16841] 자식 프로세스[1] 시작
[16842] 자식 프로세스[2] 시작
[16841] 자식 프로세스[1] 종료
[16840] 자식 프로세스[1] 16841 종료
종료코드 1
[16842] 자식 프로세스[2] 종료
```

sleep이 영향을 미친다.

<br>

## fork() 후에 파일 공유

- 자식은 부모의 fd 테이블을 복사한다. 
  - 부모와 자식이 **같은 파일 디스크립터를 공유** (FD table, U area 복사) 
  - 같은 파일 오프셋을 공유 
  - 부모와 자식으로부터 출력이 서로 섞이게 됨 
- 자식에게 상속되지 않는 성질 
  - fork()의 반환값 
  - 프로세스 ID
  -  부모 프로세스가 설정한 프로세스 잠금, 파일 잠금 
  - 설정된 알람과 시그널



<br>

## fork()

![image](https://user-images.githubusercontent.com/79521972/168189002-751ff4cb-edf9-4626-861f-da0f4cf94b17.png)

자식 프로세스가 생성되면 동일한 내용의 file descriptor를 그대로 복사하고 open file table 뒷단은 모두 공유하게 된다.(같은 곳을 가리킴)

그래서 부모와 자식으로부터 출력이 서로 섞이게 됨 

<br>

## Copy-on-write

fork() 의 효율성을 높이기 위한 방법

- Copy-on-write is a **lazy optimization strategy** designed to mitigate the overhead of duplicating resources. 
  - duplication을 가능한한 지연

- The premise is simple: if multiple consumers request read access to their own copies of a resource, duplicate copies of the resource need not be made. Instead, each consumer can be handed a pointer to the same resource. 
- So long as no consumer attempts to modify its "copy" of the resource, the illusion of exclusive access to the resource remains, and the overhead of a copy is avoided. 
- If a consumer does attempt to modify its copy of the resource, at that point, the resource is transparently duplicated, and the copy is given to the modifying consumer
  - modify가 일어나지 않은 경우에는 한 개의 copy만 공유하도록 하고 만약 두 프로세스 중에서 한 프로세스가 modify 하게 되면 그제서야 비로소 lazy하게 뒤늦게 copy를 만들어준다.

<br>

## fork()

**Copy on write**

![image](https://user-images.githubusercontent.com/79521972/168189249-70076eb1-83c5-40e6-83d2-c388cdc87bfd.png)

- After parent process write a page C.

![image](https://user-images.githubusercontent.com/79521972/168189298-84a8013c-2960-49dd-879d-d50f0b87ef3e.png)

c만 modify를 하고자 하면 별도의 copy본을 만들어 각자 다른 것을 가리키게 하는 것이다.

<br>

## Vfork()

- Creates a new process only to exec a new program 

  - No copy of parent's address space for child (not needed!) 
  - Before exec, child runs in "address space of parent" 
  - Efficient in paged virtual memory 

- **Child runs first** 
- Parent waits until child exec or exit (block)
  
- Then the parent resume 
  
- The deadlock(block된 상태) possibility if the child wait for something from the parent

<br>

## vfork()

```c
#include <unistd.h>

pid_t vfork(void);
 	Returns: 0 in child, process ID of child in parent, -1 on error 
```

- same calling sequence and same return values as fork(). 
- intended to create a new process when the purpose of the new process is to exec() a new program. 
  - Does not copy the address space of parent into the child. 
  - The child calls exec() or exit() right after the vfork(). 
  - The child runs in the address space of the parent. (exec() 전까지는)
  - Provides an efficiency. 
- vfork() guarantees that the child runs first.

<br>

- Example

```c
#include "apue.h“
int glob = 6; /* external variable in initialized data */
int main(void)
{
    int var; /* automatic variable on the stack */
    pid_t pid;
    var = 88;
    
    printf("before vfork\n"); /* we don't flush stdio */
    if ((pid = vfork()) < 0) {
        err_sys("vfork error");
    } else if (pid == 0) { /* child */
        glob++; /* modify parent's variables */
        var++;
        exit(0); /* child terminates */
    }
    /*
 * Parent continues here.
 */
    printf("pid = %d, glob = %d, var = %d\n", getpid(), glob, var);
    exit(0);
}
```

- 실행 (13 페이지 fork 경우와의 차이점)

```shell
$ ./a.out
before vfork
pid = 29039, glob = 7, var = 89
$
```

vfork는 자식 프로세스를 즉시 복제하지 않고 exec()을하기 전까지는 자식 프로세스가 부모 프로세스의 주소 공간에서 실행된다. 따라서 자식 프로세스가 변수(var, glob)을 증가  시켰는데 자식과 부모 프로세스가 glob, var 변수에 대해 별도로 copy를 가지고 있는 것이 아니라 부모 프로세스가 갖고 있는 것을 공유하는 것이기 때문에 부모가 증가시키지 않았더라도 자식이 증가시킨 것 때문에 부모가 출력하는 내용에 이것이 반영되어 나타나게 된 것이다.

고로 이 점이 fork() 와의 차이점이라고 할 수 있다.
