---
layout: single
title: "[System Programming] 7-2장. C 임의 접근"
categories: ['System', 'System Programming']
tag: ['C', 'File I/O']
---

<br>

# 7.4 임의 접근

sequential access vs. random access

전 시간에 배운 것이 sequential access

## 파일 내 위치

- 현재 파일 위치(current file position) 
  - 열린 파일에서 다음 읽거나 기록한 파일 내 위치 
- 파일 위치 포인터(file position pointer) 
  - 시스템 내에 그 파일의 현재 파일 위치를 저장하고 있다.

![image](https://user-images.githubusercontent.com/79521972/167298990-62b3a94e-b63d-480d-8448-6221d9af959e.png)



<br>

## 파일 위치 관련 함수

- REVIEW)
- `off_t lseek (int fd, off_t offset, int whence);` // **off_t: file size in bytes**
  - 이동에 성공하면 현재 위치(measured in bytes from the beginning of the file)를 리턴하고 실패하면 -1을 리턴한다.

<br>

- `int fseek(FILE *fp, long offset, int mode)`
  - 파일 위치 포인터를 임의로 설정할 수 있는 함수
- `void rewind(FILE *fp)`
  - 현재 파일 위치를 **파일 시작**에 위치시킴.
- `long ftell(FILE *fp)`
  - 파일의 현재 파일 위치를 나타내는 파일 위치 지정자 값 리턴

<br>

## fseek() 함수

- `fseek(FILE *fp, long offset, int mode)`
  - 파일이 binary mode로 open 된 경우
    - FILE 포인터 fp가 가리키는 파일의 현재 파일 위치(currnet file position)를 모드(mode) 기준으로 오프셋(offset)만큼 옮긴다.

