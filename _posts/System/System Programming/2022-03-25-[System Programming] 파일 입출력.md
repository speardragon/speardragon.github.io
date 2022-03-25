---
layout: single
title: "[System Programming] 파일 입출력"
categories: ['System', 'System Programming']
tag: ['System Programming', 'Unix']
---





# 4.1 시스템 호출

## 컴퓨터 시스템 구조

- 유닉스 커널(kernel)
  - 하드웨어 위에 탑재된 소프트웨어(kernel)
  - 하드웨어를 운영 관리하여 다음과 같은 서비스를 제공
  - 파일관리 (File management)
  - 프로세스 관리(Memory mangement)
  - 통신 관리(Communication management)
  - 주변 장치 관리(Device management)

![image](https://user-images.githubusercontent.com/79521972/160031305-ed2c70cf-47d6-4d3d-a36f-f8c0a8ce7324.png)



<br>

## 저수준 파일 입출력과 고수준 파일 입출력

- 저수준 파일 입출력: 유닉스 커널의 시스템 호출을 사용하여 파일 입출력을 실행하며, 특수 파일도 읽고 쓸 수 있다.
  - int fd = open (const char *path, int oflag, [ mode_t mode ]);
- 고수준 파일 입출력: 표준 입출력 라이브러리로 다양한 형태의 파일 입출력 함수를 제공한다. (유닉스 커널의 시스템 호출을 사용하지 않음)
  - FILE *fopen(const char *name, const char *mode)



<br>

## 시스템 호출

- 시스템 호출은 커널에 서비스 요청을 위한 프로그래밍 인터페이스
- 응용 프로그램은 시스템 호출을 통해서 커널에 서비스를 요청한다.

![image](https://user-images.githubusercontent.com/79521972/160031553-e2711aa0-15d5-42cc-995b-d8f66f1ab96a.png)

응용 프로그램은 커널에게 서비스 요청을 직접적으로 할 수 있다. 라이브러리 함수를 호출하게 되면 시스템 호출을 이 라이브러리에서  대행을 해 준다.

<br>

## 시스템 호출과 라이브러리 함수의 비교

- 시스템 호출: 커널의 해당 서비스 모듈을 직접 호출하여 작업하고 결과를 리턴
- 라이브러리 함수: 일반적으로 커널 모듈을 직접 호출안함

![image](https://user-images.githubusercontent.com/79521972/160031821-9eef6823-45fd-4fce-9a1a-9aa2200bff20.png)

- 응용프로그램이 직접 시스템호출을 하는 경우 영역변환에 필요한 준비를 시스템 호출 코드가 실행하는 것이다.

- main함수에서 특정 라이브러리 함수를 통해 시스템 호출 코드를 실행시키는 경우 해당 함수 안에 시스템 호출 코드가 있는 것이기 때문에 시스템 호출이 종료 되고 라이브러리 함수로 돌아와 라이브러리 함수가 종료되면 main함수로 돌아오는 것이 과정이라면
- main함수에서 직접 시스템 호출 코드를 실행하는 경우 사용자 영역에서 커널영역으로 변환후 커널이 종료되면 다시 사용자 영역으로 돌아온다.

 

<br>

#### 시스템 호출 과정

![image](https://user-images.githubusercontent.com/79521972/160032207-fb3f91fe-d06c-489b-bf54-64497d052afa.png)



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
- 디스크 파일뿐만 아니라 외부 장치에 대한 인터페이스

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





- #### Oflag

  - Access mode (One of three constants must be specified.)
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
    - 파일이 이미 있는 경우 내용을 지운다.
  - O_EXCL
    - O_CREAT와 함께 사용되며 해당 파일이 이미 있으면 오류
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

<br>

- file descriptor
  - all open files are referred to by file descriptors.
  - how to obtain file descriptor
    - return value of open(), create()
  - when we want to read or write a file, 
    - we identify the file with the file descriptor
  - file descriptor is the <span style="color:blue">index of user file descriptor table</span>
  - STDIN_FILENO(0), STDOUT_FILENO(1), STDERR_FILENO(2)
    - defined in <unistd.h\>
  - Ranged of file descriptor
    - 0 ~ OPEN_MAX (63 in many systems)

<br>

## dup() and dup()



파일을 STDOUTput table에 복제를 하여 표준 출력을 사용할 수 없게 되고 코드의 결과인 printf가 실행되지 못할 것이다. 그래서 만약 출력을 하고 싶으면 cat 명령어를 사용해야 한다.









