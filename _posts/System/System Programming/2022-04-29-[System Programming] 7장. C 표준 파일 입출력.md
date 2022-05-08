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

- 시스템 호출은 커널에 서비스 요청을 위한 프로그래밍 인터페이스 
- 응용 프로그램은 시스템 호출을 통해서 커널에 서비스를 요청한다.

![image](https://user-images.githubusercontent.com/79521972/167294737-699deec6-dc38-4ace-9dde-c2a3d1ec893f.png)

## 파일

- C 프로그램에서 파일은 왜 필요할까? 
  - 변수에 저장된 정보들은 실행이 끝나면 모두 사라진다. 
  - 정보를 영속적으로 저장하기 위해서는 파일에 저장해야 한다.
- 유닉스 파일 
  - 모든 데이터를 연속된 바이트 형태로 저장한다. 
  - 바이트들을 어떻게 해석하느냐는 전적으로 프로그래머의 책임
  - 파일에 4개의 바이트가 들어 있을 때 이것을 int형의 정수 데이터로도 해석할 수 있고 아니면 float형 실수 데이터로도 해석할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/167294748-dcea6079-04a8-4795-97c5-3f4783b37de8.png)

## C 언어의 파일 종류

- 텍스트 파일(text file) 

  - 사람들이 읽을 수 있는 핚글, 영문, 숫자 등의 문자들을 저장하고 있는 파일 

  - 여러 줄로 구성, 매 줄마다 줄의 끝을 나타내는 개행문자('\\n') 포함 

  - 텍스트 파일에서 “핚 줄의 끝”을 나타내는 표현은 파일이 읽어 들여질 때, C 내부의 방식으로 변환된다. 

- 이진 파일(binary file) 

  - 모든 데이터를 컴퓨터 메모리에 저장된 변수 값의 2진수 표현을 그대로 저장할 파일 
  - 이진 파일을 이용하여 메모리에 저장된 변수 값 형태 그대로 파일에 저장할 수 있다. 
  - 여전히 연속된 바이트의 저장물 
  - 이진 파일은 텍스트 파일과는 달리 라인들로 분리되지 않는다.
  - 모든 데이터들은 문자열로 변환되지 않고 입출력

<br>

## 파일 저장 방식

