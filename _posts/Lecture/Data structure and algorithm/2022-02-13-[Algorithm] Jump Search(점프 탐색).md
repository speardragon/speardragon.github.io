---
layout: single
title: "[Algorithm] Jump Search(점프 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'BinarySearch']
tag: ['Data structure', 'Algorithm', 'Jump Search', '점프 탐색']
toc: true
toc_sticky: true
---



이번 시간에는 저번 시간(이진 탐색)에 이어서 점프 탐색(Jump search)에 대해서 알아보도록 하자.



### 점프 탐색이란?

Jump Search는 순차적으로 탐색하는 선형 탐색과는 달리 블록 단위로 이동(점프)하면서 탐색하는 알고리즘이다.

이 역시도 이진 탐색과 마찬가지로 정렬된 배열에서만 사용할 수 있고, 한 칸씩 이동하는 것이 아니기 때문에 선형 탐색보다 적은 요소를 탐색할 수 있어 더 빠르게 탐색할 수 있다.

<br>

이 알고리즘은 선형 탐색보다는 빠르지만 이진 탐색 보다는 느리다.

<br>

#### 점프 탐색 방식

1. 배열을 블록(block) 단위로 나누어서 탐색을 진행하기 때문에 이 블록 사이즈 m의 크기를 구한다.
   - m의 최적 값은 다음과 같다. 
   - m = √n (n: 요소의 수)
2. 한 블록에서 값을 탐색하고 없으면 다음 블록으로 이동한다.
3. 값을 가진 블록을 찾으면 해당 블록에서 선형 탐색을 사용하여 정확한 인덱스를 찾는다.

<br>

##### 블록의 최적 사이즈를 구하는 방법

m의 최적 값을 구하는 공식은 다음과 같다.

- 블록 크기가 m이고 n개의 요소를 가지고 있다면 점프 탐색에서 총 n/m 만큼 블록을 탐색하게 된다.
- 이후 선형 탐색은 (m - 1)번 일어나기 때문에 총 n/m + m - 1 만큼 수행하는 것이다.

이 상황에서 m의 최적 해는 다음 공식으로 구할 수 있다.
$$
{\operatorname{d}\!\over\operatorname{d}\!m}({\operatorname{n}\over\operatorname{m}} + 1) = 0
$$

> -nm<sup>-2</sup> + 1 = 0 
>
> nm<sup>-2</sup> = 1
>
> m =  √n

<br>

### Search 방식

다음 그림을 보면서 어떤 식으로 탐색이 진행되는 지 살펴 보자

아래 그림에서 크기가 11인 배열에서 key값인 22를 찾는다고 가정하자.

1. 블록 사이즈 m을 구한다.
   - m = √11 = 3 

![image](https://user-images.githubusercontent.com/79521972/155670545-eebb009a-17ac-4777-8700-ebbf94c98ee4.png)

<br>

2. key 값을 포함한 블록을 찾을 때까지 탐색을 진행한다.

   - 첫 번째 블록을 확인한다. 블록의 최댓값(11)이 key값(22)보다 작기 때문에 더 볼 것도 없이 다음 블록으로 이동한다.

   ![image](https://user-images.githubusercontent.com/79521972/155671140-5581bc0f-d182-402d-b7c8-2e756476ab2d.png)

   - 두 번째 블록을 확인한다. 블록의 최댓값(18)이 key값(22)보다 작기 때문에 더 볼 것도 없이 다음 블록으로 이동한다.

   ![image](https://user-images.githubusercontent.com/79521972/155671268-d9dda9fd-b211-495f-836c-b5a046914e6f.png)

   - 세 번째 블록을 확인한다. 블록이 key값(22)을 포함하고 있다. 

   ![image](https://user-images.githubusercontent.com/79521972/155671376-dfd03e37-eee3-475d-8829-2abb348c9807.png)

   - 찾았기 때문에 탐색 종료

   

3. 세 번째 블록 첫 번째 인덱스부터 선형 탐색으로 key값을 찾는다.

![image](https://user-images.githubusercontent.com/79521972/155671538-7594e6a6-3a21-4f33-a6d9-8e8f476d3642.png)

<br>

### 점프 탐색의 종료 조건

점프 탐색에서 종료 조건은 두 가지가 있다. 다음 조건 중 하나라도 만족하면 탐색은 종료된다.

<br>

1. 데이터 검색을 성공한 경우
   - 검색 key를 가진 블록을 발견한 경우이다.

![image](https://user-images.githubusercontent.com/79521972/155671538-7594e6a6-3a21-4f33-a6d9-8e8f476d3642.png)

<br>

2. 데이터 검색에 실패한 경우
   - 더 이상 검색할 블록이 없는 경우이다.

![image](https://user-images.githubusercontent.com/79521972/155671854-d11019f9-40c3-46ac-a80b-9a998f479044.png)



<br>

## 점프 탐색 구현

```java
int jumpSearch(int arr[], int n, int key) {
    int m = sqrt(n);
    
    int prev = 0;
    
    while (arr[min(m, n) -1] < key) {
        prev = m;
        m += m;
        if (prev >= n) 
            return -1;
    }
    
    while (arr[prev] < key) {
        prev++;
        
        if (prev == min(m, n)) return -1;
    }
    
    if (arr[prev] == key) 
        return prev;
    
    return -1;
}
```

1. m의 크기를 정한다.
2. 블록의 크기만큼 점프하면서 key값을 포함하는 블록을 찾는다.
3. key 값을 포함한 블록을 찾았다면 블록의 첫 번째 인덱스부터 선형 탐색을 시작한다.
4. 검색 key를 찾은 경우 prev를 반환한다.

<br>

#### 시간 복잡도

| 연산   | Best | Average | Worst |
| ------ | ---- | ------- | ----- |
| Search | O(1) | O(√n)   | O(√n) |

<br>