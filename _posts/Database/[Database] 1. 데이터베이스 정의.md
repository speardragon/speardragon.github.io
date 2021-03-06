

# 데이터 베이스 기본 개념

### 데이터베이스  정의

- 데이터베이스
  - 특정 조직의 여러 사용자가 공유하여 사용할 수 있도록 통합해서 저장한 운영 데이터의 집합
- 데이터베이스 예제
  - 은행: 계좌정보, 입출금 내역 등
  - 항공사: 예약정보, 비행기 스케쥴
  - 대학교: 학생정보, 수강 신청
  - 온라인 쇼핑몰: 고객 기록, 주문 내역
  - 제조업: 제품 목록, 주문, 재고, 공급망
  - 회사 인사시스템: 직원정보, 연봉

<br>

### 데이터 베이스 특징

- 데이터베이스의 특징 -> 쇼핑몰 예제
  - 실시간 접근 가능 -> 목록 조회
  - 계속적으로 변환 -> 구입정보, 물건재고 정보
  - 동시 공유가 가능 -> 많은 고객이 동시 접속, 구매 가능
  - 저장된 주소가 아닌 내용으로 참조 가능 -> 가장 많이 팔린 제품은?

<br>

### 데이터의 유형

> 데이터베이스는 데이터가 모여 있는 집합

- 데이터의 유형
  - 정형 데이터(structed data)
    - 액셀의 스프레시트, 관계데이터베이스의 테이블(sql, csv)
  - 반정형 데이터(semi-structed data)
    - self-describing data: HTML, XML, JSON
  - 비정형 데이터 (unstructed data)
    - 정해진 구조가 없이 저장된 데이터
    - text, 멀티미디어 데이터(txt, png)



<br>

### 파일 시스템을 사용했을 때의 문제점-1

- 데이터 중복성 문제: 공간 낭비
- 업데이트 및 데이터 일관성(data consistency) 유지에 어려움
- 데이터 무결성 (Data integrity constraints) 유지 어려움
  - 응용프로그램이 모두 체크해야 함.
  - 엑셀의 스프레드시트, 관계데이터베이스의 테이블
- 데이터 종속성
  - 응용프로그램이 파일 데이터 구조에 종속적
  - 파일구조가 바뀔 때마다 응용프로그램 교체 필요



### 파일 시스템을 사용했을 때의 문제점-2

- 동시성 (Concurrency) 제공이 어려움
  - 여러 사용자가 동시에 접근할 때 문제 해결에 어려움
- 원자성 (Atomicity) 제공 어려움
  - 파일 변경 중에 시스템 장애가 발생했을 때 처리가 어려움
- 보안 (Security) 제공 이슈
  - 사용자 별 파일 안의 일부 데이터 읽기 권한 제어가 어려움



<br>

### 데이터 베이스 관리시스템(DBMS)이 해답

- 응용프로그램과 데이터 파일 분리
  - 프로그램은 데이터베이스 통해서 파일시스템의 파일에 저장한 데이터에 접근

- 데이터베이스 관리시스템 (DBMS) 
  - 응용프로그램과 데이터 연결을 도와주며 데이터 관리와 데이터에 대한 기본 처리를 담당하는 추가적인 소프트웨어

