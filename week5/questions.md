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

----------

Q.  대부분의 시스템은 프로그램이 자신의 주소 공간에 메모리를 할당하는 것을 지원하고 있다. 프로그램의 힙 세그먼트에 할당된 데이터는 이러한 메모리 할당이 수행되는 대표적인 예라고 할 수 있다. 다음 기법들에서 동적 메모리 할당을 지원하기 위해서 어떤 것들이 요구되는가?

- 연속적 메모리 할당, 순수 세그먼테이션, 순수 페이징



A.  a. 연속적 메모리 할당

  \> 현재 할당된 메모리의 공간을 키울 때 여유 공간이 넉넉하지 않으면 프로그램 전체의   재배치를 요구 할 수도 있다.



 b. 순수 세그먼테이션

  \> 세그먼트의 할당된 공간이 커질 때 여유 공간이 부족하면 여유 공간이 부족한 세그먼트만 재배치를 요구 할 수도 있다.



 c. 순수 페이징

  \> 증가하는 새로운 페이지들의 할당은 프로그램 주소공간의 재배치 없이 가능하다.



Q. Android는 부트 디스크에서는 스와핑을 지원하지 않더라도, 별도의 SD 비휘발성 메모리 카드를 사용하여 스왑 공간을 설정하는 것이 가능하다. Android가 부트 디스크에서는 스와핑을 불허하지만 보조 디스크에서는 허용하는 이유는 무엇인가?



A. 일반적으로 모바일 장치는 하드디스크보다 flash memory를 사용한다. 여기서 스와핑을 지원하지 않는 가장 큰 이유는 메모리 저장 장소의 공간이 작기 때문이고, 또 다른 이유는 flash memory가 사용의 허용되는 쓰기 횟수가 정해져 있고 flash memory와 CPU사이에 처리량이 저조하기 때문이다.

