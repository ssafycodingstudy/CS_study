# 19주차 CS 발표

## 마이크로 서비스 아키텍처(MSA)

### MSA란?

MicroService Architecture의 줄임말

마이크로서비스 아키텍처에 대한 정확한 정의는 없음

하지만 `마이크로서비스`란 작고, 독립적으로 배포 가능한 각각의 기능을 수행하는 서비스로 구성된 프레임워크라고 할 수 있음

마이크로서비스는 완전히 독립적으로 배포가 가능하고, 다른 기술 스택(개발 언어, 데이터베이스 등)이 사용가능한 단인 사업 영역에 초점을 둠



### MSA 등장 배경

![img](https://blog.kakaocdn.net/dn/cTAT6G/btqD2bvNaYo/42aEDP4R01hfklHBQBZefk/img.png)



>  **Monolithic Architecture**는 소프트웨어의 모든 구성 요소가 한 프로젝트에 통합되어있는 형태

웹 개발을 예로 들면 웹 프로그램을 개발하기 위해 모듈별로 개발하고, 개발이 완료된 웹 어플리케이션을 하나의 결과물로 패키징하여 배포되는 형태를 말함

이런 어플리케이션을 모놀리식 어플리케이션이라 하며, 웹의 경우 WAR파일로 빌드되어 WAS에 배포하는 형태를 말함. 주로 **소규모 프로젝트에서 사용됨**

하지만 일정 규모 이상의 서비스, 혹은 수백명 이상의 개발자가 투입되는 프로젝트에서 Monolithic Architecture는 한계를 보임

- 부분 장애가 전체 서비스의 장애로 확대될 수 있음
- 부분적인 Scale-out (여러 server로 나누어 일을 처리하는 방식)이 어려움
- 서비스의 변경이 어렵고, 수정 시 장애의 영향도 파악이 힘듦
- 배포 시간이 오래걸림
- 한 프레임워크와 언어에 종속적임

이러한 모놀리식 아키텍쳐의 문제점들을 보완하기 위해 MSA가 등장함

기존의 특정한 물리적 서버에 서비스를 올리던 on-promise 서버 기반의 모놀리식 아키텍쳐에서 이제는 클라우드 환경을 이용하여 서버를 구성하는 MicroService Architectue로 많은 서비스들이 전환되고 있음



### MSA의 특징

MSA는 API를 통해서만 상호작용할 수 있음. 즉, 마이크로 서비스는 서비스의 end-point(접근점)을 API 형태로 외부에 노출하고, 실질적인 세부 사항은 모두 추상화함.

내부의 구현 로직, 아키텍처와 프로그래밍 언어, 데이터베이스, 품질 유지 체계와 같은 기술적인 사항들은 서비스 API에 의해 철저하게 가려짐

따라서 **SOA(Service Oriented Architecture)** 의 특징을 다수 공통으로 가짐

- 제대로 설계 된 마이크로 서비스는 하나의 비즈니스 범위에 맞춰 만들어지므로 하나의 기능만 수행함. 즉, 어플리케이션 출시처럼 하나의 목표를 향해 일하지만 자기가 개발하는 서비스만 책임짐. 그리고 여러 어플리케이션에서 재사용할 수 있어야함
- 어플리케이션은 항상 기술 중립적 프로토콜을 사용해 통신하므로 서비스 구현 기술과는 무관함. 따라서 마이크로서비스 기반의 어플리케이션을 다양한 언어와 기술로 구축할 수 있다는 것을 의미함
- 마이크로서비스는 SOA에서 사용되는 집중화된 관리 체계를 사용하지 않음. 마이크로서비스 구현체의 공통적인 특징 중 하나는 ESB(Enterprise Service Bus)와 같은 무거운 제품에 의존하지 않는다는 점. REST 등 가벼운 통신 아키텍처, 또는 Kafka 등을 이용한 message stream을 주로 사용함



### MSA의 장점

- 각각의 서비스는 모듈화가 되어있으며 이러한 모듈끼리는 RPC 또는 message-driven API 등을 이용하여 통신함. 이러한 MSA는 각각 개별의 서비스 개발을 빠르게 하며, 유지보수도 쉽게할 수 있도록 함
- 팀단위로 적절한 수준에서 기술 스택을 다르게 가져갈 수 있음. 회사가 Java의 Spring 기반이라도 MSA를 적용하면 node.js로 블록체인 개발 모듈을 연동함에 무리가 없음
- 마이크로 서비스는 서비스별로 독립적 배포가 가능함. 따라서 지속적인 배포 CD도 모놀리식에 비해서 가볍게 할 수 있음
- 마이크로 서비스는 각각 서비스의 부하게 따라 개별적으로 scale-out이 가능함. 메모리, CPU적으로 상당 부분 이득이 됨



### MSA의 단점

- MSA는 모놀리식에 비해 상대적으로 많이 복잡함. 서비스가 모두 분산되어있기 때문에 개발자는 내부 시스템의 통신을 어떻게 가져가야 할 지 정해야함. 또한, 통신의 장애와 서버의 부하 등이 있을 경우에 어떻게 transaction을 유지할지 결정하고 구현해야함
- 모놀리식에서는 단일 트랜잭션을 유지하면 됐지만 MSA에서는 비즈니스에 대한 DB를 가지고 있는 서비스도 각기 다르고, 서비스의 연결을 위해서는 통신이 포함되기 때문에 트랜잭션을 유지하는게 어려움
- 통합 테스트가 어려움. 개발 환경과 실제 운영 환경을 동일하게 가져가는 것이 쉽지 않음
- 실제 운영 환경에 대해서 배포하는 것이 쉽지 않음. 마이크로서비스의 경우 서비스 1개를 재배포한다고 하면 다른 서비스들과의 연계가 정상적으로 이루어지고 있는지도 확인해야함



---

[참고 1] : <https://wooaoe.tistory.com/57>

