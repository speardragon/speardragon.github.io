

## 1. 데이터베이스 설계

### 데이터베이스 설계

- 데이터베이스 설계란?
  - 사용자의 다양한 요구 사항을 고려하여 데이터베이스를 생성하는 과정 
  - 이미 구축된 데이터베이스는 구조를 변경하기 어려우므로 체계적인 설계 과정을 통해 데이터베이스가 올바르게 구축되어야 함 
  - 대표적인 설계 방법 
    - E-R 모델과 Relation 변환 규칙을 이용한 설계 
    - 정규화를 이용한 설계

<br>

### 데이터베이스 설계 단계

1. 요구 사항 분석 

   - 실제 세계에서 어떤 요구사항의 시스템을 구출할 것인지 대한 데이터베이스의 용도 파악 

   - 요구 사항에 대해 어떤 데이터들이 필요한지, 어떤 기능들이 필요한지 분석 

   - 결과물 : 요구 사항 명세서 

2. 개념적 설계 (Conceptual Model) 

   - 요구 사항 분석 결과물을 개념적 데이터 모델을 이용해 개념적 구조로 표현 

   - 요구 사항 명세서를 E-R다이어그램으로 표현 

3. 논리적 설계 

   - 관계 모델 ( Relational model)을 통해 개념적 모델을 논리적으로 표현 

   - 결과물 : 릴레이션 스키마 

4. 물리적 설계 

   - DBMS로 구현이 가능한 물리적인 구조를 설계 

   - 저장 레코드 타입 및 인덱스 타입 등 설계 

   - 결과물 : 물리적 스키마

<br>

### 1. 요구 사항 분석

- 요구 사항 분석 작업
  - 데이터베이스를 사용할 주요 사용자의 범위 결정 
  - 사용자가 수행하는 업무를 분석 
  - 수집된 요구 사항 대한 분석 결과를 요구 사항 명세서로

![image](https://user-images.githubusercontent.com/79521972/176434180-8dde196b-3164-4aa0-b64c-ecc8c9461fc8.png)

- ※ 출처 : 데이터베이스 개론 2판 – 김연희 한빛아카데미 인터넷 쇼핑몰 한빛 마트를 위한 데이터베이스 개발

<br>

### 2. 개념적 설계

- 개념적 설계
  -  DBMS에 독립적인 개념적 스키마를 설계하는 과정 
  - 요구 사항 분석 결과를 기반으로 중요한 개체를 추출하고 개체 간의 관계를 결정하여 E-R다이어그램으로 표현 
  - 작업 과정 
    - Step 1 : 개체 추출, 각 개체의 주요 속성과 키 속성 선별 
    - Step 2 : 개체 간의 관계 결정 
    - Step 3 : E-R 다이어그램 작성



#### Step 1: 개체 추출, 각 개체의 주요 속성과 키 속성 선별

- 개체 : 저장할 만한 가치가 있는 중요한 데이터를 가진 사람이나 사물 등 

  예) 병원 데이터베이스 개발에 필요한 개체 

  - 병원 운영에 필요한 사람 : 환자, 의사, 간호사 등 
  - 병원 운영에 필요한 사물 : 병실, 수술실, 의료 장비 등 

- 개체 / 속성 추출 방법 

  - 요구 사항 문장에서 업무와 관련이 깊은 의미 있는 명사를 찾는다
  - 찾아낸 명사를 개체와 속성으로 분류