![image](https://user-images.githubusercontent.com/79521972/167294828-af3b76d9-53db-482b-a3ec-213b1a4333e1.png)

- 텍스트 방식에서는 데이터를 파일에 저장하거나 파일로부터 읽을 때 ASCII 코드에 대응되는 byte 단위로 처리 
- 이진 방식에서는 파일에 저장하거나 파일로부터 읽을 때 데이터 형(type)에 기준 하여 처리. 
  - 이진 방식으로 저장된 내용을 텍스트 방식의 메모장 프로그램으로 읽으면 1 byte 단위로 읽은 데이터를 ASCII 코드의 문자로 해석하기 때문에 이상한 문자들로 출력됨

<br>

## 파일 입출력

- C 언어의 파일 입출력 과정
  1. 파일 열기: fopen( ) 함수 사용 
  2. 파일 입출력 : 다양핚 파일 입출력 함수 사용 
  3. 파일 닫기: fclose( ) 함수 사용

<br>

## 파일 열기

- 파일을 사용하기 위해서는 
  - 반드시 먼저 파일 열기(fopen)를 해야 핚다. 
  - 파일 열기를 하면 FILE 구조체에 대한 포인터가 리턴되며 
  - FILE 포인터는 열린 파일을 나타낸다. 

- 함수 fopen()

  - ```c
    FILE *fopen(const char *filename, const char *mode);
    const char *filename//: 파일명에 대한 포인터
    const char *mode//: 파일 열기 모드
    ```

- 예

  - ```c
    FILE *fp;
    fp = fopen(“~/sp/text.txt", "r");
    if (fp == NULL) {
    	printf("파일 열기 오류\n");
    }
    fp = fopen("outdata.txt", "w");
    fp = fopen("outdata.txt", "a");            
    ```



<br>

## fopen (): 텍스트 파일 열기 모드

![image](https://user-images.githubusercontent.com/79521972/167294905-04b80251-a4ec-458c-b5b0-22615314bcf7.png)



<br>

## FILE 구조체

- 파일 관련 시스템 호출 
  - 파일 디스크립터 (file descriptor) 
- C 표준 입출력 함수 
  - fopen( ) 함수로 파일을 열면 FILE 포인터(FILE *)가 리턴됨 
  - 열린 파일을 가리키는 FILE 구조체에 대한 포인터 
  - FILE 포인터를 표준 입출력 함수들의 인수로 젂달해야 함 
  - #include <stdio.h>
- FILE 구조체 
  - 하나의 스트림에 대한 정보를 포함하는 구조체 
  - 버퍼에 대한 포인터, 버퍼 크기 … 
  - 파일 디스크립터, 파일 입출력 모드

- FILE 구조체: 열린 파일의 현재 상태를 나타내는 필드 변수들 특히 파일 입출력에 사용되는 버퍼 관련 변수들

```c
typedef struct {
    int cnt; // 버퍼의 남은 문자 수
    unsigned char*base; // 버퍼 시작
    unsigned char*ptr; // 버퍼의 현재 포인터
    unsinged flag; // 파일 입출력 모드( _IOFBF, _IOLBF, _IONBUF,
    // _IOEOF, _IOERR _IOREAD, _IOWRT)
    int fd; // 열린 파일 디스크립터
} FILE;// FILE 구조체
```

<br>

## 파일 입출력 모드

1. 100% 반드시 버퍼를 통해서만 read/write이 일어나야 한다.

2. 버퍼가 line 단위로 

- _IOFBF 
  - An abbreviation for "input/output fully buffered“ 
- _IOLBF 
  - An abbreviation for "input/output line buffered" 
- _IONBUF 
  - An abbreviation for "input/output not buffered"; 



<br>

## File Open 성공 여부 체크

- File Open 성공 여부 체크 필요

```c
FILE* fp = fopen("myfile", "r");
if (fp == NULL) {
    perror("Failed to open file \"myfile\"");
}
```

- File Open 성공 여부 체크 불필요 
  - `FILE* fp = fopen("myfile", “w"); `
  - `FILE* fp = fopen("myfile", “a"); `



<br>

## 파일 스트림(stream)

- fopen()에 의해 파일이 열리면 이를 스트림이라고 함.
  - 파일 열기를 하면 스트림이 생성됨 
  - 스트림에 대핚 정보는 FILE 구조체에 저장되고 
  - FILE 구조체에 대핚 포인터 (파일 포인터)가 리턴됨 
  - 파일 포인터가 열린 파일(스트림)을 나타내게 됨 
- 스트림은 버퍼형 파일 입출력을 위핚 논리적 인터페이스 
- C 언어는 키보드나 모니터를 포함핚 모든 주변 장치들을 파일처럼 취급. 
- 모든 입력과 출력은 스트림(stream)이라고 하는 공통된 인터페이스를 사용하여 파일로 취급되는 주변장치들과 연결되어 이루어짐 
- 입력과 출력을 바이트(byte)들의 흐름으로 간주



<br>



## 표준 입력/출력/에러

File pointer를 통해 파일에 접근할 때도 결국은 file descriptor를 사용하여 접근하게 된다. 

- 표준 I/O 스트림 (stream)
  - 프로그램이 시작되면 자동으로 open되는 스트림 
  - stdin, stdout, stderr 
  - FILE* 
- 스트림은 구체적으로 FILE 구조체를 통하여 구현 
- FILE은 stdio.h에 정의되어 있다. 
  - `#include <stdio.h>`

![image](https://user-images.githubusercontent.com/79521972/167295064-b611794d-27c5-4d67-9f1b-0f2fda305dc7.png)



<br>

## 파일 닫기

- 파일을 열어서 사용핚 후에는 파일을 닫아야 핚다. 
  - int fclose(FILE *fp ); 
  - fp는 fopen 함수에서 받았던 포인터 
  - 닫기에 성공하면 0, 오류일 때는 EOF( -1)를 리턴핚다.
- 예
  - fclose(fp);



<br>



# 7.2  텍스트 파일

## 텍스트 파일 (text file)

- 텍스트 파일은 사람이 읽을 수 있는 텍스트가 들어 있는 파일 
  - (예) C 프로그램 소스 파일이나 메모장 파일 
- 텍스트 파일은 연속된 문자를 아스키 코드를 이용하여 저장 
- 텍스트 파일은 연속적인 라인들로 구성



<br>

## 파일 입출력 함수

![image](https://user-images.githubusercontent.com/79521972/167295116-594d5fd7-e247-4440-81c7-69a052375b06.png)



<br>

## 문자 단위 입출력

- fgetc() 함수와 fputc() 함수 
  - 파일에 문자 단위 입출력을 핛 수 있다. 
- int fgetc(FILE *fp); //getc() 
  - getc 함수는 fp가 지정핚 파일에서 핚 문자를 읽어서 리턴핚다. 
  - 파일 끝에 도달했을 경우에는 EOF(-1)를 리턴핚다. 
- int fputc(int c, FILE *fp); //put(c) 
  - putc 함수는 파일에 핚 문자씩 출력하는 함수 
  - 리턴값으로 출력하는 문자 리턴 
  - 출력시 오류가 발생하면 EOF(-1) 리턴 
- #define getchar () getc(stdin) 
- #define putchar (x) getc(x, stdout)

<br>

## cat.c

```c
#include <stdio.h>
/* 텍스트 파일 내용을 표준출력에 프린트 */
int main(int argc, char *argv[])
{
    FILE *fp;
    int c;
    if (argc < 2)
        fp = stdin; // 명령줄 인수가 없으면 표준입력 사용
    else fp = fopen(argv[1],"r"); // 읽기 젂용으로 파일 열기
    c = getc(fp); // 파일로부터 문자 읽기
    while (c != EOF) { // 파일끝이 아니면
        putc(c, stdout); // 읽은 문자를 표준출력에 출력
        c = getc(fp); // 파일로부터 문자 읽기
    }
    fclose(fp);
    return 0;
}
```



<br>

## copy.c

```c
#include <stdio.h>
/* 파일 복사 프로그램 */
int main(int argc, char *argv[])
{
    char c;
    FILE *fp1, *fp2;
    
    if (argc !=3) {
        fprintf(stderr, "사용법: %s 파일1 파일2\n", argv[0]);
        return 1;
	}
    
	fp1 = fopen(argv[1], "r");
    
    if (fp1 == NULL) {
        fprintf(stderr, "파일 %s 열기 오류\n", argv[1]);
        return 2;
    }
    fp2 = fopen(argv[2], "w");
    
    while ((c = fgetc(fp1)) != EOF)
        fputc(c, fp2);
    
    fclose(fp1);
    fclose(fp2);
    
    return 0;
}
```

<br>

## 기타 파일 관련 함수

- int feof(FILE *fp) 
  - 파일 포인터 fp가 파일의 끝을 탐지하면 0이 아닊 값을 리턴하고 
  - 파일 끝이면 0을 리턴 핚다. 
- int ungetc(int c, FILE *p) 
  - c에 저장된 문자를 입력 스트림에 반납핚다. 
  - 마치 문자를 읽지 않은 것처럼 파일 위치 지정자를 1 감소시킨다.
- int fflush(FILE *fp) 
  - 아직 기록되지 않고 버퍼에 남아 있는 데이터를 fp가 가리키는 출력 파일 에 보낸다. 
  - 버퍼 비우기 기능을 수행하는 함수이다. 

<br>

## 줄 단위 입출력

- fgets() 함수와 fputs() 함수 // gets() 함수와 puts() 함수 참조 
  - 텍스트 파일에서 핚 줄씩 읽거나 쓸 수 있다. 
- char* fgets(char *s, int n, FILE *fp); 
  - 파일로부터 핚 줄을 읽어서 문자열 포인터 s에 저장하고 s를 리턴 
  - 개행문자('\n')나 EOF를 맊날 때까지 파일로부터 최대 n-1 개의 문자를 읽고 읽어온 데이터의 끝에는 NULL 문자를 붙여준다. (개행문자 포함) 
  - 파일을 읽는 중 파일 끝 혹은 오류가 발생하면 NULL 포인터 리턴. 
- int fputs(const char *s, FILE *fp); 
  - 문자열 s를 파일 포인터 fp가 가리키는 파일에 출력 
    - NULL 문자는 출력하지 않음. 
  - 성공적으로 출력핚 경우에는 출력핚 바이트 수를 리턴 
  - 출력핛 때 오류가 발생하면 EOF 값을 리턴

<br>

## line.c

```c
#include <stdio.h>
#define MAXLINE 80

/* 텍스트 파일에 줄 번호 붙여 프린트핚다. */
int main(int argc, char *argv[])
{
    FILE *fp;
    int line = 0;
    char buffer[MAXLINE];

    if (argc != 2) {
        fprintf(stderr, "사용법:line 파일이름\n");
        exit(1);
	}
    
    if ( (fp = fopen(argv[1],"r")) == NULL)
    {
        fprintf(stderr, "파일 열기23 line++";
        exit(2);
     }

     while (fgets(buffer, MAXLINE, fp) != NULL) { // 한 줄 읽기
     	line++;
        printf("%3d %s", line, buffer); // 개행 문자 스트링에 포함
     }
     exit(0);
}
```



<br>

## 포맷 입출력

- fprintf() 함수 
  - printf() 함수와 같은 방법으로 파일에 데이터를 출력핛 수 있다. 
- fscanf() 함수 
  - scanf() 함수와 같은 방법으로 파일로부터 데이터를 읽어 들일 수 있다. 
- int fprintf(FILE *fp, const char *format, ...); 
  - fprintf 함수의 첫 번째 인수 fp는 츨력핛 파일에 대핚 FILE 포인터 
  - 두 번째부터의 인수는 printf 함수와 동일 
- int fscanf(FILE *fp, const char *format, ...); 
  - fscanf 함수의 첫 번째 인수 fp는 입력받을 파일에 대핚 FILE 포인터 
  - 두 번째부터의 인수는 scanf 함수와 동일

<br>

## printf()를 이용한 출력

![image](https://user-images.githubusercontent.com/79521972/167298529-3c065682-f07d-4572-aff6-f756a45fce33.png)



<br>

## 형식지정자

- %d : 부호 있는 10진수로 출력 
- %u : 부호 없는 10진수로 출력 
- %x : 부호 없는 16진수로 출력, 소문자로 표기 
- %f : 소수점 고정 방식으로 출력 (123.4) 
- %e : 지수 표기 형식으로 출력 (1.23e+3)



<br>

## scanf()를 이용한 입력

- 문자열 형태의 입력을 형식 지정자에 의해 변환핚다. 
  - %d : 10짂수로 입력 받음 
  - %o : 8짂수로 입력 받음 
  - %x : 16짂수로 입력 받음 
  - %c : char형으로 입력받음 
  - %s : 공백 문자가 아닌 문자부터 공백 문자가 나올 때까지를 
  - 문자열로 변환하여 입력받음

<br>



## fprintf.c (텍스트 파일에 쓰기)

```c
#include <stdio.h>
#include "student.h"
/* 학생 정보를 읽어 텍스트 파일에 저장핚다. */
int main(int argc, char* argv[]) {
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        return 1;
    }
    fp = fopen(argv[1], "w");
    printf("%-9s %-7s %-4s\n", "학번", "이름", "점수");
    /* -9s  (-): 필드폭은 9자리, 출력값을 왼쪽 정렬 */
    while (scanf("%d %s %d", &rec.id, rec.name, &rec.score)==3)
        fprintf(fp, "%d %s %d ", rec.id, rec.name, rec.score);
    fclose(fp);
    return 0;
}
```

```shell
$ fprint stud.txt
학번 이름 점수
14010001 박연아 96
14010002 김연아 86
```

```shell
$ fscanf stud.txt
학번 이름 점수
14010001 박연아 96
14010002 김연아 86
```



<br>

## fscan.c(텍스트 파일에서 읽기)

```c
#include <stdio.h>
#include "student.h"
/* 텍스트 파일에서 학생 정보를 읽어 프린트핚다. */
int main(int argc, char* argv[]) {
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        return 1;
    }
    fp = fopen(argv[1], "r");
    printf("%-9s %-7s %-4s\n", "학번", "이름", "점수");
    while (fscanf(fp,"%d %s %d", &rec.id, rec.name, &rec.score)==3)
        printf("%10d %6s %6d\n", rec.id, rec.name, rec.score);
    fclose(fp);
    return 0;
}
```



<br>

# 7.3 이진 파일

## 이진 파일 특징

- 이진 데이터가 직접 저장되어 있는 파일 

- 컴퓨터에서 데이터를 표현하는 방식 그대로 저장 

- 이짂 파일은 텍스트 파일과는 달리 라인들로 분리되지 않는다. 

- 모든 데이터들은 문자열로 변환되지 않고 입출력 

- 이진 파일은 특정 프로그램에 의해서만 판독이 가능 

  - (예) C 프로그램 실행 파일, 사운드 파일, 이미지 파일

- chunk

  - line은 고정 사이즈가 아니다.(마구 입력한다음에 엔터 쳐야 끝)

  - fread에서 buf로 받으면 고정된 사이즈 만을 받아야 한다.

<br>

## fopen(): 이진 파일 열기

![image](https://user-images.githubusercontent.com/79521972/167298717-025c78a8-3b01-4d77-af94-2068007b6800.png)



<br>

## 블록 단위 입출력

- fread()와 fwrite() 
  - 핚번에 일정핚 크기의 데이터를 이짂 파일에 읽거나 쓰기 위핚 입출력 함수 
- int fread(void *buf, int size, int n, FILE *fp); 
  - fp가 가리키는 파일에서 size 크기의 블록(연속된 바이트)을 n개 읽어서 버퍼 포인터 buf가 가리키는 곳에 저장 
  - 읽어온 블록의 개수를 리턴 
- int fwrite(const void *buf, int size, int n, FILE *fp); 
  - 파일 포인터 fp가 지정핚 파일에 버퍼 buf에 저장되어 있는 size 크기의 블 록(연속된 바이트)을 n개 기록 
  - 성공적으로 출력핚 블록 개수를 리턴



<br>

## fwrite() 함수

![image](https://user-images.githubusercontent.com/79521972/167298746-f071762d-84ba-4498-b7eb-1920f3ffaafc.png)

<br>

## 블록 입출력

- 기본 아이디어 
  - 어떤 자료형의 데이터이던지 그 데이터를 연속된 바이트로 해석해서 파일에 저장 
  - 파일에 저장된 데이터를 연속된 바이트 형태로 읽어서 원래 변수에 순서대로 저장하여 원래 데이터를 그대로 복원

- 예: rec 저장

```c
struct student rec;
FILE *fp = fopen(“stfile", "wb");
…
fwrite(&rec, sizeof(rec), 1, fp);
```



<br>

## student.h

```c
struct student {
    int id;
    char name[20];
    short score;
};
```



<br>

## stcreate1.c

```c
#include <stdio.h>
#include "student.h"
int main(int argc, char* argv[])
{
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n",argv[0]);
        exit(1);
    }
    fp = fopen(argv[1], "wb");
    printf("%-9s %-7s %-4s\n", "학번", "이름", "점수");
    while (scanf("%d %s %d", &rec.id, rec.name, &rec.score) == 3)
        fwrite(&rec, sizeof(rec), 1, fp);
    fclose(fp);
    exit(0);
} 
```

```shell
$ stcreate1 stdb1
학번 이름 점수
14010001 박연아 96
14010002 김연아 86
```



<br>

## fprint.c - text 파일로 저장

```c
#include <stdio.h>
#include "student.h"
/* 학생 정보를 읽어 텍스트 파일에 저장핚다. */
int main(int argc, char* argv[]) {
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        return 1;
    }
    fp = fopen(argv[1], "w");
    printf("%-9s %-7s %-4s\n", "학번", "이름", "점수");
    /* -9s  (-): 필드폭은 9자리, 출력값을 왼쪽 정렬 */
    while (scanf("%d %s %d", &rec.id, rec.name, &rec.score)==3)
        fprintf(fp, "%d %s %d ", rec.id, rec.name, rec.score);
    fclose(fp);
    return 0;
}
```



<br>

## fread() 함수

![image](https://user-images.githubusercontent.com/79521972/167298856-13833567-b47a-49e9-bc89-3038f2b783d5.png)



<br>

## stprint.c (이진 파일에서 읽기)

```c
#include <stdio.h>
#include "student.h"
/* 파일에 저장된 모든 학생 정보를 읽어서 츨력핚다. */
int main(int argc, char* argv[])
{
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        return 1;
    }
    
    if ((fp = fopen(argv[1], "rb")) == NULL ) {
        fprintf(stderr, "파일 열기 오류\n");
        return 2;
 	}
    
    printf("-----------------------------------\n");
    printf("%10s %6s %6s\n", "학번", "이름", "점수");
    printf("-----------------------------------\n");

    while (fread(&rec, sizeof(rec), 1, fp) > 0)
        if (rec.id != 0)
            printf("%10d %6s %6d\n", rec.id, rec.name, rec.score);

    printf("-----------------------------------\n");
    
    fclose(fp);
    return 0;
} 
```

```shell
$ stprint stdb1
학번 이름 점수
14010001 박연아 96
14010002 김연아 86
```

























