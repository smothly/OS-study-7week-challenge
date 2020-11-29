## 6주차 문제

---

<br><br>
- Q. 다음 질문에 대해 답하라.

  ```
  A. Clock 알고리즘은 (1) 알고리즘의 근사 알고리즘이다. (1)에 들어갈 단어는?
  B. 개선된 Clock 알고리즘은 (2),(3) 비트를 사용한다. (2),(3)에 들어갈 단어는?
  C. 개선된 Clock 알고리즘은 단순한 Clock 알고리즘을 어떤 점을 개선하였고 그에 대한 효과는 어떠한 지 설명해라.
  ```
   - (1) : 근사 / clock 알고리즘은 LRU 알고리즘의 근사 알고리즘이다.
   - (2) : 참조 (3) : 변경 
   - 참조 비트에 변경 비트를 사용하여 I/O 횟수를 줄이기 위해 변경된 페이지에 대해 우선순위를 준다. 



  <br><br>

-  Q. 다음 페이지 참조열을 고려하여 질문에 답하시오.

  ```
  1,2,3,4,2,1,5,6,2,1,7
  ```

  - 4 frame과 FIFO 알고리즘을 사용할 경우의 page fault 수를 구하라.
  - 4 frame과 LRU 알고리즘을 사용할 경우의 page fault 수를 구하라.

   - <img src="https://github.com/gashe-soo/OS-7week-KOCW/blob/main/asset/week6_answer_img1.jpg?raw=true" alt="week6_answer_img1.jpg"/>
   - <img src="https://github.com/gashe-soo/OS-7week-KOCW/blob/main/asset/week6_answer_img2.jpg?raw=true" alt="week6_answer_img2.jpg"/>
<br><br>

- Q. ** 페이지 부재가 발생할 때, 페이지를 요청한 프로세스는 페이지가 디스크로부터 물리메모리로 적재될 때까지 기다리면서 봉쇄되어야 한다. 5개의 사용자 수준 스레드를 포함하고 사용자 수준 스레드와 커널 스레드의 사상이 다대일인 프로세스가 있다고 하자. 만약 한 사용자 스레드가 자신의 스택을 접근하려다 페이지 부재를 발생시키면 같은 프로세스에 속한 다른 사용자 스레드도 페이지 부재의 영향을 받아야 하는가? 즉 그 페이지가 메모리에 적재될 때까지 같이 기다려야 하는가? 여러분의 답을 정당화 하시오.**

  
 >  A. 하나의 프로세스가 페이지 부재에 의해서 봉쇄형이 되어버리면 다른 스레드도 봉쇄가 되어 그 페이지가 메모리에 적재될 때까지 기다려야 한다. 

<br><br>

* Q.  Optimal 알고리즘에서 미래의 참조를 어떻게 아는가?



​		A) Offline Algorithm통해

<br><br>

- Q. PFF와 Working Set Model의 공통점과 차이점은?

> A. 둘다 thrashing을 해결하기 위한 알고리즘이지만, working set은 그때 특정 프로세스가 필요할 최소한의 frame을 할당함으로서 thrashing이 일어나지 않도록 미연에 방지하지만, PFF는 page fault가 난후에 결과에 따라 frame 할당을 조절한다.

<br><br>

- Q. LRU와 LFU 알고리즘의 작동 방법과 시간측면도는?

> A. LRU는 doubly linked list와 각각 노드들을 해쉬테이블에 저장함으로써 모든 작업을 O(1)으로 할수 있다. LFU는 힙으로 구현하면 O(log n)으로 가능하지만, 각 frequency를 LRU로서 구현을 하면 사실상 모든 작업을 O(1)로 구현할수도 있다. 자세한 구현방법은 [Implementation](https://leetcode.com/problems/lfu-cache/discuss/207673/Python-concise-solution-**detailed**-explanation%3A-Two-dict-%2B-Doubly-linked-list)을 참조.

<br><br>

- Q. page를 캐싱하는데에 있어 LRU, LFU를 쓰지 못하는 이유를 설명하시오.

> A. os가 page fault가 날때마다 접근이 가능하여 참조횟수나 사용시점을 알 수 없다.

<br><br>

- Q. 옳지 않은 설명을 모두 고르시오. 옳지 않은 이유도 설명해주시오.
    - a. 디렉토리도 하나의 파일이다.
    - b. 파일시스템에서 버퍼캐시는 커널메모리 내부에 있다.
    - c. LRU, LFU 알고리즘을 파일 버퍼캐시에 사용이 가능하다.
    - d. file descriptor table은 system전체에 하나로 존재한다.

> A. d
> - d - process마다 존재하다. open file table이 시스템 전체에 존재한다.
