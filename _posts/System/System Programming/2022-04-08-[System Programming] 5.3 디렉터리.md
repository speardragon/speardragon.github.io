---
layout: single
title: "[System Programming] 5.3 디렉터리"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File System']
---



## What is Directory

- A kind of **file** used to organize 
  - regular, pipe, special files and other directory **as directory entries** 
    - directory entry로 관리
  - format imposed by Unix/Linux 
- **file names** are contained only in **directory entries** 
  - i-노드에는 file 이름이 없음

- permissions 
  - r: readable
  - w: writable  
  - x: searchable
    - 파일에서는 실행이 검색 가능으로 대치

![image](https://user-images.githubusercontent.com/79521972/162351770-e763fafc-47a9-4f1e-aa28-b6274963c47f.png)



<br>

## File/Directory access permissions

<span style="color:red">시험 내용</span>

- Read
  - file: file data를 읽을 수 있는가?(copy 가능?)
  - Directory: file list를 read할 수 있는가?(ls 가능?) 
- write
  - File: file data를 수정할 수 있는가? 
  - Directory: directory의 file을 생성, 삭제할 수 있는가? 
    - **Does not mean that we can write to the directory itself.** 
    - write() 시스템콜을 이용해 디렉터리 엔트리 수정, 추가하는 것 <span style="color:red">불허</span>
    -  We **need** some **APIs** that can deal with directory itself. 
- execute 
  - File: file을 실행할 수 있는가? 
  - Directory: 이동할 수 있는가?(cd 가능?) 
- 디렉토리에 **write** 권한과 **execute** 권한이 동시에 있어야 한다. 
  - 그 디렉토리에 파일을 생성할 수 있고 
  - 그 디렉토리의 파일을 삭제할 수 있다 
- **삭제할 때는 그 파일에 대한 read권한은 없어도 됨**
  - Q) write은??

<br>

## Inode와 directory 관계

<span style="color:red">시험</span>

- /home/obama/test.c

![image](https://user-images.githubusercontent.com/79521972/162352105-cbff72f0-2678-4835-95e1-008fcec01e5e.png)



- root directory의 i-노드를 찾아간다. -> 2 (In Linux)

- 2번 노드를 찾아가면 그것이 루트 디렉토리의 i-노드 이다.
  - 파일에서는 0번이었는데 디렉토리에서는 2번 노드

- i-노드에는 UID, GID, Type, 권한, **블록 포인터** 등이 들어있고 그 중에서 블록 포인터를 들어가 보면 그곳에 directory entry (file name + i-node)가 존재한다.
- directory data block에는 접근을 원하는 파일의 i-노드 정보가 담겨 있고 그 i-노드를 찾아 들어가 그 안의 블록포인터 정보를 보고 이러한 일련의 과정을 반복하여 원하는 위치의 data block에 접근하는 것이다.

 위와 같은 일련의 과정을 반복하여 test.c 파일의 블록 포인터를 찾아갈 수 있게된다.



<br>

## 디렉터리 구현

- 디렉터리 내에는 무엇이 저장되어 있을까?
  - 디렉터리 엔트리들(directory entry)

```c
#include <dirent.h>
struct dirent {
    ino_t d_ino; // i-노드 번호
    char d_name[NAME_MAX + 1]; //파일 이름, null-terminated
}
```

![image](https://user-images.githubusercontent.com/79521972/162352280-8be44692-e58a-4b0e-a9d7-fec74d259f59.png)

<br>

## Open directory

- opendir()
  - 디렉터리 열기 위한 함수
  - DIR 포인터(열린 디렉터리를 가리키는 포인터) 리턴
    - 즉, DIR 구조체의 시작 주소를 리턴
  - DIR: 열린 디렉토리 정보를 저장하는 구조체
    - represents a **directory stream**(directory entry들이 나열된 것)
    - Defined in <dirent.h>

```c
#include <sys/types.h>
#include <dirent.h>
DIR *opendir (const char *path);
//path 디렉터리를 열고 성공하면 DIR 구조체 포인터를, 실패하면 NULL을 리턴
```



<br>

## Read directory

- readdir() 
  - 디렉터리 읽기 함수. 
  - 하나의 디렉터리 **엔트리 정보 읽음**, <mark>읽기권한 필요 </mark>
  - On the first call, the first directory entry is read into dirent. 
  - On completion, **the directory pointer is moved** onto the **next entry** in the directory. 
  - If the end of the directory is reached, NULL is returned.

```c
#include <sys/types.h>
#include <dirent.h>
struct dirent *readdir(DIR *dp);
 //한 번에 디렉터리 엔트리를 하나씩 읽어서 리턴한다.
 //디렉토리의 current file position은 읽을 때마다 읽은 디렉터리 엔트리 (구조체 dirent) 크기 만큼 이동한다

 //Returns: pointer if OK, NULL at end of directory or error
```

parameter: directory stream의 주소

return: 특정 diretory entry의 주소

<br>

## Close directories

```c
#include <dirent.h>

int closedir(DIR *dp);
// Returns: 0 if OK, -1 on error 
```

- Open a directory / Close an open directory 
  - DIR: represents a **directory stream** 
    - Defined in <dirent.h>
    - Similar to FILE type in the standard I/O library. 
- closedir() 
  - 디렉토리 파일을 닫는다. 
  - 리턴 값: 성공하면 0, 실패하면 -1



<br>

## Rewind directories

```c
#include <dirent.h>

void rewinddir(DIR *dp);
```

- readdir() 시스템 콜로 directory entry를 읽다보면 directory stream을 가리키는 포인터가 계속 이동할 것인데 이 포인터를 다시 처음 위치로 되돌리고 싶을 때 사용하는 system call
- If you want to read from the beginning of directory, use rewinddir. 
  - 처음부터 다시 읽고 싶을 때
  - 디렉토리 파일의 current file position을 처음 (first entry of the directory) 으로 옮긴다
  



---

Q) 구조체 포인터를 쓰는 이유

A) 포인터와 참조는 Call by Reference 용도로 많이 씁니다. 구조체도 별다를 것 없이 Call by Reference 로 값을 참조하는게 더 효율적인 경우, 혹은 필요한 경우에 포인터와 참조를 사용하는 것입니다.

