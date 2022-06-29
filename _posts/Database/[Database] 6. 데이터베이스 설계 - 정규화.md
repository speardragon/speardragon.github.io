

## 1. 정규화의 개념

### 정규화의 개념

- **정규화의 개념**
  - 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화하는 프로세스를 정규화라고 한다 
  - 조금 더 이론적으로 접근해 보면 함수적 종속성을 이용해서 연관성 있는 속성들을 분류하고,  각 릴레이션들에서 이상현상이 생기지 않도록 하는 과정을 말한다.

- **정규화의 목적** 
  - 불필요한 데이터를 제거, 데이터 중복을 최소화 
  - 데이터베이스 구조 확장 시 재 디자인을 최소화 
  - 무결성 제약조건의 시행을 간단하게 하기 위해 
  - 이상 (Anomaly) 현상 을 방지하기 위해 테이블 구성을 논리적이고 직관적으로 만들기 위해 
    - 이상 (Anomaly) 현상 
      - 삽입 이상 (insertion anomaly) : 데이터를 저장할 때 원하지 않는 정보가 함께 삽입되는 경우 
      - 갱신 이상 (update anomaly) : 중복된 튜플 중 일부의 속성만 갱신 시킴 으로써 정보의 모순성이 발생하는 경우 
      - 삭제 이상 (deletion anomaly) : 튜플을 삭제함으로써 유지되어야 하는 정보 까지도 연쇄적으로 삭제되는 경우

<br>

### 이상현상의 종류

