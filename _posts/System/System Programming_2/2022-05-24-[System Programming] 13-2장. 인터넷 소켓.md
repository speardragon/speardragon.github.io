---
layout: single
title: "[System Programming] 13-2장. 인터넷 소켓"
categories: ['System', 'System Programming']
tag: ['Socket', 'Network']
---

<br>

# 13.2 인터넷 소켓

## 인터넷 상의 호스트

- 인터넷 상의 호스트는 32 비트 IP 주소를 갖는다 
  - 예: 203.252.201.8 
- IP 주소는 대응하는 도메인 이름을 갖는다. 
  - 예: 203.252.201.8 --> www.sookmyung.ac.kr 
- 32 비트 IP 주소는 저장

```c
/* 인터넷 주소 구조체 */
struct in_addr {
 unsigned int s_addr; // 네트워크 바이트 순서(big-endian)
};
```

<br>

## Byte ordering

- Two types of byte order 
  - Little endian 
    - the least significant byte contains the lowest byte address 
    - Intel, DEC 
  - Big endian 
    - the least significant byte contains the highest byte address 
    - IBM, Motorola 
  - Eg. 0xC3E2

> The byte order for the TCP/IP protocol suite is big endian 

![image](https://user-images.githubusercontent.com/79521972/169861981-beb04e6a-f8b7-422f-a83e-2e45779b07ab.png)



<br>

## 바이트 정렬 함수

- 네트워크 애플리케이션에서 바이트 정렬 방식을 고려해야 하는 경우

![image](https://user-images.githubusercontent.com/79521972/169862096-405e6df8-edf2-4707-b7b3-58b9e4791097.png)

- 프로토콜 구현을 위해 필요한 정보 
  - (a) IP 주소 -> 빅 엔디안 
  - (b) 포트 번호 -> 빅 엔디안 
- 애플리케이션이 주고 받는 데이터 
  - (c) 빅 엔디안 또는 리틀 엔디안으로 통일

![image](https://user-images.githubusercontent.com/79521972/169862220-a81e7596-0f0c-4b14-874d-43eee8b7527a.png)



<br>

## 바이트 정렬 함수

- 바이트 정렬 함수(유닉스 호환)

```c
u_short htons (u_short hostshort); // host-to-network-short
u_long htonl (u_long hostlong); // host-to-network-long
u_short ntohs (u_short netshort); // network-to-host-short
u_long ntohl (u_long netlong); // network-to-host-long
```



<br>

## Network byte order

![image](https://user-images.githubusercontent.com/79521972/169862339-7b2f1f5e-e081-404a-a155-6eec867d1310.png)



<br>

## 바이트 정렬 함수

- SOCKADDR_IN 구조체의 바이트 정렬 방식

![image](https://user-images.githubusercontent.com/79521972/169862431-720e8f84-3197-458f-8da0-d08e50ff3fe0.png)

<br>

## DNS(Domain Name System)

- 인터넷은 IP 주소와 도메인 이름 사이의 맵핑을 DNS라 부르는 젂세계적인 분산 데이터베이스에 유지한다.

```c
/* DNS 호스트 엔트리 구조체 */
struct hostent {
    char *h_name; // 호스트의 공식 도메인 이름
    char **h_aliases; // null로 끝나는 도메인 이름의 배열 alias list
    int h_addrtype; // 호스트 주소 타입(AF_INET)
    int h_length; // 주소의 길이
    char **h_addr_list; // null로 끝나는 in_addr 구조체의 배열, for
    backward compatibility
}; 
```



<br>

## hostent 구조체

![image](https://user-images.githubusercontent.com/79521972/169862608-dfd7a1b7-218d-4988-a22a-92881bd3b9a6.png)



```c
struct hostent
{
    char *h_name;
    char **h_aliases;
    int h_addrtype;
    int h_length;
    char **h_addr_list;
}
```



<br>

## DNS 관련 함수

- 호스트의 IP 주소 혹은 도메인 이름을 이용하여 DNS로부터 호스트 엔트리를 검색할 수 있다

```c
/* IP 주소(네트워크 바이트 정렬)  도메인 이름 */
struct hostent *gethostbyaddr(const char* addr, int len, int type);
//길이가 len이고 주소 타입 type인 호스트 주소 addr에 해당하는 hostent 구조체를 리턴한다.
```

```c
/* 도메인 이름  IP 주소(네트워크 바이트 정렬) */struct hostent*
struct hostent* gethostbyname(char* name); // 도메인 이름
//도메인 이름에 대응하는 hostent 구조체에 대한 포인터를 리턴한다.
```



<br>

## DNS 관련 함수 (IP 주소 변환 함수)

- in_addr 구조체 형식으로 된 IP 주소를 프린트 할 수 있는 스트링으로 변환

```c
char* inet_ntoa(struct in_addr address);
//IP 주소 address에 대응하는 A.B.C.D 포맷의 스트링을 리턴한다.문자열 형태로 IP 주소를 입력받아 32비트 숫자(네트워크 바이트 정렧)로 리턴
```

```c
unsigned long inet_addr(char* string); // network-to-ascii
//A.B.C.D 포맷의 IP 주소를 네트워크 바이트 순서로 된 이진 데이터로 변환하여 리턴한다.
//32비트 숫자(네트워크 바이트 정렬)로 IP 주소를 입력받아 문자열 형태로 리턴
```

<br>

## 인터넷 소켓

- 인터넷 소켓 
  - 서로 다른 호스트에서 실행되는 클라이언트-서버 사이의 통싞 
  - 양방향(2-way) 통싞 
  - 소켓을 식별하기 위해 호스트의 IP 주소와 포트 번호를 사용 
- 인터넷 소켓 연결 예

![image](https://user-images.githubusercontent.com/79521972/169863277-0dfa10b4-166f-4a5c-8511-781e1692e2a6.png)



- 인터넷 소켓 통싞을 사용하는 SW 
  - 웹 브라우저 
  - ftp 
  - telnet 
  - ssh 
- 잘 알려진 서비스의 포트 번호 (well-known port number) 
  - 시갂 서버:     13번 포트 
  - ftp 서버:        20,21번 포트 
  - 텔넷 서버:     23번 포트 
  - 메일 서버:     25번 포트 
  - 웹 서버:         80번 포트

<br>

## 클라이언트-서버 인터넷 소켓 연결 과정

![image](https://user-images.githubusercontent.com/79521972/169863518-86ba78ce-0b39-48f7-8aaf-2762cb0f0632.png)



<br>

## 인터넷 소켓 이름(주소)

- 인터넷 소켓 이름(주소)

```c
struct sockaddr_in {
    unsigned short sin_family; // AF_INET
    unsigned short sin_port; // 인터넷 소켓의 포트 번호
    struct in_addr sin_addr; // 32-bit IP 주소
    char sin_zero[8]; // 사용 안 함
}
```

- 소켓 이름을 위한 포괄적 구조체

```c
struct sockaddr {
    unsigned short sa_family; // 프로토콜 패밀리
    char sa_data[14]; // 주소 데이터
}; 
```

<br>

## 파일 서버-클라이언트

- 서버 
  - **파일 이름을 받아 해당 파일을 찾아 그 내용을 보내주는 서비스** 
  - 명령줄 인수로 포트 번호를 받아 해당 소켓을 만듞다. 
  - 이 소켓을 통해 클라이언트로부터 파일 이름을 받아 
  - 해당 파일을 열고 그 내용을 이 소켓을 통해 클라이언트에게 보낸다. 
- 클라이언트 
  - 명령줄 인수로 연결할 서버의 이름과 포트 번호를 받아 해당 서버에 소켓 연결을 한다. 
  - 이 연결을 통해 서버에 원하는 파일 이름을 보낸후 
  - **서버로부터 해당 파일 내용을 받아 사용자에게 출력한다**

<br>

## fserver.c

```c
#include <stdio.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#define DEFAULT_PROTOCOL 0
#define MAXLINE 100

/* 파일 서버 프로그램 */
int main (int argc, char* argv[])
{
    int listenfd, connfd, port, clientlen;
    FILE *fp;
    char inmsg[MAXLINE], outmsg[MAXLINE];
    struct sockaddr_in serveraddr, clientaddr;
    struct hostent *hp;
    char * haddrp;
    signal(SIGCHLD, SIG_IGN);

    if (argc != 2) {
        fprintf(stderr, "사용법: %s <port>\n", argv[0]);
        exit(0);
    }
    port = atoi(argv[1]);

    listenfd = socket(AF_INET, SOCK_STREAM, DEFAULT_PROTOCOL);

    bzero((char *) &serveraddr, sizeof(serveraddr));
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = htonl(INADDR_ANY);
    serveraddr.sin_port = htons((unsigned short)port);
    bind(listenfd, &serveraddr, sizeof(serveraddr));
    listen(listenfd, 5);
    while (1) {
        clientlen = sizeof(clientaddr);
        connfd = accept(listenfd, &clientaddr, &clientlen);

        /* 클라이언트의 도메인 이름과 IP 주소 결정 */
        hp = gethostbyaddr((char *)&clientaddr.sin_addr.s_addr,
                           sizeof(clientaddr.sin_addr.s_addr), AF_INET);
        haddrp = inet_ntoa(clientaddr.sin_addr);
        printf("서버: %s (%s) %d에 연결됨\n",
               hp->h_name, haddrp, clientaddr.sin_port);
        if (fork ( ) == 0) {
            readLine(connfd, inmsg); /* 소켓에서 파일 이름을 읽는다 */
            fp = fopen(inmsg, "r");
            if (fp == NULL) {
                write(connfd, "해당 파일 없음", 10);
            } else { /* 파일에서 한 줄씩 읽어 소켓을 통해 보낸다 */
                while(fgets(outmsg, MAXLINE, fp) != NULL)
                    write(connfd, outmsg, strlen(outmsg)+1);
            }
            close(connfd);
            exit (0);
        } else close(connfd);
    } // while
} // main
```



<br>

## fclient.c

```c
#include <stdio.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>
#define DEFAULT_PROTOCOL 0
#define MAXLINE 100

/* 파일 클라이언트 프로그램 */
int main (int argc, char* argv[])
{
    int clientFd, port, result;
    char *host, inmsg[MAXLINE], outmsg[MAXLINE];
    struct sockaddr_in serverAddr;
    struct hostent *hp;
    if (argc != 3) {
        fprintf(stderr, "사용법 : %s <host> <port>\n", argv[0]);
        exit(0);
    }

    host = argv[1];
    port = atoi(argv[2]);
    clientFd = socket(AF_INET, SOCK_STREAM, DEFAULT_PROTOCOL);

    /* 서버의 IP 주소와 포트 번호를 채운다. */
    if ((hp = gethostbyname(host)) == NULL)
        perror("gethostbyname error"); // 호스트 찾기 오류
    bzero((char *) &serverAddr, sizeof(serverAddr));
    serverAddr.sin_family = AF_INET;
    bcopy((char *)hp->h_addr_list[0],
          (char *)&serverAddr.sin_addr.s_addr, hp->h_length);
    serverAddr.sin_port = htons(port);
    do { /* 연결 요청 */
        result = connect(clientFd, &serverAddr, sizeof(serverAddr));
        if (result == -1) sleep(1);
    } while (result == -1);

    printf("파일 이름 입력:");
    scanf("%s", inmsg);
    write(clientFd,inmsg,strlen(inmsg)+1);

    /* 소켓으로부터 파일 내용 읽어서 프린트 */
    while (readLine(clientFd,outmsg))
        printf("%s", outmsg);
    close(clientFd);
    exit(0);
} 
```

- 실행

```shell
$ fclient eeinfo.kw.ac.kr 5000
다운로드할 파일 이름 입력: youtxt
----------------
```

![image](https://user-images.githubusercontent.com/79521972/169865142-65dbb855-6579-419e-9d8b-74b7254468e0.png)



<br>

## 클라이언트-서버

![image](https://user-images.githubusercontent.com/79521972/169865209-5a4ce818-0175-40b4-939a-1e20ddf86f09.png)



<br>

## 핵심 개념

- 소켓은 양방향 통신 방법으로 클라이언트-서버 모델을 기반으로 프로세스 사이의 통신에 매우 적합하다. 
- 소켓에는 같은 호스트 내의 프로세스 사이의 통신을 위한 유닉스 소켓과 다른 호스트에 있는 프로세스 사이의 통신을 위한 인터넷 소켓 이 있다. 