즉, 구조체의 멤버 변수의 수정을 요할 때 사용한다.



---

<br>

## Reading directories(example)

```c
#include <stdio.h>
#include <dirent.h>

main (int argc, char ** argv)
{
    char pathname[128]; 		//문자 배열 선언
    if (argc == 1) { 		//실행 파일 이름만 주어진 경우, input argument가 없는 경우
    	strcpy(pathname, “.”); //대상 경로를 현재 디렉토리로 할 것이다.
    }
    else if (argc > 2) {
        printf(“Too many parameter…\n”);
        exit(1);
    }
    else { //argc = 2
    	strcpy(pathname, argv[1]); //인자를 pathname 변수에 저장
    }
    if (my_double_ls(pathname) == -1 )
    	printf(“Could not open the directory\n”);
}

int my_double_ls (const char *name) {
    struct dirent *d;
    DIR *dp;
    
    if ((dp = opendir(name)) == NULL) //opendir() 시스템콜 호출
    	return (-1);
    
    while (d = readdir(dp)) { //readdir의 포인터는 자동을 이동한다.
        if (d->d_ino !=0)
        	printf ("%s\n", d->d_name);
    }
    
    rewinddir(dp);
    
    while (d = readdir(dp)) {
        if (d->d_ino != 0)
        	printf ("%s\n", d->d_name);
    }
    
    closedir(dp);
    return (0);
}

int my_double_ls (const char *name) {
    struct dirent *d;
    DIR *dp;
    
    if ((dp = opendir(name)) == NULL)
    	return (-1);
    
    while (d = readdir(dp)) {
        if (d->d_ino !=0)
        	printf ("%s\n", d->d_name);
    }
    rewinddir(dp); //첫 directory entry로 이동
    
    while (d = readdir(dp)) {
        if (d->d_ino != 0)
        	printf ("%s\n", d->d_name);
    }
    closedir(dp);
    return (0);
}
```



- 실행

```shell
$ ls temp_dir/
abc bookmark fred
$ ./a.out temp_dir/
fred
abc
bookmark
..
.		//rewind 이후
fred
abc
bookmark
..
.
$
```

출력에 나오는 '.'과 '..'은 각각 current directory와 parent directory를 의미한다.

<br>

## Reading directories - list1.c

