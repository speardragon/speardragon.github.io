---
layout: single
title: "[Algorithm] CH2. 리스트 - ArrayList"
categories: ['Lecture', 'Data structure and algorithms']
tag: ['Data structure', 'Algorithm', '리스트', 'ArrayList', 'LinkedList']
toc: true
toc_sticky: true
---

<br>

# List

리스트는 선형적인 자료구조로서 데이터를 일렬로 늘여 놓은 형태입니다. 이 때문에 리스트는 순서를 가진다는 특징이 있다.

리스트를 관리를 할 때 중요한 기능 3가지가 있습니다.

- 데이터 삽입하기
- 데이터 삭제하기
- 리스트 탐색하기

이 3가지는 리스트를 다룰 때 중요한 요소이기 때문에 이것들에 초점을 맞춰서 앞으로 진행을 해 보도록 하겠습니다.



### 리스트의 종류

- ArrayList
- LinkedList
  - Single Linked List
  - Double Linked List





## ArrayList

배열 기반의 리스트로 메모리 공간을 연속적으로 사용합니다. 배열 기반의 구조이기 때문에 배열과 동일한 인덱스를 사용하고 인덱스를 이용한 데이터 접근은 배열의 크기가 아무리 크더라도 인덱스로 접근을 하게되면 값을 찾는데 항상 동일한 시간이 걸립니다.

<img src="https://user-images.githubusercontent.com/79521972/152724760-07a4a709-c02e-4237-a2a3-f073b3e47fdb.png" alt="image" style="zoom:80%;" />

ArrayList는 컴퓨터의 실제 메모리 공간에서도 연속적인 공간를 사용하고 있기 때문에 컴퓨터가 연산하기 쉬운 구조로 되어있습니다. 그렇기 때문에 이전 인덱스에서 다음 인덱스로 접근하는 속도가 매우 빠릅니다.

반면 LinkedList는 뒤에서 더 설명하겠지만 데이터를 논리적으로 연결시켜서 마치 연결된 것처럼 사용할 수는 있지만 실제 컴퓨터 메모리 공간 상에서는 연속적인 공간을 차지하고 있는 것이 아니기 때문에 데이터 간의 이동 시간 자체는 ArrayList보다 느릴 수 있습니다.





### 추가

![image](https://user-images.githubusercontent.com/79521972/152729616-ac832bb7-5d7d-4c22-872d-89cfcf155548.png)

이 그림은 크기가 7인 ArrayList를 시각화 한 모습입니다. 현재 0번부터 2번 index까지 데이터가 자리를 차지하고 있는 모습입니다.



![image](https://user-images.githubusercontent.com/79521972/152729639-3efe5dcd-8504-4b67-a66a-4b307d876354.png)

위 그림과 같이 가장 끝까지 채워져 있던 2번 index의 바로 옆 index인 3번 index에 데이터가 추가됩니다.

따라서 리스트에서 데이터의 추가라는 것은 리스트의 가장 마지막으로 채워진 index 바로 뒤에 데이터를 삽입하는 것을 말합니다.

### 삽입

![image](https://user-images.githubusercontent.com/79521972/152729809-d9f2c3ac-0401-4ae2-91f5-6f5559f56ef6.png)

추가와 달리 데이터의 삽입은 데이터를 맨 뒤에 추가하는 개념이 아닌 내가 원하는 위치에 데이터가 **이미** 들어가 있더라도 해당 index의 데이터와 그 뒤의 데이터는 모두 뒤로 밀고 그 자리에 데이터가 삽입되는 형태를 말합니다.

<br>

![image](https://user-images.githubusercontent.com/79521972/152729740-94b1800e-3d57-45dd-acd6-6d000b6d8016.png)

데이터를 한칸씩 밀어주는 과정에서 만약 데이터가 매우 많다면 많은 만큼의 시간이 소요가 되므로 O(N)의 시간 복잡도를 가진다고 말할 수 있습니다.

![image](https://user-images.githubusercontent.com/79521972/152729754-d4c10e69-f184-429f-b140-d470a74d4a59.png)

삽입의 경우 추가하고자 하는 index에 데이터를 그냥 넣게 되면 기존에 있던 데이터는 사라지게 되므로 데이터 손실이라는 매우 중요한 결함이 생기게 되므로 이에 유의하여 삽입을 사용해야 합니다.

<br>

### 삭제

index 기반의 데이터 삭제를 할 때에도 데이터를 이동시켜주는 작업이 반드시 필요합니다.

![image](https://user-images.githubusercontent.com/79521972/152730056-a57271a0-d3d2-4a7f-b8d5-a85b800f56b3.png)

만약 2번 index를 삭제하고자 합니다.<br>
<br>

![image](https://user-images.githubusercontent.com/79521972/152730118-a4f91044-38f7-488c-bd12-b40904d3a308.png)

그래서 삭제를 하고 그냥 냅두게 되면 이 ArrayList는 중간이 뻥 뚫리게 되는 구조를 가지게 되므로 이 역시 결함이 생기게 됩니다. 따라서 빈 공간을 뒤의 데이터를 당겨오면서 메꿔 주어야 합니다.

![image](https://user-images.githubusercontent.com/79521972/152730447-08e78c65-6f60-4aae-9bda-ae8dd7308aa4.png)

그 결과 그림처럼 빈 공간 없이 List에 데이터가 나열된 모습을 보실 수 있습니다.



### 탐색 by index

마지막으로 index에 의한 탐색 과정입니다. ArrayList는 Random access로 해당 index에 직접 접근하게 됩니다. 데이터가 아무리 많아져도 해당 index에 접근하는 것이기 때문에 이에 대한 탐색 속도는 매우 빠르고 이렇듯 데이터 갯수와는 상관없이 소요시간이 일정하다면 이런 경우에는 **O(1)**의 시간 복잡도를 갖습니다.

![image](https://user-images.githubusercontent.com/79521972/152732341-48855b47-17e4-4eb8-a12b-ac910139d8a8.png)





## List 실습









## LinkedList

(single) linked list는 **Node**라는 객체로 구성이 되어있는데 이는 데이터를 저장할 수 있는 field와 다음 Node를 가리킬 수 있는 next pointer



## Double Linked List

single linked list보다 prev 노드를 추가했기 때문에 안 좋은 점이 있음에도 사용하는 이유?

