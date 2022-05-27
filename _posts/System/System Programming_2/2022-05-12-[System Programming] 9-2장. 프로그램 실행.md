---
layout: single
title: "[System Programming] 9-2장. 프로그램 실행"
categories: ['System', 'System Programming']
tag: ['program', 'process']
---

<br>

## 프로그램 실행

- fork() 후 
  - 프로세스 이미지가 변경하는 것은 아니기 때문에 자식 프로세스는 부모 프로세스와 똑같은 코드 실행 
- 자식 프로세스에게 새 프로그램 실행 
  - exec() 시스템 호출 사용 
  - 프로세스 내의 프로그램을 새 프로그램으로 대치 (원래는 부모 프로그램이 깔려있었다면)
  - **PID는 변경되지 않음** 
    - <mark>단지 프로세스 내에 탑재되는 프로그램만 교체가 되는 것이다.</mark>
- 보통 fork() 후에 exec( ) 호출 
  - When a process calls one of the exec functions, the process is **completely replaced** by the new program. 
  - New program starts executing at its main() function. 
  - exec() merely replaces the current process(text, data) with a brand new program from disk.

<br>

## fork/exec

- 보통 fork() 호출 후에 exec() 호출 
  - 새로 실행할 프로그램에 대한 정보를 arguments로 전달한다 

- exec() 호출이 성공하면 
  - 자식 프로세스는 새로운 프로그램을 실행하게 되고 
  - 부모는 계속해서 다음 코드를 실행하게 된다. 


```c
if ((pid = fork()) == 0 ){
 exec( arguments );
 exit(1);
}
// 부모 계속 실행
```



<br>

## 프로그램 실행: exec()

- 프로세스가 exec() 호출을 하면, 
  - 그 프로세스 내의 프로그램은 완전히 새로운 프로그램으로 대치 
  - 자기대치(自己代置) 
  - 새 프로그램의 main()부터 실행이 시작된다.

