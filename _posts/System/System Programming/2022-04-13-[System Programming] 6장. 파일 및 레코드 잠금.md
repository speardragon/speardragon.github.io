---
layout: single
title: "[System Programming] 6장. 파일 및 레코드 잠금"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File', 'Locking']

---



<br>

# 6장. 파일 및 레코드 잠금

## 프로세스간 통신(IPC)과 동기화(Synchronization)

- Signal 
  - Event notification 
- Pipe 
  - 프로세스 간 데이터 전송용 FIFO 
- Socket 
  - 프로토콜 스택을 이용한 데이터 전송
- File Locking 
  - 다른 프로세스에 의한 파일 읽기/갱신 방지 목적의 파일 잠금
- Message Queue 
  - 메시지 단위로 데이터 전달
- Semaphore 
  - **동기화** 
- Shared Memory 
  - 메모리 공유



<br>



## 파일 및 레코드 잠금의 원리

- 어떻게 프로세스 사이에 데이터를 주고받을 수 있을까? (두 개의 ms word 창이 띄워진 것을 생각해 보자.)
  - 한 프로세스가 파일에 쓴 내용을 다른 프로세스가 읽음

![image](https://user-images.githubusercontent.com/79521972/163096953-0a604ef1-3e5a-41e5-b721-1409f2770e93.png)



- 문제점 
  - 한 프로세스가 파일 내용을 수정하는 동안에 다른 프로세스가 그 파일을 읽는 경우 
  - 두 개의 프로세스가 하나의 파일에 동시에 접근하여 데이터를 쓰는 경우

이 두 프로세스 간 indepenent하게 동작하도록 한다. 그치만 어떤 경우에는 정보 교환이 가능하게 해야 할 경우도 있을 것이다.

<br>

## 잠금 (lock)

- 파일 혹은 레코드(파일의 일부 영역) 잠금 
- 한 프로세스가 그 영역을 읽거나 수정할 때 다른 프로세스의 접근을 제한
- **잠금된 영역에 한 번에 하나의 프로세스만 접근**

![image](https://user-images.githubusercontent.com/79521972/163097039-1b01059d-baaf-472b-8307-7dc6daa5040d.png)



- 특히 레코드에 쓰기(혹은 수정)를 할 경우 대상 레코드에 대해 잠금을 해서 다른 프로세스가 접근하지 못하게 해야 한다.

<br>

### 잠금이 필요한 예

- 잠금이 없는 경우

(1) 프로세스 A가 잔액을 읽는다: 잔액 100만원 

(2) 프로세스 B가 잔액을 읽는다: 잔액 100만원 

(3) 프로세스 B가 잔액에 입금액을 더하여 레코드를 수정한다: 잔액 120만원 

(4) 프로세스A가 잔액에 입금액을 더하여 레코드를 수정한다: 잔액 110만원



- 잠금 사용

(1) 프로세스A가 레코드에 잠금을 하고 잔액을 읽는다: 잔액 100만원 

(2) 프로세스A가 잔액에 입금액을 더하여 레코드를 수정하고 잠금 을 푼다: 잔액 110만원 

(3) 프로세스 B가 레코드에 잠금을 하고 잔액을 읽는다: 잔액 110만원 

(4) 프로세스 B가 잔액에 입금액을 더하여 레코드를 수정하고 잠금 을 푼다: 잔액 130만원

<br>

## 잠금 구현

- flock() 함수
  - 전체 파일을 잠근다. 
  - Concurrency 제약 (수정하고 있지 않은 부분까지 읽지 못하는 문제가 발생)
    - concurrency: 두 개 이상의 프로세스가 동시에 실행되는 것

- 잠금의 종류
  - 공유 잠금
  - 전용 잠금

![image](https://user-images.githubusercontent.com/79521972/163097208-dfbdf14c-0b6c-46b8-857a-738911df71fa.png)

<br>

- fcntl() 함수
  - 파일 및 레코드 잠금을 구현할 수 있다. 

- 잠금의 종류
  - F_RDLCK : 여러 프로세스가 공유 가능한 읽기 잠금 (공유잠금에 해당) **(시험)**
  - F_WRLCK : 한 프로세스만 가질 수 있는 배타적인 쓰기 잠금(전용 잠금에 해당)
  - 데이터를 읽기 전에 transaction은 읽기 잠금을 설정한다. 데이터를 쓰기 전에는 쓰기 잠금을 설정한다.
  - 읽기 잠금은 쓰기 잠금과 충돌을 일으키며, 쓰기 잠금은 읽기 잠금 및 쓰기 잠금과 충돌을 일으킨다.
  - 트랜잭션은 같은 데이터 항목에 대하여 다른 트랜잭션과 충돌을 일으키는 잠금이 없을 경우에만 잠금을 설정할 수 있다.
    - 트랜잭션은 데이터 항목 x에서 쓰기 잠금이 없는 경우에만 x에서 읽기 잠금을 설정할 수 있다.
    - 트랜잭션은 데이터 항목 x에서 읽기 잠금과 쓰기 잠금이 없는 경우에만 x에서 쓰기 잠금을 설정할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/163097415-da6a5348-fd69-4cd2-8c53-95d8401405d0.png)



<br>

### flock()

```c
#include <sys/file.h>
int flock(int fd, int operation);
//전체 파일을 잠근다. 성공하면 0, 실패하면 -1을 리턴
```

- fd는 대상이 되는 파일 디스크립터
- Operation
  - LOCK_SH : fd가 참조하는 파일에 공유 잠금 설정
  - LOCK_EX : fd가 참조하는 파일에 전용 잠금 설정
  - LOCK_UN : fd가 참조하는 파일의 잠금을 해제

<br>

### fcntl()

```c
#include <sys/types.h> // primitive system data types
#include <unistd.h> // symbolic constants
#include <fcntl.h> // file control
int fcntl(int fd, int cmd, struct flock *lock);
//cmd에 따라 잠금 검사 혹은 잠금 설정을 한다. 성공하면 0 실패하면 -1을 리턴
```

- fd는 대상이 되는 파일 디스크립터
- cmd 
  - F_GETLK : 잠금 검사(fd가 현재 잠금이 걸려있는지의 여부)
  - F_SETLK : 잠금 설정 혹은 해제 
  - F_SETLKW: 잠금 설정(블로킹 버전) 혹은 해제 
- flock 구조체 
  - 잠금 종류, 프로세스 ID, 잠금 위치 등에 관한 정보를 담는 구조체



<br>

### flock 구조체

```c
struct flock {
 short l_type; // 잠금 종류: F_RDLCK, F_WRLCK, F_UNLCK
 off_t l_start; // 잠금 시작 위치: 바이트 오프셋
 short l_whence; // 기준 위치: SEEK_SET, SEEK_CUR, SEEK_END
 off_t l_len; // 잠금 길이: 바이트 수 (0이면 파일끝까지)
 pid_t l_pid; // 프로세스 번호
};
```

![image](https://user-images.githubusercontent.com/79521972/163097646-af89051b-1f59-4c07-af33-095f601410a4.png)

<br>

### cmd

- F_GETLK : 잠금 검사 
  - Flock 구조체 lock에 지정된 잠금 설정이 가능한지 검사 
  - **요청된 잠금 설정을 불가하게 하는 기존의 잠금이 있다면** 기존의 잠금 정보가 lock 구조체에 담겨짐 
    - '기존의 담겨있는 잠금이 ~~이기 때문에 해당 잠금 설정을 불허한다.'
    -  short l_type;   // 잠금 종류: F_RDLCK, F_WRLCK (- RDLCK 이나 WRLCK이 걸려있어 잠금 설정을 할 수 없다.)
  - **기존의 잠금이 없다면** l_type 이 F_UNLCK 으로 설정됨(잠금이 걸려있지 않다는 의미)
- F_SETLK : 잠금 설정 
  - l_type이 **F_RDLCK** 혹은 **F_WRLCK**으로 설정됨 (잠금 요청) 
  - 요청 불허 시, 전역변수 errno가 EAGAIN으로 세팅 
- F_SETLK : 잠금 설정 
  - l_type이 F_UNLCK으로 설정됨 (잠금 해제 요청)
- F_SETLKW: 잠금 설정(블로킹 버전) 혹은 해제 
  - <mark>요청된 잠금 설정을 불가하게 하는 기존의 잠금이 있다면, 잠금이 가능할 때까지 블럭된 상태를 유지한다.</mark>
  - 즉 잠금에 성공할 때까지 함수가 호출된 곳으로 리턴하지 않다가 잠금이 되고 나서야 돌아가는 것을 말한다.

(넌블럭킹은 승인이 거절되면 일단 리턴하고 그 이유만 보는 것이다.)

<br>

# 6.2 잠금 예제 및 잠금 함수

## 잠금 예제

- 학생 레코드를 질의하는 프로그램: rdlock.c 
- 학생 레코드를 수정하는 프로그램: wrlock.c 
- 핵심 idea
  - **수정 프로그램에서 어떤 레코드를 수정하는 중에는** 질의 프로그램에서 **그 레코드를 읽을 수 없도록** 레코드 **잠금**을 이용하여 제한한다.


<br>

## rdlock.c **(시험)**

- 검색할 학번을 입력 받는다. 
- 해당 레코드를 읽기 전에 그 레코드에 대해 읽기 잠금 (F_RDLCK)을 한다. 
  - `fcntl(fd, F_SETLKW, &lock); `
- 해당 레코드 영역에 대해 잠금을 한 후에 레코드를 읽는다.

```c
#include <stdio.h>
#include <fcntl.h>
#include "student.h"

/* 잠금을 이용한 학생 데이터베이스 질의 프로그램 */
int main(int argc, char *argv[])
{
    int fd, id;
    struct student record;
    struct flock lock;

    if (argc < 2) {
        fprintf(stderr, "사용법 : %s 파일\n", argv[0]);
        exit(1);
    }

    if ((fd = open(argv[1], O_RDONLY)) == -1) {
        perror(argv[1]);
        exit(2);
    }

    printf("\n검색할 학생의 학번 입력:");
    while (scanf("%d", &id) == 1) {
        lock.l_type = F_RDLCK;
        lock.l_whence = SEEK_SET;
        lock.l_start = (id-START_ID)*sizeof(record); //학생의 고유 위치
        lock.l_len = sizeof(record);
        if (fcntl(fd, F_SETLKW, &lock) == -1) { /* 읽기 잠금 (블락킹 모드)*/
            perror(argv[1]);
            exit(3);
        }

        lseek(fd, (id-START_ID)*sizeof(record), SEEK_SET);
        if ((read(fd, (char *) &record, sizeof(record)) > 0) && (record.id != 0))
            printf("이름:%s\t 학번:%d\t 점수:%d\n", record.name, record.id,
                   record.score);
        else printf("레코드 %d 없음\n", id);

        lock.l_type = F_UNLCK;
        fcntl(fd, F_SETLK, &lock); /* 잠금 해제 (넌 블락킹;해제는 무조건 되기 때문에)*/
        printf("\n검색할 학생의 학번 입력:");
    } 
    close(fd);
    exit(0);
}
```

```shell
$ rdlock stdb1
검색할 학생의 학번 입력: 140010
--
```



<br>

## wrlock.c

- 수정할 학번을 입력 받는다.

- 해당 레코드를 읽기 전에 그 레코드에 쓰기 잠금(F_WRLCK)을 한다. 
  - `fcntl(fd, F_SETLKW, &lock); `

- 해당 레코드 영역에 대해 잠금을 한 후에 레코드를 수정한다.

```c
#include <stdio.h>
#include <fcntl.h>
#include "student.h"

/* 잠금을 이용한 학생 데이터베이스 수정 프로그램 */
int main(int argc, char *argv[])
{
    int fd, id;
    struct student record;
    struct flock lock;

    if (argc < 2) {
        fprintf(stderr, "사용법 : %s 파일 \n", argv[0]);
        exit(1);
    }

    if ((fd = open(argv[1], O_RDWR)) == -1) {
        perror(argv[1]);
        exit(2);
    }

    printf("\n수정할 학생의 학번 입력:");
    while (scanf("%d", &id) == 1) {
        lock.l_type = F_WRLCK;
        lock.l_whence = SEEK_SET;
        lock.l_start = (id-START_ID)*sizeof(record);
        lock.l_len = sizeof(record);
        if (fcntl(fd, F_SETLKW, &lock) == -1) { /* 쓰기 잠금(blocking version) */
            perror(argv[1]);
            exit(3);
        }
		
        //읽기 전에 파일 잠금
        lseek(fd, (long) (id-START_ID)*sizeof(record), SEEK_SET);
        if ((read(fd, (char *) &record, sizeof(record)) > 0) && (record.id != 0))
            printf("이름:%s\t 학번:%d\t 점수:%d\n",
                   record.name, record.id, record.score);
        else printf("레코드 %d 없음\n", id);

        printf("새로운 점수: ");
        scanf("%d", &record.score);
        lseek(fd, (long) -sizeof(record), SEEK_CUR);//학생 정보의 처음 위치로 돌아감
        write(fd, (char *) &record, sizeof(record));

        lock.l_type = F_UNLCK;
        fcntl(fd, F_SETLK, &lock); /* 잠금 해제(넌 블락킹) */ 
        printf("\n수정할 학생의 학번 입력:"); 
    } 
    close(fd);
    exit(0);
} 
```

```shell
$ wrlock test.db
수정할 학생의 학번 입력 : 140010
이름:홍길동 학번:140010 점수:90
새로운 점수:
---
```



<br>

## Non-blocking version

```c
while (fcntl(fd, F_SETLK, &lock) == -1){
    if (errno == EAGAIN) {
        if (try++ < MAX) { // MAX 번 만큼 실행 도전!
            sleep(1);
            continue;
        }
        printf("%s다시 시도\n", arv[1]); //안되면 종료!
        exit(2);
    }
    perror(agv[1]);
    exit(3);
}
```

blocking version은 쓰기 설정이 요구되었으면 반영될 때까지 무한정 기다리는데 반해서

non-blocking version은 바로바로 return이 되기 때문에 errno 체크를 통해서 다시 한 번 설정할지 말지를 사용자가 결정하게 된다.

<br>

## 잠금 함수

fcntl()함수는 구조체를 사용하고 있기 때문에 번거로운 단점이 있다. 그래서 lockf는 구조체를 사용하지 않고 len이라는 변수로 기준점은 항상 현재 파일 위치로 정해 놓고 len 길이만 조절하여 파일에 접근한다.

```c
#include <unistd.h>
int lockf(int fd, int cmd, off_t len);
/*
cmd에 따라 잠금 설정, 잠금 검사 혹은 잠금 해제 한다.
잠금 영역은 현재 파일 위치부터 len 길이 만큼이다.
len이 0이면 현재 파일 위치부터 파일끝까지임.
성공하면 0 실패하면 –1을 반환한다. 
*/
```

- cmd 
  - F_LOCK : 지정된 영역에 대해 잠금을 설정한다. 이미 잠금이 설정되어 있으면 잠금이 해제될 때까지 기다렸다가 잠금을 한다. :블락킹
  - F_TLOCK : 지정된 영역에 대해 잠금을 설정한다. 이미 잠금이 설정되어 있으면 기다리지 않고 오류(-1)를 반환한다.  : 넌블락킹
  - F_TEST : 지정된 영역이 잠금 되어 있는지 검사한다. 잠금이 설정되어 있지 않으면 0을 반환하고 잠금이 설정되어 있으면 –1을 반환한다. 
  - F_ULOCK : 지정된 영역의 잠금을 해제한다.
- 잠금과 해제가 분리되어 있음

<br>

## wrlockf.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include "student.h"

/* 잠금 함수를 이용한 학생 데이터베이스 수정 프로그램 */
int main(int argc, char *argv[])
{
    int fd, id;
    struct student record;

    if (argc < 2) {
        fprintf(stderr, "사용법 : %s file\n", argv[0]);
        exit(1);
    }

    if ((fd = open(argv[1], O_RDWR)) == -1) {
        perror(argv[1]);
        exit(2);
    }

    printf("\n수정할 학생의 학번 입력:");
    while (scanf("%d", &id) == 1) {
        lseek(fd, (long) (id-START_ID)*sizeof(record), SEEK_SET);
        if (lockf(fd, F_LOCK, sizeof(record)) == -1) { // 쓰기 잠금; 블락킹
            perror(argv[1]);
            exit(3);
        }

        if ((read(fd, (char *) &record, sizeof(record)) > 0) && (record.id != 0))
            printf("이름:%s\t 학번:%d\t 점수:%d\n",
                   record.name, record.id, record.score);
        else printf("레코드 %d 없음\n", id);

        printf("새로운 점수: ");
        scanf("%d", &record.score);
        lseek(fd, (long) -sizeof(record), SEEK_CUR);
        write(fd, (char *) &record, sizeof(record));

        lseek(fd, (long) (id-START_ID)*sizeof(record), SEEK_SET);
        lockf(fd, F_ULOCK, sizeof(record)); // 잠금 해제
        printf("\n수정할 학생의 학번 입력:");
    }

    close(fd);
    exit(0);
}
```

<br>

# 6.3 권고 잠금과 강제 잠금

## 권고 잠금과 강제 잠금

- **권고 잠금** (advisory locking) 
  - 지금까지 살펴본 잠금: 잠금을 할 수 있지만 강제되지는 않음. 
  - 즉 이미 잠금 된 파일의 영역에 대해서도 잠금 규칙을 무시하고 읽거나 쓰는 것이 가능 
  - 모든 관련 프로세스들이 자발적으로 잠금 규칙을 준수해야만 이 기능이 가능한 것. 
- **강제 잠금 **(mandatory locking) 
  - 이미 잠금 된 파일 영역에 대해 잠금 규칙을 무시하고 읽거나 쓰는 것이 불가능 
  - **커널이** 잠금 규칙을 **강제**하므로 시스템의 부하가 증가 
  - System V 계열에서 제공됨. 
  - Linux의 경우 파일 시스템을 마운트할 때 `-o mand` 옵션을 사용 해서 마운트 해야 제공됨

default는 권고 잠금이다.(강제 잠금을 원하면 위와 같이 옵션을 명시)

<br>

## 강제 잠금

- 강제 잠금을 하는 방법 
  - 해당 파일에 대해 set-**group**-ID 비트를 설정하고 
  - **group**-execute 비트를 끄면 된다 (3번째 'x' 위치에 S가 들어간다. )

```shell
$ chmod 2644 mandatory.txt (set-group-ID 비트 설정)  # group은 4: read만 가능하도록 하였음
$ ls -l mandatory.txt 
-rw-r-Sr-- 1 chang faculty 160 1월 31일 11:48 stdb1
```

- 강제 잠금 규칙

![image](https://user-images.githubusercontent.com/79521972/163098865-5d3c7ed8-4c2c-42de-b18e-ac0a0609d30c.png)

넌 블락킹은 바로 리턴되고 오류 넘버를 리턴

대상 영역의 현재 잠금 상태가 읽기 잠금 일 때 넌 블로킹이든 블로킹이든 읽기 잠금 요청은 허락된다.

<br>

## 강제 잠금을 하는 방법

- special bits
  - #define S_ISUID 0004000 /* set uid on execution */ 
  - #define <span style="color:red">S_ISGID 0002000</span> /* set group id on execution \*/ 
  - #define S_ISVTX 0001000 /* save text(sticky bit) */

![image](https://user-images.githubusercontent.com/79521972/163098960-65984e6d-4181-4971-9db5-ea4e45b09d68.png)



<br>

## file_lock.c

```c
#include <stdio.h>
#include <fcntl.h>
int main(int argc, char **argv) {
    static struct flock lock;
    int fd, ret, c;
    if (argc < 2) {
        fprintf(stderr, "사용법: %s 파일\n",
                argv[0]);
        exit(1);    
    }
    fd = open(argv[1], O_WRONLY);
    if (fd == -1) {
        printf("파일 열기 실패 \n");
        exit(1);
    }
    lock.l_type = F_WRLCK;
    lock.l_start = 0;
    lock.l_whence = SEEK_SET;
    lock.l_len = 0; //파일 전체
    lock.l_pid = getpid();
    ret = fcntl(fd, F_SETLKW, &lock);
    if(ret == 0) { // 파일 잠금 성공하면
        c = getchar();
    }
}
```

<br>

## 실행

- 강제 잠금 설정

```shell
$ file_lock mandatory.txt  #강제 잠금 설정이 되어 있기 때문에 블락 되어 있음
 - 		
```

```shell
$ ls >> mandatory.txt
- 							# 잠금 해제 때 까지 블록
							# ls 명령어가 종료되지 않음
```

강제 잠금을 이미 한 상태에서 쓰기 잠금을 요청하기 때문에 프롬프트가 나오지 않는 것이다.

<br>

- 강제 잠금 미설정 (권고 잠금)

```shell
$ file_lock advisory.txt
$
```

```shell
$ ls >> advisory.txt
$ 						# 잠금 여부에 관계 없이 ls 명령어 결과가 advisory.txt에 저장됨
```



<br>

## 핵심 개념

- 한 레코드 혹은 파일에 대한
  - 읽기 잠금은 여러 프로세스가 공유할 수 있지만 
  - 쓰기 잠금은 공유한 수 없으며 한 프로세스만 가질 수 있다. 
- fcntl() 시스템 호출을 이용하여 지정된 영역에 대해 잠금 검사, 잠금 설정 혹은 잠금 해제를 할 수 있다. 
- lockf() 함수를 이용하여 지정된 영역에 대해 잠금 검사, 잠금 설정 혹은 잠금 해제를 할 수 있다
  - 두 개가 다른 점은 fcntl은 lock 정보를 담는 구조체로 파일 잠금을 하는 것이지만 lockf는 len이라는 파일의 길이로 파일 잠금을 어디를 해야할지는 정하는 것이 다르다.


<br>















