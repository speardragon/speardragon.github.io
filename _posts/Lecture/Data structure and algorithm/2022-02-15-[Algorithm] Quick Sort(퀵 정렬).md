---
layout: single
title: "[Algorithm] Quick Sort(퀵 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬', 'Quick sort', '퀵 정렬']
toc: true
toc_sticky: true
---

<br>

# 퀵 정렬

이번시간에는 퀵 정렬에 대해서 배워 보도록 하겠습니다.

먼저 퀵정렬의 특징은 다음과 같습니다.

- 퀵 정렬은 합병 정렬과 마찬가지로 배열을 둘 씩 분할하며 정렬하는 과정을 거치기 때문에 시간복잡도 O(NlogN)을 갖습니다. 하지만 같은 시간 복잡도라도 실제 정렬에서는 퀵 정렬이 훨씬 더 빠른 시간 안에 정렬이 가능합니다.

  - 리스트 파트에서 배웠던 배열리스트가 메모리 공간 상에서 인접하게 위치해 있기 때문에 연산에 더 유리했다고 했던 것처럼 이 역시 컴퓨터의 하드웨어 특성 때문에 더 빠른 성질을 가질 수 있게 됩니다. -> 참조 지역성(locality of reference)의 원리
    - 퀵 정렬은 알고리즘 자체의 특성 상 동일한 배열 내에서 자리를 이동시킵니다. 그래서 인접한 데이터들 사이의 이동이 발생하기 때문에 제일 처음 배열에 접근 할 때만 실제 메모리에서 데이터를 가져오고 이후에는 캐시(cache)로 배열에 접근하기 때문에 메모리로 접근할 때보다 훨씬 더 빠른 접근이 가능한 것입니다.
      이후 더 자세한 내용은 운영체제 수업에서 보도록 하겠습니다.

  - 한 번 위치가 결정된 pivot 값은 이후의 연산에서는 해당 값을 제외하고 연산을 진행하기 때문에 정렬이 진행 될 수록, 즉 분할이 점점 될 수록 계산해야 할 데이터의 수가 점점 줄어드는 특성이 있습니다. 따라서 데이터가 줄어들어 정리할 속도가 줄어들게 되는 것입니다.

- merge sort에서는 인메모리 정렬로 구현을 하더라도 정렬을 보조해 주는 추가 메모리 공간이 필요하다는 단점이 있었지만
  quick sort에서는 추가 공간을 더 사용하지 않고 합병 정렬과 비슷하기 **divide and conquer** 방식으로 진행이 됩니다.

- 불안정 정렬 방식입니다.

<br>

### 퀵 정렬의 개념

- 하나의 리스트를 pivot을 기준으로 두 개의 비균등한 크기로 분할하고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 하는 방법입니다.
- 단계는 다음과 같습니다.
  1. 분할(Divide): 입력 배열을 pivot을 기준으로 비균등하게 2개의 서브 리스트(pivot을 중심으로 왼쪽: pivot보다 작은 요소들, 오른쪽: pivot보다 큰 요소들)로 분할합니다.
  2. 정복(Conquer): 서브 리스트를 정렬합니다. 서브 리스트의 크기가 충분히 작지 않다면 재귀 호출을 이용하여 다시 분할 정복 방법(Divide and conquer)을 적용합니다.
  3. 결합(Combine): 정렬된 부분 배열들을 하나의 배열에 합병합니다.
- 재귀 호출이 한 번 진행될 때마다 최소한 하나의 pivot은 최종적으로 위치가 정해지기 때문에 이 알고리즘은 반드시 끝난다는 것을 보장 받을 수 있는 것입니다.



<br>

---

자 그럼 이제 그림으로 한 번 퀵 정렬을 이해해 보도록 하겠습니다.

- pivot값을 먼저 정해줍니다.
  - pivot값은 리스트의 가장 앞에 위치한 원소도 가능하고 가장 뒤에 위치한 원소도 가능합니다. 어느 원소로 잡아도 되기 때문에 저는 가장 중간에 위치한 '5'로 정해보겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153881076-391ba485-1f88-41f1-86b9-f41099d05ee8.png)

- 그렇게 해서 이 pivot값을 기준으로 원소들을 재배치하게 됩니다. 

- 재배치하는 방식은 아래와 같이 5보다 작은 값들은 5의 왼쪽으로 5보다 큰 값들은 5의 오른쪽으로 이동시킵니다.

![image-20220214232337193](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220214232337193.png)

이 때 pivot값의 index가 바뀌는 것은 pivot값 자체가 중요한 것이기 때문에 신경 쓰시지 않아도 됩니다.

- 그럼 pivot값을 중심으로 두 개의 서브 리스트가 생긴 것입니다.

- 이 서브 리스트들에서 다시 각각 pivot값을 정해 줍니다.
- 저는 왼쪽 서브 리스트에서는 중간 값인 1, 오른쪽 서브리스트에서는 6으로 잡았습니다.

