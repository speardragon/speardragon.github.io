---
layout: single
title: "[Algorithm] CH7. Tree(트리)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Tree']
tag: ['Data structure', 'Algorithm', 'Tree', '트리', 'Binary Search Tree']
toc: false
toc_sticky: true
---

<br>

# Tree - 트리

이번 시간에는 트리 구조에 대해서 배워보도록 하겠습니다.

> ### 개요

트리는 한 노드가 여러 노드를 가르킬 수 있는 비선형적 자료구조를 일컫는 말입니다. 앞에서 배웠던 리스트(List), 스택(Stack), 큐(Queue)는 이전 데이터와 다음 데이터 간의 순서가 존재했었습니다. 트리도 물론 내부적으로 순서 정보를 가지도록 구현할 수도 있지만 트리 구조 자체의 특성 상 순서는 그렇게 중요하지 않습니다.

그래서 트리는 뒤에서 배우게 될 그래프의 일종이며 데이터 구조의 상하 개념 계층 구조의 속성을 표현한다는 특징도 있습니다.

우리가 자주 쓰는 컴퓨터의 파일, 폴더의 계층 구조가 트리 구조라고 할 수 있습니다.

![image](https://user-images.githubusercontent.com/79521972/153970439-9db53747-cec3-439c-9525-ba08323e1d84.png)



> ### 관련 용어

트리의 구조를 살펴보면서 이와 관련된 용어에 대해 정리를 해 보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/154190733-666e345d-cebe-4b07-ad6a-7420993d6053.png)

- Node: 트리 구조에서 각 구성요소를 의미하는 단위를 Node라고 합니다.
  위 그림에서는 A~J까지를 모두 Node라고 할 수 있습니다.
- Root Node: 트리의 시작 노드로 부모가 없으며 위 그림에서 A노드를 뜻합니다.
  - 트리는 최대 1개의 Root Node를 가질 수 있습니다.
- Edge: 노드와 노드 간의 연결을 하는 선을 의미합니다.
- Path: 특정 노드에서 노드까지의 경로를 의미하며 한 번 지나쳤던 경로를 다시 지나는 것은 허락하지 않습니다.
- Terminal Node(Leaf Node): 자식 노드가 존재하지 않는, 즉 다시 말해 밑으로 또 다른 노드가 연결 되어있지 않는 노드를 의미하며, 위 그림에서 H, I, J, F, G와 같은 노드를 말합니다.
- Sub-Tree: 전체 큰 트리 구조 안의 작은 트리 구조를 말합니다.
  - 트리의 재귀적인 특성을 보여줍니다.
- Depth: 부모 노드로부터 얼마나 떨어져 있는 지를 뜻하는 단위로 부모 노드의 바로 아래 노드는 depth 1을 가집니다.
- Level: 트리 구조에서 같은 위치, 즉 같은 depth를 가지는 노드들을 한 레벨로 나타내는 단위입니다. Root Node가 기준이기 때문에 level 0을 갖습니다.
- Height: 트리에서 가장 최고 레벨, 가장 깊은 층을 말합니다.

상위 노드를 부모 노드라 하며 부모 노드에서 파생된 노드를 자식노드라 합니다. 
이때, 같은 부모를 가진 노드를 형제 관계에 있다고 하며 자식 수를 차 수로 나타내어 degree라고도 합니다.

예를 들어, Node D는 H, I 노드의 부모(Parent) 노드이며 degree 2의 차 수를 갖습니다.

<br>

또한 아래의 그림과 같은 트리구조는 노드의 연결이 하나씩만 되어있는 트리로 경사 트리라는 것도 존재합니다.

