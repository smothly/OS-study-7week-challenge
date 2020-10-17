# Process

> 개념, 상태흐름, PCB, 문맥교환



#### 개념

>  실행중인 프로그램



#### 프로세스의 문맥 (context)

>  프로세스의 현재 상태를 나타내는데 필요한 모든 요소

- CPU 수행 상태를 나타내는 하드웨어 문맥

    Program counter

    각종 register

- 프로세스의 주소 공간

    code, data, stack

- 프로세스 관련 커널 자료 구조

    PCB(Process Control Block)

    Kernel stack



#### 프로세스의 상태

> 프로세스는 상태(state)가 변경되며 수행된다.



<img src="https://blog.kakaocdn.net/dn/dVPmA3/btqBt9OwFqv/99tTUyD3poNnk1kk1NsXik/img.png" alt="img" style="zoom:70%;" />



* Running : CPU를 잡고 인스트럭션(instruction)을 수행중인 상태

* Ready : CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족하는 상태)

* Blocked (wait, sleep) :

  * CPU를 주어도 당장 instruction을 수행할 수 없는 상태

  *  process 자신이 요청한 event(ex: I/O)가 즉시 만족되지 않아 이를 기다리는 상태

     ex) 디스크에서 file을 읽어와야 하는 경우

* Suspended (stopped) :

* 외부적인 이유로 프로세스의 수행이 정지된 상태

* 프로세스는 통째로 디스크에 swap out 된다 

  * ex) 사용자가 프로그램을 일시 정지 시킨 경우, 메모리에 너무 많은 프로세스가 올라와 있을 때(Medium-term Scheduler에 의해)

* New : 프로세스가 생성 중인 상태 (보통 프로세스의 상태로 포함되지 않음)

* Terminated : 수행이 끝난 상태 (보통 프로세스의 상태로 포함되지 않음)



