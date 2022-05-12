---
layout: single
title: "[System Programming] 8-2장. 프로세스 ID"
categories: ['System', 'System Programming']
tag: ['Process ID']
---

<br>

## 프로세스 ID

- 각 프로세스는 프로세스를 구별하는 번호인 프로세스 ID를 갖는다. 
  - PID : a unique identifier 
  - The pid is guaranteed to be **unique** at any single point in time 
  - As processes terminate, IDs become candidates for reuse. 
- 각 프로세스는 자신을 생성해준 부모 프로세스가 있다.
  - init process를 제외한 모든 process는 부모 프로세스가 있다.


```c
int getpid( ); //프로세스의 ID를 리턴한다.
int getppid( ); //부모 프로세스의 ID를 리턴한다. 
```



<br>

## pid.c

```c
#include <stdio.h>

/* 프로세스 번호를 출력한다. */
int main()
{
    int pid;
    printf("나의 프로세스 번호 : [%d] \n", getpid());
    printf("내 부모 프로세스 번호 : [%d] \n", getppid());
}
```

![image](https://user-images.githubusercontent.com/79521972/167991887-a27e4077-0c5b-4ee3-980d-16355a3e210d.png)

shell prompt에 pid 실행파일을 치면 나의 프로세스는 pid process가 되는 것이고 나의 부모 process는 shell process가 되는 것이다.

<br>

## Process identifiers

![image](https://user-images.githubusercontent.com/79521972/167991918-a18226f1-fe2a-43b2-b706-a46e0bb745d8.png)

같은 번호를 갖지 않도록 할당 됨

<br>

## Current working directory of process

- 각 프로세스는 Current working directory 를 가지고 있음 
- chdir() 시스템 콜을 이용해 Current working directory를 pathname 이 지정하는 디렉토리로 변경 가능 
  - 단, 프로세스는 pathname이 지정하는 디렉토리에 대한 **실행 권한**이 있어 야 함. (5장 파일 시스템 참조)

```c
#include <unistd.h>
int chdir(const char *pathname); // return: 0 if OK, -1 on error
```



<br>

## Process identifiers

- Process 0 (idle process : scheduler process or swapper) 
  - The process that the kernel “runs” **when there are no other runnable processes** 
  - Part of the kernel. 
  - System process. 
- Process 1 (**init process**) 
  - Normally, the init process on Linux is the init program 
  - **The first process** that the kernel executes **after booting the system** 
    - Invoked at the end of the bootstrap procedure. 
  - invoked by the kernel (/sbin/init or /etc/init ) 
  - **Initialize the UNIX system** with initialization files: /etc/rc* or /etc/inittab. 
  - Never dies. 
  - User process with superuser privilege. 
- Process 2 (pagedaemon) 
  - supports the paging of the virtual memory system 
  - kernel process



<br>

## Init Process

- Unless the user explicitly tells the kernel what process to run (through the init kernel command-line parameter), the kernel has to identify a suitable init process on its own 
- The Linux kernel tries four executables, in the following order: 

1. /sbin/init: The preferred and most likely location for the init process. 

2. /etc/init: Another likely location for the init process. 
3. /bin/init: A possible location for the init process. 
4. /bin/sh: The location of the Bourne shell, which the kernel tries to run if it fails to find an init process. 

- 위 우선순위를 기반하여 init process가 결정

- After the handoff from the kernel, the init process handles the remainder of the boot process. 
- Typically, this includes **initializing the system**, **starting various services**, and **launching a login program**.



<br>

## 프로세스의 사용자 ID(uid)

- 프로세스는 프로세스 ID 외에 
- 프로세스의 사용자 ID와 그룹 ID를 갖는다. 
  - 그 **프로세스를 실행시킨 사용자**의 ID와 사용자의 그룹 ID 
  - 프로세스가 수행할 수 있는 연산 권한 (예:파일 사용권한)을 결정 하는 데 사용된다. 
  - 새로운 프로세스 생성 시 부모의 사용자 ID와 그룹 ID를 물려 받음, 프로세스 ID는 별도 ID를 부여 받음

- 프로세스의 실제 사용자 ID (**real user ID**) 
  - 그 프로세스를 실행한 원래 사용자의 사용자 ID로 설정된다. 
  - 예를 들어 chang이라는 사용자 ID로 로그인하여 어떤 프로그램을 실행시키면 그 프로세스의 실제 사용자 ID는 chang이 된다. 
- 프로세스의 유효 사용자 ID (**effective user ID**) 
  - 현재 유효한 사용자 ID로 새로 파일을 만들 때나 파일에 대한 접근 권한을 검사할 때 주로 사용된다. 
  - 보통 유효 사용자 ID와 실제 사용자 ID는 **특별한 실행파일을 실행 할 때를 제외하고는** 동일하다. 



<br>

## 프로세스의 사용자 ID

- 프로세스의 실제/유효 사용자 ID 반환 
- 프로세스의 실제/유효 그룹 ID 반환
- 오류 리턴은 없음

```c
#include <sys/types.h>
#include <unistd.h>
uid_t getuid( ); //프로세스의 실제 사용자 ID를 반환한다.
uid_t geteuid( ); //프로세스의 유효 사용자 ID를 반환한다.
gid_t getgid( ); //프로세스의 실제 그룹 ID를 반환한다.
gid_t getegid( ); //프로세스의 유효 그룹 ID를 반환한다.
```



<br>

## PID, UID

```c
#include <unistd.h>

pid_t getpid(void);
 	Returns: process ID of calling process
pid_t getppid(void);
 	Returns: parent process ID of calling process
uid_t getuid(void);
 	Returns: real user ID of calling process
uid_t geteuid(void);
 	Returns: effective user ID of calling process
gid_t getgid(void);
 	Returns: real group ID of calling process
gid_t getegid(void);
 	Returns: effective group ID of calling process 
```

`pid_t defined in <sys/types.h> `

- pid_t: Used for process IDs and process group IDs.


<br>

## uid.c

```c
#include <stdio.h> $ uid
#include <pwd.h> 
#include <grp.h> 

/* 사용자 ID를 출력한다. */
int main()
{
     int pid;
     printf("나의 실제 사용자 ID : %d(%s) \n", getuid(), getpwuid(getuid())->pw_name);
     printf("나의 유효 사용자 ID : %d(%s) \n", geteuid(), getpwuid(geteuid())->pw_name);
     printf("나의 실제 그룹 ID : %d(%s) \n", getgid(), getgrgid(getgid())->gr_name);
     printf("나의 유효 그룹 ID : %d(%s) \n", getegid(), getgrgid(getegid())->gr_name);
}
```

```c
getpwuid() : uid에 해당하는 패스워드파일 엔트리를 읽어 사용자이름으로 변환
getgrgid() : gid에 해당하는 그룹파일 엔트리를 읽어 그룹이름으로 변환
```



<br>

## Getpwuid (5장)

- 사용자 id와 일치하는 **엔트리를** /etc/pswd 파일에서 찾아 그 엔트리에 대한 포인터를 반환
- 이 엔트리의 pw_name 필드가 사용자 이름을 가지고 있음

```c
#include <pwd.h>
struct passwd *getpwuid(uid_t uid);
```

```c
struct passwd{ /* pwd.h */
    char *pw_name; /* Username. */
    char *pw_passwd; /* Password. */
    uid_t pw_uid; /* User ID. */
    gid_t pw_gid; /* Group ID. */
    char *pw_gecos; /* Real name. */
    char *pw_dir; /* Home directory. */
    char *pw_shell; /* Shell program. */
};
```

<br>

getgrgid : 그룹 id로 그룹 정보 제공

```c
#include <grp.h>
struct group *getgrgid(gid_t gid);
```



<br>

## /etc/passwd 파일

- 사용자 계정 별로 콜론(:) 으로 구분된 7개의 필드를 기록 

- Login name 

  - 로그인할 때 사용하는 ID (문자로 표현됨) 

  - user name이라고도 함. 

- Password 
  - 암호화된 패스워드 

- User ID 

  - Number로 표현된 ID 

  - root (슈퍼유저) login name은 user ID가 0임 

- Group ID 
  - 사용자가 소속된 Group의 ID

- Comment 
  - 사용자에 대한 설명 
- Home directory 
  - 로그인하면 최초 존재하는 디랙토리 
  - HOME 환경 변수의 값 
- Login shell
  - 쉘 프로그램의 종류 
  - 로그인 후 사용자가 처음 접하는 프로그램 
  - SHELL 환경 변수의 값



<br>

## 프로세스의 사용자 ID

- 프로세스의 실제/유효 사용자 ID 변경 
- 프로세스의 실제/유효 그룹 ID 변경

```c
#include <sys/types.h>
#include <unistd.h>
int setuid(uid_t uid); //프로세스의 실제 사용자 ID를 uid로 변경한다.
int seteuid(uid_t uid); //프로세스의 유효 사용자 ID를 uid로 변경한다.
int setgid(gid_t gid); //프로세스의 실제 그룹 ID를 gid로 변경한다.
int setegid(gid_t gid); //프로세스의 유효 그룹 ID를 gid로 변경한다.
```



<br>

## st_mode

- Stat 구조체  \<sys/stat.h>
  - 파일 타입과 사용권한 정보는 st->st_mode 필드에 함께 저장됨. 
- st_mode 필드 
  - 4비트: 파일 타입 
  - **3비트: 특수용도** 
  - 9비트: 사용권한 
    - 3비트: 파일 소유자의 사용권한 
    - 3비트: 그룹의 사용권한 
    - 3비트: 기타 사용자의 사용권한

![image](https://user-images.githubusercontent.com/79521972/168038128-16249028-f2d2-414c-908c-b895d75a1d8e.png)

<br>

- **special bits (3비트: 특수용도)** 
  - #define S_ISUID 0004000     /* set uid on execution */ 
  - #define S_ISGID 0002000     /* set group id on execution **/*
  - #define S_ISVTX 0001000     /* save text(sticky bit) */

Stick bit의 사용 

- 원래의 용도는 실행 프로그램을 swap area에 저장하여 속도 향상. 
  - virtual memory의 사용으로 필요가 없어짐. 
- 기능이 확장되어 현재는 다음 용도로 많이 사용됨.
  - /tmp, /var/spool/uucppublic에서는 모든 사용자가 파일을 읽고, 쓰고, 실행 가능 
  - 하지만, 다른 사용자의 파일을 delete/rename하면 안 된다. 
  - 이러한 directory들은 sticky bit로 설정한다.(위 두 기능을 수행하기 위해서)

<br>

## Set-user-ID and set-group-ID

- real ID and effective ID 
  - **real user**(group) ID 
    - Identifies who we really are. 
    - Written in the password file(/etc/passwd,/etc/shadow). 
  - **effective user**(group) ID 
    - Determines file access permission, signal delivery permission. 
    - 내가 누군지를 입증해 주는 id가 아니라 file을 access할 때나 signal delivery 할 때의 허락을 결정하는 id이다.
    - effective User ID가 0인 프로세스를 privileged process라고 함 
      - 모든 권한을 갖고 있음.
  - **Normally**, effective user(group) ID = real user(group) ID. 
  - But, effective user(group) ID **can be different** from the real user(group) ID, 
    - In the case of executing programs with the setuid(setgid) bit set. 
      - 이에 대해서 아래에서 설명하도록 하겠다.
    - **In the case of calling system call: setuid(). setgid()** 
      - 원래 둘이 같았었는데 직접 변경한 경우

<br>

## set-user-id 실행파일(중요)

- 특별한 실행권한 set-user-id(set user ID upon execution) 
  - 어떤 프로세스가 set-user-id 설정된 실행파일을 실행하면 (ex. shell 명령어를 치면)
  - 이 프로세스의 유효 사용자 ID는 그 실행파일의 소유자로 바뀜. 
  - 이 프로세스는 실행되는 동안 그 파일의 소유자 권한을 갖게 됨. 
    - exec()을 통해 set-user-id 프로그램을 메모리에 탑재하면 프로세스의 유효 사용자 ID를 실행 파일의 사용자 id로 설정됨 
- **예** : /usr/bin/passwd 명령어 실행 파일 
  - set-user-id 실행권한이 설정된 실행파일이며 소유자는 root 
  - 일반 사용자가 쉘에서 이 파일을 실행하게 되면, 이 프로세스의 유효 사용자 ID는 **root**가 됨. 
  - /etc/passwd처럼 root만 수정할 수 있는 파일의 접근 및 수정 가능(이 실행파일을 실행시키는 동안에는)

<br>

## Set-user-ID and set-group-ID

- passwd program를 이용하여 password를 바꿀 경우 
  - passwd program을 실행하는 도중 root의 권한을 부여받음. (잠정적으로)
    - **Real user ID of passwd program: user** 
    - **Effective user ID of passwd program: root**

![image](https://user-images.githubusercontent.com/79521972/168038743-5199badc-29cf-4ecc-9021-b70636a069a3.png)

s가 set-user-ID가 세팅 되었음을 의미한다.

이를 실행하면 effective user ID가 root로 setting 되어 passwd 파일을 수정할 수 있게 된다.(root만 할 수 있던)

<br>

## Ownerships of new files

- When a file is created using open or creat 
  - Generally, **the user ID of a new file** is set to the effective user ID of the process

![image](https://user-images.githubusercontent.com/79521972/168038789-911fc7e3-a32d-49ec-8862-56af3bb6e34b.png)

<br>

## set-user-id 실행파일

- set-user-id 실행권한은 심볼릭 모드로 's'로 표시, 8진수로는 4000

```shell
$ ls –asl /bin/su /usr/bin/passwd
32 -rwsr-xr-x. 1 root root 32396 2011-05-31 01:50 /bin/su
28 -rwsr-xr-x. 1 root root 27000 2010-08-22 12:00 /usr/bin/passwd
```

- set-uid 실행권한 설정 

```shell
$ chmod 4755 file1
```

- set-gid 비트 설정 // 강제 잠금 설정

```shell
$ chmod 2755 file1
```

<br>

- $ /bin/su 
- \# chown root uid   : uid 실행파일에게 root 권한을 준다.
- \# exit 
- $ chmod 4755 uid 
- $ uid

나의 실제 사용자 ID : 109 (chang)
나의 유효 사용자 ID : 0 (root)
나의 실제 그룹 ID : 101 (faculty) 
나의 유효 그룹 ID : 101 (faculty)

```shell
$ uid
나의 실제 사용자 ID : 109 (chang)
나의 유효 사용자 ID : 109 (chang)
나의 실제 그룹 ID : 101 (faculty)
나의 유효 그룹 ID : 101 (faculty)
```



<br>

## 8.5 프로세스 구조

## 프로세스

- 프로세스는 실행중인 프로그램이다. 
  - Program instance of program definition 
- 프로그램 실행을 위해서는 
  - 프로그램의 코드, 데이터, 스택, 힙, U-영역 등이 필요하다. 
- 프로세스 이미지(구조)는 메모리 내의 프로세스 레이아웃 
- 프로그램 자체가 프로세스는 아니다 !



<br>

## 프로세스 구조

- 프로세스 구조

![image](https://user-images.githubusercontent.com/79521972/168039330-78c5ac79-3fe4-47f1-a6f3-dfc977368c6e.png)

command-line arguments and environment variables

<br>

- **텍스트**(text) 
  - 프로세스가 실행하는 **실행코드**를 저장하는 영역이다. 
  - 읽기 전용, 공유는 가능함 
- **데이터** (data) 
  - 전역 변수(global variable) 및 정적 변수(static variable)를 위한 메모리 영역이다. 
  - 프로세스가 종료되지 않으면 없어지지 않는다.
  - **Initialized data segment** : exec()에 의해 프로그램에서 읽어옴
    - int x = 10; 
  - **Unnitialized data segment** (bss) : exec()에 의해 0으로 초기화 
    - int array[10]; 
- **힙**(heap) 
  - 동적 메모리 할당을 위한 영역이다. (malloc)
  - C 언어의 malloc 함수를 호출하면 이 영역에서 동적으로 메모리를 할당해준다. 
- **스택**(stack area) 
  - 함수 호출을 구현하기 위한 실행시간 스택(runtime stack)을 위한 영역으로 활성 레코드(activation record, or stack frame)가 저장된다. 
  - **activation record** 안에` automatic local variables`, `parameter variables`, `return address`, `호출함수의 레지스터` 등이 저장 
  - 함수가 종료되면 없어지는 이유는 함수가 호출될 때 만들어진 stack frame이 함수가 return 될 때 없어지게 된다. 그래서 local variable은 함수가 실행되는 도중에만 존재하고 그 함수가 return 하면 없어지게 되는 특징을 갖는 것이다.
- U-영역(user-area) 
  - 열린 파일 디스크립터, 현재 작업 디렉터리 등과 같은 **프로세스의 정보를 저장**하는 영역이다.
  - command line argument



<br>

## 핵심 개념

- 프로세스는 실행중인 프로그램이다. 
- 쉘은 사용자와 운영체제 사이에 창구 역할을 하는 소프트웨어로 사 용자로부터 명령어를 입력받아 이를 처리하는 명령어 처리기 역할을 한다. 
- 프로그램이 실행되면 프로그램의 시작 루틴에게 명령줄 인수와 홖경 변수가 젂달된다. 
- exit()는 뒷정리를 한 후 프로세스를 정상적으로 종료시키고 _exit()는 뒷정리를 하지 않고 프로세스를 즉시 종료시킨다. 
- exit 처리기는 exit()에 의한 프로세스 종료 과정에서 자동으로 수행된 다. 
- 각 프로세스는 프로세스 ID를 갖는다. 각 프로세스는 자싞을 생성해 준 부모 프로세스가 있다. 
- 각 프로세스는 실제 사용자 ID와 유효 사용자 ID를 가지며 실제 그룹 ID와 유효 그룹 ID를 갖는다. 
- 프로세스 이미지는 텍스트(코드), 데이터, 힙, 스택 등으로 구성된다.

