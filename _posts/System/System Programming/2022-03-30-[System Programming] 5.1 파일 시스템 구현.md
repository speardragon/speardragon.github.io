---
layout: single
title: "[System Programming] 5.1 파일 시스템 구현"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File System']
---



# 5장 파일 시스템



## 5.1 파일 시스템 구현

### File System

- File system resides on secondary storage (disks)
- File systems provide effiecient and convenient access to the disk by allowing data to be stored, located, and retrieved easily.
- File system 설계 이슈.
  - how the file system should look to the user.
    - Provides **user interface** to storage, mapping logical to physical storage(OS가 관리하고 있는)
    - defines a **file** and its attributes, the **operations** allowed on a file, and the **directory structure** for organizing files. 
  - creating algorithms and data structures to map the logical file system onto the physical secondary-storage devices.
    - To improve I/O efficiency, I/O transfer is between MM and disks are performed in units of bock



<br>

### Disk의 물리적 구조

- 하드 디스크는 surface, track, sector로 구성되는 3D 구조(주소)
- 파일 시스템은 디스크를 물리적인 구조로 보지 않고 논리적인 디스크 블록들의 집합으로 간주
  - 1차원으로 간주

- OS는 디스크와의 정보 전송을 블록 단위로 수행
  - 디스크의 블록에는 논리적인 주소가 할당됨 (1D 주소) 
  - 이러한 논리적인 주소를 이용하여 디스크와 정보 전송을 하기 위 해 디스크의 3D 구조를 크기가 일정한 논리 블록들의 집합인 1D 배열로 매핑하여 관리함. 
  - 논리 블록들의 1D 배열은 실제 디스크의 섹터로 매핑 
  - 디스크 블록의 번호와 대응되는 섹터들로 매핑 시키는 일은 디스크 Device Driver 또는 디스크 Controller가 담당함

