# 14주차 CS 발표

## 레디스(Redis)

### 레디스(Redis) 개념

레디스(Redis)는 `Remote Dictionary Server`의 약자로 오픈소스(BSD licensed) DBMS임

In-memory(인메모리) 데이터 저장소이며, Key-Value 기반의 NoSQL DBMS임

> **In-memory DB**
>
> disk-based DB와 달리 **메모리에 데이터를 저장**
>
> 외부 저장 장치에 데이터를 저장하지 않고 메모리에서 데이터를 읽고 씀
>
> 메모리 ↔ 디스크 간 병목이 없기 때문에 disk-based DB보다 훨신 속도가 빠름
>
> In-memory DB는 **기본적으로** 영속성(persistence)을 보장하지 않음
>
> 에러가 나서 갑자기 프로세스가 종료된다거나 하면, 데이터가 모두 유실될 수도 있음
>
> 또한 메모리에 데이터를 저장하기 때문에 저장 공간이 한정되어있음

보통 DB, Cache, 메시지 브로커 등의 용도로 사용함

Redis는 모든 데이터가 메모리에 저장되지만, 서버가 내려갔다가 올라가는 상황에서 메모리에 상주한 데이터들이 휘발되지 않도록 하기 위해 백업 과정을 거침



### Redis의 특징

#### 빠른 데이터 접근 속도

메모리에 저장하기때문에 디스크에 저장하는 데이터베이스보다 더 빠른속도로 데이터에 접근이 가능함

<img src="https://media.vlpt.us/images/mu1616/post/86a65830-72ba-4759-9daf-5bc00ace9316/image.png" alt="img" style="zoom:50%;" />

#### 다양한 자료구조 지원

Redis가 다른 In-Memory 데이터베이스와 가장 큰 차이점은 다양한 자료구조를 지원한다는 것

Redis는 아래의 자료구조들을 Key-Value 형태로 저장함

> **대표적인 5가지**
>
> 1. String 
>
>    일반적인 문자열을 저장하며, 키에 넣을 수 있는 데이터의 최대 크기는 512 MB
>
>    텍스트 뿐만 아니라 정수형이나 JPG같은 바이너리 데이터까지 저장할 수 있음
>
> 2. Set
>
>    문자열의 집합 (반복되지 않고 정렬되지 않은 요소의 집합) 
>
>    하나의 key에 N개의 중복되지 않은 데이터 값을 가짐
>
>    key에 속한 데이터들은 정렬되지 않은 그룹임
>
> 3. Sorted Set
>
>    문자열의 정렬된 집합 (스코어라고 불리는 부동소수점의 지시를 받는 반복되지 앟은 요소의 집합)
>
>    Set의 자료구조 형태에 score라는 필드가 추가된 데이터 타입 (score는 일종의 가중치)
>
>    Sorted Set 내에서 score를 기준으로 내부 정렬이 됨
>
> 4. Hashes
>
>    key와 value가 String인 Hash, field(hashes-key)와 value 쌍으로 이루어진 테이블을 저장하는 자료구조
>
> 5. List
>
>    하나의 key에 N개의 데이터를 가지며, 데이터를 중복하여 저장할 수 있음
>
>    양방향 Linked List처럼 List의 앞,뒤에 데이터를  push / pop 할 수 있고, index 값을 이용하여 지정된 위치에 데이터를 넣거나 뺄 수도 있음

따라서 다양한 방식으로 데이터를 활용할 수 있음 `ex) sortedSet을 사용하여 랭킹을 저장할 수 있음`

<img src="https://media.vlpt.us/images/mu1616/post/6610f000-c432-404b-855f-417c04bd39dc/image.png" alt="img" style="zoom:50%;" />

#### 데이터 영속화

Redis는 디스크에 저장하여 영속화 시키는 기능을 갖고 있기 때문에 서버가 내려갔다가 올라간 후에도 데이터가 유실되지 않음

데이터를 저장하는 방법은 `Snapshot` 과 `AOP(Append Only File)`가 있음

