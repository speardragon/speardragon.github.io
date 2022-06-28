

## 1. SQL 소개

- SEQUEL (Structured English QUEry Language) 
  - 1974년 IBM San Jose Research Lab에서 연구용 DBMS 인 SYSTEM R 를 위한 언어로 개발된 언어 연산자로 하나 또는 두 개의 릴레이션 (Unary and binary operations) 
  - Paper : [https://researcher.watson.ibm.com/researcher/files/us-dchamber/sequel-1974.pdf](https://researcher.watson.ibm.com/researcher/files/us-dchamber/sequel-1974.pdf) SQL (Structured Query Language) : SEQUEL에서 이름이 바뀜 
- ANSI/ISO 가 표준 standard SQL 를 지정 
  - SQL-86 : 1986년도 만들어진 첫번째 표준 SQL, SQL1이라고도 불림 
  - SQL-92 : SQL2 로 1992년에 개정된 표준 
  - SQL-99, SQL 2011 …

- SQL 이란?
  - 대부분의 DBMS 는 SQL-92 표준의 대부분을 지원하고 추후의 표준에 지정된 기능을 추가하여 지원하고 고유의 기능도 제공함 
  - 최근 SQL-2011 에 추가된 기능 예제 
    - Temporal table (system versioned temporal table) 
      - 데이터 변경 내용의 전체 기록을 유지해 간편한 지정 시간 분석을 허용하도록 설계된 사용자 테이블의 종류

<br>

- SQL 분류 
  - DDL (Data Definition Language) 
    - 테이블을 생성하고 변경, 제거하는 기능을 제공 
  - DML (Data Manipulation Language) 
    - 테이블에 새 데이터를 삽입하거나, 테이블에 저장된 데이터를 수정, 삭제하는 기능을 제공 
  - SELECT 
    - 테이블 데이터를 조회하는 기능을 제공 
  - DCL (Data Control Language) 
    - 보안을 위해 데이터에 대한 접근 및 사용권한을 조절하는 기능을 제공

![image](https://user-images.githubusercontent.com/79521972/176189375-e20ead9b-fd0b-4a12-a6ea-e1135ae1d138.png)



<br>

## 2. DDL: 데이터 정의

- 데이터베이스 구조를 정의하고 변경하는 기능 제공하는 언어 
- Create : 새로운 데이터베이스 오브젝트들을 생성 (schema, table, view 등) 
- Alter : 존재하는 오브젝트의 정의를 변경 
- Drop : 존재하는 오브젝트를 데이터베이스에서 삭제

<br>

### CREATE TABLE syntax

![image](https://user-images.githubusercontent.com/79521972/176189577-439207bf-4866-47b7-898e-cbedc6b5eeae.png)



<br>

### 데이터 타입 on SQL

![image](https://user-images.githubusercontent.com/79521972/176189751-d74219d1-e92f-40aa-9598-30b4d704833b.png)

<br>

Decimal example 

- price decimal(10, 4)

![image](https://user-images.githubusercontent.com/79521972/176189835-957e1dbb-14d6-4e28-938a-55f7f103243e.png)



<br>

### default clause

- SQL 분류 := DEFAULT  
  - INSERT문에 컬럼에 대한 값이 지정되지 않았을 때 null 대신에  사용 
  - credit INT DEFAULT 0  
  - contact char(20) DEFAULT ‘james'



<br>

### constraint clause

\<integrity constraint> :={ UNIQUE | <primary key\> | <reference contraint\> | <check constraint\>}

- UNIQUE : 대체 키를 지정하고 유일성을 가지는 컬럼임을 지정 
- PRIMARY KEY : ROW를 식별할 수 있는 기본 키 ( unique + not null ) 
- \<reference constraint> : 참조하는 테이블을 지정하는 외래키 설정 (다음 장에서 설명)  
- \<check constraint> : column 값의 가능 domain 지정 (다음 장에서 설명>



<br>

### CREATE TABLE LIKE

CREATE TABLE \<table name> \<like_table_clause> 

\<like_table_clause> := LIKE \<table_name> [WITH DATA]

- Ex) CREATE TABLE customer_backup LIKE customer WITH DATA;



<br>

### ALTER TABLE

- ![image](https://user-images.githubusercontent.com/79521972/176191340-20b7cd27-a23c-40e4-b10e-6ac5e4563583.png)



### DROP TABLE

- 테이블 데이터 및 catalog 삭제 
- DROP TABLE \<table_name>



<br>

## 3. SELECT: 데이터 조회

### SELECT

- 테이블에서 원하는 데이터를 검색하는 SQL 
- Syntax 
  - SELECT [DISTINCT] {* | \<column_name>, ...} FROM ; 
- Example 
  - SELECT DISTINCT dept_name from instructor; 
    - DISTINCT : 결과 테이블에서 중복된 레코드를 제거하는 키워드 
- SELECT clause 는 **산술식** (arithmetic expression) 을 포함 가능 
  - +, -, * and / 
  - SELECT ID, name, salary/12 from employee;

<br>

### SELECT with WHERE

- 테이블에서 조건을 맞는 데이터만 검색하기 위해 WHERE clause 이용 
- Syntax 
  - SELECT [DISTINCT] {* | \<column_name>, ...} FROM \<table list> [ WHERE condition ]; 
  - Condition은 비교 연산자 (=, <>, <, >, <= , =>) 와 논리연산자 (AND, OR, NOT) 
- Example 
  - SELECT name from employee WHERE dept_name = ‘SALES’ and salary > 100000; 
- LIKE keyword 
  - 문자열 컬럼인 경우 부분적인 매칭으로 조건 검색 가능 
  - % : 0개 이상의 문자 
  - _ : 1개의 문자 
    - SELECT * from employee where name like ‘이%’; 
    - SELECT * from employee where name like ‘이정_’;

<br>

### NULL Value

- null 은 empty 가 아닌 unknown  
- null 값과 산술 연산 및 비교 연산 결과는 null 
  - 5 + null returns null 
  - null > 5 returns null 
  - null = null returns null 
- null 과 논리 연산 결과 
  - OR : (null OR true) = true, (null or false) = null, (null or null) = null 
  - AND : (true AND null) = null, (false AND null) = false, (null and null) = null 
  - NOT : (NOT null) = null 
- null 값을 가진 row을 찾기 위한 조건식 – IS NULL 
  - SELECT id, name FROM customer WHERE aga IS NULL;



<br>

### ORDER BY

- SELECT 의 검색 결과를 정렬하기 위해 ORDER BY 키워드 사용 
- Syntax 
  - SELECT [DISTINCT] {* | \<column_name>, ...}  
    FROM  \<table list> [ WHERE condition ] 
    [ ORDER BY \<column_name>, ... [ASC | DESC] ]; 
  - ASC: 오름 차순 정렬 (default), DESC: 내림 차순 정렬 
- Example 
  - SELECT id, name, age, address FROM customer ORDER BY age DESC; 
  - SELECT id, name, dept_name, salary FROM employee ORDER BY salary; 
  - SELECT id, product, quantity, order_date FROM sales_orders 
    ORDER BY order_date ASC, quantity DESC;

<br>

### Aggregation Function

- 특정 컬럼의 값을 통계적으로 계산한 결과를 보여주는 SQL **함수** 
  - COUNT : 컬럼 값의 개수 
  - MAX : 컬럼 값의 최대값 
  - MIN : 컬럼 값의 최소값 
  - SUM : 컬럼 값의 합계 
  - AVG : 컬럼 값의 평균 
- COUNT, MAX, MIN 은 LOB 타입을 제외한 모든 타입에서 사용 가능 
  SUM, AVG 는 숫자 데이터만 가능 
  - EX) 
  - SELECT sum(salary) FROM employee WHERE dept_name = ‘SALES’; 
  - SELECT count(distinct id) FROM sales_orders WHERE product = ‘computer’;



<br>

## Null value in aggregation function

- SELECT sum (salary) FROM employee; 
  - 만약 컬럼 값이 null이면 합을 계산할 때 무시 
  - 만약 컬럼 값이 모두 null이면 null이 출력된다 
- MAX, MIN, SUM, AVG 경우 null이 아닌 값으로만 계산 
  - 모두 null인 경우 null을 리턴 
- COUNT : null이 아닌 값의 개수를 출력 
  - 컬럼이 모두 null 인 경우 0 리턴 
- 테이블에 레코드가 없는 경우 
  - sum, avg, min, max : return null 
  - count : return 0



<br>

### GROUP BY

- 테이블에서 특정 컬럼의 값이 같은 rows를 모아 그룹을 만들고, **그룹별로 검색하기 위해 사용되는** keyword 
- 그룹에 대한 조건을 추가하려면 HAVING 키워드를 이용 
  - Where 조건 : 레코드를 grouping 하기 전에 조건을 검색 
  - Having 조건 : 레코드들을 grouping 후에 그룹에 대한 조건을 검색 
- **Syntax** 
  - SELECT [DISTINCT] {* | \<column_name>, ...}  
    FROM \<table list> [ WHERE condition ] 
    [ GROUP BY \<column_name_list> [ HAVING condition ] ] 
    [ ORDER BY \<column_name_list> [ASC | DESC] ]

- **Aggregation function을 제외한 SELECT문의 컬럼은 GROUP BY 리스트에 포함된 컬럼들만 가능** 
- SELECT dept_name, ID, avg (salary) from employee group by dept_name; 
  - Return error : SELECT list is not in GROUP BY clause and contains nonaggregated column 
- Example 
  - SELECT dept_name, avg (salary) FROM employee GROUP BY dept_name; 
  - SELECT dept_name, avg (salary) FROM employee GROUP BY dept_name HAVING avg(salary) > 50000000; 
  - SELECT company_name, count(product_name) as number_of_product,  
    max(price) as highest_price 
    FROM product GROUP BY company_name; 
  - SELECT company_name, count(product_name) as number_of_product,  
    max(price) as highest_price FROM product GROUP BY company_name 
    HAVING COUNT(product_name) >= 3;



<br>

### Set



![image](https://user-images.githubusercontent.com/79521972/176200347-b7e7f74b-6d9a-44cd-8c88-1feb3ec82869.png)

![image](https://user-images.githubusercontent.com/79521972/176199983-2415a50a-2e07-41a6-8dc0-abb3d24d38b0.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176200270-562b88f1-e4ce-47ec-abe9-364ee4e9e346.png)

![image](https://user-images.githubusercontent.com/79521972/176200451-edaa6b1e-0839-4851-a88e-f22154c0aa4f.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176200549-6418fd6a-3b7a-4e62-a511-5b3533fe2eac.png)

![image](https://user-images.githubusercontent.com/79521972/176200604-bcbddcd7-2069-4b37-bac7-d300efefb8bc.png)

<br>

### JOINs

![image](https://user-images.githubusercontent.com/79521972/176200740-64982cb3-1b8e-4096-be04-5aa1ade731ea.png)

![image](https://user-images.githubusercontent.com/79521972/176200814-2c9926a9-b1be-45b9-a116-50720dd7710b.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176200898-d5e2f0ff-02e8-4b94-8508-8cde5385de78.png)

![image](https://user-images.githubusercontent.com/79521972/176201002-39d74bbd-00a7-455e-ae00-b5d9eded6444.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176201096-41cc966d-bd7d-4655-b31b-8a39ee04efd0.png)

![image](https://user-images.githubusercontent.com/79521972/176201270-b1939076-6e17-41b8-9f29-737f4a740933.png)

<br>

### outer join

![image](https://user-images.githubusercontent.com/79521972/176201461-30b9bb73-cd20-4e00-bba6-cbd4c191a1f9.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176201517-cd4185e1-6861-4641-bbcb-540b4371f45c.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176202268-f5db2de6-4315-4d2c-b1e4-acd035614d11.png)

<br>

![image](https://user-images.githubusercontent.com/79521972/176202335-6cf877f2-947e-4e99-b482-1ff18140100f.png)





<br>

## 4. DML: 데이터 조작

### INSERT

- 테이블에 새로운 row을 삽입하는 SQL 
- **Syntax** 
  - INSERT \<table_name> [(\<column_name>, ...)] VALUES (value, …) 
- **Example** 
  - INSERT student values (’20210001’, ‘James’, ‘Computer’, 1); 
  - INSERT student (id, name, dept_name, grade) VALUES (‘20210001’, ‘James’,  ‘Computer’, 1) 
  - INSERT student (id, name, dept_name) VALUES (‘20210001’, ‘James’, ‘Computer’) - set grade value as null

```sql
CREATE TABLE student (
    id varchar(8) primary key,
    name varchar(20) not null,
    dept_name varchar(20),
    grade smallint);
```



<br>

- INSERT INTO SELECT 
  - 다른 테이블에서 질의 결과를 삽입하는 구문 
  - INSERT INTO  [(, ...)] 
- Example 
  - INSERT INTO student SELECT * from new_students;

```sql
CREATE TABLE student (
    id varchar(8) primary key,
    name varchar(20) not null,
    dept_name varchar(20),
    grade smallint);
```

<br>

### DELETE

- 테이블에서 쿼리에 해당되는 row을 지우는 SQL Syntax 
  - DELETE FROM  [WHERE condition] 
- Example 
  - DELETE FROM student; -> 모든 rows 삭제 
  - DELETE FROM student WHERE dept_name = ‘Computer’;  
  - DELETE FROM student WHERE dept_name in (select dept_name from  department where building = ‘SEL01’)



<br>

### UPDATE

- 테이블에 저장된 데이터를 변경하는 SQL Syntax 
  - UPDATE  SET  = value [WHERE condition] 
- Example 
  - UPDATE employee set salary = salary * 1.07; 
  - UPDATE employee set salary = salary * 1.07 WHERE hire_date < ‘2020.01.01’; 
  - UPDATE employee set salary = salary * 1.07 WHERE dept_name in (SELECT *  from department WHERE location = ‘SEOUL’

