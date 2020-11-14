# 08_ Memory Management

---

> [toc]



### 1. Logical Address vs Physical Address

1. Logical Address (=Virtual Address)
   - 프로세스마다 독립적으로 가지는 주소 공간
   - 각 프로세스마다 0번지부터 시작
   - CPU가 보는 주소는 Logical Addrss이다.
2. Physical Address
   - 메모리에 실제 올라가는 위치

\* **주소 바인딩 : 주소를 결정하는 것**
**Symbolic Address -> Logical Address -> Physical Address**

\* **Symbolic Address** : 프로그래머는 숫자로 된 주소를 사용하지 않는다.
(변수 이름 이라던지 함수 이름이라던지..) => 컴파일 되면 숫자인 **Logical Address**
=> 실행되려면 물리적 메모리에 올라가야하므로 주소변환이 이루어져야한다.
(주소결정 = **Address Binding**)
=> **Physical Address**

\* 각 프로그램마다 가지고 있는 논리적 주소가 물리적 주소로 언제 결정되는가?
=> 총 3가지 시점으로 나눌 수 있다.



### 2. 주소 바인딩 (Address Binding)

1. Compile Time Binding

    

   : 컴파일 시

   - 물리적 메모리 주소(Physical Address)가 컴파일 시 알려짐
   - 시작 위치 변경시 재컴파일
   - 컴파일러는 절대코드(Absolute Code) 생성
     ( = 컴파일시 주소가 Fixed 된다.)

2. Load Time Binding

    

   : 실행이 시작될 시

   - Loader의 책임하에 물리적 메모리 주소 부여
   - 컴파일러가 재배치가능코드(Relocatable Code)를 생성한 경우 가능
     ( = 정해져 있는 것이 아니라 실행시 비어있는 곳 어디든 올라간다.)

3. Execution Time Binding (=Run Time Binding)

    

   : 실행도중

   - 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음.
   - CPU가 주소를 참조할 때마다 Binding을 점검 (=Address Mapping Table)
   - **하드웨어적인 지원이 필요 (ex. MMU)**

   

##### Example

