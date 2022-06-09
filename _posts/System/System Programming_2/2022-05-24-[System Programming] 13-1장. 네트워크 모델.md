---
layout: single
title: "[System Programming] 13-1장. 소켓"
categories: ['System', 'System Programming']
tag: ['Socket', 'Network']
---

<br>

# 13.0 네트워크 모델

## Network Model

- How is data transferred through network? 
  - **circuit switching**: <span style="color:red">dedicated circuit per call: telephone net </span>
    - the whole message is sent from the source to the destination without being divided into packets 
    - End-end resources reserved for “**call**” 
    - QoS 보장 : No end-to-end delay 
      - network가 커지더라고 항상 end-to-end를 보장한다.
    - Call blocking in case of congestion 
- **packet-switching**: data sent thru net in discrete “chunks” 
  - Store & forward 
  - each end-end data stream divided into **packets** 
  - **자원 공유** 
  - QoS 보장 X 
  - Increaed end-to-end delay

<br>

## Circuit-switching

![image](https://user-images.githubusercontent.com/79521972/169854256-f8620029-3dac-4ff1-a7fe-c6bcd7489665.png)

회선을 점유

waiting 시간 없이 바로 전송

<br>

## Packet-switching

![image](https://user-images.githubusercontent.com/79521972/169854337-f3365e2a-abfa-4e75-8cac-833db40da2e3.png)

waiting time: packet이 router에 저장됐다가 다시 보내기 전까지 걸리는 시간

매 packet마다 다음 router가 어딘지를 찾아서 이동을 한다.

<br>

## Internet

- A network of networks
  - 여러 network로 이루어진 하나의 network


![image](https://user-images.githubusercontent.com/79521972/169854408-91111412-3f1b-415a-b465-bab0f7d508c2.png)



<br>

## OSI 7 Layer Model

![image](https://user-images.githubusercontent.com/79521972/169854489-324d95c9-24c2-4064-82fd-50e40c8f0f4b.png)



<br>

## 인터넷 구성

![image](https://user-images.githubusercontent.com/79521972/169854560-adf0af46-3949-4a0c-b368-c189e1175f4a.png)



<br>

## TCP/IP 프로토콜

![image](https://user-images.githubusercontent.com/79521972/169854660-c6264ff9-b9f6-4304-841f-e5740fb403b9.png)



<br>

## Designing Applications for a Distributed Environment

분산 애플리케이션

- Programmers who build applications for a distributed environment try to **make distributed application** behave as much as possible like the **non-distributed version** of the program 
- Goal of distributed computing is to provide an environment that hides the geographic location of computers and services and makes them appear to be local 
- Service **should** be provided with **Concurrency**



<br>

## TCP/IP 프로토콜

- TCP/IP 프로토콜 
  - 인터넷의 핵심 프로토콜인 TCP와 IP를 포함한 각종 프로토콜 
  - **운영체제**에서 구현을 제공하며, 일반 애플리케이션은 운영체제가 제공하는 TCP/IP 프로토콜의 서비스를 사용하여 통신을 수행

<br>

## TCP/IP 프로토콜 구조

- TCP/IP 프로토콜 구조 
  - 계층적 구조

![image](https://user-images.githubusercontent.com/79521972/169855067-7978daea-0060-41b5-8c93-db836baf029a.png)



<br>

## TCP/IP 프로토콜 구조

- **네트워크 액세스 계층**(network access layer) 
  - 물리적 네트워크를 통한 실제적인 데이터 전송 
  - 구성 요소 
    - 네트워크 하드웨어 + 디바이스 드라이버 
  - 주소 지정 방식 
    - 물리 주소(physical address) - MAC address
- 예 
  - 이더넷(Ethernet)

- **인터넷 계층**(Internet layer) 
  - 네트워크 액세스 계층의 도움을 받아, 젂송 계층이 내려 보낸 데이터를 종단 시스템까지 전달 
  - 사용 주소  
    - IP 주소(Internet Protocol address) 
  - 라우팅(routing) 
    - 목적지까지 데이터를 전달하기 위한 일련의 작업 라우팅을 위한 정보 획득 라우팅 정보를 기초로 실제 데이터 전달(forward)

- **전송 계층**(transport layer) 
  - 최종적인 통신 목적지(프로세스)를 지정하고, 오류 없이 데이터를 전송 
  - 사용 주소 
    - 포트 번호(port number) 
  - 예 
    - TCP(Transmission Control Protocol) 
    - UDP(User Datagram Protocol) 
- **애플리케이션 계층**(application layer) 
  - 다수의 응용 프로토콜과, 응용 프로토콜을 이용하는 애플리케이션을 포괄 
  - 예 
    - Telnet, FTP, HTTP, SMTP 등

<br>

## TCP/IP 프로토콜 구조

- TCP와 UDP

![image](https://user-images.githubusercontent.com/79521972/169855643-9098e98b-e897-4f4b-aef3-87aff2d264d7.png)

둘 다 packet switching

<br>

## 패킷 전송 원리

- 패킷 전송 형태 
  - 송신측

![image](https://user-images.githubusercontent.com/79521972/169855889-3c204209-577f-4f99-a936-40919040bb69.png)

- 수신측

![image](https://user-images.githubusercontent.com/79521972/169855917-2686df25-ab2d-4c75-a2c8-fb178f9fa384.png)

-  계층별 (peer-to-peer communication)

![image](https://user-images.githubusercontent.com/79521972/169855956-9a1845d2-3263-4196-9bfe-ab40a479b9c1.png)



- TCP/IP 프로토콜을 이용한 패킷 전송

![image](https://user-images.githubusercontent.com/79521972/169856120-39874334-50ab-4c46-a52b-ecf9c3fc09cd.png)

가운데 두 라우터가 연결된 부분이 network

<br>

## IP 주소와 포트 번호

- **IP 주소** 
  - 인터넷 계층에서 사용하는 주소 
  - 인터넷에 존재하는 호스트(종단 시스템, 라우터)를 유일하게 구별 할 수 있는 식별자 
  - Software defined **Globally unique address** 
    - 전 세계에 같은 IP주소를 사용하는 경우는 허락되지 않은 것
  - IPv4는 32비트, IPv6는 128비트 사용 
  - 8비트 단위로 구분하여 10진수로 표기(IPv4) 
  - 예) 147.46.114.70 
- **포트 번호** 
  - 전송계층에서 사용하는 주소 
  - 통신 종착지(하나 혹은 여러 개의 프로세스)를 나타내는 식별자 
- 네트워크 액세스 계층에서 사용하는 주소는 MAC address임
- IP + 포트 번호 -> socket address

<br>

- IP 주소와 포트 번호

![image](https://user-images.githubusercontent.com/79521972/169856319-ebf4ffcc-1518-4ce9-99e8-3d7538a21086.png)

- 모든 포트들은 unique한 번호를 할당 받는다.
  - 포트 번호는 어떤 컴퓨터 안에있는 프로세스를 구분하기 위한 것

- IP주소는 host(특정 컴퓨터)를 구별하기 위한 번호



<br>

# 13.1 소켓

## 클라이언트-서버 모델

- 네트워크 응용 프로그램 
  - 클라이언트-서버 모델을 기반으로 동작한다. 

- 클라이언트-서버 모델 
  - 하나의 서버 프로세스와 여러 개의 클라이언트로 구성된다. 
  - 서버는 어떤 자원을 관리하고 클라이언트를 위해 자원 관련 서비스를 제공한다.

![image](https://user-images.githubusercontent.com/79521972/169856512-c8f507d2-637a-4ffb-aefe-6b63e0e270bb.png)



<br>

## Communications in Client-Server Systems

- **Sockets** – low level form of communication between distributed processes 
- Remote Procedure Calls (RPC)
- Pipes 
- Remote Method Invocation (Java)



<br>

## IPC (Inter Process Communication)

- pipes, FIFOs, message queues, semaphores, and shared memory 
  - allow processes running on the **same machine** to communicate with one another. 

- **socket** 
  - allows processes running on **different machines** to communicate with one another.
  - 다른 머신에서 동작해도 돌아간다!

![image](https://user-images.githubusercontent.com/79521972/169856722-e180503b-37c9-449b-9495-b7614f599fbb.png)



<br>

## Socket

- Socket 
  - an abstraction of a communication endpoint. 
- Socket descriptor 
  - Just as applications would use file descriptors to access a file, applications use <span style="color:red">socket descriptors</span> to access sockets. 
  - Socket descriptors are implemented <span style="color:red">as file descriptors</span>. 
    - 이름만 다르고 socket descriptor와 file descriptor는 같은 방식으로 구현되어 있음
    - many of the functions that deal with file descriptors (**read** and **write**) will work with a socket descriptor. 
    - But, **lseek** doesn't work with sockets, since sockets don't support the concept of a file offset.

<br>

- Socket descriptor and file descriptor **shares a table**. 
  - 3 and 4 are file descriptors. 
  - 5 is a socket descriptor.

![image](https://user-images.githubusercontent.com/79521972/169857068-88ef3c3e-8be5-40ad-9375-ca63cc0a1899.png)



- descriptor table은 per process structure이다. 따라서, 다른 process간에는 같은 값의 descriptor를 가질 수 있다.

<br>

## 소켓의 개념

- 세 가지 관점 
  - ① 데이터 타입 
  - ② 통신 종단점(communication end-point) 
  - ③ 네트워크 프로그래밍 인터페이스




<br>

- **데이터 타입**
  - 운영체제가 통신을 위해 관리하는 데이터를 간접적으로 참조할 수 있도록 만든 **일종의 핸들**(handle) 
  - 생성과 설정 과정이 끝나면 이를 이용하여 통신과 관련된 다양한 작업을 할 수 있는 간편한 데이터 타입
  - socket()의 리턴 타입은 SOCKET이며 해당 데이터 타입인 변수 sock을 통해 데이터를 보내고 받을 수 있다.

![image](https://user-images.githubusercontent.com/79521972/169857338-f3f5859e-105b-47ea-93ee-0b0aa36e2ae7.png)

- **통신 종단점** (communication endpoint) 
  - 소켓은 통신을 위해 필요한 여러 요소의 집합체 
    - 사용할 프로토콜(TCP/IP, UDP/IP 등) 
    - 송신측 IP 주소, 송신측 포트 번호 (socket address) 
    - 수신측 IP 주소, 수신측 포트 번호 (socket address)
  - 애플리케이션은 자신의 소켓이 상대방의 소켓과 연결된 것으로 생각하고 데이터를 교환

- 통신 종단점(cont’d)

![image](https://user-images.githubusercontent.com/79521972/169857538-dabec282-284f-4bef-915c-cc3d97b19141.png)

- 실제로는 밑으로 TCP/IP layer를 거쳐서 전송이 되는 것 -> 클라이언트 소켓 to 서버 소켓
  - 포트를 통한 logical 적인 송수신

<br>

- **네트워크 프로그래밍 인터페이스** 
  - 통신 양단이 모두 소켓을 사용할 필요는 없음 
  - TCP/IP 프로토콜 구조에서 (일반적으로) 애플리케이션 계층과 전송 계층 사이에 위치하는 것으로 간주

![image](https://user-images.githubusercontent.com/79521972/169857740-5c0b1c5f-2ce7-4233-86db-d1882e6ba574.png)



<br>

## 소켓의 종류

- 소켓 
  - 네트워크에 대한 사용자 수준의 인터페이스를 제공 
  - 소켓은 **양방향** 통신 방법으로 클라이언트-서버 모델을 기반으로 프로세스 사이의 통신에 매우 적합하다. 
  - 유닉스 소켓(AF_UNIX) 
    - **같은 호스트** 내의 프로세스 사이의 통신 방법 
  - 인터넷 소켓(AF_INET) 
    - 인터넷에 연결된 서로 **다른 호스트**에 있는 프로세스 사이의 통신 방법

<br>

## 소켓 연결

1. 서버가 소켓을 만든다. 
2. 클라이언트가 (자기 자신의)소켓을 만든 후 서버에 연결 요청을 한다. 
3. 서버가 클라이언트의 연결 요청을 수락하여 소켓 연결이 이루어진다. 

![image](https://user-images.githubusercontent.com/79521972/169857971-74e7f570-4c57-4662-9d4c-76f7ff218bd1.png)



<br>

## 소켓 연결 과정

- 서버

1. socket() 호출을 이용하여 소켓 을 만들고 이름을 붙인다. 

2. listen() 호출을 이용하여 대기 큐를 만든다. 

3. 클라이언트로부터 연결 요청을 accept() 호출을 이용하여 수락 

4. 소켓 연결이 이루어지면 

   - 자식 프로세스를 생성하여 
     - conquency하게 할 수 없기 때문에 클라이언트 마다 자식 프로세스를 하나씩 만들어 처리하게 되면 동시에 여러 client를 상대 가능하게 된다.
   
   - 클라이언트로부터 요청 처리 
   
   - 클라이언트에게 응답한다.

- 클라이언트

1. socket() 호출을 이용하여 소켓 을 만든다. 
2. connection() 호출을 이용하여 서버에 연결 요청을 한다. 
3. 서버가 연결 요청을 수락하면 소켓 연결이 만들어진다. 
4. 서버에 서비스를 요청하고 서버로부터 응답을 받아 처리



<br>

## 소켓 연결 과정

![image](https://user-images.githubusercontent.com/79521972/169858416-55c32594-0cbd-40fe-96b4-e654ab8b94f6.png)

여러 클라이언트를 받을 수 없는 서버이기 때문에(서버 프로세스 자체가 클라이언트 프로세스를 상대하고 있기 때문) 다음 클라이언트로부터 연결 요청을 기다린다.

<br>

## 소켓 만들기

- Sets values of the socket structure

```c
#include <sys/socket.h>
int socket(int domain, int type, int protocol)
//소켓을 생성하고 소켓을 위한 파일 (소켓) 디스크립터를 리턴, 실패하면 -1을 리턴
```

-  인터넷 소켓

  fd = socket(AF_INET, SOCK_STREAM, DEFAULT _PROTOCOL); 

- 유닉스 소켓 

  fd = socket(AF_UNIX, SOCK_STREAM, DEFAULT _PROTOCOL);

<br>

## Socket 구조체

![image](https://user-images.githubusercontent.com/79521972/169858862-59aed75c-dd79-4279-99dc-2c4e0048f89e.png)

socket()에 domain type protocol 세 개의 parameter를 주게 되면 구조체의 각 멤버(domain, type, protocol)가 해당 값으로 setting 된다.

<br>

## socket()

- domain argument determines the nature of the communication , including the address format. 
  - **AF_INET**: IPv4 Internet domain 
  - AF_INET6: IPv6 Internet domain 
  - AF_UNIX: UNIX domain 
  - AF_UNSPEC: unspecified 
- type argument determines the type of the socket, which further determines the **communication characteristics**. 
  - **SOCK_DGRAM**(UDP): connectionless, unreliable, datagram 
  - **SOCK_STREAM**(TCP): connection-oriented, reliable, stream 
  - SOCK_RAW: Raw socket

- protocol argument selects the default protocol for the given domain and socket type. 
  - TCP (Transmission Control Protocol): 
    - SOCK_STREAM in AF_INET domain 
  - UDP (User Datagram Protocol): 
    - SOCK_DGRAM in AF_INET domain 
  - **Usually, zero is used**.

<br>

## 소켓 생성

- 소켓 타입 
  - 사용할 프로토콜의 특성

![image](https://user-images.githubusercontent.com/79521972/169859145-d06e1cd1-ba05-4a69-800b-cdbe7062335b.png)

- 예) TCP와 UDP 프로토콜 설정(1)

![image](https://user-images.githubusercontent.com/79521972/169859192-e62bbb7b-41b6-4875-96db-5e2f9162caa9.png)



<br>

## socket()

- example

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <sys/socket.h>
int main()
{
    int fd1, fd2, sd1, sd2;
    
    fd1 = open("/etc/passwd", O_RDONLY);
    printf("/etc/passwd's file descriptor = %d\n", fd1);
    
    sd1 = socket(PF_INET, SOCK_STREAM, 0);
    printf("stream socket descriptor = %d\n", sd1); 
    
    sd2 = socket(PF_INET, SOCK_DGRAM, 0);
    printf("datagram socket descriptor = %d\n", sd2);
    
    fd2 = open("/etc/hosts", O_RDONLY);
    printf("/etc/hosts's file descriptor = %d\n", fd2);
    
    close(fd2) ;
    close(fd1) ;
    close(sd2) ;
    close(sd1) ;
}
```

- 실행

```shell
$gcc -o open_socket open_socket.c -lsocket
$./open_socket
/etc/passwd's file descriptor = 3
stream socket descriptor = 4
datagram socket descriptor = 5
/etc/hosts's file descriptor = 6
$
```

같은 table을 공유하고 있다는 것을 보여주는 예제

<br>

## Socket address

- Overall structure including socket

![image](https://user-images.githubusercontent.com/79521972/169859449-5e41be4b-ccdd-4e93-b6fa-2d7fdb0cfece.png)

응용 프로세스에서 소켓 혹은 파일을 열면 소켓은 communication endpoint이기 때문에 응용 프로그램에 붙어 있는데 

데이터가 네트웍을 타고 들어왔을 때 어떤 프로세스에 전달할 것인지를 결정하는 것이 포트 번호

<br>

## TCP 서버/클라이언트 분석 (2/2)

- 소켓 데이터 구조체

![image](https://user-images.githubusercontent.com/79521972/169859546-059c520c-582f-4a97-97fe-2b8502907300.png)



<br>

## 소켓 주소 구조체 정의

- 소켓 주소 구조체(socket address structures) 
  - 네트워크 프로그램에서 필요로 하는 주소 정보를 담고 있는 **구조체**로, 다양한 소켓 함수의 인자로 사용 
  - 주소 체계에 따라 **다양한 형태**가 존재 
  - 예) TCP/IP -> SOCKADDR_IN(Internet), IrDA -> SOCKADDR_IRDA 
  - 기본형은 SOCKADDR 구조체임

- SOCKADDR 구조체 (Generic socket address structure)

```c
#include <sys/socket.h>
struct sockaddr {
    u_short sa_family;
    char sa_data[14];
};
typedef struct sockaddr SOCKADDR;
```

- sa_family 
  - **주소 체계**를 나타내는 상수값 
  - 예) TCP/IP 프로토콜 -> AF_INET 
- sa_data 
  - 해당 주소 체계에서 사용하는 주소 정보 예) TCP/IP 프로토콜 
  - IP 주소와 포트 번호



<br>

## 소켓에 이름(주소) 주기

```c
int bind(int fd, struct sockaddr* address, int addressLen)
//소켓에 대한 이름 바인딩이 성공하면 0을 실패하면 -1을 리턴한다.
```

-  소켓에 주소를 부여하는 개념
  
-  associate an address with a socket. 
  - 서버의 지역 IP 주소와 지역 포트 번호를 결정(초기화) 
  - For a **server**, we need to associate a **well-known address** with the server's socket on which client requests will arrive. 
    - why well-known? -> 누구나 다 알아야 하는 주소이기 때문에 (그래야 클라이언트 서버 모델에서는 클라이언트가 서비스 요청을 하면 서버의 socket address를 모르면 서비스 요청을 할 수 없기 때문) 
  - For client, we can let the system choose a default address. 
    - -> bind() is not used in client side. 
  - len is the size of socket address
  
- 유닉스 소켓 이름

```c
struct sockaddr_un {
    unsigned short sun_family; 	// AF_UNIX
    char sun_path[108]; 		// 소켓 이름
}
```



<br>

## 소켓에 이름(주소) 주기

- 인터넷 소켓 이름

```c
struct in_addr {
    in_addr_t s_addr; /* IPv4 address */
};
```

```c
struct sockaddr_in {
    unsigned short sin_family; // AF_INET
    unsigned short sin_port; // 인터넷 소켓의 포트 번호
    struct in_addr sin_addr; // 32-bit IP 주소
    char sin_zero[8]; // 사용 안 함
}
```

- sockaddr 구조체는 IP address, port number 등을 직접 쓰거나 읽기가 불편함 
  - 필드가 구분되어 있지 않기 때문(IP+port number)

- sockaddr 구조체 대신 4 바이트의 IP address와 2 바이트의 port 번호를 구분하여 지정할 수 있는 인터넷 전용 소켓주소 구조체인 **sockaddr_in**을 **주로 사용함**



<br>

## Socket address

- Socket address structure 
  - sockaddr_in은 sockaddr 구조체의 데이터를 internet protocol에서 사용 하기에 적합하도록 수정한 것이다.

![image](https://user-images.githubusercontent.com/79521972/169860271-a79ff223-f03c-4b6b-a74c-f607c2fce177.png)

sockaddr_in이 사용하기 훨씬 더 편함

<br>

## TCP Socket programming

- socket programming’s basic procedure

![image](https://user-images.githubusercontent.com/79521972/169860338-9fe26c12-60b2-4570-b1ca-fae94163528b.png)



<br>

## UDP Socket programming

- UDP 서버/클라이언트 구조

![image](https://user-images.githubusercontent.com/79521972/169860392-ccb3f220-f81c-432a-8cc8-6984363c80dc.png)



server와 client가 socket을 생성하는 것까지는 TCP와 같은데

서버 측의 accept(), listen()와 클라이언트 측의 connect()가 없어졌다.

이는 UDP는 연결 설정이라는 것을 하지 않기 때문.

- UDP는 accept(), listen(), connect()가 필요 없음



<br>

## 소켓 큐 생성

- listen() 시스템 호출 
  - 소켓과 결합된 TCP 포트 상태를 LISTENING으로 변경 
  - 클라이언트로부터의 연결 요청을 기다린다. 
  - 연결 요청 대기 큐의 길이를 정한다.

```c
int listen(int socketfd, int queueLength)
//spcketfd에 대한 연결 요청을 기다린다. 성공하면 0을 실패하면 -1을 리턴
```

- A **server** announces that it is willing to accept connect requests. 
- **queueLength** : maximum number of pending connections on a socket 
  - 연결 설정을 기다리고 있는 요청의 최대 개수

- backlog specifies how many connection requests can be queued. (**queue size**) 
- listen(serverFd, 5);



<br>

## 소켓에 연결 요청

-  connect() 시스템 호출 
   -  서버에게 접속하여 TCP 프로토콜 수준의 연결 설정 
   -  클라이언트 측의 Local IP, port 번호 설정 
   -  fd가 나타내는 클라이언트 소켓과 address가 나타내는 서버 소켓과의 연결을 요청한다. 
   -  성공하면 fd를 서버 소켓과의 통신에 사용할 수 있다. 


```c
int connect(int fd, struct sockaddr* address, int addressLen)
//성공하면 0을 실패하면 -1를 리턴한다.
```

- Client requests a connection to server 
  - Before exchanging data, we need to create a connection between the socket of client and server. 
  - addrress is **the address of the server**.

<br>

## 연결 요청 수락

```c
int accept(int fd, struct sockaddr* address, int* addressLen)
//성공하면 새로 만들어진 복사본 소켓의 파일 디스크립터, 실패하면 -1을 리턴
```

- 접속한 클라이언트와 통신할 수 있도록 새로운 소켓을 생성하여 리턴 
- 접속한 클라이언트의 IP 주소와 포트 번호를 알려줌 
- 서버가 클라이언트로부터의 연결요청을 수락하는 내부 과정
- 데이터를 주고 받을 준비 완료

1.  서버는 fd가 나타내는 서버 소켓을 경청하고 
2. 클라이언트의 연결 요청이 올 때까지 기다린다. 
3. <mark>클라이언트로부터 연결 요청이 오면 원래 서버 소켓과 같은 복사본 소켓을 만들어 </mark>이 **복사본 소켓**과 클라이언트 소켓을 연결 
4. 연결이 이루어지면 address는 **클라이언트 소켓의 주소로 세팅**되고 addressLen는 그 크기로 세팅
5. **새로 만들어진 복사본 소켓의 파일 디스크립터를 리턴**

<br>

## send()

```c
#include <sys/socket.h>

ssize_t send(int sockfd, const void *buf, size_t nbytes, int flags);
//Returns: number of bytes sent if OK, -1 on error 
```

- Send data to other process. 
  - is **similar** to **write**, but 
  - allows us to specify flags to change how the data we want to transmit is treated 
    - MSG_DONTROUTE, MSG_DONTWAIT, MSG_EOR, MSG_OOB

<br>

## receive()

```c
#include <sys/socket.h>

ssize_t recv(int sockfd, void *buf, size_t nbytes, int flags);
//Returns: length of message in bytes,0 if no messages are available and peer has done an orderly shutdown, or -1 on error 
```

- Receive data from other process. 
  - is **similar** to **read**, but 
  - allows us to specify some options to control how we receive the data. 
    - MSG_OOB, MSG_PEEK, MSG_TRUNC, MSG_WAITALL

<br>

## EX)대문자 변환 서버

분산 애플리케이션의 예

- 이 프로그램 
  - 입력받은 문자열을 소문자를 대문자로 변환한다. 
  - 서버와 클라이언트로 구성된다. 
- 서버 
  - 소켓을 통해 클라이언트로부터 받은 문자열을 소문자를 대문자로 변환하여 소켓을 통해 클라이언트에 다시 보낸다. 
- 클라이언트 
  - 표준입력으로부터 문자열을 입력받아 
  - 이를 소켓을 통해 서버에 보낸 후에 
  - 대문자로 변환된 문자열을 다시 받아 표준출력에 출력

<br>

## cserver.c

```c
#include <stdio.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/un.h>
#define DEFAULT_PROTOCOL 0
#define MAXLINE 100
/* 소문자를 대문자로 변환하는 서버 프로그램 */
int main ()
{
    int listenfd, connfd, clientlen;
    char inmsg[MAXLINE], outmsg[MAXLINE];
    struct sockaddr_un serverUNIXaddr, clientUNIXaddr; //UNIX 소켓 구조체

    signal(SIGCHLD, SIG_IGN);
    clientlen = sizeof(clientUNIXaddr);

    listenfd = socket(AF_UNIX, SOCK_STREAM, DEFAULT_PROTOCOL);
    serverUNIXaddr.sun_family = AF_UNIX; //소켓 타입 지정
    
    strcpy(serverUNIXaddr.sun_path, "convert");//소켓의 이름 지정 -> convert
    unlink("convert"); //웹서버에 존재하는 파일을 삭제하는데 사용합니다
    bind(listenfd, &serverUNIXaddr, sizeof(serverUNIXaddr));

    listen(listenfd, 5);//총 5개의 연결 요청까지 받을 수 있다, 클라이언트의 연결 요청을 기다린다.

    while (1) { /* 소켓 연결 요청 수락 */
        connfd = accept(listenfd, &clientUNIXaddr, &clientlen);//클라이언트 측 주소 받음
        if (fork ( ) == 0) { //복사본 소켓 만들기
            /* 소켓으로부터 한 줄을 읽어 대문자로 변환하여 보냄 */
            readLine(connfd, inmsg); //connfd로 받은 문자열 inmsg에 복사
            toUpper(inmsg, outmsg); //결과 -> outmsg
            write(connfd, outmsg, strlen(outmsg)+1); // connfd로 내보내기
            close(connfd);
            exit (0);
        } else close(connfd);
    }
}
/* 소문자를 대문자로 변환 */
toUpper(char* in, char* out)
{
    int i;
    for (i = 0; i < strlen(in); i++)
        if (islower(in[i]))
            out[i] = toupper(in[i]);
    else out[i] = in[i];
    out[i] = NULL;
}

/* 한 줄 읽기 */
readLine(int fd, char* str)
{
    int n;
    do {
        n = read(fd, str, 1);
    } while(n > 0 && *str++ != NULL);
    return(n > 0);
} 
```

<br>

## cclient.c

```c
#include <stdio.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/un.h>
#define DEFAULT_PROTOCOL 0
#define MAXLINE 100

/* 소문자-대문자 변환: 클라이언트 프로그램 */
int main ( )
{
    int clientfd, result;
    char inmsg[MAXLINE], outmsg[MAXLINE];
    struct sockaddr_un serverUNIXaddr;

    clientfd = socket(AF_UNIX, SOCK_STREAM, DEFAULT_PROTOCOL);
    serverUNIXaddr.sun_family = AF_UNIX;
    strcpy(serverUNIXaddr.sun_path, "convert")
        
        do { /* 연결 요청 */
            result = connect(clientfd, &serverUNIXaddr, sizeof(serverUNIXaddr));
            if (result == -1) sleep(1);
        } while (result == -1);

    printf("변환할 문자열 입력:\n");
    fgets(inmsg, MAXLINE, stdin);
    write(clientfd,inmsg,strlen(inmsg)+1); // 변환할 문자열 보내기(clientfd 소켓을 통해서)

    /* 소켓으로부터 변환된 문자열을 한 줄 읽어서 프린트 */
    readLine(clientfd,outmsg); // clientfd를 통해 변환된 문자열을 받아서 outmsg에 넣음
    printf("%s --> \n%s", inmsg, outmsg);
    close(clientfd); 
    exit(0); 
}
```

```shell
$ cserver &
[1] 32014
$ client
변환할 문자열 입력
Hello socket stream //사용자 입력 부분
Hello socket stream -->
HELLO SOCKET STREAM
```