```c
#include <sys/types.h> // primitive system data types
#include <sys/stat.h> // file status
#include <dirent.h> // directory entries
#include <stdio.h>
#include <stdlib.h>

/* 디렉터리 내의 파일 이름들을 리스트한다. */
int main(int argc, char **argv)
{
    DIR *dp;
    char *dir;
    struct dirent *d;
    struct stat st;
    char path[BUFSIZ+1];
    
    if (argc == 1)
    	dir = "."; // 인자가 없는 경우 현재 디렉터리를 대상으로
    else dir = argv[1];

    if ((dp = opendir(dir)) == NULL) // 디렉터리 열기
    	perror(dir);

    while ((d = readdir(dp)) != NULL) // 각 디렉터리 엔트리에 대해
    	printf("%s \n", d->d_name); // 파일 이름 프린트

    closedir(dp); 
    exit(0); 
} 
```

```
$ list1
 .
 ..
 file1
 file2
```





<br>

## 파일 이름/크기 출력

- 디렉터리 내에 있는 파일 이름과 해당 파일의 크기(블록의 수)를 출력하도록 확장

```c
while ((d = readdir(dp)) != NULL) { 		//디렉터리 내의 각 파일
    sprintf(path, "%s/%s", dir, d->d_name); // 파일경로명 만들기 ;ex) home/etc
    if (lstat(path, &st) < 0) 				// 파일 상태 정보 st에 가져오기
    	perror(path);
    printf("%5d %s", st->st_blocks, d->name); // 블록 수, 파일 이름 출력
    putchar('\n');
}
```



<br>

## st_mode 구조체

- lstat() 시스템 호출 
  - 파일 타입과 사용권한 정보는 st->st_mode 필드에 함께 저장됨.
- st_mode 필드
  - 4비트: 파일 타입 
  - 3비트: 특수용도 
  - 9비트: 사용권한
    - 3비트: 파일 소유자의 사용권한 
    - 3비트: 그룹의 사용권한 
    - 3비트: 기타 사용자의 사용권한

