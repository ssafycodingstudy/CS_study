# 1주차 CS 발표

### 컴퓨터 구조 (Computer Architecture)



#### 2. 중앙처리장치(CPU) 작동 원리



##### 중앙처리장치 (CPU, Central Processing Unit)

주기억장치에서 프로그램 명령어와 데이터를 읽어와 처리하고, 그 명령어를 수행하기 위해 필요한 전체적인 제어를 담당. 우리 몸의 뇌 같은 역할. 

- 비교와 연산을 담당하는 **산술논리연산장치(ALU)**

- 명령어의 해석과 실행을 담당하는 **제어장치(CU)**
- 속도가 빠른 데이터 기억장소인 **레지스터**로 구성



![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbT3kS1%2FbtqA58a7sz7%2Fx6W4BkWqVk1e1K5Rk20vsk%2Fimg.png)



##### 연산 장치 (ALU)

산술 연산과 논리 연산 수행

연산에 필요한 데이터를 레지스터에서 가져오고, 연산 결과를 다시 레지스터로 보내는 역할을 수행

산술/논리 연산장치, 레지스터 세트(범용 레지스터, 특수 레지스터), 내부버스로 구성



##### 제어 장치 (CU)

명령어를 순서대로 실행할 수 있도록 제어하는 장치

주기억장치에서 프로그램 명령어를 꺼내 해독하고, 그 결과에 따라 명령어 실행에 필요한 제어신호를 기억장치, 연산장치, 입출력장치로 보냄

또한 이들 장치가 보낸 신호를 받아 다음에 수행할 동작을 결정

프로그램 카운터, 명령어 레지스터, 명령어 해독기(타이밍 회로, 제어논리)로 구성



##### 내부버스

CPU의 내부버스는 **산술/논리장치 (ALU)와 레지스터간의 데이터 전송을 위한 데이터 버스**와 **제어장치로부터 발생하는 제어신호를 위한 버스**로 구성

내부버스는 입출력 장치나 기억장치와 같은 위부장치에 데이터를 전송하기 위해 시스템버스인 주소버스, 데이터버스, 제어버스를 이용

내부버스는 시스템버스와 직접 연결되지 않으며 버퍼 레지스터나 시스템 버스 인터페이스 회로를 통해 시스템 버스와 연결

 

##### 레지스터

고속 기억 장치

명령어 주소, 코드, 연산에 필요한 데이터, 연산 결과 등을 임시로 저장

CPU 종류에 따라 사용할 수 있는 레지스터 개수와 크기가 다름

용도에 따라 범용 레지스터와 특수목적 레지스터로 구분됨

- 범용 레지스터 : 연산에 필요한 데이터나 연산 결과를 임시로 저장. 데이터를 연산할 때 메모리로부터 데이터를 인출할 경우 호출 시간이 많이 걸리기때문에 CPU 내부의 레지스터에 데이터를 기억시켜두고 연산

- 특수목적 레지스터 : 특별한 용도로 사용하는 레지스터

  1) 주소를 기억하는 레지스터

  - MAR (메모리 주소 레지스터) : 읽기와 쓰기 연산을 수행할 주기억장치 주소 저장
  - PC(프로그램 카운터) : 다음에 수행할 명령어 주소 저장

  2) 데이터를 기억하는 레지스터

  - IR(명령어 레지스터) : 현재 실행중인 명령어 저장
  - MBR(메모리 버퍼 레지스터) : 주기억장치에서 읽어온 데이터 or 저장할 데이터 임시 저장
  - AC(누산기) : 연산결과 임시 저장



##### CPU의 동작 과정
	1. 주기억장치는 입력장치에서 입력받은 데이터 또는 보조기억장치에 저장된 프로그램 읽어옴
	2. CPU는 프로그램을 실행하기 위해 주기억장치에 저장된 프로그램 명령어와 데이터를 읽어와 처리하고 결과를 다시 주기억장치에 저장
	3. 주기억장치는 처리 결과를 보조기억장치에 저장하거나 출력장치로 보냄
	* 제어장치는 1~3 과정에서 명령어가 순서대로 실행되도록 각 장치를 제어 


![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLzhcb%2FbtqAQlPMXKb%2FvJQzooBpd26OHYkciechzk%2Fimg.png)



##### 명령어 세트

CPU가 실행할 명령어의 집합

연산코드와 피연산자로 구성

 - 연산코드
   	- 실행할 연산
      	-  연산, 제어, 데이터 전달, 입출력 기능
 - 피연산자 
   	- 필요한 데이터 or 저장 위치
      	- 주소, 숫자, 문자, 논리데이터를 저장

CPU가 주기억장치에서 한번에 하나의 명령어를 인출하여 해독하고 실행하는데, 이러한 일련의 활동을 **'명령어 사이클'**이라고 함

명령어 사이클은 인출, 실행, 간접, 인터럽트 사이클로 나뉨



###### 명령어 사이클

