## 4주차 문제

---
- Q. 다음은 dining philosopher problem을 monitor로 풀어내는 코드이다. (A),(B),(C)에 들어가는 코드와 코드의 의미를 작성하시오.

```c
monitor dining_philosopher{    
    enum{thinking, hungry, eating} state[5];    
    condition self[5];        
    
 	void pickup(int i){        
        state[i] = hungry;        
        test(i);        
        // problem A!!
        (A);
    }        
    void putdown(int i){        
        state[i] = thinking;                
        test((i+4) % 5);     
        test((i+1) % 5);    
    }        
    void test(int i){        
        // problem B!!
        if((B)){            
            state[i] = eating;            
            self[i].signal();
        }    
    }        
    void init(){        
        for(int i=0;i<5;i++){            
            //problem C
            (C);        
        }    
    }
}
    
Each philosopher{    
    pickup(i);    
    eat();    
    putdown(i);    
    think();
}while(1);
```



- Q. 현재 시스템의 상태가 아래와 같다. 은행원 알고리즘을 사용하여 다음 각 상태가 불안전 상태인지 결정하라. 만일 상태가 안전하다면 safe sequence를 설명하라. 불안전하다면 이유를 설명하라.

<img src="https://github.com/gashe-soo/OS-7week-KOCW/blob/main/asset/week4_problem.jpg?raw=true" alt="week4_problem.jpg"  />


1) Available = (0, 3, 0, 1)

2) Available = (1, 0, 0, 2)


<br><br>




- Q. 어떤 시스템이 3개의 프로세스를 가지고 있는데, 각 프로세스는 2개의 리소스를 필요로 한다. 데드락이 일어나지 않는것을 보장할 최소 리소스의 갯수는?

<br><br>


- Q. 세 개의 병렬 프로세스 x, y,, z가 서로 다른 code segment를 갖고 메모리를 공유한다. process x는 세마포어 a,b,c y는 bb,c,d z는 c,d,a를 각각의 code segment를 실행하기전 요구한다. 각각 그들의 code segment를 끝내면 signal(V) 오퍼레이션을 실시한다. 모든 세마포어는 바이너리 세마포어이며 1로 초기화되었다고 생각하자. 어떤 순서도가 데드락의 염려가 없을까?

(A) X: P(a)P(b)P(c) Y:P(b)P(c)P(d) Z:P(c)P(d)P(a) <br>
(B) X: P(b)P(a)P(c) Y:P(b)P(c)P(d) Z:P(a)P(c)P(d) <br>
(C) X: P(b)P(a)P(c) Y:P(c)P(b)P(d) Z:P(a)P(c)P(d) <br>
(D) X: P(a)P(b)P(c) Y:P(c)P(b)P(d) Z:P(c)P(d)P(a) <br>

<br><br>




- Q. Semaphore를 이용한 동기화 코드와 Monitor을 이용한 동기화 코드간의 차이점은 어떤것들이 있는가?




<br><br>

- Q. Deadlock avoidance에서 Banker's algorithm과 Deadlock Detection & Recovery에서 나온 Detection 방법의 다음 할당할 프로세스를 결정하는 기준은 어떻게 다른가?

<br><br>

- Q. <br>
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblltLz%2FbtqMRlDdlpN%2FLFlESoN4Cz5LM2s8kqJJy1%2Fimg.png)<br>
위 그림에서 starvation이 어떤경우에 발생하는지 설명하고, 어떻게 해결해야하는지 설명하시오.

> A.

<br><br>

Q. <br>
![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbpfdY0%2FbtqMRkEmpkK%2FGRgHCPUKTgeJMzlViQHAL0%2Fimg.png)<br>
Deadlock Avodance내 Banker`s Algorithm을 보수적이라고 설명하는 이유를 위 그림에서 예를들어 설명해주세요.

> A.