![image](https://user-images.githubusercontent.com/79521972/153882595-17470368-e60e-43db-aaca-b98b128eb5ff.png)

- 그럼 다시 이 pivot값을 중심으로 대소 비교를 통한 재배치 과정을 진행합니다.

- 여기서 오른쪽의 서브 리스트는 원소의 갯수가 하나만 남았기 때문에 더 이상 분할 할 수가 없기 때문에 오른쪽의 퀵 정렬은 종료가 되었습니다.
  왼쪽의 서브 리스트는 아직 정렬 할 원소가 남아있기 때문에 해당 리스트에서 pivot값을 정해 정렬해 주겠습니다.
- pivot값을 중간값인 3으로 잡고 진행해 보겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153884073-7aa8b019-3a32-467e-988f-38c4f01aa986.png)

- 또 다시 pivot값을 기준으로 재배치하여 분할을 진행합니다.
- 분할을 통해 나온 양쪽 서브 리스트 원소의 갯수가 1개만 남았으므로 퀵 정렬이 완전히 종료가 되었고 실제로 정렬된 배열을 보면 잘 정렬 되었음을 보실 수 있습니다.

<br>

퀵 정렬을 움직이는 애니메이션으로 보면 다음과 같습니다.

여기서는 pivot값을 서브 리스트의 중간 값이 아니라 가장 맨 오른쪽 값으로 정하여 진행한 모습입니다.