![image](https://user-images.githubusercontent.com/79521972/176434673-a8b2ebcc-520a-45ed-bb8e-3bbb1600a951.png)



<br>

#### Step 2 : 관계 추출

- 관계 (Relationship) : 개체 간의 의미 있는 연관성 
- 관계 추출 방법 
  - 요구 사항 문장에서 개체 간의 연관성을 의미 있게 표현한 동사를 찾는다 
  - 찾아낸 관계에 대해 매핑 Cardinality 와 참여 특성을 결정 
    - 매핑 Cardinality : 1:1, 1 : n, m : n 
    - 참여 특성 : total participation / partial participation

![image](https://user-images.githubusercontent.com/79521972/176434944-319ad6b2-84e4-4252-b28d-cb2a586eb8f7.png)

- 주문 
  - 개체 : 회원 (선택), 상품 (선택) 
  - Cardinality : m : n 
  - 속성 : 주문번호, 주문수량, 배송지, 주문일자 
- 공급 
  - 개체 : 상품 (필수), 제조업체(선택) 
  - Cardinality : 1: n 
  - 속성 : 공급일자, 공급량  
- 작성 
  - 개체 : 회원(선택), 게시글 (필수) 
  - Cardinality : 1 : n

![image](https://user-images.githubusercontent.com/79521972/176435167-d29244e4-f728-47b0-ba90-387ae0a87f3a.png)

<br>

## 2. E-R 다이어그램

### ER 모델(Entity Relation Model)

- ER 모델 (Entity Relation Model)?
  - 요구 사항에서 얻어 낸 정보 등을 개체 (Entity), 속성 (Attribute), 관계성 (Relation)으로 기술하는 데이터 모델 
  - Peter Chen 이 2012년 제안 ( “The Entity-Relationship Model - Toward a Unified View of Data”) 
  - Chen notation
    - ![image](https://user-images.githubusercontent.com/79521972/176435406-b3aaed56-618d-46fc-a61c-658d739699af.png)
  - Crow’s Foot (까마귀 발) Notation
    - ![image](https://user-images.githubusercontent.com/79521972/176435434-2755a893-34d4-48d2-aacc-2f8ca17887e2.png)



<br>

### 개체 (Entity)

- 저장할 만한 가치가 있는 중요한 데이터를 가진 사람이나 사물 등 
- 개체들의 집합을 Entity Type이라 하고 개체 이름을 포함하는 네모로 표현 
- Entity 

![image](https://user-images.githubusercontent.com/79521972/176435636-56de2817-9fbf-49bd-83bb-c42be41071d6.png)

- Weak Entity 
  - 포함된 속성만으로 고유하게 식별할 수 없는 개체 
  - Weak Entity는 다른 소유하는 Entity에 의존하는 Entity  
  - Order item은 Order 항목에 종속적이고 order없이는 존재할 수 없음

![image](https://user-images.githubusercontent.com/79521972/176435670-1a9ec23f-5030-4a57-9430-6084aa3a9e41.png)

<br>

### Attribute (속성)

- Entity (개체), Relationship (관계) 그리고 Attribute의 속성 또는 특성을 나타냄 
- Attribute는 이름을 포함한 원으로 표현됨 
- Key Attribute 
  - 특정 Entity를 식별할 수 있는 고유한 값을 가진 Attribute 
  - 이름에 underscore 
- Multivalued attribute 
  - 하나의 Attribute가 여러 개의 값을 가질 수 있는 Attribute  
  - 두 개의 원으로 표현

![image](https://user-images.githubusercontent.com/79521972/176436185-b62daa61-4489-490d-9602-7b59017b1e68.png)

- Derived Attribute 
  - 다른 Attribute 들로 부터 계산되어져 나온 Attribute 
  - 점선으로 된 원으로 표현 
- Composite Attribute (복합 속성) 
  - 독립적인 Attribute들이 모여서 생성된 하나의 Attribute 
  - Attribute가 Attribute를 가지는 것으로 표현

![image](https://user-images.githubusercontent.com/79521972/176436239-58b63f71-130a-4d38-817d-bad33063c786.png)



<br>

### Relationship (관계)

- Entity간의 어떤 상호작용을 하는지 표현하는 notation 
- Relationship 이름을 포함한 다이아몬드 (마름모)로 표현 
- Relationship도 Attribute (속성)을 가질 수 있다

- Weak (identifying) relationship 
  - Weak entity 와 parent-child 관계에서의 상호작용을 표현

![image](https://user-images.githubusercontent.com/79521972/176436478-323c453a-3b8b-4d76-b8b2-e738f05a0c01.png)

<br>

### Cardinality (Degree of Relationship)

- 관계를 맺는 두 Entity 대해 한 개체가 얼마나 많은 다른 개체와 관련된 수 있는 지 나타내는 표현 
  - ”1”, “N”, ”M”으로 나타냄 
- one to one (1 : 1) 
- one to many (1 : N) 
- many to one (N : 1)  
- many to many (M : N) 

![image](https://user-images.githubusercontent.com/79521972/176436675-77dc51c0-7de5-4e7a-bd6e-a6aa88d460d0.png)

- Entity set이 Relationship에 전체로 참여하는 부분적으로 참여하는지 표현하는 notation 
- Total participation 
  - 모든 entity가 relationship에 참여 
  - Double line으로 표현 
- Partial participation 
  - Entity set에서 일부부만 relationship에 참여할 때 
  - Single line으로 표현

![image](https://user-images.githubusercontent.com/79521972/176436788-f22fe45d-fce4-4ba0-a1e7-700040d21574.png)

<br>

## 3. 릴레이션 스키마 변환

### 논리적 설계

- **논리적 설계 (논리적 모델링 or 데이터 모델링)**
  - 관계 모델 ( Relational model)을 통해 개념적 모델을 논리적으로 표현 
  - 개념적 (Conceptual) 스키마 -> 논리적(Relational) 스키마 
  - ER다이어그램 과 관계 데이터 모델의 차이점 
    - ER 모델은 Entity (개체) 와 Relationship(관계)를 구분하지만 관계 데이터 모델은 모두 릴레이션으로 표현 
    - ER 모델은 Multivalued Attribute, Composite Attribute를 표현하지만 관계 데이터 모델을 허용 하지 않음 
    - ER 모델의 Cardinality, participation constraint 도 관계 데이터 모델에서는 그에 맞는 표현으로 변경이 필요

- **릴레이션 스키마 변환 규칙** 
  - 규칙 1 : 모든 개체는 릴레이션으로 변환한다 
  - 규칙 2 : 다대다 (n:m) 관계는 릴레이션으로 변환한다 
  - 규칙 3 : 일대다 (1:n) 관계는 외래키(foreign key) 로 표현한다 
  - 규칙 4 : 일대일 (1:1) 관계는 외래키 (foreign key)로 표현한다 
  - 규칙 5 : Multivalued Attribute (다중 값 속성)은 릴레이션으로 변환한다



<br>

### 릴레이션 스키마 변환 규칙

#### 규칙 1: 모든 Entity (개체)는 릴레이션으로 변환한다

- Entity name -> Relation name 
- Entity attribute -> Relation attribute 
- Entity key attribute -> Relation primary key 
- Compose attribute (복합속성) 이 있는 경우는 Compose attribute의 attribute를 Relation의 속성(attribute)로 변경

![image](https://user-images.githubusercontent.com/79521972/176437447-330cf0db-6fdb-4ea9-8a20-ae805089760b.png)

<br>

#### 규칙 2: 다대다(n:m) Relationship(관계)는 릴레이션으로 변환한다

- Relationship name -> Relation name 
- Relationship attribute -> Relation attribute 
- 관계에 참여하는 기본 키를 관계 릴레이션에 포함시키고 외래키로 지정한다 (같은 이름이 이미 존재하면 이름 변경)

![image](https://user-images.githubusercontent.com/79521972/176437604-bb782541-c9e4-4b5d-a77f-5df1ad0a5bcc.png)

<br>

#### 규칙 3: 일대다(1:n) Relationship(관계)는 외래키로 표현한다

- 1:n 관계에서 1측 Entity 릴레이션의 기본키를 n측 Entity 릴레이션에 포함시켜 외래키로 지정 
- Relationship(관계) 속성 (Attribute)들도 n 측 릴레이션에 포함시킨다

![image](https://user-images.githubusercontent.com/79521972/176437744-022ab189-a296-46f8-b539-1e1a5846602a.png)

- Weak Entity – Weak Relationship가 참여하는 일대다 관계는 parent entity의 기본키인 외래키 attribute를 포함해서 기본키를 지정한다

![image](https://user-images.githubusercontent.com/79521972/176437827-551a1b6b-a154-43c5-8872-50ea2a1b82ae.png)

<br>

#### 규칙 4: 일대일 (1:1) 관계는 외래키로 표현한다

- 일대일 (1:1) 관계도 일대다(1:n)과 마찬가지로 릴레이션이 아닌 외래키로 표현한다 
- 데이터 중복을 피하기 위해 참여 특성에 따라 다르게 처리한다 
- 규칙 4-1 : 일반적인 일대일 관계는 외래키를 서로 주고받는다 
- 규칙 4-2 : 일대일 관계에 필수적으로 참여하는 개체가 있는 경우, 필수 참여 개체의 릴레이션만 외래키를 받는다 
- 규칙 4-3 : 모든 개체가 일대일 관계에 필수적으로 참여하면 릴레이션 하나로 합친다



<br>

#### 규칙 4-1: 일반적인 일대일 관계는 외래키를 서로 주고받는다

- 관계에 참여하는 개체 릴레이션들이 서로의 기본키를 외래키로 지정 
- 관계의 속성들도 모든 개체 릴레이션에 포함 
- 데이터 중복 발생 가능

![image](https://user-images.githubusercontent.com/79521972/176438134-26d69ec2-8cf3-4406-b765-08e5a1cdc2dd.png)

<br>

#### 규칙 4-2: 필수적으로 참여하는 개체 릴레이션만 외래키를 받는다

-  일대일 관계에 필수적으로 참여하는 개체가 있는 경우 필수 참여 개체의 릴레이션만 외래키를 포함시킴 
- 관계의 속성들은 필수적으로 참여하는 개체의 릴레이션에 포함

![image](https://user-images.githubusercontent.com/79521972/176438299-d95ba7a7-f35a-4ac8-ba81-321bd978a084.png)



<br>

#### 규칙 4-3: 모든 개체가 필수적으로 참여하면 릴레이션 하나로 합친다

- 관계에 참여하는 개체 릴레이션들을 하나의 릴레이션으로 합친다 
- 관계 이름을 릴레이션 이름으로 사용, 참여하는 두 개체의 속성들을 다 포함시킨다 
- 두 개체 릴레이션의 키 속성을 조합하여 기본키로 지정 관계의 속성들은 필수적으로 참여하는 개체의 릴레이션에 포함

![image](https://user-images.githubusercontent.com/79521972/176438421-600d14a8-6980-4ada-8f44-d3b9d9772acd.png)



<br>

#### 규칙 5: Multivalued Attribute(다중 값 속성)은 릴레이션으로 변환한다

- 다중 값 속성과 그 속성을 가지고 있던 개체의 기본키를 외래키로 포함 시킨다 
- 다중 값 속성으로 만들어진 새로운 릴레이션의 기본키는 다중값 속성과 외래키를 조합하여 지정

![image](https://user-images.githubusercontent.com/79521972/176438568-f74227ac-a316-4092-93a4-6543717f44f0.png)

<br>

## 4. 릴레이션 스키마 변환 예제

### 한빛 마트 E-R다이어그램 예제

![image](https://user-images.githubusercontent.com/79521972/176438874-97a65967-b12d-4f1b-b94f-ff95f7eef770.png)

![image](https://user-images.githubusercontent.com/79521972/176438926-1e0fab0b-cdd5-48ad-aac1-99ee6cda881b.png)
![image](https://user-images.githubusercontent.com/79521972/176438951-1e07c04f-7dc6-4839-906e-5820ae7ffa4c.png)
![image](https://user-images.githubusercontent.com/79521972/176438991-a3b95866-1d5a-4c1f-9558-9de36ba34c76.png)