![image](https://user-images.githubusercontent.com/79521972/168190101-dd456e03-e5bc-4710-b646-05f20827d768.png)

<br>

- exec() 호출이 성공하면 리턴할 곳이 없어진다. 
  - 부모 프로세스와의 connection이 끊어졌기 때문에

- 성공한 exec() 호출은 절대 리턴하지 않는다. 
  - 실패한 경우에만 리턴함.


```c
#include <unistd.h>
int execl(char* path, char* arg0, char* arg1, ... , char* argn, NULL)
int execv(char* path, char* argv[])
int execlp(char* file, char* arg0, char* arg1, ... , char* argn, NULL)
int execvp(char* file, char* argv[])
```

path: absolute path

file: current working directory 기준으로 한 파일

호출한 프로세스의 코드, 데이터, 힙, 스택 등을 path가 나타내는 새로운 프로그램으로 대치한 후 새 프로그램을 실행한다. 성공한 exec( ) 호출은 리턴하지 않으며 실패하면 -1을 리턴한다.

<br>

## exec()

- pathname(filename) argument 
  - pathname(filename) of a file to be executed. 
  - execl, execv, execle, execve take a pathname argument. 
  - execl**p**, execv**p** take a filename argument. 
    - If filename contains a '/', it is taken as a pathname. 
    - Otherwise, the executable file is searched for in the directories specified by PATH. E.g. PATH=/bin:/usr/bin:/usr/local/bin/:.

- Argument list 
  - arg0, arg1, ..., argn in the execl, execlp, and execle. 
  - The list of arguments is terminated by a NULL pointer. (인자의 끝을 알리기 위한 용도)
    - execl("/bin/ls", "ls", -"al", 0); 
- Argument vector 
  - list와 사실상 동일하나 넘겨주는 방식의 차이가 존재
  - *argv[] in execv, execve and execvp. 
  - The array of pointers is terminated by a NULL pointer. 
    - char *argv[3] = {"ls", -"al", 0}; 
    - execv("/bin/ls", argv);
  
- Environment variable 
  - *envp[] in execve and execle 
  - A pointer to an array of pointers to the **environment strings**. 
  - The other functions use the environ variable. 
    - char *env[2] = {"TERM=vt100", 0}; 
    - execle(""/bin/ls", "ls", 0, env);



<br>

- Differences among the six exec functions(family)

![image](https://user-images.githubusercontent.com/79521972/168190446-3283c968-5890-4b59-bdec-ea1a53cda589.png)

경로 이름으로 전달 vs. 파일 이름으로 전달

<br>

- execve is a system call. 
  - execl, execv, execle, execlp, execvp are library functions. 
  - Relationship of the six exec functions.

![image](https://user-images.githubusercontent.com/79521972/168190481-d126cd2c-afcb-48bf-bc85-fe3df22969ea.png)

```c
#include <unistd.h>

int execl(const char *pathname, const char *arg0, ... /* (char *)0 */ );
int execv(const char *pathname, char *const argv []);
int execle(const char *pathname, const char *arg0, … /* (char *)0,
 char *const envp[] */ );
int execve(const char *pathname, char *const argv[], char *const envp []);
int execlp(const char *filename, const char *arg0, ... /* (char *)0 */ );
int execvp(const char *filename, char *const argv []);
 			All six return: -1 on error, no return on success 
```



<br>

## execute1.c

```c
#include <stdio.h>
/* 자식 프로세스를 생성하여 echo 명령어를 실행한다. */
int main( )
{
    printf("부모 프로세스 시작\n");
    if (fork( ) == 0) {
        execl("/bin/echo", "echo", "hello", NULL); // 자식 주소 공간 변경
        fprintf(stderr,"첫 번째 실패"); // exec 성공 시 실행 안됨
        exit(1); // exec 성공 시 실행 안됨
    }
    printf("부모 프로세스 끝\n");
} 
```

```shell
$ execute1
부모 프로세스 시작
hello
부모 프로세스 끝
```

exec()이 성공했단 의미는 자식 프로세스의 공간이 echo 프로그램으로 대치되었기 때문에 그 아래 문장은 모두 없어진다는 뜻이다.

<br>

## execute2.c

```c
#include <stdio.h> …
/* 세 개의 자식 프로세스를 생성하여 각각
다른 명령어를 실행한다. */
int main( )
{
    printf("부모 프로세스 시작\n");
    if (fork( ) == 0) {
        execl("/bin/echo", "echo", "hello", NULL);
        fprintf(stderr,"첫 번째 실패");
        exit(1);
    }
    if (fork( ) == 0) {
        execl("/bin/date", "date", NULL); // argument 없음
        fprintf(stderr,"두 번째 실패");
        exit(2);
    }
    if (fork( ) == 0) {
        execl("/bin/ls","ls", "-l", NULL); // argument 1개
        fprintf(stderr,"세 번째 실패");
        exit(3);
    }
    printf("부모 프로세스 끝\n");
}
```

```shell
$ execute2
부모 프로세스 시작
부모 프로세스 끝
hello
2014년 4월 22일 화요일 오후 08시 43분 47초
총 50
-rwxr-xr-x 1 chang faculty 24296 4월 16일 13:37 execute2
-rw-r--r-- 1 chang faculty 556 4월 14일 13:17 execute2.c
```

<br>

## execute3.c

```c
#include <stdio.h> …
/* 명령줄 인수로 받은 명령을 실행시킨다. */

int main(int argc, char *argv[])
{
    int child, pid, status;
    pid = fork( );
    if (pid == 0) { // 자식 프로세스
        execvp(argv[1], &argv[1]); // 부모 프로세스의 argument을 다시 전달
        fprintf(stderr, "%s:실행 불가\n",argv[1]); 
    } else { // 부모 프로세스 
        child = wait(&status);
        printf("[%d] 자식 프로세스 %d 종료 \n", getpid(), pid);
        printf("\t종료 코드 %d \n", status>>8);
    }
}
```

```shell
$ execute3 wc you.txt
25 68 556 you.txt
[26470] 자식 프로세스 26471 종료
종료 코드 0
```

![image](https://user-images.githubusercontent.com/79521972/168190749-2a32586c-1aef-43e5-b8f4-3aa1d89494b9.png)



<br>

## exec()

- Example

```c
#include "apue.h"
#include <sys/wait.h>
char *env_init[] = { "USER=unknown", "PATH=/tmp", NULL };
int main(void)
{
    pid_t pid;
    if ((pid = fork()) < 0) {
        err_sys("fork error");
    } else if (pid == 0) { /* specify pathname, specify environment */
        if (execle("/home/sar/bin/echoall", "echoall", "myarg1",
                   "MY ARG2", (char *)0, env_init) < 0)
            err_sys("execle error");
    }
    if (waitpid(pid, NULL, 0) < 0)
        err_sys("wait error");
    if ((pid = fork()) < 0) {
        err_sys("fork error");
    } else if (pid == 0) { /* specify filename, inherit environment */
        if (execlp("echoall", "echoall", "only 1 arg", (char *)0) < 0)
            err_sys("execlp error");
    }
    exit(0);
}
```

<br>

- echoall

```c
#include "apue.h"
int main(int argc, char *argv[])
{
    int i;
    char **ptr;
    extern char **environ;
    for (i = 0; i < argc; i++) /* echo all command-line args */
        printf("argv[%d]: %s\n", i, argv[i]);
    
    for (ptr = environ; *ptr != 0; ptr++) /* and all env strings */
        printf("%s\n", *ptr);
    exit(0);
}
```

```shell
$ ./a.out
argv[0]: echoall
argv[1]: myarg1
argv[2]: MY ARG2
USER=unknown
PATH=/tmp
$ argv[0]: echoall
argv[1]: only 1 arg
USER=sar
LOGNAME=sar
SHELL=/bin/bash
…
HOME=/home/sar
```

<br>

## system()

- fork()와 wait()을 합친 시스템 콜
- Both ANSI C and POSIX define an interface that couples spawning a new process and waiting for its termination —think of it as **synchronous process creation**. 
- If a process is spawning a child only to immediately wait for its termination, it makes sense to use this interface 
- It is common to use system( ) to run a simple utility or shell script, often with the explicit goal of simply obtaining its return value

<br>

```c
#include <stdlib.h>
int system(const char *cmdstring);
//이 함수는 /bin/sh –c cmdstring를 호출하여 cmdstring에 지정된 명령어를
//실행하며, 명령어가 끝난 후 명령어의 종료코드를 반환한다.
```

- 자식 프로세스를 생성하고 /bin/sh 로 하여금 지정된 명령어(cmdstring)를 실행시킨다(exec) 
  - system("date > file"); 
- system() 함수 구현 
  - fork(), exec(), waitpid() 시스템 호출을 이용 
- 반환값 
  - 명령어의 종료코드 
  - -1 with errno: fork() 혹은 waitpid() 실패 
  - 127 : exec() 실패

<br>

## system() 함수 구현

```c
#include <sys/types.h> /* system.c */
#include <sys/wait.h>
#include <errno.h>
#include <unistd.h>
int system(const char *cmdstring)
{
    pid_t pid; int status;
    if (cmdstring == NULL) /* 명령어가 NULL인 경우 */
        return(1);
    if ( (pid = fork()) < 0) {
        status = -1; /* 프로세스 생성 실패 */
    } else if (pid == 0) { /* 자식 */
        execl("/bin/sh", "sh", "-c", cmdstring, (char *)0);
        _exit(127);  /* execl 실패 */
    } else { /* 부모 */
        while (waitpid(pid, &status, 0) < 0)
            if (errno != EINTR) { 	/* 오류 원인 시스템변수 errno로 전달 */
 				status = -1; 		/* waitpid()로부터 EINTR외의 오류 */
                break; 				/* EINTR은 시스템 호출중 인터럽트에 의해 수행이 중단된 오류*/
            }               
    }
    return(status);
}
```



<br>

## Implementing system()

```c
/* * my_system - synchronously spawns and waits for the command
* "/bin/sh -c <cmd>".
* Returns -1 on error of any sort, or the exit code from the
* launched process. Does not block or ignore any signals. */
int my_system (const char *cmd){
    int status;
    pid_t pid;
    
    pid = fork ( );
    
    if (pid == -1)
        return -1;
    else if (pid == 0) {
        const char *argv[4];
        argv[0] = "sh"; argv[1] = "-c"; argv[2] = cmd; argv[3] = NULL;
        execv ("/bin/sh", argv);
        exit (-1); // exec에 성공하면 이 줄은 실행 안 함
    }
    
    if (waitpid (pid, &status, 0) == -1)
        return -1;
    else if (WIFEXITED (status)) //정상 종료한 경우
        return WEXITSTATUS (status);
    
    return -1;
}
```

앞에서는 list를 통해 전달을 했다면 이 예제는 포인터를 통해 전달했다는 차이가 있다.

<br>

## syscall.c

```c
#include <sys/wait.h>
#include <stdio.h>
int main()
{
    int status;
    if ((status = system("date")) < 0)
        perror("system() 오류");
    printf("종료코드 %d\n", WEXITSTATUS(status));
    
    if ((status = system("hello")) < 0)
        perror("system() 오류");
    printf("종료코드 %d\n", WEXITSTATUS(status));
    
    if ((status = system("who; exit 44")) < 0)
        perror("system() 오류");
    printf("종료코드 %d\n", WEXITSTATUS(status));
}
```

```shell
$ syscall
2014. 03. 07 (금) 11:20:38 KST
종료코드 0
sh: hello: command not found
종료코드 127
chang tty 2014-03-18 10:58 (:0)
….
종료코드 44
```



<br>

# 9.3 입출력 재지정

## 입출력 재지정 (2장 리눅스 사용 참조)

- 명령어의 표준 출력이 파일에 저장 
  - $ 명령어 > 파일 
- 출력 재지정 기능 구현 
  - 파일 디스크립터 fd를 표준출력(1)에 dup2() 
  - fd = open(argv[1], O_CREAT|O_TRUNC|O_WRONLY, 0600); 
    - // argv[1]이 나타내는 파일을 쓰기전용으로 열고, 기존 내용은 모두 지움. 
    - // 파일이 없으면 새로 생성하고 사용권한은 0600으로 함. 
  - dup2(fd, 1);

```c
#include <unistd.h> //4장 파일 입출력 참조
int dup(int oldfd);
//oldfd에 대한 복제본인 새로운 파일 디스크립터를 생성하여 반환한다. 실패시 -1 반환
int dup2(int oldfd, int newfd);
//oldfd을 newfd에 복제하고 복제된 새로운 파일 디스크립터를 반환한다. 실패시 -1 반환
```



<br>

## redirect1.c

```c
#include <stdio.h>
#include <fcntl.h>
…
/* 표준 출력을 파일에 재지정하는 프로그램 */
int main(int argc, char* argv[])
{
    int fd, status;
    fd = open(argv[1], O_CREAT|O_TRUNC|O_WRONLY, 0600);
    dup2(fd, 1); /* 파일을 표준출력에 복제 */
    close(fd);
    printf("Hello stdout !\n"); 
    fprintf(stderr,"Hello stderr !\n"); //표준오류를 통해 출력
}
```

```shell
$ redirect1 out
Hello stderr !
$ cat out
Hello stdout ! 
```

<br>

## redirect2.c

```c
#include <stdio.h>
#include <fcntl.h>

/* 자식 프로세스의 표준 출력을 파일
에 재지정한다. */
int main(int argc, char* argv[])
{
    int child, pid, fd, status;

    pid = fork( );
    if (pid == 0) {
        fd = open(argv[1],O_CREAT | O_TRUNC| O_WRONLY, 0600);
        dup2(fd, 1); // 파일을 표준출력에 복제
        close(fd);
        execvp(argv[2], &argv[2]);
        fprintf(stderr, "%s:실행 불가\n",argv[1]);
    } else {
        child = wait(&status);
        printf("[%d] 자식 프로세스 %d 종료 \n", getpid(), child);
    }
}
```

```shell
$ redirect2 out wc you.txt
[26882] 자식 프로세스 26883 종료
$ cat out
 25 68 556 you.txt
 줄 단어 문자수
```

<br>

# 9.4 프로세스 그룹

## Process groups

- A collection of one or more processes. 
  - 프로세스 그룹은 여러 프로세스들의 집합이다 
- Usually associated with the **same job**. 
  - when a shell starts up a **pipeline** (e.g., when a user enters ls | more), 
  - all the commands in the pipeline go into the same process group

- ![image](https://user-images.githubusercontent.com/79521972/168475528-c116719c-4456-4bf3-8ee5-9969040a5c58.png)
  

<br>

- Each process is owned by a **user** and a group 
- Each process is also part of a process group, which simply expresses its relationship to other processes, and must not be confused with the aforementioned user/group concept
- "userid라는 유저 아이디를 가진 유저가 만든 프로세스 그룹"



<br>

## 프로세스 그룹

- 보통 부모 프로세스(그룹 리더)가 생성하는 자손 프로세스들은 부모와 같은 프로세스 그룹에 속한다. 
- 프로세스 그룹은 signal 전달 등을 위해 사용됨. 
  - The notion of a process group makes it easy 
    - to send signals to processes in the pipeline (e.g., $ ls | more)
      - multicast로 효율적으로 보내기 위함
    - to receive signals from the same terminal
    - get information on an entire pipeline 
- From the perspective of a user, a process group is closely related to a job

![image](https://user-images.githubusercontent.com/79521972/168193418-f6b5560e-e661-425a-a888-b266b488e8a1.png)



<br>

## Process groups

- Process group ID 
  - Each process group has a unique PGID. 
  - Each process group can have the process group leader, whose PID equals its PGID. 
    - 프로세스 그룹 리더: Process GID = PID 
- The process group **exists**, as long as there is at least one process in the group, **regardless whether the group leader terminates or not.**

```shell
$ ps -o pid,ppid,pgid,comm | cat
 PID 	PPID 	PGID 	COMMAND
27463 	27462 	27463 	bash
27554 	27463 	27554 	ps
27555 	27463 	27554 	cat
$
```

ps와 cat은 하나의 job을 형성하니까 동일 process group id를 갖고 shell(bash)는 별도의 process

<br>

## 프로세스 그룹

- 프로세스 IDs 
  - 프로세스 ID(PID) 
  - 프로세스 그룹 ID(GID) 
  - 각 프로세스는 하나의 프로세스 그룹에 속함. 
  - 각 프로세스는 자신이 속한 프로세스 그룹 ID를 가지며 fork 시 물려받는다. 

```c
#include <sys/types.h>
#include <unistd.h>
pid_t getpgrp(void);
// 호출한 프로세스의 프로세스 그룹 ID를 반환한다.
```



```c
#include <unistd.h>

pid_t getpgid(pid_t pid);
 	Returns: process group ID if OK, -1 on error 
```

- Return the process group ID of the process with pid. 
  - If pid is 0, return the process group ID of the calling process. 
    - getpgid(0); is equivalent to getpgrp();

<br>

## 프로세스 그룹: pgrp1.c

```c
#include <sys/types.h>
#include <unistd.h>
main()
{
    int pid, gid;
    printf("PARENT: PID = %d GID = %d\n", getpid(), getpgrp());
    pid = fork();
    if (pid == 0) { // 자식 프로세스
        printf("CHILD: PID = %d GID = %d\n", getpid(), getpgrp());
    }
}
```

```shell
$ pgrp1
PARENT: PID = 17768 GID = 17768
CHILD: PID = 17769 GID = 17768
```

parent process와 child process는 동일 process group에 속한다.

<br>

## 프로세스 그룹

- 프로세스 그룹 만들기 
  - A process can create a new process group and become leader 
  - int setpgid(pid_t pid, pid_t pgid); 
- 프로세스 그룹 소멸 
  - the last process terminates OR 
  - joins another process group 

```c
#include <sys/types.h>
#include <unistd.h>
int setpgid(pid_t pid, pid_t pgid);
//프로세스 pid의 프로세스 그룹 ID를 pgid로 설정한다.
//성공하면 0을 실패하면 -1를 반환한다.
```

- 새로운 프로세스 그룹을 생성하거나 다른 그룹에 멤버로 참여 
  - pid == pgid  ->  pid가 새로운 그룹을 만듦과 동시에 새로운 그룹 리더가 됨. 
  - pid != pgid  ->  다른 그룹의 멤버가 됨. 
  - pid == 0 ->  호출자의 PID 사용 
  - pgid == 0  ->  새로운 그룹 리더가 됨 

- 호출자가 새로운 프로세스 그룹을 생성하고 그룹의 리더 
  - setpgid(getpid(), getpid()); 
  - setpgid(0,0);



<br>

## 프로세스 그룹: pgrp2.c

```c
#include <sys/types.h>
#include <unistd.h>
main()
{
    int pid, gid;
    printf("PARENT: PID = %d GID = %d \n", getpid(), getpgrp());
    pid = fork();
    if (pid == 0) {
        setpgid(0, 0);
        printf("CHILD: PID = %d GID = %d \n", getpid(), getpgrp());
    }
}
```

```shell
$ pgrp2
PARENT: PID = 17768 GID = 17768
CHILD: PID = 17769 GID = 17769
```

<br>

## 프로세스 그룹 사용

- 프로세스 그룹 내의 모든 프로세스에 시그널을 보낼 때 사용 
  - $ kill –9 pid          // 9번 시그널 SIGKILL 
  - $ kill –9 0             // 현재 속한 프로세스 그룹 내의 모든 프로세스에게 시그널을 보내 종료시킴 
  - $ kill –9 –pid        // –pid 는 프로세스그룹 pid에 속한 모든 프로세스에게 시그널을 보내 종료시킴 
- pid_t waitpid(pid_t pid, int *status, int options); 
  - pid == -1 : 임의의 자식 프로세스가 종료하기를 기다린다. 
  - pid > 0 : 자식 프로세스 pid가 종료하기를 기다린다. 
  - pid == 0 : 호출자와 같은 프로세스 그룹 내의 어떤(아무) 자식 프로세스가 종료하기를 기다린다. 
  - pid < -1 : pid의 절대값과 같은 프로세스 그룹 내의 어떤 자식 프로세스가 종료하기를 기다린다.




