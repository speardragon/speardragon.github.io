---
layout: single
title: "[System Programming] 5.2 파일 상태 정보"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File System']
---



## Contents

- File system = file data + <u>**file attribute**</u>
  - File attributes
    - Type, permission, size, time, user/group ID
  - File attributes modification
    - chown, chmod, ...
  - Symbolic link
  - Directories



<br>

## 파일 상태(file status)

- 파일 상태
  - 파일에 대한 모든 정보
  - 블록수, 파일 타입, 사용 권한(Permission), 링크수, 파일 소유자의 사용 자 ID 및 그룹 ID (user/group ID), 파일 크기, 최종 수정 시간 등
  - File attributes(file status) 수정
    - chown, chmod, ...
- 예

```assembly
$ ls -l hello.c
2	-rw-r--r-- 	 1		chang     cs     617		11월 17일 15:53  hello.c
```

블록수  사용권한 	링크수	사용자ID    그룹ID    파일 크기             최종 수정 시간 	파일이름



<br>

## 상태 정보: stat()

- 파일 하나당 하나의 i-노드가 있으며
- i-노드 내에 파일에 대한 모든 상태 정보가 저장되어 있다.
- lstat는 대상 파일이 **심볼릭 링크**일 때 사용, 링크 자체에 대한 상태정보 읽어옴.

```c
#include <sys/types.h>
#include <sys/stat.h>
int stat (const char *filename, struct stat *buf);//filename에 해당하는 관리정보를 stat에 받음
int fstat (int fd, struct stat *buf);
int lstat (const char *filename, struct stat *buf);
//파일의 상태 정보를 가져와서 stat 구조체 buf에
//저장한다. 성공하면 0, 실패하면 -1을 리턴한다.
```

<br>

- stat
  - Gets information about the named file.
- fstat
  -  Gets information about the file that is already open.
- lstat
  - Gets information about the symbolic link itself.
  - If pathname is not a symbolic link, equivalent to stat.



<br>

## stat 구조체 <sys/stat.h>에 정의

```c
struct stat {
     mode_t st_mode; // 파일 타입과 사용권한 (mode)
     ino_t st_ino; // i-노드 번호
     dev_t st_dev; // 장치 번호 (device number (file system))
     dev_t st_rdev; // 특수 파일 장치 번호
     nlink_t st_nlink; // 링크 수
     uid_t st_uid; // 소유자의 사용자 ID
     gid_t st_gid; // 소유자의 그룹 ID
     off_t st_size; // 파일 크기 (bytes)
     time_t st_atime; // 최종 접근 시갂
     time_t st_mtime; // 최종 수정 시갂
     time_t st_ctime; // 최종 상태 변경 시갂
     long st_blksize; // 최적 블록 크기
     long st_blocks; // 파일의 블록 수
};
```



<br>

## File types

- About file type

  - `Regular file`

    - Contains data of some form

    - No distinction to UNIX kernel whether the data is **text or binary**. (Applications interpret the file contents.)

  - `Directory`
    - Contains **the names of other files** and **pointers to information on these files**

  - `Block special file`

    - Provides buffered I/O access in fixed-size units to devices 

    - E.g. disk(하드 디스크)

<br>

- About file type(cont.) 
  - Character special file 
    - Provides unbuffered I/O access in variable-sized units to devices 
    - Keyboard, mouse, … 
  - FIFO 
    - Is used for communication between processes. 
    - Also called named pipe
  - Socket 
    - Is used for network communication between processes.
  - Symbolic link
    - **Points** to another file.



<br>

## 파일 타입

