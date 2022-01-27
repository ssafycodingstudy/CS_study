# 6주차 CS 발표

## OSI 7계층

### 개념

- OSI(Open System Interconnection) 7계층은 국제 표준화 기구인 ISO(International Standardization Organization)에서 개발한 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 개방형 시스템 상호 연결 모델의 표준

- 통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문에 7계층으로 나눔

  ![img](https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png)

  

- 실제 인터넷에서 사용되는 **TCP/IP는 OSI 참조 모델을 기반으로 상업적이고 실무적으로 이용될 수 있도록 단순화**한 것

- ![img](https://media.vlpt.us/images/poiuyy0420/post/ad150139-1620-40c0-a12a-9b245a169dd1/iso7.jpg)

  계층을 지날 때마다 헤더(Header)가 붙는데, 이것은 해당 계층의 기능과 관련된 제어 정보가 포함되어있음

  ```
  AH : Application Header
  PH : Presentation Header
  SH : Session Header
  TH : Transport Header
  NH : Network Header / NT : Network Tail
  DH : Data Link Header / DT : Data Link Tail
  ```

- 제어 정보들을 모두 운영체제가 제공하는 프로토콜에 의해 송신 측에서는 계층을 지날 때마다 덧붙여서 추가되고, 수신 측에서는 계층을 지날 때마다 제거됨

  

### OSI 7계층

#### 1) 물리 (Physical)

실제 장치들을 연결하기 위해 필요한 전기적, 물리적 세부 사항들을 정의하는 계층. 통신 채널을 통해 전송되는 사용자 장치의 디지털 데이터를 이에 상응하는 신호들로 변환함

![img](https://media.vlpt.us/images/poiuyy0420/post/1ace8506-4022-49e9-938d-f97782656a22/image.png)

- OSI 계층을 타고 **전달된 데이터를 전기적인 신호(Bit)로 변환**시켜 통신을 수행함
- 데이터 링크 개체간의 비트 전송을 위한 물리적 연결을 설정, 유지, 해제하기 위한 수단을 제공함
- 프로토콜 : RS-232
- 장비 : 허브, 리피터, 케이블 등



#### 2) 데이터 링크 계층 (Data Link Layer)

링크의 설정과 유지 및 종료를 담당하며, 노드 간의 오류제어, 흐름제어, 회선제어 기능을 수행하는 계층.

네트워크 계층에 데이터를 전달하고, 물리계층에서 발생할 수 있는 오류를 탐지하고 수정하는 기능을 제공

![img](https://media.vlpt.us/images/poiuyy0420/post/196a557a-986a-4116-9463-15c870d9b466/image.png)

- 시스템 간에 오류 없는 데이터 전송을 위해 상위 계층에서 받은 패킷을 프레임으로 변환하여 물리 계층으로 전송함

  > 데이터 링크 계층에서 데이터 단위는 프레임(Frame)

- Mac 주소를 통해 통신함. 프레임에 Mac 주소를 부여하고 에러검출, 재전송, 흐름제어를 진행함

- 헤더의 끝에는 물리 주소 정보가 들어있고, 트레일러에는 오류를 검출하는 비트 포함한다.

- 프로토콜 : HDLC, PPP, 프레임 릴레이, ATM

- 장비 : 스위치, 브릿지 등



#### 3) 네트워크 계층 (Network Layer)

네트워크 계층은 다양한 길이의 패킷을 네트워크들을 통해 전달하고, 그 과정에서 전송 계층이 요구하는 서비스 품질(Qos)을 위한 수단을 제공하는 계층. 

라우팅, 흐름 제어, 오휴 제어, 세그먼테이션, 패킷 포워딩, 인터 네트워킹 등을 수행함

![img](https://media.vlpt.us/images/poiuyy0420/post/acb87719-7c85-47fa-92ea-ff7367508f8a/image.png)

- 패킷을 네트워크를 통해 발신지에서 목적지까지 전달하기 위해, 라우팅 프로토콜을 사용하여 최적의 경로를 선택함
- 데이터를 전송할 데이터의 주소 확인 후 전송 계층으로 전달함
- 프로토콜 : IP, ARP, RARP, ICMP, IGMP, 라우팅 프로토콜
- 장비 : 라우터, L3 스위치



#### 4) 전송 계층 (Transport Layer)

상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해주면서 종단 간의 사용자들에게 신뢰성있는 데이터를 전달하는 계층

순차번호 기반의 오류 제어 방식을 사용하고, 종단 간 통신을 다루는 최하위 계층으로 종단 간 신뢰성있고 효율적인 데이터를 전송함

![img](https://media.vlpt.us/images/poiuyy0420/post/ac993f35-0d71-4627-b1db-21c0d63079ac/image.png)

- 프로토콜(TCP, UDP)로 구성되어 오류 제어, 흐름 제어, 혼잡 제어 등을 담당하면서, 두 시스템 간에 신뢰성 있는 데이터를 전송함
- 프로토콜 
  - TCP : 신뢰성, 연결지향적
  - UDP : 비신뢰성, 비연결성, 실시간
- 장비 : L4 스위치



#### 5) 세션 계층 (Session Layer)

응용 프로그램 간의 대화를 유지하기 위한 구조를 제공하고, 이를 처리하기 위해 프로세스들의 논리적인 연결을 담당하는 계층.

통신 중 연결이 끊어지지 않도록 유지시켜주는 역할을 수행하기 위해 TCP/IP 세션 연결의 설정과 해제, 세션 메세지 전송 등의 기능을 수행함

![img](https://media.vlpt.us/images/poiuyy0420/post/cd181df3-fd96-4c42-8b2c-d6444e841142/image.png)

- 통신을 하기 위한 세션 확립, 유지, 중단을 수행함
- 통신하는 사용자들을 동기화해주고, 오류 복구 명령들을 일괄적으로 처리함
- 프로토콜 :PRC, NetBIOS



#### 6) 표현 계층 (Presentation Layer)

애플리케이션이 다루는 정보를 통신에 알맞은 형태로 만들거나, 하위 계층에서 온 데이터를 사용자가 이해할 수 있는 형태로 만드는 역할을 담당하는 계층

수신자 장치에서 적합한 애플리케이션을 사용하여 응용계층 데이터의 부호화 및 변환 수행을 통해 송신 장치로부터 온 데이터를 해석함

![img](https://media.vlpt.us/images/poiuyy0420/post/80348520-caba-46b9-8a11-b378911c421e/image.png)

- 데이터의 효율과 보안을 위해 압축과 암호화를 수행하고, 전송을 위한 포맷으로 변경을 수행함
- 프로토콜 : JPEG, MPEG



#### 7) 응용 계층 (Application Layer)

응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행하는 역할을 담당하는 계층

응용 프로세스가 개방된 형태로 다양한 범주의 정보처리기능을 수행할 수 있도록 여러가지 프로토콜 개체에 대하여 사용자 인터페이스를 제공함

![img](https://media.vlpt.us/images/poiuyy0420/post/969dec51-5e53-4dc0-ba72-91094518932d/image.png)

- 응용 프로세스간의 정보 교환, 파일 전송 등을 담당
- 사용자 인터페이스, 전자우편, 데이터베이스 관리, 인터넷, 동영상 플레이어 등의 서비스 제공
- 프로토콜 : HTTP, FTP, SMTP, POP3, IMAP, Telnet
- 장비 :  L7 스위치



---

[참고 1] : <https://velog.io/@cgotjh/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5-OSI-7-LAYER-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-%EA%B0%81-%EA%B3%84%EC%B8%B5-%EC%84%A4%EB%AA%85>

[참고 2] : <https://velog.io/@poiuyy0420/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC>

[참고 3] : <https://gyoogle.dev/blog/computer-science/network/OSI%207%EA%B3%84%EC%B8%B5.html>


