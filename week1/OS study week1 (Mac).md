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





