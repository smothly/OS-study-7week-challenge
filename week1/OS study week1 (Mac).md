# chapter. 0



운영체제란(Operating System, OS) ?

> 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
>
> 각 층이 서로 상호작용 interaction
>
> 관리자



운영체제의 목표

> * 컴퓨터 시스템을 편리하게 사용할 수 있는 환경을 제공
>   - 동시 사용자/프로그램들이 각각 독자적 컴퓨터에서 수행되는 것 같은 환상을 제공
>   - 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행
> * 컴퓨터 시스템의 자원을 효율적으로 관리
>   * 프로세서,기억장치,입출력 장치 등의 효율적 관리
>   * cpu 번갈아 할당, 메모리 분배



운영체제의 분류

> * BY 동시 작업 가능 여부
>   * 단일 작업(single tasking) - 한 번에 하나의 작업만 처리 - MS-DOS
>   * 다중 작업(multi tasking) - 동시에 두 개 이상의 작업 처리 - UNIX, MS Windows
> * BY 사용자의 수
>   * 단일 사용자(single user)
>   * 다중 사용자(multi user)
> * BY 처리방식
>   * 일괄 처리 방식(batch processing) - 작업 요청의 일정량 모아서 한꺼번에 처리, 작업이 완전히 종료될 때까지 기다려야 함 - 초기 Punch Card 처리 시스템
>   * 시분할(time sharing) - 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용, 일괄 처리 시스템에 비해 짧은 응답 시간 , interactive한 방식 - UNIX
>   * 실시간 (Realtime OS) - 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야하는 실시간시스템을 위한 OS - 원자로 제어, 반도체 장비 - 확장 개념 ( Hard realtime system, Soft realtime system )



새로운 용어

> Multitasking 
>
> Multiprogramming - 여러 그로그램이 메모리에 올라가 있음을 강조
>
> Time Sharing - CPU의 시간을 분할하여 나누어 쓴다는 의미 강조
>
> Multiprocess
>
> -> 모두 컴퓨터에서 여러 작업을 동시에 수행하는 것을 뜻한다.
>
> Multiprocessor - 하나의 컴퓨터에 여러개의 CPU가 붙어 있음을 의미



운영체제의 예

> 유닉스, DOS, MS Windows, Handheld device를 위한 OS(PalmOs,TinyOS)



운영체제의 구조

> ![image-20201010082916653](C:\Users\biire\AppData\Roaming\Typora\typora-user-images\image-20201010082916653.png)





# chapter. 1

> review 
>
> ------------------------------
>
> 운영체제의 의미 - 협의(mostly), 광의
>
> 운영체제의 목적 =  상단에게 , 하단에게


컴퓨터 시스템 구조

> cpu -memory- / I/O device 



Mode bit

> * 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치 필요
> * Mode bit을 통해 하드웨어적으로 2가지 모드의 operation 지원 ( 1사용자 모드, 0모니터 모드 )



Timer

> 정해진 시간이 흐른 뒤 운영체제에서 제어권이 넘어가도록 인터럽트를 발생시킴
>
> 타이머는 매 클럭 틱때마다 1씩 감소
>
> 타이머 값이 0이 되면 타이머 인터럽트 발생
>
> CPU를 특정 프로그램이 독점하는 것으로부터 보호
>
> time sharing을 구현하기 위해 널리 이용



Device Controller

> 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
>
> 제어 정보를 위해 control register, status register를 가짐
>
> local buffer를 가짐 (일종의 data register)
>
> 실제 device와 local buffer 사이에서 일어남
>
> Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림
>
> (device driver-OS 코드 중 장치별 처리루틴, device controller-각 장치를 통제하는 일종의 작은 CPU)



입출력(I/O)의 수행

> 모든 입출력 명령은 특권 명령
>
> 과정 : 시스템콜 -> trap 사용하여 인터럽트 벡터의 특정 위치로 이동 -> 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동 -> 올바른 I/O요청인지 확인 후 I/O 수행 -> I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김



인터럽트

> 인터럽트 당한 시점의 레지스터와 program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
>
> 넓은 의미 - 하드웨어인터럽트(하드웨어가 발생시킴), 소프트웨어인터럽트(Trap, Exception-프로그램이 오류를 범한 경우, System call-프로그램이 커널 함수를 호출하는 경우)
>
> 인터럽트 벡터 - 해당 인터럽트의 처리 루틴 주소를 가지고 있음
>
> 인터럽트 처리 루틴 - 해당 인터럽트를 처리하는 커널 함수
>
> 현대의 OS는 인터럽트 에 의해 구동된다



시스템콜

> 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것



동기식 입출력과 비동기식 입출력

> 동기식 입출력(synchronous I/O)
>
> I/O 요청후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
>
> 구현방법( I/O가 끝날 때까지 CPU를 낭비시킴, 매시점 하나의 I/O만 일어날 수 있음)
>
> 구현방법2(I/O가 완료될 떄까지 해당 프로그램에게서 CPU를 빼앗음,I/O 처리를 기다리는 줄에 그 프로그램을 줄을 세움, 다른 프로그램에게 CPU를 줌)
>
> 
>
> 비동기식 입출력(asynchronous I/O)
>
> I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감
>
> 
>
> 두 경우 모두 I/O의 완료는 인터럽트로 알려줌



DMA

> 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
>
> CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
>
> 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴
>
> 



저장장치 계층구조

> ![image-20201010085744176](C:\Users\biire\AppData\Roaming\Typora\typora-user-images\image-20201010085744176.png)



프로그램의 실행 (메모리 load)

> <img src="C:\Users\biire\AppData\Roaming\Typora\typora-user-images\image-20201010085825824.png" alt="image-20201010085825824" style="zoom:50%;" />



커널 주소 공간의 내용

> 커널 코드
>
> 시스템콜, 인터럽트 처리 코드
>
> 자원 관리를 위한 코드
>
> 편리한 서비스 제공을 위한 코드
>
> data
>
> stack



사용자 프로그램이 사용하는 함수

> 사용자 정의 함수
>
> 라이브러리 함수
>
> 커널 함수













---------------------------

## Quiz

1. Multiprocess와 Multiprocessor의 의미 차이?

2. 운영체제의 자원 관리시에 효율성 말고도 신경써야 하는 특성은?

3. Mode bit는 2가지 모드의 operation을 지원한다. 2가지는?

4. DMA가 인터럽트를 발생시키는 단위는?

   


