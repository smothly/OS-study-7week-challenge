## 3주차 문제

---

- Q. CPU Scheduling에서 Round Robin 방식에 대한 설명으로 옳지 않은 것을 모두 고르시오. 옳지 않은 이유도 설명해주시오.
    - a. 할당 시간(time quantum)이 끝나면 해당 프로세스는 바로 CPU를 얻는다.
    - b. SJF보다 Average turnaround time과 response time은 더 짧아진다.
    - c. 기다리는 시간은 프로세스의 CPU burst time에 비례하게 된다.
    - d. 할당 시간(time quantum)을 크게 잡으면 context switch 오버헤드가 커진다.
> A.  a, b, d
> - a - 바로 CPU를 얻는것이 아니라 Ready queue에 맨 뒤에 가서 순서를 기다리게 된다.
> - b - SJF보다 Average turnaround time은 길어지게 된다.
> - d - context switch 오버헤드가 작아진다.
<br><br>

- Q. 아래 이미지에서 예상되는 문제점을 설명하고 어떻게 해결해야 하는지 설명하시오.
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBJu9u%2FbtqMdVlVcrN%2F5mw3gV7jaoEJd1rQrTMSR1%2Fimg.png)<br>

> A. P0과 P1이 S와 Q자원을 공유하는 상황이다.<br>
> P0가 S P1이 Q를 지니고 있으면 둘다 무한정 기다리게 되는 deadlock상황이 발생한다.<br>
> 자원의 접근 순위를 지정해 놓으면 즉, S와 Q의 우선순위를 지정해 놓으면 문제는 해결된다.

<br><br>
- Q. peterson의 알고리즘의 동작 방식을 깊이 이해하고 정상적으로 작동하는 이유에 대해 설명하세요.
> A. 


<br><br>

- Q. 세마포어와 뮤텍스가 어떤것인지 설명하고 각각 어떤 상황에서 사용할 수 있는지 설명하세요.
> A.

<br><br>

- Q. FCFS 알고리즘은 선점방식과 비선점방식을 사용할 수 있는 지 여부를 설명하고, FCFS 와 같은 전략이 적용되고 있는 실생활의 사례를 들어보시오

> A.  

<br><br>

- Q. 실생활에서 SJF 알고리즘보다 FCFS 알고리즘을 사용하는 것이 타당한 사례를 제시하고 그 이유를 설명하시오

> A.

<br><br>