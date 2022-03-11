---
layout: single
title: "[System Programming] 유닉스 기초"
categories: ['System', 'System Programming']
tag: ['System Programming', 'Unix']
toc: true
toc_sticky: true
---



# 제 2장 유닉스 사용





# 2 유닉스 사용

## 2.0 유닉스 기초

### Logging in

- login
  - Unix는 multi-user system
  - 따라서, "login name + password"로 시스템에 로그링ㄴ
  - /etc/passwd라는 파일에 다음과 같은 정보(entry) 저장.
    - login name:password:UID:GID:comment:home directory:shell program

![image](https://user-images.githubusercontent.com/79521972/157785790-a22cfb94-2443-4252-971d-2a1901e0b850.png)

### Shell

- Unix는 기본적으로 command line interface를 사용
- 사용자의 명령을 읽어들여 실행하는 명령어 해석기가 필요.(terminal or shell script)

![image](https://user-images.githubusercontent.com/79521972/157786221-f0b63cb2-dad1-44b0-9b8d-416b07abefcc.png)

- Shell의 종류
  - Bourne shell, C shell, Korn shell, Tenex shell, Born Again shell



### File system

- File system의 구성
  - File : 실제 정보(data)를 담고 있는 문서
  - Directory: 여러개의 파일들에 대한 정보를 담고 있는 것
    - Directory entry(file name + attribute pointer)들의 집합
    - Hard link

- Hierarchical structure
  - ![image](https://user-images.githubusercontent.com/79521972/157786440-7dd4baf8-b69d-44cd-9781-30e343a5b357.png)



- Attribute
  - Per file data sturcture
  - `Type, size, owner, permission, access time` 등

![image](https://user-images.githubusercontent.com/79521972/157786589-8f1e4308-fd8c-4706-93b6-f2103ccd3f04.png)

- ls: directory에 들어있는 모든 파일의 속성을 보여주는 명령어
  - 맨 앞이 d로 시작하면 디렉토리라는 의미이고 맨앞이 '-'로 시작하면 파일이라는 의미이다.



- Filename

  - NULL과 '/'를 <span style="color:red">제외한</span> 문자열로 구성.
    - '/'는 경로이름을 구성하는데 있어서 각 경로를 구분하는데 사용되는 문자이기 때문이다.
    - Null 문자는 경로 이름을 종료한다

  - Some Unix FS 14자로 제한, BSD의 경우 255자 이내
  - Special filenames
    - 2개의 파일 이름은 새로운 디렉토리가 만들어질때 항상 자동적으로 생성된다.
      - . (current directory)
      - .. (parent directory)



- pathname
  - /home/obama   -> home directory (login시 위치)
  - /home/obama/test -> working directory (작업 위치)

![image](https://user-images.githubusercontent.com/79521972/157787269-b93fab90-ef77-47ad-81d4-d42644ddae4a.png)

- Absolute path name(절대 경로)
  - A path name that begins with a slash

- Relative path name(상대 경로)
  - A path name that begins with CWD

| % cd ~     | is equivalent to 'cd'          |
| ---------- | ------------------------------ |
| **% cd -** | **is equivalent to '$OLDPWD'** |



### Input and output

외부 디바이스도 파일로 간주

- File descriptor
  - Non-negative integers that the kernel used to identify the files being accessed by a process

![image](https://user-images.githubusercontent.com/79521972/157791307-f4dda25f-30da-4892-92d7-4754d384beae.png)

process가 어떤 program에서 생성이 될 때, 기본적으로 3 개의 file descriptor가 자동적으로 생성이 되고(0, 1, 2) process에서 첫 파일을 생성하게 되면 4번째 위치인 3번 index에 파일이 생성이 된다.

<br>

### Programs and processes

- Program
  - An executable file residing on disk
- Process
  - An executing instance of a program (also calles task)
  - Process id: a unique numeric identifier for process

![image](https://user-images.githubusercontent.com/79521972/157791647-47465c41-8f27-4cf7-9f8c-2f90675eb69c.png)

<br>

### Error handling

- When an error occurs,
  - A negative value is often returned
  - errno(errornumber) is set to a value that gives additional informaion(error가 발생하면 발생한 이유가 환경 변수 errno에 저장이 된다.)
    - e.g. When open, returns -1 if an error occurs 
    - error from open has 15 possible errno values
      - file doesn't exit, permission problem, ...
  - <errno.h>
    - Defines errno symbols and constants for each error.
    - Each constant begins with the character E.
    - E.g. ENOENT, EACCESS,...

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

strerror와 perror의 사용 예
```c
#include "apue.h"
#includ <errno.h>

int main(int argc, char *argv[])
{
    fpringf(stderr, "EACCES: %s\n", strerror(EACCES));
    errno = ENOENT;
    perror(argc[0]);
    exit(0);
}
```

실행 예

```shell
$ ./a.out
EACCES: Permission denied
./a.out: No such file or directory
```

<br>

### User identification

- user ID
  - a numeric value that identifies the user
  - is assigned by the system administrator.
  - 0: superuser or root
- gourp ID
  - is used to collect users together into projects.
- If the full ASCII user/group name is used instead?
  - additional disk space will be required.
  - the cost of string comparison for permission check is high.



- userID와 groupID의 출력 에제

  - ```c
    #include "apue.h"
    
    int main(void) 
    {
        printf("uid = %d.grid = %d\n", getui();getgid()));
        exit(0);
    }
    ```

  - 실행 예

  - ```shell
    $ ./a.out
    uid = 205, gid = 105
    ```



<br>

#### Signals

- Signal 
  - is used to notify a process that some condition has occurred.
  - e.g. i f divide by zero, SIGFPE(floating point exception) is sent to the process
- Action of process received the signal
  - ignore the signal.
  - let the default action occur.
  - execute your own action.

- Signal의 예제
  - kill

![image](https://user-images.githubusercontent.com/79521972/157793357-0d8855f1-879c-4195-9343-6d3270b3b1ea.png)





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
  - clock time
    - the amount of time the process takes to run
    - depends on \# of other processes being run on the system
  - user CPU time
    - CPU time attributed to user instruction
  - system CPU time
    - CPU time attributed to the kernel.

![image](https://user-images.githubusercontent.com/79521972/157793722-84111232-36b7-4da6-9788-07aad10bbe3f.png)

<br>

## 2.1 기본 명령어

간단한 명령어 사용

```shell
$ date
$ hostname
$ uname
$ who
$ ls
$ clear
$ passwd
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
  - 키보드(stdin), 모니터(stdout), 프린터 등도 파일처럼 사용



<br>

## 디렉터리 계층구조

- 유닉스의 디렉터리는 루트로부터 시작하여 계층구조를 이룬다.

![image](https://user-images.githubusercontent.com/79521972/157794129-fc8f9f79-bac9-43a9-8e1f-79f8da72d1d7.png)

- 리눅스 디렉터리(유닉스 디렉터리와 유사함)

![image](https://user-images.githubusercontent.com/79521972/157794229-485771de-1283-424b-839a-f7dca2e36d27.png)



### 홈 디렉터리/현재 작업 디렉터리

- 홈 디렉터리(home directory)
  - 각 사용자마다 별도의 홈 디렉터리가 있음
  - 사용자가 로그인하면 홈 디렉터리에서 작업을 시작함
- 현재 작업 디렉터리(curren working directory, cwd)
  - 현재 작업 중인 디렉터리
  - 로그인 하면 홈 디렉터리에서부터 작업이 시작된다.





<br>

### 디렉터리 관련 명령

- pwd(print working directory)

  - 현재 작업 디렉터리를 출력

  - ```shell
    $ pwd
    ```

- cd (change directory)

  - 현재 작업 디렉터리를 이동

  - ~~~shell
    $ cd [디렉터리]
    ~~~

- mkdir (make directory)

  - 새 디렉터리를 만듬

  - ```shell
    $ mkidr 디렉터리
    ```

- ls(list)

  - 디렉터리의 내용을 리스트

- $ ls

  cs1.txt

- $ ls -s     -> -s(size)

  총 6

  6cs1.txt

- $ ls -a         -> -a(all)

  . .. cs1.txt

- $ ls -l             -> -l(long,자세하게)

  -rw-r--r-- 1 cjamge faculty 2088 4월 16일 13:37 cs1.txt

  - 2088 바이트

- $ ls -asl

  총 10

  2 drwxr-xr-x 2 chang faculty 512 4월 16일 13:37 .
  2 drwxr-xr-x 3 chang faculty 512 4월 16일 13:37 ..
  6 -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt

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
- 절대 경로명 (absolute pathname)
  - 루트 디렉터리로부터 시작하여 경로 이름을 정확하게 적는 것
- 상대 경로명 (relative pathname)
  - 현재 작업 디렉터리부터 시작해서 경로 이름을 적는 것

![image](https://user-images.githubusercontent.com/79521972/157795237-313eef89-6288-437a-abc9-7aaf95dda231.png)

<br>

### 파일 내용 리스트

- 패일 내용 출력과 관련된 다음 명령어들
  - cat(catalog), more, head, tail, wc, 등

- $ 명령어 파일
- $ 명령어 파일*  : 파일 이름을 줄 수도 있고 안 줄 수도 있다.
- $ more 파일+ : 파일을 한 개 이상을 줄 수 있다.

<br>

```
$ cat cs1.txt        -> cs1.txt의 내용이 모니터에 print
```

```
$cat                 -> 파일을 지정하지 않았기 때문에 키보드 입력을 출력하는 모드이다.

...                  -> 사용자 입력

^D                   -> 입력 종료
```

```
$ cat > cs1.txt       -> redirection:키보드로 입력한 내용은 txt파일에 출력
...
^D
```

<br>

- more 명령어

  - 하나 이상의 파일 이름을 받을 수 있으며 각 파일의 내용을 페이지 단위로 출력

- head 명령어

  - 파일의 앞부분(10줄)을 출력한다.

- tail 명령어

  - 파일의 뒷부분(10줄)을 출력한다.

- wc(word count)

  - 파일에 저장된 줄, 단어, 문자의 개수를 세서 출력

  - ```shell
    $ wc cs1.txt
    38 318 2088 cs1.txt
    ```



### cp 명령어(copy)

- $ cp 파일1 파일2
  - 파일 1의 복사본 파일 2를 현재 디렉터리 내에 만듦

```shell
$ cp cs1.txt cs2.txt
$ ls -l cs1.txt cs2.txt
-rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt
-rw-r--r-- 1 chang faculty 2088 4월 16일 13:45 cs2.txt
```

-  cp 파일 디렉터리

  - 파일 1의 복사본을 디렉터리 내에 만듦

  - ```shell
    $ cp cs1.txt /tmp
    ```

- mv(move)

  - 파일 1의 이름을 파일 2로 변경한다.

  - $ mv  파일1 파일2

  - ```shell
    $ mv cs2.txt cs3.txt
    $ ls -l
    -rw-r--r-- 1 chang faculty 2088 4월 16일 13:37 cs1.txt
    -rw-r--r-- 1 chang faculty 2088 4월 16일 13:56 cs3.txt
    ```

- 파일을 디렉터리 내로 이동

  - $ mv 파일 디렉터리

  - ```shell
    $ mv cs3.txt /tmp
    ```

- rm(remove) 명령어

  - 명령줄 인수로 받은 파일(들)을 지운다.

  - $ rm 파일+

  - ```shell
    $ rm cs1.txt
    ```

- $ rm -r 디렉터리

  - 디렉터리 내의 모든 파일 및 하위 디렉터리들을 단번에 지운다.

- rmdir(remove dircectory) 명령어

  - 명령줄 인수로 받은 디렉터리(들)을 지운다.

  - $ rmdir 디렉터리+

  - 주의: 디렉터리 내에 아무 것도 없어야 한다.

  - ```shell
    $ rmdir test
    ```

<br>

**정리**

| 명령어           | 의미                           |
| ---------------- | ------------------------------ |
| cat 파일*        | 파일 디스플레이                |
| more 파일+       | 한 번에 한 페이지씩 디스플레이 |
| head 파일*       | 파일의 앞부분 디스플레이       |
| tail 파일*       | 파일의 뒷부분 디스플레이       |
| wc 파일*         | 줄/단어/문자 수 세기           |
| cp 파일1 파일2   | 파일1을 파일2로 복사           |
| mv 파일1 파일2   | 파일1을 파일2로 이름 변경      |
| rm 파일+         | 파일 삭제                      |
| rmdir 디렉터리+  | 디렉터리 삭제                  |
| grep 키워드 파일 | 파일에서 키워드 찾기           |

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

- 파일의 사용권한은 소유자(owner)/그룹(group)/기타(others)로 구분하여 관리한다.
- 예
  - 소유자 그룹 기타
  -    rw-     r--     r--



### chmod(change mode)

- 파일 혹은 디렉터리의 사용권한을 변경하는 명령어
  - $ chmod [-R] 사용권한 파일
  - -R 옵션은 디렉터리 내의 모든 파일, 하위 디렉터리에 대해서도 적용
- ![image](https://user-images.githubusercontent.com/79521972/157796858-c138ffc0-bdfb-4b3e-ab99-8b711087dae2.png)



### chown(change owner)/ chgrp(change group)

- chown 명령어
  - 파일이나 디렉터리의 소유자를 변경할 때 사용한다.
  - $ chown 사용자 파일
  - $ chown [-R] 사용자 디렉터리
- chgrp 명령어
  - 파일의 그룹을 변경할 수 있다.
  - $ chgrp 그룹 파일
  - $ chgrp [-R] 그룹 디렉터리
- 파일의 소유자 또한 슈퍼 유저(superuser)만이 사용 가능!



## 2.4 입출력 재지정 및 파이프

### 출력 재지정(output redirection)

- 명령어의 표준 출력 내용을 모니터에 출력하는 대신에 파일에 저장

  - $ 명령어 > 파일

  - ```shell
    $ who > names.txt
    ```

  - ![image](https://user-images.githubusercontent.com/79521972/157797328-13b40f4c-39ab-492e-8714-8dea1c08a2cb.png)

- 출력 재지정 예

  - ```shell
    $ cat < list1.txt
    Hi!
    This is the first list.
    ^D
    ```

  - ```shell
    $ cat list2.txt
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

<br>

### 출력 추가(append)

- 명령어의 표준 출력을 모니터 대신에 기존 파일에 추가

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

- 명령어의 표준 입력을 키보드 대신에 파일에서 받는다.

  - $ 명령어 < 파일

  - ```shell
    $ wc < list1.txt
    4 17 71 list1.txt
    ```

<br>

### 문서 내 입력(here document)

- < : 지정된 파일로부터 입력을 받음
- \> : 지정된 파일로 출력
- \>\> 출력을 지정된 파일에 추가
- << : 실제 파일은 아니지만, 시스템에 입력되는 파일처럼 보이는 입력 라인



- 명령어의 표준 입력을 << 다음의 단어가 다시 나타날 때까지의 내용으로 사용

- 명령어를 실행할 때 문서(script) 내에서 입력을 줄 때 사용

  - ```
    $ 명령어 << 단어(delimeter 단어)
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

<br>

### 파이프

- 로그인 된 사용자들을 ==정렬==해서 보여주기

```shell
$ who > names.txt
$ sort < names.txt
```

- $ 명령어1 | 명령어2
  - 명령어1의 표준 출력을 명령어2의 표준입력으로 바로 받는다.

```shell
$ who | sort
```

-  첫 번째 방식과의 차이점은 위와 같이 작성하면 names.txt가 생성되지 않는 다는 점이다.



![image](https://user-images.githubusercontent.com/79521972/157798125-1df4be71-8192-4ba0-8ccf-0bd07ba2abcf.png)

<br>

## 2.5 후면 처리 및 프로세스

### 전면 처리 vs. 후면처리

- 전면 처리

  - 명령어를 입력하면 명령어가 전면에서 실행되며 명령어 실행이 끝날 때까지 쉘이 기다려 준다.
  - 명령어는 키보드와 모니터로 입출력
  - 한 명령어만 실행 가능, 실행중인 명령어는 Ctrl-C 입력 시 강제 공료, Ctrl-Z 입력 시 실행 중단

  - 중단 된 명령어는 prompt에서 fg 명령어 입력 시 실행이 계속 됨

- 후면 처리

  - 명령어들을 후면에서 처리하고 전면에서는 다른 작업을 할 수 있으면 동시에 여러 작업을 수행할 수 있다.
  - $ 명령어 &



<br>

### 후면 처리 예

fg(foreground)

```shell
$ (sleep 100; echo done) &
[1] 8320
$ find . -name test.c -print &
[2] 8325
$ jobs // 후면에서 실행되고 있는 작업들을 리스트
[1] + Running ( sleep 100; echo done )
[2] - Running find . -name test.c –print
```

- $ fg %작업 번호

```shell
$ fg %1
( sleep 100; echo done )
```

- 후면 처리 입출력

```shell
$ find . -name test.c -print > find.txt & // 출력 뒤섞임 방지
$ find . -name test.c -print | mail chang &
$ wc < inputfile & // 키보드 입력 불가능, 파일로부터 받아야 함
```



<br>

### 프로세스(process)

- 실행중인 프로그램을 프로세스(process)라고 부른다.

- 각 프로세스는 유일한 프로세스 번호 PID를 갖는다.

- ps 명령어를 사용하여 나의 프로세스들을 볼 수 있다.

  - ```shell
    PID TTY TIME CMD
    8695 pts/3 00:00:00 csh
    8720 pts/3 00:00:00 ps
    ```

  - ```shell
    $ ps u //자세한 정보 출력
    USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
    chang 8695 0.0 0.0 5252 1728 pts/3 Ss 11:12 0:00 -csh
    chang 8793 0.0 0.0 4252 940 pts/3 R+ 11:15 0:00 ps u
    ```

- ps aux

  - ```shell
    $ ps aux //시스템 내의 모든 프로세스 정보 출력
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

- 프로세서를 강제적으로 종료시키는 명령어

  - $ kill 프로세스ㅂ 번호
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





## 2장 정리

- 유닉스의 디렉터리는 루트로부터 시작하여 계층 구조를 이룬다.
- 절대 경로명은 루트 디렉터리부터 시작하고 상대 경로명은 현재 디렉터리부터 시작한다.
- 파일의 사용권한은 소유자, 그룹, 기타로 구분하여 관리한다.
- 출력 재지정은 표준 출력 내용을 파일에 저장하고 입력 재지정은 표준 입력을 파일에서 받는다.
- 실행 중인 프로그램을 프로세스라고 한다.

































