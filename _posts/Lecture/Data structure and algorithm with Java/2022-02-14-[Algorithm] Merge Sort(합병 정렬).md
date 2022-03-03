---
layout: single
title: "[Algorithm] Merge Sort(합병 정렬)"
categories: ['Lecture', 'Data structures and algorithms with Java', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬', 'merge sort', '합병 정렬']
toc: true
toc_sticky: true
---

이번 시간에는 merge sorting에 대해서 배워보도록 하겠다.

저번 포스팅에서 설명했던 정렬 방식들(버블 정렬, 삽입 정렬, 선택 정렬) 은 가장 기본적인 수준의 정렬 방식들이다. 

그러나 이것들은 일반적으로 좋은 성능은 낼 수 없기 때문에 실제로 개발 시에는 잘 사용하지 않는다.

앞으로 배울 Merge / Quick 정렬은 굉장히 중요하기 때문에 자세히 알아보도록 하자.

<br>

### Merge Sort(합병 정렬) 이란?

![image](https://user-images.githubusercontent.com/79521972/155832316-604b0cc8-3559-467b-8856-17915b01a39a.gif)

merge sort와 다음 시간에 배울 quick sort가 정렬 알고리즘의 핵심이라고 할 수 있습니다. 하지만 중요한 만큼 이전에 배웠던 것들과는 다르게 매우 복잡하게 진행이 된다.

<br>

하나의 리스트를 두 개의 균등한 크기의 리스트로 분할하고 부분 리스트를 합치면서 정렬하여 최종적으로 전체가 정렬되게 하는 방법이다.

#### 개요

- 일반적인 방법으로 구현했을 때 이 정렬은 **안정 정렬**에 속하고, 분할 정복 알고리즘의 하나이다.
- 이에 대한 내용은 뒤에서 더 자세히 다루어 보도록 하겠다.

<br>

**전반적인 과정**

1. 리스트의 크기가 0또는 1이면 이미 정렬된 것으로 본다.
2. 1번에 해당하지 않고 정렬되지 않은 리스트는 절반으로 균등하거나 비슷한 크기의 두 부분의 리스트로 잘 나눈다.
3. 각 서브 리스트를 재귀적으로 합병 정렬을 이용해 정렬한다.
4. 두 서브 리스트를 다시 하나의 정렬된 리스트로 합병한다.



<br>

다음 그림은 합병정렬의 과정을 도식화 한 것이다.



![image](https://user-images.githubusercontent.com/79521972/153863310-e33d3d9d-a301-427c-8e12-74059aad50fc.png)

그렇다면 이 과정에 대해 하나하나 자세히 살펴 보도록 하겠습니다.

<br>

### 합병 정렬의 방식

- 하나의 리스트를 두 개의 균등한 크기의 서브 리스트로 분할하고 분할 된 리스트를 정렬한 다음, 이 두 정렬도니 서브 리스트를 합쳐서 정련된 전체의 리스트가 되게끔 하는 방법이다.
- 합병 정렬은 다음의 단계들로 이루어져 있다.
  - **분할(Divide)**: 입력 배열을 같은 크기의 2개의 부분 배열로 **분할**한다.
  - **정복(Conquer)**: 부분 배열을 **정렬**한다. <span style="color:red">부분 배열의 크기가 조금 큰 편이라고 생각이 들면 재귀 호출을 통해 다시 분할 정복 방법을 진행</span>한다.

<br>

### 합병 정렬 - Merge Sort

아래 그림처럼 정렬되기 전의 리스트가 있다고 가정하자. 이 리스트를 크기가 같은 두 개의 리스트로 분할한다.

![image](https://user-images.githubusercontent.com/79521972/153852169-afc62f74-36b4-448e-8290-48ece08dfd7c.png)

<br>

여기서 이 서브 리스트들을 또 다시 같은 크기의 두 서브 리스트로 분할한다.

![image](https://user-images.githubusercontent.com/79521972/155832398-e56feee1-2501-4a4d-9846-7bb099a0c8e5.png)

<br>

<span style="color:red">이 과정을 가장 하위의 서브 리스트의 크기가 1이 될 때까지 반복한다.</span> 현재 하위 서브 리스트의 크기는 2이므로 이 과정을 한 번 더 반복해 주자.

![image](https://user-images.githubusercontent.com/79521972/153853037-ad37eae2-7a8b-4885-83a6-f1e7e9cba420.png)

<br>

이제 크기가 1인 8개의 서브 리스트들로 분할이 되었다.  

![image](https://user-images.githubusercontent.com/79521972/153853298-e31887f9-44d6-4e37-b5f9-eaa8a782c541.png)<

<br>

이제 이 분할된 부분 리스트들을 정렬하면서 다시 합쳐주는 과정(combine)을 진행한다.

크기가 1인 부분 리스트들을 정렬하여 크기가 2인 부분 리스트로 다시 합친다. 이때 합치면서 각 원소들은 자신의 위치로 정렬이 되기 때문에  합쳐진 서브 리스트 들은 항상 정렬된 상태를 유지하게 된다.

![image](https://user-images.githubusercontent.com/79521972/153853592-cf070b8c-988c-489a-9d11-bec9afecddcb.png)

이 과정을 모든 서브 리스트들이 하나의 리스트가 될 때까지 반복해 준다. 

다음 그림은 이 과정을 반복한 것이다.

![image](https://user-images.githubusercontent.com/79521972/153854146-24638758-d9bf-4fac-8d3e-a5b79ac644dd.png)

결과적으로 마지막에 합치는 과정에서 크기가 8인 정렬된 리스트를 얻을 수 있게됩니다.

<br>

---

### **분할 정복 (Divide and Conquer) 알고리즘**

![Merge-sort-example-300px](https://user-images.githubusercontent.com/79521972/153854261-24df4ec9-8d31-4893-b0c4-079e98c6ffcb.gif)

이렇게 하나의 문제를 동일한 유형의 작은 문제들로 분할한 다음, 작은 문제에 대한 결과들을 조합해서 큰 문제들을 해결하는 알고리즘은 `분할 정복(Divide and conquer)`이라고 한다.

이 분할 정복은 알고리즘 풀이에 있어서 적지 않게 사용되고 보통 재귀함수로 구현이 된다. 그러한 이유로 merge sort 또한 재귀함수로 구현을 할 것이다.

<br>

이에 대해서 더 자세히 다룬 포스팅은 아래 링크를 참조하시오.

- [분할 및 정복 알고리즘]()

<br>

### 시간 복잡도

- O(NlogN)

![image](https://user-images.githubusercontent.com/79521972/153855999-07d32dac-d19c-4338-a41b-ba3d9ca8a70f.png)

우선 분할 과정에서 리스트의 크기가 1/2씩 감소하게 된다. 그래서 처음 n개의 리스트에서 분할을 진행하면 n/2크기의 서브 리스트 2개를 얻게 되고 이 상태에서 분할을 다시 진행하게 되면 n/4크기의 서브 리스트 4개를 얻게 된다.

이러한 분할 연산을 모든 서브 리스트들의 크기가 1이 될 때까지 반복해야 하는데 **리스트의 크기**가 2라면 1번, 4개라면 2번,  8개라면 3번을 반복 해야 크기가 1인 리스트를 얻을 수 다. 

따라서 분할 과정에서 O(log<sub>2</sub>n)의 시간복잡도를 갖게 된다.

<br>

![image](https://user-images.githubusercontent.com/79521972/153855972-1bfa732b-f4f7-41d3-8512-2d65a68fe02b.png)

또한 분할된 상태에서 리스트들을 다시 합칠 때(합병,Merge) 각 element들을 비교하면서 진행되기 때문에 리스트의 크기가 1인 상태일 때 총 n개의 리스트가 있을 것이므로 이때, **n번의 비교연산**을 수행하게 된다.

 이후 합쳐진 상태에서 리스트의 크기가 n/4이고 갯수가 4개라면 또 다시 n번의 비교연산이 필요하다. 결국 각 depth에서 n번 비교 연산을 갖게 되고 이는 O(n)의 시간 복잡도를 요구하게 된다.

![image](https://user-images.githubusercontent.com/79521972/153855949-f5328279-9c87-434f-b5ca-8a408bc3acab.png)

따라서 결과적으로 merge sort의 시간 복잡도는 분할과 합병과정을 <mark>합친</mark> O(nlog<sub>2</sub>n) 의 시간복잡도를 갖게 된다.

<br>

## Merge Sort 구현(자바)

merge sort를 구현하는 방법에는

1. In-place 방법으로 구현하는 것
2. Out-of-place 방법으로 구현하는 것

이 있는데 여기서는 1번 방법으로 구현을 해 보도록 할 것이다. Out-of-place 방법은 In-place 방법보다 훨씬 쉽기 때문에 혼자서도 구현을 해 볼 수 있을 것이다.

```java
package sort;

public class MergeSort implements ISort {
    
   	...
}
```

<br>

제일 처음 sort() 메소드를 살펴 보겠습니다. sort()메소드에서 In-place sort가 이루어진다.(void 리턴)

분할과정을 먼저 살펴보자.

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

초기시작이므로 처음에는 mergeSort()로 배열의 0번 부터 끝까지를 index를 인자로 넘겨 분할을 진행한다.

**분할(mergeSort())**

먼저 분할 과정에서 재귀 호출로 중간값을 찾아 이를 기준으로 큰 부분(오른쪽)과 작은 부분(왼쪽)의 인덱스로 나눠지게 하여 이 index를 기반으로 두 개의 서브 리스트들로 분할 되게 한다.

종료조건은  **low가 high보다 크거나** 같을 때, 즉 다시 말해서 **배열의 크기가 1이 되는 경우** 종료하는 것으로 한다. 종료 시 mergeSort를 호출한 곳으로 돌아간다.

<br>

다음으로 합병을 살펴 보자.

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

- 합병에 필요한 <mark>보조배열</mark>을 temp라는 이름으로 만들어 주고 이 배열의 크기는 합쳐진 배열의 크기와 같기 때문에 (high - low + 1)이다.
- 보조 배열의 인덱스로 사용하기 위한 idx 변수를 선언한다.
- left 변수는 low, 즉 분할된 왼쪽 리스트의 시작 index를 의미하고,
  right변수는 mid + 1, 즉 분할된 오른쪽 리스트의 시작 index를 의미하도록 한다.
-   첫 번째 while문
  - 이 반복문을 통해 분할 정렬 된 리스트의 합병이 될 것이다.
  - ( left <= mid and right <= high ) : left나 right 중 어느 index라도 리스트의 모든 값을 꺼내게 되면 종료하도록 한다.
  - 만약 arr[left] 값이 arr[right]보다 작거나 같으면 보조 배열 temp에 arr[left]값을 넣고 left 인덱스를 1 증가시킨다.
  - 위의 경우가 아닌 경우, 즉 arr[left]값이 arr[right]값보다 크면 보조 배열 temp에 arr[right]값을 넣고 right 인덱스를 1 증가 시킨다
  - 마지막으로 temp 배열의 idx도 증가시킨다.

- 두 번째 while문
  - 이 반복문에서 왼쪽 리스트에 값이 남아 있는 경우를 처리하는 것을 목표로 한다.
  - left 인덱스가 mid값보다 작으면 아직 왼쪽 서브 리스트에 값이 남아있는 것이기 때문에 
    남아있는 값들을 temp 배열에 쭉 넣어준다.

- 세 번째 while문 
  - 이 반복문에서는 오른쪽 서브 리스트에 값이 남아 있는 경우를 처리하는 것을 목표로 합한다.
  - right 인덱스가 high값보다 작으면 아직 오른쪽 서브 리스트에 값이 남아있는 것이기 때문에 남아있는 값들을 temp 배열에 쭉 넣어준다.

- 마지막으로 low 인덱스 부터 high 인덱스까지 temp 배열에 담았던 정렬된 리스트를 다시 arr배열에 담아줌으로써 마무리한다.(배열의 이동? 복사?)

<br>

코드를 봤을 때 이해가 잘 가지 않을 수 있기 때문에 그림과 함께 설명을 덧붙이겠다.



![image](https://user-images.githubusercontent.com/79521972/153868939-90235335-0754-4feb-8fd9-897c79c09afe.png)

- 초기 배열 상태가 21, 10, 12, 20, 25, 13, 15, 22라고 하자.
- 두 개의 리스트의 값들을 처음부터 하나씩 비교하여 두 개의 리스트의 값들 중 더 작은 값을 
  **보조 배열**(sorted)에 옮긴다.
- 둘 중 하나가 끝날 때까지 이 과정을 되풀이 한다(재귀적으로).
- 만약 둘 중에서 하나의 리스트가 먼저 끝나게 되면 나머지 리스트의 값들을 전부 보조 배열로 복사한다.
- 보조 배열을 원래의 리스트(arr)로 옮긴다.



## 합병 정렬의 시간 복잡도

- 분할 단계

  - 비교 연산과 이동 연산이 수행되지 않는다.

- 합병 단계

  - 비교 횟수
    

    ![image](https://user-images.githubusercontent.com/79521972/153869839-5f03cc8a-e732-457f-af28-c10e0115dbc0.png)

  - 재귀 호출의 depth (합병 단계의 수)

    - 테이터의 개수 n이 2의 거듭제곱이라고 가정(n=2<sup>k</sup>)했을 때, n=2<sup>3</sup>의 경우, n=2<sup>3</sup> -> n=2<sup>2</sup> -> n=2<sup>2</sup> -> n=2<sup>0</sup> 순으로 줄어들어 순환 호출의 깊이가 3임을 알 수 있다. 이것을 **일반화**하면 n=n=2<sup>k</sup> 경우, k(k=log₂n)임을 알 수 있다.
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

> 합병 정렬은 일반적인 경우 다음 시간에 배울 퀵 정렬보다 느리지만 어떠한 상황에서도 정확히 O(NlogN)을 보장할 수 있다는 점에서 매우 효율적인 알고리즘이다.



<br>

### 정렬 간 시간복잡도 비교

| 정렬 방식               | Average                  | Worst                    | Memory   | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| ----------------------- | ------------------------ | ------------------------ | -------- | ----------- | ------------- | --------------------------------- |
| Bubble 정렬             | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | O           | O             | 7.438                             |
| Selection 정렬          | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | X           | O             | 10.842                            |
| Insertion 정렬          | O(n<sup>2</sup>)         | O(n<sup>2</sup>)         | O(1)     | O           | O             | 22.894                            |
| Shell 정렬              | O(nlog<sub>2</sub>n)     | O(n<sup>2</sup>)         | O(1)     | X           | O             | 0.056                             |
| <mark>Merge 정렬</mark> | **O(nlog<sub>2</sub>n)** | **O(nlog<sub>2</sub>n)** | **O(n)** | **O**       | **X**         | **0.014**                         |
| Quick 정렬              | O(nlog<sub>2</sub>n)     | O(n<sup>2</sup>)         | O(1)     | X           | O             | 0.034                             |
| Heap 정렬               | O(nlog<sub>2</sub>n)     | O(nlog<sub>2</sub>n)     | O(1)     | X           | O             | 0.026                             |

<br>



**정렬(Sorting)**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Sorting(%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Bubble 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Bubble-Sort(%EB%B2%84%EB%B8%94-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Insertion 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Quick 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Quick-Sort(%ED%80%B5-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>



<br>

## 참고 자료

.