![image](https://user-images.githubusercontent.com/79521972/160800483-dd38867f-56cb-4f01-894b-7b9efbec8eea.png)

<br>

### File system 에서의 주소변환

- 디스크 내 3D 구조에서 1D 구조로의 매핑

![image](https://user-images.githubusercontent.com/79521972/160800596-a4059662-5921-4062-b140-169468c25192.png)

sector의 배열(1차원 구조)

- 그래서 사실상 file system 내부에서는 1차원 구조인 것이다.



<br>



### 파일 시스템 구조

![image](https://user-images.githubusercontent.com/79521972/160800695-5a8e6aa3-1927-4dec-bdae-6912f4f0d1dd.png)



- 부트 블록(Boot control block)
  - contains info needed by system to boot OS from that volume(OS 부팅)
  - 부트스트랩 코드가 저장되는 블록
  - 파일 시스템 시작부에 위치하고 보통 첫 번째 섹터 (block)를 차지
- 슈퍼 블록(Volume control block, superblock, master file table)
  - **전체 파일 시스템**에 대한 정보를 저장
  - Total # of blocks, 사용 중인 블록 수, # of free blocks, block size, 사용 가능 한 블록 비트 맵 (free block pointers or array), 사용 가능한 i-노드 개수
- i-리스트(i-list)
  - 각 파일을 나타내는 모든 i-노드들의 리스트
  - 한 블록은 약 40개 정도의 i-노드를 포함
- 데이터 블록(Data block)
  - 파일의 내용(데이터)을 저장하기 위한 블록들



<br>

### File

- File
  - Logical storage unit
  - Collection of related information
- File data와 File의 inode(관리 정보, attribute)로 구성됨

![image](https://user-images.githubusercontent.com/79521972/160801175-85ee8b77-fedf-49e5-8294-2c438debc8c4.png)

<br>

### Unix File system

- inode 저장 영역과 data block 저장 영역의 **분리**
- inode 크기는 일정 (attribute 정보이므로)
  - i-number로 접근 가능 (아래 그림에서 0, 4, 6)

![image](https://user-images.githubusercontent.com/79521972/160801336-f714ec16-0eea-4b11-849d-4dd4843e35f7.png)





<br>

### Layered File System

![image](https://user-images.githubusercontent.com/79521972/160801419-5792db82-3c0d-4cb7-9e6f-27fe76bb2dbe.png)

file system도 계층 구조로 이루어져 있는 것이 일반적이다.



<br>

### File System Layers

- **Logical file system** manages metadata information
  - Metadata includes all of the FS structure except actual data
  
  - Directory management(manages directory structure)
  
  - **Translates file name into** file number, file handle, location by maintaining **file control blocks** (<span style="color:blue">inodes </span>in Unix)
  
  - Protection
  
  - Logical block number 사용

<br>

- **File organization module** understands files, logical blocks as well as physical blocks
  - Translates **logical block #** to **physical block #** for the basic file system
  
  - Manages free space (unallocated blocks), disk allocation
    - 파일이 저장 될 때 저장되는 공간을 할당
  



- **Basic file system** given command like “retrieve block 123” translates to device driver (mapping 1D address to 3D address)
  - Also manages memory buffers and caches (allocation, freeing, replacement) 
    - Buffers hold data in transit(하드디스크에 바로 write하는 것이 아니라 buffer에 담아놨다가 전달)
    - Caches hold frequently used meta data
  - Pysical block number 사용



- **Device drivers** manage I/O devices at the **I/O control layer**
  - Given commands lile "read drive1, cylinder 72, track 2, sector 10, into memory location 1060” outputs low-level hardware specific commands to hardware controller 

- Layering useful for reducing complexity(복잡도) and redundancy(중복?), but adds overhead and can decrease performance (minimize code duplication)
  - Logical layers can be implemented by any coding method according to OS designer

- Many file systems, sometimes many within an operating system
  - Each with its own format 
    - CD-ROM is ISO 9660; 
    - Unix has UFS, FFS;
    - Windows has FAT, FAT32, NTFS, Linux has more than 40 types, with extended file system ext2 and ext3 leading; 
    - plus distributed file systems(분산 파일 시스템), etc

<br>

### i-node(i-node)

- inod는 각 file마다 하나씩 존재.
- 아래의 block array가 하나의 paartition이라 가정.
- inode의 사이즈는 파일 사이즈와 무관하게 일정

![image](https://user-images.githubusercontent.com/79521972/160802244-317064f7-a9a3-4ed1-bd94-fd6899e7cbff.png)



<br>

### File Control Block(i-node)

- 파일 관리를 위해 커널이 관리해야 할 정보
- 파일 속성(파일 시스템 인터페이스)
  - 파일 속성들의 집합을 **메타데이터**라고 함
  - 메타데이터는 파일 내용 자체인 데이터를 제외한 모든 파일 시스템의 구조를 뜻하는 말임.

- 프로세스와 관련된 메타데이터는 PCB(Process Control Block)
- 파일 구조와 관련된 메타데이터는 <span style="color:red">FCB(File Control Block)</span>



<br>

- 파일을 관리하기 위하여 metadata를 kernel 공간에서 관리해야 함.

![image](https://user-images.githubusercontent.com/79521972/160805581-a8eabc16-11ba-46d2-939c-4df880b913bd.png)



- 한 파일/디렉토리는 하나의 i-노드를 갖는다.
- 파일에 대한 모든 정보를 가지고 있음
  - 파일 타입: (e.g. 일반 파일, 디렉터리, 블록 장치, 문자 장치 등)
  - 파일 크기 (e.g. 16KB) 
  - 사용 권한 (e.g. rwxr--r--) 
  - 파일 소유자 및 그룹 (e.g. obama) 
  - 마지막 접근 및 갱신 시간
  - <mark>일반 파일: 데이터 블록에 대한 포인터(주소, 위치정보) </mark>
  - 스페셜 파일: device number (e.g. /dev/hda0) 
  - **접근 위치** (e.g. offset) 
  - …

<br>

### i-node를 이용한 data block 탐색

![image](https://user-images.githubusercontent.com/79521972/160805928-9667e2d4-68e5-4f0d-b7b2-1c598cb9f063.png)

- 그렇다면 inode는 어떻게 찾을까?
  - **Directory file**에 file name 및 inode number를 저장.

![image](https://user-images.githubusercontent.com/79521972/160806036-dda9c96d-5b10-4a49-aed9-57481f30bb23.png)

etc 디렉토리는 4번 inode...

<br>

### Allocation of blocks for files

파일을 디스크에 저장하는 방법

- Techniques for Assigning blocks to directory / files 
  - An allocation method refers to how disk blocks are allocated for files:
  - Directory entry for each file indicates the address of the starting block and the length allcocated for this file
- Should
  - Utilize disk space effectively 
  - Provide **fast** access to file 
- Major technique 
  - Contiguous
  - Linked 
  - Indexed



<br>

### Contiguous(접촉하는) Allocation of Disk Space

![image](https://user-images.githubusercontent.com/79521972/160806387-7227fd43-3427-4dae-850f-81ab16a6ecce.png)



count라는 파일은 0번 블락이 시작 지점이고 길이는 2이기 때문에 0,1 블럭에 저장된다. 그런데 연속된 공간을 할당하려 하는데 해당 블럭이 이미 차지되어 있는 경우 할당하지 못하는 문제가 생긴다.

대신 read가 빠름

<br>

### Linked Allocation of Disk Space

<img src="https://user-images.githubusercontent.com/79521972/160806520-2c596a80-b113-450e-95a1-3133279dd003.png" alt="image" style="zoom:67%;" />

- 연속된 블럭을 할당할 필요가 없기 때문에 위의 단점을 보완하였음.

- linking정보가 존재
- read 시간이 느림

<br>

### Indexed Allocation of Disk Space

<img src="https://user-images.githubusercontent.com/79521972/160806584-2f70b8b8-cfd4-4c53-85c6-b0a7a687be36.png" alt="image" style="zoom:67%;" />

1, 2번 방식을 보완한 방법

<br>

### 블록 포인터

- 유닉스 파일 시스템에서 inode는 데이터 블록을 가리키는 블록 포인터 배열 을 포함
- 파일의 크기는 **가변적**임. 또한, 커널은 디스크에 블록단위로 I/O를 하며 데이터 블록은 불연속적인 위치에 저장됨. 
- 가변적인 크기의 파일의 블록들을 블록 포인터 배열을 이용하여 찾아가는 방법은?
  - 다음 슬라이드의 경우 13개의 블록 포인터 배열 존재. 이 블록 포인터들 은 동작 방식에 따라 4가지 방식으로 구분. 
  - 직접(direct) : 파일의 데이터가 저장된 블록에 직접 접근 가능한 포인터 (indexed allocation)
  - 단일 간접(single indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 단일 포인터
  - 이중 간접(double indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 이중 포인터
  - 삼중 간접(triple indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 삼중 포인터




<br>

### Unix file system의 indexing

![image](https://user-images.githubusercontent.com/79521972/160806881-6212e68f-cc6e-4ade-bb39-8826d7498b49.png)





### 블록 포인터

![image](https://user-images.githubusercontent.com/79521972/160806934-03b35e01-2af2-4a98-9330-fec46b28b36e.png)

삼중 간접 블록 포인터는 나와있지 않다.

- 데이터 블록에 대한 포인터 
  - 파일의 내용을 저장하기 위해 할당된 데이터 블록의 주소

- 하나의 i-노드 내의 블록 포인터
  - 직접 블록 포인터 10개
  - 간접 블록 포인터 1개 (1,024 직접 블록) 
    - if 블록 포인터 크기 = 4B(32bit), 블록 크기 = 4,096B) -> 그래서 1024개의 블럭

  - 이중 간접 블록 포인터 1개 (1,024의 간접 블록 포인터 )

- 최대 몇 개의 데이터 블록을 가리킬 수 있을까?(이 파일 시스템의 최대 사이즈는?)
  - 아래에 나와 있음

<br>

### Block map

![image](https://user-images.githubusercontent.com/79521972/160807105-9de4dcf8-9837-46bd-b3d4-26a1b54ef6f7.png)

### Block Pointers for Big File

- 각 블록의 크기를 4Kbyte라 가정한다면, 
  - 데이터 블록을 직접 가리킬 수 있는 직접 포인터는 10개 이므로 총 40Kbyte 크기 의 파일을 가리킬 수 있게 됨. 
  - 그렇다면 40Kbyte가 넘는 파일은 어떻게 가리킬 것인가?
    - 직접 포인터와 간접 포인터를 같이 이용함. 
    - 우선, 단일 간접 포인터가 가리키는 데이터 블록을 찾음.
    - 포인터 블록 크기가 4Kbyte이므로 1024개의 포인터(포인터는 4byte)를 간접적 으로 사용하여 데이터 블록을 가리킬 수 있음.
    -  이 경우 1024*4KB의 크기 즉, 4MB의 크기까지의 파일을 (single 방식으로) 가리킬 수 있음. 
  - 만약, 파일의 크기가 4MB가 넘을 경우 **이중 간접 포인터**를 사용 
    - 단일 간접 포인터보다 레벨을 하나 더 두어서 총 1024\*1024*4KB가 되어 총 4GB 크기를 가지는 파일을 가리킬 수 있음. 
  - 4GB를 넘는 파일의 경우 **삼중 간접 포인터**를 사용
    - *1024*1024*1024*4KB의 크기 즉, 4TB의 크기를 가지는 파일을 가리킬 수 있음.
    - 따라서, 블록의 크기가 4Kbyte일 경우 지원되는 파일의 최대 크기는 4KB + 4MB + 4GB + 4TB가 됨. 블록의 크기가 바뀌면 **파일의 최대 크기**도 바뀌게 됨
      - 지원되는 n중 간접 포인터의 최대 크기만큼을 더해 주는 것



<br>

### Unix file system의 inode

- attribute외에 data block을 찾을 수 있는 pointer정보를 포함.

![image](https://user-images.githubusercontent.com/79521972/160807556-f9bab1ee-d167-45d5-b252-9b85f96afab7.png)



<br>

### File 경로명을 이용한 파일 탐색

- “/etc/inittab” 파일을 접근하려면?

  - ``'\' inode ``->
  - ``'\' data block` ->
  - ``'etc\' inode` ->
  - ``'etc\' data block`->
  - ``'inittab' inode` ->

  - `'inittab' data block`

![image](https://user-images.githubusercontent.com/79521972/160807866-ccc77a24-8797-49c1-8c74-7398fb5bcbb2.png)



- '\\' inode는 어떻게 찾는가?
  - 일반적으로 '\\'의 i-number는 0임.



<br>

### File I/O system call review

-  `fd = open(“/etc/inittab”, O_RDONLY);`

![image](https://user-images.githubusercontent.com/79521972/160808270-5e8f79c0-c740-4f8e-89ef-247a99f87bb0.png)

- `nread = read(3, buffer, BUF_SIZE);`

![image](https://user-images.githubusercontent.com/79521972/160808361-1e90812c-c916-4f00-9db6-9f0892eb0841.png)



- `npos = lseek(3, 4096, SEEK_CUR);`

![image](https://user-images.githubusercontent.com/79521972/160808468-b0f2458e-3371-49d6-8972-c4a14f283e43.png)



- open과 read

<img src="https://user-images.githubusercontent.com/79521972/160808528-5a2cd3fd-e6f6-497c-b4f7-d943036831aa.png" alt="image" style="zoom:67%;" />

open -> 하드디스크 inode 정보(FCB)를 읽어옴

<br>

### 다수의 Process에 의한 File Open

- 동일 파일을 두 process가 사용하려면 2개의 FCB 필요.
  - process A가 권한 정보를 수정한다면?

<img src="https://user-images.githubusercontent.com/79521972/160809774-5cfccdf9-cddf-4d12-b352-d44f58129c84.png" alt="image" style="zoom:67%;" />

<br>

- <span style="color:red">두 process가 각각 metadata(FCB)를 수정한 후, 각각 디스크에 복사 한다면? </span>
  - 정보 불일치 (inconsistency) 발생 가능성
  - 비효율적(inefficient)
- 가능하면 여러 프로세스가 metadata를 공유하는 것이 바람직함.
  - 접근 권한, 파일의 크기, 유형 등은 process 간 공유 가능
  - 하지만, offset은 공유 불가 
    - 모든 process가 다른 위치에서 read/write 중. 
    - 이 정보는 process마다 별도로 관리해야 함.



<br>

### FCB 자료구조 분리의 필요성

- 소유자, 권한은 process간 공유 가능
- offset은 process간 공유 불가능

<img src="https://user-images.githubusercontent.com/79521972/160810570-cdc022cd-256a-4814-9a38-cee3ea6b37c2.png" alt="image" style="zoom:67%;" />



<br>

### File Table의 필요성

- offset만 따로 file table에 저장
- 나머지 정보는 vnode에 저장

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220330192457645.png" alt="image-20220330192457645" style="zoom:67%;" />





- File Table (Per-process Open File table) 
  - 프로세스가 File을 Open할 때마다 하나씩 생성. 
  - 내용
    - File status flags (read, write, append, sync, and nonblocking)
    - Offset 
    - vnode에 대한 포인터 
- vnode 
  -  Vnode 정보 
    - 파일 타입 등 
  -  Inode 정보 : 디스크의 inode에서 읽어 옴.
    -  접근 권한(protection mode) 
    -  소유자(owner) 
    - 크기(size) 
    - 시간(time)
    - **디스크 상의 data block 위치**



<br>

#### vnode

- The v-node was invented by Peter Weinberger (Bell Lab.) and Bill Joy (Sun Microsystems) to provide support for multiple file system types on a single computer system. 
- Sun called this the **Virtual File System** and called the file system–independent portion of the i-node the **v-node** [Kleiman 1986]. 
- Linux has no v-node. 
  - Instead, a generic i-node structure is used



<br>

### Virtual File System

- VFS allows the same system call interface (the API) to be used for different types of file systems 
  - Separates file-system generic operations from implementation details 
  - Implementation can be one of many file systems types, or network file system
    -  Implements vnodes which hold inodes or network file details 
  - Then dispatches operation to appropriate file system implementation routines



# 5.2

---

<br>

### File Descriptor Table의 필요성

- file descriptor table의 추가
  - open() 후 자료 구조를 용이하게 접근하기 위해

![image](https://user-images.githubusercontent.com/79521972/160811335-2816b897-c843-4036-9a94-9e73106164dc.png)





<br>

### 파일 입출력 구현

- 파일 입출력 구현을 위한 커널 내 자료구조
  - 파일 디스크립터 배열(Fd array, file descriptor table) 
  - 열린 파일 테이블(Open File Table) 
  - 동적 i-노드 테이블(Active i-node table); 위 그림에서 vnode
    - 리눅스에서는 i노드



<br>

### 파일을 위한 커널 자료 구조

```
fd = open(“file”, O_RDONLY); //디렉토리에서 파일의 inode 번호를 찾음
							 // inode를 동적 i-노드 테이블로 복사
```

![image](https://user-images.githubusercontent.com/79521972/160812684-841fdbb3-42f1-4ba5-981a-99b1a70f1c73.png)

- 디렉토리에서 파일의 inode 번호를 찾음
- 찾은 inode 번호를 파일 시스템(하드 디스크)으로부터 읽어서 동적 i-노드 테이블에 카피한다.
- open file table에 entry를 하나 만듦, 이 entry에서 i-노드를 가리키는 포인터를 삽입하게 된다.
- file descriptor table에서 위의 entry를 가리키는 entry를 하나 할당 받아야 하는데 3번부터 받게 된다.(0,1,2는 자동으로 오픈되는 파일) 이 index 값이 file descriptor



### 파일 디스크립터 배열(Fd Array)

- 프로세스 당 하나씩 갖는다.
  - 파일 Open 때마다 엔트리 생성



- 파일 디스크립터 배열
  - 열린 파일 테이블의 엔트리를 가리킨다.



-  파일 디스크립터
  - 파일 디스크립터 배열의 인덱스
  - 열린 파일을 나타내는 번호

<br>

- file descriptor table 
  - Per process data structure 
  - fd = open(“/a/b”, …)
  - fd는 file을 접근하기 위한 파일 디스크립터 테이블의 index 
  - fd는 0부터 시작하는 정수(file descriptor) 
  - 0, 1, 2는 standard input/output/error로 예약됨. 
    - 그래서 우리가 파일을 열면 3번부터 할당 받게 됨
  - 공유하고 있는 inode 에 대한 포인터 포함 
  - read와 write 시스템 콜을 호출하면
    - 커널은 파일 디스크립터를 사용하여 파일 디스크립터 테이블에 접근해 파일 테이블과 inode 의 해당 엔트리에 대한 포인터를 이용 
    - 해당 inode를 찾게 되고 이로부터 파일에 있는 실제 데이터를 찾아냄
      - 하드디스크의 i 리스트를 찾을 필요가 없다.

<br>

### 열린 파일 테이블 (Open File Table)

- 파일 테이블 (file table) 
  - 커널 자료구조 (**시스템에 1개** 생성) 
  - 파일 Open 때마다 엔트리 생성 
  - 열려진 모든 파일 목록 
  - 열려진 파일 -> 파일 테이블의 항목
- 파일 테이블 항목 (file table entry)
  - **파일 상태 플래그** (read, write, append, sync, nonblocking,…) 
  - **파일의 현재 위치** (current file offset) – lseek 가 조정하는 값 
  - i-node에 대한 포인터

<br>

### 동적 i-노드 테이블(Active i-node table)

- 동적 i-노드 테이블 
  - 커널 내의 자료 구조 (시스템에 1개 생성) 
  - Open 된 파일들의 i-node를 저장하는 테이블 
- i-노드 
  - 하드 디스크에 저장되어 있는 파일에 대한 자료구조 
    - 파일의 i-노드는 디렉토리에 저장되어 있음 
  - 한 파일에 하나의 i-node 
  - 하나의 파일에 대한 정보 저장 
    - 소유자, 크기 
    - 파일이 위치한 장치
    - 파일 내용 디스크 블럭에 대한 포인터 
- i-node table vs. i-node



<br>

### open() 정리

```
$ cat /home/obama/test.c
```

- `open(“/home/obama/test.c”, O_RDONLY)`

![image](https://user-images.githubusercontent.com/79521972/160813660-126819c8-91df-4ad2-b239-ae94eaf6a0d7.png)



<br>

#### how many disk access occurred?

- 1. Pathname lookup (/home/obama/test.c)
     - 데이터 블럭을 계속해서 읽고 가져오는 구조

![image](https://user-images.githubusercontent.com/79521972/160813803-c67f2e96-eb3b-450c-a3f5-46953e7d35f8.png)

최종적인 test.c 의 inode를 가져오기 위해서 총 4개의 inode를 가져오게 됨.

디렉토리/파일 접근까지 하여 총 7번의 disk I/O를 함으로써 가져오게 되었다.

<br>

- 2. “test.c”를 위한 커널 내 자료 구조 완성
     - 메모리 내 inode 생성.
     - file table 생성. (offset을 0으로 설정)
     - File desciptor table에 entry 생성하고, file descriptor 리턴.
     - inode를 찾기위해서 disk I/O를 할 필요가 없어짐

![image](https://user-images.githubusercontent.com/79521972/160814037-e56c8105-c72b-48c2-b6f9-560cd9c35e32.png)



<br>

#### Pathname lookup 오버헤드

- open(“/a/b”, …)는 open하고자 하는 파일의 경로를 찾기 위해 여러 차례의 I/O 연산을 요구하는 오버헤드가 발생 
- 동일한 파일에 대해 반복해서 접근할 경우 매번 경로 검색을 해야 하므로 그 오버헤드는 감당할 수 없도록 더 커지게 됨. 
- 따라서 **경로 검색을 한번만 수행**, 파일 디스크립터에 경로를 변환하여 저장 
  - Pathname lookup은 한 번만 수행! 
  -  (pathname  file descriptor) 변환 후 저장. 
  -  **fd** = open(“/a/b”, …) 
- 계속되는 시스템 콜 사용 시 path 대신 file descriptor 사용. 
  - read(fd, …), write(fd, …), …

<br>

### On-Disk & In-Memory File System Structures

![image](https://user-images.githubusercontent.com/79521972/160814257-0357169c-fc45-4971-bd04-04e347c25681.png)



<br>

- An in-memory **directory-structure cache** holds the directory information of recently accessed directories. 
  - directory-structure cache: 하드디스크에 있던 내용을 읽어서 main memory에 가져다 놓은 것
  - Directory structure organizes the **files Names** and **inode numbers** 
  
- When a process opens the file 
  - First searches system-wide open file table (이미 사용되고 있는지 체크)
  - If yes, Per-process table entry 생성, 해당 system-wide open file table pointing 하게 함. 
  - If no, 주어진 파일 이름으로 directory structure 검색(cache or disk) 
    - FCB를 system-wide open file table에 복사 후 Per-process table entry 생성 , 해당 system-wide open file table pointing하게 함 
  - Open returns a file descriptor for subsequent use (read/write을 위한)
  - Data from read eventually copied to specified user process memory address

- When a process closes the file 
  - Per-process table entry is removed 
  - System-wide table entry’s **open count** is **decremented** 
  - 모든 프로세스가 모두 파일을 close하면 (open count = 0) 
    - Updated meta data is copied back to disk directory structure
    - System-wide table entry is removed



<br>

### 파일을 위한 커널 자료 구조

- 한 프로세스가 파일을 두 번 open 한 경우

- `fd = open(“file”, O_RDONLY); //두 번 open`

![image](https://user-images.githubusercontent.com/79521972/160814597-2f33b11f-146d-4677-9aa5-2094d3629569.png)

open file table의 entry가 하나 더 생김



<br>

- `fd = dup(3); 혹은 fd = dup2(3,4);`//fd 복제

![image](https://user-images.githubusercontent.com/79521972/160814631-ed7b6b54-76bc-4a8c-a398-589d959b616b.png)



<br>

### File sharing

- **Two independent processes** with the same file open

![image](https://user-images.githubusercontent.com/79521972/160814721-a543c510-23e9-41f4-9cdf-eab3584c0fa1.png)

동일한 i-노드를 가리키게 됨(한 번만 생성되기 때문)

<br>

### dup() and dup2()

- Example

```c
#include <unistd.h> //header file that provides access to the POSIX API
#include <fcntl.h>
int main(void)
{
    int fd;
    fd = creat("dup_result", 0644);
    dup2(fd, STDOUT_FILENO);
    close(fd);
    printf("hello world\n");
    return 0;
}
```

![image](https://user-images.githubusercontent.com/79521972/160814824-03d5baf7-bccd-4324-8994-e6fa3e482f30.png)

- 실행

```
$ cat dup_result
hello world
```

standard 출력이 가리키는 것이 모니터가 아니라 file이기 때문에 printf를 해도 출력이 되지 않지만 cat명령어로 파일을 열면 해당 내용이 출력되는 모습이다.



<br>















