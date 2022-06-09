---
layout: single
title: "[System Programming] 5.1 파일 시스템 구현"
categories: ['System', 'System Programming']
tag: ['System Programming', 'File System']
---



# 5장. 파일 시스템

이번 장이 시험에 가장 많이 나오는 부분일 듯

## 5.1 파일 시스템 구현

### File System

- 배열과 같은 것으로 파일을 저장하기에는 무리가 있어 대용량 저장소의 필요성이 대두됨.

- File system을 사용하는 이유는 disk에 데이터를 저장을 해 두었다가 나중에 **편리하게** access해서 사용할 수 있도록 하는 것이다.

- File system resides on secondary storage (disks)
- File systems provide **effiecient** and **convenient** access to the disk by allowing data to be stored, located, and retrieved easily.

<br>

- File system 설계에 있어서 중요한 이슈.
  - **How** the file system should **look** to the user.
    - Provides **user interface** to storage, mapping logical to physical storage(OS가 관리하고 있는)
      - 즉, logical storage 에서 physical storage로 mapping 하는 것을 제공하는 것이다.
    - defines a **file** and its attributes, the **operations** allowed on a file, and the **directory structure** for organizing files. 
    
  - creating **algorithms and data structures** to map the logical file system onto the physical secondary-storage devices.
    - To improve I/O efficiency, **I/O transfer** is between MM and disks are performed **in units of block**.
      - 하드디스크는 너무 느려서 하드디스크에서 데이터를 읽거나 쓸 때는 block 단위로 (512바이트) 가져오게 된다.
    - 하드디스크 조각이 많아지면 파일을 찾을 때 저장 공간을 찾기 위해 막 찾아다녀야 하기 때문에 이럴 때 효율성이 많이 떨어진다.
  
  - fragment: 사용하지 않지만 사용하기엔 size가 작은 공간 -> 이러한 공간을 낭비하지 않도록 해야 함.
  
    - 가상 공간으로 해결?
  
    - fragmentation이 발생하지 않도록 해야함



<br>

### Disk의 물리적 구조

- 하드 디스크는 **surface, track, sector**로 구성되는 3D 구조(주소) 가 파일을 찾을 때 사용된다.
  - 3차원 구조에서 cylinder number = track number
  - **surface**는 위 아래로 있으므로 아래 그림에는 6개가 있음
  - surface 중에서 바깥 **track**과 안쪽 **track**이 나누어짐.
  - **sector**는 한 surface에 조밀 조밀하게 나누어져 있는데 그러한 구간을 sector라고 한다.
    - 한 block이 한 sector에 저장

- 파일 시스템은 디스크를 물리적인 구조로 보지 않고 **논리적인 디스크 블록들의 집합**으로 간주
  - 논리적이라는 말은 즉, 1차원으로 간주 한다는 의미

- OS는 디스크와의 정보 전송을 **블록 단위**로 수행
  - 디스크의 블록에는 **논리적인 주소**가 할당됨 (**1D 주소**) 
  - 이러한 논리적인 주소를 이용하여 디스크와 정보 전송을 하기 위해 디스크의 3D 구조를 크기가 일정한 논리 블록들의 집합인 1D 배열로 매핑하여 관리함. 
  - 논리 블록들의 1D 배열은 실제 디스크의 섹터로 매핑 
  - 디스크 블록의 번호와 대응되는 섹터들로 매핑 시키는 일은 디스크 Device Driver 또는 디스크 Controller가 담당함

