# chp 05. CPU Scheduling

-------------------

> index



### 1. CPU and I/O Bursts in Program Execution

> CPU burst 와 I/O Burst가 반복적으로 일어난다.

![CPUBurstime](https://jhi93.github.io/assets/img/os/CPUBurstime.png)

- I/O-bound process
  - CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 job
  - many short CPU bursts
- CPU-bound process
  - 계산 위주의 job
  - few very long CPU bursts

I/O bound job은 사용자와 interactive한 작업을 하는데
cpu를 오랫동안 잡지 못하면 사람이 답답해할 수 있다.
=> 따라서 CPU Scheduling이 중요하다.



### 2. CPU Scheduler & Dispatcher

- **CPU Scheduler**
  **: Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.**
- **Dispatcher**
  **: CPU의 제어권을 CPU Scheduler에 의해 선택된 프로세스에게 넘긴다.**
  (= **Context Switch**)

\* CPU 스케줄링이 필요한 경우?

1. Running -> Blocked (ex. I/O 요청하는 시스템 콜)
2. Running -> Ready (ex. 할당한 시간만료 =timer interrupt)
3. Blocked -> Ready (ex. I/O 완료 후 인터럽트)
4. Terminate

Non-Preemptive(= 비선점 : 강제로 뺏지 않고 자진 반납) : 1,4
Preemptive(= 선점 : 강제로 뺏음) : 2,3



### 3. Scheduling Criteria (성능 척도)

1. - 시스템 입장에서의 성능척도

     CPU 하나 가지고 최대한 일을 많이 시키는 것.**CPU Utilization(이용률)**전체 시간에서 CPU가 놀지 않고 일한 시간의 비율**Throughput(처리량)**주어진 시간동안 몇 개의 일을 완료했는가

2. - 프로그램 입장에서의 성능척도

     내가 빨리 CPU를 얻어서 빨리 끝내는 것.**Turnaround Time(소요시간, 반환 시간)**CPU를 쓰러 들어와서 다쓰고 나가는 데까지 걸리는 시간(기다린 시간 포함)**Waiting Time(대기 시간)**CPU를 쓰기 위해 기다리는 시간(순수하게 기다린 시간)**Response Time(응답 시간)**처음으로 CPU를 얻기까지 걸린 시간

cf) 선점일 경우 완료될때 까지 CPU를 쓰는 것이 아니라 계속해서 얻었다 뺐겼다 반복.
그러면서 총 기다린 시간 = Waiting Time,
맨 처음 기다린 시간 = Response Time



### 4. Scheduling Algorithms

#### 1) FCFS (First-Come First-Served)

> 먼저 온 순서대로 처리한다. **비선점 스케줄링**

![FCFS](https://jhi93.github.io/assets/img/os/FCFS.png)

cf) Convoy Effect : 긴 프로세스가 먼저 오는 바람에 짧은 프로세스들이
실행 되지 못하고 오래 기다리게 되는 현상을 의미한다.



#### 2) SJF (Shortest-Job-First == 선점일때 : SRTF)

> CPU 작업시간이 짧은 프로세스한테 먼저 CPU를 할당한다. **비선점,선점 스케줄링**

- - **Nonpreemptive(비선점)**

    일단 CPU를 잡으면 이번 CPU Burst가 완료될 때까지 CPU를 선점 당하지 않음.

- - **Preemptive(선점)**

    현재 수행중인 프로세스의 남은 Burst Time보다 더 짧은 CPU Burst Time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김. **Shortest-Remaining-Time-First(SRTF)라고 부른다.**

#### ① 비선점 SJF

> 지금 기다리는 프로세스들 중 제일 짧은 프로세스한테 할당.
> 도중에 짧은 다른 프로세스가 들어오더라도 그대로 진행.

![PreemptiveSJF](https://jhi93.github.io/assets/img/os/PreemptiveSJF.png)

#### ② 선점 SJF = SRTF

> 지금 기다리는 프로세스들 중 제일 짧은 프로세스한테 할당.
> 도중에 들어온 프로세스가 더 짧다면 CPU를 뺏는다.

![NonpreemptiveSJF](https://jhi93.github.io/assets/img/os/NonpreemptiveSJF.png)

=> 선점 SJF는 Average Waiting Time을 최소화하는 알고리즘이다.

\* 문제점?

1. Starvation(기아상태) : CPU 사용시간이 긴 프로세스는 영원히 못 받을 수 있다.
2. CPU 사용시간을 미리 알 수 없다는 문제가 존재.

cf) CPU Burst Time을 정확히 알 수는 없고 추정은 가능하다.
과거의 CPU Burst Timed을 이용하여 추정한다.



#### 3) Priority Scheduling

> 우선순위가 높은 것 먼제 CPU를 준다. **비선점,선점 스케줄링**
> (숫자가 작을 수록 우선순위가 높다.)

- - SJF는 일종의 우선순위 스케줄링에 해당된다.

  - * 우선순위 스케줄링의 문제? => **Starvation**

  - => 효율성을 중요시하는 것도 중요하지만 특정 프로세스를 차별해선 안된다.

  - * 해결방법?

  - => **Aging 기법**

    우선순위가 낮더라도 기다리는 시간이 길어질수록 우선순위를 높여준다.



#### 4) Round Robin

> CPU 할당 시간을 정해서 넘겨준다. 시간이 지나면 CPU를 뺏어온다.

- 각 프로세스는 동일한 크기의 할당 시간(Time Quantum)을 갖는다.
- 할당 시간이 지나면 프로세스는 선점당하고 Ready Queue 제일 뒤로 간다.
- n개의 프로세스가 Ready Queue에 있고 할당 시간이 q time unit인 경우
  각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.
  => 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
- **Time Quantum이 클 경우 = FCFS**
- **Time Quantum이 작을 경우 = Context Switch 오버헤드가 커진다.**
- 장점 : 응답시간이 빨라진다(Response Time)
  CPU 시간 예측할 필요 없이 짧게 쓰는 프로세스를 빨리 쓸 수 있게끔 할 수 있다. ![RoundRobin](https://jhi93.github.io/assets/img/os/RoundRobin.png)

cf)
CPU 사용시간이 긴 프로세스와 짧은 프로세스가 섞여있을때 쓰기 좋은 스케줄링.
모든 프로세스가 CPU 사용시간이 동일한 경우 좋지 않음.
CPU 사용시간이 100초인 프로그램이 여러개 있는데 FCFS로 사용하면
0초 100초 200초 이렇게 하나라도 빨리 끝낼 수 있지만
RR 1초로 쓰면 번갈아쓰다가 천초가되면 한 번에 빠져나가는 스케줄링이 되기때문.



#### 5) MultiLevel Queue

> Ready Queue를 여러 개로 분할
>
> - **Foreground(Interactive) - RR**
> - **Background(batch - no human Interactive) - FCFS**
> - Fixed Priority Scheduling
> - Time Slice : 각 큐에 CPU Time 적절한 비율로 할당
>   ex) 80% to Foreground in RR / 20% to Background in FCFS

