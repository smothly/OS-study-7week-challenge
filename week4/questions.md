## 4주차 문제

---

- Q. 우리나라의 수도는?
> A. 서울

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
