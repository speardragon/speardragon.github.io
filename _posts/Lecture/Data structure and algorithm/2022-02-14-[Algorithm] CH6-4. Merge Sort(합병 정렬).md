---
layout: single
title: "[Algorithm] CH6-4. Merge Sort(합병 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬', 'merge sort', '합병 정렬']
toc: true
toc_sticky: true
---

<br>

# Merge Sort(합병 정렬)

이번 시간에는 merge sorting에 대해서 배워보도록 하겠습니다.

merge sort와 다음 시간에 배울 quick sort가 정렬 알고리즘의 핵심이라고 할 수 있습니다. 하지만 이전에 배웠던 것들과는 다르게 복잡하게 진행이 됩니다.

## Merge sort란?

하나의 리스트를 두 개의 균등한 크기의 리스트로 분할하고 부분 리스트를 합치면서 정렬하여 전체가 정렬되게 하는 방법입니다.

안정 정렬에 속하며, 분할 정복 알고리즘의 하나입니다. 이에 대해서는 뒤에서 더 설명하도록 하겠습니다.

**전반적인 내용**

1. 리스트의 길이가 0또는 1이면 이미 정렬된 것으로 봅니다.
2. 1번과 같지 않고 정렬되지 않은 리스트는 절반으로 잘 균등하거나 비슷한 크기의 두 부분의 리스트로 나눕니다.
3. 각 서브 리스트를 재귀적으로 합병 정렬을 이용해 정렬합니다.
4. 두 서브 리스트를 다시 하나의 정렬된 리스트로 합병합니다.

크게 보면 다음 그림과 같은 것이 합병정렬이라고 할 수 있습니다.