컴퓨터의 기본적인 기능은 기억장치에 기억되어있는 프로그램을 실행하는것이며, 실행된 프로그램은 명령어로 구성되어있다. 따라서 CPU는 기억장치에 기억되어있는 명령어를 인출하여 실행함으로써 프로그램을 수행한다. 이러한 CPU에서 명령어 수행과정을 살펴보면 다음과 같다

1. 명령어 인출
2. 명령어 해석
3. 명령어 실행
4. 저장
5. 명령어 처리



- 명령어 인출 사이클

  > 기억장치로부터 명령어를 가져오는것을 말함
  >
  > 인출된 명령어의 주소는 프로그램카운터(PC)에 들어있고, 인출된 명령어는 명령어 레지스터(IR)로 옮겨진다.

  - 명령어 인출 과정
    1. 프로그램 카운터(PC)는 다음에 실행될 명령어를 읽을 수 있도록 카운터의 내용을 1 증가시킨다.
    2. 기억장치로부터 인출된 명령어는 기억장치 버퍼 레지스터(MBR)를 경유하여 명령어 레지스터(IR)에 저장된다.
    3. 명령어 레지스터(IR)에 저장된 명령어는 명령어 해독기를 통하여 해석하고 필요한 동작을 수행한다.

![명령어 인출과정 도식화](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2udrn%2FbtqA4z79Pjj%2FizkGpbx7mdZlwSizNQcLXk%2Fimg.png)



- 명령어 실행 사이클

  > 명령어를 실행하는 단계
  >
  > 명령어 레지스터에 실린 명령어를 해독하고, 해독한 명령어에 따라 필요한 연산을 수행

  - 수행되는 연산의 종류

    - 데이터 이동 기능
    - 데이터 처리 기능
    - 데이터 저장 기능
    - 제어 기능

    

  - 데이터 이동 기능

    > 기억 장치와 CPU 혹은 입출력장치 사이에 데이터의 이동,  LOAD 명령어를 사용
    >
    > \* LOAD 명령어 : 데이터 이동을 위한 명령어로서 원하는 기억장치의 데이터를 CPU의 내부 레지스터인 누산기로 가져오는 명령어

    - 명령어 수행 과정
      1. 더해질 데이터가 들어있는 기억장치의 주소가 MAR에 실린다
      2. MAR에 있는 기억장치주소에 해당하는 데이터가 MBR에 실린다
      3. 누산기에 있는 데이터와 MBR에 있는 데이터가 더해지고, 결과가 누산기에 저장된다.

    ![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnlHCo%2FbtqA5QIyTFm%2Fw5CN8Nq6Y9f2FN2YHcmfB0%2Fimg.png)

    

  - 데이터 처리 기능

    > 산술연산 또는 논리연산을 통한 데이터 처리
    >
    > \* ADD 명령어 : 데이터 처리 명령어로서 누산기에 있는 데이터와 기억장치에 있는 데이터를 더한 후 그 결과를 누산기에 저장하는 명령어	

    - 명령어 수행 과정
      1. 더해질 데이터가 들어 있는기억장치의 주소가 MAR에 실린다.
      2. MAR에 있는 기억장치주소에 해당하는 데이터가 MBR에 실린다.
      3. 누산기에 있는 데이터와 MBR에 있는 데이터가 더해지고, 결과가 누산기에 저장된다.

    ![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnlHCo%2FbtqA5QIyTFm%2Fw5CN8Nq6Y9f2FN2YHcmfB0%2Fimg.png)

    

  - 데이터 저장 기능

    > 연산결과를 기억장치에 저장
    >
    > \* STORE 명령어 : 연산 결과를 갖고있는 누산기의 데이터를 기억장치에 저장하는 동작을 수행

    - 명령어 수행 과정
      1. 저장할 주소가 MAR에 실린다.
      2. 누산기에 있는 데이터가 MBR에 실린다.
      3. MBR에 있는 데이터가 MAR에 있는 주소로 저장된다.

    ![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGiVOe%2FbtqA6DBJfwe%2F86Zc6OKjz7Xi0EU1F2fRq1%2Fimg.png)

    

  - 제어기능

    > 프로그램의 실행순서를 결정
    >
    > \* 분기(branch) 혹은 점프(jump) 명령어 : 제어기능을 수행하는 명령어로서 프로그램 순서를 바꾸는 명령어

    - 명령어 수행 과정
      1. 점프 명령어가 있는 주소가 PC에 실린다. PC <- IR(Address)

    ![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCoTLy%2FbtqA5Q9BPtq%2FkbnqkGoxsm3kIynbUhmugk%2Fimg.png)

  


------

[참고 1] : <https://gyoogle.dev/blog/computer-science/computer-architecture/%EC%A4%91%EC%95%99%EC%B2%98%EB%A6%AC%EC%9E%A5%EC%B9%98%20%EC%9E%91%EB%8F%99%20%EC%9B%90%EB%A6%AC.html>

[참고 2] : <https://sangcho.tistory.com/entry/%EC%A4%91%EC%95%99%EC%B2%98%EB%A6%AC%EC%9E%A5%EC%B9%98CPU>