\* 프로세스를 어느줄에 넣을 것인가?
우선순위가 높은 거에만 주면 우선순위가 낮은프로세스는 Starvation문제가 생긴다. ==> 큐가 두개 존재 (두줄로 줄을 선다)
각 줄마다 스케줄링 방식을 채택

![MultilevelQueue](https://jhi93.github.io/assets/img/os/MultilevelQueue.png)



#### 6) MultiLevel FeedBack Queue

- 프로세스가 다른 큐로 이동 가능.
- **에이징(Aging)**을 이와 같은 방식으로 구현할 수 있다.
- MultiLevel-FeedBack-Queue Scheduler를 정의하는 파라미터들
  - Queue의 수
  - 각 큐의 Scheduling Algorithm
  - Process를 상위 큐로 보내는 기준
  - Process를 하위 큐로 내쫓는 기준
  - 프로세스가 CPU 서비스를 받으려 할때, 들어갈 큐를 결정하는 기준

\* Example
![MultilevelFeedbackQueue](https://jhi93.github.io/assets/img/os/MultilevelFeedbackQueue.png)

처음 들어오는 프로세스는 우선순위가 가장 높은 큐에 들어가고 RoundRobin의 할당시간을 적게 준다.
밑에 큐로 갈수록 RoundRobin의 할당시간을 길게 준다.
맨 아래는 FCFS로 처리.

cf) 맨위 큐에서 전부 처리 못하면 강등된다.
결국 CPU 사용시간이 짧은 프로세스를 우대받는 방식이 된다.



#### 7) Multiple-Processor Scheduling

> CPU가 여러개 있을때의 스케줄링
> CPU가 여러 개인 경우 스케줄링은 더욱 복잡해짐.
>
> - Homogeneous Processor인 경우
>   - Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
>   - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐.
> - Load Sharing
>   - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요.
>   - 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법
> - Symmentic Multiprocessing(SMP)
>   - 각 프로세서가 각자 알아서 스케줄링 결정 (= 모든 CPU가 동등)
> - Asymmentic Multiprocessing
>   - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름.
>     (= 하나의 CPU가 나머지 CPU를 분배 )

cf) 어떤 job은 특정 CPU가 실행해야할 수도 있다. : Homogeneous processor
ex) 미용실에서 헤어디자이너가 여러명 있을때 전담하는 헤어디자이너에게 가는 것.



#### 8) Real-Time Scheduling

> 시간 제약안에 반드시 실행 되어야. (= DeadLine이 존재)
>
> - - **Hard Real-Time Systems**
>
>     정해진 시간 안에 반드시 끝내도록 스케줄링 해야함. ex) 원자력, 미사일 등
>
> - - **Soft Real-Time Systems**
>
>     일반 프로세스에 비해 높은 Priority를 갖도록 해야함. ex) 영화, 멀티미디어 등



#### 9) Thread Scheduling

> Thread에 따른 Scheduling

- Local Scheduling

  User Level Thread의 경우 사용자 수준의 Thread Library에 의해 어떤 Thread를 스케줄할지 결정. => 사용자 프로세스 내부 어떤 쓰레드에게 줄지 결정하는 것.

- Global Scheduling

  Kernel Level Thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케줄러가 어떤 Thread를 스케줄할지 결정. => 운영체제가 쓰레드를 알고 있기때문에 운영체제가 결정.



#### 10) Algorithm Evaluation

- Queueing Models

  확률 분포로 주어지는 Arrival Rate와 Service Rate 등을 통해 각종 Performance Index 값을 계산 => 굉장히 이론적인 방법 : 계산으로 인한 방식 =>현재는 실제 시스템에서 돌려보는 방식이 더 많이 쓰임

- Implementation (구현) & Measurement(성능 측정)

  실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해 성능을 측정 비교 => 실제 시스템에 구현을 해서 돌려보고 성능을 측정한다.

- Simulation(모의 실험)

  알고리즘을 모의 프로그램으로 작성 후 Trace를 입력으로 하여 결과를 비교 (trace : 시뮬레이션 프로그램에 인풋으로 들어갈 데이터.)

