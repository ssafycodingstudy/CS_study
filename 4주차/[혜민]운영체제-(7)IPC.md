# 4주차 CS 발표

## IPC(Inter Process Communication

프로세스는 독립적으로 실행됨. 즉, 다른 프로세스에게 영향을 받지 않는다고 말할 수 있음 (스레드는 프로세스 안에서 자원을 공유하므로 영향을 받음)

이런 독립적 구조를 가진 **프로세스 간의 통신**을 해야하는 상황이 있을 것. 이를 가능하게 해주는 것이 바로 **IPC 통신**



프로세스는 커널이 제공하는 IPC 설비를 이용해 프로세스간 통신을 할 수 있게 됨

> 커널이란?
>
> 운영체제의 핵심적인 부분으로, 다른 모든 부분에 여러 기본적인 서비스를 제공



#### IPC 종류

IPC 설비 종류도 여러가지가 있기 때문에 필요에 따라 IPC 설비를 선택해서 사용

상황에 맞는 IPC의 선택은 특히 fork()를 이용해서 만들어진 멀티 프로세스의 프로그램에 있어서 중요.

 

1. 익명 PIPE

   ![img](https://t1.daumcdn.net/cfile/tistory/247CBC4357187A3411)

   파이프는 두 개의 프로세스를 연결하는데, 하나의 프로세스는 데이터를 쓰기(write)만 하고, 다른 하나는 데이터를 읽기(read)만 할 수 있음.

   한쪽 방향으로만 통신이 가능한 **반이중(Half-Duplex) 통신**이라고도 부름

   따라서 양쪽으로 모두 송/수신을 하고 싶으면 2개의 파이프를 만들어야함

   매우 간단하게 사용할 수 있는 장점이 있고, 단순한 데이터 흐름을 가질 땐 파이프를 사용하는 것이 효율적.

   단점으로는 전이중 통신을 위해 2개를 만들어야할 때는 구현이 복잡해짐

   

2. Named PIPE (FIFO)

   익명 파이프는 통신할 프로세스를 명확히 알 수 있는 경우에 사용. (부모-자식 프로세스간 통신처럼)

   Named 파이프는 전혀 보르는 상태의 프로세스들 사이 통신에 사용
   즉, 익명 파이프의 확장된 상태로 **부모 프로세스와 무관한 다른 프로세스도 통신이 가능** (통신을 위해 이름있는 파일을 사용)

   하지만, Named 파이프 역시 읽기/쓰기가 동시에 불가능. 따라서 전이중통신을 위해 익명 파이프처럼 2개를 만들어야함.

   

3. Message Queue

   입출력 방식은 Named 파이프와 동일.

   다른 점은 메시지 큐는 파이프처럼 데이터의 흐름이 아니라 **메모리 공간.**

   **메시지 큐에 쓸 데이터에 번호를 붙이면서** 여러 프로세스가 동시에 데이터를 쉽게 다룰 수 있음.

   

4. 공유 메모리

   데이터를 공유하는 방법에는 크게 두가지가 있음. 하나는 통신을 이용해서 데이터를 주고 받는 것이고, 다른 하나는 **데이터를 아예 공유, 즉 함께 사용하는 것**

   파이프, Named 파이프, 메시지 큐가 통신을 이용한 설비라면, 공유 메모리는 **공유 메모리가 데이터 자체를 공유하도록 지원하는 설비**

   프로세스는 자신만의 메모리 영역을 가지고있음. 이 메모리 영역은 다른 프로세스가 접근해서 함부로 데이터를 읽거나 쓰지 못하도록 커널에 의해서 보호가 되는데, 만약 다른 프로세스의 메모리 영역을 침범하려고 하면 커널은 침범 프로세스에 SIGSEGV(경고 시그널- 할당된 메모리의 범위를 벗어나는 곳에서 읽거나, 쓰기를 시도할 때 발생)을 보내게 됨

   프로세스의 메모리 영역은 독립적으로 가지며 다른 프로세스가 접근하지 못하도록 반드시 보호되어야 함. 
   하지만 다른 프로세스가 데이터를 사용하도록 해야하는 상황도 필요할 것. 파이프를 이용해 통신을 통해 데이터 전달도 가능하지만, Thread에서 처럼 메모리 영역을 공유한다면 더 편하게 데이터를 함께 사용할 수 있음.

   공유 메모리는 **프로세스간 메모리 영역을 공유해서 사용할 수 있도록 허용**
   프로세스가 공유 메모리 할당을 커널에 요청하면, 커널은 해당 프로세스에 메모리 공간을 할당. 이후 어떤 프로세스건 해당 메모리영역에 접근할 수 있음.

   공유 메모리는 중개자가 없이 곧바로 메모리에 접근할 수 있기때문에 다른 모든 **IPC들 중에서 가장 빠르게 작동할 수 있음**

   

5. 메모리 맵

   공유 메모리처럼 메모리를 공유해줌. 메모리맵은 **열린 파일을 메모리에 맵핑시켜서 공유**하는 방식. (즉, 공유 매개체가 파일+메모리)

   주로 파일로 대용량 데이터를 공유해야 할 때 사용.

   

6. 소켓

   네트워크 소켓 통신을 통해 데이터를 공유.

   클라이언트와 서버가 소켓을 통해서 통신하는 구조로, 원격에서 프로세스 간 데이터를 공유할 때 사용.

   서버 단에서 bind, listen, accept를 해주어 소켓 연결을 위한 준비를 해줘야하고, 클라이언트 단에서는 connect를 통해 서버에 요청하며, 연결이 수립된 후 에는 Socket을 send함으로써 데이터를 주고 받게 됨. 연결이 끝난 후에는 반드시 Socket을 close() 해줘야함.

   

7. 세마포어

   Named 파이프, 익명 파이프, 메시지 큐와 같은 다른 IPC설비들이 대부분 프로세스간 메시지 전송을 목적으로 하는 반면에, 세마포어는 **프로세스 간 데이터를 동기화하고 보호**하는데 그 목적을 두게 됨.

   프로세스간 메시지 전송을 하거나, 혹은 공유 메모리를 통해서 특정 데이터를 공유하게 될 경우 공유하는 자원에 여러개의 프로세스가 동시에 접근하면 안되고, <u>한번에 하나의 프로세스만 접근 가능하도록</u> 만들어줘야할 것이며, 이 때 사용되는것이 **세마포어**

---

[참고 1] : <https://gyoogle.dev/blog/computer-science/operating-system/IPC.html>

[참고 2] : <https://jwprogramming.tistory.com/54>