![image](https://user-images.githubusercontent.com/79521972/153863310-e33d3d9d-a301-427c-8e12-74059aad50fc.png)

그렇다면 이에 대해 하나하나 자세히 살펴 보도록 하겠습니다.

<br>

아래의 정렬되기 전의 리스트가 있다고 가정합시다. 이 리스트를 크기가 같은 두 개의 리스트로 분할 합니다.

![image](https://user-images.githubusercontent.com/79521972/153852169-afc62f74-36b4-448e-8290-48ece08dfd7c.png)

여기서 이 서브 리스트들을 또 다시 같은 크기의 두 개의 서브 리스트로 분할 합니다.

![image-20220214200649547](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220214200649547.png)

이 과정을 가장 하위의 서브 리스트의 크기가 1이 될 때까지 반복합니다. 현재 하위 서브 리스트의 크기는 2이므로 이 과정을 한 번 더 반복합니다.

![image](https://user-images.githubusercontent.com/79521972/153853037-ad37eae2-7a8b-4885-83a6-f1e7e9cba420.png)

이제 크기가 1인 8개의 서브 리스트들로 분할이 되었습니다.  

![image](https://user-images.githubusercontent.com/79521972/153853298-e31887f9-44d6-4e37-b5f9-eaa8a782c541.png)

이제 이 분할된 부분 리스트들을 정렬하면서 다시 합쳐주는 과정을 진행합니다.

크기가 1인 부분 리스트들을 정렬하여 크기가 2인 부분 리스트로 합칩니다. 합치면서 각 원소들은 자신의 위치로 정렬되기 때문에  합쳐진 서브 리스트 들은 정렬된 상태를 유지하게 됩니다.

![image](https://user-images.githubusercontent.com/79521972/153853592-cf070b8c-988c-489a-9d11-bec9afecddcb.png)

이 과정을 모든 서브 리스트들이 하나의 리스트가 될 때까지 반복해 줍니다.

![image](https://user-images.githubusercontent.com/79521972/153854146-24638758-d9bf-4fac-8d3e-a5b79ac644dd.png)

결과적으로 마지막에 합치는 과정에서 크기가 8인 정렬된 리스트를 얻을 수 있게됩니다.



---

**분할 정복 (Divide and Conquer) 알고리즘**

![Merge-sort-example-300px](https://user-images.githubusercontent.com/79521972/153854261-24df4ec9-8d31-4893-b0c4-079e98c6ffcb.gif)

이렇게 하나의 문제를 동일한 유형의 작은 문제들로 분할한 다음, 작은 문제에 대한 결과들을 조합해서 큰 문제들을 해결하는 알고리즘은 `분할 정복(Divide and conquer)`이라고 합니다.

이 분할 정복은 알고리즘 풀이에 있어서 적지 않게 사용되고 보통 재귀함수로 구현이 됩니다. 그러한 이유로 merge sort 또한 재귀함수로 구현을 할 것입니다.



### 시간 복잡도

- O(NlogN)

![image](https://user-images.githubusercontent.com/79521972/153855999-07d32dac-d19c-4338-a41b-ba3d9ca8a70f.png)

우선 분할 과정에서 리스트의 크기가 1/2씩 감소하게 됩니다. 그래서 처음 n개의 리스트에서 분할을 진행하면 n/2크기의 서브 리스트 2개를 얻게 되고 이 상태에서 분할을 다시 진행하게 되면 n/4크기의 서브 리스트 4개를 얻게 됩니다.

이러한 분할 연산을 서브 리스트들의 크기가 1이 될 때까지 반복해야 하는데 리스트의 크기가 2라면 1번, 4개라면 2번, 8개라면 3번을 반복 해야 크기 1의 리스트를 얻을 수 있습니다. 따라서 분할에 있어서 O(logN)의 시간복잡도를 갖게 됩니다.

<br>

![image](https://user-images.githubusercontent.com/79521972/153855972-1bfa732b-f4f7-41d3-8512-2d65a68fe02b.png)

또한 분할된 상태에서 리스트들을 다시 합칠 때(합병,Merge) 각 element들을 비교하면서 진행되기 때문에 리스트의 크기가 1인 상태일 때 총 n개의 리스트가 있을 것이기 때문에 이 때 n번의 비교연산을 수행 하게 됩니다. 이후 합쳐진 상태에서 리스트의 크기가 n/4이고 갯수가 4개라면 또 다시 n번의 비교연산이 필요합니다. 결국 각 depth에서 n번 비교 연산을 갖게 되고 이는 O(N)의 시간 복잡도를 갖게 됩니다.

![image](https://user-images.githubusercontent.com/79521972/153855949-f5328279-9c87-434f-b5ca-8a408bc3acab.png)

따라서 결과적으로 merge sort의 시간 복잡도는 분할과 합병과정을 합친 O(NlogN) 의 시간복잡도를 갖게 됩니다.



## Merge Sort 구현

merge sort를 구현하는 방법에는

1. In-place 방법으로 구현하는 것
2. Out-of-place 방법으로 구현하는 것

이 있는데 1번 방법으로 구현을 한 번 해 보도록 하겠습니다. Out-of-place 방법은 In-place 방법보다 훨씬 쉽기 때문에 혼자서도 구현을 하실 수 있으실 겁니다.

```java
package sort;

public class MergeSort implements ISort {
    
   	...
}
```

<br>

제일 처음 sort() 메소드를 살펴 보겠습니다. sort()메소드에서 In-place sort가 이루어집니다.

분할과정을 먼저 살펴보겠습니다.

```java
@Override
public void sort(int[] arr) {
    //in-place sort
    mergeSort(arr, 0, arr.length - 1);
}

//분할
private void mergeSort(int[] arr, int low, int high) {
   	if (low >= high) { //종료 조건
        return;
    }
    
    int mid = low + ((high - low) / 2);
    mergeSort(arr, low, mid);//왼쪽
    mergeSort(arr, mid + 1, high);//오른쪽
    
    merge(arr, low, mid, high);
}
```

**sort()**

처음에는 mergeSort()로 배열의 0번 부터 끝까지를 index로 잡아 분할을 진행합니다.

**분할(mergeSort())**

먼저 분할 과정에서 재귀 호출로 중간값을 찾아 이를 기준으로 큰 부분(오른쪽)과 작은 부분(왼쪽)의 인덱스로 나눠지게 하여 두 서브 리스트로 분할 될 것입니다.

종료조건은  low가 high보다 크거나 같을 때, 즉 다시 말해서 배열의 크기가 1이 되는 경우 종료하는 것으로 합니다. 종료 시 mergeSort를 호출한 곳으로 돌아갑니다.

<br>

다음으로 합병을 살펴 보겠습니다.

```java
//합병
private void merge(int[] arr, int low, int mid, int high) {
    int[] temp = new int[high - low + 1];
    int idx = 0;
    
    int left = low;
    int right = mid + 1;
    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp[idx] = arr[left];
            left++;
        } else {
            temp[idx] = arr[right];
            right++;
        }
        idx++
    }
    
    while (left <= mid) { //왼쪽 리스트에 값이 남아 있는 경우
            temp[idx] = arr[left];
            idx++;
            left++;
        }
        
    while (right <= high) { //오른쪽 리스트에 값이 남아 있는 경우
        temp[idx] = arr[right];
        idx++;
        right++;
    }

    for (int i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
}
```

**합병(merge())**

- 합병에 필요한 보조배열을 temp라는 이름으로 만들어 주고 이 배열의 크기는 합쳐진 배열의 크기와 같기 때문에 high - low + 1입니다.
- 보조 배열의 인덱스로 사용하기 위한 idx 변수 선언 
- left 변수는 low, 즉 분할된 왼쪽 리스트의 시작 index를 의미하고,
  right변수는 mid + 1, 즉 분할된 오른쪽 리스트의 시작 index를 의미하도록 합니다.
-   첫 번째 while문
  - 이 반복문을 통해 분할 정렬 된 리스트의 합병이 됩니다.
  - ( left <= mid and right <= high ) : left나 right 중 어느 index라도 리스트의 모든 값을 꺼내게 되면 종료하도록 합니다.
  - 만약 arr[left] 값이 arr[right]보다 작거나 같으면 보조 배열 temp에 arr[left]값을 넣고 left 인덱스를 1 증가시킵니다.
  - 위의 경우가 아닌 경우, 즉 arr[left]값이 arr[right]값보다 크면 보조 배열 temp에 arr[right]값을 넣고 right 인덱스를 1 증가 시킵니다.
  - 마지막으로 temp 배열의 idx도 증가시킵니다.

- 두 번째 while문
  - 이 반복문에서는 왼쪽 리스트에 값이 남아 있는 경우를 처리하는 것을 목표로 합니다.
  - left 인덱스가 mid값보다 작으면 아직 왼쪽 서브 리스트에 값이 남아있는 것이기 때문에 
    남아있는 값들을 temp 배열에 쭉 넣어줍니다.

- 세 번째 while문 
  - 이 반복문에서는 오른쪽 서브 리스트에 값이 남아 있는 경우를 처리하는 것을 목표로 합니다.
  - right 인덱스가 high값보다 작으면 아직 오른쪽 서브 리스트에 값이 남아있는 것이기 때문에 남아있는 값들ㅇ르 temp 배열에 쭉 넣어줍니다.

- 마지막으로 low 인덱스 부터 high 인덱스까지 temp 배열에 담았던 정렬된 리스트를 다시 arr배열에 담아줌으로써 마무리합니다.

<br>

코드를 봤을 때 이해가 잘 가지 않을 수 있기 때문에 그림과 함께 설명을 덧붙이겠습니다.



![image](https://user-images.githubusercontent.com/79521972/153868939-90235335-0754-4feb-8fd9-897c79c09afe.png)

- 초기 배열 상태가 21, 10, 12, 20, 25, 13, 15, 22라고 합시다.
- 두 개의 리스트의 값들을 처음부터 하나씩 비교하여 두 개의 리스트의 값들 중 더 작은 값을 
  보조 배열(sorted)에 옮깁니다.
- 둘 중 하나가 끝날 때까지 이 과정을 되풀이(재귀적) 합니다.
- 만약 둘 중에서 하나의 리스트가 먼저 끝나게 되면 나머지 리스트의 값들을 전부 보조 배열로 복사합니다.
- 보조 배열을 원래의 리스트(arr)로 옮깁니다.



## 합병 정렬의 시간 복잡도

- 분할 단계

  - 비교 연산과 이동 연산이 수행되지 않습니다.

- 합병 단계

  - 비교 횟수
    

    ![image](https://user-images.githubusercontent.com/79521972/153869839-5f03cc8a-e732-457f-af28-c10e0115dbc0.png)

  - 재귀 호출의 depth (합병 단계의 수)

    - 레코드의 개수 n이 2의 거듭제곱이라고 가정(n=2^k)했을 때, n=2^3의 경우, 2^3 -> 2^2 -> 2^1 -> 2^0 순으로 줄어들어 순환 호출의 깊이가 3임을 알 수 있다. 이것을 **일반화**하면 n=2^k의 경우, k(k=log₂n)임을 알 수 있다.
    - k=log₂n

  - 각 합병 단계의 비교 연산

    - 크기 1인 부분 배열 2개를 합병하는 데는 최대 2번의 비교 연산이 필요하고, 부분 배열의 쌍이 4개이므로 24=8번의 비교 연산이 필요하다. 다음 단계에서는 크기 2인 부분 배열 2개를 합병하는 데 최대 4번의 비교 연산이 필요하고, 부분 배열의 쌍이 2개이므로 42=8번의 비교 연산이 필요하다. 마지막 단계에서는 크기 4인 부분 배열 2개를 합병하는 데는 최대 8번의 비교 연산이 필요하고, 부분 배열의 쌍이 1개이므로 8*1=8번의 비교 연산이 필요하다. 이것을 일반화하면 하나의 합병 단계에서는 최대 n번의 비교 연산을 수행함을 알 수 있다.
    - 최대 n번

  - 순환 호출의 깊이 만큼의 합병 단계 * 각 합병 단계의 비교 연산 = nlog₂n

- 이동 횟수

  - 순환 호출의 깊이 (합병 단계의 수)
    - k=log₂n
  - 각 합병 단계의 이동 연산
    - 임시 배열에 복사했다가 다시 가져와야 되므로 이동 연산은 총 부분 배열에 들어 있는 요소의 개수가 n인 경우, 레코드의 이동이 2n번 발생한다.
  - 순환 호출의 깊이 만큼의 합병 단계 * 각 합병 단계의 이동 연산 = 2nlog₂n

- T(n) = nlog₂n(비교) + 2nlog₂n(이동) = 3nlog₂n = O(nlog₂n)



<br>

## 정렬 알고리즘 별 시간복잡도 비교

![image](https://user-images.githubusercontent.com/79521972/153831460-b472e26d-a089-44c4-9c19-1cb1158a75dc.png)

- 단순하지만 비효율적인 방법에는
  - 삽입 정렬, 선택 정렬, 버블 정렬
- 복잡하지만 효율적인 방법에는
  - 퀵 정렬, 힙 정렬 ,합병 정렬, 기수 정렬

이 있습니다.