# 14주차 CS 발표

## 트랜잭션(Transaction)

### 트랜잭션이란?

데이터베이스의 상태를 변화시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미

> ex)
>
> A라는 사람이 10,000원이라는 돈을 B에게 보낸다고하면, A의 잔고 10,000원 차감이란 DB작업과 B의 잔고 10,000원 증가라는 작업이 이루어짐. 
>
> 이 두쿼리 작업을 하나의 트랜잭션으로 볼 수 있음



### 트랜잭션의 특징

1. 데이터베이스 시스템에서 병행 제어 및 회복 작업 시 처리되는 작업의 논리적 단위
2. 사용자가 시스템에 대한 서비스 요구 시 시스템이 응답하기 위한 상태 변환 과정의 작업 단위
3. 하나의 트랜잭션은  Commit 되거나 Rollback 됨



### 트랜잭션의 성질

#### 원자성 (Atomicity)

- 분해가 불가능한 작업의 최소 단위
- 트랜잭션의 연산은 데이터베이스에 모두 반영되든지 아니면 전혀 반영되지 않아야 함
- 트랜잭션 내의 모든 명령은 반드시 완벽히 수행되어야 하며, 모두가 완벽히 수행되지 않고 어느 하나라도 오류가 발생하면 트랜잭션 전부가 취소되어야 함
- 주요 기법: Commit/Rollback, 회복성 보장

#### 일관성 (Consistency)

- 트랜잭션이 그 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 변환함
- 시스템이 가지고 있는 고정 요소는 트랜잭션 수행 전과 트랜잭션 수행 완료 후의 상태가 같아야 함
- 주요 기법: 무결성 제약조건, 병행 제어

#### 독립성, 격리성 (Isolation)

- 둘 이상의 트랜잭션이 동시에 병행 실행되는 경우, 어느 하나의 트랜잭션 실행 중에 다른 트랜잭션의 연산이 끼어들 수 없음
- 수행중인 트랜잭션은 완전히 완료될 때까지 다른 트랜잭션에서 수행 결과를 참조할 수 없음
- 주요 기법: 데이터베이스 고립화 수준(Read Uncommited, Read Committed, Repeatable Read, Serializable)

#### 영속성, 지속성 (Durability)

- 성공적으로 완료된 트랜잭션의 결과는 시스템이 고장나더라도 영구적으로 반영되어야 함
- 주요 기법: 회복 기법



### 트랜잭션 연산

#### Commit 연산

- 한개의 논리적 단위(트랜잭션)에 대한 작업이 성공적으로 끝났고 데이터베이스가 다시 일관된 상태에 있을 때, 이 트랜잭션이 행한 갱신 연산이 완료된 것을 트랜잭션 관리자에게 알려주는 연산

#### Rollback 연산

- 하나의 트랜잭션 처리가 비정상적으로 종료되어 데이터베이스의 일관성을 깨트렸을 때, 이 트랜잭션의 일부가 정상적으로 처리되었더라도 트랜잭션의 원자성을 구현하기 위해 이 트랜잭션이 행한 모든 연산을 취소(undo)하는 연산
- Rollback 시에는 해당 트랜잭션을 재시작하거나 폐기함



### 트랜잭션의 상태 변화

<img src="https://media.vlpt.us/images/jinho0705/post/6a8ec4c2-329d-42e4-9bbf-122f59d4df12/transaction-status.png" alt="img" style="zoom:50%;" />

- 활동 상태 (Active)
  - 초기 상태, 트랜잭션이 실행 중일 때 가지는 상태
- 부분 완료 상태 (Partially Committed)
  - 마지막 명령문이 실행된 후에 가지는 상태
- 완료 상태 (Committed)
  - 트랜잭션이 성공적으로 완료된 후 가지는 상태
- 실패 상태 (Failed)
  - 정상적인 실행이 더 이상 진행될 수 없을 때 가지는 상태
- 철회 상태 (Aborted)
  - 트랜잭션이 취소되고 데이터베이스가 트랜잭션 시작 전 상태로 환원된 상태



### 트랜잭션 제어

트랜잭션 제어언어는 TCL(Transcation Control Language)라고 하며, 트랜잭션의 결과를 허용하거나 취소하는 목적으로 사용되는 언어를 지칭함

| 명령어                   | 핵심           | 설명                                           |
| ------------------------ | -------------- | ---------------------------------------------- |
| 커밋 (Commit)            | 트랜잭션 확정  | 트랜잭션을 메모리에 영구적으로 저장하는 명령어 |
| 롤백 (Rollback)          | 트랜잭션 취소  | 트랜잭션 내역을 저장 무효화시키는 명령어       |
| 체크포인트 (Check point) | 저장 시기 설정 | Rollback을 위한 시점을 지정하는 명령어         |



