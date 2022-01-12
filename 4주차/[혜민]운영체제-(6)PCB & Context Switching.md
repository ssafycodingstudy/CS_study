# 4주차 CS 발표

## PCB & Context Switching

### PCB

##### Process Management

프로세스가 여러개일 때, CPU가 CPU 스케줄링을 통해 관리

이 때, CPU는 각 프로세스들이 누군지 알아야 관리가 가능함. 
프로세스들의 특징을 갖고 있는것이 `Process Metadata`

##### Process Metadata

- Process ID
- Process State
- Process Priority
- CPU Registers
- Owner
- CPU Usage
- Memory Usage

이 메타데이터는 프로세스가 생성되면 `PCB(Process Control Block)`이라는 곳에 저장됨

#### PCB(Process Control Block)

프로세스 메타데이터들을 저장해 놓는 곳, 한 PCB 안에는 한 프로세스의 정보가 담김


![img](https://t1.daumcdn.net/cfile/tistory/25673A5058F211C224)

```
프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 → 이 프로세스의 메타데이터들이 PCB에 저장
```



##### PCB에 포함되는 정보

![img](https://blog.kakaocdn.net/dn/tIPDr/btqUnKRlmuB/DJIs4kFAwQE5ySaiJz25Kk/img.png)

- 포인터
  프로세스의 현재 위치를 저장하는 포인터 정보
- 프로세스 상태
  프로세스의 각 상태(생성(new). 준비(ready), 실행(running), 대기(waiting), 완료(terminated))를 저장
- 프로세스 번호
  모든 프로세스에는 프로세스 식별자를 저장하는 프로세스 ID 또는 PID라는 고유한 ID가 할당
- 프로그램 카운터
  프로세스를 위해 실행될 다음 명령어의 주소를 포함하는 카운터를 저장
- 레지스터
  누산기, 베이스, 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보
- 메모리 제한
  이 필드에는 운영체제에서 사용하는 메모리 관리 시스템에 대한 정보가 포함. 여기에는 페이지 테이블, 세그먼트 테이블 등이 포함될 수 있음
- 열린 파일 목록
  이 정보에는 프로세스를 위해 열린 파일 목록이 포함

##### PCB가 필요한 이유

CPU에서는 프로세스의 상태에 따라 교체 작업이 이루어짐. (interrupt가 발생해서 할당받은 프로세스가 waiting 상태가 되고, 다른 프로세르를 running으로 바꿔 올릴 때)

이 때, **앞으로 다시 수행할 대기 중인 프로세스에 관한 프로세스에 관한 저장값을 PCB에 저장함**

##### PCB 관리 방식

Linked List 방식으로 관리

PCB List Head에 PCB들이 생성될 때마다 붇게 됨. 주소 값으로 연결이 이루어져 있는 연결리스트이기 때문에 삽입, 삭제가 용이

즉, 각 프로세스가 생성될 때마다 고유의 PCB가 생성되고, 프로세스가 완료되면 PCB는 제거됨.



이렇게 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 `Context Switching` 이라고 함



### Context Switching

CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에 읽어 레지스터에 적재하는 과정

보통 인터럽트가 발생하거나, 실행중인 CPU 사용 허가시간을 모두 소모하거나, 입출력을 위해 대기해야하는 경우에 Context Switching이 발생

`즉, 프로세스가 Ready →  Running, Running →  Ready, Running → Waiting처럼 상태 변경 시 발생 `



![img](https://blog.kakaocdn.net/dn/cmulEL/btqUtV5vwgu/5VQXzE8HpuWiTTearkJyK1/img.png)

###### Context Switching이 일어나는 시점

- 멀티 태스킹
  멀티 태스킹 환경에서 프로세스 전환 과정에서 문맥교환이 일어남
  선점형 시스템에서는 스케줄러가 프로세스를 전환할 수 있음
- 인터럽트 처리
  인터럽트가 발생할 때 문맥 교환이 일어남
- 사용자 및 커널모드 전환
  OS에서 사용자 모드와 커널 모드 사이의 전환이 필요할 때 문맥 교환이 발생할 수 있음



##### Context Switching의 Overhead

문맥 교환중에는 다른 작업을 할 수 없기 때문에 이 시간을 `오버헤드`라고 할 수 있음

프로세스 작업 중에는 오버헤드를 감수해야하는 상황이 있음

```
프로세스를 수행하다가 입출력 이벤트가 발생해서 대기 상태로 전환시킴
이때, CPU를 그냥 놀게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적
```

즉, CPU에 계속 프로세스를 수행시키도록 하기 위해서 다른 프로세스를 실행시키고 문맥교환 하는것
CPU가 놀지 않도록 만들고, 사용자에게 빠르게 일처리를 제공해주기 위한 것



###### 오버헤드 해결 방안

- 문맥 교환이 자주 발생하지 않도록 다중 프로그래밍의 정도를 낮춤
- 스택 중심의 장비에서는 Stack 포인터 레지스터를 변경하여 프로세스 간 문맥 교환을 수행
- 스레드(Thread)를 이용하여 문맥 교환 부하를 최소화



---

[참고 1] : <https://jwprogramming.tistory.com/16>

[참고 2] : <https://gyoogle.dev/blog/computer-science/operating-system/PCB%20&%20Context%20Switching.html>

[참고 3] : <https://yoongrammer.tistory.com/52>

[참고 4] : <https://yoongrammer.tistory.com/53?category=961743>
