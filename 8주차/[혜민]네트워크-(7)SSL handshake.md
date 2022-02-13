# 8주차 CS 발표

## TLS/SSL handshake

### TLS/SSL

HTTPS는 SSL (Secure Socket Layer) / TLS (Transport Layer Security) 전송 기술을 사용함. TCP, UDP와 같은 일반적인 인터넷 통신에 <u>안전한 계층(layer)</u> 을 추가하는 방식.

그리고 이 기술을 구현하기 위해 웹 서버에 설치하는 것이 SSL/TLS 인증서.

TLS는 SSL의 개선 버전으로, 최신 인증서는 TLS를 사용하지만 편의적으로 SSL 인증서라고 쭉 부르고 있음

TLS/SSL handshake는 HTTPS에서 클라이언트와 서버간 통신 전 SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식



### SSL 인증서와 SSL handshake에 탑재된 기술

SSL 인증서 관련 프로세스에는 아래와 같은 보안 기술이 탑재되어있음

- 하나의 키(key)로 암호화/복호화를 수행하는 대칭키 암호화 방식
- 한 쌍의 키 페어(key pair)로 암호화/복호화를 수행하는 비대칭키 암호화 방식
- 통신 대상을 서로가 확인하는 신분 확인 (authentication)
- 믿을 수 있는 SSL 인증서를 위한 디지털 서명 (digital signature)
- 디지털 서명을 해주는 인증 기관 (CA)
- 공개키를 안전하게 전달하고 공유하기 위한 프로토콜
- 암호화된 메시지의 변조 여부를 확인하는 메시지 무결성 알고리즘



### SSL handshake 과정

![img](https://t1.daumcdn.net/brunch/service/user/JqQ/image/lhPVlfrjDBwbTZ-ooFmx1qAgyBw)

1. Client Hello (Client → Server)

   브라우저 주소창에 blog.naver.com을 치면 브라우저는 네이버 블로그 웹 서버에 접속 시도함. 

   HTTP는 TCP의 일종이니, TCP 연결을 위한 3-Way Handshake를 수행한 브라우저는 네이버 블로그가 HTTPS를 사용하는 것을 알게 된 브라우저는 다음 정보를 Client Hello 단계에서 보냄

   - 브라우저가 사용하는 SSL 혹은 TLS 버전 정보
   - 브라우저가 지원하는 암호화 방식 모음 (cipher suite)
   - 브라우저가 순간적으로 생성한 임의의 난수(숫자)
   - 만약 이전에 SSL handshake가 완료된 상태라면, 그때 생성된 Session ID
   - 기타 확장 정보(extension)

   > **cipher suite**
   >
   > 보안의 궁극적 목표를 달성하기 위해 사용하는 방식을 패키지의 형태로 묶어놓은 것을 의미. 여기서 보안의 목표란 다음과 같음
   >
   > - 안전한 키 교환
   > - 전달 대상 인증
   > - 암호화 알고리즘
   > - 메시지 무결성 확인 알고리즘

2. Server Hello (Server → Client)

   Client Hello에 응답하면서, 다음 정보를 클라이언트에 제공

   - 브라우저의 암호화 방식 정보 중, 서버가 지원하고 선택한 암호화 방식 (cipher suite)
   - 서버의 공개키가 담긴 SSL 인증서. 인증서는 CA의 비밀키로 암호화되어 발급된 상태
   - 서버가 순간적으로 생성한 임의의 난수 (숫자)
   - 클라이언트 인증서 요청 (선택 사항)

3. 브라우저는 서버의 SSL 인증서가 믿을만한지 확인 (Client)

   대부분 브라우저에는 공신력있는 CA들의 정보와 CA가 만든 공개키가 이미 설치되어있음. 서버가 보낸 SSL 인증서가 정말 CA가 만든것인지를 확인하기 위해 내장된 CA 공개키로 암호화된 인증서를 복호화해봄. 	

   정상적으로 복호화가 되었다면 CA가 발급한 것이 증명되는 셈. 만약 등록된 CA가 아니거나, 등록된 CA가 만든 인증서처럼 꾸몄다면 이 과정에서 발각되며 브라우저 경고를 보냄

4. Premaster secret 전송 (Client → Server)

   브라우저는 자신이 생성한 난수와 서버의 난수를 사용하여 premaster secret을 만듦

   웹서버 인증서에 딸려온 웹사이트의 **공개키로 premaster secret을 암호화하여 서버로 전송**. 

5. Premaster secret 값 복호화 (Server)

   서버는 사이트의 비밀키로, 브라우저가 보낸 premaster secret 값을 복호화함.

   **복호화 한 값을 master secret 값으로 저장**함. 이것을 사용하여 브라우저와 만들어진 연결에 고유한 값을 부여하기 위한 **session key를 생성**함 (세션키는 대칭키 암호화에 사용할 키. 이것으로 브라우저와 서버 사이에 주고받는 데이터를 암호화하고 복호화함)

6. SSL handshake 종료 / HTTPS 통신 시작 (Server & Client)

   SSL handshake가 정상적으로 완료되면, 웹상에서 데이터를 세션키를 사용하여 암호화/복호화하며 HTTPS 프로토콜을 통해 주고받을 수 있음. 

   HTTPS 통신이 완료되는 시점에서 서로에게 공유된 세션키를 폐기함. 만약 세션이 여전히 유지되고 있다면 브라우저는 SSL handshake 요청이 아닌 세션 ID만 서버에게 알려주면 됨



#### HTTPS를 적용했다면 100% 안전한 것인가?

예를 들어, 웹 서버가 해커의 다양한 공격에 의해 루트 권한을 탈취당했다면 모든 기밀 데이터를 열람할 수 있는 권한이 넘어갈 수도 있음. 

또한 HTTPS는 전달 구간에 대한 보안 기술인데, 전달 구간 중간에 해커가 중간자 공격(Man in the middle attack)을 수행할 수 있는 취약점이 있다면 HTTPS는 유지되지만 전달하는 내용은 고스란히 노출됨

⇒ 따라서 인스턴트 메시징 서비스와 같이 개인과 개인 혹은 그룹 간 대화, 민감한 개인 정보 등의 전달에서는 HTTPS를 적용하면서도 <u>종단 간 암호화 기술(E2E: End-to-End Encryption)</u>을 추가적으로 적용하여 HTTPS가 무력화되어도, 노출된 데이터는 암호화를 유지하여 해커가 읽을 수 없도록 하는 것이 일반적인 추세임.



---

[참고 1] : <https://brunch.co.kr/@sangjinkang/38>

[참고 2] : <https://gyoogle.dev/blog/computer-science/network/TLS%20HandShake.html>
