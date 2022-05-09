---
layout: single
title: "[System Programming] 8장. 프로세스"
categories: ['System', 'System Programming']
tag: ['process']
---

<br>

# 8.1 쉘과 프로세스

## 쉘(Shell)이란 무엇인가?

- 쉘의 역할 
  - 쉘은 사용자와 운영체제 사이에 창구 역할을 하는 소프트웨어 
  - 명령어 처리기(command processor) 
  - 사용자로부터 명령어를 입력받아 이를 처리한다 
  - 명령어 (프로그램)이 프로세스가 됨.

![image](https://user-images.githubusercontent.com/79521972/167346442-00facc52-347b-42d8-9208-7fa7d685af12.png)

<br>

## 쉘(Shell)의 종류

- Bourne shell 
  - 유닉스용 최초의 쉘 
- C shell 
- Korn shell 
- bash shell 
  - 리눅스의 기본 쉘 
  - Bourne shell을 기반으로 개발

<br>

## 쉘의 진행 절차

![image](https://user-images.githubusercontent.com/79521972/167346426-23e33a98-4629-487b-be3d-c008fc649516.png)



<br>

## 시작 파일

- 홖경변수와 같은 사용자의 사용홖경을 초기화하는데 사용 
- 시작 파일의 위치 (bash)
  - 시스템 : /etc/profile /etc/bashrc 
  - 사용자 : ~/. bash_profile ~/. bashrc

<br>

## 쉘의 구조

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

## Shell 복합 명령어

- 명령어 열(command sequence)
  - $ 명령어1; … ; 명령어n 
  - $ date; who; pwd 
- 명령어 그룹(command group) 
- 그룹내 모든 명령어들이 stdin, stdout, stderr 공유 
  - $ (명령어1; … ; 명령어n) 
  - $ date; who; pwd > out1.txt // pwd의 결과맊 out1.txt로 출력
  - $ (date; who; pwd) > out2.txt // date, who, pwd 의 결과가 모두 out2.txt로 출력



<br>

## 전면 처리 vs 후면 처리

- 전면 처리 
  - 명령어를 입력하면 명령어가 젂면에서 실행되며 명령어 실행이 끝날 때 까지 쉘이 기다려 준다. 
  - stdin, stdout을 이용한 입출력 가능 
  - 한 명령어씩맊 수행 가능 
  - Ctrl-C 입력으로 강제 종료 가능, 
  - Ctrl-Z 입력으로 실행 중단 가능, 중단된 명령어는 fg 명령어에 의해 다시 젂면 처리 계속됨
- 후면 처리  
  - 명령어들을 후면에서 처리하고 젂면에서는 다른 작업을 할 수 있으면 동 시에 여러 작업을 수행할 수 있다. 
  - $ 명령어 & 



<br>

## 후면 처리 예

- $ (sleep 100; echo done) & // 2개의 후면 작업 생성 
  [1] 8320 
- $ find . -name test.c -print & 
  [2] 8325 
- $ jobs // 후면 작업 리스트 
  [1] + Running ( sleep 100; echo done )
  [2] - Running find . -name test.c –print 
- $ fg %작업번호 // 후면 작업을 전면 작업으로 전환 
  $ fg %1 
  ( sleep 100; echo done ) 
- 후면처리 입출력 : 프릮트 결과 뒤섞임 방지 목적 
  $ find . -name test.c -print > find.txt & // 출력 재지정, 프릮트 결과 뒤섞임 방지 목적 
  $ find . -name test.c -print | mail chang & 
  $ wc < inputfile & // 후면 프로세스의 입력 재지정을 통한 입력 처리

<br>

## 프로세스(process)

- 실행중인 프로그램을 프로세스(process)라고 부른다. 
- 각 프로세스는 유일한 프로세스 번호 PID를 갖는다. 
- ps 명령어를 사용하여 나의 프로세스들을 볼 수 있다.

```shell
$ ps // 자싞의 프로세스들의 실행 상태 출력
PID TTY TIME CMD
8695 pts/3 00:00:00 csh
8720 pts/3 00:00:00 ps
$ ps u //자세한 정보 출력,
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
chang 8695 0.0 0.0 5252 1728 pts/3 Ss 11:12 0:00 -csh
chang 8793 0.0 0.0 4252 940 pts/3 R+ 11:15 0:00 ps u
```

<br>

## 프로세스 상태: ps

- ps [-옵션] 명령어 

- 현재 존재하는 프로세스들의 실행 상태를 요약해서 출력 
  ```shell
  PID TTY TIME CMD
  25435 pts/3 00:00:00 csh
  25461 pts/3 00:00:00 ps
  ```

-  $ ps -aux (BSD 유닉스)

  - \- a: 모든 사용자의 프로세스를 출력 
  - \- u: 프로세스에 대한 좀 더 자세한 정보를 출력 
  - \- x: 더 이상 제어 터미널을 갖지 않은 프로세스들도 함께 출력

-  $ ps -ef (시스템 V)
  - \- e: 모든 사용자 프로세스 정보를 출력 
  - \- f: 프로세스에 대한 좀 더 자세한 정보를 출력



<br>

## ps aux

```shell
$ ps -aux //시스템 내의 모든 프로세스 정보 출력
Or
$ ps -ef
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
root 1 0.0 0.0 2064 652 ? Ss 2011 0:27 init [5]
root 2 0.0 0.0 0 0 ? S< 2011 0:01 [migration/0]
root 3 0.0 0.0 0 0 ? SN 2011 0:00 [ksoftirqd/0]
root 4 0.0 0.0 0 0 ? S< 2011 0:00 [watchdog/0]
...
root 8692 0.0 0.1 9980 2772 ? Ss 11:12 0:00 sshd: chang [pr
chang 8694 0.0 0.0 9980 1564 ? R 11:12 0:00 sshd: chang@pts
chang 8695 0.0 0.0 5252 1728 pts/3 Ss 11:12 0:00 -csh
chang 8976 0.0 0.0 4252 940 pts/3 R+ 11:24 0:00 ps aux

```





<br>

## 프로세스(process)

- processes are the most fundamental abstraction in a Unix system, after files. 
- As object code in execution 
  - active, alive, running programs 
  - processes are more than just assembly language; 
    - they consist of data, resources, state, and a virtualized computer 
  - program image + environment 
    - text(code) /data / stack / heap segment 
    - kernel data structure, address space, open files, ...



<br>

### Process

```c
/* test_program.c */
#include <stdio.h>
int a,b;
int glob_var = 3;
void test_func(void) {
     int local_var, *buf;

     buf = (int *) malloc(10,000 * sizeof(int));
     …
    }
    int main(int argc, char *argv[]) {
     int i = 1;
     int local_var;

     a = i + 1;
     printf(“value of a = %d\n”, a);
     test_func();
}
```

```shell
$ gcc –o test_program test_program.c
```



<br>

```shell
$ ./test_program
```

![image](https://user-images.githubusercontent.com/79521972/167347246-2c9123b1-7edd-43d1-9a36-7bfcfbcd5003.png)



- \* bss: Block Started by Symbol PCB: Process Control Block

<br>

## sleep

- sleep 명령어 
  - 지정된 시간맊큼 실행을 중지한다

`	$ sleep 초 $ (echo 시작; sleep 5; echo 끝)`



## kill

- kill 명령어 

- 수행중인 프로세스에 시그널 보냄 
- 시그널 미명시시 현재 실행중인 프로세스를 강제로 종료 
  $ kill [-시그널] 프로세스번호 
  $ kill [-시그널] %작업번호 
- $ (echo 시작; sleep 5; echo 끝) & 
  1230 
  $ kill 1230 
- $ (sleep 100; echo done) & 
  [1] 8320 
  $ kill 8320 
  $ kill %1 
  [1] Terminated ( sleep 100; echo done )



## wait

$ wait [프로세스번호] 

- 해당 프로세스 번호를 갖는 자식 프로세스가 종료될 때까지 기다린다. 쉘은 중지 

- 프로세스 번호를 지정하지 않으면 모든 자식 프로세스를 기다린다.

```shell
$ (sleep 10; echo 1번 끝) &
1231
$ echo 2번 끝; wait 1231; echo 3번 끝
2번 끝
1번 끝
3번 끝
```

```shell
$ (sleep 10; echo 1번 끝) &
$ (sleep 10; echo 2번 끝) &
$ echo 3번 끝; wait; echo 4번 끝
3번 끝
1번 끝
2번 끝
4번 끝
```

## exit

- exit
  - 쉘을 종료하고 종료코드(exit code)을 부모 프로세스에 젂달
    `$ exit [종료코드]`



<br>

# 8.2 프로그램 시작

## 프로그램 실행 시작

- 프로그램을 실행시키는 방법 
  - 쉘 프롬트에서 실행 할 프로그램을 지정하여 실행 
    - 쉘 명령어 이름 혹은 사용자가 작성한 프로그램의 실행 파일 이름 
  - 실행 중인 프로그램 안에서 exec 시스템을 호출하여 다른 프로그램 실행 
    - C 시작 루틴에 명령줄 인수(command-line arguments)와 홖경 변수를 (environment variables) 젂달하고 프로그램을 실행시킨다. 
- C 시작 루틴(start-up routine) 
  - C 프로그램을 compile 하면, C 프로그램 코드와 C 시작 루틴을 포함하는 실행파일 생성 
  - Started by the kernel (by the exec system call) 
  - main 함수를 호출하면서 명령줄 인수, 홖경 변수를 젂달

```c
exit( main(argc, argv));
```

실행이 끝나면 반환 값을 받아 exit 한다.



<br>

## 쉘의 구조

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

## 프로그램 실행 시작

![image](https://user-images.githubusercontent.com/79521972/167347783-f0c19d8f-502e-4071-8dec-7172ef1560c2.png)

<br>

## 명령줄 인수/환경 변수

```c
int main(int argc, char *argv[]);
//argc : 명령줄 인수의 개수
//argv[] : 명령줄 인수 리스트를 나타내는 포인터 배열
```

![image](https://user-images.githubusercontent.com/79521972/167347832-452a4ba7-6998-4e97-bddd-4357bc2da3c6.png)

<br>

## args.c

```c
#include <stdio.h>
/* 모든 명령줄 인수를 출력한다. */
int main(int argc, char *argv[])
{
    int i;
    for (i = 0; i < argc; i++) /* 모든 명령줄 인수 출력 */
        printf("argv[%d]: %s \n", i, argv[i]);
    exit(0);
}
```

```shell
$args hello world
argv[0]:args
argv[1]:hello
argv[2]:world
```



<br>

## 환경 변수

- 프로세스가 실행되는 기본 홖경을 설정하는 변수 
- 로그인명, 로그인 쉘, 터미널에 설정된 언어, 경로명 등 
- 홖경변수는 “홖경변수=값”의 형태로 구성되며 관례적으로 대문자 로 사용 
- 현재 쉘의 홖경 설정을 보려면 env 명령을 사용 
- 부모 프로세스에서 자식 프로세스로 젂달된다 
- .login 또는 .cshrc 파일에서 홖경 변수를 설정한다 
- 각 문자열은 '\0'로 끝난다 
- 홖경 변수 리스트의 마지막은 NULL 포인터 
- argv 와 같은 구조



<br>

## environ.c

```c
#include <stdio.h>
/* 젂역변수 environ 을 이용해 모든 홖경 변수를 출력한다. */
int main(int argc, char *argv[])
{
    char **ptr;
    extern char **environ;
    for (ptr = environ; *ptr != 0; ptr++) /* 모든 홖경 변수 값 출력*/
        printf("%s \n", *ptr);

    exit(0);
}
```

```shell
$ environ
HOME = /usr/dddd/ggg
PATH = …
………
```





<br>

## 환경 변수 접근

- getenv() 시스템 호출을 사용하여 홖경 변수를 하나씩 접근하 는 것도 가능하다.

```c
#include <stdlib.h>
char *getenv(const char *name);
//환경 변수 name 값의 포인터를 반환한다. 해당 변수가 없으면 NULL을 반환한다.
//실패하면 NULL 포인터를 리턴
```

<br>

## printenv.c

```c
#include <stdio.h>
#include <stdlib.h>
/* 홖경 변수를 3개 프릮트한다. */
int main(int argc, char *argv[])
{
     char *ptr;
    
     ptr = getenv("HOME");
     printf("HOME = %s \n", ptr);
    
     ptr = getenv("SHELL");
     printf("SHELL = %s \n", ptr); 
    
     ptr = getenv("PATH"); 
     printf("PATH = %s \n", ptr); 
    
     exit(0);
}
```

```shell
$ printenv
 HOME =
 ptr = /bin/bash
 PATH =
```

<br>

## 환경 변수 설정

- putenv(), setenv()를 사용하여 특정 홖경 변수를 설정한다.

```c 
#include <stdlib.h>
int putenv(const char *name);
//name=value 형태의 스트링을 받아서 이를 홖경 변수 리스트에 넣어준다.
//name이 이미 존재하면 원래 값을 새로운 값으로 대체한다.
int setenv(const char *name, const char *value, int rewrite);
//홖경 변수 name의 값을 value로 설정한다. name이 이미 존재하는 경우에는
//rewrite 값이 0이 아니면 원래 값을 새로운 값으로 대체하고 rewrite 값이 0이면 그대로 둔다.
int unsetenv(const char *name);
//홖경 변수 name의 값을 지운다.
```

<br>

# 8.3 프로그램 종료

## 프로그램 종료 (Process termination)

- 정상 종료(normal termination) 
  - main() 실행을 마치고 리턴하면 C 시작 루틴은 이 리턴값을 가지 고 exit()을 호출 
  - 프로그램 내에서 직접 exit()을 호출 
  - 프로그램 내에서 직접 _exit()을 호출 
- 비정상 종료(abnormal termination) 
  - abort() 시스템 콜 호출 
    - 프로세스에 SIGABRT 시그널을 보내어 프로세스를 비정상적으로 종료시킴 
  - 시그널 수싞에 의한 종료



<br>

## 프로그램 종료

- exit() 
  - Cleanup processing 을 수행하고 정상적으로 종료한다 
    - 모든 열려짂 스트림을 닫고(fclose), 
    - 출력 버퍼의 내용을 디스크에 쓰는(fflush), 메모리 회수 등의 뒷정리 후 프로세스를 정상적으로 종료 
  - 종료 코드(exit code)를 부모 프로세스에게 젂달한다.

```c
#include <stdlib.h>
void exit(int status);
//뒷정리를 한 후 프로세스를 정상적으로 종료시킨다. return to the kernel
```

- _exit()

```c
#include <stdlib.h>
void _exit(int status);
//뒷정리를 하지 않고 프로세스를 즉시 (정상)종료시킨다.
//return to the kernel immediately
```



<br>

## C 프로그램 시작 및 종료

![image](https://user-images.githubusercontent.com/79521972/167348475-2c718100-13af-4221-b31c-bce4cf0284d8.png)

<br>

## 쉘의 구조 (revisit)

```c
While (true) {
     display prompt
     read command (command, params) // Get command
     if (fork() != 0) then { // Shell process (!!!)
         wait(&status); // Execute command
         exit() // fork a child, wait for a child to finish
     }
    else
        execve(command, parmas) // 자식 프로세스
} 
```

```shell
$ ls
. .. /etc
/home /usr /dev
$
```

<br>









































