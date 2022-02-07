# 7주차 CS 발표

## TCP 3 way handshake & 4 way handshake

### TCP 3 way handshake

TCP는 장치들 사이에 논리적인 접속을 **성립(estalish)** 하기 위하여 three-way handshake를 사용함

TCP/IP 프로토콜을 이용해서 통신을 하는 응용 프로그램이 **데이터를 전송하기 전에 먼저** 정확한 전송을 보장하기 위해 상대방 컴퓨터와 **사전에 세션을 수립하는 과정**을 의미

 ```
Client > Server : TCP SYN
Server > Client : TCP SYN ACK
Client > Server : TCP ACK
 ```

*SYN : synchronize sequence numbers, 가장 많이 활용되는 패킷으로 서로 연결(프로세스 통신)을 하기 위해 최초로 connection하는 신호*

*ACK : acknowledgement, 수신자가 송신자측에서 보내온 패킷을 정상적으로 전달받았음을 알려줌*



#### TCP 3-way handshake 역할

- 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 함
- 양쪽 모두 상대편에 대한 초기 순차일련번호를 얻을 수 있도록 함



#### TCP 3-way handshake 과정

![img](https://camo.githubusercontent.com/4acea6af95884347810f057d00c6c4643a56d4a7dbbdf49740745560cd45cc1f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f5443502d636f6e6e656374696f6e2d312e706e67)

1. 클라이언트가 서버에게 접속을 요청하는 SYN 패킷을 보냄 (시퀀스 넘버를 생성해서 SYN 패킷에 담아 보냄 - sequence : x, SYN 값은 랜덤하게 정해짐 - x=rand())
2. 서버가 SYN(x)을 받고, 클라이언트로 받았다는(요청을 수락한다는) 신호인 ACK와 SYN패킷을 보냄 (서버도 시퀀스 넘버를 생성해서 SYN 패킷에 보냄 -sequence : y =rand(), 클라이언트에서 받은 시퀀스 넘버에 +1을 해서 ACK로 보냄 - ACK : x + 1)
3. 클라이언트는 서버로부터 ACK(x+1)와 SYN(y) 패킷을 받고, ACK(y+1)를 서버로 보냄. ACK가 오지 않는다면 데이터가 전달되지 않았다고 판단한 후 다시 보냄 

이렇게 3번의 통신이 완료되면 연결이 성립되고 데이터가 오가게 됨.

위와 같은 방식으로 통신하는 것이 신뢰성 있는 연결을 맺어 준다는 TCP의 3 way handshake 방식



### TCP 4 way handshake

3-way handshake는 TCP의 연결을 초기화 할 때 사용한다면, 4-way handshake는 세션을 **종료(해제)**하기 위해 수행되는 절차



#### TCP 4-way handshake 과정

![img](https://camo.githubusercontent.com/8bb8960e46a3bfada6a237a7a91bce75a0a3e0e34eab5c1f5143ca6fe34d0b5f/68747470733a2f2f6d656469612e6765656b73666f726765656b732e6f72672f77702d636f6e74656e742f75706c6f6164732f434e2e706e67)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보냄
2. 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보냄 (이 때 모든 데이터를 보내기 위해 CLOSE_WAIT 상태가 됨)
3. 데이터를 모두 보냈다면, 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보냄
4. 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버에게 보냄. (아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT(일정시간(디폴트 240초) 동안 세션을 남겨놓고 잉여패킷을 기다리는 과정)을 통해 기다림)

- 서버는 ACK를 받은 이후 소켓을 닫음 (closed)
- TIME_WAIT 시간이 끝나면 클라이언트도 닫음 (closed)

이렇게 4번의 통신이 완료되면 연결이 해제됨



---

[참고 1] : <https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake>

[참고 2] : <https://gyoogle.dev/blog/computer-science/network/TCP%203%20way%20handshake%20&%204%20way%20handshake.html>

[참고 3] : <https://websecurity.tistory.com/93>
