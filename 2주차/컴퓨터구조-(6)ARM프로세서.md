# 2주차 CS 발표

## ARM 프로세서

#### 프로세서란

메모리에 저장된 명령어들을 실행하는 유한 상태 오토마톤

시스템의 상태는 프로세서에 있는 레지스터의 값들과 메모리에 저장된 값들에 의해 결정됨.
각각의 명령어는 이들의 상태 변화를 정의하며 또한 다음번에 실행될 명령어를 결정.



#### ARM : Advanced RISC Machine

즉, `진보된 RISC 기기`의 약자로 임베디드 기기에 많이 사용되는 32bit RISC 프로세서

>  **RISC** : Reduced Instruction Set Computing (감소된 명령 집합 컴퓨팅)

`단순한 명령 집합을 가진 프로세서`가 `복잡한 명령 집합을 가진 프로세서`보다 훨씬 더 효율적이지않을까?로 탄생함

이미 모바일 기기 또는 IoT 디바이스에 많이 사용되고 있으며 전세계 수많은 반도체 벤더들에 의해 다양한 제품 라인업을 갖추고있는 아키텍쳐





### ARM 구조

ARM은 RISC 설계 기반으로 '단순한 명령집합을 가진 프로세서가 복잡한 것보다 효율적'임을 기반하기 때문에 명령 집합과 구조 자체가 단순

따라서 ARM 기반 프로세서가 더 작고, 효율적이며 상대적으로 느림




![img](https://mblogthumb-phinf.pstatic.net/MjAxODA0MTBfODkg/MDAxNTIzMzE5NzI0Nzg2.aQzjMp1qKTYXGWJkGu6iM5Rjfx71zGKfupXW8wcaPbAg.6OknKoCd_kx4KTHlR6KS0bUSdX-RZIoitA8GZpKvayAg.PNG.suresofttech/image.png?type=w800)

- ARM 코어 : ARM 아키텍처의 기본 원리를 이용하여 구현한 프로세서의 핵심 부분. 일반적인 프로세서의 기본 구조와 동일하게 **레지스터, ALU, 제어장치, 명령어 해석기**와 내부에서 서로 정보를 교환하기 위한 **데이터 경로**로 구성

  - 명령어 해석기 : 입력되는 명령을 해석하기 위한 장치
  - 제어장치 : 제어에 필요한 신호를 내부 및 외부로 구동하는 역할
  - ALU (Arithmetic Logit Unit) : 32비트 산술 및 논리 연산을 수행하는 중추로 레지스터 뱅크로부터 2개의 내부버스가 연결되어있고, 연산 결과를 레지스터 뱅크 및 어드레스 레지스터로 저장하기위한 ALU 출력 버스 존재

  ![img](https://mblogthumb-phinf.pstatic.net/MjAxODA0MTBfMjc5/MDAxNTIzMzE5Nzg2MzIx.QCtm3Aif-fLmy34xE_sey4e6w-Ic7ndk8A7S9GpdDgUg.3f30jiYElze34lCMIOa7uHcqYcidjQM_7dYnA7EwbPkg.PNG.suresofttech/image.png?type=w800)

- ARM 코어에 캐시, MMU, Buffer, TCM, 버스 인터페이스 유닉과 같은 주변회로를 구성한 독립된 형체를 말함
  - 캐시 : ARM 코어에서 읽기 요청이 있을 때 명령과 데이터 전달을 최대한 빠르게 하기위한 고속 메모리 장치. 대부분 캐시를 가지고 있는 프로세서는 메모리를 관리하기위해 MMU 또는 MPU같은 제어장치가 있고, 고속으로 동작하는 코어와 저속으로 동작하는 시스템 버스 사이의 속도차이를 극복하기위해 쓰기 버퍼를 지원
  - MMU는 가상 주소를 물리 주소로 변환하는 어드레스 변환 기능이 있어 다양한 어플리케이션을 지원할 수 있음
  - AMBA : Advanced MicroController Bus Architecture. ARM사의 Bus protocol



### ARM 장점

*ARM프로세서가 RISC 보다 나은점*

| 가변 사이클 명령어                              | 메모리에서 연속적으로 레지스터를 송신<br />그로인해 실행 사이클이 고정적이지 않아 코드 집적도가 향상 |
| ----------------------------------------------- | ------------------------------------------------------------ |
| 인라인 배럴 시프터<br />(Inline Barrel Shifter) | 입력 레지스터 중 하나를 미리 처리해주는 하드웨어 컴포넌트<br />코어의 성능과 코드 집적도가 향상 |
| 16bit Thumb 명령어                              | 32비트 고정 길이 명령어와 함께 사용되기에 코드 집적도가 30% 가량 향상 |
| 조건부 실행                                     | 특정 조건 시에만 실행되기에 분기 명령어 사용을 줄여 성능이 향상 |
| DSP 확장 명령어<br />(DigitalSignalProcessor)   | 16 x 16 비트 곱셈처리를 위한 DSP가 추가되어 수행 속도 향상   |





---

[참고 1] : <https://gyoogle.dev/blog/computer-science/computer-architecture/ARM%20%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C.html>

[참고 2] : <https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=suresofttech&logNo=221249244004>

[참고 3] : <https://codingcoding.tistory.com/692>