![image](https://user-images.githubusercontent.com/79521972/162353174-26f3ab1e-278d-4b11-9954-825cf99763b6.png)

<br>

## 디렉터리 리스트: 예

- list2.c 
  - ls –l 명령어처럼 파일의 **모든 상태 정보**를 프린트 
- 프로그램 구성(함수 구성) 
  - main() : 메인 프로그램 
  - printStat() : 파일 상태 정보 프린트 
  - type() : 파일 타입 리턴 
  - perm() : 파일 사용권한 리턴



<br>

### <mark>list2.c</mark>

중요함

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <dirent.h>
#include <pwd.h> // password file
#include <grp.h> // group file (
#include <stdio.h>
#include <stdio.h>

char type(mode_t); // 함수 원형
char *perm(mode_t);
void printStat(char*, char*, struct stat*);

/* 디렉터리 내용을 자세히 리스트한다. */
int main(int argc, char **argv)
{
    DIR *dp;
    char *dir;
    struct stat st;
    struct dirent *d;
    char path[BUFSIZ+1]; 103

    if (argc == 1)
    	dir = "."; //current directory를 대상으로 지정
    else dir = argv[1]; //인자값을 대상으로 지정

    if ((dp = opendir(dir)) == NULL) // 디렉터리 열기
    	perror(dir);

    //list1.c 로부터 수정된 부분!!!
    while ((d = readdir(dp)) != NULL) { // 디렉터리의 각 파일에 대해
        sprintf(path, "%s/%s", dir, d->d_name); // 파일경로명 만들기
        
        if (lstat(path, &st) < 0) // 파일 상태 정보 가져오기
        	perror(path);
        
        printStat(path, d->d_name, &st); // 상태 정보 출력
        putchar('\n');
    }

    closedir(dp);
    exit(0);
}

/* 파일 상태 정보를 출력 */
void printStat(char *pathname, char *file, struct stat *st) {
    printf("%5d ", st->st_blocks);
    printf("%c%s ", type(st->st_mode), perm(st->st_mode)); //type:d나 - 같은 , perm:rwx같은
    printf("%3d ", st->st_nlink); //링크의 갯수
    printf("%s %s ", getpwuid(st->st_uid)->pw_name, getgrgid(st->st_gid)->gr_name);
    printf("%9d ", st->st_size); //바이트 단위의 크기
    printf("%.12s ", ctime(&st->st_mtime)+4); //요일 정보를 빼기 위해 +4를 하였음
    printf("%s", file);
}

/* 파일 타입을 리턴 by 매크로함수*/
char type(mode_t mode) {
    if (S_ISREG(mode))
        return('-');
    if (S_ISDIR(mode))
        return('d');
    if (S_ISCHR(mode))
        return('c');
    if (S_ISBLK(mode))
        return('b');
    if (S_ISLNK(mode))
        return('l');
    if (S_ISFIFO(mode))
        return('p');
    if (S_ISSOCK(mode))
        return('s');
}

/* 파일 사용권한을 리턴 */
char* perm(mode_t mode) {
    int i;
    static char perms[10] = "---------";

    for (i=0; i < 3; i++) {
        if (mode & (S_IRUSR >> i*3))
            perms[i*3] = 'r';  //binary to character
        if (mode & (S_IWUSR >> i*3))
            perms[i*3+1] = 'w';
        if (mode & (S_IXUSR >> i*3))
            perms[i*3+2] = 'x';
    }
    
	return(perms);
}
```

### char *perm(mode_t mode) 함수

- 소유자가 읽기 권한이 있는 경우

  0000 000 100 000 000   -> 4+3+9
- S_IRUSR 00400 
  
  0000 0000 0100 0000 0000 
- mode & (S_IRUSR >> i*3) 
  - 0000 000 100 000 000 -> mode 
  - 0000 0000 1000 0000 0000 -> S_IRUSR 
  - 0000 0000 0001 0000 0000 -> S_IRUSR >> i*3

그래서  서로 해당하는 mode끼리 and 연산을 통하면 해당 자리에는 1이 나오기 때문에 이를 이용한 것이다.

<br>

### st_mode

- permission bits/sys/stat.h

```c
#define S_IRUSR 00400 /* read permission: owner */
#define S_IWUSR 00200 /* write permission: owner */
#define S_IXUSR 00100 /* execute permission: owner */
#define S_IRGRP 00040 /* read permission: group */
#define S_IWGRP 00020 /* write permission: group */
#define S_IXGRP 00010 /* execute permission: group */
#define S_IROTH 00004 /* read permission: other */
#define S_IWOTH 00002 /* write permission: other */
#define S_IXOTH 00001 /* execute permission: other */
```

```shell
$list2
8 drw-r--r-- 2 chang faculty 4096 4월 16일 13:37 .
8 drw-r--r-- 2 chang faculty 4096 3월 26일 10:37 ..
2 -rw-r--r-- 2 chang faculty 2088 4월 16일 13:37 you.txt
2 -rw-r--r-- 2 chang faculty 2088 4월 16일 13:37 my.txt
```



<br>

### getpwuid

get password user id

- 사용자 id와 일치하는 엔트리를 /etc/pswd 파일에서 찾아 그 엔트리에 대한 포인터를 반환
- 이 엔트리의 pw_name 필드가 사용자 이름을 가지고 있음
- uid는 integer 값

```c
#include <pwd.h>
struct passwd *getpwuid(uid_t uid);
```

```c
struct passwd{ 			/* pwd.h */
    char *pw_name; 		/* Username. 결과적으로 이것을 가져오게 됨*/
    char *pw_passwd; 	/* Password. */
    uid_t pw_uid; 		/* User ID. 이것을 확인하여 일치하면 이 구조체의 포인터를 반환*/
    gid_t pw_gid; 		/* Group ID. */
    char *pw_gecos; 	/* Real name. */
    char *pw_dir; 		/* Home directory. */
    char *pw_shell; 	/* Shell program. */
}; 
```

<br>

### getgrgid

- getgrgid: 그룹 id로 그룹 정보 제공

```c
#include <grp.h>
struct group *getgrgid(gid_t gid);
```

<br>

### ctime()

```c
struct utimbuf {
    time_t actime; /* access time */
    time_t modtime; /* modification time */
}
```

- char *ctime(const time_t *timep); 
- ctime() 
  - time_t 타입의 calendar time을 받아 “Wen Nov 18 18:53:20” 과 같은 형식의 문자열을 리턴
- calendar time은1970-1-1 00:00 부터 현재까지의 경과 시간을 초로 환산한 값 
- 코드에서는 (요일+공백)을 생략하고 프린트하기위해 +4를 한 것임
  - ctime(&st->st_mtime)+4);로 호출하였음



<br>

## 디렉터리 만들기

- mkdir() 시스템 호출
  - path가 나타내는 새로운 디렉터리를 만든다.
  - "."와 ".." 파일은 자동적으로 만들어진다.
    - "."은 이 디렉토리 파일의 i-node를, 
    - ".."은 부모 디렉토리 파일의 i-node를 가르킨다 
  - 디렉터리 쓰기권한이 있어도 write 함수로 쓸 수는 없다. 
    - mkdir, rmdir 를 사용해야 함(간접적으로)

```c
#include <sys/types.h>
#include <sys/stat.h>
int mkdir (const char *path, mode_t mode); //위치, permission 정보
//새로운 디렉터리 만들기에 성공하면 0, 실패하면 -1을 리턴한다.
```

<br>

## 디렉터리 삭제

- rmdir() 시스템 호출
  - path가 나타내는 디렉터리가 비어 있으면 삭제한다.
  - An empty directory is one that contains entries **only** for **dot** and **dot-dot**. 
    - 디렉토리 파일의 link count를 감소 시킴 
    - link count가 0이 되면 삭제됨(0이 아니라는 것은 또 다른 entry가 존재한다는 것)

```c
#include <unistd.h>
int rmdir (const char *path);
//디렉터리가 비어 있으면 삭제한다. 성공하면 0, 실패하면 -1을 리턴
```



<br>

## chdir() and fchdir()

```c
#include <unistd.h>

int chdir(const char *pathname);
int fchdir(int filedes);
// Both return: 0 if OK, -1 on error 
```

- Change the current working directory. (like 'cd')
  - Specify the new current working directory either as a pathname or file descriptor

<br>

- **Example**

```c
#include "apue.h"
int main(void)
{
    if (chdir("/tmp") < 0)
    	err_sys("chdir failed");
    printf("chdir to /tmp succeeded\n");
    exit(0);
}
```

- `$ gcc -o mycd chdir.c`



<br>

- <mark>**실행**</mark>(<span style="color:red">이거 중요</span>)

```
$ pwd
/usr/lib
$ mycd
chdir to /tmp succeeded
$ pwd
/usr/lib
$
```

- Current working directory of shell didn't change after mycd. 
- Each program is run in a separate process 
  - -> current working directory of shell is **unaffected** by chdir in mycd.
  - <span style="color:blue">mycd를 실행시키면 별도의 mycd의 프로세스로 실행되기 때문에 shell command의 process에는 영향을 끼치지 않는 것이다!!!</span>
- Note that “cd” is a <span style="color:red">built-in shell command!</span>

어찌 보면 당연한 건데 헷갈릴 수 있는 부분이다.

<br>

## getcwd()

```c
#include <unistd.h>

char *getcwd(char *buf, size_t size);
//Returns: buf if OK, NULL on error
```

- Obtain the current working directory. 
  - Copies an **absolute pathname** of the current working directory to the array pointed to by *buf* of length, *size*. 
  - size must be large enough to accommodate 
    - the absolute pathname + a terminating null byte('/0')

<br>

- **example**

```c
#include <stdio.h>
#include <unistd.h>
#define SIZE 200
void my_pwd (void);
int main()
{
	my_pwd();
}
void my_pwd (void) {
    char dirname[SIZE];
    
    if (getcwd(dirname, SIZE) == NULL)
        perror("getcwd error");
    else
        printf("%s\n", dirname);
}
```



- **실행**

```
$ ./a.out
/home/obama/test
$
```

<br>

## 디렉터리 구현

- 그림 5.1의 파일 시스템 구조를 보자.
  - 디렉터리를 위한 구조는 따로 없다. 
- 파일 시스템 내에서 디렉터리를 어떻게 구현할 수 있을까? 
  - 디렉터리도 일종의 파일로 다른 파일처럼 구현된다. 
  - 디렉터리도 다른 파일처럼 하나의 i-노드로 표현된다. 
  - 디렉터리의 내용은 디렉터리 엔트리(파일이름, i-노드 번호)

![image](https://user-images.githubusercontent.com/79521972/162354595-990e76ad-bdcb-42d7-96a6-4060347186c7.png)



2번에는 /(root directory)의 정보가 담겨져 있다.

그런 다음 4번 노드를 찾아가면 usr의 i-노드 정보가 있는데 블록포인터를 보고 202번 블락 번호의 내용으로 들어가 test.c의 i-노드 번호인 6번을 또 타고 들어가 204번 블럭을 찾아 들어간다.

directory entry는 file일 수도 directory일 수도 있다.





