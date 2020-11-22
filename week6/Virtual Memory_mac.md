# Virtual Memory

------

> [toc]



# 1. Demand Paging

> 요청이 있으면 Page를 메모리에 올리겠다.

- 장점
  - I/O 양의 감소
  - Memory 사용량 감소
  - 빠른 응답 시간
  - 더 많은 사용자 수용
- Valid / Invalid Bit의 사용
  - Invalid의 의미
    - 사용되지 않는 주소 영역인 경우
    - 페이지가 물리적 메모리에 없는 경우
  - 처음에는 모든 Page Entry가 Invalid로 초기화
  - Address Traslation 시에 Invalid Bit이 Set되어 있으면 => **Page Fault**

![PageTableExample](https://jhi93.github.io/assets/img/os/PageTableExample.png)

# 2. Page Fault

> 주소변환을 하려고 했으나 Invaild일 경우 Page Fault를 발생
> 요청하는 Page가 메모리에 없을 때 => CPU는 자동적으로 운영체제에 넘어가고(Trap : 소프트웨어 인터럽트) Fault 일어난 Page를 메모리에 올린다.

- Invalid Page를 접근하면 MMU가 Trap을 발생시킴 (Page Fault Trap)
- Kernel Mode로 들어가서 Page Fault Handler가 invoke됨
- Page Fault 처리 순서
  1. 잘못된 요청인지 판별
  2. 정상 요청일 경우 메모리에 올려야하는데 메모리가 꽉 차있다면 하나를 내린다.
  3. 해당 Page를 Disk에서 Memory로 읽어온다.
     ( I/O 작업이므로 느림. 따라서 CPU를 I/O 작업 끝날때 까지 넘긴다.)
  4. Disk Read 끝나면 Vaild로 바꾼다.
  5. CPU 다시 넘겨 받고 다시 명령어들 수행한다.

![StepPageFault](https://jhi93.github.io/assets/img/os/StepPageFault.png)

# 3. Page Replacement

- 어떤 frame을 빼앗아올지 결정해야함.
- 곧바로 사용되지 않읗 Page를 쫓아낸느 것이 좋음.
- 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있음.
- Replacement Algorithm
  - Page-Fault Rate을 최소화하는 것이 목표
  - 주어진 Page Reference String에 대해 Page Fault를 얼마내 내는지로 알고리즘을 평가

## 1) Optimal Algorithm

> 가장 먼 미래에 참조되는 Page를 Replace

![OptimalAlgorithm](https://jhi93.github.io/assets/img/os/OptimalAlgorithm.png)

- 미래의 참조를 어떻게 아는가?
  - Offline Algorithm
- 다른 알고리즘의 성능에 대한 Upper Bound 제공
  - Belady’s Optimal Algorithm, MIN, OPT 등으로 불림

=> 가장 좋은 알고리즘으로 PageFault를 가장 적게 하는 알고리즘이다.
하지만 실제 시스템에서는 미래에 언제 참조할지를 모르기 때문에 실제 시스템에서는 사용되지 않음.

## 2) FIFO( First-In First-Out ) Algorithm

> 먼저 들어온 것을 먼저 내쫓음.

![FIFOAlgorithm](https://jhi93.github.io/assets/img/os/FIFOAlgorithm.png)

특징 : Page Frame 메모리 크기를 늘려주면 오히려 Page Fault가 늘어나는 현상이 발생 ( = FIFO Anomaly )

## 3) LRU (Least Recently Used) Algorithm

> 가장 덜 최근에 사용된 Page를 내린다.
> ( 가장 오래전에 사용된 것을 쫓아냄 )

![LRUAlgorithm](https://jhi93.github.io/assets/img/os/LRUAlgorithm.png)
=> 최근에 사용된 Page가 미래에도 다시 참조될 가능성이 높다는 것을 아이디어로 사용.

## 4) LFU (Least Frequently Used) Algorithm

> 가장 덜 비번하게 사용된 Page를 쫓아냄
> (참조 횟수가 제일 적은 page를 쫓아낸다)

- 최저 참조 횟수 Page가 여럿 있는 경우
  - LFU 알고리즘 자체에서는 여러 Page 중 임의로 선정한다.
  - 성능 향상을 위해 가장 오래 전에 참조된 Page를 지우게 구현할 수도 있다.
- 장/단점
  - LRU 처럼 직전 참조 시점만 보는 것이 아니라 장기적으로 시간 규모를 보기 때문에 Page의 인기도를 좀 더 정확히 반영할 수 있음
  - 참조 시점의 최근성을 반영하지 못함
  - LRU보다 구현이 복잡함

### * LRU VS LFU

![LRUVSLFU](https://jhi93.github.io/assets/img/os/LRUVSLFU.png)

\* 현재시각 5번 Page를 보관하기 위해 어떤 Page를 삭제하는가?

- LRU : 1번 Page 삭제
- LFU : 4번 Page 삭제

LRU 단점 : 마지막 참조시점만 보기 때문에 그 이전의 기록을 생각하지 않는다.
(가장 오래전에 참조되긴 했지만 참조 비율이 높을 수 있다.)

LFU 단점 : 참조횟수가 적지만 이제 막 여러번의 참조가 시작되는 Page를 쫓아낼 수 있다.

![LRULFUAlogrithm](https://jhi93.github.io/assets/img/os/LRULFUAlogrithm.png)

### * LRU

**참조 시간 순서로 한 줄로 줄세우기**
맨 위 - 가장 오래전에 참조된 것
맨 아래 - 가장 최근에 참조된 페이지

-> 새로운 페이지 올라가면 맨 아래로 보내면 됨( 가장 최근이기 때문 ) 쫓아내는 것은 맨 위에 것을 쫓아내면 됨
=> 따라서 시간 복잡도 O(1) : 쫓아내기 위해서 비교가 필요 없기 때문

### * LFU

**참조 횟수로 한 줄로 줄세우기**
맨 위 - 참조횟수가 가장 적은 것
맨 아래 - 참조횟수가 가장 많은 것

-> 새로운 페이지 올라가면 하나하나 다 비교해야한다.
=> 따라서 시간 복잡도 O(n) : n번 비교해야기 때문 비효율적이다..
=> 힙을 이용해서 구현하는 것이 더 효율적이다. => O(log n)















-----------











# 1. 다양한 캐싱환경

- 캐싱 기법
  - **한정된 빠른 공간(=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식**
  - Paging System 외에도 Cache Memory, Buffer Caching, Web Caching 등 다양한 분야에서 사용
- 캐쉬 운영의 시간 제약
  - 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
  - Buffer Caching이나 Web Caching의 경우
    - O(1)에서 O(log n) 정도까지 허용
  - Paging System인 경우
    - Page Fault인 경우에만 OS가 관여함
    - 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알 수 없음
    - O(1)인 LRU의 List 조작조차 불가능

cf)
**Cache Memory : CPU와 Main Memory 사이에 존재하는 메모리**
Buffer Caching : 파일 시스템에 대한 read/write 요청을 메모리에서 빠르게 서비스하는 방식
=> 매체는 동일하고 빠른 것은 물리적 메모리
Web Caching : 웹페이지에 대한 요청 => 멀리에 있는 웹서버에서 가져와서 브라우져에 뿌려줌
동일한 URL에 요청하는것을 웹서버에서 불러오는 것은 시간이 오래걸리므로 저장해서 씀

# 2. Paging System에서 제약사항

> Paging System에서 LRU, LFU 가능한가?

\* 주소변환 증 요청한 페이지가 존재한 경우(=Vaild)
그대로 주소변환하면 되므로 CPU 권한을 그대로 프로세스가 갖고 있고 하드웨어적으로 주소변환해서 CPU로 읽어 들인다.

\* 주소변환 중 요청한 페이지가 존재하지 않는 경우 (=Invaild => Page Fault)
디스크 I/O 작업이 필요하게 되고 프로세스가 읽어올 수 없으므로 Trap이 발생한다.
이는 CPU 권한이 프로세스에서 운영체제로 넘어가게 되고 운영체제가 I/O 작업을 해준다.
( 필요한 경우 Replacement 실행
=> 운영체제가 가장 최근에 참조되었는지(LRU), 참조 빈도가 높은지(LFU) 알 수 없다.
Valid일때는 하드웨어적으로 이미 처리되어서 운영체제에 참조 시간이나 빈도에 대한 정보를 주지 않기 때문에… )

그렇다면 Paging System에서는 어떤 알고리즘을 사용해야하는가?

# 3. Clock Algorithm

- LRU의 근사 알고리즘
- 여러 명칭으로 불림
  - Second Chance Algorithm
  - NUR (Not Used Recently) or NRU (Not Recetly Used)
- Reference Bit을 사용해서 교체 대상 페이지 선정
- Reference Bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동
- 포인터 이동하는 중 Reference Bit 1은 모두 0으로 바꿈
- Reference Bit이 0인 것을 찾으면 그페이지를 교체
- 한 바퀴 돌아와서도(=Second Chance) 0이면 그때에는 Replace 당함
- 자주 사용되는 페이지라면 Second Chance가 올때 1
- 개선사항?
  - Reference Bit와 Modified Bit(Dirty Bit)을 함께 사용
  - Reference Bit = 1 : 최근에 참조된 페이지
  - Modified Bit = 1 : 최근에 변경된 페이지 (I/O를 동반하는 페이지)

\* **Reference Bit**
=> 하드웨어가 어떤 페이지가 참조가 되었다면 Reference Bit을 1로 셋팅
페이지가 참조되었다고 표시함.

\* **Modified Bit**
=> 최근 변경된 페이지로 메모리에서 쫓겨날때 Modified Bit가 1인경우는 Backing Store(=Disk)에 해당 내용을 반영한 후 쫓아내야한다.

![ClockAlogorithm](https://jhi93.github.io/assets/img/os/ClockAlogorithm.png)

설명)
사각형 : Page Frame
Reference Bit : 1인 페이지는 최근 참조되었다는 것을 의미한다.

H/W가 참조된 것을 Reference Bit 1로 바꾸는 역할을 하고
운영체제는 Reference Bit를 확인하면서 어떤 것을 쫓아낼 지 결정한다.
Circulus Linked List를 돌면서
Reference Bit이 1인 것은 0으로 바꾸고 다음 페이지를 확인한다.
Reference Bit이 0이면 쫓아낸다.
=> 어느정도 LRU와 비슷한 효과를 낼 수 있다.

# 4. Page Frame의 Allocation

- 할당 문제 : 각 Process에 얼마만큼의 Page Frame을 할당할 것인가?
- Allocation의 필요성
  - 메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조
    => 명령어 수행을 위해 최소한 할당되어야 하는 Frame의 수가 있음
  - Loop를 구성하는 Page들은 한꺼번에 Allocate 되는 것이 유리함
    => 최소한의 Allocation이 없으면 Loop마다 Page Fault
- Allocation Scheme
  - **Equal Allocation** : 모든 프로세스에 똑같은 갯수 할당
  - **Proportional Allocation** : 프로세스 크기에 비례하여 할당
  - **Priority Allocation** : 프로세스의 Priority에 따라 다르게 할당

## Global vs Local Replacement

- - **Global Replacement**

    굳이 미리 할당하지 않고 Working Set이나 PFF 같은 알고리즘을 사용하면 그때그때 프로그램별로 알아서 할당되는 효과가 나온다.

  - Replace 시 다른 Process에 할당된 Frame을 빼앗아 올 수 있다.
  - Process별 할당량을 조절하는 또 다른 방법
  - FIFO, LRU, LFU 등의 알고리즘을 Global Replacement로 사용시에 해당
  - Working Set, PFF 알고리즘 사용

- - **Local Replacement**

    프로그램들한테 미리 메모리 할당을 한다면 프로그램은 자신에게 할당된 메모리내에서 쫓아내고 올리고를 할 수 있다. (위와 달리 다른 프로그램 영역 x)

  - 자신에게 할당된 Frame 내에서만 Replacement
  - FIFO, LRU, LFU 등의 알고리즘을 Process 별로 운영시

# 5. Trashing

> 프로그램에 메모리가 너무 적게 할당이 되어 Page Fault가 지나치게 일어나는 현상

- **프로세스의 원활한 수행에 필요한 최소한의 Page Frame 수를 할당 받지 못한 경우 발생**
- Page Fault Rate이 매우 높아짐
- CPU Utilization이 낮아짐
- OS는 MPD(Multiprogramming degree)를 높여야 한다고 판단
- 또 다른 프로세스가 시스템에 추가됨 (higher MPD)
- 프로세스 당 할당된 Frame의 수가 더욱 감소
- 프로세스는 Page의 Swap In / Swap Out으로 매우 바쁨
- 대부분의 CPU는 한가함 => Low Throughput

\* Degree of multiprogramming : 메모리에 동시에 올라가 있는 프로그램에 따라서 CPU utilization : CPU 이용률은 어떠한가?
Degree of multiprogramming을 적당히 하면 좋은 효율을 내지만
너무 높게 되면 PageFault 빈번하게 일어나서 CPU utilization이 줄어든다.
=> 따라서 동시에 메모리에 올라가는 프로세스의 개수를 조절해줘야한다.

이를 가능하게 해주는 알고리즘??

## 1) Working-Set

- Locality of reference (참조 지역성)
  - **프로세스는 특정 시간 동안 일정 장소만을 집중적을 참조한다.**
  - 집중저으로 참조되는 해당 Page들의 집합을 Locality Set이라 함.
- Working-Set Model
  - ***Locality에 기반하여 프로세스가 일정 시간 동안 원활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야하는 Page들의 집합을 Working Set이라 정의\***
  - Working Set 모델에서는 Process의 Working Set 전체가 메모리에 올라와 있어야 수행되고 그렇지 않을 경우 모든 Frame을 반납한 후 Swap Out(Suspend)
  - Thrashing을 방지함
  - Mulitprogramming Degree를 결정함

![WorkingSetAlgorithm](https://jhi93.github.io/assets/img/os/WorkingSetAlgorithm.png)

## 2) PFF(Page-Fault Frequency)

> 직접 Page Fault 비율을 본다.

![PFFScheme](https://jhi93.github.io/assets/img/os/PFFScheme.png)

- Page Size를 감소 시킨다면?
  - 페이지 수 증가
  - 페이지 테이블 크기 증가
  - 내부 단편화 감소
  - Disk Transfer의 효율성 감소
  - 필요한 정보만 메모리에 올라와 메모리 이용 효율적
    - Locality의 활용 측면에서는 좋지 않음.