![image](https://user-images.githubusercontent.com/79521972/167299147-8d7fba7d-3f72-47c6-9b41-cc9b6f89c408.png)

![image](https://user-images.githubusercontent.com/79521972/167299155-3fb4d21f-e43b-44ef-9991-e3ff9f206dc3.png)



<br>

## fseek() 함수

- 파일 위치 이동
  - `fseek(fp, 0L, SEEK_SET);`             파일 시작으로 이동(rewind) 
  - `fseek(fp, 100L, SEEK_SET);`         파일 시작에서 100바이트 위치로 
  - `fseek(fp, 0L, SEEK_END);`              파일 끝으로 이동(append)

- 레코드 단위로 이동 
  - `fseek(fp, n * sizeof(record), SEEK_SET);` 		n+1번째 레코드 시작위치로 
  - `fseek(fp, sizeof(record), SEEK_CUR);`              다음 레코드 시작위치로 
  - `fseek(fp, -sizeof(record), SEEK_CUR);`             전 레코드 시작위치로

-  파일끝 이후로 이동 
  - `fseek(fp, sizeof(record), SEEK_END);`    파일끝에서 한 레코드 다음 위치로 이동



<br>

## 파일 끝 이후로 이동: 예

```c
fwrite(&record1, sizeof(record), 1, fp);
fwrite(&record2, sizeof(record), 1, fp);
fseek(fd, sizeof(record), SEEK_END);
fwrite(&record3, sizeof(record), 1, fp);
```

![image](https://user-images.githubusercontent.com/79521972/167299289-4ee239a3-bd42-4b06-b8f6-1b81485f4d95.png)

- 텍스트 파일이나 이진 파일은 쓰는 순서대로 순차적으로 저장됨 
- 읽기를 할 때 레코드 단위로 검색/읽기를 위해서 저장 시에 순차적이 아닌 저장 장소 지정이 필요 
- 앞에서 공부한 다음 두 프로그램 참조 
  - fprint.c (텍스트 파일에 쓰기) 및 stcreate1. c (이진 파일에 쓰기)




<br>

## stcreate2.c - 레코드 형식으로 저장

```c
#include <stdio.h>
#include "student.h"
#define START_ID 1201001
/* 구조체를 이용하여 학생 정보를 파일에 저장핚다. */
int main(int argc, char* argv[])
{
    struct student rec;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        exit(1);
 	}
    fp = fopen(argv[1], "wb");
    printf("%7s %6s %4s\n", "학번", "이름", "점수");
    while (scanf("%d %s %d", &rec.id, rec.name, &rec.score) == 3) {
        fseek(fp, (rec.id – START_ID)* sizeof(rec), SEEK_SET);
        fwrite(&rec, sizeof(rec), 1, fp);
    }
    fclose(fp);
    exit(0);
}
```

```shell
$ stcreate2 stdb2
학번 이름 점수
14010001 박연아 96
14010002 김연아 86
----
```



<br>

## stquery.c

```c
#include <stdio.h>
#include "student.h"
int main(int argc, char *argv[])
{
    struct student rec;
    char c;
    int id;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        exit(1);
    }
    if ((fp = fopen(argv[1], "rb")) == NULL ) {
        fprintf(stderr, "파일 열기 오류\n");
        exit(2);
    }
    do {
        printf("검색할 학생의 학번 입력: ");
        if (scanf("%d", &id) == 1) {
            fseek(fp, (id - START_ID) *sizeof(rec), SEEK_SET);
            if ((fread(&rec, sizeof(rec), 1, fp) > 0) && (rec.id != 0))
                printf("학번: %8d 이름: %4s 점수: %4d\n", rec.id, rec.name, rec.score);
            else printf("레코드 %d 없음\n", id);
        }
        else printf("입력 오류");
        printf("계속하겠습니까?(Y/N)");
        scanf(" %c", &c);
    } while (c == 'Y');
    fclose(fp);
    exit(0);
}
```

```shell
$ stquery stdb2
검색할 학생의 학번 입력: 1401003
학번: 1401003 이름: 김연아 점수: 90
계속하겠습니까?(Y/N) Y
검색할 학생의 학번 입력: 1401006
학번: 1401006 이름: 박연아 점수: 80
계속하겠습니까?(Y/N) N
$
```



<br>

## 레코드 수정 과정

(1) 파일로부터 해당 레코드를 읽어서 

(2) 이 레코드를 수정한 후에 

(3) 수정된 레코드를 다시 파일 내의 원래 위치에 써야 한다

즉, 읽기와 쓰기가 동시에

![image](https://user-images.githubusercontent.com/79521972/167299391-74ea61aa-ff0a-4749-9ca4-bb09bc89cf45.png)



<br>

## stupdate.c

```c
#include <stdio.h>
#include "student.h"
/* 파일에 저장된 학생 정보를 수정한다. */
int main(int argc, char *argv[])
{
    struct student rec;
    int id;
    char c;
    FILE *fp;
    if (argc != 2) {
        fprintf(stderr, "사용법: %s 파일이름\n", argv[0]);
        exit(1);
    }
    
    if ((fp = fopen(argv[1], "rb+")) == NULL) {
        fprintf(stderr, "파일 열기 오류\n");
        exit(2);
    }
    
    do {
        printf("수정핛 학생의 학번 입력: ");
        if (scanf("%d", &id) == 1) {
            fseek(fp, (id - START_ID) * sizeof(rec), SEEK_SET);
            
            if ((fread(&rec, sizeof(rec), 1, fp) > 0)&&(rec.id != 0)) {
                printf("학번: %8d 이름: %4s 점수: %4d\n", rec.id, rec.name, rec.score);
                printf("새로운 점수 입력: ");
                scanf("%d", &rec.score);
                fseek(fp, -sizeof(rec), SEEK_CUR);
                fwrite(&rec, sizeof(rec), 1, fp);
            } else printf("레코드 %d 없음\n", id);
        } else printf("입력오류\n");
        
        printf("계속하겠습니까?(Y/N)");
        scanf(" %c",&c);
    } while (c == 'Y');
    
    fclose(fp);
    exit(0);
}
```

```shell
$ stupdate stdb2
수정할 학생의 학번 입력: 1401003
학번: 1401003 이름: 김연아 점수: 90
새로운 점수 입력: 95
계속하겠습니까?(Y/N) N
```



<br>

# 7.5 버퍼링

## C 라이브러리 버퍼

- C 라이브러리 버퍼 사용 **목적** 
  - 디스크 I/O 수행의 최소화 
    - fget(), fputc() 호출마다 디스크 I/O 수행은 부적절(시간이 굉장히 많이 걸리기 때문에)
    - read(), write() 함수 호출의 최소화 
  - 최적의 크기 단위로 I/O 수행 
  - 시스템 성능 향상 
  - 그래서 putc로 모으고 모아서 한 번에 write 하자는 아이디어
- C 라이브러리 버퍼 방식 
  - 완전 버퍼(fully buffered) 방식 
  - 줄 버퍼(line buffered) 방식 
  - 버퍼 미사용(unbuffered) 방식



<br>

## C 라이브러리 버퍼 방식

- **완전 버퍼 방식** 
  - 버퍼가 **꽉 찼을 때** 실제 I/O 수행 
  - 디스크 파일 입출력 -–블록 단위 
- **줄 버퍼 방식** 
  - **줄 바꿈 문자(newline)에서** 실제 I/O 수행
  - 터미널 입출력 (stdin, stdout) 
- **버퍼 미사용 방식** 
  - 버퍼를 사용하지 않는다.
  - 표준 에러 (stderr)



<br>

## buffer.c

```c
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char *argv[])
{
    FILE *fp;
    if (!strcmp(argv[1], "stdin")) { // 표준 입력
        fp = stdin;
        printf("한 글자 입력:");
        if (getchar() == EOF) perror("getchar"); //파일 끝 도달
    }
    else if (!strcmp(argv[1], "stdout")) // 표준 출력
        fp = stdout;
    else if (!strcmp(argv[1], "stderr")) // 표준 에러
        fp = stderr;
    else if ((fp = fopen(argv[1], "r")) == NULL) { //일반 파일
        perror("fopen");
        exit(1);
    }
    else if (getc(fp) == EOF) perror("getc");
    printf("스트림 = %s, ", argv[1]);
    
    if (fp->_flags & _IO_UNBUFFERED)
        printf("버퍼 미사용");
    else if (fp->_flags & _IO_LINE_BUF)
        printf("줄 버퍼 사용");
    else
        printf("완전 버퍼 사용");
    
    printf(", 버퍼 크기 = %d\n", fp->_IO_buf_end - fp->_IO_buf_base);
    // FILE 구조체 참조
    exit(0);
}
```

```shell
$ buffer stdin
한 글자 입력:x
스트림 = stdin, 줄 버퍼 사용, 버퍼 크기 = 1024
$ buffer stdout
스트림 = stdout, 줄 버퍼 사용, 버퍼 크기 = 1024
$ buffer stderr
스트림 = stderr, 버퍼 미사용, 버퍼 크기 = 1
$ buffer /etc/passwd
스트림 = /etc/passwd, 완전 버퍼 사용, 버퍼 크기 = 4096
```



<br>

## setbuf()/setbuf()

```c
#include <stdio.h>
void setbuf (FILE *fp, char *buf );
int setvbuf (FILE *fp, char *buf, int mode, size_t size );
```

- 버퍼의 관리 방법을 변경한다 .
- 호출 시기 
  - 스트림이 오픈된 후, 
  - 입출력 연산 수행 전에 호출되어야 함



<br>

## setbuf()

`void setbuf(FILE *fp, char *buf)`

- 버퍼 사용을 on/off 할 수 있다. 
- fp는 열린 파일(스트림을 지칭) 
- buf 가 NULL 이면 버퍼 미사용 방식 
- char *buf: buf의 시작주소
- buf 가 BUFSIZ크기의 공간을 가리키면 완전/줄 버퍼 방식 
  - **BUFSIZ**는 \<stdio.h> 에 정의된 버퍼 사이즈 
  - 터미널 장치면 줄 버퍼 방식 
  - 그렇지 않으면 완전 버퍼 방식

<br>

## 예제: setbuf

```c
#include <stdio.h>
main()
{
    printf("안녕하세요, "); sleep(1); // printf(" \n");수행후 출력
    printf("리눅스입니다!"); sleep(1); // printf(" \n");수행후 출력 
    printf(" \n"); sleep(1);
    setbuf(stdout, NULL); // 버퍼 미사용 설정
    printf("여러분, "); sleep(1); // 바로 출력 (버퍼 미사용)
    printf("반갑습니다"); sleep(1);
    printf(" ^^"); sleep(1);
    printf(" \n"); sleep(1);
}
```

```shell
$ setbuf
안녕하세요, 리눅스입니다!
여러분, 반갑습니다^^
```

\n이 입력 되면 그 전에 있던 거와 한 꺼번에 출력이 됨(줄버퍼 사용 때문에)

- 줄버퍼: 버퍼가 꽉 차지 않더라도 \n에 출력

버퍼 미사용 설정 이후에는 출력이 바로바로 되게 된다.

<br>

## setvbuf()

`int setvbuf(FILE *fp, char *buf, int mode, size_t size()`

- 버퍼 사용 방법을 변경 
- 리턴 값: 성공하면 0, 실패하면 nonzero 
- mode
  - _IOFBF : 완전 버퍼 방식 
  - _IOLBF : 줄 버퍼 방식 
  - _IONBF : 버퍼 미사용 방식

explicitly 명시하기 때문에 훨씬 더 좋다.

<br>

- mode == _IONBF 
  - buf 와 size 는 무시됨  (버퍼 미사용이기 때문)
- mode == _IOLBF or _IOFBF 
  - buf 가 NULL이 아니면 
    - buf 에서 size 만큼의 공간 사용 
- NULL이면 라이브러리가 알아서 적당한 크기 할당 사용
  - stat 구조체의 st_blksize 크기 할당 (disk files) 
  - st_blksize 값을 알 수 없으면 BUFSIZ 크기 할당 (pipes)



<br>

## 예제:setvbuf

```c
#include <stdio.h>

int main( void )
{
     char buf[1024];
     FILE *fp1, *fp2;
     fp1 = fopen("data1", "a");
     fp2 = fopen("data2", "w");
     if( setvbuf(fp1, buf, _IOFBF, sizeof( buf ) ) != 0 )
         printf("첫 번째 스트림: 잘못된 버퍼\n" );
     else
         printf("첫 번째 스트림: 1024 바이트 크기 버퍼 사용\n" );

     if( setvbuf(fp2, NULL, _IONBF, 0 ) != 0 )
         printf("두 번째 스트림: 잘못된 버퍼\n" );
     else
         printf("두 번째 스트림: 버퍼 미사용\n" );
}
```

```shell
$ setbuf
첫 번째 스트림: 1024 바이트 크기 버퍼 사용
두 번째 스트림: 버퍼 미사용
```

<br>

## fflush()

```c
#include <stdio.h>
int fflush (FILE *fp);
```

- fp 스트림의 출력 버퍼에 남아있는 모든 내용을 write() 시스템 호출을 통하여 커널에 전달한다. 
  - fp가 가리키는 함수 내용에 버퍼에 남아있는 모든 내용을 하드 디스크에 write을 요구

- 리턴 값 : 성공하면 0, 실패하면 EOF (-1) 
- fp 가 NULL이면, **모든 출력 스트림**의 출력 버퍼에 남아있는 내용을 커널에 전달한다

<br>

# 7.5 기타 함수

## 문자열 처리 함수1

- \# include  <string.h>
- char *strcpy(char *s, const char *t); 
  - Null 문자를 포함해서 문자열 t를 문자열 s에 복사하고 s를 반환 
- char *strncpy(char *s, const char *t, size_t n); 
  - 최대 n개 문자의 문자열 t를 문자열 s에 복사하고 s를 반환. 
  - t의 길이가 n보다 작으면 NULL 문자로 채운다. 
  - 만일 t의 처음 n 바이트 중 NULL이 없다면, s는 NULL 종료가 안될 수 있다. 
- char *strcat(char *s, const char *t); 
  - 문자열 t를 문자열 s에 접합하고 s를 반환 
- char *strncat(char *s, const char *t, size_t n); 
  - 최대 n개 문자의 문자열 t를 문자열 s에 접합한다. S를 NULL로 끝내고 반환한다. 
- int strcmp(const char *s, const char *t); 
  - 문자열 s를 문자열 t와 비교한다. 
  - S < t 이면 음수값, s=t면 0, s>t이면 양수값을 반환한다. 



<br>

## 문자열 처리 함수2

- int strncmp(const char *s, const char *t, size_t n); 
  - 문자열 s의 최대 n개의 문자들을 문자열 t와 비교한다. 
  - S t 이면 양수값을 반환한다. 
- char *strchr(char *s, int c); 
  - 문자열 s 내에서 처음 나타난 문자 c에 대핚 포인터를 반환한다. 없으면 NULL 반환 
- char *strrchr(char *s, int c); 
  - 문자열 s 내에서 마지막으로 나타난 문자 c에 대한 포인터를 반환한다. 
  - 없으면 NULL 반환 
- char *strpbrk(const char *s, const char *t); 
  - 문자열 s 내에서 문자열 t 내의 문자가 처음 나타난 곳에 대한 포인터를 반환한다. 
  - 없으면 NULL 반환 
- char *strstr(const char *s, const char *t); 
  - 문자열 s 내에서 부분 문자열 t가 처음 나타난 곳에 대한 포인터를 반환한다. 
  - 없으면 NULL 반환



<br>

## 문자열 처리 함수3

- size_t strlen(const char *s); 
  - 문자열 s의 길이를 반환 
- char *strerror(int n); 
  - 오류 n에 대응되는 메시지에 대핚 포인터를 반환 
- char *strtok(char *s, const char *t); 
  - 문자열 t 내 문자로 구분된 토큰으로 s를 나눈다. 
  - If, s is not NULL, 첫번째 호출을 나타내고, 두번째 호출부터는 s를 NULL로 설정해야 함.



<br>

## 문자 처리 함수1

- \# include  <ctype.h>
- int isalnum(int c); // isalpha(c) or isdigit(c) 
- int isalpha(int c); // isupper(c) or islower(c) 
- int iscntrl(int c); // 제어문자인가? Ascii 0x00-0x1F, 0x7F 
- int isdigit(c); // 십진 숫자인가? 
- int isgraph(c); // 공백 문자가 아닊 인쇄 가능 문자인가? 
- int islower(c); // 소문자인가? 
- int isprint(c); // 공백 문자를 포함하여 인쇄 가능 문자인가? 
- int punct(c); // 공백 문자, 문자, 숫자 이외의 인쇄 가능 문자인가?



<br>

## 문자 처리 함수2

- int isspace(c) // 공백 문자, 폼피드, 새줄문자 캐리지 리턴, 탭, 수직 탭인가? 
- int isupper(c) // 대문자인가? 
- int isxdigit(c) // 16짂수 숫자인가? 
- int tolower(c) // 상응하는 소문자를 반환 
- int toupper(c) // 상응하는 소문자를 반환



<br>

## 핵심 개념

- 파일은 모든 데이터를 연속된 바이트 형태로 저장한다. 
- 파일을 사용하기 위해서는 반드시 파일 열기 fopen()를 먼저 해야 하 며 파일 열기를 하면 FILE 구조체에 대한 포인터가 리턴된다. 
- FILE 포인터는 열린 파일을 나타낸다. 
- fgetc() 함수와 fputc() 함수를 사용하여 파일에 문자 단위 입출력을 할 수 있다. 
- fgets() 함수와 fputs() 함수를 이용하여 텍스트 파일에서 한 줄씩 읽 거나 쓸 수 있다. 
- fread()와 fwrite() 함수는 한 번에 일정한 크기의 데이터를 파일에 읽거나 쓴다. 
- 열린 파일에서 다음 읽거나 쓸 파일 내 위치를 현재 파일 위치라고 하며 파일 위치 포인터가 그 파일의 현재 파일 위치를 가리키고 있다. 
- fseek() 함수는 현재 파일 위치를 지정한 위치로 이동시킨다. 