![Sorting_quicksort_anim](https://user-images.githubusercontent.com/79521972/153884489-eb997f20-34e2-4275-afa8-54b85494dcad.gif)

[출처: 위키백과 - 퀵 정렬](https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC)

<br>

  

## 퀵 정렬의 시간복잡도

- 최선의 경우

  - 비교 횟수
  - ![image](https://user-images.githubusercontent.com/79521972/153885858-13e1f947-e1c5-4f82-9187-1c32abbd63d1.png)

  - 재귀 호출의 depth
    - 레코드 개수 n이 2의 거듭제곱이라고 가정(n=2^k)했을 때, n = 2^3인 경우,
      2^3 -> 2^2 -> 2^1 -> 2^0 순으로 줄어들어 재귀 호출의 depth가 3인 것을 확인 할 수 있습니다. 이를 일반화하면 n = 2^k인 경우, k(k = log₂n) 임을 알 수 있습니다.
    - k = log₂n
  - 각 재귀 호출 단계의 비교 연산
    - 각 재귀 호출에서는 전체 리스트의 대부분의 레코드을 비교해야 하므로 평균 n번 정도의 비교가 이루어집니다.
    - 평균 n번
  - 재귀 호출의 깊이 × 각 재귀 호출 단계의 비교 연산 = nlog₂n

- 이동 횟수 

  - 비교 횟수보다 적기때문에 무시할 수 있습니다.

- 최선의 경우 T(n) = **O(nlog₂n)**

<br>

- 최악의 경우 (<u>Median of Three</u>)
  - 리스트가 계속 불균형하게 나누어지는 경우 
    - 특히, 이미 정렬되어 있는 리스트에 대하여 퀵 정렬을 실행하는 경우를 말합니다.

![image](https://user-images.githubusercontent.com/79521972/153885774-c5aac689-ce6e-4008-9f36-d31648096eb7.png)



-  비교 횟수	
  - 재귀 호출의 depth
    - 레코드의 개수 n이 2의 거듭제곱이라고 가정(n=2^k)했을 때, 재귀 호출의 depth는 n입니다.
    - n번
  - 각 순환 호출 단계의 비교 연산
    - 각 순환 호출에서는 전체 리스트의 대부분의 레코드를 비교해야 하므로 평균 n번 정도의 비교가 이루어집니다.
    - 평균 n번
  - 재귀 호출의 깊이 × 각 재귀 호출 단계의 비교 연산 = n^2
- 이동 횟수
  - 비교 횟수보다 적기 때문에 무시할 수 있습니다.
- 최악의 경우 T(n) = **O(n^2)**

<br>

- 평균 
  - 평균 T(n) = **O(nlog₂n)**
  - 시간 복잡도가 O(nlog₂n)를 가지는 다른 정렬 알고리즘과 비교했을 때도 퀵 정렬이 월등히 빠르다
  - 퀵 정렬이 불필요한 데이터의 이동을 줄이고 먼 거리의 데이터를 교환할 뿐만 아니라, 한 번 결정된 pivot들이 추후 연산에서 제외되는 특성 때문이다.



<br>

> 그래서 퀵 정렬을 사용할 때는 보통 중간값을 pivot으로 정하여 구현하는 것이 일반적입니다.

<br>



## 퀵 정렬 구현

퀵 정렬 또한 분할 정복 방법으로 구현할 것이기 때문에 quickSort라는 메소드를 재귀호출하여 구현해 보도록 하겠습니다.

```java
package sort;

public class QuickSort implements ISort {
    
    @Override
    public void sort(int[] arr) {
        quickSort(arr, 0, arr.length - 1);
    }
}
```

**sort()**

sort() 메소드에서는 본격적으로 퀵 정렬을 시작할 것이기 때문에 quickSort에 배열과 시작 인덱스는 0 마지막 인덱스는 배열의 마지막 인덱스를 넣어주어 호출합니다.

<br>

```java
private void quickSort(int[] arr, int low, int high) {
    if (low >= high) { //종료조건
        return;
    }
    
    int pivot = low + (high - low) / 2;
    int pivotValue = arr[pivot];
    
    int left = low;
   	int right = high;
    while (left <= right) {
        while (arr[left] < pivotValue) {
            left++;
        }
        
        while (arr[right] > pivotValue) {
            right--;
        }
        
        if (left <= right) {
            int tmp = arr[right];
            arr[right] = arr[left];
            arr[left] = tmp;
            left++;
            right--;
        }
    }
    
    quickSort(arr, low, right);
    quickSort(arr, low, high);
}
```

- 가장 먼저 quickSort도 종료 조건으로 low가 high와 같아지는 상황, 즉 분할을 모두 마쳐 서브 리스트의 크기가 1인 상황, 즉 원소가 하나인 상황에 return을 합니다.
- 이제 퀵 정렬을 시작해 볼 것입니다. 퀵 정렬에서는 가장 먼저 **pivot**값을 정해주어야 합니다.
  저는 여태 그래왔듯 중간 값으로 pivot값을 정해주었습니다. '(low + high) / 2'와 같이 해주지 않은 이유는 overflowException을 방지하기 위함입니다.
-  pivotValue에는 pivot인덱스에 해당하는 아주 중요한 pivot값을 넣어주도록 합니다.
- left와 right에는 배열의 low, high값을 각각 넣어줍니다.
- 첫 번째 while문
  - left의 값과 right의 값이 겹치지 않을 때 까지 다음 내용을 반복합니다.
  - 내부 while문_1
    - 배열의 left값이 pivot값보다 작으면 다음 left 배열을 순회해야 하기 때문에 left 값을 1 증가시켜 다음 index로 이동합니다.
  - 내부 while문_2
    - 배열의 right값이 pivot값보다 크면 다음 right 배열을 순회해야 하기 때문에 right값을 1 감소시켜 다음 index로 이동합니다.
  - 만약 left 와 right이 이동하다가 서로 만나게 되거나 교차하지 않은 상황이면 배열의 left의 값과 right의 값을 서로 바꾸어 주는 과정을 진행 후 left와 right의 index를 각자 방향으로 이동시킵니다.
- 이를 재귀함수를 통해 왼쪽과 오른쪽 각각에 대하여 퀵 정렬을 진행합니다.



<br>

이해를 위해 그림을 통해 설명하도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153900024-530417b3-9082-41c2-9e5f-bac31ab2fb44.png)



- pivot값이 5로 잡혔기 때문에 처음 left의 index는 0번에서 pivot이 위치한 곳까지 오게되고 right은 맨 끝에서 2가 위치한 곳까지 오게됩니다.
  - left 서브 리스트의 원소는 모두 5보다 작기 때문에 pivot까지 온 것이고
    right 서브 리스트의 원소중 5보다 크지 않은 2가 존재하여 해당 위치에서 멈춘 모습입니다.
- 그리고 왼쪽 인덱스와 오른쪽 인덱스가 아직 교차하지 않은 상태이므로 left와 right의 값을 바꿔줍니다.
- 스왑이후 다시 while문으로 돌아와 진행합니다.
- 이 상태에서 left는 7을 만나 멈추고 right은 이미 pivot이기 때문에 그대로입니다.
- 이 때도 아직 서로 교차하기 전이므로 left값과 right값을 바꾸어 줍니다.
  - 그러고 나서 left와 right가 교차하게 되어 왼쪽 리스트와 오른쪽 리스트가 pivot에 의해 정렬되었습니다.
- 이를 서브리스트의 크기가 1이 될 때까지 반복합니다.

<br>

또 다른 예로 pivot을 맨 왼쪽으로 잡은 경우를 도식화한 것을 보여드리고 마무리 하도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153901057-238955ac-bb29-453b-9ff0-6978bf6a308f.png)



**[장점]**

1. 특정 상태가 아닌 이상 평균 시간 복잡도는 NlogN이며, 다른 NlogN 알고리즘에 비해 대체적으로 속도가 매우 빠르다. 유사하게 NlogN 정렬 알고리즘 중 분할정복 방식인 merge sort에 비해 2~3배정도 빠르다. (정리 부분의 표 참고)

2. 추가적인 별도의 메모리를 필요로하지 않으며 재귀 호출 스택프레임에 의한 공간복잡도는 logN으로 메모리를 적게 소비한다.

**[단점]**

1. 특정 조건하에 성능이 급격하게 떨어진다.

2. 재귀를 사용하기 때문에 재귀를 사용하지 못하는 환경일 경우 그 구현이 매우 복잡해진다.



## 정렬 알고리즘 별 시간복잡도

![image](https://user-images.githubusercontent.com/79521972/153902016-cf868c9f-d46a-4030-94c8-aba5bdb41e4e.png)