- **삽입이상**
  - 데이터를 저장할 때 원하지 않는 정보가 함께 삽입되는 경우
  - ![image](https://user-images.githubusercontent.com/79521972/176439531-5a0fcb6e-a366-4f41-8af3-48c689c4b26b.png)
  - 새로운 교수인 Dr. Newsome이 적어도 하나의 과정을 가르치도록 지정될 때까지 그의 record를 삽입할 수 없다

- **갱신이상**
  - 중복된 튜플 중 일부의 속성만 갱신 시킴 으로써 정보의 모순성이 발생하는 경우
  - ![image](https://user-images.githubusercontent.com/79521972/176439621-7c609545-9e1f-486b-bd15-886da7d8c069.png)
  - Employee 519 두 개의 튜플에서 다른 주소 데이터를 가지고 있다

- **삭제이상**

  - 튜플을 삭제함으로써 유지되어야 하는 정보 까지도 연쇄적으로 삭제되는 경우 
    - ![image](https://user-images.githubusercontent.com/79521972/176439852-4c96b788-0fdc-462c-bdd7-e18950aeb5f2.png)
    - Dr. Giddens 교수가 일시적으로 수업 배정을 중단하게 되면 그에 대한 모든 정보가 손실되게 된다

  - 이런 이상 현상이 발생하는 원인은 관련 없는 속성들을 다 모아서 하나의 릴레이션으로 만들었기 때문이다.  그래서 정규화를 통해 릴레이션을 관련이 있는 속성들 로만 구성되는 작은 여러 개의 릴레이션으로 분해 (decomposition) 해야 한다. 
    - 속성들의 간의 관련성 판단 : 함수적 종속성 (FD : Functional Dependency) 
    - 일반적으로 하나의 릴레이션에는 하나의 함수적 종속성만이 존재하도록 정규화를 하게 된다

<br>

### 함수적 종속성

- 함수적 종속성 (Functional Dependency)
  - 릴레이션(Relation) 의 속성(Attribute)들 사이의 관계를 표현. 주로 기본키 (primary key)와 다른 non key 속성들 사이의 관계를 표현 
  - 함수적 종속성의 분석을 통해 정규화를 실행

![image](https://user-images.githubusercontent.com/79521972/176440011-aa6b06e7-bb1d-4805-9cd1-1a2a4dd4409c.png)



<br>

### Inference Rule

- Inference Rule (추론 규칙)
  - Armstrong’s axioms 을 기본 추론규칙으로 한다 
  - Armstrong’s axioms는 릴레이션에서 함수 종속 관계를 추론해 내는 inference rule들의 set  
  - Functional dependency는 6가지의 추론 규칙을 가진다

![image](https://user-images.githubusercontent.com/79521972/176440171-8c77d722-30c2-4834-a660-765b6441a8a7.png)



<br>

## 2. 정규형의 종류 - 1

### 1NF: First Normal Form

- UNF (Unormalized Form) 

  - 기본키를 가지고 튜플을 식별할 수 있어야 한다 (no duplicate tuples)
  - ![image](https://user-images.githubusercontent.com/79521972/176440298-5284459b-57a5-447f-b6b7-0f37c3bcb00a.png)
  - ISBN# : primary key

- #### 제 1 정규형 (1NF, First Normal Form)

  - 릴레이션에 속한 속성들은 원자값 (Atomic value, 하나의 값)을 갖도록 해야 한다 
  - 예제 
    - Subject 가 여러 값을 가지고 있으므로 다른 릴레이션으로 분리한다 
    - 릴레이션의 기본키가 분리된 릴레이션의 외래키(Foreign key)로 추가한다

![image](https://user-images.githubusercontent.com/79521972/176440490-9fcc0997-e2ca-4284-80c8-7e3dc5ae7123.png)

![image](https://user-images.githubusercontent.com/79521972/176440529-91f27cac-cbd4-402e-a475-63042de32c24.png)

<br>

### 2NF: Second Normal Form

![image](https://user-images.githubusercontent.com/79521972/176440690-189012d6-ce07-4b6d-943c-71ac8f600da7.png)

- 기본키 : {Title, Format} 
- 함수적 종속성 
  - {Title} -> {Author, Author Nationality, Pages,  Thickness, Genre ID, Genre Name, Publisher ID} 
  - {Title, Format } -> {price}  
- 데이터 중복이 발생

#### 제 2 정규형

- 1NF을 만족하고 기본키가 아닌 속성들은 기본키의 일부가 아닌 기본키 전체에 함수적 종속해야 한다 
- Title 에 종속적인 컬럼만 가진 릴레이션과 Format에 종속적인 릴레이션을 분리한다

![image](https://user-images.githubusercontent.com/79521972/176440919-6111a09f-5f8f-47fd-858f-5c4b45cde007.png)



<br>

### 3NF: Third Normal Form

![image](https://user-images.githubusercontent.com/79521972/176441014-67f93e22-2e56-42d0-9692-e6038cfad8e0.png)

제 3 정규형 (3NF, Second Normal Form) 

- 2NF을 만족하고 Transitive functional dependency (이행적 종속) 없어야 한다 

  - 함수적 종속 관계가 슈퍼키에서 시작하거나 prime attribute(candidate key에 포함된 attribute)로 끝나야 한다 

- {Title} -> {Author, Author Nationality, Pages, Thickness, Genre ID, Genre Name, Publisher ID} 

  {Title} -> { Author}, {Author} -> {Author Nationality} , then {Title} -> {Author Nationality} 

  {Title} -> {Genre ID}, {Genre ID} -> {Genre Name} , then {Title} -> {Genre Name} 

- 이행적 종속을 가지는 {Author Nationality} 와 {Genre Name}을 분리하여 각각 릴레이션으로 만듦

![image](https://user-images.githubusercontent.com/79521972/176441175-b228e915-da70-40de-8f1e-d81e17886bf0.png)

![image](https://user-images.githubusercontent.com/79521972/176441211-654465c7-0f10-450c-a346-d0cf16e9dda5.png)



<br>

### BCNF: Boyce-Codd Normal Form

- 3NF에서 조금 더 강화된 버전 (3.5NF 라고도 불림) 
- 모든 함수 종속 관계 ( X -> Y) 에서 X로 가능한 집합의 속성이 모두 슈퍼 키 이면 BCNF을 만족한다 
  - ![image](https://user-images.githubusercontent.com/79521972/176441385-6cee1153-7f5d-4062-aca7-bab5758a9051.png)
  - 위 함수적 종속 관계에는 이행적 종속이 존재하지 않기 때문에 3NF을 만족한다 
  - {D}가 슈퍼키에 포함되어 있지 않기 때문에 BCNF 조건에는 만족하지 않는다 
- 위반하는 함수 종속 관계를 다른 릴레이션으로 분리하여 BCNF 만족시킨다

![image](https://user-images.githubusercontent.com/79521972/176441423-2868547a-719a-42c9-9757-1370522e8cd1.png)

<br>

- 3NF 을 만족하면 정규화 되었다고 한다 
- Analytic query를 주로 실행하는 OLAP 애플리케이션인 경우 성능 향상을 위해 낮은 수준의 정규화를 추구하기도 한다 
- Denormalization : 일부 쓰기 성능의 손실을 감수하고 데이터를 묶거나 데이터의 복제 사본을 추가함으로써 데이터베이스의 읽기 성능을 개선하려고 시도하는 과정

<br>

## 정규형의 종류 - 2

### 4NF: Fourth normal form

#### Multi-valued dependency

- 함수적 종속성과 달리 이 조건은 튜플이 생성될 때 속성들 사이의 관계를 말한다 
- 릴레이션에 3개 이상의 속성이 있을 때 (X, Y, Z) 존재한다. X 값이 Y 값의 집합을 가지고 Z의 값의 집합을 가질 때 Y, Z값은 서로 독립적일 때 mult-valued dependency 가 있다고 한다 
- 릴레이션 R – Attribute-sets α ∈ R, β ∈ R  
  - α →→ β (α multidermines β)

![image](https://user-images.githubusercontent.com/79521972/176441769-443a2425-1c4c-459d-a338-f5501308635b.png)



<br>

#### Multi-valued dependency

- 예제) 두 개의 multi-valued dependency를 가진다 
  - { Course } →→ { Book } 
  - { Course } →→ { Lecturer } 
- Trivial multi-valued dependency : X →→ Y 
  - If Y is a subset of X 
  - Or X ⋃ Y is whole set of Relation

![image](https://user-images.githubusercontent.com/79521972/176441996-46f784c4-fcd8-41da-a878-192f05173861.png)

<br>

#### 제 4 정규형 (4NF, Fourth Normal Form)

- BCNF 를 만족하면서 릴레이션의 모든 notrivial multi-valued dependency  X →→ Y의 경우 X가 superkey인 경우

![image-20220629220001897](C:\Users\c_dragon\AppData\Roaming\Typora\typora-user-images\image-20220629220001897.png)



<br>

### 5NF: Fifth normal form

####  제 5 정규형 (5NF, Fifth Normal Form) – PJNF (Project-Join Normal Form)

- Join Dependency : 릴레이션 T의 부분 속성 집합 (projection table of T) {A, B, C} 를 조인한 결과가 릴레이션 T와 같을 때 JD 만족 
- 제 5 정규형의 조건 
- 릴레이션에 존재하는 모든 Join Dependency 이 기존 릴레이션의 슈퍼키를 가질 때 만족한다 여러개의 릴레이션으로 분해했을 때 나누어진 릴레이션이 기존 릴레이션의 슈퍼키를 가지며 더 이상 무손실 분해가 불가능할 때

![image](https://user-images.githubusercontent.com/79521972/176442502-90164423-9cab-46f2-b310-e1993dd59645.png)



![image](https://user-images.githubusercontent.com/79521972/176442574-47b05c80-2ed0-457f-b471-646fbb6b2d48.png)

(Franchisee ID, Title) 의 (Franchisee ID, Location) 조인은 기존 테이블을 만들지 못하므로 Join Dependency를 만족 못함 즉, 기존 테이블은 무손실 분해가 불가능한 상태이므로 5NF 를 만족한다 4NF을 만족하는 릴레이션이 5NF을 만족 못 하는 매우 드물다

<br>

### 6NF: Sixth normal form

#### 제 6 정규형 (6NF, Sixth Normal Form)

- a table is in 6NF when the row contains the Primary Key, and at most one other attribute 
- ![image](https://user-images.githubusercontent.com/79521972/176442790-4ca30671-7d85-4576-87c8-2d8478c79e32.png)
- 하나의 entity를 표현하기 위해 많은 테이블이 필요함 (만약 5NF 테이블이 primary key + N Attributes ) -> N 테이블이 필요함 
- 데이터 업데이트할 때 여러 테이블을 함께 업데이트 하므로 OLTP에 안 좋음



#### Analytics query (OLAP)을 중시하는 Datawarehouse 솔루션들은 내부적 저장 포맷을 column-based로 해서 6NF와 비슷한 포맷으로 저장함

- Aggregation과 같은 range query에 빠른 성능 
- Compression이 잘 되기 때문에 저장공간 절약

<br>

## 4. 데이터베이스 설계 프로세스

### 전체적인 데이터베이스 설계 프로세스

#### 데이터베이스 디자인을 통해 테이블의 스키마는 다음과 같은 경우로 생성된다

- ER 다이어그램을 릴레이션으로 변환을 통하여 생성 
- 모든 속성(Attributes)을 포함하는 하나의 릴레이션 – Universal relation 
  - 정규화를 통해서 작은 테이블들로 분해 된다 
- ad hoc 디자인을 통해서 생성 
  - 정규화를 통해서 테스트하고 좋은 디자인으로 변화



<br>

### ER 다이어그램과 Normalization

- ER 다이어그램이 모든 데이터를 잘 표현할 수 있게 설계되어 있다면 ER 다이어그램으로 생성된 테이블들은 추가적인 정규화 과정이 필요하지 않다 
- 그러나 실제 디자인에서는 키 속성이 아닌 속성 (attribute)에서 시작하는 함수적 종속성 (functional dependency)이 있을 수 있고 이런 문제들을 정규화를 통해서 분해 과정을 거친다 
- 예제) Employee (employee_id, name, department_name, building) 
           { department_name } -> { building }



<br>

### Denormalization

#### 정규화를 거쳐서 분해 된 테이블들을 조회할 때 분해되기 전 속성들을 볼 필요가 있다면 JOIN QUERY가 필요

- 아래 예제에서 Book의 전체 데이터를 조회할 때 book, Author, Genre 테이블의 join query가 필요 
- 만약 정규화 되기 전의 테이블이었다면 single table scan

![image](https://user-images.githubusercontent.com/79521972/176443649-54d59696-8ccc-4f77-939d-96e67761f615.png)

<br>

#### Denormalization

- 업데이트가 거의 발생하지 않으며 조회 성능이 중요한 애플리케이션에 사용 
- 빠른 조회 성능 제공 
- 중복 데이터 저장으로 인해 저장 공간이 더 필요하게 되고 update가 느려진다 
- 애플리케이션에서 이상 현상 등이 발생하지 않게 추가적인 코딩이 필요

#### Alternative way: Materialized View

- 필요한 속성들을 포함한 join query에 대한 materialized view을 생성한다 
- 테이블 형태로 저장되기 때문에 denormalization과 같은 성능 제공 
- 추가적인 저장 공간이 필요, view을 업데이트하는 cost가 필요 
- 그러나 애플이케이션에서 이상 현상에 대한 에러 처리가 필요 없음



<br>

### 다른 디자인 이슈들

####  정규화 과정을 통해 발견 못하는 디자인 이슈들

- 새로운 데이터가 추가 될 때 마다 컬럼 추가가 필요한 경우 

- 새로운 데이터가 추가 될 때 마다 테이블 추가가 필요한 경우 

- 예제) 

  - Good design 
             Earings (company_id, year, earning) 

  - Bad 

    ​		Earning_2019 (company_id, earning), Earning_2020 (company_id, earning) 
    ​		Earnings (company_id, earning_2019, earning_2020