![img](https://blog.kakaocdn.net/dn/T6usN/btqBuiEsfEK/P4u62g2ntsMfP9rSy3E0Hk/img.png)



* Running -> Ready -> 다음 프로세스 처리 -> Blocked -> 인터럽트 걸어서 CPU에게 통지



#### PCB (Process Control Block)

> 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보

![img](https://blog.kakaocdn.net/dn/wIsxd/btqBubr1Ias/nXdeTn0kpjo4pNiN7H2Hj1/img.png)



(1) OS가 관리상 사용하는 정보

Process state, Process ID, Priority, Scheduling information

(2) CPU 수행 관련 하드웨어 값

Program Counter, registers

(3) 메모리 관련

Code, Data, Stack 위치정보

(4) 파일 관련

Open File Descriptors



#### 문맥 교환 (Context Switch)

> CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정 (중요)



<img src="https://t1.daumcdn.net/cfile/tistory/993B77385AE8102836" alt="img" style="zoom:80%;" />



1. CPU를 내어주는 프로세스 상태를 그 프로세스의 PCB에 저장

2. CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴



> 헷갈릴 수 있는 부분 !

<img src="https://t1.daumcdn.net/cfile/tistory/9942F44E5AE817BD0F" alt="img" style="zoom:80%;" />



(1) 프로세스 A --> 인터럽트 or 시스템콜 --> 커널 --> 프로세스 A : 문맥교환 발생 X

(2) 프로세스 A --> 인터럽트 or 시스템콜 --> 커널 --> 프로세스 B : 문맥교환 발생O

(1)의 경우에도 CPU 수행 정보 등 context 일부를 PCB에 저장해야 하지만 문맥교환을 하는 (2)의 경우가 오버헤드가 훨씬 더 크다. ( (2)의 경우 cache memory flush 가 수행됨 )



#### 프로세스 스케줄링하기 위한 큐

> 프로세스들은 각 큐들을 오가며 수행된다.



* Job Queue : 현재 시스템 내에 있는 모든 프로세스의 집합 (Ready Queue + Device Queues)

- Ready Queue : 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스 집합 (Device Queue와 베타관계)

- Device Queues : I/O device 의 처리를 기다리는 프로세스 집합 (Ready Queue와 베타적 관계)



#### 스케줄러

> 스케줄러는 순서를 정해준다. 각각의 자원별로 뭘 할지 계획을 세워서 순서대로 하는 것.



- Long-term Scheduler (장기 스케쥴러/Job scheduler)
  - 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정(new -> ready 로 갈 프로세스를 결정)

  - 프로세스에 memory를 주는 문제

  - 메모리에 올라갈 프로세스 수를 제어 (메모리에 프로그램이 너무 적거나 많으면 효율/성능이 좋지 않음)

  - Time sharing system 에서는 보통 장기 스케쥴러가 없다 (무조건 ready 상태로.. (무조건 메모리에 올라감))

 

- Short-term Scheduler (단기 스케쥴러/CPU scheduler)
  - 어떤 프로세스를 다음번에 running 시킬지 결정

  - 프로세스에 CPU를 주는 문제

  - millisecond 단위로 매우 빨라야 함

 

- Medium-term Scheduler (중기스케쥴러/Swapper)
  - 메모리 여유 공간 마련을 위해 프로세스를 통째로 디스크로 쫓아냄
  - 프로세스에게서 memory를 뺏는 문제







---------------------







# Thread

> 정의, 효과, 장점



#### 정의

> "A thread (or lightweight process is a basic unit of CPU utilication" ,
>
> 프로세스(heavyweight process) 내부에 CPU 수행 단위가 여러개 존재하는 경우 ,
>
> CPU를 수행하는 단위



* thread 의 구성 ( CPU 수행과 관련, 별도로 갖고 있는 부분) 
  * PC , register set , stack space
* task ( thread 와 공유하는 부분 , 메모리 공유가 가능한 자원은 최대한 공유 ) 
  * code section , data section , os resources



<img src="https://t1.daumcdn.net/cfile/tistory/99FA6F4C5AED070303" alt="img" style="zoom:80%;" />



#### Thread 의 장점

> 장점, 기대효과, 구현방식



장점

- 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리 가능.

- 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능을 얻을 수 있다

- 병렬성을 높일 수 있다(CPU가 여러개인 컴퓨터인 경우)



기대효과

- Responsiveness " if one thread is blocked, another thread continues
- Resource Sharing : n threads can share binary code, data, resource of the process
- Economy : creating & CPU switching thread rather than a process (overhead)
- Utilization of MP(Multi Processor) Architectures : each thread may be running in parallel on a different processor



구현방식

-  Kernel Threads (supported by kernel)

- User Threads (supported by library)

- Real-time Threads







---------------------------







# Process Management 

> 프로세스 생성, 종료



#### 프로세스 생성 (Process Creation)

> COW = copy on write
>
> 과정, 모델, 주소 공간



* 과정

1. 부모 프로세스가 자식 프로세스를 생성

     COW : Copy-On-Write = 자식은 부모 자원을 그대로 공유하여 사용하고 있다가 write 발생할 경우 복사

2. 프로세스의 트리 형성

3. 프로세스는 자원을 필요로 함
   - 운영체제로 부터 받는다
   - 부모와 공유한다

4. 자원의 공유

    (1) 부모와 자식이 모든 자원을 공유하는 모델

    (2) 일부를 공유하는 모델

    (3) 전혀 공유하지 않는 모델

5. 수행(Execution)
   - 부모와 자식은 공존하며 수행되는 모델
   - 자식이 종료(terminate)될 때까지 부모가 기다리는(wait) 모델

 

* 주소 공간(Address space)
  * 자식은 부모의 공간을 복사한다

  * 자식은 그 공간에 새로운 프로그램을 올린다

    ex) UNIX 

    ​	 (1) fork() **시스템 콜**이 새로운 프로세스 생성

    ​			- 부모를 그대로 복사

     		   - 주소 공간 할당

     	(2) fork 다음에 이어지는 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올린다



#### 프로세스 종료 (Process Termination)

> exit, abort



1. 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려준다(exit)
   - 자식이 부모에게 output data를 보낸다 (via wait)
   -  프로세스의 각종 자원들이 운영체제에게 반납된다

2. 부모 프로세스가 자식의 수행을 종료시킴 (abort)

    (1) 자식이 할당 자원의 한계치를 넘어설 때

    (2) 자식에게 할당된 테스크가 더 이상 필요하지 않을 때

    (3) 부모가 종료(exit)해야하는 경우

      		- 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다
         		- 딸려있는 모든 자식을 종료시킨 후 부모를 죽이는 단계적인 종료



* exit( ) 시스템 콜 : frees all the resources, notify parent = 프로세스의 종료

1. 자발적 종료

    (1) 마지막 statement 수행 후 exit() 시스템 콜을 통해

    (2) 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다

2. **비**자발적 종료

     (1) 부모 프로세스가 자식 프로세스를 강제로 종료시킨다

      		- 자식 프로세스가 한계치를 넘어선 자원을 요청할 때
         		- 자식에게 할당된 task가 더 이상 필요하지 않을 때

     (2) 키보드로 kill, break 등을 친 경우

     (3) 부모가 종료하는 경우

   - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료된다



#### 프로세스 간 협력

> 독립적, 협력, 메커니즘



- 독립적 프로세스(Independent process)

  - 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을  미치지 못한다

- 협력 프로세스(Cooperating process)

  - 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

- 프로세스 간 협력 메커니즘 (**IPC** : Interprocess Communication)

  <img src="https://blog.kakaocdn.net/dn/lwhvc/btqBAE2wps0/erEuU6DRFV4itc36GmuJJ0/img.png" alt="img" style="zoom:80%;" />

  - message passing

  - shared memory



* Message Passing : 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템

  - Direct Communication : 통신하려는 프로세스의 이름을 명시적으로 표시

  <img src="https://blog.kakaocdn.net/dn/HuhwH/btqBAVbKAAb/NgrJnfAFoGOkmOGZtRualk/img.png" alt="img" style="zoom:80%;" />

  - Indirect Communication : mailbox(또는 port)를 통해 메시지 간접 전달

  <img src="https://blog.kakaocdn.net/dn/yGa2X/btqBCUJIJAG/f4QnMEwyvqkSkH4S2XBkz1/img.png" alt="img" style="zoom:80%;" />



- shared memory : 주소 공간을 공유하는 방법
  - shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘 
  - thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능



