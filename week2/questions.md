## 2주차 문제

---

- Q. Program Counter, code section, register set, data section, OS resources, stack space 중<br>
Thread와 공유하는 부분과 공유하지 않는 부분을 나눠 주세요.


> A.<br>
공유하는 부분: code section, data section, OS resources<br>
공유하지 않는 부분:Program Counter, register set, stack space

<br><br>

- Q. Multi thread의 장점 중 한가지를 예시로 들어 설명해주세요. ex) 웹 브라우저

> A.
응답성(Responsiveness): 웹 브라우저를 멀티 스레드로 사용하면 한 스레드는 텍스트를 먼저 가져와 보여주고 이미지 같은 경우는 오래걸리므로 다른 스레드를 사용해 가져온다.<br>
멀티 프로세싱 이용 (Utilization of MP Architectures): 행렬 곱셈 같이 독립적인 연산이 가능할 경우 멀티프로세싱이 필요한데 이 병렬 작업을 멀티스레드로 지원이 가능하다.

<br><br>

- Q. 아래 코드는 process를 생성하고 수행하는 코드이다.<br>
아래 코드를 실행했을 때의 출력과정을 설명해 주세요.<br>
![image](https://user-images.githubusercontent.com/37397737/96326477-f1404c00-106b-11eb-8ccc-4a5786913a73.png)<br>
사진 출처: https://blog.naver.com/PostView.nhn?blogId=scy7351&logNo=221522253222&parentCategoryNo=&categoryNo=21&viewDate=&isShowPopularPosts=true&from=search<br>

> A. <br>
1. hello world 출력
2. fork에서 자식프로세스가 생성되어 hello i am a child 출력
3. ls 명령어 실행한 결과 출력
4. 자식프로세스가 죽고 부모 프로세스로 감
5. 부모프로세스에서 hello i am parent 출력하고 실행 끝

<br><br>
- Q. 다음 코드에서 출력되는 값과 그 이유를 설명하시오.<br/>
```c
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <unistd.h>

int value = 5;
int main()
{
    pid_t pid;
    pid = fork();
    
    if(pid == 0){
        value += 15;
        return 0;
    }
    else if(pid > 0){
        wait(NULL);
        printf("Parent: value = %d\n",value);
        return 0;
    }
}
```
출처 : 운영체제(Operating System Concepts) 10th edition<br/>
> A. 출력된 값은 5이다. 자식 프로세스가 value +=15를 실행할 때 새로운 주소 공간으로 복제하기 때문에 부모 프로세스의 값은 변하지 않는다. <br/>
<br/>
- Q. O/X를 고르시오.<br/>
    - 같은 프로세스에 있는 각각의 쓰레드는 각각 가상 메모리 mapping을 가진다. (O/X)<br/>
    - 쓰레드는 프로세스보다 문맥 교환(context switch) 비용이 적다.(O/X)<br/>
    - 한 프로세스 내의 스레드들은 Code, Data, Heap 영역을 공유한다.(O/X)<br/>
<br/>
> A. 1) X => 스레드는 메모리를 공유한다. [참고](https://stackoverflow.com/questions/5440128/thread-context-switch-vs-process-context-switch) <br/>
2) O => 프로세스의 문맥 교환은 code,data,heap영역도 바뀌어야 하기 때문<br/>
3) O => 스레드는 stack space, program counter만 따로 갖는다.<br/>
<br/><br/>





#### Q.1. Blocked와 Suspended의 차이 ?

> Running, Ready, Blocked 모두 CPU 관점에서의 상태 분류일 뿐 실제로 **프로세스**의 작업이 수행이 되고 있는 상태 (CPU에서 프로세스 수행중(Running), I/O에서 프로세스 수행중(Blocked)) , 
>
> 반면 Suspended 는 **프로세스** 수행 자체가 외부에 의해 정지된 상태
>
> 따라서, 
>
> Blocked : Blocked 자신이 요청한 event가 만족되면 Ready
>
> Suspended : 외부에서 resume 해주어야 Active



#### Q.2.  PCB가 갖고 있는 정보가 아닌 것은?

> 1.  할당되지 않은 주변장치의 상태 정보
> 2. 프로세스의 현재 상태
> 3. 프로세스 고유 식별자
> 4. 스케줄링 및 프로세스의 우선순위



<br><br>

- Q. 프로세스와 스레드의 협력 방식의 차이점에 대해 설명하시오

> A.프로세스는 협력 매커니즘(ipc)혹은 shared memory를 통해서 협력하고 스레드는 주소 공간(code, data, heap) 공유로 협력. 스레드간 공유가 속도가 빠르고 메모리 절약이 됨<br>
<br><br>

- Q. 한 프로세스가 long term scheduler에 의해 메모리에 올라간 후 대기하다 cpu를 점유한 후

i/o request가 들어와 device controller에 관리되어진 후  i/o가 끝나 다시 memory로 돌아가는 과정을

job queue, ready queue, device queue의 관점에서 서술하시오

> A. second storage에 있는 jpb queue에서 메모리의 ready queue로 올라간 후 대기하다가 pcb가 우선순위를 갖게 됨 ->  우선순위가 있는 pcb가 i/o request를받으면 device queue로 옮겨가고 i/o가 끝나면 다시 ready queue<br>



######### Q1 ###########

프로세스와 스레드의 협력 방식의 차이점에 대해 설명하시오

########################
######### Q2 ###########

한 프로세스가 long term scheduler에 의해 메모리에 올라간 후 대기하다 cpu를 점유한 후

i/o request가 들어와 device controller에 관리되어진 후  i/o가 끝나 다시 memory로 돌아가는 과정을

job queue, ready queue, device queue의 관점에서 서술하시오

########################
