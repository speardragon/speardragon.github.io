---
layout: single
title: "[System Programming] 5.4 링크"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File System']
---





## 링크

- 링크는 기존 파일에 대한 또 다른 이름을 의미한다.
- **하드 링크**와 **심볼릭(소프트) 링크**가 있다.



```c
#include <unistd.h>
int link(char *existingpath, char *newpath); //Returns: 0 if OK, -1 on error
int unlink(char *path); //directory entry가 없어짐
```

- Create a link(hard link) to an **existing file** 
  - creates a new directory entry, newpath, that references the existing file existing path. 
  - If newpath exists, an error is returned.



<br>

## link()

- `link(“file1”, “file2”);`

![image](https://user-images.githubusercontent.com/79521972/162354771-aef43523-58c3-4e0f-82c8-f57435bf317a.png)



- It is impossible to tell which name was the original. 
- Hard links, as created by link, cannot span file systems
- 하나의 directory entry를 추가하여 동일한 i-노드를 가리키게 하는 하드 링크 방식

<br>

- "In" vs. "cp"

![image](https://user-images.githubusercontent.com/79521972/162354828-a81e4a6c-c784-40ce-9acd-b94a062526f2.png)

copy의 경우 directory entry부터 i-노드, data block을 모두 하나씩 추가하여 복사하게 된다.

<br>

## unlink()

```c
#include <unistd.h>

int unlink(const char *pathname);
//Returns: 0 if OK, -1 on error
```

- Removes the **directory entry** and decrements the **link count** of the file referenced by pathname. 
  - If other process has opened the file, its contents will not be deleted. 
  - When the link count reaches 0, file content(data block, inode) is deleted.

- link count: 해당 i-노드를 가리키고 있는 directory entry의 수

<br>

## stat 구조체?

- <sys/stat.h> 에 정의

```c
struct stat {
    mode_t st_mode; /* file type & mode (permissions) */
    ino_t st_ino; /* i-node number (serial number) */
    dev_t st_dev; /* device number (filesystem) */
    dev_t st_rdev; /* device number for special files */
    nlink_t st_nlink; /* number of links */
    uid_t st_uid; /* user ID of owner */
    gid_t st_gid; /* group ID of owner */
    off_t st_size; /* size in bytes, for regular files */
    time_t st_atime; /* time of last access */
    time_t st_mtime; /* time of last modification */
    time_t st_ctime; /* time of last file status change */
    long st_blksize; /* best I/O block size */
    long st_blocks; /* number of 512-byte blocks */
};
```



<br>

## Link count

- i-node 의 link count 
  - stat 구조체의 st_nlink 
  - 이 i-node 를 가리키는 directory entry 수 
- 파일의 삭제
  - directory block에서 파일을 삭제하면 i-node 의 link count 가 1 감소
  - link count 가 0 에 도달하면 그 파일의 i-node 와 data block 이 삭제됨 
- 파일의 이동 
  - UNIX 명령어 mv 
  - 파일의 data block 이나 i-node 의 변화는 없이 directory entry만 변경됨'
  - i-node를 가리키던 directory entry만 변화한 것이다.(file1 -> file2)
    - 기존의 entry는 삭제되고 새로운 entry가 기존 entry가 가리키던 i-node를 가리키게 되는 것



<br>

## 링크의 구현

- link() 시스템 호출
  - 기존 파일 existing에 대한 새로운 이름 new 즉 링크를 맊든다. 
- hard link가 만들어짐 
  - 같은 i-node를 가리키는 directory entry가 하나 생김 
  - 그 i-node의 link count 가 하나 증가

![image](https://user-images.githubusercontent.com/79521972/162378780-afcf90ac-4e32-4a5b-bd39-ce53d5f1a669.png)

<br>

## link.c

```c
#include <unistd.h>
int main(int argc, char *argv[ ])
{
    if (link(argv[1], argv[2]) == -1) {
        exit(1);
    }
    exit(0);
}
```

```
$ link you.txt my.txt
$ ls –l you.txt my.txt
 2 -rw-r--r-- 2 chang faculty 2088 4월 16일 13:37 my.txt
 2 -rw-r--r-- 2 chang faculty 2088 4월 16일 13:37 you.txt
```

파일 이름만 다른 것을 볼 수 있다.

<br>

## unlink.c

```c
#include <unistd.h>
main(int argc, char *argv[ ])
{
    int unlink();
    if (unlink(argv[1]) == -1) {
        perror(argv[1]);
        exit(1);
    }
        exit(0);
}
```



<br>

## 하드 링크 vs 심볼릭 링크

- 하드 링크(hard link) 
  - 지금까지 살펴본 링크 
  - hard link points directly to the inode of the file. 
    - The link and the file should **reside** **in the same file system.** 
    - Only the superuser can create a hard link to a directory. 
- 심볼릭 링크(symbolic link) 
  - 소프트 링크(soft link) 
  - 실제 파일의 경로명을 저장하고 있는 링크 
  - symbolic link is an **indirect pointer** to a file 
    - There are **no file system limitations** on a symbolic link. 
    - Anyone can create a symbolic link to a directory 
  - 다른 파일 시스템에 있는 파일도 링크할 수 있다. 
    - 하드 링크의 단점 보완



<br>

### 심볼릭 링크

```c
int symlink (const char *actualpath, const char *sympath );
//심볼릭 링크를 만드는데 성공하면 0, 실패하면 -1을 리턴한다.
```

- Create a new directory entry, sympath that points to actualpath. 
  - Not require that actualpath exist when the symbolic link is created. 
  - Actualpath and sympath need not reside in the same file system.

```c
#include <unistd.h> => $ ls -s /usr/bin/gcc cc
int main(int argc, char *argv[ ]) $ ls –l cc
{ 
    if (symlink(argv[1], argv[2]) == -1) {
        exit(1);
    }
    exit(0);
}
```

```
$ slink /usr/bin/gcc cc
=> $ ls -s /usr/bin/gcc cc
$ ls –l cc
  2 lrw-r--r-- 1 chang faculty 2088
 	4월 16일 13:37 cc /usr/bin/gcc
```



<br>

## symlink()

- "In" vs. In -s"

<br>

![image](https://user-images.githubusercontent.com/79521972/162355503-68423ce1-c8d1-4b8a-80c0-1f44b9de56e9.png)

```
$ ln file1 file2
```

- 동일 inode를 포인팅 하는
- directory entry 추가

<br>

![image](https://user-images.githubusercontent.com/79521972/162355558-ac66702a-9aca-46b4-bcbe-8af45dfb95f7.png)

```
$ ln –s file1 file2
```

- directory entry 추가
- 별도 inode를 통해 기존 file1의 경로명을 갖는 데이터 블록 생성
- copy는 data block까지 copy하여 별도의 data block을 갖는데 link는 file1을 찾아가기 위한 경로명이 저장되어 있다.

<br>

- #### Dangling link
  
  - may point to an non-existing file

```c
//Example 1
$ ln -s /no/such/file myfile 		//create a symbolic link
$ ls myfile
myfile 								//ls says it's there
$ cat myfile 						//so we try to look at it
cat: myfile: No such file or directory
$ ls -l myfile 						//try -l option
lrwxrwxrwx 1 sar 	13 Jan 22 00:26 myfile -> /no/such/file
$
```

```c
//Example 2
$ ln -s testfile newfile
$ ls -l newfile
lrwxrwxrwx 1 yhshin users 8 Aug 27 20:02 newfile -> testfile
$ rm testfile
$ cat newfile
cat: newfile: No such file or directory
$
```



<br>

## 심볼릭 링크 내용

```c
#include <unistd.h>
int readlink (const char *path, char *buf, size_t bufsize);
//path 심볼릭 링크의 실제 내용을 읽어서 buf에 저장한다.
//성공하면 buf에 저장한 바이트 수를 반환하며 실패하면 –1을 반환한다.
```

```c
ssize_t readlink(const char *pathname, char *buf, size_t bufsize);
 //Returns: number of bytes read if OK, -1 on error 
```

<br>

- read value of a symbolic link 
  - places the contents of the symbolic link path in the buffer buf, which has size bufsiz. 
- Return value 
  - the count of characters placed in the buffer if it succeeds 
  - -1 if an error occurs
- buf size를 넘어가면 -1 에러 명시

<br>

## rlink.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char *argv[ ])
{
    char buffer[1024];
        int nread;
            nread = readlink(argv[1], buffer, 1024);
    if (nread > 0) {
        write(1, buffer, nread); //표준 출력
        exit(0);
    } else {
        fprintf(stderr, "오류 : 해당 링크 없음\n");
        exit(1);
    }
}

```

```
$ rlink cc
 /usr/bin/gcc
```

<br>



## remove()

```c
#include <stdio.h>

int remove(const char *pathname);
 //Returns: 0 if OK, -1 on error 
```

- Unlink a file or a directory 
  - For a file, identical to unlink. 
  - For a directory, identical to rmdir.

<br>

## rename()

```c
#include <stdio.h>

int rename(const char *oldname, const char *newname);
 //Returns: 0 if OK, -1 on error 
```

- Rename 
  - **renames** a file or a directory, **moving** it between directories if required. 
  - oldname and newname should be in the same file system.



<br>

## File times (revisit: 81p 참조)

- The three time values associated with each file

![image](https://user-images.githubusercontent.com/79521972/162356189-f8920e10-f85b-474a-80ae-0cc7fbe1cb7b.png)

- st_mtime: time the <span style="color:red">file contents</span> were last modified. 
- st_ctime: time the <span style="color:red">inode of the file</span> was last modified.



<br>

## utime()

```c
#include <utime.h>

int utime(const char *pathname, const struct utimbuf *times);
 //Returns: 0 if OK, -1 on error 
```

- Change the access time and modified time 
  - utimbuf structurev

```c
struct utimbuf {
    time_t actime; /* access time */
    time_t modtime; /* modification time */
}
```

- If times is NULL, the access and modification times of the file are set to the **current time**. 
- ctime field는 시스템 콜을 사용해도 변경이 되지만 utime()을 호출해서 시간적으로 변경이 되면 ctime 정보가 자동적으로 수정이 된다.
- st_ctime is automatically updated **when the utime is called**.
  - 시간 정보는 i-노드에 저장되어 있기 때문




<br>

- **example**

```c
#include "apue.h"
#include <fcntl.h>
#include <utime.h>
int
main(int argc, char *argv[])
{
    int i, fd;
    struct stat statbuf;
    struct utimbuf timebuf;
    
    for (i = 1; i < argc; i++) {
        if (stat(argv[i], &statbuf) < 0) { /* fetch current times */
            err_ret("%s: stat error", argv[i]);
            continue;
        }
        if ((fd = open(argv[i], O_RDWR | O_TRUNC)) < 0) { /* truncate */
            err_ret("%s: open error", argv[i]);
            continue;
        }
        close(fd); 	//시간 정보가 바뀜
        timebuf.actime = statbuf.st_atime;
        timebuf.modtime = statbuf.st_mtime;
        if (utime(argv[i], &timebuf) < 0) { /* reset times */ //open하기 전 시간 상태를 요구
            err_ret("%s: utime error", argv[i]);
            continue;
        }
    }
    exit(0);
}
```



<br>

- **실행**

```
$ ls -l changemod times 					look at sizes and last-modification times
-rwxrwxr-x 1 sar 15019 Nov 18 18:53 changemod
-rwxrwxr-x 1 sar 16172 Nov 19 20:05 times
$ ls -lu changemod times 					look at last-access times (마지막으로 access된 시간과 마지막으로 수정된 시간은 같다)
-rwxrwxr-x 1 sar 15019 Nov 18 18:53 changemod
-rwxrwxr-x 1 sar 16172 Nov 19 20:05 times
$ date 										print today's date
Thu Jan 22 06:55:17 EST 2004
$ ./a.out changemod times 					run the program in the previous page
$ ls -l changemod times 					and check the results
-rwxrwxr-x 1 sar 0 Nov 18 18:53 changemod	O_TRUNC로 파일이 지워짐
-rwxrwxr-x 1 sar 0 Nov 19 20:05 times
$ ls -lu changemod times 					check the last-access times also
-rwxrwxr-x 1 sar 0 Nov 18 18:53 changemod
-rwxrwxr-x 1 sar 0 Nov 19 20:05 times
$ ls -lc changemod times 					and the changed-status times
-rwxrwxr-x 1 sar 0 Jan 22 06:55 changemod	i-노드가 변경됨
-rwxrwxr-x 1 sar 0 Jan 22 06:55 times
$
```

<br>

# 핵심 개념

- 표준 유닉스 파일 시스템은 부트 블록, 슈퍼 블록, i-리스트, 데이터 블록 부분으로 구성된다 
- 파일 입출력 구현을 위해서 커널 내에 파일 디스크립터 배열, 파일 테이블, 동적 i-노드 테이블 등의 자료구조를 사용한다. 
- 파일 하나당 하나의 i-노드가 있으며 i-노드 내에 파일에 대한 모든 상태 정보가 저장되어 있다. 
- 디렉터리는 일련의 디렉터리 엔트리들을 포함하고 각 디렉터리 엔트리는 파일 이름과 그 파일의 i-노드 번호로 구성된다. 
- 링크는 기존 파일에 대한 또 다른 이름으로 하드 링크와 심볼릭(소프트) 링크가 있다. 

<br>

















