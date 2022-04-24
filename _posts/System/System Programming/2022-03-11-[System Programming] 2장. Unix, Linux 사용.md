---
layout: single
title: "[System Programming] 2장. Unix,Linux 기초"
categories: ['System', 'System Programming']
tag: ['System Programming', 'Unix']
toc: true
toc_sticky: true
---



# 제 2장 유닉스 사용

# 2. 유닉스 사용

## 2.0 유닉스 기초

### Logging in

- login
  - Unix는 multi-user system
  - 따라서, "login name + password"로 시스템에 로그인
  - /etc/passwd라는 파일에 다음과 같은 정보(entry) 저장.
    - login name:password:UID:GID:comment:home directory:shell program

![image](https://user-images.githubusercontent.com/79521972/157785790-a22cfb94-2443-4252-971d-2a1901e0b850.png)

### Shell(prompt)

- Unix는 기본적으로 **command line interface**를 사용
  - command line interpreter라고도 함.

- 사용자의 명령을 읽어들여 실행하는 명령어 해석기가 필요.(terminal or shell script)
- 로그인 시 아래와 같은 창이 뜸

![image](https://user-images.githubusercontent.com/79521972/157786221-f0b63cb2-dad1-44b0-9b8d-416b07abefcc.png)

- .(current directory) 
- ..(parent directory)

- Shell의 종류
  - Bourne shell, C shell, Korn shell, Tenex shell, Born Again shell

- Linux 계열(window도 마찬가지)의 파일 계층은 트리 구조를 갖는다.



<br>

### File system

- File system의 구성
  - File : 실제 정보(data)를 담고 있는 문서
  - Special file: IO장치와 같은 것 or network connection 등등
  - Directory: 여러 개의 **파일들에 대한 정보**를 담고 있는 것
    - Directory entry(file name + attribute pointer)들의 집합
    - Hard link
- Hierarchical structure
  - / : root directory
  - 동그라미: 일반 file
  - 네모: directory entry
  - ![image](https://user-images.githubusercontent.com/79521972/157786440-7dd4baf8-b69d-44cd-9781-30e343a5b357.png)




- Attribute
  - 모든 directory entry는 모든 파일의 속성 정보를 전부 갖고 있음
  - Per file data sturcture
  - `Type`, `size`, `owner`, `permission`, `access time` 등
  
  ![image](https://user-images.githubusercontent.com/79521972/157786589-8f1e4308-fd8c-4706-93b6-f2103ccd3f04.png)
  
  - d로 시작하는 것은 directory를 의미
  - -로 시작하는 것은 일반 파일을 의미
  - -a: all이라는 의미로 hidden file까지 전부 보여준다.
  - -l: long이라는 의미로 file의 자세하게 나오게 해 준다.
  - x: navigate할 수 있다.
  - rw: read/write
  - obama는 users 그룹의 멤버다.
  - 4096: 해당 entry의 size(byte 단위)
  - 가장 최근에 수정된 날짜와 시간



- ls: directory에 들어있는 모든 파일의 속성을 보여주는 명령어
  - 맨 앞이 d로 시작하면 디렉토리라는 의미이고 맨앞이 '-'로 시작하면 파일이라는 의미이다.

<br>

- Filename (파일 이름)

  - NULL과 '/'를 <span style="color:red">제외한</span> 문자열로 구성.
    - '/'는 경로 이름을 구성하는데 있어서 각 경로를 구분하는데 사용되는 문자이기 때문이다.
    - Null 문자는 경로 이름을 종료한다
      - 즉 Null은 path name의 마지막을 의미하기 때문에 사용하면 안된다.
  - Some Unix FS 14자로 제한, BSD의 경우 255자 이내
  - Special filenames (얘도 파일로 간주)
    - 2개의 파일 이름은 새로운 디렉토리가 만들어질 때 항상 <span style="color:red">자동적으로 생성</span>된다.
      - . (current directory)
      - .. (parent directory)
    - current directory와 parent directory의 속정 정보(commition, size,read/write 등)를 갖게 하기 위해(쉽게 찾기 위해)

<br>

- Pathname (경로 이름)
  - 파일 위치를 지정하기 위한 수단
  - /home/obama   -> **home directory (login시 위치)**
  - /home/obama/test -> working directory (작업 위치)
  

![image](https://user-images.githubusercontent.com/79521972/157787269-b93fab90-ef77-47ad-81d4-d42644ddae4a.png)

- pwd: print working directory

- **Absolute path name(절대 경로)**
  - root directory를 기점으로 원하는 경로를 전부 작성하여 들어가는 방법
  - A path name that begins with a slash
  
- **Relative path name(상대 경로)**
  - current working directory를 기점으로 다음 경로로 들어가는 방법
  - A path name that begins with CWD


| $ cd ~     | is equivalent to 'cd'          | go to home directory |
| ---------- | ------------------------------ | -------------------- |
| **$ cd -** | **is equivalent to '$OLDPWD'** |                      |

<br>

c프로그램으로 폰노이만 아키텍처를 구현해 보기

### Input and output

외부 디바이스도 파일로 간주

- **File descriptor**
  - In C, fopen() 안에 있는 인자는 file 경로를 받는데 이 함수가 이 파일의 file descriptor를 return 한다.
  - **Non-negative integers** that the kernel used to identify the files being accessed by a process
  - special file도 file이기 때문에 descriptor를 리턴한다.
  - file descriptor는 고정된 숫자가 아니라 매 실행마다 바뀔 수 있는 것이다.
    - 하지만 0, 1, 2는 고정된 숫자이다.
  

![image](https://user-images.githubusercontent.com/79521972/157791307-f4dda25f-30da-4892-92d7-4754d384beae.png)

process가 어떤 program에서 생성이 될 때, 기본적으로 3 개의 file descriptor가 자동적으로 생성이 되고(0, 1, 2) process에서 첫 파일을 생성하게 되면 4번째 위치인 3번 index에 파일이 생성이 된다.

<br>

### Programs and processes

- Program
  - instruction의 나열
  - An executable file residing on disk(program definition)
  - program의 성능을 결정하는 것은 algorithm+data structure
  
- Process
  - An executing instance of a program (also called task)
  - source file -> 실행 파일
    - compile / assemble
    - default로 실행 파일이 만들어지면 a.out 이라는 이름을 갖게 된다.
      - a.out : **program definition**
      - program definition은 지시(direction)만 내리고 실제 공간을 확보하지는 않는다.
  
  
  - loader를 통해 위 a.out을 main memory에 탑재
  - program definition에 따라서 공간까지 실제로 확보하는 것은 **program instance process**
  - Process id: a unique numeric identifier for process
    - non-fixed numeric identifier
  


![image](https://user-images.githubusercontent.com/79521972/157791647-47465c41-8f27-4cf7-9f8c-2f90675eb69c.png)

<br>

#### Main memory

- text
  - instruction set
- data
  - global data, static 변수 
  - 함수 호출과 관계없이 프로그램 종료 때까지 살아있어야 하기 때문에 stack 공간에 저장되면 안됨
- heap
  - malloc(동적 할당)을 위해 남겨둔 공간
- 완충지역
  - 너무 많은 함수 호출로 인해 stack frame이 너무 많이 쌓여 위 메모리 공간을 침범하는 것을 막기 위한 공간
  - stackoverflow 발생
- stack
  - 함수 호출에 의해 변수 등이 저장되는 공간
  - 함수마다 stack frame이라는 것이 만들어진다.
    - 함수의 지역변수는 해당 stack frame에 생기는 것이다.
    - 함수가 리턴할 때 해당 데이터가 사라짐
  - **stack frame**
    - os가 main함수를 호출하면 stack 아래에 main function에 대한 stack frame이 생김
    - 이곳에는 해당 함수에서 사용하는 local 변수들이 저장됨
      - parameter 변수, return address(함수 종료 후 다시 돌아가야 할 주소) 등도 포함 
  - 그래서 다른 함수에서 같은 변수 이름을 사용해도 둘은 다른 stack frame안에 존재하기 때문에 서로 달리 사용할 수 있는 것이다.

<br>

#### Process Life Cycle

- program definition이 실행되는 것이 아니라 그것을 바탕으로 process life cycle이 실행되는 것이다.
  - 프로그램 실행요구

- new : process를 만드는 과정
  - process control block(PCB) 생성
  - pid 생성
- ready
  - 줄 서는 과정(실행을 기다림)
- running
  - 누가 먼저 실행할지를 결정하는 과정
  - 우선순위 scheduling, FIFO

- wait
  - I/O를 기다리는
- terminated
  - new에서 address 할당했던 것들과 같은 것들을 회수하여 흔적을 지우는 과정



이와 같이 program definition은 하드디스크에 계속 머물고 있는 것과 달리 process life cycles는 메모리를 동적으로 실행을 했다가 회수까지 전부 한다.

<br>

---

교과서 밖 내용

과거의 os는 multi programming을 지원하지 않았음. 즉, 한 program이 실행 중이면 다른 program을 실행하지 못했음.

multi-programming이란 cpu가 쉬는 타임에 다른 프로그램을 실행시키는 것이다.

- pre-emptive(강제로 뺏는) 스케쥴링: 실행중인 program이 우선순위가 떨어진다 판단되면 cpu 점유를 뺏어서 ready로 보내고 ready에서 차례를 기다리던 높은 우선순위를 갖는 program이 그 자리를 차지한다.

- non-pre-emptive 스케쥴링
- 라운드 로빈 스케쥴링: 번갈아 가면서 우선순위 할당, 공정성(fairness)

---



### Error handling

- When an error occurs,
  - A **negative value** is often returned
  - <span style="color:red">errno</span>(errorn number) is set to a value that gives additional informaion(error가 발생하면 발생한 이유가 환경 변수 errno에 저장이 된다.)
    - e.g. When open, returns -1 if an error occurs 
    - error from open has 15 possible errno values
      - file doesn't exit, permission problem, ...
  - <errno.h>
    - 어떤 number가 어떤 error인지 정보가 담겨있는 헤더 파일
    - Defines errno symbols and constants for each error.
    - Each constant begins with the character E.(항상 E로 시작하는 단어임)
      - E.g. ENOENT, EACCESS,...

<br>

```c
#include <string.h>
char *strerror(int errnum);
/* maps errnum(errno value) into an error message string & returns a pointer to the string*/
```

```c
#include <stdio.h>
void perror(const char *msg);  /*outputs the string pointed to by msg*/
						/*output format"string pointed by msg:errormessage of errno"*/
```

- strerror와 perror의 사용 예

```c
#include "apue.h"
#includ <errno.h>

int main(int argc, char *argv[])
{
    fprintf(stderr, "EACCES: %s\n", strerror(EACCES)); //error number to message string
    errno = ENOENT;
    perror(argc[0]);
    exit(0);
}
```

- 실행 예

```shell
$ ./a.out
EACCES: Permission denied
./a.out: No such file or directory
```

<br>

### User identification

- user ID
  - **a numeric value** that identifies the user
  - is assigned by the system administrator.
  - **0**: superuser or root
- group ID
  - is used to collect users together into projects.
- If the full ASCII user/group name is used instead?(ASCII를 사용하지 않는 이유)
  - additional disk space will be required.
  - **the cost of string** comparison for permission check is high.

- userID와 userName은 의미는 같다.

- userID와 groupID의 출력 예제

  - ```c
    #include "apue.h"
    
    int main(void) 
    {
        //userId, groupId 를 알려주는 함수
        printf("uid = %d, grid = %d\n", getuid(), getgid());
        exit(0);
    }
    ```
    
  - 실행 예
  
  - ```shell
    $ ./a.out
    uid = 205, grid = 105
    ```



<br>

#### Signals(시험)

통보(notofication) 수단

- **Signal** 

  - is used to notify a process that some condition has occurred.(by kernal)
  - e.g. if divide by zero, SIGFPE(floating point exception) is sent to the process

- **Action of process received the signal**(받은 시그널에 대한 대응 방법) (시험에 나온 부분)

  - ignore the signal.
  - let the **default action** occur.
  - execute your own action.

- Signal의 예제
  - kill

    ![image](https://user-images.githubusercontent.com/79521972/157793357-0d8855f1-879c-4195-9343-6d3270b3b1ea.png)



<br>

### Time values

- calendar time
  - \# of seconds since the Epoch
    - Epoch is 00:00:00 January 1, 1970.
  - time_t
- process time(CPU time)
  - CPU time used by a process (measured in clock ticks.)
  - clock ticks are 50, 60, 100 ticks/seconds.
  - clock_t

- Process time 의 표현
  - **clock time**
    - the amount of time the process **takes to run**
    - depends on **\# of other processes** being run on the system
    - 그래서 하나의 프로세스를 가지고서만 clock time을 구하는 것이 의미가 있다.
  - **user CPU time**
    - CPU time attributed to **user instruction**.(user code)
  - **system CPU time**
    - CPU time attributed to **the kernel**.(kernal code)

![image](https://user-images.githubusercontent.com/79521972/157793722-84111232-36b7-4da6-9788-07aad10bbe3f.png)

<br>

## 2.1 기본 명령어

간단한 명령어 사용

```shell
$ date // 날짜를 출력하거나 설정
$ hostname
$ uname //시스템 정보를 출력
$ who //시스템에 로그인되어 있는 사용자들을 로그인 정보와 같이 출력해주는 명령어
$ ls //list
$ clear //터미널의 내용을 모두 지우는 명령어
$ passwd //사용자 계정의 비밀번호를 입력 또는 변경하는 명령어
```



## 2.2 파일 및 디렉토리

### 파일의 종류

- 일반 파일(ordinary file)
  -  데이터를 갖고 있으면서 disk에 저장된다.
- 디렉터리/폴더
  - 디렉터리 자체도 하나의 파일로 한 디렉터리는 다른 디렉터리들을 포함함으로써 계층 구조를 이룬다.
  - 부모 디렉터리는 다른 디렉터리들을 서브 디렉터리로 갖는다.
- 특수 파일(special file)
  - 물리적인 장치에 대한 내부적인 표현
  - 키보드(stdin), 모니터(stdout), 프린터 등도 UNIX에서는 모두 파일처럼 사용



<br>

## 디렉터리 계층 구조

- 유닉스의 디렉터리는 루트로부터 시작하여 계층 구조를 이룬다.

![image](https://user-images.githubusercontent.com/79521972/157794129-fc8f9f79-bac9-43a9-8e1f-79f8da72d1d7.png)

<br>

- 리눅스 디렉터리(유닉스 디렉터리와 유사한 계층 구조를 갖는다.)

![image](https://user-images.githubusercontent.com/79521972/157794229-485771de-1283-424b-839a-f7dca2e36d27.png)

<br>



### 홈 디렉터리/현재 작업 디렉터리

- 홈 디렉터리(home directory)
  - 각 사용자마다 별도의 홈 디렉터리가 있음
  - 사용자가 **로그인**하면 홈 디렉터리에서 작업을 시작함
- 현재 작업 디렉터리(curren working directory, cwd)
  - 현재 작업 중인 디렉터리
  - 로그인 하면 홈 디렉터리에서부터 작업이 시작된다.



<br>

### 디렉터리 관련 명령

- pwd(print working directory) (시험)

  - **현재** 작업 디렉터리를 출력

  - ```shell
    $ pwd
    ```

- cd (change directory)

  - 현재 작업 디렉터리를 **이동**

  - ~~~shell
    $ cd [디렉터리]
    ~~~

- mkdir (make directory)

  - 새 디렉터리를 **만듦**

  - ```shell
    $ mkdir 디렉터리
    ```

- ls(list)

  - 디렉터리의 내용을 리스트

- $ ls

  cs1.txt

- $ ls -s     -> -s(size) ; 히든 파일 미포함

  총 6

  6cs1.txt

- $ ls -a         -> -a(all; 히든,special 파일 포함)

  . .. cs1.txt

- $ ls -l             -> -l(long,자세하게)

  -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt

  - 2088 바이트

- $ ls -asl

  총 10                              ->    (10개의 block을 사용)

  2 drwxr-xr-x 2 chang faculty 512 4월 16일 13:37 .
  2 drwxr-xr-x 3 chang faculty 512 4월 16일 13:37 ..
  6 -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt

(시험)

<br>

**정리**

| 명령어      | 의미                        |
| ----------- | --------------------------- |
| ls          | 파일 및 디렉터리 리스트     |
| ls -a       | 모든 파일과 디렉터리 리스트 |
| ls -asl     | 모든 파일 자세히 리스트     |
| mkdir       | 디렉터리 만들기             |
| cd 디렉터리 | 디렉터리로 이동             |
| cd          | 홈 디렉터리로 이동          |
| cd ~        | 홈 디렉터리로 이동          |
| cd ..       | 부모 디렉터리로 이동        |
| pwd         | 현재 작업 디렉터리 프린트   |

<br>

### 경로명

- 파일이나 디렉터리에 대한 정확한 이름
- **절대 경로명** (absolute pathname)
  - **루트 디렉터리**로부터 시작하여 경로 이름을 **정확하게** 적는 것
- **상대 경로명** (relative pathname)
  - **현재 작업 디렉터리**부터 시작해서 경로 이름을 적는 것

![image](https://user-images.githubusercontent.com/79521972/157795237-313eef89-6288-437a-abc9-7aaf95dda231.png)

<br>

### 파일 내용 리스트

- 파일 내용 출력과 관련된 다음 명령어들
  - cat(catalog), more, head, tail, wc, 등

- $ [명령어] [파일]
- $ [명령어] [파일*]  : 파일 이름을 줄 수도 있고 안 줄 수도 있다.
- $ [more] [파일+] : 파일을 한 개 이상을 줄 수 있다.

<br>

```
$ cat cs1.txt        -> cs1.txt의 내용을 모니터에 print
```

```
$cat                 -> 파일을 지정하지 않았기 때문에 키보드 입력을 출력하는 모드이다.

...                  -> 사용자 입력

^D                   -> 입력 종료
```

```
$ cat > cs1.txt       -> redirection: 키보드로 입력한 내용은 txt파일에 출력
...
^D
```

<br>

- more 명령어

  - 하나 이상의 파일 이름을 받을 수 있으며(한 페이지에 해당하는) 
  - 각 파일의 내용을 페이지 단위로 출력
  - 텍스트 파일의 내용을 한 번에 한 화면씩 보여주기 위한 명령어

- head 명령어

  - 파일의 앞부분 (10줄)을 출력한다.

- tail 명령어

  - 파일의 뒷부분 (10줄)을 출력한다.

- wc(word count)

  - 파일에 저장된 줄, 단어, 문자의 개수를 세서 출력

  - ```shell
    $ wc cs1.txt
    38 318 2088 cs1.txt
    ```

<br>

### cp 명령어(copy)

- $ cp [파일1] [파일2]
  - 파일 1의 복사본 파일 2를 현재 디렉터리 내에 만듦

```shell
$ cp cs1.txt cs2.txt
$ ls -l cs1.txt cs2.txt
-rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt
-rw-r--r-- 1 chang faculty 2088 4월 16일 13:45 cs2.txt  # 더 나중에 수정된 것을 볼 수 있음
```

-  cp [파일] [디렉터리]

  - "파일 1의 복사본을 디렉터리 내에 만들어라."

  - ```shell
    $ cp cs1.txt /tmp
    ```

- mv(move)

  - 파일 1의 **이름**을 파일 2로 **변경**한다.

  - `$ mv 파일1 파일2`

  - ```shell
    $ mv cs2.txt cs3.txt
    $ ls -l
    -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt
    -rw-r--r-- 1 chang faculty 2088 4월 16일 13:56 cs3.txt
    ```

  - 파일을 디렉터리 내로 **이동**

    - `$ mv [파일] [디렉터리]`

    - ```shell
      $ mv cs3.txt /tmp
      ```

- rm(remove) 명령어

  - 명령줄 인수로 받은 파일(들)을 지운다.

  - `$ rm 파일+`

  - ```shell
    $ rm cs1.txt
    ```

- $ rm -r 디렉터리

  - 디렉터리 내의 모든 파일 및 하위 디렉터리들을 **단번에** 지운다.

- rmdir(remove dircectory) 명령어

  - 명령줄 인수로 받은 디렉터리(들)을 지운다.

  - `$ rmdir 디렉터리+`

  - 주의: 디렉터리 내에 아무 것도 없어야 한다.

  - ```shell
    $ rmdir test
    ```

<br>

**정리**

| 명령어           | 의미                                              |
| ---------------- | ------------------------------------------------- |
| cat 파일*        | 파일 디스플레이                                   |
| more 파일+       | 한 번에 한 페이지씩 디스플레이                    |
| head 파일*       | 파일의 앞부분 디스플레이                          |
| tail 파일*       | 파일의 뒷부분 디스플레이                          |
| wc 파일*         | 줄/단어/문자 수 세기                              |
| cp 파일1 파일2   | 파일1을 파일2로 복사                              |
| mv 파일1 파일2   | 파일1을 파일2로 이름 변경                         |
| rm 파일+         | 파일 삭제                                         |
| rmdir 디렉터리+  | 디렉터리 삭제                                     |
| grep 키워드 파일 | 파일에서 키워드 찾기                              |
| who              | 호스트에 로그인한 사용자의 정보를 출력하는 명령어 |

<br>

## 2.3 파일 속성

### 파일 속성(file attribute)

- 파일의 이름, 타입, 크기, 소유자, 사용 권한, 수정 시간

```shell
$ ls -sl cs1.txt 
6 -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt
```

![image](https://user-images.githubusercontent.com/79521972/157796618-17da1bad-679c-4357-ba65-28c881bc8ff5.png)

<br>

### 사용 권한(permission mode)

- 읽기(r), 쓰기(w), 실행(x) 권한

![image](https://user-images.githubusercontent.com/79521972/157796686-2f914b09-5f01-4b7f-9b3a-3437a6143066.png)

- 파일의 사용 권한은 `소유자(owner)/그룹(group)/기타(others)`로 구분하여 관리한다.
- 예
  - 소유자 그룹 기타
  -    rw-r--r--

<br>

### chmod(change mode)

- 커미션 정보를 변경할 때 사용하는 명령어

- 파일 혹은 디렉터리의 사용권한을 변경하는 명령어
  - `$ chmod [-R] 사용 권한 파일`
  - -R 옵션은 디렉터리 내의 **모든** 파일, 하위 디렉터리에 대해서도 적용
- ![image](https://user-images.githubusercontent.com/79521972/157796858-c138ffc0-bdfb-4b3e-ab99-8b711087dae2.png)
  - 3 digit으로 사용
    - owner, group, others

<br>

### chown(change owner)/ chgrp(change group)

- chown 명령어
  - 파일이나 디렉터리의 소유자를 변경할 때 사용한다.
  - `$ chown 사용자 파일`
  - `$ chown [-R] 사용자 디렉터리`
- chgrp 명령어
  - 파일의 그룹을 변경할 수 있다.
  - `$ chgrp 그룹 파일`
  - `$ chgrp [-R] 그룹 디렉터리`
- 파일의 소유자 또한 슈퍼 유저(superuser)만이 사용 가능!

<br>

## 2.4 입출력 재지정 및 파이프

### 출력 재지정(output redirection)

- 명령어의 표준 출력 내용을 모니터에 출력하는 대신에 파일에 저장

  - `$ 명령어 > 파일`

  - ```shell
    $ who > names.txt
    ```

  - 기존 파일의 내용이 지워짐

  - ![image](https://user-images.githubusercontent.com/79521972/157797328-13b40f4c-39ab-492e-8714-8dea1c08a2cb.png)

- 출력 재지정 예

  - ```shell
    $ cat > list1.txt
    Hi!
    This is the first list.
    ^D
    ```

  - ```shell
    $ cat > list2.txt
    Hello !
    This is the second list.
    ^D
    ```

  - ```Shell
    $ cat list1.txt list2.txt >list3.txt
    ```

  - ```shell
    $ cat list3.txt
    Hi!
    This is the first list.
    Hello !
    This is the second list.
    ```

말 그대로 출력 재지정이다! (화면 -> 파일)

<br>

### 출력 추가(append)

- 명령어의 표준 출력을 모니터 대신에 **기존 파일**에 추가

  - $ 명령어 >> 파일

  - ```shell
    $ cat >> list1.txt
    Bye !
    This is the end of the first list.
    ^D
    $ cat list1.txt
    Hi ! 
    This is the first list.
    Bye !
    This is the end of the first list.
    ```

<br>

### 입력 재지정(input redirection)

- 명령어의 표준 입력을 키보드 대신에 파일에서 받는다.(방향이 반대 '<')

  - $ 명령어 < 파일

  - ```shell
    $ wc < list1.txt         #word count
    4 17 71 list1.txt
    ```

<br>

### 문서 내 입력(here document)

- < : 지정된 파일로부터 입력을 받음
- \> : 지정된 파일로 출력
- \>\> 출력을 지정된 파일에 추가(append)
- **<<** : 실제 파일은 아니지만, 시스템에 입력되는 파일처럼 보이는 입력 라인

<br>

- 명령어의 표준 입력을 << 다음의 단어가 다시 나타날 때 까지의 내용으로 사용

- 명령어를 실행할 때 문서(script) 내에서 입력을 줄 때 사용

- 단어가 나올 때까지의 명령어를 실행 중인 프로그램에 **입력**해 줄 수 있다.

  - ```shell
    $ 명령어 << 단어            #(delimeter 단어)
    ...
    단어
    ```

  - ```shell
    $ wc << end
    hello ! 
    word count
    end
    2 420
    ```
  
  - list1.txt 파일의 워드 카운트 결과를 result 파일에 기록한다.
  
    ```shell
    $ wc < list1.txt > result
    ```
  
  - 

<br>

### 파이프(시험)

- 로그인 된 사용자들을 정렬해서 보여주기

```shell
$ who > names.txt
$ sort < names.txt
```



- $ 명령어1 | 명령어2
  - 명령어1의 표준 출력을 명령어2의 표준입력으로 바로 받는다.
  -  기호 | 는 좌측의 출력을 우측의 입력으로 인가한다. 

```shell
$ who | sort
```

-  첫 번째 방식과의 차이점은 위와 같이 작성하면 names.txt와 같은 임시 파일을 생성하지 않아도 된다는 점이다.



![image](https://user-images.githubusercontent.com/79521972/157798125-1df4be71-8192-4ba0-8ccf-0bd07ba2abcf.png)

<br>

## 2.5 후면 처리 및 프로세스

### 전면 처리 vs. 후면처리

- **전면 처리**

  - 명령어를 입력하면 명령어가 전면에서 실행되며 명령어 실행이 끝날 때까지 쉘이 기다려 준다.
  - 명령어는 키보드와 모니터로 입출력
  - **한 명령어만 실행 가능**, 실행 중인 명령어는 Ctrl-C 입력 시 강제 종료, Ctrl-Z 입력 시 실행 중단

  - 중단 된 명령어는 prompt에서 **fg** 명령어 입력 시 실행이 계속 됨

- **후면 처리**
  - 명령어들을 후면에서 처리하고 전면에서는 다른 작업을 할 수 있으면 **동시에 여러 작업**을 수행할 수 있다.
  - $ [명령어] &



<br>

### 후면 처리 예

fg(foreground)

- 현재 디렉토리에서 test.c라는 이름을 가진 파일을 찾아라

```shell
$ (sleep 100; echo done) &      #두 명령어를 background로 실행
[1] 8320
$ find . -name test.c -print &        #test.c 라는 파일을 찾아서 print(bg 실행)
[2] 8325
$ jobs # 후면(bg)에서 실행되고 있는 작업들을 리스트
[1] + Running ( sleep 100; echo done )
[2] - Running find . -name test.c –print
```

- $ fg %작업 번호

```shell
$ fg %1   #fg실행으로 바뀜
( sleep 100; echo done )
```

- 후면 처리 입출력

```shell
$ find . -name test.c -print > find.txt & # 출력 뒤섞임 방지(fg가 표준 출력이기 때문)
$ find . -name test.c -print | mail chang & # test.c 내용을 mail의 입력으로
$ wc < inputfile & # 후면처리는 키보드 입력 불가능, 파일로부터 받아야 함
```



<br>

### 프로세스(process)

- 실행 중인 프로그램을 프로세스(process)라고 부른다.

- 각 프로세스는 유일한 프로세스 번호 PID를 갖는다.

- ps(process) 명령어를 사용하여 나의 프로세스들을 볼 수 있다.(내가 실행시킨 프로세스)

  - ```shell
    $ ps
    PID TTY TIME CMD
    8695 pts/3 00:00:00 csh
    8720 pts/3 00:00:00 ps
    ```
    
  - ```shell
    $ ps u # 자세한 정보 출력
    USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
    chang 8695 0.0 0.0 5252 1728 pts/3 Ss 11:12 0:00 -csh
    chang 8793 0.0 0.0 4252 940 pts/3 R+ 11:15 0:00 ps u
    ```

- ps aux

  - ```shell
    $ ps aux # 시스템 내의 모든 프로세스 정보 출력
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

#### kill 명령어

- 프로세서를 강제적으로 종료 시키는 명령어

  - $ kill 프로세스 번호
  - $ kill %작업번호

  -  ```shell 
     $ kill 8320  //or
     $ kill %1
     [1] Terminated( sleep 100; echo done )
     ```



<br>

## 2.6 문서 편집기

- gedit

- kwrite



<br>

## 2장 정리

- 유닉스의 디렉터리는 루트로부터 시작하여 계층 구조를 이룬다.
- 절대 경로명은 루트 디렉터리부터 시작하고 상대 경로명은 현재 디렉터리부터 시작한다.
- 파일의 사용권한은 소유자, 그룹, 기타로 구분하여 관리한다.
- 출력 재지정은 표준 출력 내용을 파일에 저장하고 입력 재지정은 표준 입력을 파일에서 받는다.
- 실행 중인 프로그램을 프로세스라고 한다.

