![AddressBinding](https://jhi93.github.io/assets/img/os/AddressBinding.png)

**

**① Compile Time Binding** : 컴파일때 물리적 주소가 이미 결정되므로 ***Logical Address가 곧 Physical Address\***가 됨.
주소가 많이 비어있어도 그 자리에 꼭 올려야하므로 비효율적이다.
=> 현대 컴퓨터에서는 사용하지 않음.



**② Load Time Binding** : 논리적 메모리까지 결정된 상태에서 실행시키면 물리적 메모리 주소가 결정됨.
( ex. 물리적 메모리 살펴보니 500번지부터 비어있더라 -> 500번지부터 올리자)

*=> 1,2번은 프로그램 시작시 한 번 결정 후 바뀌지 않음.*



**③ Execution Time Binding(Run Time Binding)** : 프로그램 실행 도중 물리적 메모리 주소 이동이 가능.
( ex. 300번지에 있다가 메모리에서 쫓겨났다가 다시 올라왔는데 빈 곳이 700번지 -> 이번엔 700번지로 올리자)
-> 프로그램 실행 중에도 주소가 바뀌므로 CPU가 메모리 주소를 요청할 때 마다 Binding을 체크해야함.
=> ***하드웨어적 지원이 필요하다. (MMU : 그때그때 주소변환을 해줌.)\***



\* CPU가 바라보는 주소는?
=> **Physical Address가 아니라 Logical Address를 바라본다.**
코드 내에 들어 있는 주소까지 변환이 되지 않기 때문..
(Logical Address에서 Add 20 30 : 20번지 30번지 더하라!
=> Pysical Address 내에서도 20번지 30번지 더하라라고 되어있음.)
=> 따라서 CPU도 논리적 메모리 주소를 볼 수 밖에 없다.
=> CPU가 매번 메모리 몇 번지에 있는 내용을 달라 요청하면
그때 주소 변환해서 물리적 메모리 위치를 찾아서 읽어서 CPU한테 전달해줘야함.



### 3. Memory Management Unit (MMU)

> **Logical Address를 Physical Address로 매핑해주는 Hardware Device**

- * MMU Scheme

  사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소 값에 대해 Base Register (=Relocation Register)의 값을 더한다.

\* User Program

- Logical Address만을 다룬다.

- 실제 Physical Address를 볼 수 없으며 알 필요가 없다.

  

  ### Dynamic Relocation

![DynamicRelocation](https://jhi93.github.io/assets/img/os/DynamicRelocation.png)

=> 프로세스 P1 실행 중… ( 논리주소 0~3000번지 / 물리주소엔 14000번지에 올라가 있는 상황 )
CPU가 346번지 내용 달라 -> 논리 주소이므로 물리 주소로 변환이 필요하다.
주소 변환을 해주는 것은 **MMU : 가장 간단한 방법은 레지스터 2개로 주소변환 하는 것**
=> 물리적 주소에 올라가 있는 **시작 위치 + 요청한 논리 주소 값**



### ① Relocation Register (=Base Register) : 프로그램이 올라간 물리적 주소 시작 위치를 저장해둔다.

#### ② Limit Register : 프로그램의 주소 크기를 저장해둔다. ( 다른 메모리 주소를 요청하지 못하게 제한 )

![AddressTransaction](https://jhi93.github.io/assets/img/os/AddressTransaction.png) * 운영체제 및 사용자 프로세스 간의 메모리 보호를 위해 사용하는 레지스터

- Relocation Register : 접근할 수 있는 물리적 메모리 주소의 최소값
- Limit Register : 논리적 주소의 범위

=>논리 주소가 프로그램 크기보다 더 큰 메모리 주소를 요청했는 지 판단.
크다 : 트랩 발생! => 운영체제에 CPU 권한이 넘어가고 에러를 발생.
작다 : Base Register의 값을 더해서 메모리 주소를 구한 후 CPU에 전달해준다.



### 4. Some Terminologies

1. **Dynamic Loading**

2. **Dynamic Linking**

3. **Overlays**

4. **Swapping**

   

### 1) Dynamic Loading

> 프로그램을 메모리에 동적으로 올린다.
> => 그때그때 필요할때 마다 메모리에 올린다.
> \* Loading : 메모리에 올리는 것을 의미.



- 프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 메모리에 load
- Memory Utilization의 향상
- 가끔씩 사용되는 많은 양의 코드의 경우 유용
- 운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능
  ( 운영체제가 라이브러리를 제공해주며 그걸 이용하여 구현)



\* 현재 컴퓨터 시스템 - 필요한 부분만 메모리에 올라가고 필요 없는 부분은 다시 내림
=> Dynamic Loading이 아니라 OS가 관리해주는 Paging System에 해당.
(현대는 구분하지 않고 사용할 때도 있음)



### 2) Dynamic Linking

> 프로그램 작성 후 컴파일하고 링크에서 실행파일을 만듬.
> \* Linking : 여려 곳에 존재하는 컴파일된 파일들을 하나로 묶어서 실행파일로 만드는 것을 의미



\* Linking을 실행 시간까지 미루는 기법

1. Static Linking
   - 라이브러리가 프로그램의 실행 파일 코드에 포함됨
   - 실행 파일의 크기가 커짐
   - 동일한 라이브러리를 각각의 프로세스가 메모리에 오리므로 메모리 낭비
2. Dynamic Linking
   - 라이브러리가 실행시 연결됨
   - 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
   - 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고, 없으면 디스크에서 읽어옴
   - 운영체제의 도움이 필요



\* Dynamic Linking을 해주는 라이브러리를 Shared Library라고 부른다.
리눅스에서는 Shared Object,
**윈도우에서는 DLL (Dynamic Linking Library)**





## 3) Overlays 올

## 려놓는 것.

- 메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림

- 프로세스의 크기가 메모리보다 클 때 유용

- 운영체제의 지원없이 사용자에 의해 구현

- 작은 공간의 메모리를 사용하던 초창기 시스템에서 수작업으로 프로그래머가 구현

  - Manual Overlay
  - 프로그래밍이 매우 복잡

  

**Dynamic Loading과 차이?**
과거 메모리 크기가 작아서 프로그램 하나를 메모리에 올려놓는 것 마저도 불가능했음.
=> 프로그래머가 큰 프로그램을 쪼개서 올리고 내리고를 수작업으로 코딩했음 이를 Overlay라고 부름.
=> Overlays는 운영체제의 라이브러리 제공 x



### 4) Swapping

> 프로세스를 메모리에서 통째로 쫓아내는 것
> (하드디스크 = Backing Store = Swap Area)



\* **Swap In / Swap Out**

- 일반적으로 중기 스케줄러에 의해 Swap Out 시킬 프로세스 선정
- Priority-Based CPU Scheduling Algorithm
  - Priority가 낮은 프로세스를 Swapped Out 시킴
  - Priority가 높은 프로세스를 메모리에 올려놓음
- Compile Time 혹은 Load Time Binding에서는 원래 메모리 위치로 Swap In 해야함.
- Swap Time은 대부분 Transfer Time (Swap 되는 양에 비례)



\* **Swap Time?** 보통 디스크를 접근하는 시간은 **Seek Time(디스크 헤더가 이동하는 시간)**이 대부분을 차지하고 **Transfer Time(데이터 전송 시간)**은 미미하다.
그런데, 용량이 방대한 ***Swapping에서는 파일입출력과는 다르게 디스크 접근 시간 대부분이 Swap 되는 데이터 양에 비례하는 Transfer Time이 차지한다.\***

1. **Swap Out : Main Memory-> Swap Area**
2. **Swap In : Swap Area -> Main Memory**



### 5. 물리 메모리의 할당

> 메모리는 일반적으로 두 영역으로 나누어 사용한다. **OS 상주영역** : Interrupt Vector와 함께 낮은 주소 영역 사용 **사용자 프로세스 영역** : 높은 주소 영역 사용



\* 사용자 프로세스 영역의 할당 방법?

1. - **Contiguous Allocation( 연속 할당 )**

     각각의 프로세스가 메모리의 **연속적인 공간에 적재**도록 하는 것***Fixed Partition Allocatoin ( 고정 분할 )\******Variable Partition Allocation (가변 분할 )\***

2. - **NonContiguous Allocation( 불연속 할당 )**

     하나의 프로세스가 메모리의 **여러 영역에 분산**되어 올라가도록 하는 것**Paging****Segmentation****Paged Segmentation**

     

#### 1) 연속 할당

##### **① 고정 분할 방식** : 프로그램이 들어갈 사용자 영역을 미리 파티션으로 나누어 두는 것.

- **물리적 메모리를 몇 개의 영구적으 분할로 나**눔

- 분할의 크기가 모두 동일한 방시과 서로 다른 방식이 존재

- 분할당 하나의 프로그램 적재

- 융통성이 없음

  - 동시에 메모리에 load되는 프로그램의 수가 고정됨.
  - 최대 수행 가능 프로그램 크기 제한

- 내부 단편화 발생 (외부 단편화도 발생)

  

  ##### **② 가변 분할 방식** : 미리 나눠두지 않는 것.

- **프로그램의 크기를 고려해서 할당**

- 분할의 크기, 개수가 동적으로 변함

- 기술적 관리 기법 필요

- **외부 단편화 발생**

![ContiguousAllocation](https://jhi93.github.io/assets/img/os/ContiguousAllocation.png)

\* **외부 조각** : 올릴려는 프로그램보다 메모리 조각이 작을 경우
(프로그램이 들어갈 수 있음에도 작아서 사용하지 못함)
\* **내부 조각** : 프로그램 크기가 공간보다 작아서 남는 공간이 있을 경우
(할당되었지만 사용되지 않는 영역)



문제점) 미리 나눠둠으로써 이런 문제가 생긴 것. 나눌 필요가 있는가?
=> 프로그램이 실행될때마다 차곡차곡 메모리에 올리는 방식 ( 가변 분할 방식 )
=> 가변 분할 방식으로 하더라도 프로그램 크기가 균일하지 않기 때문에 외부조각은 생길 수 있다.



**Hole**

- 가용 메모리 공간
- 다양한 크기의 Hole들이 메모리 여러 곳에 흩어져 있음
- 프로세스가 도착하면 수용가능한 Hole을 할당
- 운영체제는 다음의 정보를 유지
  1. 할당 공간
  2. 가용 공간

![Hole](https://jhi93.github.io/assets/img/os/Hole.png)

=> 가변분할 방식을 쓰다 보면 프로그램이 종료 될때마다 작은 Hole들이 산발적으로 생기게 된다.

가용공간 어디에 프로그램을 할당할 것인가?
=> 가변 분할 방식에서 가용공간에서 할당하는 방식
=> 3가지 방식이 있다.



1. First-Fit

   - Size가 n 이상인 것 중 **최초로 찾아지는 Hole에 할당**

2. Best-Fit

   - Size가 n 이상인 가장 작은 Hole을 찾아서 할당( **가장 적합한 Hole에 할당** )
   - Hole들의 리스트가 크기순으로 정렬되지 않은 경우 모든 Hole의 리스트를 탐색해야함
   - 많은 수의 아주 작은 Hole들이 생성됨

3. Worst-Fit

   - **가장 큰 Hole에 할당**
   - 역시 모든 리스트를 탐색해야함
   - 상대적으로 아주 큰 Hole들이 생성됨

   

\* **Compaction**

- 외부 단편화 문제를 해결하는 한 가지 방법
- 사용 중인 메모리 영역을 한군데로 몰고 Hole들을 다른 한 곳으로 몰아 큰 Block을 만드는 것
- 매우 비용이 많이 드는 방법
- 최소한의 메모리 이동으로 Compaction하는 방법
- Compaction은 프로세스의 주소가 실행 시간에 동적으로 재배치가 가능한 경우에만 수행

=> 사용중인 공간을 한 곳으로 미뤄두고 Hole들을 모아서 하나로 묶는 것.
( ex. 디스크 조각 모음)
전체 프로그램의 binding에 관련된 문제이므로 비용이 많이 드는 방식이다.
Runtime Binding이 지원되야지만 사용할 수 있다.







-----------







#### 2) 불연속 할당

1. Paging

    

   : 기법은 프로그램을 구성하는 주소공간을

    

   같은 크기의 Page로 자른다.

   ( 4kb)

   - Page 단위로 물리 메모리에 올리거나 내린다
     **PageFrame = 페이지가 들어갈 수 있는 공간**
   - Hole들의 크기가 균일하지 않은 문제
     Compaction 하는 것 등을 해결 가능
   - 단점 : 주소 변환이 복잡해진다.
     잘려진 각각의 페이지가 물리메모리 어디에 올라갔는지 확인해야함.
     페이지별로 주소 계산을 해야한다.

2. Segmentation

    

   :

    

   Page 단위가 아닌 의미있는 단위로 자르는 것.

   - Code,Data,Stack으로 의미 있는 공간으로 잘라서 각각의 segment를 필요시에 물리 메모리 다른 위치에 올리는 것임.
     ( 함수 단위로도 가능 )
   - 단점 : 의미 단위로 잘랐기 때문에 크기가 균일하지 않다.
     Hole 문제 발생할 수 있다.

3. **Paged Segmentation**



##### **① Paging 기법**

> 프로그램을 구성하는 Address Space을 같은 크기의 Page 단위로 쪼갠 것.

- Process의 Virtual Memory를 동일한 사이즈의 Page 단위로 나눔
- Virtual Memory의 내용이 Page 단위로 불연속하게 저장됨
- 일부는 Backing Storage에, 일부는 Physical Memory에 저장



\* Basic Method

- Physical Memory를 동일한 크기의 Frame으로 나눔
- Logical Memory를 동일 크기의 Page로 나눔(Frame과 같은 크기)
- 모든 가용 Frame들을 관리
- Page Table을 사용하여 Logical Address를 Physical Address로 변환
- 외부 단편화 발생 안함
- 내부 단편화 발생 가능

![Paging](https://jhi93.github.io/assets/img/os/Paging.png)

=> 프로그램을 구성하는 논리적 메모리를 동일한 크기의 Page로 잘라서
각각의 Page별로 물리적 메모리 어디든 비어있는 위치에 올라갈 수 있게 해준다. ( **Page Frame** ) => 주소변환을 위해서는 **Page Table**이라는 것이 필요하다.
=> ***각각의 논리적 메모리에 있는 Page들이 물리적 메모리 어디에 올라가 있는가를 알려줌\***

** Page Table은 Page 갯수만큼 엔트리가 존재한다.**



##### ①-1. Address Translation Architecture

![AddressTranslation](https://jhi93.github.io/assets/img/os/AddressTranslation.png)

CPU가 논리적 주소를 주게 되면 물리적 메모리상에 주소로 바꿔야하는데 **Paging 기법에서는 Page Table**을 사용한다.

위 그림에서 (p, d)는
( 페이지 번호, 페이지 내에서 얼마나 떨어져 있는 지 나타내는 offset )을 의미한다.

논리적인 페이지 번호에 해당하는 엔트리를 페이지 테이블에서 p번째를 찾는다.
=> f 라는 Page Frame 번호가 나옴.
=> 물리적 메모리에서 f 번째를 찾는다. => 주소변환 끝!
=> d는 변환되지 않고 그대로인 이유는 내부에서 상대적 위치는 논리나 물리나 같기 때문



##### ①-2. Implementation of Page Table

- Page Table은 Main Memory에 상주
- **Page-Table Base Register(PTBR)가 Page Table을 가리킴**
- **Page-Table Length Register(PTLR)가 테이블 크기를 보괸**
- 모든 메모리 접근 연산에는 2번의 Memory Access 필요
- Page Table 접근 1번, Data/Instruction 접근 1번
- 속도 향상을 위해 **Associative Register 혹은 Translation Look-aside Buffer(TLB) 라 불리는 고속의 Lookup Hardware Cache 사용**

\* 동적할당에서 MMU에 Base Register, Limit Register가 존재했음
=> Paging 기법에서는 Page-Table Base Register, Page-Table Length Register라는 용도로 사옹됨.

**Page Table은 프로그램마다 별도로 존재**해야하고 용량이 크기 때문에 레지스터, 캐시메모리에 넣기 어려움 => 메모리에 집어 넣는다.
=> 주소변환을 위해 1번 접근
=> 변환된 주소로 접근 1번
=> 총 2번의 접근이 필요하다.

2번의 접근은 시간 소모가 많이 되므로 속도 향상을 위해 **일종의 캐시인 TLB를 사용한다.**
=> Main memory 와 CPU 사이에 존재.



##### ①-3. Paging Hardware with TLB

![TLB](https://jhi93.github.io/assets/img/os/TLB.png)

2번의 메모리 접근의 속도 개선을 위해 TLB라는 캐시 메모리를 둔다.
Main-Memory에서 빈번히 사용되는 데이터를 캐시 메모리에 저장해서 CPU로부터 빠르게 접근하게 해줌.
주소변환을 위한 캐시메모리임.
***TLB는 페이지테이블에서 빈번히 참조되는 일부 엔트리를 캐싱하고 있다.\***



TLB는 전체를 검색해야하므로 시간을 단축하기 위해 **Parallel Search가 필요하다.**
Page Table은 p번호 = p인덱스이므로 주소변환이 바로 이루어지므로 Parallel Search가 필요하지 않다.
Page Table이 프로세스마다 별도로 존재해야하므로 TLB도 마찬가지다.



##### ①-4. Effective Access Time

￡ : TLB를 접근하는 시간 ( 1보다 작다)
1 : Main memory 접근시간
∂ : TLB로부터 주소변환이 되는 비율 (= Hit ratio)

=> 실제로 메모리에 접근하는 시간 :
(1+￡)∂ + (2+￡)(1-∂)



##### ①-5. 2단계 페이지 테이블

- 현대의 컴퓨터는 Address Space가 매우 큰 프로그램 지원
- 32 bit Address 사용시 : 2^32(4G)의 주소 공간
  - Page Size가 4K시 1M개의 Page Table Entry가 필요
  - 각 Page Entry 4B시 프로세스당 4M의 Page Table 필요
  - 그러나, 대부분의 프로그램은 4G의 주소 공간 중 지극히 일부분만 사용하므로 Page Table 공간이 심하게 낭비됨

=> Page Table 자체를 Page로 구성
=> 사용되지 않는 주소 공간에 대한 Outer Page Table의 엔트리 값은 NULL



정리)
논리적인 메모리의 크기가 Maximum 어디까지 가능한가?
=> 메모리를 크기를 표시하는 주소체계를 몇 비트쓰느냐에 달림.
=> 주소는 바이트 단위로 구성된다.
=> 32비트로 구성되면 0 ~ 2^32 -1 까지 가능 (4기가)
=> 4기가를 Page 단위로 쪼갠다 (Page 크기는 4kb) => 총 1M개의 Page 개수가 얻어진다.
=> Page Table 엔트리 1M(100만개)가 필요. (엔트리 하나는 4byte)
=> 프로그램마다 Page Table을 위해 4M을 사용해야한다.



cf) 기존의 PageTable의 문제점 : 메모리 주소공간에서 실제 프로그램이 사용하는 공간은 지극히 일부분임
Code, Data, Stack있고 중간에는 사용되지 않는 부분이 상당부분 존재
페이지 테이블의 엔트리는 중간에 구멍있다고 빼고 만들 수 없다.
따라서 주소체계에서 상당히 일부분만 사용됨에도 페이지 테이블 엔트리는 다 만들어져야함)



=> 이를 해결하기 위해 2단계 페이지 테이블 방식을 사용.
(시간 더 걸리지만 공간을 줄일 수 있다. => 공간도 더쓰는 것 같은가?
=> cf에 나와있듯이 사용하지 않는 공간이 많음.
=> 이를 2단계에서는 바깥쪽 테이블 엔트리 NULL로 설정(포인터로)
=> 안쪽 테이블이 만들어지지 않음)

![TwoLevelPageTable](https://jhi93.github.io/assets/img/os/TwoLevelPageTable.png)
=> 안쪽 페이지 테이블의 하나의 크기는 페이지의 크기와 똑같다 = 4kb
엔트리 크기는 4byte => 1k개 집어 넣을 수 있다.



![TwoLevelPageTable2](https://jhi93.github.io/assets/img/os/TwoLevelPageTable2.png) (4kb = 2^12바이트)
page offset = 페이지 크기 4kb를 byte단위로 구분하기 위해서는 12비트가 필요하다.
(1k = 2^10)
안쪽 페이지 테이블의 엔트리 = 1k의 엔트리를 구분하기 위해서는 10비트가 필요하다.
32 - 22이므로 밖을 위한 페이지 테이블 = 10비트가 필요하다.



![Address-Translation-Scheme](https://jhi93.github.io/assets/img/os/Address-Translation-Scheme.png)
바깥 쪽 페이지 테이블의 인덱스 => 안쪽 페이지 테이블의 어떤 것인지 결정
안쪽 페이지 테이블의 인덱스 => 물리적 페이지 프레임 번호











------------









##### ①-6. Multilevel Paging

- Address Space가 더 커지면 다단계 페이지 테이블 필요

- 각 단계의 페이지 테이블이 메모리에 존재하므로 Logical Address의 Physical Address 변환에 더 많은 메모리 접근 필요

- TLB를 통해 메모리 접근 시간을 줄일 수 있음

- 4단계 페이지 테이블을 사용하는 경우

  - 메모리 접근 시간이 100ns, TLB 접근시간이 20ns이고 TLB hit ratio가 98%인 경우
    Effective Memory Access Time = 0.98 x 120 + 0.02 x 520 => 128 nanoSecond => 결과적으로 주소변환을 위해 28ns만 소요

  

=> TLB 덕분에 다단계 Page Table로 구성되어 있어도 빠르게 접근이 가능하다.

![BitInPageTable](https://jhi93.github.io/assets/img/os/BitInPageTable.png)

=> Page Table에 주소변환 정보만 들어 있는 것이 아니라 엔트리마다 부가정보가 들어가 있음.
(사용되지 않는 주소영역을 위해서도 엔트리가 만들어져야함
ex. 위 그림에서 6번 7번에 해당
테이블이라는 자료구조 특성상 위에서 부터 인덱스로 접근해야하므로..)



##### * Memory Protection을 위한 부가 정보

> Page Table의 각 Entry 마다 아래의 bit를 둔다.



- Protection bit

  - Page에 대한 접근 권한(read / write / read-only)
  - 어떤 연산에 대한 접근 권한이 있는가를 나타냄.
    ( 다른 프로세스가 이 페이지 접근하는 지에 대한 판단 x)
  - 코드영역 (read-only) : CPU에서 읽어서 Instruction을 실행하는 용도기 때문
  - 데이터 영역/스택 영역 (read/ wirte)

  

- Valid-Invalid bit

  - Valid는 해당 주소의 frame에 그 프로세스를 구성하는 유요한 내용이 있음을 뜻함 ( 접근 허용 )

  - Invalid는 해당 주소의 frame에 유효한 내용이 없음을 뜻함 (접근 불허 )

    - 프로세스가 그 주소 부분을 사용하지 않는 경우
    - 해당 페이지가 메모리에 올라와 있지 않고 Swap Area에 있는 경우

    

##### ①-7. Inverted Page Table

- Page Table이 매우 큰 이유

  - 모든 Process 별로 그 Logical Address에 대응하는 모든 Page에 대해 Page Table Entry가 존재
  - 대응하는 Page가 메모리에 있든 아니든 간에 Page Table에는 Entry로 존재

- Inverted Page Table

  - Page Frame 하나당 Page Table에 하나의 Entry를 둔 것 (System-Wide)
  - 각 Page Table Entry는 각각의 물리적 메모리의 Page Frame이 담고 있는 내용 표시 (Process-id, Process의 Logical Address)
  - 단점 : 테이블 전체를 탐색해야함
  - 조치 : Associative Register 사용 (비쌈)

  

![InvertedPageTable](https://jhi93.github.io/assets/img/os/InvertedPageTable.png)

***Inverted Page Table은 시스템 내에 페이지 테이블이 딱 하나 존재한다.\***
페이지 테이블의 엔트리가 프로세스의 페이지 수 만큼 존재하는 것이 아니라 물리적 메모리의 페이지 프레임 개수만큼 존재하는 것.
(첫 번째 엔트리는 첫 번째 페이지 프레임에 들어가는 논리적인 페이지 주소가 들어 있다)
논리주소에 해당하는 p가 물리적 메모리 어디에 올라가 있는 보려면 엔트리를 다 찾아봐야한다.



\* 사용하는 이유? PageTable을 위한 공간을 줄이기 위해!
단, 시간적 오버헤드가 존재 한다.
=> Associative register라는 HW를 통해 병렬적으로 검색할 수 있게 해줌

페이지 테이블 구성
(pid, p) => pid는 프로세스 아이디, p는 논리적 주소
논리적 주소 p뿐만 아니라 어떤 프로세스의 p인지 알아야하므로 프로세스 아이디(pid)도 같이 저장해야한다.



##### ①-8. Shared Page

> Shared 가능한 코드는 물리적 메모리에 하나만 올리는 방식



- Re-entrant Code (=Pure code)

  1. read-only로 하여 프로세스 간에 하나의 code만 메모리에 올림
  2. Shared Code는 모든 프로세스의 Logical Address Space에서 동일한 위치에 있어야 함

  

- Private Code and Data

  - 각 프로세스들은 독자적으로 메모리에 올림
  - Private Data는 Logical Address Space의 아무 곳에 와도 무방

  

![SharedPagesExample](https://jhi93.github.io/assets/img/os/SharedPagesExample.png)
ex. 아래 한글을 프로세스 3개가 돌린다면 내용(data)은 다르지만 코드자체는 같다.















----------













##### **② Segmentaion 기법**

> 프로그램을 구성하는 Address Space을 의미 단위로 쪼갠 것
> ex) Code, Data, Stack ( 더 작게 쪼갠다면 Code 중에서 함수별로..)

- Logical Address는 <segment-number, offset>으로 구성
- **Segment Table Base Register(STBR) : Segment Table의 위치**
- **Segment Table Length Register(STLR) : 프로그램이 사용하는 Segment 수**



##### **Segmentation 기법에 의한 주소 변환**

![SegmentationHW](https://jhi93.github.io/assets/img/os/SegmentationHW.png)

CPU가 논리 주소를 주게 되면 두 부분으로 나눈다.
( s,d ) = ( 세그먼트 번호, offset )



Paging 기법과는 다르게 **엔트리에 2가지 정보를 가지고 있다.(= SegmentTable)**
=> ( 세그 먼트의 길이, 물리적 메모리의 시작 위치 )
페이징 기법에서는 페이지 크기가 다 동일했지만
***세그먼테이션 기법에서는 의미 단위로 자르기 때문에 세그먼트의 길이가 균일하지 않기 때문에 길이 정보 필요.\***

**세그먼트 테이블의 시작위치 = STBR**
**세그먼트 길이 = STLR**

잘못된 세그먼트 번호가 들어왔을때 STLR와 비교한다.
( 5번 요청했는데 구성은 3개가 최대라면 체크 )



#### Segmentation Architecture

- Protection

  - 각 세그먼트 별로 Protection bit가 있음.
  - Each Entry
    - Valid bit = 0 => illegal segemnt
    - Read / Write / Executrion 권한 bit

- Sharing

  - Shared Segment
  - Same Segemnt Number

- Allocation

  - First Fit / Best Fit
  - 외부 단편화 발생

  

장점 : 세그먼트는 의미 단위이기 때문에 공유와 보안에 있어 Paging보다 훨씬 효과적이다.
단점 : 할당에 있어서 외부 단편화

cf) 테이블을 위한 메모리 낭비가 심한 것은 세그먼테이션 기법보다는 페이징 기법이 심함.



##### **③ Segmentaion 기법**

> Paging 기법과 Segmetation 기법을 혼합.
> 세그먼트를 여러개의 페이지로 구성하는 기법



![PagedSegmentation](https://jhi93.github.io/assets/img/os/PagedSegmentation.png)



#### 주소변환 과정

- 논리 주소 => ( s, d ) = (세그먼트 주소, 세그먼트 offset)
- 세그먼트 테이블 => s번째 엔트리를 가면 세그먼트에 대한 주소 변환 정보가 있음.

기존 세그멘테이션 기법은 세그먼트가 통째로 메모리에 올라가기 때문에 시작 위치만 알려줬지만 현재는 세그먼트가 페이지 단위로 쪼개져서 메모리에 올라간다.
=> Allocation 문제 해결 (Hole문제)
=> 의미나 보안 같은 경우는 세그먼트 테이블 레벨에서 시행함
=> 2가지 장점을 획득!

- 세그먼트당 페이지 테이블이 존재 => 페이지 테이블의 시작위치
  offset d를 잘라서 (p, d) = ( 페이지번호, 페이지 offset )을 사용
- 페이지 테이블 시작위치 + 페이지 번호 떨어진 엔트리 => 프레임 번호
- 프레임번호, 오프셋 => 물리적 메모리 주소

