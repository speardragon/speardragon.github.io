---
layout: single
title: "[System Programming] 4장. 파일 입출력"
categories: ['System', 'System Programming']
tag: ['System Programming', 'Unix']
---





# 4.1 시스템 호출

## 컴퓨터 시스템 구조

- 유닉스 커널(kernel)
  - 하드웨어 위에 탑재된 소프트웨어(kernel)
  - **하드웨어를 운영 관리(efficiency)**하여 다음과 같은 **서비스를 응용 프로그램에게 제공(convinient)**
  - 파일관리 (File management)
  - 프로세스 관리(Memory mangement)
  - 통신 관리(Communication management)
  - 주변 장치 관리(Device management)
  - 응용 프로그램에게 서비스 제공

![image](https://user-images.githubusercontent.com/79521972/160031305-ed2c70cf-47d6-4d3d-a36f-f8c0a8ce7324.png)



<br>

## 저수준 파일 입출력과 고수준 파일 입출력

- 저수준 파일 입출력
  - 유닉스 커널의 시스템 호출을 사용하여 파일 입출력을 실행하며, 특수 파일도 읽고 쓸 수 있다.
  - int fd = open (const char *path, int oflag, [ mode_t mode ]);
  
- 고수준 파일 입출력
  - **표준 입출력 라이브러리**로 다양한 형태의 파일 입출력 함수를 제공한다. (유닉스 커널의 시스템 호출을 사용하지 않음)
  - FILE *fopen(const char *name, const char *mode)




<br>

## 시스템 호출

- 시스템 호출은 커널에 서비스 요청을 위한 프로그래밍 인터페이스
- 응용 프로그램은 시스템 호출을 통해서 커널에 서비스를 요청한다.

![image](https://user-images.githubusercontent.com/79521972/160031553-e2711aa0-15d5-42cc-995b-d8f66f1ab96a.png)

응용 프로그램은 커널에게 서비스 요청을 직접적으로 할 수 있다. 라이브러리 함수를 호출하게 되면 시스템 호출을 이 라이브러리에서  대행을 해 준다. 결과적으로는 같지만 응용프로그램이 시스템 호출을 직접적으로 하냐 간접적으로 하냐에 따라 달라진다.

<br>

## 시스템 호출과 라이브러리 함수의 비교

- 시스템 호출: 커널의 해당 서비스 모듈을 직접 호출하여 작업하고 결과를 리턴
- 라이브러리 함수: 일반적으로 커널 모듈을 직접 호출안함

![image](https://user-images.githubusercontent.com/79521972/160031821-9eef6823-45fd-4fce-9a1a-9aa2200bff20.png)

- 응용프로그램이 직접 시스템호출을 하는 경우 영역변환에 필요한 준비를 시스템 호출 코드가 실행하는 것이다.

- 반면 응용프로그램이 라이브러리를 호출하는 경우 main함수에서 특정 라이브러리 함수를 통해 시스템 호출 코드를 실행시키는 경우 해당 함수 안에 시스템 호출 코드가 있는 것이기 때문에 시스템 호출이 종료 되고 라이브러리 함수로 돌아와 라이브러리 함수가 종료되면 main함수로 돌아오는 것이 과정이라면
- main함수에서 직접 시스템 호출 코드를 실행하는 경우 사용자 영역에서 커널영역으로 변환후 커널이 종료되면 다시 사용자 영역으로 돌아온다.

 

<br>

#### 시스템 호출 과정

![image](https://user-images.githubusercontent.com/79521972/160032207-fb3f91fe-d06c-489b-bf54-64497d052afa.png)

fd: file descriptor의 약자

<br>

### 시스템 호출 요약

![image](https://user-images.githubusercontent.com/79521972/160034362-fc9b0e08-9005-4bd5-b063-428911592116.png)



<br>

# 4.2 파일

## 이 장의 기본 내용

- Process가 file을 사용하려면?
  - File system에서 file의 위치를 찾는다. -> open()
  - File의 data를 읽거나 쓴다. -> read()/write()
  - File의 사용을 마친다. -> close()

![image-20220325100248687](C:\Users\c_dragon\AppData\Roaming\Typora\typora-user-images\image-20220325100248687.png)



 <br>

## 유닉스에서 파일

- 연속된 바이트의 나열
- 특별한 다른 포맷을 정하지 않음
  - 어디서부터 어디까지는 integer만 들어갈 수 있고... 이런 것들이 존재하지 않는다는 것

- 디스크 파일뿐만 아니라 외부 장치에 대한 인터페이스
  - 외부 장치도 special file로 간주하여 byte sequnece가 컴퓨터로 읽혀 들어온다.


![image](https://user-images.githubusercontent.com/79521972/160037209-0f7937a7-4ea5-4421-a74a-5acd0ff63aa9.png)

<br>

## 파일 열기:open()

- 파일을 사용하기 위해서는 먼저 open() 시스템 호출을 이용하여 파일을 열어야 한다.

- ```c
  #include <sys/types.h> /defines Various data types used elsewhere
  #inlcude <sys/stat.h>  / File information (stat et al.)
  #include <fcntl.h>     / File opening, locking and other operations
  int open (const char *path, int oflag, [mode_t mode]);
  ```

  - 몇 가지 헤더파일이 포함되어야 함

- 파일 열기에 성공하면 파일 디스크립터를 반환, 실패하면 -1을 반환한다.

- 파일 디스크립터는 **열린 파일**을 나타내는 번호이다.

- mode: 파일의 access permission 값, 새로운 파일을 만드는 경우에만 사용됨

<br>



- #### Oflag

  - Access mode (<mark>One of three constants must be specified.</mark>)
    - O_RDONLY
      - 읽기 모드, read() 호출은 사용 가능
    - O_WRONLY
      - 쓰기 모드, write() 호출은 사용 가능
    - O_RDWR
      - 읽기/쓰기 모드, read(), write() 호출 사용 가능
  - The followings are optional.
    - O_APPEND
      - 데이터를 쓰면 파일 끝에 이어서 첨부된다.
    - O_CREAT
      - 해당 파일이 없는 경우에 새로 생성하며 mode는 생성할 파일의 사용권한을 나타낸다.

- oflag

  - O_TRUNC
    - 파일이 이미 있는 경우 내용을 지운다.(새로운 파일을 여는 것과 같은 행위)
  - O_EXCL
    - O_CREAT와 함께 사용되며 해당 파일이 이미 있으면 오류를 반환
  - O_NONBLOCK
    - 넌블로킹 모드로 입출력 하도록 한다.
  - O_SYNC
    - write() 시스템을 호출을 하면 디스크에 물리적으로 쓴 후 반환된다.
    - Any writes on the resulting file descriptor will block the calling process until the data has been physically written to the underlying hardware.

  - nonblock은 디스크에  물리적으로 다 쓰지 않아도 반환이 되지만 sync는 반드시 다 쓴 후에 반환된다.

    

<br>

## 파일 열기: 예

```c
fd = open("account",O_RDONLY);
fd = open(argv[1], O_RDWR);
fd = open(argv[1], O_RDWR | O_CREAT, 0600);
fd = open("tmpfile", O_WRONLY|O_CREAT|O_TRUNC, 0600);
fd = open("/sys/log", O_WRONLY|O_APPEND|O_CREAT, 0600);
if ((fd = open("tmpfile", O_WRONLY|O_CREAT|O_EXCL, 0666))==-1)
```



## fopen.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, char *argv[])
{
     int fd;
     if ((fd = open(argv[1], O_RDWR)) == -1)
     	printf("파일 열기 오류\n");
     else printf("파일 %s 열기 성공 : %d\n", argv[1], fd);
     
     close(fd);
     exit(0);
}
```



<br>

## File Descriptor

- 현재 열려있는 파일을 구분하는 정수값
- 저수준 파일 입출력에서 열린 파일을 참조하는데 사용

![image](https://user-images.githubusercontent.com/79521972/160038332-1cebe98f-bbb8-46b9-b0d1-482571f1fafc.png)

- 처음으로 부여 받는 file descriptor는 3번

<br>

- file descriptor
  - all open files are referred to by file descriptors. (not path name)
  - how to obtain file descriptor
    - return value of open(), creat()
  - when we want to read or write a file, 
    - we identify the file with the file descriptor
  - file descriptor is the <span style="color:blue">index of user file descriptor table</span>(배열의 인덱스)
  - STDIN_FILENO(0), STDOUT_FILENO(1), STDERR_FILENO(2)
    - defined in <unistd.h\>
  - Ranged of file descriptor
    - 0 ~ OPEN_MAX (63 in many systems)

<br>

## 파일 생성: creat()

- creat() 시스템 호출
  - path가 나타내는 파일을 생성하고 쓰기 전용으로 연다.
  - 생성된 파일의 사용권한은 mode로 정한다.
  - 기존 파일이 있는 경우에는 그 내용을 삭제하고 연다.
  - 다음 시스템 호출과 동일
    - open(path, WRONLY | O_CREAT | O_TRUNC, mode);
  - Note that the file is opend **only for writing.**

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int creat (const char *path, mode_t mode);
```

파일 생성에 성공하면 파일 디스크립터를, 실패하면 -1을 리턴



- 다음과 같이 사용자 영역에서 구현 가능함에도 creat()는 시스템 콜로 존재 

- 최근 glibc에서도 제공

  - ```c
    int creat (const char *path, mode_t mode){
        return open(path, WRONLY | O_CREAT | O_TRUNC, mode);
    }
    ```

- The GNU C Library is the GNU Project's implementation of the C standard library.



<br>

## 파일 닫기: close()

- close() 시스템 호출은 fd가 나타내는 파일을 닫는다.
- When a process termiantes, all of its open files are closed automatically by the kernel.
  - Many program often do not explicily close open files.
- On Unix-like systems, the interface defined by unistd.h is typically made up largely of system call wrapper functions such as fork, pipe and I/O primitives (read, write, close, etc.).
- unistd.h is the header file that provides access to the POSIX OS API

```c
#include <unistd.h>
int close(int fd);
```

- fd가 나타내는 파일을 닫는다.
- 성공하면 0, 실패하면 -1을 리턴한다.

<br>

## 데이터 읽기: read()

- read()  시스템 호출
  - read up to nbytes from file (fd) into the buffer starting at *buf*
    - read() starts at the file's **current offset**.
    - Before a successful return, the offset is incremented by the number of bytes actually read.

```c
#include <unistd.h>
ssize_t read (int fd, void *buf, size_t nbytes);
```

- 파일 읽기에 성공하면 읽은 바이트 수, 파일 끝을 만나면 0
- 실패하면 -1을 리턴
- (size_t: unsigned integer)



<br>

## fsize.c

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#define BUFSIZE 512

/* 파일 크기를 계산 한다.*/
int main (int argc, char *argc[])
{
    char buffer[BUFSIZE];
    int fd;
    ssize_t nread;
    long total = 0;
    if (fd = open(argc[1], O_RDONLY)) == -1)
        perror(argc[1]);
    
     /* 파일의 끝에 도달할 때까지 반복해서 읽으면서 파일 크기 계산 */
     while((nread = read(fd, buffer, BUFSIZE)) > 0)
         total += nread;
    close(fd);
    printf ("%s 파일 크기 : %ld 바이트 \n", argv[1], total);
    exit(0);
    }

}
```



<br>

## read()

- The number of bytes actually read may be less than the amount requested.
  - If **the end of regular file** is reached before the requested number of bytes has been read.
  - When reading from a terminal device, up to one line is read at a time.
    - 엔터 키를 치면 다음 라인으로 넘어간다.
  - When reading from a network, **buffering** within the network may cause less than the requested amount to be returned.
  - When reading from a **pipe**, if the pipe contains **fewer bytes** than requested, read will return only what is available.

<br>

## 데이터 쓰기: write()

- write() 시스템 호출
  - writes up to nbytes to the file referenced by filedes from the buffer starting at buf
  - write start at the <span style="color:blue">file’s current offset.</span>
  - if O_APPEND was specified when the file was opened
    - The file's offset is set to the end of file before write

```c
#include <unistd.h>
ssize_t write (int fd, void *buf, size_t nbytes);
```

- 파일에 쓰기를 성공하면 실제 쓰여진 바이트 수를 리턴하고, 실패하면 -1을 리턴



<br>

## copy.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
/* 파일 복사 프로그램 */
main (int argc, char *argc[])
{
     int fd1, fd2, n;
     char buf[BUFSIZ];
     if (argc != 3) {
     	fprintf(stderr,"사용법: %s file1 file2\n", argv[0]);
     exit(1); 
     }
     if ((fd1 = open(argv[1], O_RDONLY)) == -1) {
         perror(argv[1]);
         exit(2);
     }
     if ((fd2 =open(argv[2], O_WRONLY | O_CREAT|O_TRUNC 0644)) == -1) {
         perror(argv[2]);
         exit(3);
     }
     while ((n = read(fd1, buf, BUFSIZ)) > 0)
         write(fd2, buf, n); // 읽은 내용을 쓴다.
         exit(0);
    }
}
```



<br>

## 파일 디스크립터 복제

- dup()/dup2() 호출은 기존의 파일 디스크립터를 복제한다.

```c
#include <unistd.h>
int dup(int oldfd);
//oldfd에 대한 복제본인 새로운 파일 디스크립터를 생성하여 반환한다.
//실패하면 –1을 반환한다.

int dup2(int oldfd, int newfd);
//oldfd을 newfd에 복제하고 복제된 새로운 파일 디스크립터를 반환하고 실패하면 -1을 반환한다.
```



![image](https://user-images.githubusercontent.com/79521972/160048835-3ebc0fba-5591-40f4-be32-2e5194e0f1b7.png)

<br>

## dup() and dup()

- 실제로는 dup()가 oldfd를 반환해 주는 것이 아니라 kernel 안에서 kernal data structure에 대한 수정이 일어난다.

- Kernel data structures after "dup(1)"
  - The next available descriptor is 3.

![image](https://user-images.githubusercontent.com/79521972/160048908-a84b4359-ab19-4a2e-8cf5-ba7fbe7e80db.png)

- Example 

  - ```c
    #include <unistd.h>
    #include <fcntl.h>
    int main(void)
    {
    	int fd;
        fd = creat("dup_result", 0644);
        dup2(fd, STDOUT_FILENO);
        close(fd);
        printf("hello world\n");
        return 0;
    }
    ```

![image](https://user-images.githubusercontent.com/79521972/160264508-f23f7616-14bc-4c15-8fc4-0232f2217165.png)

<br>

- 실행

  - ```assembly
    $ cat dup_result
    hello world
    ```

파일을 STDOUT table에 복제를 하여 표준 출력을 사용할 수 없게 되고 코드의 결과인 printf가 실행되지 못할 것이다. 그래서 만약 출력을 하고 싶으면 cat 명령어를 사용해야 한다.

<br>

## dup.c

```c
#include <unistd.h>
 #include <fcntl.h>
 #include <stdlib.h>
 #include <stdio.h>
int main()
{
    int fd1, fd2;

    if((fd1 = creat("myfile", 0600)) == -1)
    	perror("myfile");

    write(fd1, "Hello! Linux", 12);
    fd2 = dup(fd1);
    write(fd2, "Bye! Linux", 10);
    exit(0);
}
```

```
$ dup
$ cat myfile
Hello! LinuxBye! Linux
```

fd1과 fd1을 duplicate한 fd2는 같은 파일을 가리키기 때문에 두 파일이 이어져서 작성이 되었다.

<br>

## sync(), fsync(), and fdatasync()

- Delayed write
  - When write data to a file, the data is copied into buffers.
    - file에 데이터가 write 될 때 file에 쓰여지는 것이 아니라 buffer에 copy되는 것이다.
  - The data is physically written to disk at some later time.
  - 버퍼를 사용하는 이유 -> 동일한 데이터에 대한 연속적인 read/write시 성능 향상.
- When the delayed-write blocks are written to disk?
  - Buffer is filled with the delayed-write blocks or 
  - Periodically by update **daemon** (usually every 30 seconds)

```c
#include <unistd.h>

int fsync(int filedes);
int fdatasync(int filedes);

void sync(void);
 
//Returns: 0 if OK, -1 on error
```

- sync
  - Write **all** the modified buffer blocks to disk.
- fsync
  - Write only the modified (**data + attribue**) buffer blocks of a single file.
- fdatasync
  - Write **only** the modifies **data** buffer blocks of a single file.

<br>

## fcntl()

```c
#include <fcntl.h>

int fcntl(int filedes, int cmd, .../*int arg*/); // filedes:파일디스크립터, cmd: 커맨드
//Return: depends on cmd if OK (see following), -1 on error
```

- Change the properties of a file that is already open
  - Duplicate an existing desciptor (cmd = F_DUPFD)
  - Get/set file descriptor flags (cmd = F_GETFD or F_SETFD)
  - Get/set file status flags (cmd = F_GETFL or F_SETFL)
  - Get/set asynchronous I/O ownership (cmd = F_GETOWN or F_SETOWN)
  - Get/set record locks (cmd = F_GETLK, F_SETLK, or F_SETLKW)

- example

```c
/* include header files 생략 */
int main()
{
     int mode, fd, value;
    
     fd = open("test.sh", O_RDONLY|O_CREAT);
     value = fcntl(fd, F_GETFL, 0);
    
     mode = value & O_ACCMODE;
     if (mode == O_RDONLY)
     	printf("O_RDONLY setting\n");
     else if (mode == O_WRONLY)
     	printf("O_WRONLY setting\n");
     else if (mode == O_RDWR)
     	printf("O_RDWR setting\n");
}
```



- 실행

```
# ./fgetfl_test
O_RDONLY setting
$ 
```



- Macro: int **O_ACCMODE**
  - This macro stands for **a mask** that can be bitwise-ANDed with **the file status flag** value to produce a value representing the file access mode. The mode will be O_RDONLY, O_WRONLY, or O_RDWR.



<br>

# 4.3 임의 접근 파일



## 파일 위치 포인터(file position pointer)

- 파일 위치 포인터는 파일 내에 읽거나 쓸 위치인 현재 파일 위치(currnet file position)를 가리킨다.

![image](https://user-images.githubusercontent.com/79521972/160050554-ecab5614-966a-4c0b-958c-378b86764516.png)

<br>

## 파일 위치 포인터 이동: lseek()

- lseek() 시스템 호출
  - **임의의 위치로** 파일 위치 포인터를 이동시킬 수 있다.
  - The offset for regular files must be non-negative

```c
#include <unistd.h>
off_t lseek (int fd, off_t offset, int whence ); //whence:기준점
//이동에 성공하면 현재 위치(measured in bytes from the beginning of the file)를 리턴하고 실패하면 -1을 리턴한다.
```

off_t: 리눅스나 c에서 사용하는 데이터 타입

![image](https://user-images.githubusercontent.com/79521972/160050692-1709068a-0b5c-49c5-9e2b-d6b522c1c0d8.png)





<br>

lseek()

- whence
  - SEEK_SET
    - THe offset is set to offset bytes from **the beginning of the file.**
  - SEEK_CUR
    - The offset is set to **its current location** plus offset bytes.
  - SEEK_END
    - The offset is set to **the size of the file** plus offset bytes.(파일의 마지막)



<br>

## 파일 위치 포인터이동: 예

- 파일 위치 이동
  - lseek(fd, 0L, SEEK_SET);  // 파일 시작으로 이동(rewind)
  - lseek(fd, 100L, SEEK_SET); // 파일 시작에서 100바이트 위치로
  - lseek(fd, 0L, SEEK_END); //파일 끝으로 이동(append 하기 위한 행동)
  - lseek(fd, 100L, SEEK_CUR); //현재위치에서 100바이트 이동



- 레코드 단위로 이동 (레코드 구조체의 size만큼 이동한다.)
  - lseek(fd, n * sizeof(record), SEEK_SET); //n+1번째 레코드 시작위치로
  - lseek(fd, sizeof(record), SEEK_CUR); //다음 레코드 시작위치로
  - lseek(fd, -sizeof(record), SEEK_CUR); // 전 레코드 시작위치로



- 파일 끝 이후로 이동
  - lseek(fd, sizeof(record), SEEK_END);  //파일 끝에서 한 레코드 다음 위치로



### lseek()

- hole
  - The file's offset can be greater than the file's size.
    - Next write to the file will extend the file.
  - It means that a **hole** in file is created and is allowed.
  - read from the data in hole **returns 0**.

![image](https://user-images.githubusercontent.com/79521972/160264881-a319ead0-778c-429b-96b0-414dafdb177d.png)



- **example**

- ```c
  #include "apue.h"
  #include <fcntl.h>
  
  char buf1[] = "abcdefghij";
  char buf2[] = "ABCDEFGHIJ";
  
  int main(void)
  {
      int fd;
      if ((fd = creat("file.hole", FILE_MODE)) < 0)
     		err_sys("creat error");
      /* FILE_MODE is defined as 644 in “apue.h”. */
      
      if (write(fd, buf1, 10) != 10)
      	err_sys("buf1 write error");
      /* offset now = 10 */
      
      if (lseek(fd, 16384, SEEK_SET) == -1)
      	err_sys("lseek error");
      /* offset now = 16384 */
      
      if (write(fd, buf2, 10) != 10)
      	err_sys("buf2 write error");
      /* offset now = 16394 */
      
      exit(0);
  }
  ```

- 실행 예

- ```
  $ ./a.out
  $ ls -l file.hole 	check its size
  -rw-r--r-- 1 sar	16394 Nov 25 01:01 file.hole
  $ od -c file.hole	let's look at the actual contents
  0000000 a b c d e f g h i j \0 \0 \0 \0 \0 \0
  0000020 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0 \0
  *
  0040000 A B C D E F G H I J  //16384
  0040012						//16394
  ```

- > od utility: dump files in octal.(8진수로 file을 dump)
  >
  > ​              -c: print the contents as characters.

<br>



- <mark>Are the disk blocks allocated for hole?</mark>

- ```
  $ ls -ls file.hole file.nohole
   	8 -rw-r--r-- 1 sar 			16394 Nov 25 01:01 file.hole
   	20 -rw-r--r-- 1 sar 		16394 Nov 25 01:03 file.nohole
  ```

- Compare the sizes of file.hole and file.nohole

  - file.hole: with hole
    - 8 blocks are allocated.
    - so, 하드디스크의 공간을 차지하지 않는다.
  - file.nohole: afile of the same size, but without holes.
    - 20 blocks are allocated.

> ls utility
>
> -s: with -l, print size of each file, in blocks.



<br>

## 레코드 저장 예

```
write(fd, &record1, sizeof(record));
write(fd, &record2, sizeof(record));
lseek(fd, sizeof(record), SEEK_END);
write(fd, &record3, sizeof(record));
```

![image](https://user-images.githubusercontent.com/79521972/160264999-adc81c14-0c9e-48ef-b0f1-914ad46162d5.png)

<br>

## dbcreate.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include "student.h"
/* 학생 정보를 입력받아 데이터베이스 파일에 저장한다. */
int main(int argc, char *argv[])
{
    int fd;
    struct student record;
    if (argc < 2) {
    	fprintf(stderr, "사용법 : %s file\n", argv[0]);
    	exit(1);
    }
    
    if ((fd = open(argv[1], O_WRONLY|O_CREAT|O_EXCL, 0640)) == -1) {
    	perror(argv[1]);
    	exit(2);
    }
    printf("%-9s %-8s %-4s\n", "학번", "이름", "점수");
    while (scanf("%d %s %d", &record.id, record.name, &record.score) == 3) {
        lseek(fd, (record.id - START_ID) * sizeof(record), SEEK_SET);
        write(fd, (char *) &record, sizeof(record) );
    }
    close(fd);
    exit(0);
}
```



## student.h

```c
#define MAX 24
#define START_ID 1401001
struct student {
 char name[MAX];
 int id;
 int score;
};

```



## dbquery.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include "student.h"
/* 학번을 입력받아 해당 학생의 레코드를 파일에서 읽어 출력한다. */
int main(int argc, char *argv[])
{
    int fd, id;
    struct student record;
    if (argc < 2) {
        fprintf(stderr, "사용법 : %s file\n", argv[0]);
        exit(1);
    }
    if ((fd = open(argv[1], O_RDONLY)) == -1) {
        perror(argv[1]);
        exit(2);
    }
    do {
        printf("\n검색할 학생의 학번 입력:");
        if (scanf("%d", &id) == 1) {
            lseek(fd, (id-START_ID)*sizeof(record), SEEK_SET);
            if ((read(fd, (char *) &record, sizeof(record)) > 0) && (record.id != 0))
                printf("이름:%s\t 학번:%d\t 점수:%d\n", record.name, record.id, 			 record.score);
            else printf("레코드 %d 없음\n", id);
        } else printf(“입력 오류”);
        printf("계속하겠습니까?(Y/N)");
        scanf(" %c", &c);
    } while (c=='Y');
    close(fd);
    exit(0);
}
```





## 레코드 수정 과정

1. 파일로부터 해당 레코드를 읽어서
2. 이 레코드를 수정한 후에
3. 수정된 레코드를 다시 파일 내의 원래 위치에 써야 한다.



## 레코드 수정

![image](https://user-images.githubusercontent.com/79521972/160265080-abf6cdea-96d0-4a5e-b4e1-61ef296269b9.png)

<br>

## dbupdate.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include "student.h"
/* 학번을 입력받아 해당 학생 레코드를 수정한다. */
int main(int argc, char *argv[])
{
    int fd, id;
    char c;
    struct student record;
    if (argc < 2) {
        fprintf(stderr, "사용법 : %s file\n", argv[0]);
        exit(1);
    }
    if ((fd = open(argv[1], O_RDWR)) == -1) {
        perror(argv[1]);
        exit(2);
    }
    do {
        printf("수정할 학생의 학번 입력: ");
        if (scanf("%d", &id) == 1) {
            lseek(fd, (long) (id-START_ID)*sizeof(record), SEEK_SET);
            if ((read(fd, (char *) &record, sizeof(record)) > 0) && (record.id != 0)) {
                printf("학번:%8d\t 이름:%4s\t 점수:%4d\n",
                       record.id, record.name, record.score);
                printf("새로운 점수: ");
                scanf("%d", &record.score);
                lseek(fd, (long) -sizeof(record), SEEK_CUR); //원래 위치로 돌아감
                write(fd, (char *) &record, sizeof(record));
            } else printf("레코드 %d 없음\n", id);
        } else printf("입력오류\n");
        printf("계속하겠습니까?(Y/N)");
        scanf(" %c",&c);
    } while (c == 'Y');
    close(fd);
    exit(0);
}

```



<br>

## 핵심 개념

- 시스템 호출은 커널에 서비스를 요청하기 위한 프로그래밍 인터페이스로 응용 프로그램은 시스템 호출을 통해서 커널에 서비스를 요청할 수 있다. 
- 파일 디스크립터는 열린 파일을 나타낸다. 
- open() 시스템 호출을 파일을 열고 열린 파일의 파일 디스크립터를 반환한다. 
- read() 시스템 호출은 지정된 파일에서 원하는 만큼의 데이터를 읽고 write() 시스템 호출은 지정된 파일에 원하는 만큼의 데이터를 쓴다. 
- 파일 위치 포인터는 파일 내에 읽거나 쓸 위치인 현재 파일 위 치를 가리킨다. 
- lseek() 시스템 호출은 지정된 파일의 현재 파일 위치를 원하는 위치로 이동시킨다















