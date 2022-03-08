# 11주차 CS 발표

## DB Join

### Join

둘 이상의 테이블을 연결해서 데이터를 검색하는 방법

연결하려면 테이블들이 적어도 하나의 컬럼을 공유하고 있어야함

이 공유하고 있는 컬럼을 PK 또는 FK값으로 사용



### Join의 종류

<img src="https://user-images.githubusercontent.com/26523814/156868686-ed8b619d-5dd7-488b-bdff-904a5d6a18f0.png" alt="스크린샷 2022-03-05 오후 1 55 58" style="zoom:100%;" />

- INNER JOIN : 내부 조인 → 교집합
- LEFT/RIGHT (OUTER) JOIN→ 부분집합
- FULL OUTER JOIN : 외부 조인 → 합집합
  - 오라클은 full outer join이 있지만 mysql에서는 지원하지 않기 때문에 left join과 right join을 union하여 full outer join을 사용
- CROSS JOIN
- SELF JOIN



A 테이블

| ID   | ENAME |
| ---- | ----- |
| 1    | AAA   |
| 2    | BBB   |
| 3    | CCC   |

B 테이블

| ID   | KNAME |
| ---- | ----- |
| 1    | 가    |
| 2    | 나    |
| 4    | 라    |
| 5    | 마    |



#### INNER JOIN

교집합, 공통적인 부분만 select

<img src="https://camo.githubusercontent.com/a8fc07a00af9d97c2898104cb7881a0519983ee570fdb711aed5dd6ee318b016/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65392e75662e746973746f72792e636f6d253246696d61676525324639393739394633453541383134384437303336363539" alt="img" style="zoom:33%;" />

```mysql
SELECT A.ID, A.ENAME, B.KNAME
FROM A INNER JOIN B
ON A.ID = B.ID;		
```

| ID   | ENAME | KNAME |
| ---- | ----- | ----- |
| 1    | AAA   | 가    |
| 2    | BBB   | 나    |



#### LEFT OUTER JOIN 

조인 기준 **왼쪽에 있는 것 <u>다</u>** select (공통적인 부분 + LEFT에 있는 것)

<img src="https://camo.githubusercontent.com/c76a34d9927d99d7def46c2839694677d160586ca2af3eff32d98fa2ae969568/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c65362e75662e746973746f72792e636f6d253246696d61676525324639393745374634313541383134393035303746303237" alt="img" style="zoom:33%;" />

```mysql
SELECT A.ID, A.ENAME, B.KNAME
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID;   
```

| ID   | ENAME | KNAME    |
| ---- | ----- | -------- |
| 1    | AAA   | 가       |
| 2    | BBB   | 나       |
| 3    | CCC   | **NULL** |

##### 차집합 표현 (A-B)

> LEFT가 갖고있는 것 중 공통적인 부분을 제외한 값

<img src="https://user-images.githubusercontent.com/26523814/156869235-40c3e540-087e-45f7-ba46-b2e680345b02.png" alt="image" style="zoom: 50%;" />

```mysql
SELECT A.ID, A.ENAME, B.KNAME
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID
WHERE B.KNAME IS NULL;   
```

| ID   | ENAME | KNAME |
| ---- | ----- | ----- |
| 3    | CCC   | NULL  |



#### RIGHT OUTER JOIN

조인 기준 **오른쪽에 있는 것 <u>다</u>** select (공통적인 부분 + RIGHT에 있는 것)

<img src="https://camo.githubusercontent.com/371a3f188280420a933172a212f74285204b85837603ae3cb973c77eb66be74d/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532352e75662e746973746f72792e636f6d253246696d61676525324639393834434533353541383134393138304142443144" alt="img" style="zoom:33%;" />

```mysql
SELECT A.ID, A.ENAME, B.KNAME
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID;   
```

| ID   | ENAME    | KNAME |
| ---- | -------- | ----- |
| 1    | AAA      | 가    |
| 2    | BBB      | 나    |
| 4    | **NULL** | 라    |
| 5    | **NULL** | 마    |

##### 차집합 표현 (B-A)

> RIGHT가 갖고있는 것 중 공통적인 부분을 제외한 값

<img src="https://user-images.githubusercontent.com/26523814/157267175-153e00b3-6049-43af-93c8-547a6ac0acfe.png" alt="image" style="zoom:50%;" />

```mysql
SELECT A.ID, A.ENAME, B.KNAME
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID
WHERE A.ENAME IS NULL;   
```

| ID   | ENAME    | KNAME |
| ---- | -------- | ----- |
| 4    | **NULL** | 라    |
| 5    | **NULL** | 마    |



#### FULL OUTER JOIN

