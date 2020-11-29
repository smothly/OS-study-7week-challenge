## 7주차 문제

---

- Q. 파일 데이터를 디스크에 할당하는 방법에 대한 설명 중 옳지 않은 설명을 모두 고르시오. 옳지 않은 이유도 설명해주시오.
    - a. Contiguous allocation은 컨트롤러가 접근할 때 순차접근을 해야한다.
    - b. Contiguous allocation에서 file grow제약이 있어 미리 빈 공간을 할당할 경우 내부조각이 발생한다.
    - c. Linked Allocation에서 단점으로는 직접접근 불가, 외부 조각 발생, pointer를 위한 공간낭비가 있다.
    - d. Indexed Allocation에서는 작은 파일이 많을경우 유리한 할당 방법이다.

> A.  a, c, d
> - a - 직접 접근이 가능하다.
> - c - 외부 조각은 발생하지 않는다.
> - d - 작은 파일의 경우 적어도 인덱스 파일 포함하여 2개의 파일이 생성되므로 좋지 않다.


<br><br>

- Q. Disk Scheduling에서 아래 4가지 항목에 대해서 각각 설명해주세요.<br>
1. SCAN 알고리즘과 C-SCAN알고리즘의 차이점
2. SCAN 알고리즘과 LOOK 알고리즘이 차이점
3. SCAN 알고리즘과 N-SCAN알고리즘의 차이점
4. SSTF 알고리즘 단점


> A. <br>
> 1. scan은 돌아오면서도 요청에 대해서 검사하고 c-scan은 처음자리로 바로돌아가서 단방향으로만 수행한다.
> 2. Look 알고리즘은 끝에 요청이 없으면 바로 방향을 바꾸고, scan은 무조건 끝까지 확인을 하고 방향을 바꾼다.
> 3. n-scan은 단 방향으로 움직일 동안 추가된 요청에 대해서 돌아올 때 처리한다.
> 4. starvation 문제

<br>
<br>
- Q. 다음 질문에 답하시오

  ```
  1) FAT는 (A) 할당 방법을 사용했다. A에 들어갈 말과 A의 장단점을 서술하시오.
  2) 1번의 A의 단점을 FAT는 어떻게 해결했는가?
  ```
  <br>
> 1) FAT는 linked allocation을 사용했다. linked allocation은 외부 단편화가 발생하지 않는다. 
 다만, Direct access가 불가하며, 포인터의 유실로 인한 reliability의 문제, 낮은 공간 효율성의 문제가 있다.<br>
 2) FAT는 pointer를 별도의 위치에 보관하여 reliability와 공간효율성 문제를 해결했다.

<br><br>




- Q. Page Cache와 Buffer Cache를 합친 Unified Buffer Cache가 있다. 왜 두 개의 캐시를 합쳤는가? 
> A. I/O mapped I/O일 때는 buffer cache만을 이용하지만, memory mapped I/O일 경우 buffer cache, page cache에 동일한 내용을 적재하는 이중 캐시의 문제로 공간 효율성이 하락한다.

<br><br><br>



* Q. 디스크에 파일을 저장하는 방식 중 링크연결 할당 방식의 장단점 
* <br>
* <br>
* A.
* 장점 :
  ① 외부 단편화 문제 발생하지 않는다.
* 단점 :
  ① 직접 접근이 안된다.
  ( ex. 중간 위치를 보기 위해서는 앞에서 부터 순차 접근해야함)
  ② 디스크의 섹터들이 간혹 Dead Sector가 되는데 이러면 뒷 부분을 접근할 수 없게 된다.
  ③ 한 섹터당 512 바이트 배수로 저장하는데 4 바이트를 포인터를 사용해서 두 섹터를 사용할 수도 있게되므로 공간 낭비를 할 수 있다.
* Q. 유닉스파일시스템구조에서 inode list를 간단히 설명하시오
* <br>
* <br>
* A. File meta data는 그 파일을 가지고 있는 디렉토리에 가면 기록되어있다고 했는데 실제 파일시스템에서는 디렉토리가 다 가지고 있지 않고 별도의 위치에 저장
  (디렉토리에 파일 이름과 Inode번호가 저장)

* Q. Free-space management (비어있는블록 관리) 에서 grouping 방식에 대해 간단히 설명하시오
* <br>
* <br>
* A. 첫 번째 비어있는 위치가 인덱스 역할->비어있는 곳들을 가르킴
  - Linked list방법의 변형
  - 첫 번째 free block이 n개의 pointer를 가짐
    - n-1 pointer는 free data block을 가리킴
    - 마지막 pointer가 가리키는 block은 또 다시 n pointer를 가짐
