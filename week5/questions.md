## 5주차 문제

---

- Q. 조건이 다음과 같을 때 Effective Access Time을 계산하시오.

```
3page level paging system일 때
Memory Access Time = 110ns
TLB Access Time = 10ns
TLB hit ratio = 0.95
```



- Q. Inverted page table을 사용하는 이유와 동작 방식, 장단점에 대해 서술하시오

[위 두 문제에 대한 Answer link](https://github.com/gashe-soo/OS-7week-KOCW/blob/main/week_5/Problem.md)

---

- Q. 32-bit logical address를 가진 시스템에서, 각 page의 크기가 4Kb라면 각 프로세스당 page table의 크기는 몇 바이트인가? (single-layer)

- Q. 64-bit logical address를 가진 시스템에서, 각 page의 크기가 4Kb이고 page table의 크기가 page와 같아야한다면 총 몇겹의 page table이 필요한가?

- Q. Paging, Segmentation, Paged Segmentation에서 각각 physical memory의 구현 방법을 어떻게 되어있는가? Segmentation에서 각 segment의 physical memory는 무엇에 비례하는가?

---

- Q. 옳지 않은 설명을 모두 고르시오. 옳지 않은 이유도 설명해주시오.
    - a. paging 기법에서 TLB를 사용하면 2번의 메모리 접근이 필요하다.
    - b. segement-table length register는 프로그램이 사용하는 segment의 수를 뜻한다.
    - c. paged segmentation에서 의미적인 부분은 segment를 진행하고, 물리적 메모리에 올라갈 때는 page 단위로 올라간다.
    - d. paging기법에서 page table은 각 logical memory에 존재한다.

- Q. 주소바인딩의 3가지 시점에 대해 설명해주세요.