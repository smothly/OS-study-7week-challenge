## 1주차 문제

---

- Q. I/O를 걸때 인터럽트가 몇 가지 걸리는지 설명하고, 각각의 인터럽트의 특징을 설명하시오.
> A. 2가지 <br>
> 소프트웨어 인터럽트 = 프로그램이 I/O를 위해 OS에게 CPU 권한을 넘겨주는 System call<br>
> 하드웨어 인터럽트 = I/O가 끝났을땐 하드웨어 인터럽트를 한다.

<br><br>

- Q. cache, main memory, magneitic disk, register, optical disk를 저장장치 계층 순으로 나열하고,
휘발성, 속도, 가격측면에서 어떤 계층이 높은지 설명하시오.
> A. register - cache - main memory - magneitic disk - optical disk<br>
> 위에서 아래 기준으로 휘발성 -> 비휘발성, 속도 높음 -> 낮음, 가격 높음 -> 낮음 이다. 

<br><br>

- Q. 소프트웨어 인터럽트에서 시스템 콜 말고 어떤 인터럽트가 있는가?
> A. 소프트웨어 인터럽트 = Trap<br>
시스템 콜과 Exception이 있음. Exception에러는 컴파일 에러가 안뜬다는 가정하로 진행되고 런타임 에러에 속한다.

<br><br>

- Q. 가상메모리는 어디에 위치하는가?
> A. 추상적인 개념으로 실제 하드웨어에는 저장되지 않는다. <br>
필요한 부분들은 메인메모리에 할당되고 나머지 대부분은 swap area저장된다. <br>
virtual memory는 현재 main memory + swap area라고 생각하면 된다.

<br><br>

Q. 다음은 Interrupt-driven I/O Cycle의 과정이다. 박스 안에 정답을 채워 넣으시오.

   ![week1_problem_1.jpg](https://github.com/gashe-soo/OS-7week-KOCW/blob/main/asset/week1_problem_1.jpg?raw=true)

   - a.  interrupt handler process data, returns from interrupt
   - b. initiates I/O
   - c. CPU receiving interrupt, transfers control to interrupt handler
   - d. CPU resumes processing of interrupted task
   - e. input ready, output complete, or error generates interrupt signal
   - f. device driver initiates I/O

> A. f-b-e-c-a-d
![week1_problem_1_answer.jpg](https://github.com/gashe-soo/OS-7week-KOCW/blob/main/asset/week1_problem_1_answer.jpg?raw=true)

<br><br>

Q. 다음 중 옳은 것을 모두 고르시오.
   - ㄱ. 운영체제는 사용의 용이성을 위해 설계되었고 자원의 이용에는 신경쓰지 않는다.
   - ㄴ. CPU는 하나의 명령어의 실행을 완료할 때마다 인터럽트 라인을 감지한다.
   - ㄷ. Mode bit이 1이 되면서 커널 모드에 진입한다.
   - ㄹ. 메인 메모리는 비휘발성 저장장치이다.
   - ㅁ. DMA는 CPU의 개입 없이 메모리와 버퍼 장치 간의 데이터를 바이트 단위로 전송한다.
   - ㅂ. Swap area는 메모리에 속한다.
   - ㅅ. Memory Mapped I/O는 메모리와 I/O가 하나의 연속된 address 영역에 할당된다.

> A. ㄴ,ㅅ <br>
> - ㄱ. 운영체제는 사용의 용이성을 위해 설계되었고 자원의 이용에도 **신경 쓴다.**
> - ㄷ. **Mode bit이 0**이 되면서 커널 모드에 진입한다.
> - ㄹ. 메인 메모리는 **휘발성** 저장장치이다.
> - ㅁ. DMA는 CPU의 개입 없이 메모리와 버퍼 장치 간의 데이터를 **블록** 단위로 전송한다.
> - ㅂ. Swap area는 **디스크**에 속한다.

<br><br>

Q. DMA 컨트롤러가 하는일이 무엇인지?
> A. I/O장치의 인터럽트를 너무 많이 걸경우 CPU에서 Memory로 가져오는 overhead가 너무 큼<br>
DMA가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송

Q. 커널 주소 공간 메모리의 code, stack, data 가 하는 일에 대해 설명해보기
> A. 
> - 코드: 커널코드 자원 관리, 시스템콜, 인터럽트 처리 코드
> - 데이터: 하드웨어 종류마다 관리하기 위한 자료구조가 있음
각 프로그램마다 PCB가 하나씩 만들어져 관리
> - 스택: 사용자 프로그램마다 커널 스택을 따로 두고 있음

<br><br>

Q. Multiprocess와 Multiprocessor의 의미 차이?
> A. 
> - Multiprocess = process가 여러개
> - Multiprocessor = cpu가 여러개

<br><br>

Q. 운영체제의 자원 관리시에 효율성 말고도 신경써야 하는 특성은?
> A. 형평성

<br><br>

Q. Mode bit는 2가지 모드의 operation을 지원한다. 2가지는?
> A.
> - 0: 커널모드
> - 1: 사용자모드

<br><br>

Q. DMA가 인터럽트를 발생시키는 단위는?
> A. byte가 아닌 block 단위