![image](https://user-images.githubusercontent.com/79521972/161682038-be4622a0-16e7-4d3f-ab04-fb1b5ab30167.png)

<br>

## st_mode

- file type

- 기호 상수를 사용

- ```c
  //type.h
  #define S_IFSOCK 0140000 /* socket */
  #define S_IFLNK 0120000 /* symbolic link */
  #define S_IFREG 0100000 /* regular */
  #define S_IFBLK 0060000 /* block special */
  #define S_IFDIR 0040000 /* directory */
  #define S_IFCHR 0020000 /* character special */
  #define S_IFIFO 0010000 /* FIFO */
  ```



<br>

## 파일 타입 검사 함수

- 파일 타입을 검사하기 위한 매크로 함수(Linux 제공)
- Argument of macros is the **st_mode** from the stat structure
- #include <sys/stat.h>

![image](https://user-images.githubusercontent.com/79521972/161682116-83020a03-3113-417d-9007-fcda803d1d5a.png)





<br>

## ftype.c

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
/* 파일 타입을 검사한다. */
int main(int argc, char *argv[])
{
    int i;
    struct stat buf;
    for (i = 1; i < argc; i++) { //argv[0]는 실행 파일 이름
    	printf("%s: ", argv[i]);
    	if (lstat(argv[i], &buf) < 0) { //argv[i]의 관리정보를 buf에 저장
    		perror("lstat()");
     		continue;
    	}
        if (S_ISREG(buf.st_mode))
            printf("%s \n", "일반 파일");
        if (S_ISDIR(buf.st_mode))
            printf("%s \n", "디렉터리");
        if (S_ISCHR(buf.st_mode))
            printf("%s \n", "문자 장치 파일");
        if (S_ISBLK(buf.st_mode))
            printf("%s \n", "블록 장치 파일");
        if (S_ISFIFO(buf.st_mode))
            printf("%s \n", "FIFO 파일");
        if (S_ISLNK(buf.st_mode))
            printf("%s \n", "심볼릭 링크");
        if (S_ISSOCK(buf.st_mode))
            printf("%s \n", "소켓");
    }
    exit(0);
}     
```

```
% a.out /etc /dev/ttya /bin a.out  //4개의 관리정보 출력
/etc: directory
/dev/ttya: symbolic link
/bin: symbolic link
a.out: regular
```



<br>

## 파일 사용권한(File Permissions)

- 각 파일에 대한 권한 관리
  - 각 파일마다 사용권한이 있다. 
  - 소유자(owner)/그룹(group)/기타(others)로 구분해서 관리한다. 
- 파일에 대한 권한
  - 읽기 r 
  - 쓰기 w
  - 실행 x

- 파일 사용권한(file access permission)
- stat 구조체의 <span style="color:red">st_mode</span> 의 값

```c
#include <sys/stat.h>
//mode_t는 unsigned int
```

![image](https://user-images.githubusercontent.com/79521972/161682701-49178a10-6fe1-4c9d-a9d3-2e012f404ae6.png)

![image](https://user-images.githubusercontent.com/79521972/161682721-568b5699-5653-4eea-86c7-9179819368a2.png)

mode 타입이 정확히 무슨 타입?

<br>

## File access permisisons

- user, group, others에 대해 R/W/X의 권한 부여 
  - Read
    - file: file data를 읽을 수 있는가?(copy 가능) 
      - O_RDONLY O_RDWR 을 사용하여 파일을 열 수 있다
    - Directory: file list를 read할 수 있는가?(즉 ls가 가능한가?) 
  - write 
    - File: file data를 수정할 수 있는가? 
      - O_WRONLY O_RDWR O_TRUNC 을 사용하여 파일을 열 수 있다.
    - Directory: file을 생성, 삭제할 수 있는 권한이 있는가? 
  - execute 
    - File: file을 실행할 수 있는가?
    - Directory: 이동할 수 있는가?(즉 cd가 가능한가?) 

<br>

- 디렉토리에 write 권한과 execute 권한이 있어야 
  - 그 디렉토리에 파일을 생성할 수 있고
  - 그 디렉토리의 파일을 삭제할 수 있다 
  - 삭제할 때 그 파일에 대한 read write 권한은 없어도 됨



<br>

## File access permissions

- File permission의 예

![image](https://user-images.githubusercontent.com/79521972/161682893-56844b87-5aa0-4ce8-a528-b3fd4e1107a1.png)

<br>

- Directory permission의 예

![image](https://user-images.githubusercontent.com/79521972/161682918-3524875a-d489-4eb6-90e9-4aea19a17b85.png)

<br>

## chmod(), fchmod()

```c
#include <sys/stat.h>
#include <sys/types.h>
int chmod (const char *path, mode_t mode); //파일의 경로명
int fchmod (int fd, mode_t mode);  //파일 디스크립터
```

- 파일의 사용 권한(access permission)을 변경한다 
- 리턴 값 
  - 성공하면 0, 실패하면 -1 
- mode : bitwise OR
  - S_ISUID, S_ISGID 
  - S_IRUSR, S_IWUSR, S_IXUSR
  - S_IRGRP, S_IWGRP, S_IXGRP 
  - S_IROTH, S_IWOTH, S_IXOTH



<br>

### ex) fchmod.c – $ fchmod 664 you.txt 

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <stdlib.h>
/* 파일 사용권한을 변경한다. */
main(int argc, char *argv[]){
         long strtol( ); //string관련 library
         int newmode;
         newmode = (int) strtol(argv[1], (char **) NULL, 8); //8진수 int로 변환(664)
         /* long strtol( const char *nptr, char **endptr, int base);*/
         if (chmod(argv[2], newmode) == -1) {
             perror(argv[2]);
             exit(1);
     	 }
     exit(0);
}
```



<br>

## chown()

```c
#include <sys/types.h>
#include <unistd.h>
int chown (const char *path, uid_t owner, gid_t group );
int fchown (int filedes, uid_t owner, gid_t group );
int lchown (const char *path, uid_t owner, gid_t group );
```

- 파일의 user ID와 group ID를 변경한다. (오너를 변경)
- 리턴
  - 성공하면 0, 실패하면 -1
- lchown()은 **심볼릭 링크 자체를 변경**한다
- **super-user만 변환 가능**

<br>

## utime()

```c
#include <sys/types.h>
#include <utime.h>
int utime (const char *filename, const struct utimbuf *times );
```

- 파일의 최종 접근 시간과 최종 변경 시간을 조정한다. 
  - 이 시스템 콜을 호출하면 시간 정보(times)를 file의 i노드가 갖고 있는 정보에 반영 요구
- times가 NULL 이면, 현재시간으로 설정된다
- 리턴 값
  - 성공하면 0, 실패하면 -1
-  UNIX 명령어 touch 참고 
  - used to **update** the **access date** and or **modification date** of a file or directory. 
  - In its default usage, it is the equivalent of creating or opening a file and saving it without any change to the file contents

<br>

```c
struct utimbuf {
 time_t actime; /* access time */
 time_t modtime; /* modification time */
}
```

- 각 필드는 1970-1-1 00:00 부터 현재까지의 경과 시간을 초로 환산한 값



<br>

## 예제: cptime.c

```c
#include <sys/types.h> 
#include <sys/stat.h> 
#include <sys/time.h>
#include <utime.h> 
#include <stdio.h> 
#include <stdlib.h>
int main(int argc, char *argv[])
{
     struct stat buf; // 파일 상태 저장을 위한 변수
     struct utimbuf time;
     if (argc < 3) {
         fprintf(stderr, "사용법: cptime file1 file2\n");
         exit(1);
     }
     if (stat(argv[1], &buf) <0) { // 상태 가져오기
         perror("stat()");
         exit(-1);
     }
     time.actime = buf.st_atime;
     time.modtime = buf.st_mtime;
     if (utime(argv[2], &time)) // 접근, 수정 시간 복사
    	 perror("utime");
     else exit(0);
}

```

```
% ls –asl a.c b.c
4 _rwxr-xr-x 2 chang faculty 0 2014-04-16 13:37 a.c
4 -rwxr-xr-x 2 chang faculty 5 2014-04-16 14:37 b.c
% cptime a.c b.c
% ls –asl a.c b.c
4 _rwxr-xr-x 2 chang faculty 0 2014-04-16 13:37 a.c
4 -rwxr-xr-x 2 chang faculty 5 2014-04-16 13:37 b.c
```

<br>









