A테이블, B테이블 모두 select (합집합)

<img src="https://camo.githubusercontent.com/8b69d9df60427a56c5ffd62ad4d9468150dc645331e15ce27ad22e09c71d09bb/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532342e75662e746973746f72792e636f6d253246696d61676525324639393139354633343541383134393339314245304333" alt="img" style="zoom:33%;" />

```mysql
/* oracle */
SELECT A.ID, A.ENAME, B.KNAME
FROM A FULL OUTER JOIN B
ON A.ID = B.ID

/* mysql */
SELECT A.ID, A.ENAME, B.KNAME
FROM A LEFT JOIN B
ON A.ID = B.ID
UNION
SELECT A.ID, A.ENAME, B.KNAME
FROM A RIGHT JOIN B
ON A.ID = B.ID;
```

| ID   | ENAME | KNAME |
| ---- | ----- | ----- |
| 1    | AAA   | 가    |
| 2    | BBB   | 나    |
| 3    | CCC   | NULL  |
| 4    | NULL  | 라    |
| 5    | NULL  | 마    |



#### CROSS JOIN

모든 경우의 수를 전부 표현해주는 방식

A가 3개, B가 4개면 총 3*4 = 12개의 데이터가 검색

<img src="https://camo.githubusercontent.com/c8170fd119eac82de056d7b1659824b1d398627fa09cccb70553becd4906d146/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6531302e75662e746973746f72792e636f6d253246696d61676525324639393346344534343541384132443238314143363642" alt="img" style="zoom:50%;" />



```mysql
SELECT * FROM A CROSS JOIN B;
SELECT * FROM A JOIN B;
SELECT * FROM A, B;
// 아래 두 query는 on 조건절 없이 사용하면 cross join의 결과값이 도출
```



#### SELF JOIN 

자기 자신과 자기 자신을 조인

하나의 테이블을 여러번 복사해서 조인한다고 생각

자신이 갖고 있는 컬럼을 다양하게 변형시켜 활용할 때 자주 사용

보통 자기 자신과의 조합은 제거

<img src="https://camo.githubusercontent.com/3600303a038c6cc6f6189738e96de0f791673b542f84c1895afa9b32a4fb6208/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d687474702533412532462532466366696c6532352e75662e746973746f72792e636f6d253246696d61676525324639393334314433333541384133363344303631344538" alt="img" style="zoom:50%;" />



```mysql
SELECT *
FROM A T1, A T2
WHERE T1.ENAME <> T2.ENAME //자기 자신과의 조합 제거, <> 는 != 이라는 뜻
```

##### SELF JOIN 활용

selft join은 한 테이블 내의 row끼리 어떤 계산을 할 때 유용하게 쓰임

> 도시의 위,경도를 활용해 거리를 계산하는 예제
>
> ```mysql
> SELECT a.city as city1, b.city as city2, ROUND(SQRT(POW(69.1 * (b.latitude-a.latitude),2) + POW(69.1 * (b.longitude-a.longitude) * COS(a.latitude/57.3),2)) * 1.6, 0) as distance  
> FROM cities a, cities b 
> WHERE a.city <> b.city;›
> ```
>
> ![image](https://user-images.githubusercontent.com/26523814/156870810-fae4a13b-7097-469e-ba96-efd6494dbb45.png)
>
> 여기서 문제점은 (광주,서울)이나 (서울,광주)는 순서만 반대이고 같은 계산이기 때문에 불필요함
>
> 이를 제거하는 방법은 처음 query와는 다르게 WHERE 구문에 <> 대신 **부등호** 를 사용함.
>
> SQL에서 텍스트 값이 비교가 가능하기 때문에 반대 순서는 출력되지 않음
>
> ```mysql
> SELECT a.city as city1, b.city as city2, ROUND(SQRT(POW(69.1 * (b.latitude-a.latitude),2) + POW(69.1 * (b.longitude-a.longitude) * COS(a.latitude/57.3),2)) * 1.6, 0) as distance  
> FROM cities a, cities b 
> WHERE a.city > b.city;
> ```
>
> ![image](https://user-images.githubusercontent.com/26523814/156870907-67096f6a-d76e-4870-b538-dfe548964c9e.png)
>
> 서울은 사전 방식(lexicographically)으로 광주보다 큰 값이기 때문에 (광주, 서울)은 출력되지 않음

---

[참고 1] : <https://pearlluck.tistory.com/46>

[참고 2] : <https://wkdgusdn3.tistory.com/entry/mysql%EC%97%90%EC%84%9C-full-outer-join-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0>

[참고 3] : <https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/%5BDatabase%20SQL%5D%20JOIN.md>

[참고 4] : <https://yahwang.github.io/posts/33>