![image](https://user-images.githubusercontent.com/79521972/154193740-b3587214-789d-4b8f-91b6-8a6a5dbfe4eb.png)

<br>

## 이진 트리

자 그럼 트리 구조 중에서 이진 트리에 대해 배워 보도록 하겠습니다.

<br>

- 각 노드가 최대 2개(0-2)의 자식 노드를 가지는 트리를 의미합니다.

  최대 2개라는 것은 자식이 없을 수도(0개) 있고 1개만 있을 수도 있다는 것입니다.

  - 자식 노드에는 왼쪽 자식 노드와 오른쪽 자식노드가 있으며 이 둘은 엄연히 다른 노드입니다.

![image](https://user-images.githubusercontent.com/79521972/154196285-fc0ccd70-7b94-410f-88fb-920baa5b34bc.png)

#### **정 이진 트리(full binary tree) or 엄격한 (strict) 이진 트리**

: 모든 노드가 2개의 자식을 가지는 트리를 의미합니다. (즉 자식 노드가 1개인 노드가 없어야 만족합니다.)

- 왼쪽 그림은 자식 노드가 하나인 노드가 존재하기 때문에 정 이진 트리가 될 수 없고 오른쪽 그림은 자식 노드가 하나인 노드가 존재하지 않기 때문에 정 이진 트리라고 할 수 있습니다.

- ![image](https://user-images.githubusercontent.com/79521972/154196254-cceb69f0-ac2a-4dce-9ab2-2a6d60df217f.png)



<br>

#### 포화 이진 트리(Perfect Binary tree)

: 모든 노드가 2개의 자식을 가지고 leaf 노드가 모두 같은 레벨일 때를 말합니다.

- 높이가 h일 때 노드 갯수는 2<sup>k+1</sup> - 1 개를 가진다는 특징있습니다.
- Leaf 노드의 갯수는 2<sup>h</sup> 개가 됩니다.



![image](https://user-images.githubusercontent.com/79521972/154196042-8f7a8dd2-f96c-4c4d-9211-545da52f2388.png)



<br>



#### 완전 이진 트리(Complete Binary Tree)

- 마지막 레벨을 제외하고 모든 노드가 채워진 트리 구조입니다.

- 노드는 왼쪽에서 오른쪽으로 채워져야합니다.

위 두 조건이 만족할 때 완전 이진 트리라고 합니다.

아래 그림에서 왼쪽과 오른쪽 그림 모두 완전 이진 트리라고 할 수 있습니다. 만약 왼쪽그림에서 level 1의 오른쪽 노드의 자식 노드가 오른쪽 자식 노드만 있었다면 완전 이진 트리가 될 수 없는 것입니다.

![image](https://user-images.githubusercontent.com/79521972/154196589-a8dc629a-b5d3-4f0e-8b4b-550db26f6172.png)



> 포화 이진 트리는 완전 이진 트리의 조건을 모두 만족하므로 포화 이진 트리는 완전 이진 트리에 속한다고 볼 수 있고 이의 역은 무조건 성립하는 것은 아닙니다.

아래 그림은 완전 이진 트리가 될 수 없는 구조를 보여주고 있습니다.

![image](https://user-images.githubusercontent.com/79521972/154196894-d228f359-164b-4fbe-83cd-faefba3d19f2.png)



### 1차원 배열로 표현하는 이진 트리

그래서 트리는 선형 구조인 1차원 배열로도 표현이 가능합니다.

아래의 그림을 살펴봅시다.

![image](https://user-images.githubusercontent.com/79521972/154196965-ddf199d8-af87-4a97-be56-de15a5661094.png)

왼쪽 트리는 level 순, 그리고 왼쪽에서 오른쪽 순서로 각 노드에 index를 붙여 표현가능합니다. 

따라서 완전 이진 트리는 위의 그림과 같이 배열을 빈틈없이 모두 채울 수 있습니다.



더 쉽게 이해 되도록 그림을 추가하였습니다.

![image](https://user-images.githubusercontent.com/79521972/154232616-916fe9f1-b8a4-4eda-94f0-e22646ca6a3e.png)



<br>

반면 중간이 비어있는 트리의 경우 배열 사이에 null 값이 들어간 구조로 나타나게 됩니다.

![image](https://user-images.githubusercontent.com/79521972/154197127-93fbfe92-c857-44a7-8bbd-4e4306df0dc4.png)

한 가지 신기한 점은 0번 index는 비우고 1번 index 부터 Root Node를 채운 점인데요. 이는 이를 이용하여 탐색을 할 수 있기 때문입니다.

<br>

다음 표를 보면서 생각해 봅시다.

| **루트 노드**            | **1**         |
| ------------------------ | ------------- |
| **노드 i의 부모**        | **i / 2**     |
| **노드 i의 왼쪽 자식**   | **i * 2**     |
| **노드 i의 오른쪽 자식** | **i * 2 + 1** |

위 표와 같은 연산을 통해 쉽게 특정 노드의 index를 찾을 수 있습니다.

- 6의 index를 찾도록 해 봅시다.
  - 6은 노드 3의 왼쪽 자식 노드입니다.
  - 표의 3번째 식을 통해 3 * 2 = 6.
  - 따라서 6의 index는 6입니다.

그래서 이진 트리를 구현함에 있어서 노드를 사용할텐데 이때 left 노드와 right노드를 통해 구현을 하도록 할 것입니다. 

```java
class Node {
    int data;
    Node left;
    Node right;
}
```



### 이진 트리의 응용

- 힙
- 이진 탐색 트리 (Binary Search Tree)
- B-Tree
- AVL 트리



바로 다음 챕터에서 배울 힙(Heap) 구조라는 것도 트리의 한 가지이고
 Binary Search Tree 줄여서 BST라고 하는 것도 이진 트리 구조입니다.
또한 B-Tree라는 것은 이진 트리의 응용으로 데이터 베이스나 파일 시스템에 사용되는 중요한 구조입니다.

<br>

### 이진 트리의 기본 연산

트리도 자료 구조이기 때문에 앞에서 배운 다른 구조들과 동일하게 다음과 같은 연산이 가능합니다.

- 트리에 데이터 삽입하기
- 데이터 삭제하기
- 데이터 검색하기
- <u>트리 탐색</u>하기

앞에서 배웠던 선형 자료구조들은 앞에서 부터 데이터를 탐색하는 것이 그렇게 어렵지 않은 작업이었는데 연결 관계로 표현되는 구조는 그것들의 특성상 탐색하는 방식이 선형 구조와 다를 수 밖에 없습니다.

그래서 트리 구조의 탐색은 더욱 신경써서 다뤄 보도록 하겠습니다.

<br>

## 트리 순회

트리 순회에 대해서 배워보도록 하겠습니다.

- **트리 순회란?**

트리 구조에서 각 노드를 한 번씩 방문하는 과정입니다.



- **트리 순회 방법**

1. 전위 탐색 preorder
2. 중위 탐색 inorder
3. 후위 탐색 postorder

노드 방문을 언제 하느냐에 따라 순회 방법이 나뉘어집니다.

<br>

#### 1. Preorder

- 이름 그대로 노드를 가장 먼저 방문하는 방법입니다.

- 이후 <u>왼쪽</u> 서브 트리를 preoder하고 (재귀 호출로), 

- <u>오른쪽</u> 서브 트리를 preorder하게 됩니다.

<br>



#### 2. inorder

- 먼저 <u>왼쪽</u> 서브 트리를 inorder합니다. 

- 그 다음에 (루트)노드를 방문하고

- <u>오른쪽</u> 서브 트리를 inorder(재귀적으로)

  하는 방식입니다.

<br>

#### 3. postorder

- 먼저 <u>왼쪽</u> 서브 트리를 postorder합니다. 
- <u>오른쪽</u> 서브 트리를 postorder합니다.

- 마지막으로 (루트)노드를 방문합니다.



<br>

자 그럼 그림을 통해 전, 중, 후위 탐색이 어떻게 이루어 지는 지 보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/154202666-9f5ecd7d-64c4-459f-b9ec-f6eec9708214.png)

<br>

**preorder**

![1_VDapzFYMTK0dI5kHNnrZxg](https://user-images.githubusercontent.com/79521972/154231791-cdb5a452-1c7a-44a4-b7b2-322c6eb55c03.gif)

먼저 전위탐색은 각 노드가 루트 노드 - 왼쪽 서브 트리 - 오른쪽 서브 트리 순으로 preorder를 진행합니다.

순서가 

1. 노드 방문
2. 왼쪽 서브 트리 preorder
3. 오른쪽 서브 트리 preorder

가 되며 이것이 모두 만족해야 해당 노드의 preorder가 끝난 것임을 유의합시다.

<img src="https://user-images.githubusercontent.com/79521972/154202666-9f5ecd7d-64c4-459f-b9ec-f6eec9708214.png" alt="image" style="zoom:50%;" />

자 그럼 루트 노드인 A노드부터 시작하겠습니다.

- A 노드 방문(A의 1 만족 - A<sub>1</sub>)
  - 왼쪽 서브 트리인 B 노드 방문(B<sub>1</sub>, A<sub>1</sub>)
    - 왼쪽 서브 트리인 D 노드 방문 (D<sub>1</sub>, B<sub>12</sub>)
    - D노드는 왼쪽 노드가 없고 오른쪽 노드만 있기 때문에 H 노드 방문(H<sub>1</sub>, D<sub>123</sub>)
    - H노드는 leaf노드이기 때문에 preorder가 완료되었고 이에 따라 D노드도 자동적으로 preorder가 완료됩니다. (H<sub>123</sub>)
    - D 노드는 B노드의 왼쪽 노드이었기에 그 다음은 B의 3번 조건을 만족 시키기 위해 오른쪽 노드인 E노드에 방문합니다.(B<sub>123</sub>)

이러한 방식으로 모든 노드가 preorder가 될 때까지 반복합니다.

이 Path(노드 방문 순서)를 나타내면 다음과 같습니다.

> A - B - D - H - E- C- F - I - J - G - K

<br>



**inorder**

![1_hbJOJjABy16aJzbhfPfpQQ](https://user-images.githubusercontent.com/79521972/154232049-46edb3b8-ac61-47ac-be99-5871dfa75ca5.gif)

중위 탐색은 각 노드가 왼쪽 서브 트리 - 루트 노드  - 오른쪽 서브 트리 순으로 preorder를 진행합니다.

순서가 

1. 왼쪽 서브 트리 inorder
2. 노드 방문
3. 오른쪽 서브 트리 inorder

가 되며 이것이 모두 만족해야 해당 노드의 inorder가 끝난 것임을 유의합시다.

<img src="https://user-images.githubusercontent.com/79521972/154202666-9f5ecd7d-64c4-459f-b9ec-f6eec9708214.png" alt="image" style="zoom:50%;" />

중위 탐색에서는 Root Node인 A에서 시작했을 때 1번 조건이 노드 방문이 아니므로 왼쪽 서브 트리를 탐색하면서 시작합니다. 이 과정으로 inoder를 진행하며 거쳐가는 노드 path(노드 방문 순서)는 다음과 같습니다.

>  D- H - B- E - A - I - F - J - C - G - K

<br>



**postorder**

![1_NqVMFGOioTCqJdkJYUOCkw](https://user-images.githubusercontent.com/79521972/154232093-ccfe92c0-17f7-4776-952c-4fab8d08ad3f.gif)

중위 탐색은 각 노드가 왼쪽 서브 트리 - 루트 노드  - 오른쪽 서브 트리 순으로 preorder를 진행합니다.

순서가 

1. 왼쪽 서브 트리 postorder
2. 오른쪽 서브 트리 postorder
3. 노드 방문

가 되며 이것이 모두 만족해야 해당 노드의 postorder가 끝난 것임을 유의합시다.

<img src="https://user-images.githubusercontent.com/79521972/154202666-9f5ecd7d-64c4-459f-b9ec-f6eec9708214.png" alt="image" style="zoom:50%;" />

후위 탐색에서는 Root Node인 A에서 시작했을 때 1번 조건이 왼쪽 서브 트리를 탐색하면서 시작합니다. 그 후 이 과정으로 재귀 호출로 계속 반복하며 postorder를 진행하며 거쳐가는 노드 path(노드 방문 순서)는 다음과 같습니다.

> H- D - E - B - I - J - F - K - G - C - A



| pre-order                                    | In-order                                    |
| -------------------------------------------- | ------------------------------------------- |
| **A - B - D - H - E- C- F - I - J - G - K**  | **D- H - B- E - A - I - F - J - C - G - K** |
| **Post-order**                               |                                             |
| **H- D - E - B - I - J - F - K - G - C - A** |                                             |

전위 탐색에서는 루트 노드를 가장 먼저 방문하였고

중위 탐색에서는 루트 노드를 중간에(루트 노드의 왼쪽 노드들을 모두 방문한 후에)방문하였으며

후위 탐색에서는 루트 노드를 가장 마지막에 방문한 것을 알 수 있습니다.

<br>



## 이진 탐색 트리(BST)

이번엔 이진 탐색 트리에 대해서 배워보겠습니다.

트리 구조 자체적으로는 데이터의 특성에 아무런 제약이 걸리지 않습니다. 그렇기 때문에 트리 노드를 하나하나씩 방문하여 탐색해야 하기 때문에 시간 복잡도 측면에서 큰 이점을 얻지 못합니다.

이에 대하여 더 빠른 탐색을 위해 데이터의 특성에 제약을 줌으로써 Binary Search와 마찬가지로 O(logN)의 시간 복잡도를 가지며 나온 것이 이진 탐색 트리라고 할 수 있습니다.



> 특징

- 이진 탐색 트리도 **이진 트리** 구조로 이루어져 있습니다.
- 노드의 왼쪽 서브 트리에는 루트 노드보다 작은 값이 들어있습니다.
- 노드의 오른쪽 서브 트리에는 루트 노드보다 큰 값이 들어있습니다.
- 서브 트리는 다시 이진 탐색 트리 구조 입니다.
- 중복된 값은 일체 없습니다.



<br>

다음 그림은 이진 탐색 트리 구조입니다.

![image](https://user-images.githubusercontent.com/79521972/154207993-3fe6d49f-b017-4efd-b595-9cd6f8539ca1.png)

루트 노드인 5를 기준으로 왼쪽 서브 트리에는 5보다 작은 수로 구성 되어있고 오른쪽 서브 트리에는 5보다 큰 수로 구성 되어있습니다.

![image-20220216152418963](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220216152418963.png)

연속적으로 하위의 서브 트리를 봐도 각 루트 노드를 기준으로 오른쪽 왼쪽이 작은 수와 큰 수로 나뉘어져 있는 것을 보실 수 있습니다.

<br>

**트리의 최솟값**

![image](https://user-images.githubusercontent.com/79521972/154208294-0109e47f-756f-4c16-9b6b-53b97744578f.png)

트리의 최솟값은 위의 그림처럼 트리 구조의 가장 왼쪽 끝에 위치한 노드가 최솟값이 됩니다. 만약 1의 왼쪽 서브 노드가 있다면 해당 노드가 최솟값이 되겠죠?

<br>

**트리의 최댓값**

![image](https://user-images.githubusercontent.com/79521972/154208534-83d3967e-5d10-4d75-9661-7bfb589f99e0.png)

반대로 트리의 최댓값은 위의 그림처럼 트리 구조의 가장 오른쪽 끝에 위치한 노드가 최댓값이 됩니다. 만약 9의 오른쪽 서브 노드가 있다면 해당 노드가 최댓값이 됩니다.



다음과 같은 이진 탐색 트리가 있다고 합시다.

![image](https://user-images.githubusercontent.com/79521972/154208639-2a2aa982-6769-4556-9dec-a832e924504f.png)

이 트리 구조를 중위 탐색 하면 오름 차순으로 숫자를 가져올 수 있게 됩니다.

- 중위 탐색
  - 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> 8 -> 9



### 이진 탐색 트리의 삽입

이진 탐색 트리에서 삽입은

- 가장 먼저 해당 데이터가 자신이 들어갈 위치를 찾아야 합니다.

- 그리고 중복된 데이터는 삽입하지 않습니다.
- 그렇기 때문에 만약 숫자를 삽입하려 하는데 탐색을 하다가 같은 숫자가 있는 것을 발견하면 삽입하지 않고 그대로 종료하게 됩니다.

- 데이터를 삽입하거나 삭제하더라도 그 특성이 유지되어야 합니다.
- 추가된 노드는 트리의 leaf에 삽입을 합니다.
  - 자식 노드가 3개를 넘으면 안 되기 때문입니다.

<br>

![image](https://user-images.githubusercontent.com/79521972/154210855-e3f2c8cd-b2be-4b87-b033-7add75184515.png)

만약, 1이라는 데이터를 삽입하고자 합니다.

1. 그럼 먼저 루트 노드인 8과 비교하여 더 작기 때문에 왼쪽 노드인 5와 비교합니다.
2.  5와 비교했을 때도 더 작기 때문에 또 왼쪽 노드로 가 2와 비교합니다.
3. 2와 비교했을 때 더 작은데 왼쪽 노드가 비어있기 때문에 2의 왼쪽 노드로 삽입합니다.





## 이진 탐색 트리의 삭제

삽입은 leaf노드의 위치만 잘 찾아서 삽입만 하면 되는 삭제는 좀 더 까다롭습니다. leaf노드만 삭제 되는 것이 아니라 트리 구조 중간에 위치한 노드도 삭제 될 수 있기 때문이죠.

삭제 과정을 한 번 보겠습니다.

- 삭제 데이터의 위치를 찾음
  1. 삭제할 데이터가 leaf인 경우
  2. 한 개의 자식 노드를 가질 경우
  3. 두 개의 자식 노드를 가질 경우



아래와 같은 이진 탐색 트리가 있다고 합시다.

![image](https://user-images.githubusercontent.com/79521972/154208639-2a2aa982-6769-4556-9dec-a832e924504f.png)

<br>



**1. 삭제 할 데이터가 leaf인 경우**

![image](https://user-images.githubusercontent.com/79521972/154214616-1a7dc68f-b7e5-4972-a53a-508ac54c711b.png)

삭제 할 데이터가 leaf인 경우 그림 상에서 2와 8을 의미합니다. 이때는 null을 부모 노드에게 리턴시켜서 자신을 가리키던 자식 포인터를 null로 바꿔줍니다.

<br>

**2. 한 개의 자식 노드를 가질 경우**

![image](https://user-images.githubusercontent.com/79521972/154215065-28b38d37-c484-4155-b6e8-6045aeef7fab.png)

삭제하고자 하는 노드가 한 개의 자식 노드를 가질 경우에 해당 노드의 자식 노드를 1의 부모 노드였던 3의 자식 노드와 연결 시켜 주면 됩니다. 

그러면 다음과 같이 되어 이진 탐색 트리의 속성이 유지 되는 것이죠.

![image](https://user-images.githubusercontent.com/79521972/154215226-3e96aa6b-7361-4506-8973-660df0003d08.png)

<br>

**3. 두 개의 자식 노드를 가질 경우**

삭제 하고자 하는 노드가 두 개의 자식 노드를 가질 경우에는 두 가지 옵션이 존재합니다.

- 왼쪽 서브 트리의 최댓값과 교체
- 오른쪽 서브 트리의 최솟값과 교체

![image](https://user-images.githubusercontent.com/79521972/154215357-9aa0601a-5c51-4843-bb49-fb892f19944b.png)

저는 두 번째 방법인 오른쪽 서브 트리의 최솟값과 교체하여 보겠습니다.
<br>

8의 오른쪽 서브 트리에는 8과 9가 존재하여 이 중 최댓값인 8과 삭제하고자 하는 노드와 교체를 진행합니다. 

이런 과정을 통해 8이라는 데이터가 중복되기 때문에 8을 지워주어야 합니다.

![image](https://user-images.githubusercontent.com/79521972/154215658-86d9de1e-f57a-427f-946a-060b2946431d.png)

8은 leaf 노드이기 때문에 null을 대입하는 것으로 삭제를 해 주면 다음과 같이 됩니다.

![image](https://user-images.githubusercontent.com/79521972/154215712-df11bee3-edf3-4eae-9651-db6b85cd3fc7.png)

결과적으로 이진 탐색 트리의 구조가 망가지지도 않았고 원하는 노드만 삭제를 했기 때문에 깔끔하게 진행이 된 것을 확인 하실 수 있습니다.



## 이진 탐색 트리 구현

이제는 그럼 코드로 구현해 보겠습니다. 이진 탐색 트리는 오히려 코드로 보면서 이해하는 것이 더 도움이 되실 수 있습니다.

**ITree 인터페이스**

```java
package tree;
 
public interface ITree<T> {
    void insert(T val);
    void delete(T val);
    boolean contains(T val);
    int size();
}
```

트리 구조에 필요한 기능 들을 메소드 포로토타입으로 선언하였습니다.

이외의 preorder, inorder, postorder와 같은 탐색 기능은 따로 트리 클래스에서 구현해 보도록 하겠습니다.

<br>

**BinarySearchTree 클래스**

```java
package tree;

import java.util.List;

public class BinarySearchTree<T extends Comparable<T>> implements ITree<T> {
    
    ...
}
```



### **노드 클래스**

```java
private class Node {
    T data;
    Node left;
    Node right;
    
    Node(T data) { this.data = data;}
    
    Node(T data, Node left, Node right) {
        this.data = data;
        this.left = left;
        this.right = right;
    }
}
```



### **멤버 변수**

루트 노드가 시작이기 때문에 루트 노드를 가리키는 변수와 트리 구조 크기를 나타내는 변수 size를 선언합니다.

```java
private Node root;
private int size;
```

<br>

### **생성자**

```java
public BinarySearchTree() {
    this.root = null;
    this.size = 0;
}
```

<br>

### **min(), minNode()**

트리의 데이터 중 가장 작은 값(최솟값)을 찾아주는 메소드입니다.

```java
public T min() {
    return this.minNode(this.root);
}

private T minNode(Node node) {
    T minData = node.data;
    while (node.left != null) {
        minData = node.left.data;
        node = node.left;
    }
}
```

가장 왼쪽의 값을 가져 오는 것이기 때문에 left노드를 계속 타고 들어가면 됩니다.

**minNode()** :

minData에는 최솟값을 계속해서 업데이트를 해 줄 것입니다. 

반복문을 통해 left노드가 null일 때까지 반복해 줍니다.

- 반복문 안의 내용은 left노드를 계속해서 타고 들어가며, 각 노드의 데이터를 minData변수에 업데이트를 해 줍니다.
  그러면 반복문이 끝났을 때는 더 이상 타고 들어갈 곳이 없는 가장 왼쪽 노드의 값을 가져오게 됩니다.

**min()** :

루트 노드부터 시작하여 타고 내려가기 때문에 인자로 루트 노드를 넘겨 minNode()의 리턴값을 리턴합니다.

<br>

**max(), minMax()**

max() 메소드는 위의 min() 메소드와 다르게 최댓값을 찾는 것이므로 방향만 바꾸어 가장 오른쪽 값을 가져오도록 합니다.

```java
public T max() {
    return this.maxNode(this.root);
}

private T maxNode(Node node) {
    T maxData = node.data;
    while (node.right != null) {
        maxData = node.right.data;
        node = node.right;
    }
}
```



<br>

### **전위 탐색 - preOrder()**

```java
public List<T> preOrder() {
    return this.preorderTree(this.root, new ArrayList<>());
}

private List<T> preorderTree(Node node, List<T> visited) {
    if(node == null) return visited;
    
    visited.add(node.data);
    preorderTree(node.left, visited);
    preorderTree(node.right, visited);
    
    return visited;
}
```

1. 노드 방문
2. 왼쪽 서브 트리 preorder
3. 오른쪽 서브 트리 preorder

전위 탐색의 순서에 맞게 코드를 구현했습니다.

**preorderTree**()

visited는 노드가 방문 했음을 기록하는 리스트 객체이고 이곳에 node의 데이터를 넣어줍니다.

그리고 왼쪽 서브트리에 대해 반복적으로 재귀 호출을 진행하고 완료되면 오른쪽 서브 트리에 대해 반복적으로 재귀 호출이 진행되도록합니다.

그리고 재귀가 종료조건 없이 계속해서 호출되면 오류가 발생할 수 있기 때문에 if문으로 node가 null일 때, 즉 leaf 노드 다음은 노드가 없으므로 return 하여 종료하도록 메소드 가장 앞에 작성해 줍니다.



<br>

### **중위 탐색 - inOrder()**

```java
public List<T> inOrder() {
    return this.inorderTree(this.root, new ArrayList<>());
}

private List<T> inoderTree(Node node, List<T> visited) {
    if(node == null) return visited;
    
    inorderTree(node.left, visted);
    visited.add(node.data);
    inorderTree(node.right, visited);
    
    return visited;
}
```

1. 왼쪽 서브 트리 preorder
2. 노드 방문
3. 오른쪽 서브 트리 preorder

중위 탐색의 순서에 맞게 코드를 구현했습니다.

코드 방식은 앞서 설명한 전위 탐색과 순서만 다르지 굉장히 비슷하기 때문에 자세한 설명은 생략하도록 하겠습니다.



<br>

### **후위 탐색 - postOrder()**

```java
public List<T> postOrder() {
    return this.postorderTree(this.root, new ArrayList<>());
}

private List<T> postoderTree(Node node, List<T> visited) {
    if(node == null) return visited;
    
    postorderTree(node.left, visted);
    postorderTree(node.right, visited);
    visited.add(node.data);
    
    return visited;
}
```

1. 왼쪽 서브 트리 preorder
2. 오른쪽 서브 트리 preorder
3. 노드 방문

중위 탐색의 순서에 맞게 코드를 구현했습니다.

코드 방식은 앞서 설명한 전위 탐색, 중위 탐색과 순서만 다르지 굉장히 비슷하기 때문에 자세한 설명은 생략하도록 하겠습니다.

<br>

### **contains(T val)**

인자로 넘겨준 값이 존재하는 지를 판단하는 메소드로 이 역시도 재귀 호출을 통해 구현해 보도록 하겠습니다. 그러기 위해서 별도로 containsNode() 메소드를 만들어줍니다.

```java
@Override
public boolean contains(T val) {
    return this.containsNode(this.root, val);
}

private boolean containsNode(Node node, T val) {
    if (node == null) return false;
    
    // a.compareTo(b)
    // a < b : -1
    // a == b : 0
    // a > b : 1
    
    //val과 node.data가 같은 경우
    if (val.compareTo(node.data) == 0) { 
        return true;
    }
    
    //val이 node.data보다 작은 경우
    if (val.compareTo(node.data) < 0) {
        return containsNode(node.left, val);
    }
    
    //val이 node.data보다 큰 경우
    return containsNode(node.right, val);
    
}
```

**containsNode()**

루트 노드부터 인자로 받아온 값과 비교하는 연산(compareTo())을 진행하여 일치하면 true를 리턴하고 그렇지 않은 경우는 다음과 같이 진행한다.

- 찾고자 하는 값이 현재 노드의 데이터보다 <u>작은</u> 경우 left노드를 타고 들어가 재귀적으로 계속해서 판단을 진행한다.
- 찾고자 하는 값이 현재 노드의 데이터 보다 <u>큰</u> 경우 right노드를 타고 들어가 재귀적으로 계속해서 판단을 진행한다.

if문으로 종료조건을 달아준다

- 현재 노드가 null인 경우(leaf 노드에서 다음 노드를 타고 들어간 경우)



<br>

### **Insert(T val), insertNode(Node node, T val)**

삽입 같은 경우도 마찬가지로 값 비교를 통해 작다면 left노드에 크다면 right노드에 삽입을 진행해 주어야 합니다.

```java
@Override
public void insert(T val) {
    this.root = this.insertNode(this.root, val);
    this.size++;
}

private Node insertNode(Node node, T val) {
    if (node == null) {
        return new Node(val);
    }
    
    //leaf노드를 찾는 과정
    if (val.compareTo(node.data) < 0) {
        node.left = insertNode(node.left, val);
    } else if (val.compareTo(node.data) > 0) {
        node.right = insertNode(node.right, val);
    }
    
    return node;
}
```

첫 시작을 root 노드로 합니다.

node가 null인 경우 데이터를 삽입할 수 있는 경우 이므로 val 데이터를 가진 노드를 생성하여 리턴하면 insert()메소드에서 이를 다시 root 노드로 대입하여 이 노드를 가지고 insertNode가 호출됩니다.

만약 자식 노드가 있다면 값을 비교하여 삽입하고자 하는 값이 더 작다면 left노드를 계속 타고 들어가 삽입하려는 시도를 재귀적으로 합니다.

반대도 마찬가지로 right노드를 타고들어가 삽입하려는 것을 재귀 호출을 이용하여 구현합니다.



<br>

### **delete(T val)**

삭제 연산도 재귀 호출로 진행됩니다.

```java
@Override

public void delete(T val) {
    this.deleteNode(this.root, val);
}

private Node deleteNode(Node node, T val) {
    if (node == null) return null;
    
    if (val.compareTo(node.data) < 0) {
        node.left = deleteNode(node.left, val);
    } else if(val.compareTo(node.data) > 0) {
        node.right = deleteNode(node right, val);
    }else {
        // val == node.data
        this.size--;
        if (node.left == null) {
            return node.right;
        } else if (node.right == null) {
            return node.left
        }
        
        node.data = this.minNode(node.right);
        node.right = deleteNode(node.right, node.data);
    }
    return node;
}
```

삭제하려고 하는 값의 위치를 찾기 위해서 해당 값과 현재 노드의 데이터 값을 재귀 연산으로 비교하여 위치를 찾아 나갑니다.

- 만약 비교했을 때 삭제하고자 하는 값이 더 작으면 왼쪽 서브 트리 어딘가에 값이 위치해 있을 것입니다.
  - 그래서 left 노드를 타고 들어가며 재귀 호출을 해 줍니다.
- 반대 경우 right노드를 타고 들어가는 재귀 호출을 합니다.
- 만약 위치를 찾은 경우(val == node.data)
  - 삭제 이므로 size를 1 감소 시킵니다.
  - 삭제 하고자 하는 노드가 **<u>leaf 노드</u>**인 경우 null을 리턴시켜 부모 노드의 자식 노드와 연결 시킵니다.
  - 삭제 하고자 하는 노드의 자식 노드가 **<u>1개</u>**인 경우
    - 해당 노드의 자식 노드를 리턴해 줍니다.
  - 삭제 하고자 하는 노드의 자식 노드가 <u>**2개**</u>인 경우
    - 오른쪽 서브 트리의 최솟값을 삭제할 노드의 위치로 가져오는 방법 사용합니다.
    - 가져온 위치의 노드는 삭제 되어야 하므로 deleteNode메소드를 재귀 호출하여 노드의 오른쪽 서브 트리에 값을 가져온 노드 데이터가 삭제된 서브 트리가 위치 하도록 해 줍니다.

- 마지막으로 node를 반환하여 함수를 종료합니다.

<br>

### **size()**

```java
@Override
public int size() {
    return this.size;
}
```

size함수는 간단하게 변수를 리턴하면서 마무리 해 줍시다.



## 관련된 문제

[백준 9934번 완전 이진 트리](https://www.acmicpc.net/problem/9934)









