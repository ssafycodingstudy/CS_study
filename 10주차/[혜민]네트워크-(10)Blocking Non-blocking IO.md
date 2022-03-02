# 10주차 CS 발표

## Blocking & Non-Blocking I/O

### Blocking

- I/O 작업은 유저레벨에서 직접 수행할 수 없음. 실제 I/O를 수행하는것은 **커널레벨**에서만 가능

  ⇒ 유저 프로세스(또는 Thread)는 커널에게 I/O를 요청해야함

- I/O에서 블로킹 형태의 작업은 유저 프로세스가 커널에게 I/O를 요청하는 함수를 호출하고, 커널이 작업을 완료하면 함수가 작업 결과를 반환함

- I/O 작업이 진행되는 동안 유저 프로세스는 자신의 작업을 중단한 채 대기 (I/O 작업이 CPU자원을 거의 쓰지 않기 때문에 이런 형태의  I/O는 리소스 낭비가 심함)

  > 여러 Client가 접속하는 서버를 Blocking 방식으로 구현하는 경우
  >
  > I/O 작업이 Blocking 방식으로 구현되면 
  >
  > 하나의 클라이언트가 I/O 작업을 진행하면 해당 프로세스(스레드)가 진행하는 작업을 중지 → 다른 Client가 진행중인 작업을 중지하면 안되므로, Client 별로 별도의 Thread를 생성해야함 → 접속자 수가 매우 많아질수록 Thread 수 증가 → 많아진 Thread들로 Context Switching 횟수가 증가 ⇒ 비효율적

<img src="https://t1.daumcdn.net/cfile/tistory/2371EC4955160B8714" alt="img" style="zoom:75%;" />

### Non-Blocking

- Blocking 방식의 비효율성을 극복하고자 만들어진 방식
- I/O 작업을 진행하는 동안 유저 프로세스의 작업을 중단시키지 않음.
- 유저 프로세스가 커널에게 I/O를 요청하는 함수를 호출하면, 함수는 I/O를 요청한 다음 진행상황과 상관없이 바로 결과를 반환함

<img src="https://t1.daumcdn.net/cfile/tistory/253D9E475516150118" alt="img" style="zoom:75%;" />

- 진행 순서

  1. User Process가 recvfrom 함수 호출 (커널에게 해당 Socket으로부터 데이터를 받고싶다고 요청)
  2. 커널은 이 요청에 대해서 상대방의 데이터를 전송받아 recvBuffer에 저장 & 유저에게 그 내용을 복사
     -  recvBuffer가 비어있다면  "EWOULDBLOCK"을 리턴 → 유저 프로세스는 다른 작업을 진행할 수 있음
     - recvBuffer에 유저가 받을 수 있는 데이터가 있는 경우, Buffer로부터 데이터를 복사하여 받아옴
     - recvBuffer는 커널이 갖고 있는 메모리에 적재되어 있으므로 메모리간 복사로 인해 I/O보다 훨씬 빠른 속도로 데이터를 받아올 수 있음 
  3. recvfrom함수는 빠른 속도로 읽을 수 있는 데이터를 복사해주고 복사한 데이터의 길이와 함께 반환함

  ⇒ 위 모든 반환이 I/O의 진행시간과는 관계없이 빠르게 동작하기 때문에, 유저 프로세스는 자신의 작업을 오랜 시간 중지하지 않고도 I/O 처리를 수행할 수 있음




---

[참고 1] : <https://ozt88.tistory.com/20>

[참고 2] : <https://gyoogle.dev/blog/computer-science/network/Blocking%20&%20Non-Blocking.html>
