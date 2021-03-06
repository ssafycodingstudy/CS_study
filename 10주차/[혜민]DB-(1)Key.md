# 10주차 CS 발표

## DB Key 정리

Key는 데이터베이스에서 조건에 만족하는 튜플을 찾거나 순서대로 정렬할 때 다른 튜플과 구별할 수 있는 유일한 기준이 되는 Attribute(속성)



### 1. 후보키 (Candidate Key)

- 릴레이션을 구성하는 속성들 중에서 튜플을 **유일하게 식별할 수 있는 속성들의 부분집합** (기본키로 사용할 수 있는 속성들)
- **모든 릴레이션은 반드시 하나 이상의 후보키를 가져야함**
- 릴레이션에 있는 모든 튜플에 대해서 **유일성과 최소성을 만족** 시켜야함
  - 유일성 : key로 하나의 튜플을 유일하게 식별할 수 있음
  - 최소성 : 꼭 필요한 속성으로만 구성



### 2. 기본키 (Primary Key)

- 후보키 중 선택한 Main Key
- 한 릴레이션에서 **특정 튜플을 유일하게 구별할 수 있는 속성**
- **null 값을 가질  수 없음** (개체 무결성의 첫번째 조건)
- 기본키로 정의된 속성에는 **동일한 값이 중복되어 저장될 수 없음** (개체 무결성의 두번째 조건)



### 3. 대체키 (Alternate Key)

- **후보키가 둘 이상일 때 기본키를 제외한 나머지 후보키들**
- 보조키라고도 함



### 4. 슈퍼키 (Super Key)

- **한 릴레이션 내에 있는 속성들의 집합으로 구성된 키**
- 릴레이션을 구성하는 모든 튜플 중 슈퍼키로 구성된 속성의 집합과 동일한 값은 나타내지 않음
- 릴레이션을 구성하는 모든 퓨틀에 대해 **유일성은 만족하지만, 최소성은 만족시키지 못하는 키**



### 5. 외래키 (Foreign Key)

- 관계(relation)을 맺고 있는 릴레이션 R1, R2에서 릴레이션 R1이 참조하고 있는 릴레이션 R2의 기본키와 같은 R1 릴레이션의 속성
- **참조되는 릴레이션의 기본키와 대응되어 릴레이션 간의 참조 관계를 표현하는데 중요한 도구**로 사용됨
- **외래키로 지정되면 참조 테이블의 기본키에 없는 값은 입력할 수 없음** (참조 무결성 조건)



<img src="https://user-images.githubusercontent.com/26523814/156388419-d6a7ba1c-d71b-4b69-b0ca-f071ec42a45d.png" alt="image-20220302235859312" style="zoom:33%;" /><img src="https://user-images.githubusercontent.com/26523814/156388747-0c4e4a7a-5346-4e30-813f-46fdcab220f7.png" alt="image-20220302235917132" style="zoom:50%;" />

- 후보키 : <학생> 릴레이션에서 학번, 주민번호
- 기본키 : <학생> 릴레이션에서 학번 or 주민번호 / <수강> 릴레이션에는 '학번+과목명'
- 대체키 :  <학생> 릴레이션에서 '학번'을 기본키로 정의하면 '주민번호'는 대체키가 됨
- 슈퍼키 : <학생> 릴레이션에서는 '학번', '주민번호', '학번'+'주민번호', '학번'+'주민번호'+'성명' 등

- 외래키 :  <수강> 릴레이션이 <학생> 릴레이션을 참조하고 있으므로 <학생> 릴레이션의 '학번'은 기본키이고, <수강> 릴레이션의 '학번'은 외래키

---

[참고 1] : <https://limkydev.tistory.com/108>

[참고 2] : <https://gyoogle.dev/blog/computer-science/data-base/Key.html>

