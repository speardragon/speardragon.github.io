

# 관계형 데이터 모델

## 1. 데이터 모델링

- 데이터 모델링 (data modeling)
  - 현실 세계에 존재하는 데이터를 컴퓨터 세계의 데이터베이스로 옮기는 과정 
  - 데이터베이스 설계의 핵심 과정
- 데이터 모델링 3단계
  - ![image](https://user-images.githubusercontent.com/79521972/176185879-72fae928-b4bd-4456-a543-01cb3cfcffc7.png)
  - 개념적 데이터 모델링
    - 현실세계를 추상화하여 중요 데이터를 개념 세계로 추출해 가는 과정 
    - 결과물로 개념적 데이터 모델 (객체 – 관계 (E-R) 모델) 
  - 논리적 데이터 모델링 
    - 개념 세계의 데이터를 데이터베이스가 저장할 구조로 변환하는 과정 
    - 결과물로 관계 데이터 모델 
  - 물리적 데이터 모델링 
    - 논리 데이터 모델이 실제 데이터베이스 저장소에 저장되는 저장 구조 
    - (테이블, 컬럼)로 변경



<br>

## 데이터 모델링 예제

![image](https://user-images.githubusercontent.com/79521972/176186178-9492881b-de56-4622-b39c-4aeb1d3cd2bc.png)





<br>

## 2. 관계형 데이터 모델

### 관계 데이터 모델

-  개체에 대한 데이터를 저장하는 논리적 구조 – 릴레이션 (2차원의 테이블 구조)

![image](https://user-images.githubusercontent.com/79521972/176186319-e373d1af-cf36-4708-841f-a9c526cf9a5e.png)



<br>

### 릴레이션의 특성

- 튜플의 유일성 : 동일한 튜플이 존재할 수 없다 
- 튜플의 무순서 : 튜플 사이의 순서는 무의미 
- 속성(애트리뷰트)의 무순서 : 속성 사이의 순서는 무의미 
- 속성의 원자성(Atomic) : 애트리뷰트 값으로 하나 (나누어지지 않는) 값만 가짐



<br>

## Key의 종류

### Key란?

- 릴레이션에 튜플을 구별하는 역할을 하는 속성 또는 속성의 집합 
- Super key : 튜플을 구별하기 위해 유일성을 제공할 수 있는 속성 또는 속성의 집합 
  - Ex) {ID}, {ID, name} 
- Candidate key : super key 중에서 개수가 가장 작은 키 
  - Ex) {ID} 
- Primary key : candidate key 중에서 디자인을 고려하여 선택된 키 
- Foreign key : 다른 릴레이션의 primary key을 참조하는 속성 또는 속성의 집합

![image](https://user-images.githubusercontent.com/79521972/176186692-63b315b1-4b31-4e05-bd5d-f165fa753aea.png)



<br>

## 3. 관계대수 - 1

### 관계 데이터 연산

모든 DBMS는 데이터 처리를 위해 하나 이상의 데이터 언어를 제공 

- Formal query language 

  - 수학기호(notation)을 사용하여 데이터 처리를 기술한 언어 

  - 새로운 언어의 개념과 유용성을 검증하는 기준 

  - 관계 대수 (Relation algebra) 

- Commercial language 

  - 수학적인 원리를 기반으로 사용하기 쉽게 만들어진 단어 

  - 관계 대수로 만들어진 모든 질의가 표현 가능 – Relationally complete 

  - SQL



<br>

### 관계대수 연산자

- 관계 대수 연산자 (Relational Algebra Operations) 
  - 피연산자로 하나 또는 두 개의 릴레이션 (Unary and binary operations) 
  - 각 연산자의 연산 결과는 새로운 릴레이션-연산의 합성(compose)&체이닝(chaining) 가능 
- 연산자 종류 
  - select 
  - project 
  - union 
  - difference 
  - intersection
  - cartesian product 
  - natural join  
  - theta join 
  - outer join



<br>

### select operation

![image](https://user-images.githubusercontent.com/79521972/176187379-996707b9-ec5d-4e27-9470-ec451a915e59.png)

![image](https://user-images.githubusercontent.com/79521972/176187397-01b61e9c-1c9a-4088-b6cb-cb2b0ec429c9.png)

<br>

### project operation

![image](https://user-images.githubusercontent.com/79521972/176187479-9d36348c-03fd-4d07-9d71-cfb0bbfa6db1.png)

![image](https://user-images.githubusercontent.com/79521972/176187510-05b1eb61-bd41-4b2c-adf6-07f56179d19c.png)

<br>

### union(합집합) operation

![image](https://user-images.githubusercontent.com/79521972/176187640-ad8acae0-b3d2-48c2-a79f-7e9867687bc2.png)

![image](https://user-images.githubusercontent.com/79521972/176187672-c53ce086-d53e-4db3-9dc1-259258d2ecb2.png)



<br>

### difference(차집합) operation

![image](https://user-images.githubusercontent.com/79521972/176187768-33e318c5-40e0-4c2d-bbf9-33cd4e18333d.png)

![image](https://user-images.githubusercontent.com/79521972/176187788-64006cce-3d6a-4983-b1d7-7a6295d83e93.png)



<br>

### intersection operation

![image](https://user-images.githubusercontent.com/79521972/176187864-84904d47-45fc-4dda-b272-b4bc6e621936.png)

![image](https://user-images.githubusercontent.com/79521972/176187893-7a8ed307-1934-4091-8d74-d29b282b950c.png)



<br>

## 4. 관계 대수 -2

### Cartesian product(카티션 프로덕트) operation

![image](https://user-images.githubusercontent.com/79521972/176188077-7cbf3a74-4b6d-4e30-aa92-b320068e74d9.png)

![image](https://user-images.githubusercontent.com/79521972/176188111-cd93cee9-359d-4f6f-a2f9-ab66ec604c74.png)



<br>

### Natural join(자연 조인)

![image](https://user-images.githubusercontent.com/79521972/176188181-cb9c950b-efc6-4246-9421-e65166f9f9d9.png)

![image](https://user-images.githubusercontent.com/79521972/176188221-20cc5178-0d43-46c9-9999-c217a47c3a9e.png)



<br>

### theta join(세타 조인)

![image](https://user-images.githubusercontent.com/79521972/176188297-00aac603-91c1-45b9-b3a7-8ae51b3753e9.png)



![image](https://user-images.githubusercontent.com/79521972/176188320-53bbe128-f824-4da5-b2b3-265bd6446b4d.png)



<br>

### outer join(외부 조인) operation

![image](https://user-images.githubusercontent.com/79521972/176188415-7a2df99f-5a17-4182-9651-a62f594cb17e.png)

![image](https://user-images.githubusercontent.com/79521972/176188483-a8b7dd3c-a7c3-44fb-a074-5fa20c5826d5.png)













