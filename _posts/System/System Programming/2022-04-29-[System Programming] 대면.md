---
layout: single
title: "[System Programming] 7장. C 표준 파일 입출력"
categories: ['System', 'System Programming']
tag: ['C', 'File I/O']
---



<br>

# 7.1 파일 및 파일 포인터

## 저수준 파일 입출력과 고수준 파일 입출력

- 저수준 파일 입출력: 유닉스 커널의 시스템 호출을 사용하여 파일 입출력을 실행하며, 특수 파일도 읽고 쓸 수 있다.
  - int fd = open (const char *path, int oflag, [ mode_t mode ]);

- 고수준 파일 입출력: 표준 입출력 라이브러리로 다양한 형태의 파일 입출력 함수를 제공한다.
  - FILE *fopen(const char *name, const char *mode)
    - 파일 구조체의 시작 주소



<br>

## 시스템 호출과 C 라이브러리 함수

- 시스템 호출(System Calls)
  - Unix 커널에 서비스 요청하는 호출
  - UNIX man의 Section 2에 설명되어 있음
  - C 함수처럼 호출될 수 있음.
  - OS가 제공하는 user interface.
    - Efficiency: 자원을 효과적으로 관리해야 throughput이 증가함
    - Convenience: Hardware로 바꾸어 줌
    - trap: user to kernal mode change
- C 라이브러리 함수(Library Functions)
  - C 라이브러리 함수는 보통 시스템 호출을 포장해 놓은 함수
  - 보통 내부에서 시스템 호출을 함
  - 1975년에 Dennis Ritchie에 의해 작성
  - ANSI C Standard Library



<br>

## Unix man

```
Section # Topic
1 Commands available to users
	실행 가능핚 프로그램이나 쉘 명령어
2 Unix and C system calls
	시스템 콜(커널에서 제공하는 기능)
3 C library routines for C programs
	라이브러리 콜(프로그램 라이브러리 기능)
4 Special file names
	특별핚 파일들(일반적 /dev 디렉토리에 있는 파일 )
5 File formats and conventions for files used by Unix
	파일 포맷과 /etc/passwd와 같은 파일 명명 규칙
6 Games
	게임
7 Word processing packages
	그 외의 여러가지 것들(매크로 패키지와 명명규칙 등을 포함
8 System administration commands and procedures
	시스템 관리 명령어(일반적으로 root 유저를 위핚 명령어)
9 커널 루틴[ 비 표준 ]

```





## 시스템 호출





## 파일





## C 언어의 파일 종류

- 텍스트 파일(text file)
  - 

- 이진 파일
  - 차이점?





## 파일 입출력



## 파일 열기

파일이 없는 경우에: read write



<br>

## 파일 입출력 모드

1. 100% 반드시 버퍼를 통해서만 read/write이 일어나야 한다.

2. 버퍼가 line 단위로 





## 표준 입력/출력/에러

File pointer를 통해 파일에 접근할 때도 결국은 file descriptor를 사용하여 접근하게 된다. 





# 7.2  텍스트 파일

## 텍스트 파일 (text file)







## fprintf

rec.name -> &rec.name







## 이진 파일

chunk

line은 고정 사이즈가 아니다.(마구 입력한다음에 엔터 쳐야 끝)

fread에서 buf로 받으면 고정된 사이즈 만을 받아야 한다.





## C 라이브러리 버퍼

하드디스크 블럭 사이즈





## 프로세스

fork를 호출할 시점과 똑같이 생긴 자식이 만들어짐



concurrency를 하기위해 백그라운드