![image](https://user-images.githubusercontent.com/79521972/176182568-5f2f1a2a-a1a5-4375-b9fe-68771c2e243b.png)





<br>

## 2. 데이터베이스 관리시스템(DBMS)

### 데이터베이스 관리시스템(Database Management System)

- 데이터베이스 관리시스템 (DBMS) 
  - 파일시스템의 데이터중복과 데이터 종속 문제를 해결하기 위해 제시된 소프트웨어 
  - 데이터베이스의 생성과 관리를 담당 
  - 모든 응용프로그램은 데이터베이스 공유 가능, DBMS 통해 데이터 삽입, 수정, 검색 
- DBMS는 운영체제와 함께 중요한 시스템 소프트웨어 패키지 
  - Postgres : 약 800,000 code line / 오라클 : 8,000,000 code line 
- 대표적 DBMS 
  - Oracle, MS SQL Server, Sap HANA, MySQL, PostgreSQ

<br>

### 데이터베이스 관리시스템의 주요 기능

- 정의 기능 -> 데이터베이스 구조를 정의하거나 수행
- <mark>조작 기능</mark> -> 데이터를 삽입, 삭제, 수정, 검색하는 연산을 수행
- 제어 기능 -> 데이터를 항상 정확하고 안전하게 유지하는 기능

<br>

### DBMS의 장단점

- DBMS 장점

  - 파일시스템의 데이터 중복 문제 해결 

  - 데이터 독립성 확보 (데이터 구조가 바뀌어도 응용프로그램을 바꿀 필요 없음) 

  - 데이터 동시 공유 (Concurrency control) 

  - 데이터 보안 향상
    - 사용자 별 접근 가능한 데이터베이스 영역 제한 가능 

  - 데이터 무결성 유지

  - 표준화 방식으로 데이터에 접근(with 데이터베이스 언어) 

  - 장애 발생 후 회복 시 데이터 일관성과 무결성 유지 
    - 트랜잭션 가능 

  - 응용 프로그램 개발 비용이 줄어 듬

- DBMS 단점

  - DBMS 구매 비용 
  - 응용프로그램이 DBMS를 통해 데이터에 접근하기 때문에 추가적인 오버헤드가 있다 
  - 응용 프로그래머가 DBMS가 어떻게 동작하는 지와 표준 데이터 언어에 대한 지식 필요 
  - DBMS 장애가 발생할 때 모든 응용프로그램 장애가 발생함



<br>

### Data Dictionary On DBMS

- Data Dictionary (System Catalog) 
  - DBMS는 **database**와 함께 **metadata**(data about data)을 저장한다 
  - 데이터 정의어 (DDL)D에 의해 생성된다. 
- Metadata 
  - 각 데이터에 접근할 수 있는 데이터의 이름(테이블, 컬럼 이름) 
  - 스토리지에 데이터가 저장된 위치 
  - 보안을 위한 제약 (security constraint) : 누가 어떤 데이터를 접근할 수 있나 
  - 무결성을 위한 제약 (integrity constraint) : 어떤 값이 데이터에 유효한가





<br>

## 3. 데이터베이스 시스템 구조

### 데이터베이스 시스템

-  데이터베이스에 데이터를 저장하고, 저장된 데이터를 관리하여 조직에 필요한 정보를 생성해주는 시스템  
- 데이터베이스와 이를 관리하는 소프트웨어인 DBMS와 응용프로그램 모두를 칭하는 용어 
- 통상적으로 DBMS와 구분없이 사용



<br>

### 데이터베이스의 중요 개념

- 스키마 (schema)
  -  데이터베이스에 저장되는 데이터의 논리적구조와 제약조건을 정의한 것

![image](https://user-images.githubusercontent.com/79521972/176184050-b6d3a2f5-69bf-49ea-9d15-7daa54587b9d.png)



- 인스턴스(instance)
  - 정의된 스키마에 따라 데이터베이스에 실제로 저장된 값



<br>

### 3단계 데이터 베이스 구조

- 미국의 기관인 ANSI/SPARC에서 데이터베이스의 내부 구조를 감추고 일반 사용자가 데이터베이스를 쉽게 이용하고 사용할 수 있게 하는 구조 제안

![image](https://user-images.githubusercontent.com/79521972/176184377-69e0e035-7d22-476d-82a3-849af6b999b9.png)

- Physical level – Internal Schema (내부 스키마) 
  - 실제 데이터가 데이터베이스에 어떻게 저장되는지 기술 
  - 파일에 저장되는 레코드의 구조(사이즈), 압축방법, 암호화 방법 등 
- Conceptual level – Conceptual Schema (개념 스키마) 
  - 일반적인 스키마 
  - 데이터베이스를 개념적으로 표현 그리고 데이터 테이블 간의 관계를 기술 
  - 물리적 데이터 독립성

- External level – External Schema (외부 스키마) 
  - 사용자가 보는 view level 
  - 사용자에게 view의 형태로 필요한 정보만 보여주는 단계 
  - 각 사용자들이 개별 요구 사항에 따라 다른 view를 정의해서 데이터를 볼 수 있다. 
  - 데이터베이스 하나에 여러 개의 외부 스키마 가능 
  - 개념스키마와 논리적 데이터 독립성



<br>

## 4. 데이터베이스 시스템 언어

### 데이터베이스 사용자

- 데이터베이스 관리자 (DBA) 
  - 데이터베이스 시스템을 운영하고 관리 
  - 데이터베이스를 설계 및 구축, 제어 
  - DBMS 자체는 물론 데이터베이스 구축, 관리에 해박한 지식과 많은 경험이 요구됨 
- 응용 프로그래머 
  - 데이터베이스 언어를 이용하여 응용프로그램을 작성 
- 최종 사용자 (End user) 
  - 데이터베이스에 접근하여 데이터를 조작 
  - casual end user : 데이터베이스 언어를 이용해 데이터베이스 접근 및 조작 
  - native end user : 데이터베이스 언어를 쓰지 않고 응용프로그램을 통해 데이터베이스 접근

![image](https://user-images.githubusercontent.com/79521972/176184831-5c6b0b2b-ab89-4af4-872a-fa13d25d6505.png)



<br>

### 데이터 언어

- 사용자가 데이터 베이스를 구축하고 이에 접근하기 위해 데이터베이스 관리 시스템과 통신하는 수단
- 데이터 언어 종류 
  - 데이터 정의어 (DDL) 
    - 스키마 구조와 제약조건 등을 정의, 삭제, 수정

- ```sql
  create table instructor ( 
      ID char(5), 
      name varchar(20),
      deptname varchar(20), 
      salary numeric(8,2), primary key(ID));
  ```



<br>

- 데이터 조작어 (DML)
  - 데이터의 삽입, 삭제, 수정, 검색
- 데이터 제어어 (DCL)
  - 내부적으로 필요한 규칙(보안, 무결성 체크) 등을 정의하는 데이터 언어 
  - 데이터베이스 백업하거나 백업이미지로 회복(recover)하는 명령



<br>

### 질의 처리기

- DDL Interpreter 
  - DDL로 작성된 스키마의 정의를 해석, 데이터 딕셔너리(Catalog)에 저장 
- DML Compiler 
  - DML로 작성된 데이터 처리 요구를 processing engine이 이해할 수 있는 코드로 해석하여 plan을 작성 
- DML(Query) Processing Engine 
  - 컴파일된 plan을 데이터베이스에 실제로 실행



<br>

![image](https://user-images.githubusercontent.com/79521972/176185442-9e718623-85ef-4628-893e-3257712eb5f3.png)



<br>

## Reference

- 패스트캠퍼스 - 이주연 강사님



