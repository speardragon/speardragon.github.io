

## 1. View에 대한 개념 및 활용

### Views

- View의 목적은?
  - 어떤 경우에 모든 사용자가 전체 논리적 모델 (데이터베이스에 저장된 모든 실제 테이블)을 보는 건 문제가 될 수 있음
  - 필요한 데이터만 **특정 사용자**들에게 유출할 필요가 있을 때 사용
    - employ 테이블에서 아이디, 이름, 부서 조회 가능하게 하지만 salary 정보를 숨기고 싶을 때
  - 질의문 작성을 쉽게 만들어 준다.
    - Group by나 aggregation function 등을 미리 정의
  - 데이터 종속성 제거
    - 응용프로그램은 뷰를 통해 접근함으로써 테이블 스키마 변화에 신경 쓸 필요가 없다.

- View 정의
  - CREATE VIEW  \<view_name>[(\<column_name_list>) as \<query expression> 
  - 다른 view을 이용한 query로 view 생성 가능 
  - Column list가 생략된 경우 query문의 결과 relation의 컬럼 리스트로 view 컬럼이 지정됨 
  - 스키마 변화에 신경 쓸 필요가 없다
- Example
  - CREATE VIEW public_employee_information AS SELECT ID, name, dept_name FROM employee; 
  - CREATE VIEW department_total_salary (dept_name, total_salary) AS SELECT dept_name, sum (salary) FROM employee GROUP BY dept_name;

- 다른 view를 사용하는 예제
  - ![image](https://user-images.githubusercontent.com/79521972/176428928-1d5112a8-6e76-41c3-93c1-e73fc8ee7d2a.png)





<br>

### View Expansion

- 질의 처리에서 view가 query expression 으로 대체되는 로직
  - DO
    - query 절에서 view relation를 찾음
    - view relation을 view의 정의로 치환
  - While (there is no more views in expression)

![image](https://user-images.githubusercontent.com/79521972/176429174-53ad7ac1-686c-4252-9228-0437f525a7ea.png)



![image](https://user-images.githubusercontent.com/79521972/176429210-c780b453-2281-4997-ae03-74ccaeff354a.png)



<br>

### Updatable View

- View에 대한 삽입, 수정, 삭제 연산도 가능 
  - 기본 테이블에 대한 연산으로 변경되어 실행 
- Example 
  - CREATE VIEW public_employee_information AS SELECT ID, name, dept_name FROM employee; 
  - INSERT INTO public_employee_information values (‘10101’, ‘Green’, ‘Sales’); 
    - -> (‘10101’, ‘Green’, ‘Sales’, null) 이 employee 테이블에 대한 insert로 변환 
- Updatable View가 되기 위한 조건 
  - 베이스 테이블이 하나인 경우 (JOIN 인 경우 불가능) 
  - Select clause에 컬럼 이름만 있는 경우 (Aggregation 함수나 DISTINCT 있는 경우 불가능) 
  - Group by 나 having 이 없는 경우

- Where 절이 있는 경우 
- CREATE VIEW sales_employee 
            AS SELECT * FROM employee WHERE dept_name = ‘Sales’; 
- INSERT INTO sales_employee values (‘052881’, ‘James’, ‘Development’, 5000000); 
  - -> Succeed

<br>

### Materialized View

- Materialized View 
  - View에 대한 expression의 결과가 persistence 테이블 형태로 저장되는 뷰 
  - View에 대한 계산이 복잡한 경우 (여러 테이블들에 대한 조인, aggregation) 쿼리 결과를 테이블에 미리 계산 하여 저장하여 view에 대한 쿼리 성능을 높임 
- materialized view maintenance 
  - View에서 사용되는 베이스 테이블들이 업데이트가 있을때 Materialized View도 업데이트 해주는 방법 
  - 많은 DBMS 들은 on-demand mode / real-time mode (제한된 경우) 로 refresh 방법을 제공한다

<br>

### Drop View

- Syntax : DROP VIEW  \<view_name>
- View만 삭제하고 베이스 테이블들은 영향을 받지 않는다



<br>

## 2. 트랜잭션 SQL

### Transaction

- 논리적인 작업의 단위: LUW, Logical Units of Work
- Transaction은 read, write, delete, update등의 연산들로 구성되나 한 Transaction 단위로 일관성이 보장된다.
  - All or Nothing

<br>

#### Transaction의 특성

- **Atomicity** (원자성) 
  - Transaction을 구성하는 연사들이 모두 실행이 되거나 또는 하나도 실행되지 않거나.. 
  - (All of Nothing) 
  - 연산이 실패하면 이미 실행된 연산들로 바뀌었던 부분들이 다시 원 상태로 바뀐다 (Rollback) 
  - 계좌이체 예제 
    - 성호 잔액 10,000원, 은경 잔액 0원 
    - 성호 -> 은경 5,000원 이체 - 성호 잔액 = 성호잔액 - 5,000 - 은경 잔액 = 은경 잔액 + 5,000 
    - 성호 잔액 5,000원, 은경 잔액 5,000원

- **Consistency** (일관성)  
  - Transaction이 실행된 후에도 일관성 있는 데이터베이스 상태로 유지하는 것을 의미한다 
  - 무결성 제약 조건으로 정의된 일관성 조건이 있다면 transaction의 구성하는 연산 중 조건을 어기는게 있다면 트랜잭션은 실패 
- **Isolation** (격리성) 
  - Transaction을 수행 시 다른 Transaction의 연산 작업이 끼어들지 못하도록 보장하고 다른 Transaction 들이 Transaction 
  - 안의 중간 연산을 볼 수 있다는 것도 의미한다 
  - 성호 -> 은경 5,000 원 이체 중에 관리자가 성호/은경 계좌의 잔액 합을 조회해 보면 항상 10,000원이 보장된다 
- **Durability** (지속성) 
  - 성공적으로 수행된 Transaction은 영원히 반영되어야 함을 의미 
  - 시스템이 장애가 발생했더라도 성공적으로 수행된 Transaction 결과는 데이터베이스에 반영되어 있음을 보장 
  - 전형적으로 Transaction은 업데이트에 대한 로그로 적고 로그가 저장된 후에 Transaction이 Commit으로 간주 
  - 장해 후 로그 데이터를 가지고 데이터베이스 재 구성 (Recovery)



**DBMS는 Transaction ACID를 보장하기 위해 많은 구현이 필요 목차-7에서 다룸.**



<br>

### Transaction SQL

- Transaction Mode

  - Autocommit mode : default  

    - Statement가 시작할 때 transaction이 내부적으로 시작하고 매 statement가 끝날 때 마다 commit이 자동으로 실행된다 

    - Session level로 mode를 변경 가능 

      ​	SET AUTOCOMMIT = OFF 

      ​	JDBC : connection.setAutoCommit(false)  

  - Explicit mode 

    - START TRANSACTION; -> Transaction을 시작 
    - COMMIT; -> Transaction의 commit에서 변경 내용을 데이터베이스에 저장하는 statement 
    - ROLLBACK; -> Transaction 의 연산들로 변경된 내용을 취소하는 statement



<br>

## 3. 무결성 제약 조건

### Integrity Constraints on Single Table

- Integrity Constraints (무결성 제약 조건) in CREATE TABLE

  - not null 

    - 컬럼 값으로 null을 허용하지 않을 때 지정 
    - name char(10) NOT NULL -> 고객 이름 컬럼으로 사이즈 10 이내, 필수 입력 사항 

  - primary key 

    - 테이블에서 튜플 (row)을 찾는 기본 키 
    - unique + not null 
    - 만약 같은 값이 이미 테이블에 존재하거나 null을 입력하려고 하면 insert 실패 

  - Unique 

    - Primary key와 같이 튜플에 유일성을 체크하는 대체키 (candidate key) 지정한다. 

      이건 null을 허용



- Integrity Constraints (무결성 제약 조건) in CREATE TABLE 
  - Check (p) p : predicate 
    - 특정 속성에 값의 도메인을 check 키워드를 사용하여 지정할 수 있

![image](https://user-images.githubusercontent.com/79521972/176431797-55806b8e-852c-4e0d-a57e-a2c1a407ed51.png)



<br>

### Referential constraint

- 참조 무결성 (Referentail Integrity)
  - 관계 데이터베이스 관계 모델에서 2개의 관련 있던 테이블 간의 일관성 (데이터 무결성)  유지하는 걸 말함 
  - 참조 무결성을 정의하기 위해 foreign key (외래키)을 지정하는데 foreign key에 포함 되는 컬럼은 참조하는 부모 테이블의 primary key 또는 candidate key이어야 한다 
  - Syntax  
  - FOREIGN KEY (\<column_name>) REFERENCES  \<parent _table_name>(\<column_names>) 
    [ON DELETE reference_option] 
    [ON UPDATE reference option]



- reference_option at foreign key
- 부서테이블에서 (3, 홍보부) 투플을 삭제하려고 할 때 
  - ON DELETE NO ACTION (default) : 투플을 삭제 못하게 한다 
    - 3을 참조하는 투플이 존재하므로 삭제 불가 
  - ON DELETE CASCADE : 관련 튜플을 함께 삭제 
    - (1001, 정소화, 3) 을 함께 삭제 
  - ON DELETE SET NULL : 관련 투플의 외래값을 NULL로 변경 
    - 정소화 사원의 투플의 소속부서를 NULL로 변경 
  - ON DELETE SET DEFAULT : 관련 투플의 외래키 값을 default 값으로 변경. Default가 없는 경우 에러 발생 
    - 정소화 사원의 투플의 소속부서를 default값으로 변경 
  - ON UPDATE NO ACTION (default) : 투플의 변경 불가 
  - ON UPDATE CASCADE : 외래키 값을 같이 변경 
  - ON DELETE SET NULL : 관련 투플의 외래값을 NULL로 변경 
  - ON DELETE SET DEFAULT : 관련 투플의 외래키 값을 default 값으로 변경. Default가 없는 경우 에러 발생

![image](https://user-images.githubusercontent.com/79521972/176432438-4ddfe502-1b7a-4334-8335-ae4c2095a846.png)



<br>

### Example

![image](https://user-images.githubusercontent.com/79521972/176432563-def2f8ed-14b5-4d2b-b3b5-7827cfa1e8a1.png)

<br>

## 4. SQL DCL: 접근 권한

### 권한 관리(Authorization Management)

- 사용 권한 
  - 데이터베이스의 모든 객체는 해당 객체를 생성한 사용자만 사용 권한을 가진다
- 권한 부여 
  - 여러 사용자가 공유해서 사용할 목적으로 다른 사용자들에게 자신의 객체에 대한 권한을 부여 할 수 있다 
- Privilege 의 종류 
  - SELECT 
  - INSERT 
  - UPDATE 
  - DELETE 
  - REFERENCES



<br>

### GRANT

- **객체의 소유자가 다른 사용자에게 객체의 대한 사용 권한을 부여하기 위해 사용되는 SQL** 

- GRANT \<privilege list> ON \<view or table name> TO \<user list> [ WITH GRANT OPTION ]; 

  ​	\<user list>

  ​			 - database user 

   			- public (모든 사용자들에게 권한을 주고 싶을 때) 

  ​			 - role name 

- View 에 대해서 권한을 주었다고 베이스 테이블에 대해서 사용권한이 주어진 건 아님 

- WITH GRANT OPTION 

- GRANT로 부여 받은 권한은 기본적으로 다른 사용자에게 부여 할 수 없다. 그러나 WITH GRANT OPTION으로 부여 받은 권한은 다른 사용자에게 부여 가능



<br>

### REVOKE

- GRANT통해 다른 사용자에게 권한을 부여한 사용자가 부여한 권한을 취소하는 SQL

- REVOKE  ON  FROM CASCADE | RESTRICT; 

  CASCADE | RESTRICT 

  - CASCADE : WITH GRANT OPTION 으로 부여된 모든 권한을 연쇄적으로 다 취소하는 옵션 
    - B가 C에게 준 권한 삭제, A가 B에게 준 권한 삭제 

- RESTRICT : 만약 다른 사용자가 권한을 준 사용자가 있을 경우 REVOKE가 실패한다 

  - REVOKE SELECT ON employee FROM B RESTRICT 
    - Failed : B가 C에게 준 권한 때문에



<br>

### GRANT/REVOKE 예제

![image](https://user-images.githubusercontent.com/79521972/176433362-786821ca-949e-48dc-9651-df8cbfc2a5d3.png)



<br>

### Role

- 여러 사용자가에게 동일한 권한들을 부여하고 취소하는 작업을 편하게 하기 위해 ROLE을 사용 

  - ROLE에게 사용자는 권한을 부여 할 수 있다 
  - 다른 사용자들에게 ROLE을 GRANT 할 수 있다 
  - CREATE ROLE  \<role_name>
  - DROP ROLE  \<role_name>
  - GRANT  TO \<role_name> TO \<user_list>;
  - ROVOKE  \<role_name> FROM \<user list>

- Ex)

  - ```mysql
    CREATE ROLE role_1;
    GRANT SELECT, INSERT, DELETE, UPDATE on employee TO role_1;
    GRANT role_1 TO user1;
    GRANT role_2 TO user2;
    GRANT role_2 TO user3
    ```



