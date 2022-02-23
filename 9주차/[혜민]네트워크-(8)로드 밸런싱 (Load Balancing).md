# 9주차 CS 발표

## 로드 밸런싱 (Load Balancing)

### 로드 밸런싱이란?

부하분산 또는 로드 밸런싱은 컴퓨터 네트워크 기술의 일종으로, 중앙처리장치 혹은 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미함.

서버에게 가해지는 부하(=로드)를 분산(=밸런싱)해주는 장치 또는 기술.

사업의 규모가 확장되고, 클라이언트의 수가 늘어나게 되면 기존 서버만으로는 정상적인 서비스가 불가능하게 되는데, 이런 증가한 트래픽에 대처할 수 있는 방법은 크게 두가지임.

- Scale up : 서버 자체의 성능을 높이는 것
- Scale out : 여러 대의 서버를 두는 것</br>
  ⇒ Scale out 방식은 여러대의 서버로 트래픽을 균등하게 분산해주는 로드밸런싱이 반드시 필요함.

로드밸런싱은 로드 밸런서들 클라이언트와 서버풀(Server Pool, 분산 네트워크를 구성하는 서버들의 그룹) 사이에 두고, 부하가 일어나지 않도록 여러 서버에 분산시켜주는 방식. 



### 로드밸런싱 알고리즘

로드 밸런서가 서버를 선택하는 방식

- 라운드 로빈 (Round Robin)
  - CPU 스케쥴링의 라운드로빈 방식 활용
  - 서버에 들어온 요청을 순서대로 돌아가며 배정하는 방식
  - 서버와의 연결이 오래 지속되지 않는 경우 적합함
- 가중 라운드로빈 방식 (Weighted Round Robin)
  - 각 서버에 가중치를 매기고 가중치가 높은 서버에 요청을 우선적으로 배정하는 방식
  - 서버의 트래픽 처리 능력이 다른 경우 사용함
- 최소 연결 방식(Least Connections)
  - 요청이 들어온 시점에 가장 적은 연결 상태를 보이는 서버에 트래픽을 배정하는 방식
  - 서버에 분배된 트래픽들이 일정하지 않은 경우에 적합함
  - 트래픽으로 인해 세션이 길어지는 경우 권장
- IP 해시 방식(IP Hash)
  - 클라이언트의 IP 주소를 특정 서버로 매핑하여 요청을 처리하는 방식
  - 특정 사용자가 항상 동일한 서버로 연결되는것을 보장



### L4 로드밸런서와 L7 로드밸런서

부하 분산에는 L4 로드밸런서와   L7 로드밸런서가 가장 많이 활용됨. </br>
그 이유는 L4 로드밸런서부터 포트 정보를 바탕으로 로드를 분산하는 것이 가능하기 때문. </br>
한대의 서버에 각기 다른 포트를 부여하여 다수의 서버 프로그램을 운ㅇ영하는 경우라면 최소 L4 로드밸런서 이상을 사용해야함

#### L4 로드 밸런싱

<img src="https://post-phinf.pstatic.net/MjAxOTEyMTBfNCAg/MDAxNTc1OTU1MzY3OTM2.nG91HOEOh6Sc1AuUgbN3O4pcnEI-rh24UKSrrrjkrcsg.VcG18MidW4az7Oh0RQfRPLDBHNRyGayE1BsQxDImL3Ig.JPEG/L4-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200" alt="img" style="zoom:50%;" />


- 네트워크 계층(IP, IPX)나 트랜스포트 계층(TCP, UDP)의 정보를 바탕으로 로드를 분산함
- IP 주소나 포트번호, MAC주소, 전송 프로토콜에 따라 트래픽을 나누는 것이 가능
- 데이터 안을 보지 않고 패킷 레벨에서만 로드를 분산하기때문에 속도가 빠르고 효율이 높음
- 섬세한 라우팅이 불가능하지만 L7 로드 밸런서보다 저렴함



#### L7 로드 밸런싱

<img src="https://post-phinf.pstatic.net/MjAxOTEyMTBfMjA1/MDAxNTc1OTU1MzgxODY5.odnG4CRES0e5bH7sOKyWRP1c8uO_XC4VX9A3HPeI1JQg.lNL2eJYbMz6NX1e5YFzfHDMQHn4YrdOJR2VYHmq5e1Ig.JPEG/L7-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1.jpg?type=w1200" alt="img" style="zoom:50%;" />

- 애플리케이션 계층(HTTP. FTP, SMTP)에서 로드를 분산함
- HTTP 헤더, 쿠키 등과 같은 사용자 요청을 기준으로 특정 서버에 트래픽을 분산하는 것이 가능</br>
  즉, 패킷 내용을 확인하고 그 내용에 따라 로드를 특정 서버에 분배하는 것이 가능
- URL에 따라 부하를 분산시키거나, HTTP 헤더의 쿠키 값에 따라 부하를 분산하는 등 클라이언트의 요청을 보다 세분화해 서버에 전달할 수 있음 
- 특정한 패턴을 지닌 바이러스를 감지해 네트워크를 보호할 수 있으며, Dos/DDos와 같은 비정상적인 트래픽을 필터링 할 수 있음 → 네트워크 보안 분야에서도 활용되고 있음
- 패킷의 내용을 복호화해야하기 때문에 더 많은 비용이 듦



![img](https://post-phinf.pstatic.net/MjAxOTEyMTBfMTUx/MDAxNTc1OTU2NzEwMzMy.SekjHws4oCgNmCkjYoiZg_pfAlBu2yC66wPkLq0JHbsg.Zn9aLJYZX7rbdEL-X4HRkVO4PCgDNanhQntuR-iGBwkg.PNG/%EC%9B%B9_1920_%E2%80%93_1.png?type=w1200)





---

[참고 1] : <https://velog.io/@jisoo1170/Load-Balancing%EC%9D%B4%EB%9E%80>

[참고 2] : <https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/%EB%A1%9C%EB%93%9C%20%EB%B0%B8%EB%9F%B0%EC%8B%B1(Load%20Balancing).md>

[참고 3] : <https://post.naver.com/viewer/postView.nhn?volumeNo=27046347&memberNo=2521903>
