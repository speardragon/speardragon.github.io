---
layout: single
title: "[System Programming] 8-2장. 프로세스 ID"
categories: ['System', 'System Programming']
tag: ['Process ID']
---

<br>

## 프로세스 ID

- 각 프로세스는 프로세스를 구별하는 번호인 프로세스 ID를 갖는다. 
  - PID : a unique identifier 
  - The pid is guaranteed to be unique at any single point in time 
  - As processes terminate, IDs become candidates for reuse. 
- 각 프로세스는 자신을 생성해준 부모 프로세스가 있다.

```c
int getpid( ); //프로세스의 ID를 리턴한다.
int getppid( ); //부모 프로세스의 ID를 리턴한다. 
```



<br>

## pid.c

```c
#include <stdio.h>

/* 프로세스 번호를 출력한다. */
int main()
{
    int pid;
    printf("나의 프로세스 번호 : [%d] \n", getpid());
    printf("내 부모 프로세스 번호 : [%d] \n", getppid());
}
```

![image](https://user-images.githubusercontent.com/79521972/167991887-a27e4077-0c5b-4ee3-980d-16355a3e210d.png)



<br>

## Process identifiers

![image](https://user-images.githubusercontent.com/79521972/167991918-a18226f1-fe2a-43b2-b706-a46e0bb745d8.png)



<br>

## Current working directory of process

- 각 프로세스는 Current working directory 를 가지고 있음 
- chdir() 시스템 콜을 이용해 Current working directory를 pathname 이 지정하는 디렉토리로 변경 가능 
  - 단, 프로세스는 pathname이 지정하는 디렉토리에 대한 **실행 권한**이 있어 야 함. (5장 파일 시스템 참조)

```c
#include <unistd.h>
int chdir(const char *pathname); // return: 0 if OK, -1 on error
```























