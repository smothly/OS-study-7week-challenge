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

<br><br><br>

- Q. Page Cache와 Buffer Cache를 합친 Unified Buffer Cache가 있다. 왜 두 개의 캐시를 합쳤는가?
<br><br><br>