### 트랜잭션 관리를 위한 DBMS 전략

> 이해를 위한 2가지 개념 : DBMS 구조 / Buffer 관리 정책

#### DBMS 구성



![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile1.uf.tistory.com%2Fimage%2F256646455886BEC51364A6)

- 질의 처리기 (Query Processor) : 상부 구성

- 저장 시스템(Storage System) : 하부 구성. MySQL의 경우 InnoDB/MyISM 등과 같이 여러 하부 저장 시스템 선택 가능

  > MySQL: 상부의 질의 처리기와 하부의 저장 시스템이 명확하게 구분되는 계층 구조에 해당

#### 트랜잭션이 종료되는 경우

- 쿼리가 정상적으로 수행되어 commit을 통해 종료되는 경우
- 잘못된 입력 or 일관성 제약 조건 위배 등 사용자 요청에 의해 철회되는 경우
- time out or 교착상태 등 시스템이 감지하는 문제로 DBMS가 철회되는 경우

DB 시스템은 비휘발성 저장 장치인 디스크에 데이터를 저장하며, 전체 DB의 일부분을 메인 메모리에 유치시킴

DBMS는 데이터를 고정 길이의 페이지로 저장하며 디스크에서 읽거나 쓸 때 페이지 단위로 입출력이 이루어짐

⇒ 메인 메모리에 유치되는 페이지들을 관리하는 모듈을 페이지 버퍼 (Page buffer) 또는 버퍼 관리자(Buffer Manager)라 부르는데, DBMS의 주요 모듈 중 하나임

> Buffer 관리 정책에 따라, UNDO 복구와 REDO 복구가 요구되거나 그렇지 않게 되므로, transaction 관리에 매우 중요한 결정을 가져옴

#### 트랜잭션 복구 시 버퍼 관리 정책

##### UNDO 복구

- 해당 트랜잭션이 어떤 이유든 정상적으로 종료할 수 없게 되면 트랜잭션이 변경했던 페이지들이 그 이전 상태로 원상 복구 되는 것
- STEAL / ¬STEAL(No-steal) 2가지가 존재

##### REDO 복구

- UNDO 복구의 반대
- 커밋한 트랜잭션 수정은 어떤 경우에도 유지되어야 함
- 이미 commit한 트랜잭션의 수정을 재반영하는 복구 작업
- FORCE / ¬FORCE(No-force) 2가지가 존재함

> DBMS는 버퍼관리 정책으로
>
> - UNDO 복구의 STEAL : 수정된 페이지를 언제든지 디스크에 쓸 수 있음
> - REDO 복구의 ¬FORCE : 수정했던 페이지를 트랜잭션 commit 시점에서 디스크에 반영하지 않음
>
> 두 개를 채택하고 있어 UNDO + REDO 복구 두 가지가 모두 다 필요함

#### 로그 (Log)

>  UNDO 복구와 REDO 복구를 사용하기 위해 가장 널리 쓰이는 구조
>
> 로그는 로그 레코드의 모음으로 이루어짐. DB의 모든 갱신 작업을 기록. 이론적으로는 가장 안정적인 저장 매체에 기록됨

로그는 덧붙이는(append) 방식으로 기록되며, 각 로그 레코드는 고유 식별자를 가짐

항상 뒤에 덧붙이는 방식으로 쓰이기 때문에 로그 식별자는 단조 증가함

- 레코드 고유 식별자 : LSN (Log Sequence Number) or LSA(Log Sequence Address)로 불림

기록할 Object 타입에 따라 물리적/논리적 로깅으로 분류됨

- 물리적 상태 로깅(Physical State Logging) 
  - DBMS에서 가장 널리 쓰이는 기본적인 로깅 방법
- 로그 버퍼 
  - DBMS가 로그 레코드 관리를 위해 유지하는 별도의 버퍼. 
  - 성능을 위해 로그 버퍼에 로그 레코드를 모았다가, block 단위로 로그 파일에 출력함
- 더티 페이지(Dirty Page)
  - 데이터 캐시에서는 변경되었지만 데이터 파일(*.mdf)에서는 아직 변경되지 않은 데이터(페이지)
  - 더티 페이지가 데이터 파일에 성공적으로 적용된 후 체크포인트가 설정 됨



---

[참고 1] : <https://coding-factory.tistory.com/226>

[참고 2] : <https://velog.io/@jinho0705/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98Transaction>

[참고 3] : <https://diaryofgreen.tistory.com/36>

[참고 4] : <https://gyoogle.dev/blog/computer-science/data-base/Transaction.html>