![image](https://user-images.githubusercontent.com/79521972/160800483-dd38867f-56cb-4f01-894b-7b9efbec8eea.png)

- 한 sector에 한 block이 저장된다.
- 데이터를 읽어오고 싶다면 해당 주소에 접근해야 하는데 
- 그러기 위해서는 어떤 surface, 어떤 track, 어떤 sector에 속해 있는지를 가지고 나서 특정 sector에 접근할 수 있다.
- disk arm이 데이터를 읽어옴.

<br>

### File system 에서의 주소변환**(시험)**

- 디스크 내 3D 구조에서 1D 구조로의 매핑 (생각(이해)하기 편하게 하기 위함)

![image](https://user-images.githubusercontent.com/79521972/160800596-a4059662-5921-4062-b140-169468c25192.png)

- sector의 배열(1차원 구조)
  - file system에서 hard disk에 access 할 때는 3차원 구조를 사용하지만 사실상 file system 내부에서는 1차원 구조를 사용하게 된다.


- sector에 접근하여 다시 블락을 읽거나 write 할 때는 다시 3차원 주소로 번역하여 접근해야 한다.

<br>



### 파일 시스템 구조(중요)

파일이 많아짐에 따라 디렉터리 structure가 필요해졌는데 이보다 더 큰 개념을 파일 시스템이라고 한다.

![image](https://user-images.githubusercontent.com/79521972/160800695-5a8e6aa3-1927-4dec-bdae-6912f4f0d1dd.png)



- **부트 블록**(Boot control block)
  - contains info needed by system to boot OS from that volume(OS 부팅을 위한 정보를 담는 블럭)
  - 부트스트랩 코드가 저장되는 블록
  - 파일 시스템 시작부에 위치하고 보통 첫 번째 섹터 (block)를 차지
  - 부트 블록은 사실 하나만 있으면 되기 때문에 모든 file system이 갖고 있되 하나의 file system에서만 부팅을 하게 된다.(실제로 사용하는 것은 하나)
- 슈퍼 블록(Volume control block, superblock, master file table)**(시험)**
  - **전체 파일 시스템**에 대한 정보를 저장 (관리 정보)
  - Total # of blocks, 사용 중인 블록 수, # of free blocks, block size, 사용 가능 한 블록 비트 맵 (free block pointers or array), 사용 가능한 i-노드 개수
- i-리스트(i-list)
  - 각 파일을 나타내는 모든 i-노드들의 리스트
  - 한 블록은 약 40개 정도의 i-노드를 포함
  - 한 개의 파일은 한 개의 i-노드를 갖는다.
- 데이터 블록(Data block)
  - 파일의 내용(데이터)을 저장하기 위한 블록들
  - 실제 파일의 content를 저장하고 있는 부분



하나의 파일이 저장되기 위해서 한 개의 i-노드가 할당을 받고, 그 파일의 content를 저장하기 위해서 한 개 이상의 데이터 블록을 할당 받아서 저장된다.

<br>

### File

- File
  - **Logical storage unit**
  - Collection of related information
- **File data**와 File의 **i-node**(관리 정보, attribute)로 구성됨(리눅스에서 inode는 다른 곳에서는 file control block이라고 하기도 함.)

![image](https://user-images.githubusercontent.com/79521972/160801175-85ee8b77-fedf-49e5-8294-2c438debc8c4.png)

<br>

### Unix File system

- `inode 저장 영역`과 `data block 저장 영역`의 **분리**
- inode 크기는 일정 (attribute 정보이므로; 즉 format이 일관되어 있기 때문에)
  - i-number로 접근 가능 (아래 그림에서 0, 4, 6)
- 파일의 내용(file data)은 block 단위로 저장되게 된다
- ex) 한 개의 i-노드와 2 개의 data block으로 구성된 file

![image](https://user-images.githubusercontent.com/79521972/160801336-f714ec16-0eea-4b11-849d-4dd4843e35f7.png)





<br>

### Layered File System

![image](https://user-images.githubusercontent.com/79521972/160801419-5792db82-3c0d-4cb7-9e6f-27fe76bb2dbe.png)

- abstraction과 hierarchy를 이용하여 구현하는 것이 complex 시스템을 공략하기 효율적인 방법이다.

- application programs: 우리가 짜는 코드
- devices: 하드 디스크
- logical block number에서 3D로 바꾸는 과정

구현과 관리 용이로 file system도 계층 구조로 이루어져 있는 것이 일반적이다.

logical file system 부터 devices까지가 file system이다.

- <span style="color:red">왜 logical block에서 바로 physical block unmber를 거쳐서 3D로 변환하였는가?(시험에 나옴)</span>
  - 용량 때문에?
  - LBA는 하드디스크의 물리적인 구조를 생각하지 않고 각각의 sector가 일렬로 연결되어 있다고 논리적으로 생각하는 주소 접근 방식이다.
  - CHS(cylinder head sector) 를 사용하다 보니 주소 지정의 한계가 와서 최대로 사용할 수 있는 기억 장치의 용량의 제한이 생겨 대체된 방식이 LBA
  - https://rootfriend.tistory.com/entry/%ED%95%98%EB%93%9C%EB%94%94%EC%8A%A4%ED%81%AC%EC%9D%98-CHS%EC%99%80-LBA-%EA%B7%B8%EB%A6%AC%EA%B3%A0-CHS-LBA-Translator



---

이러한 장벽을 넘기 위해서 만들어진 것이 LBA 인터페이스입니다. LBA 인터페이스가 적용되면 바이오스는 기존의 CHS 방식 대신 LBA 방식을 사용해서 하드디스크 드라이브에 접근합니다. LBA는 그 이름 그대로, 물리적, 혹은 논리적(변수전환된) CHS 수치를 무시하고 하드디스크 전체를 하나의 영역으로 보면서 이 안에서 일련의 주소를 가지는 블록을 만듭니다. 그리고 이러한 블록의 주소를 지정하기 위한 정보로써 28bit를 사용합니다.

출처: https://rootfriend.tistory.com/entry/하드디스크의-CHS와-LBA-그리고-CHS-LBA-Translator [A Kind of Magic]



<br>

### File System Layers

- **Logical file system** manages metadata information
  - Metadata includes all of the File System structure except actual data
    - actual data는 I/O device에서만 의미있는 것이기 때문에 4개 layer는 metadata만 가지고 있는 것.
  
  - Directory management (manages directory structure)
  
  - **Translates file name into** file number, file handle(파일 디스크립터), location by maintaining **file control blocks** (<span style="color:blue">inodes </span>in Unix)
  
  - Protection
  
  - Logical block number 사용
  
  - Physical block(3D)은 이해하지 못하는 계층

<br>

- **File organization module** understands files, logical blocks as well as physical blocks
  
  - Translates **logical block #** to **physical block #** for the basic file system
    - 그래서 basic file system에게 physical block number를 넘겨줌
  
  - Manages free space (unallocated blocks), disk allocation
    - 사용되지 않는 공간을 관리하여, 파일이 저장 될 때 저장되는 공간을 할당해 주는 역할을 한다.
  - Logical block(1D)과 Physical block(3D)을 모두 이해하고 있는 계층
    - Logical to Physical convert
  - device layer에서는 3D, sector들을 주어진 원칙(계산식)에 의해서 3차원 주소를 일련번호를 붙여서 1D로 mapping
    - 배열 index가 physical block number
  
  
  

<br>

- **Basic file system** given command like “retrieve block 123” translates to device driver (mapping 1D address to 3D address)
  - 여기서 123은 physical block number
  - Also manages memory **buffers** and **caches** (allocation, freeing, replacement) 
    - Buffers hold data in transit(하드디스크에 바로 write하는 것이 아니라 buffer에 담아놨다가 전달)
      - 속도 문제로.
    - Caches hold frequently used meta data (최근 data를 버리지 않고 cache에 저장)
      - disk I/O 의 횟수를 줄일 수 있음.
  - Pysical block number 사용
  

<br>

- **Device drivers** manage I/O devices at the **I/O control layer**
  - Given commands lilk "read drive1, cylinder 72, track 2, sector 10, into memory location 1060” outputs low-level hardware specific commands to hardware controller 
    - harddisk controller를 구동 시켜서 disk arm이 데이터를 읽어오도록 control
  - main memory 1060번지 부터 copy를 요청 
  - device driver layer에서는 실제 hard disk에 hardware specific한 command를 전달하여 하드디스크가 동작하게 된다.

<br>

- **Layering useful** for reducing complexity(복잡도) and redundancy(중복), but adds overhead and can decrease performance (minimize code duplication)
  - Logical layers can be implemented by any coding method according to OS designer

- Many file systems, sometimes many within an operating system
  - Each with its own format 
    - CD-ROM is ISO 9660; 
    - Unix has UFS, FFS;
    - Windows has FAT, FAT32, NTFS, Linux has more than 40 types, with extended file system ext2 and ext3 leading; 
    - plus distributed file systems(분산 파일 시스템), etc

<br>

### i-node(i-node)

- inode는 각 file마다 하나씩 존재.
- 아래의 block array가 하나의 partition이라 가정.
- inode의 사이즈는 파일 사이즈와 무관하게 일정(file 하나당 i-노드 하나)

![image](https://user-images.githubusercontent.com/79521972/160802244-317064f7-a9a3-4ed1-bd94-fd6899e7cbff.png)



<br>

### File Control Block(i-node)

범용적인 용어는 File Control Block이고, Linux에서는 i-노드라고 부른다.

- 파일 관리를 위해 커널이 관리해야 할 정보
- 파일 속성(파일 시스템 인터페이스), 자료구조
  - 파일 속성들의 집합을 **메타데이터**라고 함
  - 메타데이터는 파일 내용 자체인 **데이터를 제외한** 모든 파일 시스템의 구조를 뜻하는 말임.

- 프로세스와 관련된 메타데이터는 PCB (Process Control Block)
- 파일 구조와 관련된 메타데이터는 <span style="color:red">FCB (File Control Block)</span>



<br>

- 파일을 관리하기 위하여 metadata를 kernel 공간에서 관리해야 함.
- 예를 들어, msword file을 open 하면 msword 실행 파일은 하드 디스크에 있는데 이를 읽어 메모리에 탑재 되는 것이고, 더불어 process control block이 생겨야 life cycle management가 가능하다.

![image](https://user-images.githubusercontent.com/79521972/160805581-a8eabc16-11ba-46d2-939c-4df880b913bd.png)

- 프로세스 실행을 위한 process control block이 생성됨
  - PCB안에 들어 있는 내용은 어떤 프로그램이냐를 구분하기 위한 pid와 우선순위 등이 들어있음.

<br>

- 한 **파일/디렉토리**는 반드시 하나의 i-노드를 갖는다.
  - 디렉토리도 사실상 파일의 한 종류

- 파일에 대한 모든 정보를 가지고 있음
  - 파일 타입: (e.g. 일반 파일, 디렉터리, 블록 장치, 문자 장치 등)
  - 파일 크기 (e.g. 16KB) 
  - 사용 권한 (e.g. rwxr--r--) 
  - 파일 소유자 및 그룹 (e.g. obama) 
  - 마지막 접근 및 갱신 시간
  - <mark>일반 파일: 데이터 블록에 대한 포인터(주소, 위치정보) </mark>
    - i-노드에서 데이터 블럭이 어디 있는지의 주소를 포함하고 있다.
  - 스페셜 파일: device number (e.g. /dev/hda0) 
  - **접근 위치** (e.g. offset) 
  - …

<br>

### i-node를 이용한 data block 탐색

![image](https://user-images.githubusercontent.com/79521972/160805928-9667e2d4-68e5-4f0d-b7b2-1c598cb9f063.png)

- 먼저 inode는 어떻게 찾을까?
  - 일반적으로 **Directory file** 안에 file name 및 inode number가 저장되어 있다.

![image](https://user-images.githubusercontent.com/79521972/160806036-dda9c96d-5b10-4a49-aed9-57481f30bb23.png)

- ex) etc 디렉토리: 4번 inode
- physical block number를 갖고 있는 것임(4, 11, 86)

<br>

### Allocation of blocks for files

**파일을 디스크에 저장하는 방법**

- Techniques for Assigning blocks to directory / files (파일을 저장할 block을 할당 받아야 함)
  - An allocation method refers to how disk blocks are allocated for files:
  - Directory entry for each file indicates the address of the starting block and the length allcocated for this file.
- **Should**
  - Utilize **disk space effectively** 
  - Provide **fast** **access** to file 
- Major technique 
  - Contiguous
  - Linked 
  - Indexed



<br>

### Contiguous(접촉하는) Allocation of Disk Space**(시험)**

![image](https://user-images.githubusercontent.com/79521972/160806387-7227fd43-3427-4dae-850f-81ab16a6ecce.png)



- 파일은 continuous하게 그 공간이 할당되어 있다. 예를 들어, count라는 파일은 0번 블락이 시작 지점이고 길이는 2이기 때문에 0,1 블럭에 저장되어 있다.


- 이렇게 시작 지점은 상관이 없지만 파일이 연속적인 공간에 block으로 할당 되는 방법이 `Contiguous` 이다.
- 위의 number는 시작 지점의 physical block number이다.

<br>

그런데 이와 같은 방법으로 할당을 하다보면 연속된 공간을 할당하려 하는데 해당 블럭이 이미 차지되어 있는 경우 할당하지 못하는 문제가 생긴다.

- 즉, external fragmenation이 많이 발생한다.(돌고 있는 공간이 많은데 사용하지 못하는 문제)**(시험)**

- 대신 read가 빠르다는 장점이 있긴 함 

<br>

### Linked Allocation of Disk Space

<img src="https://user-images.githubusercontent.com/79521972/160806520-2c596a80-b113-450e-95a1-3133279dd003.png" alt="image" style="zoom:67%;" />

- 연속된 블럭을 할당할 필요가 없기 때문에 위의 단점을 보완하였음.
  - external framentation이 발생하지 않는다.

- 연속적으로 block이 할당 되어 있지 않은 대신에 다음 block이 어디인지 정보가 들어있는 linking 정보가 따로 존재
- linking 정보를 계속 따라 들어가야 하기 때문에 **read 시간이 느림**
- disk 조각이 Contiguous 방식보다 덜 생긴다는 장점이 있다.

첫 번째 찾아간 것이 logical block number가 0인 것이고 두 번째 찾아간 것이 logical block number가 1인 것이다...

따라서 배열의 원소가 physical block number 인 것이고 배열의 index가 logical block number인 것이다.

- 그래서 왜 physical number를 거치는 것이지 ?? -> 

<br>

### Indexed Allocation of Disk Space**(시험)**

<img src="https://user-images.githubusercontent.com/79521972/160806584-2f70b8b8-cfd4-4c53-85c6-b0a7a687be36.png" alt="image" style="zoom:67%;" />

- file에 접근하기 위해서 **index block** 에 쓰여진 **number**를 보게 되는데 해당 번호의 block으로 가보면 여러 숫자가 적혀있는 table을 볼 수 있다.
  - 이 숫자들은 Linked allocation에서 사용한 sequence of block pointer를 의미
  - <span style="color:red">physical block number가 들어있는 배열</span>
  - <span style="color:red">배열의 index가 logical block 번호</span>
  
- 1번의 단점인 disk 조각 측면에서도 disk 조각을 덜 사용하고,

- 2번의 단점인 access가 느리다는 점에 대해서도 더 빨리 access 할 수 있는 방식이기 때문에

- 1,2번 방식의 보완된 방식이라고 보면된다.

- 그러나 굉장히 큰 size의 file의 경우 하나의 file index로는 부족할 수도 있다. 그렇기 때문에 다음과 같은 기능이 추가적으로 사용된다.
  - Linking index block: 다음 index block을 가리키는 주소가 들어있음.
  - Multi-level indexing: index block이 가리키는 주소로 가면 여러 개의 2단계 index block을 쫙 가리키고 있음.
  - Combined scheme - unix

<br>

### 블록 포인터**(시험)**

- 유닉스 파일 시스템에서 inode는 데이터 블록을 가리키는 블록 포인터 배열 을 포함
- 파일의 크기는 **가변적**임. 또한, 커널은 디스크에 블록단위로 I/O를 하며 데이터 블록은 불연속적인 위치에 저장됨. (disk 조각을 줄이기 위한 목적 때문에 연속은 사용 잘 안함)
- 가변적인 크기의 파일의 블록들을 블록 포인터 배열을 이용하여 찾아가는 방법은?
  - 다음 슬라이드의 경우 13개의 블록 포인터 배열 존재. 이 블록 포인터들 은 동작 방식에 따라 4가지 방식으로 구분. 
  - 직접(direct) : 파일의 데이터가 저장된 블록에 직접 접근 가능한 포인터 (indexed allocation)
  - 단일 간접(single indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 단일 포인터
    - 다른 index 블럭을 가리키고 있는 방식
  - 이중 간접(double indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 이중 포인터
  - 삼중 간접(triple indirect) : 파일의 데이터가 저장된 블록에 간접적으로 접근하는 삼중 포인터




<br>

### Unix file system의 indexing

- direct indexing method
  - 여러 개(10개)의 direct indexing 정보(block pointer)가 들어있음
- 

![image](https://user-images.githubusercontent.com/79521972/160806881-6212e68f-cc6e-4ade-bb39-8826d7498b49.png)

<br>

<img src="https://user-images.githubusercontent.com/79521972/160806584-2f70b8b8-cfd4-4c53-85c6-b0a7a687be36.png" alt="image" style="zoom: 50%;" />

- 그래서 이 방식은 direct indexing 방식임을 알 수 있다.(19번 index가 index배열(10 크기의)을 가리키고 있는 것)



#### single indirect

index block을 하나 둔 것





### 블록 포인터

![image](https://user-images.githubusercontent.com/79521972/160806934-03b35e01-2af2-4a98-9330-fec46b28b36e.png)

(삼중 간접 블록 포인터는 위 그림에 나와있지 않다.)

- 데이터 블록에 대한 포인터 
  - 파일의 내용을 저장하기 위해 할당된 데이터 블록의 주소

- 하나의 i-노드 내의 블록 포인터
  - 직접 블록 포인터 10개
  - 간접 블록 포인터 1개 (1,024 직접 블록) 
    - if 블록 포인터 크기 = 4B(32bit), 블록 크기 = 4,096B -> 그래서 1024개의 블럭 포인터 정보를 가질 수 있다.

  - 이중 간접 블록 포인터 1개 (1,024의 간접 블록 포인터 )
    - 최대 몇 개의 데이터 블록을 가리킬 수 있을까?(즉, 이 파일 시스템이 지원하는 파일의 최대 사이즈는?)
    - 아래에 나와 있음
  

<br>

### Block map

![image](https://user-images.githubusercontent.com/79521972/160807105-9de4dcf8-9837-46bd-b3d4-26a1b54ef6f7.png)

블럭 단위로는 몇개까지 지원가능할까?

-> 한 i-node 당 2058 개의 data block을 사용가능(0~2057)

<br>

### Block Pointers for Big File**(시험)**

- 각 블록의 크기를 4Kbyte라 가정한다면, 
  - 데이터 블록을 직접 가리킬 수 있는 직접 포인터는 10개 이므로 총 40Kbyte 크기 의 파일을 가리킬 수 있게 됨. 
  - 그렇다면 40Kbyte가 넘는 파일은 어떻게 가리킬 것인가?
    - 직접 포인터와 간접 포인터를 같이 이용함. 
    - 우선, 단일 간접 포인터가 가리키는 데이터 블록을 찾음.
    - 포인터 블록 크기가 4Kbyte이므로 1024개의 포인터(포인터는 4byte)를 간접적으로 사용하여 데이터 블록을 가리킬 수 있음.
    -  이 경우(single 방식) 1024*4KB의 크기 즉, **4MB**의 크기까지의 파일을  가리킬 수 있음. 
  - 만약, 파일의 크기가 4MB가 넘을 경우 **이중 간접 포인터**를 사용 
    - 단일 간접 포인터보다 레벨을 하나 더 두어서 총 1024\*1024*4KB가 되어 총 **4GB** 크기를 가지는 파일을 가리킬 수 있음. 
  - 4GB를 넘는 파일의 경우 **삼중 간접 포인터**를 사용
    - 1024\*1024\*1024*4KB의 크기 즉, **4TB**의 크기를 가지는 파일을 가리킬 수 있음.
    - 따라서, 블록의 크기가 4Kbyte일 경우 지원되는 파일의 최대 크기는 4KB + 4MB + 4GB + 4TB가 됨. 블록의 크기가 바뀌면 **파일의 최대 크기**도 바뀌게 됨
      - 3중 간접 포인터까지 지원 되기 때문에 4KB부터 4MB, 4GB, 4TB를 모두 더한 것이 지원하는 파일의 최대 크기임
      - 지원되는 n중 간접 포인터의 최대 크기만큼을 더해 주는 것



<br>

### Unix file system의 inode

- metadata에서 attribute외에 data block을 찾을 수 있는 pointer정보를 포함.

![image](https://user-images.githubusercontent.com/79521972/160807556-f9bab1ee-d167-45d5-b252-9b85f96afab7.png)



<br>

### File 경로명을 이용한 파일 탐색

- “/etc/inittab” 파일을 접근하려면?

  - ``'\' inode ``-> i-노드가 포함하고 있는 데이터 블락을 읽어온다.
  - ``'\' data block` -> root directory의 data block에는 현재 포함하고 있는 directorty 들의 대한 정보들이 들어있는데 그것들 중에서 내가 접근을 원하는 디렉토리(etc)의 i-node 정보를 또 다시 읽는다.
  - ``'etc\' inode` -> etc data block 포인터를 읽어 해당 data block에 접근
  - ``'etc\' data block`->
  - ``'inittab' inode` ->

  - `'inittab' data block`

![image](https://user-images.githubusercontent.com/79521972/160807866-ccc77a24-8797-49c1-8c74-7398fb5bcbb2.png)



- '\\' inode는 어떻게 찾는가?
  - **일반적으로 '\\'의 i-number는 0임.**



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

- X라는 파일을 open 하게 되면 하드디스크에 있는 i-노드를 가장 먼저 읽어온다.

- 그런데 하드디스크에서부터 i-노드 blcok 정보를 읽기 위해서는 어마어마한 I/O 를 거쳐야 하는데, <span style="color:red">이를 피하기 위해서 하드디스크의 i-노드를 main memory로 읽어오게 된다.</span>
  - 그래야 block pointer 정보를 쉽게 사용할 수 있고 디스크의 어마어마한 I/O를 하지 않기 위해

- 위 과정으로 m.m안에 **FCB**이 생기게 되는 것이고, 

- read를 하는 경우에 원하는 BUF_SIZE 만큼 데이터를 읽어와서 m.m에 탑재 시키는데 이때 이동된 offset의 정보가 FCB에 저장된다.



open -> 하드디스크 inode 정보(FCB)를 메인 메모리로 읽어옴

<br>

### 다수의 Process에 의한 File Open

- 동일 파일을 두 process가 사용하려면 2개의 FCB 필요.
  - process A가 권한 정보를 수정한다면?

<img src="https://user-images.githubusercontent.com/79521972/160809774-5cfccdf9-cddf-4d12-b352-d44f58129c84.png" alt="image" style="zoom:67%;" />

<br>

- <span style="color:red">두 process가 각각 metadata(FCB)를 수정한 후, 각각 디스크에 복사(override) 한다면? </span>
  - 정보 불일치 (inconsistency) 발생 가능성
  - 비효율적(inefficient)
- 가능하면 여러 프로세스일 때는 metadata를 공유하는 것이 바람직함.
  - 대부분의 i-노드 정보 (접근 권한, 파일의 크기, 유형)는 process 간 공유가 가능하다.
  - 하지만, offset은 공유 불가 (공유를 하면 당연히 안되겠지?)
    - 모든 process가 다른 위치에서 read/write 중이기 때문에. 
    - 이 정보는 process마다 별도로 관리해야 함.

<br>

### FCB 자료구조 분리의 필요성

- 소유자, 권한은 process간 공유 가능
- offset은 process간 공유 불가능

<img src="https://user-images.githubusercontent.com/79521972/160810570-cdc022cd-256a-4814-9a38-cee3ea6b37c2.png" alt="image" style="zoom:67%;" />



<br>

### File Table의 필요성

- offset만 따로 file table에 저장(공유가 불가능한 부분)
  - file table의 부분은 process마다 하나씩 생성되게 된다.
  - 즉, 각 process가 관리하는 부분

- 나머지 정보는 vnode에 저장
  - vnode는 프로세스끼리 공유가 가능한 부분의 정보를 모아둔 곳이고
  - vnode는 열린 file당 하나씩 생긴다.

![image](https://user-images.githubusercontent.com/79521972/163504544-10639ec8-e248-457d-925d-1a151071d866.png)

<br>

- **File Table** (Per-process Open File table) 
  - 프로세스가 File을 Open할 때마다 하나씩 생성. 
  - 내용
    - File status flags (read, write, append, sync, and nonblocking)
    - Offset 
    - vnode에 대한 포인터
- **vnode** 
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

- The v-node was invented by Peter Weinberger (Bell Lab.) and Bill Joy (Sun Microsystems) **to provide support for multiple file system types on a single computer system.** 
- Sun called this the **Virtual File System** and called the **file system–independent portion of the i-node** the **v-node** [Kleiman 1986]. 
- Linux has no v-node. (리눅스에는 vnode 개념이 없다!! 두둥!)
  - Instead, a generic i-node structure is used



<br>

### Virtual File System

- VFS allows the same system call interface (the API) to be used for different types of file systems 
  - Separates file-system generic operations from implementation details 
  - Implementation can be one of many file systems types, or network file system
    -  Implements vnodes which hold inodes or network file details 
  - Then dispatches operation to appropriate file system implementation routines

---

<br>

### File Descriptor Table의 필요성

- **file descriptor table**의 추가
  - disk에서 inode 내용을 메모리에 가져오는데 이 때 파일의 경로명을 통해서 가져오게 되는데 이 때 소모 되는 시간은 매우 크다.
  - open() 후에 자료 구조를 용이하게 접근하기 위해 (특히 read, write)
  - 그래서 file descriptor를 가지고 파일에 접근할 수 있도록 하는 것이다.

![image](https://user-images.githubusercontent.com/79521972/160811335-2816b897-c843-4036-9a94-9e73106164dc.png)





<br>

### 파일 입출력 구현

- 파일 입출력 구현을 위한 커널 내 자료구조
  - 파일 디스크립터 배열(Fd array, file descriptor table) 
  - 열린 파일 테이블(Open File Table) 
  - 동적 i-노드 테이블(Active i-node table); 위 그림에서 vnode
    - 리눅스에서 말하는 i노드에 해당하는 부분

<br>

### 파일을 위한 커널 자료 구조

```
fd = open(“file”, O_RDONLY); //디렉토리에서 파일의 inode 번호를 찾음
							 // inode를 동적 i-노드 테이블로 복사
```

![image](https://user-images.githubusercontent.com/79521972/160812684-841fdbb3-42f1-4ba5-981a-99b1a70f1c73.png)

- 디렉토리에서 파일의 inode 번호를 찾음 (디렉토리는 파일의 i-노드 정보를 포함한다.)
- 찾은 inode 번호를 파일 시스템(하드 디스크)으로부터 읽어서 **동적 i-노드 테이블**에 카피한다.
  - 얘가 vnode에 해당하는 것(공유가 가능한)

- **open file table**에 entry를 하나 만듦, 이 entry에서 동적 i-노드를 가리키는 포인터를 삽입하게 된다.
  - 얘가 file table로 변경이 일어나지 않는 곳이다. (공유가 불가능한, 프로세스당 하나)

- file descriptor table에서 file table의 entry를 가리키는 fd값를 하나 할당 받아야 하는데 파일을 처음 열었을 때 받는 번호는 3번부터 받게 된다.(0, 1, 2는 프로세스 실행 시 이미 자동으로 오픈된 파일) 
  - 여기서 부여 받는 index 값이 file descriptor가 된다.


<br>

### 파일 디스크립터 배열(Fd Array)

- 프로세스 당 하나씩 갖는다.
  - 파일 Open 때마다 엔트리 생성
  
  - 프로세스가 파일을 하나 오픈할 때마다 entry가 생성된다는 뜻인가?

- 파일 디스크립터 배열
  - 열린 파일 테이블의 엔트리를 가리킨다.

-  파일 디스크립터
  - 파일 디스크립터 배열의 인덱스
  - 열린 파일을 나타내는 번호

<br>

- file descriptor table 
  - Per process data structure 
  - fd = open(“/a/b”, …)
  - fd는 file을 접근하기 위한 파일 디스크립터 테이블의 index 값
  - fd는 0부터 시작하는 정수(file descriptor) 
  - 0, 1, 2는 standard input/output/error로 프로세스 실행시 할당 받기로 예약됨. 
    - 그래서 우리가 파일을 열면 3번부터 할당 받게 됨
  - 공유하고 있는 inode(열린 파일 table) 에 대한 포인터를 포함 
  - read와 write 시스템 콜을 호출하면
    - 이제는 경로명을 사용하여 접근하는 것이 아니라 커널은 파일 디스크립터를 사용하여 파일 디스크립터 테이블에 접근해 파일 테이블과 inode 의 해당 엔트리에 대한 포인터를 이용하여 접근한다.
    - 해당 inode를 찾게 되고 이로부터 파일에 있는 실제 데이터를 찾아냄
      - 동적 i-노드 테이블에는 데이터 블럭 포인터가 저장되어 있어서 그것으로 데이터 블럭에 접근하여 데이터를 가져온다.
      - 하드디스크의 i 리스트를 찾을 필요가 없다.(이게 오래 걸리는 건데 이거를 할 필요가 없어진다는 것이다.)

<br>

### 열린 파일 테이블 (Open File Table)

- 파일 테이블 (file table) 
  - 커널 자료구조 (**시스템에 1개** 생성) 
  - 파일 Open 때마다 엔트리 생성 
  - **열려진 모든 파일 목록** 
  - 열려진 파일 -> 파일 테이블의 항목
- 파일 테이블 항목 (file table entry)
  - **파일 상태 플래그** (read, write, append, sync, nonblocking,…) 
  - **파일의 현재 위치** (<mark>current file offset</mark>) – lseek 가 조정하는 값 
  - <mark>(active) i-node에 대한 포인터</mark>
    - active i node table을 가리키는 포인터

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

```shell
$ cat /home/obama/test.c
```

- `open(“/home/obama/test.c”, O_RDONLY)`

![image](https://user-images.githubusercontent.com/79521972/160813660-126819c8-91df-4ad2-b239-ae94eaf6a0d7.png)



#### how many disk access occurred?

<span style="color:red"> 시험 문제 나옴</span>**(시험)**

- 1. Pathname lookup (/home/obama/test.c)
     - i-노드에 저장되어 있는 데이터 블럭 포인터로 데이터 블럭을 계속해서 읽고 원하는 파일 혹은 디렉토리의 i-노드를 다시 가져와서 읽고 데이터 블록 포인터고 가고를 반복하는 구조
     - open 할 때는 file descriptor가 없기 때문에 i-노드에 무조건 접근해야 함.

![image](https://user-images.githubusercontent.com/79521972/160813803-c67f2e96-eb3b-450c-a3f5-46953e7d35f8.png)

최종적인 test.c 의 inode를 가져오기 위해서 총 4개의 inode를 가져오게 됨.

디렉토리/파일 접근까지 하여 총 7번의 disk I/O를 함으로써 가져오게 되었다.

- i-노드 복사: 4번
- data block 접근 : 3번

<br>

- 2. “test.c”를 위한 커널 내 자료 구조 완성**(시험)**
     - 메모리 내 i-node 생성(copy).
     - file table 생성. (offset은 0으로 설정)
     - File desciptor table에 entry 생성하고, file descriptor(3) 리턴
     - inode를 찾기위해서 disk I/O를 할 필요가 없어짐
       - file descriptor만 받으면 i-node에 한 번만 접근하여 데이터 블럭을 가져와 데이터에 접근 가능

![image](https://user-images.githubusercontent.com/79521972/160814037-e56c8105-c72b-48c2-b6f9-560cd9c35e32.png)



<br>

#### open()정리 - Pathname lookup 오버헤드

- open(“/a/b”, …)는 open하고자 하는 파일의 경로를 찾기 위해 여러 차례의 I/O 연산을 요구하는 오버헤드가 발생 
- 동일한 파일에 대해 반복해서 접근할 경우 **매번 경로 검색을 해야 하므로** 그 오버헤드는 감당할 수 없도록 더 커지게 됨. 
- 따라서 **경로 검색을 한번만 수행**(open 할 때 한 번), 파일 디스크립터에 경로를 변환하여 저장 
  - Pathname lookup은 한 번만 수행! 
  -  (pathname -> file descriptor) 변환 후 저장. 
  -  **fd** = open(“/a/b”, …) 
- 계속되는 시스템 콜 사용 시 path 대신 file descriptor로 사용. 
  - read(fd, …), write(fd, …), …

<br>

### On-Disk & In-Memory File System Structures

![image](https://user-images.githubusercontent.com/79521972/160814257-0357169c-fc45-4971-bd04-04e347c25681.png)



<br>

- An in-memory **directory-structure cache** holds the directory information of **recently accessed directories.** 
  - directory-structure cache: 하드디스크에 있던 내용을 읽어서 main memory에 가져다 놓은 것
  - Directory structure organizes the **files Names** and **inode numbers** 
  
- When a process opens the file 
  - First searches system-wide open file table (이미 사용되고 있는지 체크, 이미 열렸는지를 보는 것임)
    - If yes, Per-process table entry 생성, 해당 system-wide open file table pointing 하게 함. 
    - If no, 그 때서야 주어진 파일 이름으로 directory structure 검색(cache or disk) 
      - FCB를 system-wide open file table에 복사 후 Per-process table entry 생성 , 해당 system-wide open file table pointing하게 함 
  - Open returns a file descriptor for subsequent use (추가적인 사용 : read/write을 위한)
  - Data from read eventually copied to specified user process memory address

- When a process closes the file 
  - Per-process table entry is removed 
  - System-wide table entry’s **open count** is **decremented** 
  - 모든 프로세스가 모두 파일을 close하면 (open count = 0) 
    - Updated meta data is copied back **to disk directory structure**
    - System-wide table entry is removed

<br>

### 파일을 위한 커널 자료 구조**(시험)**

- 한 프로세스가 파일을 두 번 open 한 경우

- `fd = open(“file”, O_RDONLY); //두 번 open`

![image](https://user-images.githubusercontent.com/79521972/160814597-2f33b11f-146d-4677-9aa5-2094d3629569.png)

open file table의 entry가 하나 더 생기는데 이곳에는 현재 파일 위치와 같이 공유된 상태를 가질 수 없는 정보들을 포함한다.



<br>

- `fd = dup(3); 혹은 fd = dup2(3,4);`//fd 복제

![image](https://user-images.githubusercontent.com/79521972/160814631-ed7b6b54-76bc-4a8c-a398-589d959b616b.png)

<mark>dup()은 파일을 open하는 것이 아니기 때문에 open file table에 entry가 추가적으로 생기는 것이 아니라</mark> 새로운 file descriptor가 기존의 entry를 가리키는 행위를 한다.

<br>

### File sharing

- **Two independent processes** with the same file open

![image](https://user-images.githubusercontent.com/79521972/160814721-a543c510-23e9-41f4-9cdf-eab3584c0fa1.png)

두 프로세스가 파일을 각자 오픈을 한다고 하더라고 open file table은 각자의 table이 생기지만 동일한 i-노드를 가리키게 된다. (i-노드는 무조건 한 번만 생성되기 때문에)

---

파일 하나당 하나의 i-노드만을 갖는다는 것을 명심!

---

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

```shell
$ cat dup_result
hello world
```

standard 출력이 가리키는 것이 모니터가 아니라 file이기 때문에 실행 파일을 실행해도(`./a.out`)를 해도 printf의 결과가 STDOUT이기 때문에 출력이 되지 않지만 
cat명령어로 해당 파일을 열어보면 해당 내용은 잘 출력된다.