- Snapshot
  - 메모리에 있는 데이터들을 디스크에 옮겨 담는 방식
  - 메모리의 상태를 그대로 뜬 것이기 때문에 특정 시점의 백업 및 복구에 유리하고, AOF 방식에 비해 더 빠르게 데이터를 메모리에 올릴 수 있음
  - 만약 Redis를 재기동하면 이 파일부터 데이터를 불러와 복원시킴 
  - 하지만 서버가 다운되면 백업된 스냅샷 사이에 변경된 데이터들이 유실되는 단점이 있음
  - 저장 파일의 확장자는 보통 `.rdb`로 사용
  - 메모리에 있는 데이터들을 디스크에 옮겨 담는 방식은 다음과 같이 두가지가 있음
    - SAVE : blocking 방식으로 순차적으로 모든 redis의 동작을 정지시키고, 그때의 Snapshot을 Disk에 저장함
    - BGSAVE : non-blocking 방식으로 별도의 process를 통해서 수행 당시의 메모리 Snapshot을 디스크에 저장함(메모리가 현재 사용량의 2배가 될 수 있음).</br>
      non-blocking 방식이므로 redis의 동작이 멈추지 않음
- AOF (Append Only File)
  - redis의 모든 write / update 연산 자체를 모두 log 파일의 형태로 기록하는 방법
  - 서버가 실행되면 순차적으로 연산을 재실행하여 데이터를 복구함
  - 연산작업이 실행될 때마다 기록하기 때문에 현재 시점까지의 로그를 남길 수 있음
  - 로그파일에 대해서 append만 수행하기 때문에 write 속도가 빠르고, 서버가 내려가도 데이터 유실이 발생하지 않음
  - 하지만 모든 연산에 대해서 로그를 남기기 때문에 데이터의 양이 매우 큼
  - 서버를 재시작 시, 모든 연산을 다시 수행하기 때문에 restart 속도가 느림
  - 확장자는 보통 `.aof` 를 사용함

> 결론적으로는 Snapshot과 AOF의 장점들을 모아서 혼용하는 방식으로 사용하는것이 바람직함
>
> 주기적으로 Snapshot으로 백업을 하고, 백업 주기 사이의 간격에는 AOF 방식으로 수행을 함
>
> 이런 방법은 서버가 restart될 때는 Snapshot을 로드하고, 적은 양의 AOF 로그만 수행하기 때문에 restart 수행시간이 빠르고 데이터 유실을 방지할 수 있음



#### 부하 분산

Redis에는 다른 컴퓨터로 복제를 하는 기능이 있어, 갱신이 가능한 1대를 `마스터` 로, 그 복제된 읽기 전용의 복수의 컴퓨터를 `슬레이브` 로 구성하여 분산환경을 만들 수 있음

<img src="https://media.vlpt.us/images/mu1616/post/a590d41d-36c2-4dea-a756-80c44a7ef047/image.png" alt="img" style="zoom:50%;" />



#### 메모리 파편화

메모리를 할당하고 해제하는 과정에서 아래 그림과 같이 메모리 파편화가 발생할 수 있음

따라서 메모리 공간을 할당하지 못해 프로세스가 죽는 문제가 발생할 수 있음

<img src="https://media.vlpt.us/images/mu1616/post/a1f17594-1d86-4f19-b546-a70ab008af1f/image.png" alt="img" style="zoom: 67%;" />



#### 싱글 스레드

Redis는 싱글 스레드로 동작함

따라서 race condition(경쟁 상태)을 피해 데이터의 정합성을 보장하기 쉬움

반면, 처리 시간이 긴 명령어가 들어올 경우 싱글 스레드이기 때문에 다른 명령어들을 처리할 수 없는 상태가 됨

왠만하면 처리 시간이 긴 명령어는 피하고 O(1)로 동작하는 명령을 실행시키는 것을 권고함



### 사용 사례

Redis는 빠른 데이터 접근 속도와 데이터의 consistency(일관성) 를 보장함

따라서 이런 장점을 살릴 수 있는 캐싱과 세션 스토어에 많이 사용됨

그 외에도 Redis의 장점을 살릴 수 있는 곳이라면 어디든지 사용할 수 있음 ex) 한 사용자가 한번만 좋아요를 누를 수 있도록 Redis의 Set을 활용

<img src="https://media.vlpt.us/images/mu1616/post/85c5e1f6-64d9-4c57-8d41-9cac584a4946/image.png" alt="img" style="zoom:50%;" />





---

[참고 1] : <https://velog.io/@mu1616/%EB%A0%88%EB%94%94%EC%8A%A4-Redis%EC%9D%98-%EA%B0%9C%EB%85%90-%ED%8A%B9%EC%A7%95>

[참고 2] : <https://akasai.space/redis/about_redis_1/>

[참고 3] : <https://blog.naver.com/PostView.naver?blogId=dktmrorl&logNo=222512563443&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView>

[참고 4] : <https://2kindsofcs.tistory.com/40>

[참고 5] : <https://gyoogle.dev/blog/computer-science/data-base/Redis.html>
