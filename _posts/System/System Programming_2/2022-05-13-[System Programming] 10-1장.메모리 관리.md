---
layout: single
title: "[System Programming] 10-1장. 메모리 관리"
categories: ['System', 'System Programming']
tag: ['memory', 'variable']
---

<br>

# 10.1 변수와 메모리

## 프로세스

- 프로세스는 실행중인 프로그램이다. 
- 프로그램 실행을 위해서는 
  - 프로그램의 코드, 데이터, 스택, 힙, U-영역 등이 필요하다. 
- 프로세스 이미지(구조)는 메모리 내의 프로세스 레이아웃 
- 프로그램(program definition) 자체가 프로세스(program instance)는 아니다 !

<br>

## The Process Address Space

- Linux virtualizes its physical resource of memory 
- Processes do not directly address physical memory. 
- Instead, the kernel associates each process with a unique **virtual** **address space**. 
  - 프로세스 마다

- This address space is **linear**, with addresses starting at zero, and increasing to some maximum value 
- Pages and Paging 
  - The virtual address space is composed of **pages**. 
  - The system architecture and machine type determine the size of a page, which is fixed; typical sizes include 4 KB (for 32-bit systems), and 8 KB (for 64-bit systems)

<br>

## Paging Model

![image](https://user-images.githubusercontent.com/79521972/168198658-5f67ba47-8f05-46c7-b542-197692732d02.png)

logical memory를 physical memory에 mapping 하기 위한 page table이 존재함.

<br>

## 프로세스 이미지 구조 (주소공간)

- 프로세스 구조

![image](https://user-images.githubusercontent.com/79521972/168198905-d71739aa-fccf-4f5d-8986-aae32c569db3.png)

- 코드 세그먼트(code segment) 
  - 코드를 번역한 기계어 명령어 
- 데이터 세그먼트(data segment) 
  - e.g. int maxcount = 99; (initialized) 
  - e.g. long sum[1000]; (uninitialized) 
- 스택(stack) 
  - 지역 변수, 매개 변수, 반환 주소, 반환 값, 등 
- 힙(heap) 
  - 동적 메모리 핛당 
  - malloc() in C, 
  - new class() in Java

<br>

## 프로세스 이미지 세그먼트

- Text segment contains 

  - The program code, string literals, constant variables, and other read-only data. 

  - In Linux, this segment is marked read-only 

- Data segment contains 
  - initialized global variables. (함수 밖 선언) 
  - Static variables (함수 안, 밖 선언) 
- bss segment contains 
  - uninitialized global variables. 
  - These variables contain special values (essentially, all **zeros**)

- Stack contains 
  - the process’ execution stack (stack frame), which grows and shrinks dynamically as the stack depth increases and decreases. 
  - The execution stack contains local variables and function return data. 
- Heap contains 
  - a process’ dynamic memory. 
  - This segment is writable and can grow or shrink in size. 
  - This is the memory returned by malloc( )

<br>

## vars.c

```c
#include <stdio.h>
#include <stdlib.h>
int a = 1;
static int b = 2;
int main() {
    int c = 3;
    static int d = 4;
    char *p;
    
    p = (char *) malloc(40);
    fun(5);
}
void fun(int n)
{
    int m = 6;
    ...
} 
```



<br>

## 프로그램 시작할 때 메모리 영역

![image](https://user-images.githubusercontent.com/79521972/168521542-ffb1cb2b-464d-4445-8b1f-e6d2c7443237.png)



<br>

## main() 함수 실행할 때 메모리 영역

![image](https://user-images.githubusercontent.com/79521972/168521575-a311dc3c-3ddc-40ae-8942-ea9fe148c828.png)

local 변수 -> stack에 저장

<br>

## 함수 fun() 실행할 때 메모리 영역

![image](https://user-images.githubusercontent.com/79521972/168521624-1f19b8c2-ee00-4dc4-b6ec-c9b1841181ce.png)



<br>

## 함수 fun() 실행이 종료될 때 메모리 영역

![image](https://user-images.githubusercontent.com/79521972/168521660-9d752ddd-18c4-4b49-9661-500581462c9f.png)



<br>

## 할당 방법에 따른 변수들의 분류

![image](https://user-images.githubusercontent.com/79521972/168521688-93e2ca93-7336-4dcb-9d7b-95ffbd345308.png)

정적 변수: 함수 밖에서 선언된 변수

자동 변수: 자동으로 사라지고 생기는 변수

동적 변수: 동적 할당 되어 heap에 생성되는 변수

<br>

## Stack model vs. Heap model 

- Modern programming languages employ two different memory models, namely the Stack and the Heap. 

- We need to understand how calling procedures/functions/methods effects the memory model. 

<br>

## Procedures 

- High level languages usually **employ the concept of procedures** (or functions, methods, routines, etc). 
- A program is composed of several procedures. Each procedure is a small piece of code, which can receive arguments, define variables, perform a simple and defined operation and optionally return a value. 
- Procedures may call other procedures(nested function) or themselves (recursion). Note that even object oriented programs are procedural. 
- When you call a member method of some object, you basically call the method with the object as one of the arguments. 

<br>

## The Call Stack 

- Procedural calls can be viewed in a stack-like manner; each procedure's activation (function call) requires a dedicated frame on a stack. 
- This function-call-dedicated stack frame is called an Activation Frame. 
  - Each time a procedure is called, a new activation frame is generated on the stack. 
  - Each activation frame holds the following information: 
    - The parameters passed to the procedure 
    - Local primitive variables declared in the procedure 
    - The return address - Where to return to when the procedure ends 
- When an activation frame is created, the binding between local variables and their location on the stack is made. When a procedure terminates, its associated activation frame is popped from the stack, and discarded.
-  In Java, only the variables located on the top-most activation frame may be accessed (parameters and local variables). However, this is not the case in C++. 

<br>

## Stack Implementation 

- To realize the concept of procedures and activation frames, modern computer systems inherently support the call stack data structure; 
- For each execution element (process, thread, etc.) there exists an associated memory region which is called the call stack. 
- The operating system is responsible for allocating enough space for the stack of each process (or thread), and the process (or thread) implicitly, using a code generated by the compiler, manages the stack on its own.



<br>

## Stack: Advantages and Disadvantages 

- Advantages 

  - call-stack memory model provides a fast managed memory 
    - the memory used by the stack is automatically discarded when popping the stack frame just by changing the value of a single register. 

  - In addition, the compiler is responsible of generating the instructions that manipulate the stack and therefore can optimize them. 

- Disadvantages 

  - the stack is often limited in size 

  - one cannot use values that resides inside a stack frame once it is popped (i.e., by one of the methods that correspond to the upper stack frames). 

  - the allocations must be static and the size allocated must be known at compile time, because the compiler generate the instructions that manipulate the stack at compile time.

<br>

# 10.2 동적 할당

## 동적 메모리 할당

- 동적 할당을 사용하는 이유 
  - 필요할 때 필요할 만큼만 메모리를 요청해서 사용하여 
  - 메모리를 절약한다. 
- malloc( ) 
- calloc( ) 
- realloc( ) 
- free( )

<br>

## Dynamic memory allocation

- Static memory allocation 
  - Array 
  - Array requires exact element number. 
  - In some cases, we could not predict the amount of required memory in compilation time. 
- Functions for dynamic memory allocation 
  - malloc : allocate memory space 
  - calloc : allocate memory space and initialize it to zero 
  - free : frees previously allocated memory space 
  - realloc : modifies the size of previously allocated space

<br>

## 메모리 할당

```c
#include <stdlib.h>
void *malloc(size_t size);
//size 바이트만큼의 메모리 공갂을 핛당하며 그 시작주소를 void* 형으로 반환한다.
void free(void *ptr);
//포인터 p가 가리키는 메모리 공갂을 해제핚다.
```

- 힙에 동적 메모리 핛당 
- 라이브러리가 메모리 풀을 관리핚다 
- malloc() 함수는 메모리를 핛당핛 때 사용하고 free()는 핛당 핚 메모리를 해제핛 때 사용핚다. 



<br>

## 메모리 할당 예

- char *ptr; 
- ptr = (char *) malloc(40); // 좌변은 char * 형, 우변은 void * 형이어서 
                                               // 형변환 필요

![image](https://user-images.githubusercontent.com/79521972/168522226-28804a8d-6e73-40ae-b3dc-9c03dfaf03a0.png)



- int *ptr; 
- ptr = (int *) malloc(10 * sizeof(int));

![image](https://user-images.githubusercontent.com/79521972/168522259-68362e36-9b52-4ad8-92f7-cf9d0474f35b.png)



<br>

## 구조체를 위한 메모리 할당 예

```c
struct student {
    int id;
    char name[10];
};
struct student *ptr;
ptr = (struct student *) malloc(sizeof(struct student));
```

![image](https://user-images.githubusercontent.com/79521972/168522309-00533f00-896d-4b55-8e95-5fbd5f3f50f3.png)



<br>

## 구조체 배열을 위한 메모리 할당 예

```c
struct student *ptr;
ptr = (struct student *) malloc(n * sizeof(struct student));
```

![image](https://user-images.githubusercontent.com/79521972/168522350-72945a3b-aa0f-4bc2-9095-65de84425567.png)

*(ptr+i) or ptr[i] : (i+1)번째 구조체



<br>

## stud1.c

```c
#include <stdio.h>
#include <stdlib.h>
struct student {
    int id;
    char name[20];
};
/* 입력받을 학생 수를 미리 입력받고 이어서 학생 정보를 입력받은 후,
이들 학생 정보를 역숚으로 출력하는 프로그램 */
int main()
{
    struct student *ptr; // 동적 핛당된 블록을 가리킬 포인터
    int n, i;
    printf("몇 명의 학생을 입력하겠습니까? ");
    scanf("%d", &n);
    if (n <= 0) {
        fprintf(stderr, "오류: 학생 수를 잘못 입력했습니다.\n");
        fprintf(stderr, "프로그램을 종료합니다.\n");
        exit(1);
    }
    ptr = (struct student *) malloc(n * sizeof(struct student));
    if (ptr == NULL) {
        perror("malloc");
        exit(2);
    }
    printf("%d 명의 학번과 이름을 입력하세요.\n", n);
    for (i = 0; i < n; i++)
        scanf("%d %s\n", &ptr[i].id, ptr[i].name);
    printf("\n* 학생 정보(역숚) *\n");
    for (i = n-1; i >= 0; i--)
        printf("%d %s\n", ptr[i].id, ptr[i].name);
    printf("\n");
    exit(0);
}
```



```shell
$ stud1
몇 명의 학생을 입력하겠습니까? 5
명의 학번과 이름을 입력하세요.
1401001 박연아
1401003 김태환
1401006 김현진
1401009 장샛별
1401011 홍길동
^D
* 학생 정보(역순) *
1401011 홍길동
1401009 장샛별
1401006 김현진
1401003 김태환
1401001 박연아
```



<br>

## Example of memory allocation

```c
#include<stdio.h>
 #include<stdlib.h>
int main(void) {
    char *buffer;
    /*Allocating memory*/
    if((buffer=(char *) malloc(sizeof(char)*20))==NULL) {
        printf(“Malloc failed\n”);
        exit(1);
    }
    strcpy(buffer, “Kwangwoon Univ.”);
    printf(“%s\n”, buffer);
    /*freeing memory*/
    free(buffer);
} 
```

<br>

## 배열 할당

- 같은 크기의 메모리를 여러 개를 핛당핛 경우

```c
#include <stdlib.h>
void *calloc(size_t n, size_t size);
// 크기가 size인 메모리 공갂을 n개 핛당핚다. 값을 모두 0으로 초기화핚다. 실패하면 NULL를 반환핚다.
```

- 이미 할당된 메모리 크기 변경

```c
#include <stdlib.h>
void *realloc(void *ptr, size_t newsize);
//ptr이 가리키는 이미 핛당된 메모리의 크기를 newsize로 변경핚다.
```

<br>

## calloc() 예

```c
int *p,*q;
p = malloc(10*sizeof(int));
if (p == NULL)
	perror(“malloc”);
q = calloc(10, sizeof(int));
if (q == NULL)
	perror(“calloc”);
```

<br>

## 스택에 메모리 할당

- \#include  
- Void * alloca( size_t size); 
- Life cycle?

<br>

## Dynamic vs. Static

- Dynamic 
  - More efficient memory usage 
  - Slower execution time 
  - Less reliable compared to static mechanism 
  - Realization : malloc( ), free( ) 
- Static 
  - Inefficient in memory usage 
  - Faster 
  - Reliable because all required memory spaces are reserved at compilation time 
  - Realization : declaring memory 
- Real world : Dynamic + Static

<br>

## Heap

- Sometimes, programs need to store information which is relevant across function calls, too big to fit on the stack, or of size that is unknown at compile time. 
- We would like to be able to specify that we want a block of memory of a given size to store some information. 
- We usually do not care where the memory comes from, we are just interested in getting a block of it for our use. 
- As a result, we abstract this service as a heap, where blocks of memory are heaped in a pile, and we can get to the block we need if we remember where we left it. 



<br>

## Heap: Advantages and Disadvantages

- Advantages 
  - The Heap model allows for dynamic memory allocation - i.e., the size of the allocation does not have to be known at compile time. 
  - memory on the heap stays on the heap until it is explicitly freed (either by the user or by the garbage collector),  
  - the heap is much larger than the stack and consists from most of the memory available to the process (the virtual memory). 

- Disadvantages 
  - heap allocation costs more than stack allocation (as the runtime must find an empty memory location to allocate on the fly) and can cause memory fragmentation. 
  - If the user is responsible to free the allocated memory by hand 
    - the program may be pruned to memory leaks and access to non-allocated memory. 
  - If the runtime is responsible to free the allocated memory (i.e., using automatic garbage collection) 
    - the performance of the program may degrade as CPU time will be dedicated for the garbage collection execution and also since most garbage collection algorithms require to stop all or some parts of the program while they perform the cleaning.

<br>

# 10.3 연결 리스트

## 연결 리스트의 필요성

- 예: 여러 학생들의 데이터를 저장해야 한다고 생각해보자. 
  - 가장 갂단핚 방법은 구조체 배열을 선얶하여 사용하는 것이다. 
  - 이 방법은 배열의 크기를 미리 결정해야 하는 문제점이 있다. 
  - 배열의 크기보다 많은 학생들은 처리핛 수 없으며 이보다 적은 학 생들의 경우에는 배열의 기억공간은 낭비된다.

- 연결리스트(linked list)를 사용하여 해결핛 수 있다

![image](https://user-images.githubusercontent.com/79521972/168522865-330a6668-dcea-44b7-881a-764a1810a124.png)

<br>

## 자기 참조 구조체를 위한 메모리 할당

```c
struct student {
    int id;
    char name[20];
    struct student *next;
};
struct student *ptr;
ptr = (struct student *) malloc(sizeof(struct student));
```

![image](https://user-images.githubusercontent.com/79521972/168522900-58b26e4d-0f88-404f-9797-033aaa215c54.png)

<br>

## stud2.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
…
/* 학생 정보를 입력받아 연결 리스트에 저장하고 학생 정보를 역숚으로
 출력핚다. */
int main()
{
    int count = 0, id;
    char name[20];
    struct student *p, *head = NULL;
    printf("학번과 이름을 입력하세요\n");
    
    while (scanf("%d %s", &id, name) == 2) {
        p = (struct student *) malloc(sizeof(struct student));
        if (p == NULL) {
            perror("malloc");
            exit(1);
        }
        p->id = id;`
            strcpy(p->name,name);
        p->next = head;
        head = p;
    }
    
    printf("\n* 학생 정보(역숚) *\n");
    p = head;
    
    while (p != NULL) {
        count++;
        printf("학번: %d 이름: %s \n", p->id, p->name);
        p = p->next;
    }
    
    printf("총 %d 명입니다.\n", count);
    exit(0);
}
```



```shell
$ stud2
학번과 이름을 입력하세요.
1401001 박연아
1401003 김태환
1401006 김현진
1401009 장샛별
1401011 홍길동
^D
* 학생 정보(역순) *
1401011 홍길동
1401009 장샛별
1401006 김현진
1401003 김태환
1401001 박연아
```



<br>

## 큐 형태의 연결 리스트

```c
struct student *ptr, *head = NULL, *tail = NULL;
ptr = (struct student *) malloc(sizeof(struct student));
...
tail->next = ptr; // 큐의 끝에 연결
tail = ptr; // 큐의 끝을 가리킴
```

![image](https://user-images.githubusercontent.com/79521972/168523025-a5a6e129-54ba-4f4b-a39d-751e7ebad78d.png)

<br>

# 10.4 메모리 관리 함수

## 문자열 처리 함수와의 차이점

- strcpy() or strncpy() vs memcpy, strcmp() vs memcmp()
-  기본적 동작은 동일함 
- 차이점 
  - 인자와 반환값이 다름 
    - 문자열 처리함수 문자열을 대상으로하므로 인자와 반환값이 char * 형 
    - 메모리 관리함수 임의의 값을 갖는 메모리 영역을 대상으로 하므로 인자와 반환값이 void * 형 메모리에 저장된 값의 타입에 무관함 
  - 길이 정보 
    - 문자열 처리함수 
      - 문자열은 시작 주소를 알려주면 NULL 문자를 문자열의 끝으로 인지하므로 길이를 알려주지 않음. 
    - 메모리 관리함수 
      - 스트링 단위로 처리하지 않으므로 길이를 알려주어야 함

<br>

## 메모리 관리 함수

```c
# include <string.h>
void *memset(void *s, int c, size_t n);
//s에서 시작하여 n 바이트만큼 바이트 c로 설정한 다음에 s를 반환한다.
int memcmp(const void *s1, const void *s2, size_t n);
//s1과 s2에서 첫 n 바이트를 비교해서, 메모리 블록 내용이 동일하면 0을 반환하고 s1이
//s2보다 작으면 음수를 반환하고, s1이 s2보다 크다면 양수를 반환한다.
void *memchr(const void *s, int c, size_t n);
//s가 가리키는 메모리의 n 바이트 범위에서 문자 c를 탐색한다. c와 일치하는 첫 바이트에
//대한 포인터를 반환하거나,c를 찾지 못하면 NULL을 반환한다.
void *memmove(void *dst, const void *src, size_t n);
//src에서 dst로 n 바이트를 복사하고, dst를 반환한다.
void *memcpy(void *dst, const void *src, size_t n);
//src에서 dst로 n 바이트를 복사한다. 두 메모리 영역은 겹쳐지지 않는다. 만일 메모리 영
//역을 겹쳐서 쓰길 원핚다면 memmove() 함수를 사용해라. dst를 반환한다.
```



<br>

## mem.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void main()
{
    char str[32]="Do you like Linux?";
    char *ptr,*p;
    
    ptr = (char *) malloc(32);
    memcpy(ptr, str, strlen(str));
    puts(ptr);
    memset(ptr+12,'l',1); 
    puts(ptr); 

    p = (char *) memchr(ptr,'l',18); 
    puts(p); 
    memmove(str+12,str+7,10);
    puts(str);
}
```

