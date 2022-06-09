---
layout: single
title: "[System Programming] 12-1장. 파이프"
categories: ['System', 'System Programming']
tag: ['Pipe']
---

<br>

# 12.1 파이프

## 파이프 원리

- $ who | sort

![image](https://user-images.githubusercontent.com/79521972/169849580-00d96551-e817-464c-8564-216a23f2ef7c.png)

- 파이프
  - 물을 보내는 수도 파이프와 비슷 
  - 한 프로세스는 쓰기용 파일 디스크립터를 이용하여 파이프에 데이터를 보내고(쓰고) 
  - 다른 프로세스는 읽기용 파일 디스크립터를 이용하여 그 파이프에서 데이터를 받는다(읽는다). 
  - **한 방향(one way) 통신**
- 프로세스 주소를 통해서 전달할 방법이 없다.
- pipe size가 꽉찬 경우 알아서 속도를 조절해준다. -> synchronization

<br>

## 파이프 생성

- 파이프는 두 개의 파일 디스크립터를 갖는다. 
- 하나는 쓰기용이고 다른 하나는 읽기용이다.

```c
#include <unistd.h>
int pipe(int fd[2])
 //파이프를 생성한다. 성공하면 0을 실패하면 -1을 반환한다
```

![image](https://user-images.githubusercontent.com/79521972/169849812-8a317e43-2397-4b23-88f2-c23e3a3a474a.png)



<br>

## IPC using Pipes

Inter-Process Communication - process 간 통신

- IPC using **regular files** 
  - unrelated processes can share 
  - fixed size 
  - life-time 
  - **lack of synchronization** (매우 불편)
- IPC using **pipes** 
  - for transmitting data between related processes 
  - can transmit an unlimited amount of data 
  - automatic synchronization on open()
  - pipe size가 꽉찬 경우 알아서 속도를 조절해준다. -> synchronization
  
- Limitations of Pipes 
  - Half duplex (data flows in one direction) 
  - Can only be used between processes that have a common ancestor (**Usually used between the parent and child processes**) 
  - A child process inherits pipes of its parent process

<br>

## 파이프 사용법

1. 한 프로세스가 파이프를 생성한다.
2. 그 프로세스가 자식 프로세스를 생성한다.
3. 쓰는 프로세스는 읽기용 파이프 디스크립터(fd[0])를 닫는다. 
   읽는 프로세스는 쓰기용 파이프 디스크립터(fd[1])를 닫는다. 
4. write()와 read() 시스템 호출을 사용하여 파이프를 통해 데이터를 송수신한다.
5. 각 프로세스가 살아 있는 파이프 디스크립터를 닫는다.



- 자식 생성 후

![image](https://user-images.githubusercontent.com/79521972/169850263-e4b5f69f-cc6b-443d-ac6c-8345ed501fad.png)

- 자식에서 부모로 보내기

![image](https://user-images.githubusercontent.com/79521972/169850302-d7c6d860-c32f-48ca-b58f-004f6d22d003.png)



<br>

## pipe.c

```c
#include <unistd.h>
#define MAXLINE 100
/* 파이프를 통해 자식에서 부모로 데이터를 보내는 프로그램 (자식 to 부모)*/
int main( )
{
    int n, length, fd[2];
    int pid;
    char message[MAXLINE], line[MAXLINE];

    pipe(fd); /* 파이프 생성 */
    
    if ((pid = fork()) == 0) { /* 자식 프로세스 */
        close(fd[0]); //읽기는 필요 없으니까 close
        sprintf(message, "Hello from PID %d\n", getpid());
        length = strlen(message)+1;
        write(fd[1], message, length);
    } else { /* 부모 프로세스 */
        close(fd[1]);
        n = read(fd[0], line, MAXLINE);
        printf("[%d] %s", getpid(), line);
    }

    exit(0);
}
```

```shell
$ pipe
[3068] Hello from PID 3069
```



<br>

# 12.2 쉘 파이프 구현

## 표준 출력을 파이프로 보내기

- 자식 프로세스의 표준출력을 파이프를 통해 부모 프로세스에게 보내려면 어떻게 하여야 할까? 
  - 쓰기용 파이프 디스크립터 fd[1]을 표준출력을 나타내는 1번 파일 디스크립터에 복제 
  - dup2(fd[1],1)

![image](https://user-images.githubusercontent.com/79521972/169851121-e4d259b5-7873-467e-9c07-bb982fa088ee.png)



<br>

## stdpipe.c

```c
#include <stdio.h>
#include <unistd.h>
#define MAXLINE 100

/* 파이프를 통해 자식에서 실행되는명령어 출력을 받아 프린트 */
int main(int argc, char* argv[])
{
    int n, pid, fd[2];
    char line[MAXLINE];

    pipe(fd); /* 파이프 생성 */
    if ((pid = fork()) == 0) { //자식 프로세스
        close(fd[0]);
        dup2(fd[1],1);
        close(fd[1]);
        printf("Hello! pipe\n");
        printf("Bye! pipe\n");
    } else { // 부모 프로세스
        close(fd[1]);
        printf("자식 프로세스로부터 받은 결과\n");
        while ((n = read(fd[0], line, MAXLINE))> 0)
            write(STDOUT_FILENO, line, n);
    }

    exit(0);
}
```

```shell
$ stdpipe
자식 프로세스로부터 받은 결과
Hello! pipe
Bye! pipe
```

자식 프로세스의 결과 fd[1]이 부모 프로세스의 fd[0]로 간다.

<br>

## 명령어 표준출력을 파이프로 보내기

- 프로그램 pexec1.c는 부모 프로세스가 자식 프로세스에게 
- 명령줄 인수로 받은 명령어를 실행하게 하고 
- 그 표준출력을 파이프를 통해 받아 출력한다.



<br>

## pexec1.c

```c
#include <stdio.h>
#include <unistd.h>
#define MAXLINE 100

/* 파이프를 통해 자식에서 실행되는명령어 출력을 받아 프린트 */
int main(int argc, char* argv[])
{
    int n, pid, fd[2];
    char line[MAXLINE];

    pipe(fd); /* 파이프 생성 */
    if ((pid = fork()) == 0) { //자식 프로세스
        close(fd[0]);
        dup2(fd[1],1);
        close(fd[1]);
        execvp(argv[1], &argv[1]);
    } else { // 부모 프로세스
        close(fd[1]);
        printf("자식 프로세스로부터 받은 결과\n");
        while ((n = read(fd[0], line, MAXLINE))> 0)
            write(STDOUT_FILENO, line, n);
    }

    exit(0);
}

```

```shell
$ pexec1 date
자식 프로세스로부터 받은 결과
2012년 3월 1일 목요일 오전 11시59분44초
```

date 명령어(argv[1])를 파이프에 쓰고 이를 부모 프로세스의 fd[0]에 전달이 되면 이를 읽어서 화면에 출력한다.

이 때 fd[0]를 통해 읽는 것은 부모 프로세스가 표준 입력으로 입력 받는 것이 아니라 fd[0]으로 받기 때문이다.

- 자식 프로세스는 fd[1]로 쓰는 것이 아니라 표준 출력으로 썼음

<br>

## 쉘 파이프(앞에와 차이점을 잘 보자)

- 쉘 파이프 기능

   [shell] command1 | command2

  - 자식 프로세스가 실행하는 command1의 **표준출력**을 파이프를 통해서 부모 프로세스가 실행하는 command2의 **표준입력**으로 전달

![image](https://user-images.githubusercontent.com/79521972/169851629-d4a88e66-8157-4322-bd1f-8b87f538af61.png)



<br>

## shellpipe.c

```c
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#define READ 0
#define WRITE 1
int main(int argc, char* argv[])
{
    char str[1024];
    char *command1, *command2;
    int fd[2];
    
    printf("[shell]");
    fgets(str,sizeof(str),stdin);
    str[strlen(str)-1] ='\0';
    if(strchr(str,'|') != NULL) { // 파이프 사용하는 경우
        command1 = strtok (str,"| ");
        command2 = strtok (NULL, "| ");
    } 
    pipe(fd);
    
    if (fork() ==0) {
        close(fd[READ]);
        dup2(fd[WRITE],1); // 쓰기용 파이프를 표준출력에 복제
        close(fd[WRITE]);
        execlp(command1, command1, NULL);
        perror("pipe");
    } else {
        close(fd[WRITE]);
        dup2(fd[READ],0); // 읽기용 파이프를 표준입력에 복제
        close(fd[READ]);
        execlp(command2, command2, NULL);
            perror("pipe");
    }
}
```

```shell
$ shellpipe
[shell] ls | wc
```

ls가 자식 프로세스에서 command1으로 실행되고 wc는 부모 프로세스에서 command2로 실행된다.

wc를 표준 입력을 통해 실행 -> ls를 표준 출력을 통해 실행

<br>

# 12.3 파이프 함수

## popen()

- 자식 프로세스에게 명령어를 실행시키고 그 출력(입력)을 파이프를 통해 받는 과정을 하나의 함수로 정의

```c
#include <stdio.h> //여기에 저장되어 있는 표준 라이브러리
FILE *popen(const char *command, const char *type);
//성공하면 파이프를 위한 파일 포인터를 실패하면 NULL을 리턴한다.
int pclose(FILE *fp);
//성공하면 command 명령어의 종료 상태를, 실패하면 -1을 리턴한다. 
```

- fp = popen(command, "r")
  - 자식 프로세스가 보내는 내용을 읽기 위한 목적으로 파이프 생성
  - ![image](https://user-images.githubusercontent.com/79521972/169852162-5165fd95-7826-44de-9579-579a629ed114.png)
  
- fp = popen(command, “w");
  - 자식 프로세스에게 데이터를 보내는 목적으로 파이프를 생성
  - ![image](https://user-images.githubusercontent.com/79521972/169852224-43bd8a41-653c-44cc-b2f5-fd56d51d4e23.png)


<br>

## pexec2.c

pexec1.c 코드로부터 많이 줄어든 것을 볼 수 있을 것이다.

```c
#include <stdio.h>
#define MAXLINE 100
/* popen() 함수를 이용해 자식에서 실행되는 명령어 출력을 받아 프린트 */
int main(int argc, char* argv[])
{
    char line[MAXLINE];
    FILE *fpin;
    if ((fpin = popen(argv[1],"r")) == NULL) { // 자식 프로세스가 파이프로 보내는 내용 보기 위함
        perror("popen 오류");
        return 1;
    }
    printf("자식 프로세스로부터 받은 결과\n");
    while (fgets(line, MAXLINE, fpin))
        fputs(line, stdout);
    pclose(fpin);
    return 0;
}
```

```shell
$ pexec2 date
자식 프로세스로부터 받은 결과
2012년 3월 1일 목요일 오전 11시 59분 44초
```



<br>

# 12.4 이름 있는 파이프

지금까지 배웠던 파이프들은 unnamed pipe, 즉 이름이 없는 파이프였다.

- pipe(fd);

그 전에 파이프는 자식 프로세스가 부모 프로세스의 fd를 그대로 복제하여 사용했기 때문에 이름을 몰라도 사용할 수 있었다.

## 이름 있는 파이프(named pipe)

- (이름 없는) 파이프
  - 이름이 없으므로 부모 자식과 같은 서로 관련된 프로세스 사이의 통신에만 사용될 수 있었다. 

- 이름 있는 파이프 (named pipe, fifo file) 
  - 다른 파일처럼 이름이 있으며 파일 시스템 내에 존재한다. 
  - 서로 관련 없는 프로세스들도 공유하여 사용할 수 있다.
  - IPC를 가능케 한다.



<br>

## 이름 있는 파이프를 만드는 방법

- p 옵션과 함께 mknod 명령어

`$mknod myPipe p`

`$chmod ug+rw myPipe`

- user group read/write

`$ls -l myPipe`

`prw-rw-r-- 1 chang faculty 0 4월 11일 13:03 myPipe //p는 pipe파일 임을 명시`

- mkfifo() 시스템 호출

```c
#include <sys/types.h>
#include <sys/stat.h>
int mkfifo(const char *pathname, mode_t mode);
//Mode: 접근권한
//이름 있는 파이프를 생성한다. 성공하면 0을 실패하면 -1을 리턴한다. 
```

<br>

## npreader.c

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#define MAXLINE 100
/* 이름 있는 파이프를 통해 읽은 내용을 프린트한다. */
int main( )
{
    int fd;
    char str[MAXLINE];
    
    unlink("myPipe");
    mkfifo("myPipe", 0660); //myPipe 생성, 접근 권한 0660
    fd = open("myPipe", O_RDONLY); // regular file처럼 열 수 있음.
    
    while (readLine(fd, str))
        printf("%s \n", str);
    close(fd);
    return 0;
}

int readLine(int fd, char *str)
{
    int n;
    do {
        n = read(fd, str, 1);
    } while (n > 0 && *str++ != NULL);
    return (n > 0);
}
```



<br>

## npwriter.c

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#define MAXLINE 100
/* 이름 있는 파이프를 통해 메시지를 출력한다. */
int main( )
{
    int fd, length, i;
    char message[MAXLINE];
    
    sprintf(message, "Hello from PID%d", getpid());
    length = strlen(message)+1;
    
    do {
        fd = open("myPipe", O_WRONLY);
        if (fd == -1) sleep(1);
    } while (fd == -1);
    for (i = 0; i <= 3; i++) {
        write(fd, message, length);
        sleep(3);
    }
    close(fd);
    return 0;
} 
```

```shell
& npwriter &
[2] 10716
[3] 10717
Hello from PID 10717
Hello from PID 10717
Hello from PID 10717
Hello from PID 10717
```

<br>

## 파이프를 이용한 일대일 채팅

- 이 프로그램은 채팅 서버와 채팅 클라이언트 프로그램으로 구성된다. 
- 채팅 서버에서 채팅 클라이언트로 데이터를 보내는데 하나의 파이프가 필요하고 
- 반대로 채팅 클라이언트에서 채팅 서버로 데이터를 보내는데 또 하나의 파이프가 필요하다.



<br>

## chatserver.c

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#define MAXLINE 256
main() {
    int fd1, fd2, n;
    char msg[MAXLINE];
    
    if (mkfifo("./chatfifo1", 0666) == -1) {// named pipe 생성
        perror("mkfifo");
        exit(1);
    }
    
    if (mkfifo("./chatfifo2", 0666) == -1) {
        perror("mkfifo");
        exit(2);
    }
    
    // 파일 이름을 명시하는 이유는 이름있는 pipe이기 때문이다.
    fd1 = open("./chatfifo1", O_WRONLY);// 클라이언트는 이것과 반대가 되어야 겠지?
    fd2 = open("./chatfifo2", O_RDONLY);// 클라이언트는 이것과 반대가 되어야 겠지?
    if (fd1 == -1 || fd2 == -1) {
        perror("open");
        exit(3);
    }
    printf("* 서버 시작 \n");
    while(1) {
        printf("[서버] : ");
        fgets(msg, MAXLINE, stdin);// 키보드 입력
        n = write(fd1, msg, strlen(msg)+1);
        if (n == -1) {
            perror("write");
            exit(1);
        }
        n = read(fd2, msg, MAXLINE);
        printf("[클라이언트] -> %s\n", msg);
    }
}
```

<br>

## chatclient.c

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#define MAXLINE 256
main() {
    int fd1, fd2, n;
    char inmsg[MAXLINE];
    
    fd1 = open("./chatfifo1", O_RDONLY);
    fd2 = open("./chatfifo2", O_WRONLY);
    
    if(fd1 == -1 || fd2 == -1) {
        perror("open");
        exit(1);
    }
    printf("* 클라이언트 시작 \n");
    while(1) {
        n = read(fd1, inmsg, MAXLINE);
        printf("[서버] -> %s\n", inmsg);
        printf("[클라이언트] : ");
        fgets(inmsg, MAXLINE, stdin);
        write(fd2, inmsg, strlen(inmsg)+1);
    }
}
```

```shell
$ chatserver
* 서버 시작
[서버] : 안녕하세요 클라이언트
[클라이언트] -> 반갑습니다.서버.
```

```shell
$ chatclient
* 클라이언트 시작
[서버] : 안녕하세요 클라이언트
[클라이언트] -> 반갑습니다.서버.
```



<br>

## 핵심 개념

- 파이프는 데이터를 한 방향으로 보내는데 사용된다. 
- 파이프는 두 개의 파일 디스크립터를 갖는다. 하나는 쓰기용이고 다른 하나는 읽기용이다. 
- 이름 있는 파이프는 파일처럼 파일 시스템 내에 존재하고 이름이 있으며 서로 관련 없는 프로세스들도 공유하여 사용할 수 있다.