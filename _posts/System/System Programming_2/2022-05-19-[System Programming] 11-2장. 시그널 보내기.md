```
layout: single
title: "[System Programming] 11-2장. 시그널 보내기"
categories: ['System', 'System Programming']
tag: ['Signal']
```

<br>

# 11.3 시그널 보내기

## 시그널 보내기: kill 명령어

- kill 명령어 
  - 한 프로세스가 다른 프로세스를 제어하기 위해 특정 프로세스에 임의의 시그널을 강제적으로 보낸다. 

![image](https://user-images.githubusercontent.com/79521972/169327444-b216aa64-f7ce-4296-ac5b-cdaecf92b4fa.png)

- $ kill [-시그널] 프로세스ID 

  - $ kill -9 프로세스ID // 시그널 번호 
  - $ kill -KILL 프로세스ID // 시그널 이름 

- $ kill –l // 시그널 리스트 

  HUP INT QUIT ILL TRAP ABRT BUS FPE KILL USR1 SEGV USR2 PIPE ALRM TERM STKFLT CHLD CONT STOP TSTP TTIN TTOU URG XCPU XFSZ VTALRM PROF WINCH POLL PWR SYS ...

```shell
$ 명령어 &
[1]1234
$ kill -STOP 1234 // 1234 프로세스 중지시킴
[1] +Suspended (signal) 명령어
$ kill -CONT 1234 // 중지된 1234 프로세스의 실행을 재개
```

<br>

## 시그널 보내기: kill()

- kill() 시스템 호출 
  - 특정 프로세스 pid에 원하는 임의의 시그널 signo를 보낸다. 
  - 보내는 프로세스의 소유자가 프로세스 pid의 소유자와 같거나 혹은 보내는 프로세스의 소유자가 슈퍼유저이어야 한다. 
  - Signo를 명시하지 않으면 SIGTERM 시그널을 보내 해당 프로세스 를 강제 종료시킴.

```c
#include <sys/types.h>
#include <signal.h>
int kill(int pid, int signo);
//프로세스 pid에 시그널 signo를 보낸다. 성공하면 0 실패하면 -1를 리턴한다.
```

<br>

## kill()

- pid > 0 : 
  - signal to the process whose process ID is pid 
- pid == 0 : 
  - signal to the processes whose process group ID equals that of sender 
  - process group ID가 sender의 group ID와 동일한 모든 프로세스에게 전달
- pid < 0 : 
  - signal to the processes whose process group ID equals **absolute** of pid 
- pid == -1 : 
  - POSIX.1 leaves this condition unspecified (used as a broadcast signal in SVR4, 4.3+BSD)



<br>

## 예제: 제한 시간 명령어 실행

- tlimit.c 프로그램 
  - 명령줄 인수로 받은 명령어를 제한 시간 내에 실행 
  - execute3.c 프로그램을 알람 시그널을 이용하여 확장 
- 프로그램 설명 
  - 자식 프로세스가 명령어를 실행하는 동안 정해진 시간이 초과되면 SIGALRM 시그널이 발생 
  - SIGALRM 시그널에 대한 처리함수 alarmHandler()에서 자식 프로세스를 강제 종료 
  - kill(pid, SIGINT) 호출을 통해 자식 프로세스에 SIGINT 시그널을 보내어 강제 종료 
  - 만약 SIGALRM 시그널이 발생하기 전에 자식 프로세스가 종료하면 이 프로그램은 정상적으로 끝남



<br>

## tlimit.c

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
int pid;
void alarmHandler();
/* 명령줄 인수로 받은 명령어 실행에
제한 시간을 둔다. */
int main(int argc, char *argv[])
{
    int child, status, limit;
    
    signal(SIGALRM, alarmHandler);
    sscanf(argv[1], "%d", &limit);
    alarm(limit);
    
    pid = fork(); //부모 프로세스의 pid
    if (pid == 0) {
        execvp(argv[2], &argv[2]);
        fprintf(stderr, "%s:실행 불가\n", argv[1]);
    } else {
        child = wait(&status);
        printf("[%d] 자식 프로세스 %d 종료 \n", getpid(), pid);
    }
}
void alarmHandler()
{
    printf("[알람] 자식 프로세스 %d 시간 초과\n", pid);
    kill(pid,SIGINT);
}
```

```shell
$ tlimit 3 sleep 5
[알람] 자식 프로세스 27260 시간 초과
[27259] 자식 프로세스 27260 종료
```

<br>

## 시그널을 이용한 프로세스 제어

- 시그널을 이용하여 다른 프로세스를 제어할 수 있다. 

```
SIGCONT 프로세스 재개
SIGSTOP 프로세스 중지
SIGKILL 프로세스 종료
SIGTSTP Ctrl-Z에서 발생
SIGCHLD 자식 프로세스 중지 혹은 종료 시 부모 프로세스에 전달
```

- 예제: 시그널을 이용한 자식 프로세스 제어

![image](https://user-images.githubusercontent.com/79521972/169328047-f07bc8a7-6293-4c30-90fd-1f7cd3fd3139.png)

<br>

## control.c(시험)

```c
#include <signal.h>
#include <stdio.h>
/* 시그널을 이용하여 자식 프로세스들을 제어한다. */
int main( )
{
    int pid1, pid2; count1=0, count2=0;
    pid1 = fork( );
    if (pid1 == 0) {
        while (1) {
            sleep(1);
            printf("자식 [1] 실행:%d\n", ++count1);
        }
    }
    pid2 = fork( );
    if (pid2 == 0) {
        while (1) {
            sleep(1);
            printf("자식 [2] 실행:%d\n―", ++count2);
        }
    }
    sleep(2);
    kill(pid1, SIGSTOP);
    sleep(2);
    kill(pid1, SIGCONT);
    sleep(2);
    kill(pid2, SIGSTOP);
    sleep(2);
    kill(pid2, SIGCONT);
    sleep(2);
    kill(pid1, SIGKILL);
    kill(pid2, SIGKILL);
}
```

```shell
$ control
자식 [1] 실행:1
자식 [2] 실행:1
자식 [2] 실행:2
자식 [2] 실행:3
자식 [1] 실행:2
자식 [2] 실행:4
자식 [1] 실행:3
자식 [2] 실행:5
자식 [1] 실행:4
자식 [1] 실행:5
자식 [2] 실행:6
자식 [1] 실행:6
자식 [2] 실행:7
자식 [1] 실행:7

```

![image](https://user-images.githubusercontent.com/79521972/169328281-d9049c54-8678-4067-96d2-b53f6ab1301c.png)



<br>

## raise()

```c
#include <signal.h>

int raise(int signo);
 //Both return: 0 if OK, -1 on error 
```

- Sends a signal to itself. 
  - `raise(signo);` is equivalent to kill(getpid(), signo);
  - 자기 자신에게 signal을 보내라.

<br>

## abort()

```c
#include <stdlib.h>

void abort(void);
 //This function never returns
```

- **Sends the SIGABRT to the caller**

<br>

# 11.4 시그널과 비지역 점프

## 비지역 점프 (non-local) :setjmp/longjmp

- C에서는 함수 밖의 label로 goto 불가 (only local 영역에서만)
- 임의의 위치로 비지역 점프 
  - 오류/예외 처리, 시그널 처리 등에 유용함 
- int setjmp(jmp_buf env) 
  - longjmp 전에 호출되어야 함 
  - longjmp 할 곳을 지정함. 
  - 한 번 호출되고 여러 번 반환함. 
  - longjmp로 인해 돌아오면 setjmp는 0이 아닌 값으로 돌아오기 때문에 그 안의 값을 출력하는 것이다.
- void longjmp(jmp_buf env, int val) 
  - setjmp 후에 호출됨 
  - **setjmp에 의해 설정된 지점**으로 비지역 점프 
  - 한 번 호출되고 반환하지 않음. 

<img src="https://user-images.githubusercontent.com/79521972/169328857-49fbaa38-2d51-43cf-b13d-329dfb253fd0.png" alt="image" style="zoom:67%;" />



<br>

## setjmp/longjmp

```c
#include <setjmp.h>
int setjmp(jmp_buf env);
//비지역 점프를 위해 실행 스택 내용 등을 env에 저장한다. setjmp()는 처음 반환할 때 0을 반환하고 저장된 내용을 사용하는 longjmp()에 의해 두 번째 반환할때는 0이 아닌 val 값을 반환한다.

void longjmp(jmp_buf env, int val);
//env에 저장된 상태를 복구하여 스택 내용 등이 저장된 곳으로 비지역 점프한다. 구체적으로 상응하는 setjmp() 함수가 val 값을 반환하고 실행이 계속된다.
```

비지역 점프를 위해 실행 스택 내용 등을 env에 저장한다. setjmp()는 처음 반환할 때 0을 반환하고 저장된 내용을 사용하는 <mark>longjmp()에 의해 두 번째 반환할때는 0이 아닌 val 값을 반환한다.</mark>

<br>

## jump1.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <setjmp.h>
void p1(), p2();
jmp_buf env; // jmp_buf는 setjmp.h에서 정의
int main()
{
    if (setjmp(env) != 0) {
        printf("오류로 인해 복귀\n");
        exit(0);
    } 
    else printf("처음 통과\n"); 
    p1();
} 

void p1()
{
    p2();
}

void p2()
{
    int error;
    error = 1;
    if (error) {
        printf("오류 \n");
        longjmp(env, 1);
    }
}
```

```shell
$ jump1
처음 통과
오류
오류로 인해 복귀
```

longjmp가 실행되면 setjmp로 실제로 리턴을 하는 것은 아니지만 리턴 위치로 복귀하게 된다.

longjmp로 인해 돌아오면 setjmp는 0이 아닌 값(val)으로 돌아오기 때문에 그 안의 값을 출력하는 것이다.

<br>

## jump2.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <setjmp.h>
#include <signal.h>
void p1();
void intHandler();
jmp_buf env;
int main() 
{
    signal(SIGINT, intHandler);
    if (setjmp(env) != 0) {
        printf("인터립트로 인해 복귀\n");
        exit(0); 
    } 
    else printf("처음 통과\n");
    p1();
} 

void p1()
{
    while (1) {
        printf("루프\n");
        sleep(1);
    }
}
void intHandler()
{
    printf("인터럽트\n");
    longjmp(env, 1);
}
```

```shell
$ jump2
처음 통과
루프
루프
루프
^c 인터럽트
인터립트로 인해 복귀
```



<br>

## 핵심 개념

- 시그널은 예기치 않은 사건이 발생할 때 이를 알리는 소프트웨어 인터럽트이다. 
- signal() 시스템 호출은 특정 시그널에 대한 처리 함수를 지정한다. 
- kill 명령어나 kill() 시스템 호출을 이용하여 특정 프로세스에 원하는 시그널을 보낼 수 있다. 
- longjmp() 함수는 setjmp() 함수에 의해 설정된 지점으로 비지역 점프를 한다. 



<br>

## 주요 시그널

![image](https://user-images.githubusercontent.com/79521972/169329431-0f4df9f2-edbc-4f9f-959c-ebcda5877257.png)

![image](https://user-images.githubusercontent.com/79521972/169329502-d8e04a87-29d4-4bc3-b54c-8d643a4afd71.png)