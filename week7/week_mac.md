# File Implementation

---

> [toc]



# 1. 디스크에 파일을 저장하는 방식

1. Contiguous Allcocation
2. Linked Allocation
3. Indexed Allocation

cf) 파일은 크기가 균일하지 않다.
디스크에다가 파일을 저장할 때는 **동일한 크기의 Sector 단위로 나누어서 저장**하고 있다.
동일한 저장단위를 **논리적인 블록**이라고 본다.
임의의 크기의 파일을 동일 크기의 블록단위로 나누어서 저장하고 있다.

## 1) 연속 할당 (Contiguous Allcocation)

> 하나의 파일이 디스크 상에 연속해서 저장되는 방식

![ContiguousAllocation](https://jhi93.github.io/assets/img/os/ContiguousAllocation.png)
\* 장/단점

- 장점 : ① 빠른 I/O가 가능하다.
  ( 디스크 접근 시간은 대부분은 Head가 이동하는 시간이기 때문인데
  연속 할당 되어 있다면 한 번 Seek로 많은 양의 데이터를 읽고 쓸 수 있다.)
  ② Swapping과 같이 속도가 중요할 때 효과적이다.
  ③ 직접 접근이 가능하다.
- 단점 : ① 외부 단편화 ( 중간중간 Hole이 생긴다)
  ② 파일의 크기를 키우는데 제약이 존재
  => 미리 할당해두는 방식은? 내부 단편화이 생김.

## 2) 링크연결 할당 (Linked Allocation)

> 하나의 파일이 디스크 상에 빈 곳 아무데나 저장되는 방식

![LinkedAllocation](https://jhi93.github.io/assets/img/os/LinkedAllocation.png)

\* 장/단점

- 장점 :
  ① 외부 단편화 문제 발생하지 않는다.
- 단점 :
  ① 직접 접근이 안된다.
  ( ex. 중간 위치를 보기 위해서는 앞에서 부터 순차 접근해야함)
  ② 디스크의 섹터들이 간혹 Dead Sector가 되는데 이러면 뒷 부분을 접근할 수 없게 된다.
  ③ 한 섹터당 512 바이트 배수로 저장하는데 4 바이트를 포인터를 사용해서 두 섹터를 사용할 수도 있게되므로 공간 낭비를 할 수 있다.

=> 약간 변형해서 효율적으로 바꾼다.
**File-Allocation Table (FAT)**

## 3) 인덱스 할당 (Indexed Allocation)

> 인덱스 블록에 디스크 상에 어디어디 저장되어있는지 표시하는 방식

![IndexedAllocation](https://jhi93.github.io/assets/img/os/IndexedAllocation.png)

\* 장/단점

- 장점 : ① 외부 단편화 발생하지 않는다.
  ② 직접 접근이 가능하다.
- 단점 : ① 아무리 작은 사이즈라고 하더라도 인덱스를 위한 블록과 실제 데이터를 저장하기 위한 블록이 필요하다.
  ② 큰 사이즈의 파일인 경우 하나의 인덱스 블록으로는 표시가 불가능하다.

# 2. 유닉스 파일 시스템 구조

![UNIX1](https://jhi93.github.io/assets/img/os/UNIX1.png)
![UNIX2](https://jhi93.github.io/assets/img/os/UNIX2.png)

- **Boot block** : 부팅에 대한 정보
- **Super block** : 어디가 빈 블록이고 실제로 사용중인 블록인지 총체적으로 관리
- **Inode list**(파일 하나당 Inode 하나) : File meta data는 그 파일을 가지고 있는 디렉토리에 가면 기록되어있다고 했는데 실제 파일시스템에서는 디렉토리가 다 가지고 있지 않고 별도의 위치에 저장
  (디렉토리에 파일 이름과 Inode번호가 저장)

# 3. FAT File System

![FAT](https://jhi93.github.io/assets/img/os/FAT.png)

- **FAT** : 파일의 메타 데이터 일부를 이 곳에 보관
  (위치정보만! 나머지는 디렉토리가 가지고 있다)
  FAT 배열의 크기는 디스크가 관리하는 데이터 블록의 개수만큼 이며, 그 블록의 다음 블록이 어디인지를 담고 있다.
  ***FAT은 Linked Allocation의 단점을 모두 극복한다.\***

# 4. Free-Space Management

> 비어 있는 블록을 어떻게 관리할 것인가?

![FreeSapceManagement](https://jhi93.github.io/assets/img/os/FreeSapceManagement.png)

- **Bit map or bit vector** : 블록번호를 이용하여 사용중인지 비어있는지 비트로 표시

- Linked list

   

  : 비어있는 블록들을 연결해둔다.

  - 모든 free block들을 링크로 연결(free list)
  - 연속적인 가용공간을 찾는 것은 쉽지 안핟.
  - 공간의 낭비가 없다.

- Grouping

   

  : 첫 번째 비어있는 위치가 인덱스 역할->비어있는 곳들을 가르킴

  - Linked list방법의 변형
  - 첫 번째 free block이 n개의 pointer를 가짐
    - n-1 pointer는 free data block을 가리킴
    - 마지막 pointer가 가리키는 block은 또 다시 n pointer를 가짐

- Counting

   

  : 빈 블록의 위치를 가르키고 거기에 몇 개가 빈 블록이라는 것을 쌍으로 갖는다.

  - 프로그램들이 종종 여러개의 연속적인 block을 할당하고 반납한다는 성질에서 착안 ( Linked, Grouping은 연속적인 부분을 찾기 어렵지만 Counting은 가능)

# 5. Directory Implementation

> 디렉토리는 그 디렉토리 밑에 있는 파일의 메타데이터를 관리하는 특별한 파일.
> 디렉토리 내용을 어떻게 저장할 것인가, 디렉토리를 어떻게 구현할 것인가?

- Linear List
  - <file name, file의 meta data> 의 list
  - 구현이 간단
  - 디렉토리 내에 파일이 있는지 찾기 위해서는 Linear Serach 필요
- Hash Table
  - Linear List + Hashing
  - Hash Table은 File Name을 이 파일의 Linear List의 위치로 바꾸어줌
  - Search Time을 없앰
  - Collision 발생 가능

\* File의 meta data의 보관 위치

- 디렉토리 내에 직접 보관
- 디렉토리에는 포인터를 두고 다른 곳에 보관
  - inode, FAT 등

\* Long File Name의 지원

- <file name, file의 meta data>의 list에서 각 entry는 일반적으로 고정 크기
- file name이 고정 크기의 entry 길이보다 길어지는 경우 entry의 마지막 부분에 이름의 뒷부분이 위치한 곳의 포인터를 두는 방법
- 이름의 나머지 부분은 동일한 directory file의 일부에 존재

# 6. VFS and NFS

- Virtual File System (VFS)
  - 서로 다른 다양한 File System에 대해 동일한 시스템 콜 인터페이스(API)를 통해 접근할 수 있게 해주는 OS의 layer
- Network File System (NFS)
  - 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음
  - NFS는 분산 환경에서의 대표적인 파일 공유 방법임

![VFSandNFS](https://jhi93.github.io/assets/img/os/VFSandNFS.png)

# 7. Page Cache와 Buffer Cache

## 1) Page Cache

- Virtual Memory의 Paging System에서 사용하는 Page Frame을 Caching의 관점에서 설명하는 용어
- Memory-Mapped I/O를 쓰는 경우 File의 I/O에서도 Page Cache 사용

\* **Memory-Mapped I/O**

- File의 일부를 Virtual Memory에 Mapping 시킴
- 매핑시킨 영역에 대한 메모리 접근 연산은 파일의 입출력을 수행하게 함
- 파일을 접근할 때 read/write 콜을 하지 않고 파일의 일정부분을 메모리 영역에 맵핑해서 사용

## 2) Buffer Cache

- 파일 시스템을 통한 I/O 연산은 메모리의 특정 영역인 Buffer Cache 사용
- File 사용의 Locality 활용
  - 한번 읽어온 block에 대한 후속 요청시 Buffer Cache에서 즉시 전달
- 모든 프로세스가 공용으로 사용
- Replacement Alogorithm 필요 (LRU, LFU 등)

## 3) Unifined Buffer Cache

- 최근의 OS에서는 기존의 Buffer Cache와 Page Cache에 통합됨

# 8. Page Cache와 Buffer Cache에 관리하는 방법

> Page Cache와 Buffer Cache에 관리하는 방법이 다르다.

가상 메모리에서의 **Page Cache**는 운영체제에게 전해지는 정보가 지극히 적었음.
이미 메모리가 존재하는 데이터 대해서는 HW에서 주소변환만 해서 정확한 접근 시간같은 정보를 알 수 없어서 **Clock 알고리즘을 사용**했다.

파일 시스템에서의 **Buffer Cache**는 메모리에 올라가 있던 아니던 간에 어차피 파일을 접근할 때는 시스템 콜을 해야하기 때문에 CPU 제어권이 운영체제에게 넘어가기 때문에 모든 정보를 알 수 있어서 **LRU, LFU를 사용**할 수 있다.







# 1. Page Cache와 Buffer Cache

![PageCacheBufferCache](https://jhi93.github.io/assets/img/os/PageCacheBufferCache.png)

# 1) Page Cache

Page Frame에다가 당장 필요한 내용을 메모리에 올려두고 필요하지 않은 내용들을 쫓아낸다. (= **Page Cache**)

=> 프로세스 주소 공간을 구성하는 Page가 Swap Area에 내려가 있는가?
Page Cache에 올라와 있는가? 를 처리하는 문제
( 단위는 Page로 4kbyte )

# 2) Buffer Cache

프로그램이 파일 입출력이 필요한 경우가 있는데, read System Call 같은걸 하게 되면 디스크에 있는 파일 시스템에서 운영체제가 대신 파일의 내용을 읽어와서 자신의 메모리 영역에 카피해두고 사용자 프로그램에게 전달해준다. (= **Buffer Cache**)

=> 다음에 동일한 파일에 대한 read/write에 대한 시스템 콜이 오면 디스크에서 읽어 오는게 아니라 buffer cache에 저장된 내용을 전달 해준다.

=> 파일 데이터가 파일 시스템 Storage에 저장되어 있느냐?
운영체제 Buffer Cache에 올라와 있는가? 를 처리하는 문제
( 단위는 Disk에서 어떤 파일의 블록을 읽어와라

=> 블록은 논리적인 블록 디스크에서는 섹터를 의미
예전에 섹터는 512byte이었으나 최근에는 Buffer Cache와 Page Cache와 통일 되어서 page 단위를 씀. = **Unified Buffer Cache** )

# 3) Memory Mapped I/O

프로세스의 주소공간의 일부를 파일에 매핑 해둔다.
맵핑 이후에는 메모리 접근하는 연산을 이용해서 파일 입출력을 할 수 있게 해준다.

# 2. File I/O

![PageCacheBufferCache2](https://jhi93.github.io/assets/img/os/PageCacheBufferCache2.png)

## 1) 기존의 파일 읽고 쓰는 방법

> 2가지 인터페이스가 존재.

**① 파일을 open한 후 read, write 시스템 콜을 한다.**

1. 시스템 콜이므로 운영체제가 해당하는 파일의 내용이 Buffer Cache에 있는 지 확인.
2. 있으면 그대로 전달해주고 없으면 디스크에서 읽어서 전달해준다.
   사용자 프로그램은 자신의 주소 영역중에 있는 Page에다가 Buffer Cache 내용을 카피해서 사용

**② Memory Mapped I/O**

1. 처음에는 운영체제에게 ‘Memory Mapped I/O 쓰겠다’라는 **mmap 시스템 콜을 호출**.
2. 자신의 주소공간 중 일부를 파일에다가 맵핑한다. ( = 디스크에서 Buffer Cache로 읽어오는 것은 동일하다.)
3. 읽어온 내용을 Page Cache에다가 카피한다. ( Page에 파일에 Mapped된 내용이 들어감. 맵핑 이후는 운영체제의 간섭없이 내 메모리 영역에다가 데이터를 읽거나 쓰는 방식으로 파일입출력이 가능하다. )
4. 만약 매핑만 해두고 메모리로 안읽어왔다면 **Page Fault**가 발생한다.

### * 1, 2번 방식의 차이점

진행 방식은 동일하나 1번 방식은 Buffer Cache에 있던 없던 운영체제에게 요청을 해서 받아와야하지만 2번의 경우는 일단 Page Cache에 있다면 운영체제에게 도움을 받지 않고 I/O가 가능하다.

### * Memory Mapped I/O를 사용하는 이유

이미 메모리에 올라온 내용에 대해서는 운영체제를 호출하지 않고 자신의 메모리에 접근하듯이 읽고 쓸 수 있다.
=>결론적으로 기존의 방식은 두가지 인터페이스 어떤 것을 쓰던지 파일 입출력을 할 경우는 Buffer Cache를 통해야한다.
따라서 Buffer Cache의 내용을 자신의 Page Cache에 복제해야하는 오버 헤드가 있다.

## 2) Unified Buffer Cache 사용한 파일 읽고 쓰는 방법

> 운영체제가 공간을 따로 나누어놓지 않고 필요에 따라서 Page Cache에 공간을 할당해서 쓴다.

**① 파일을 open한 후 read, write 시스템 콜을 한다.**
=> 해당하는 내용이 Disk File System에 있던 Buffer Cache(=Page Cache)에 있던 상관없이 운영체제에게 CPU 제어권이 넘어간다. 위와 동일

**② Memory Mapped I/O**
=> 처음에 운영체제에게 자신의 주소 영역중 일부를 파일에 매핑하는 단계를 거치고 나면 사용자 프로그램 주소 영역에 Page Cache가 매핑 된다. Buffer Cache가 별도로 존재하지 않고 Page Cache에다가 읽고 쓸 수 있다.

# 3. 프로그램의 실행

![ProgramExecution-1](https://jhi93.github.io/assets/img/os/ProgramExecution-1.png)

1. **프로그램**이 파일 시스템의 실행파일 형태로 저장되어 있다가 **실행시키면 프로세스**가 된다.
2. **프로세스**가 되면 그 프로세스만의 독자적인 주소공간인 **Virtual Memory라는 것이 만들어진다.**
3. **주소변환**을 해주는 하드웨어에 의해서 당장 필요한 부분은 **물리적 메모리에 올라가게 된다.**
4. **물리적 메모리**는 공간이 한정되어 있으므로 쫓겨나는 것들은 Disk의 **Swap Area**로 넘어간다.

cf)

1. **Memory Mapped I/O**를 쓰는 대표적인 방법이 실행파일에 해당하는 **Code** 부분이다.
   Code 영역 부분은 별도의 Swap Area 영역을 가지고 있지 않고 파일 시스템에 파일로 존재하는 내용이 그대로 프로세스 주소영역에 매핑이 되어있다.
   만약, 이프로그램이 특정 Code에 접근하는데 메모리에 안올라와 있다면 Swap Area에서 올리는 것이 아니라 파일에서 올려 써야한다.
   (***Code 영억 부분은 메모리에 올라간 다음 쫓겨날 때 Swap Area로 내려가지 않는다.\***
   ***read only라서 File System에 저장되어 있기 때문이다.\***)
2. 실행파일도 파일시스템에 저장되어 있지만 데이터 파일도 저장되어있다.
   프로그램이 실행되다가 자신의 메모리 접근만 하는 것이 아니라 파일의 내용을 읽어오라는 read 시스템 콜을 할 수 있고, Memory Mapped I/O를 쓸 수 도 있다.

## 1) Memory Mapped I/O 사용시

![ProgramExecution-2](https://jhi93.github.io/assets/img/os/ProgramExecution-2.png)

데이터 파일을 읽을때 read 시스템 콜이 아니라 Memory Mapped I/O를 쓰고 싶다면,
mmap 시스템 콜을 이용하여 ‘데이터 파일의 일부를 자신의 주소공간 일부에 매핑해주세요’ 라고 운영체제 요청 해야한다.

![ProgramExecution-3](https://jhi93.github.io/assets/img/os/ProgramExecution-3.png)

![ProgramExecution-4](https://jhi93.github.io/assets/img/os/ProgramExecution-4.png)

1. 운영체제가 데이터 파일의 일부를 Virtual Memory 주소공간 일부에다가 매핑을 해준다.
2. 프로그램이 실행되면서 이 메모리 위치를 접근했을 때 메모리에 안올라와있으면 Page Fault를 일으킨다.
3. 그러면 운영체제가 Page Fault가 일어난 Page를 물리적 메모리에 올려준다.
4. 그 이후는 가상 메모리 Page가 물리적 메모리의 Page와 Mapping이 되어 접근할때는 운영체제 도움을 받지 않아도 된다.
   ( 운영체제 도움없이 물리적 메모리에 읽거나 쓰거나 하게 됨 )
5. 메모리에 쫓겨날 때는 Swap Area에 쫓겨나는 게 아니라 File System에 수정된 내용을 써주고 메모리에서 쫓아낸다.

\* 단점 : 여러 프로세스가 같이 공유하게 되면 일관성을 주의 해야한다.



## 2) read/write 시스템 콜 이용시

![ProgramExecution-5](https://jhi93.github.io/assets/img/os/ProgramExecution-5.png)

![ProgramExecution-6](https://jhi93.github.io/assets/img/os/ProgramExecution-6.png)

1. read 시스템콜로 파일 요청
2. 운영체제는 자신의 Buffer Cache에 내용을 읽어온다.
3. Unified Buffer Cache는 요청한 데이터 파일의 내용이 이미 Buffer Cache에 올라와 있다면 그 내용을 카피해서 사용자 프로세스에게 전달한다.

















# Disk Management and scheduling

------



![PageCacheBufferCache](https://jhi93.github.io/assets/img/os/PageCacheBufferCache.png)

# 1. Disk Structure

- Logical Block
  - 디스크의 외부에서 보는 디스크의 단위 정보 저장 공간들
  - 주소를 가진 1차원 배열처럼 취급
  - 정보를 전송하는 최소 단위
- Sector
  - Logical Block이 물리적 디스크에 매핑된 위치
  - 디스크를 관리하는 최소의 단위
  - Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번째 섹터이다.

# 2. Disk Management

- Physical Formatting
  - 디스크를 컨트롤러가 읽고 쓸 수 있도록 **sector들로 나누는 과정**
  - 각 섹터는 header + 실제 data(보통 512byte) + trailer로 구성
  - header와 trailer는 sector number, ECC(Error-Correcting Code) 등의 정보가 저장되며 controller가 직접 접근 및 운영
    (ECC : 내용을 요약하여 해쉬 함수를 적용한 데이터 - 축약본으로 오류 검출용)
- Partitioninig
  - 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
  - OS는 이것을 **독립적 Disk로 취급**(Logical Disk)
- Logical Formatting
  - **파일 시스템을 만드는 것**
  - FAT, Inode, Free Space 등의 구조 포함
- Booting
  - ROM에 있는 “small bootstrap loader”의 실행
  - sector 0 (boot block)을 load하여 실행
  - sector 0 “full bootstrap loader program”
  - OS를 디스크에서 load하여 실행
  - **하드디스크에서 0번 sector를 읽고 실행 => 운영체제 커널 위치를 찾아서 올림**

# 4. 디스크 접근 시간

![DiskScheduling](https://jhi93.github.io/assets/img/os/DiskScheduling.png)

cf) Seek Time이 가장 많은 시간을 소요

# 5. Disk Scheduling Alogrithm

- **FCFS**
- **SSTF**
- **SCAN**
- **C-SCAN**
- **N-SCAN**
- **LOOK**
- **C-LOOK**

ex. 큐에 다음과 같은 실린더 위치의 요청이 존재하는 경우, 디스크 헤드 53번에서 시작한 각 알고리즘의 수행 결과는?
(실린더 위치는 0-199)

98, 183, 37, 122, 14, 124, 65, 67

## 1) FCFS (First Come First Service)

> 들어온 순서대로 처리한다.

![DiskSchedulingFCFS](https://jhi93.github.io/assets/img/os/DiskSchedulingFCFS.png)

## 2) SSTF (Shortest Seek Time First)

> 현재 헤드 위치에서 제일 가까운 요청을 처리한다.
> => 디스크 이동 시간이 적지만 Starvation이 일어날 가능성이 높다.

![DiskSchedulingSSTF](https://jhi93.github.io/assets/img/os/DiskSchedulingSSTF.png)

## 3) SCAN

> 상황에 좌우받지 않고 한 방향으로 끝까지 가면서 처리하고 끝까지 가면 방향을 바꿔서 반복
> => 가장 간단하면서도 획기적인 방법으로 엘레베이터와 비슷하다.

- Disk arm이 디스크의 한쪽 끝에서 다른쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리한다.
- 다른 한쪽 끝에 도달하면 역방향으로 이동하며 오는 길목에 있는 모든 요청을 처리하며 다시 반대쪽 끝으로 이동한다.
- 문제점 : 실린더 위치에 따라 대기 시간이 다르다.

![DiskSchedulingSCAN](https://jhi93.github.io/assets/img/os/DiskSchedulingSCAN.png)

## 4) C-SCAN (Circular Scan)

> 디스크 헤드가 한 쪽 방향만 처리 (되돌아갈때는 처리하지 x)
> => 이동거리는 늘어날 수 있으나 SCAN 보다는 대기 시간이 균일해진다.

- 헤드가 한쪽 끝에서 다른 쪽 끝으로 이동하며 가는 길목에 있는 모든 요청을 처리
- 다른쪽 끝에 도달했으면 요청을 처리하지않고 곧바로 출발점으로 다시 이동
- SCAN보다 균일한 대기 시간을 제공

![DiskSchedulingCSCAN](https://jhi93.github.io/assets/img/os/DiskSchedulingCSCAN.png)

![DiskSchedulingCSCAN2](https://jhi93.github.io/assets/img/os/DiskSchedulingCSCAN2.png)

## 5) N-SCAN

- SCAN의 변형 알고리즘
- 일단 arm이 한 방향으로 움직이기 시작하면 그 시점 이후에 도착한 job은 되돌아올 때 Service
- 출발하기 전에 들어온 것들은 가면서 처리 하지만 가는 도중 들어오는 애들은 처리X

## 6) LOOK and C-LOOK

- SCAN이나 C-SCAN은 헤드가 디스크 끝에서 끝으로 이동
- LOOK과 C-LOOK은 헤드가 진행 중이다가 그 방향에 더이상 기다리는 요청이 없으면 헤드의 이동방향을 즉시 반대로 이동한다.

![DiskSchedulingCLOOK](https://jhi93.github.io/assets/img/os/DiskSchedulingCLOOK.png)

# 6. Disk-Scheduling Algorithm 결정

> 현대 디스크 시스템에서는 SCAN 기반한 알고리즘을 주로 쓰고 있다.

- SCAN, S-SCAN 및 그 응용 알고리즘은 LOOK, C-LOOK등이 일반적으로 디스크 입출력이 많은 시스템에서 효율적인 것으로 알려져 있음.
- File의 할당 방법에 따라 디스크 요청이 영향을 받음
- 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는 것이 바람직하다.

# 7. Swap-Space Management

- Disk

  를 사용하는 두 가지 이유

  1. Memory의 Volatile한 특성 => **File System**
  2. 프로그램 실행을 위한 Memory 공간 부족 => **Swap Space**

- Swap Space

  - Virtual Memory System에서는 디스크를 Memory의 연장 공간으로 사용
  - 파일 시스템 내부에 둘 수 있으나 별도 Partition 사용이 일반적
    - 공간효율성보다는 속도 효율성이 우선
    - 일반 파일보다는 훨씬 짧은 시간만 존재하고 자주 참조됨
    - block의 크기 및 저장 방식이 일반 파일 시스템과 다름

![SwapSapceManagement](https://jhi93.github.io/assets/img/os/SwapSapceManagement.png)

\* 하드디스크에서 Swap Space를 어떻게 관리해야하는가?
물리적 디스크를 Partitioning 해서 Logical Disk를 만들 수 있고,
운영체제는 각각의 Logical Disk를 각각을 독립적인 디스크로 간주하게 된다.

\* Logical Disk는 File System으로 사용될 수 있고 Swap Area로도 사용될 수 있다.

- **File System**은 512Byte의 Sector 단위로 데이터를 저장하고 있고 데이터를 저장하는 방식은 연속할당, 링크 할당, 인덱스 할당 등이 존재했다.
- **Swap Area**는 프로그램이 실행되는 동안에 Swap Area에 머물러 있던 프로세스의 주소공간이 프로그램이 끝나면 사라지게 된다.
  => 그렇기 때문에 공간활용성보다는 시간활용성이 중요하다.
  (빨리 올리고 내리고가 중요 => Seek Time을 줄여야한다.)

# 8. RAID

> Redundant Array of Independent Disks
> 여러 개의 디스크를 묶어서 사용

- 사용 목적
  - 디스크 처리 속도 향상
    - 여러 디스크 block의 내용을 분산 저장
    - 병렬적으로 읽어옴 (Interleaving, Sriping)
  - 신뢰성 향상
    - 동일 정보를 여러 디스크에 중복 저장
    - 하나의 디스크가 고장시 다른 디스크에서 읽어옴 (Mirroring, Shadowing)
    - 단순한 중복 저장이 아니라 일부 디스크에 Parity를 저장하여 공간의 효율성을 높일 수 있다.

=> 중복 저장, 분산 저장등을 활용

cf)
여러 곳에서 조금씩 조금씩 읽어올 수 있어서 디스크 처리 속도 향상
=> 분산 처리 후 병렬적으로 읽어온다.

- Interleaving : 여기 저기서 조금씩 읽어옴
- Striping : 디스크를 조금씩 나눠서 저장