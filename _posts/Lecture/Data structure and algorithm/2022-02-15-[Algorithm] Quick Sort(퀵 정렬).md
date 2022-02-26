---
layout: single
title: "[Algorithm] Quick Sort(퀵 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬', 'Quick sort', '퀵 정렬']
toc: true
toc_sticky: true
---

이번시간에는 퀵 정렬에 대해서 배워 보도록 하겠다.

저번 포스팅에서 설명했던 정렬 방식들(버블 정렬, 삽입 정렬, 선택 정렬) 은 가장 기본적인 수준의 정렬 방식들이다. 

그러나 이것들은 일반적으로 좋은 성능은 낼 수 없기 때문에 실제로 개발 시에는 잘 사용하지 않는다.

앞으로 배울 Merge / Quick 정렬은 굉장히 중요하기 때문에 자세히 알아보도록 하자.

<br>

## 퀵 정렬이란?

![img (3)](https://user-images.githubusercontent.com/79521972/155835114-17a1d93a-bcb2-4ea0-842e-fdd239d818d6.gif)

먼저 퀵정렬의 특징은 다음과 같다.

<br>

- 퀵 정렬(Quick Sort)은 합병 정렬(Merge Sort)과 마찬가지로 배열을 둘 씩 분할하며 정렬하는 과정을 거치기 때문에 시간복잡도 O(nlog<sub>2</sub>n)을 갖는다. 
  하지만 같은 시간 복잡도라도 실제 정렬에서는 합병 정렬보다 퀵 정렬이 훨씬 더 빠른 시간 안에 정렬이 가능하다.(이름처럼 매우 빠른 속도를 갖는다.)
  - 리스트 파트에서 배웠던 **ArrayList**가 메모리 공간 상에서 인접하게 위치해 있기 때문에 연산에 더 유리했다고 했던 것처럼 **퀵 정렬** 역시 컴퓨터의 하드웨어 특성 덕분에 더 빠른 연산 성질을 가질 수 있게 됩니다. → 참조 지역성(locality of reference)의 원리
    - 퀵 정렬은 알고리즘 자체의 특성 상 동일한 배열 내에서 자리를 이동시킨다. 그래서 인접한 데이터들 사이의 이동이 발생하기 때문에 제일 처음 배열에 접근 할 때만 실제 메모리에서 데이터를 가져오고 이후에는 캐시(cache)로 배열에 접근하기 때문에 메모리로 접근할 때보다 훨씬 더 빠른 접근이 가능한 것이다.
      이후 더 자세한 내용은 운영체제 수업에서 다루도록 하겠다.

  - 한 번 위치가 결정된 pivot 값은 이후의 연산에서는 해당 값을 제외하고 연산을 진행하기 때문에 정렬이 진행 될 수록, 즉 분할이 점점 진행 될 수록 계산해야 할 데이터의 수가 점점 줄어드는 특성이 있다. 따라서 데이터가 줄어들기 때문에 이에 따라서 정리할 속도도 줄어들게 되는 것이다.
- merge sort에서는 인메모리 정렬로 구현을 하더라도 정렬을 보조해 주는 추가 메모리 공간이 필요하다는 단점이 있었지만
- quick sort에서는 추가 공간을 더 사용하지 않고 합병 정렬과 비슷하기 **divide and conquer** 방식으로 진행이 된다다.
  - 분할 정복(Divide and Conquer) 방법
    - 문제를 작은 2개의 문제로 분리하고 각각을 해결한 다음, 결과를 모아서 원래의 문제를 해결하는 전략
    - 대개 순환(재귀) 호출을 이용하여 구현
    - **분할 정복 알고리즘**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

- 퀵 정렬은 **불안정 정렬** 방식에 속하며, 다른 원소와 비교만으로 정렬을 수행하는 **비교 정렬**에 속한다.



<br>

### 퀵 정렬의 개념

- 리스트 안에 있는 한 요소를 특정 기준에 따라 선택한다. 이렇게 고른 원소를 피벗(pivot)이라고 한다.

- 하나의 리스트를 pivot을 기준으로 두 개의 비균등한 크기로 **분할**하고 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 하는 방법이다.

  - 피벗을 기준으로 피벗보다 작은 요소들은 피벗의 왼쪽에 위치시키고 피벗보다 큰 요소는 오른쪽에 위치시킨다.
  - 피벗을 제외한 왼쪽 리스트와 오른쪽 리스트 각각을 다시 정렬한다.
    - 분할된 서브 리스트에 대해서는 순환 호출을 이용하여 정렬을 반복한다.
    - 서브 리스트에서도 재차 피벗을 정하고 이를 기준으로 2개의 서브 리스트로 다시 나누어 주는 과정을 반복한다.

  - 부분 리스트들이 더 이상 분할이 불가능할 때까지 반복한다.
    - 리스트의 크기가 0이나 1이 될 때까지 반복한다.

- 단계는 다음과 같이 이루어져 있다.
  1. **분할(Divide)**: 입력 배열을 pivot을 기준으로 비균등하게 2개의 서브 리스트(pivot을 중심으로 왼쪽: pivot보다 작은 요소들, 오른쪽: pivot보다 큰 요소들)로 분할한다.
  2. **정복(Conquer)**: 서브 리스트를 정렬한다. 서브 리스트의 크기가 조금 크다고 생각이 들면 재귀 호출을 이용하여 다시 분할 정복 방법(Divide and conquer)을 적용한다.
  3. **결합(Combine)**: 정렬된 부분 배열들을 하나의 배열에 합병한다.

  - 재귀 호출이 한 번 진행될 때마다 최소한 하나의 pivot은 최종적으로 위치가 정해지기 때문에 이 알고리즘은 반드시 끝난다는 것을 보장 받을 수 있는 것이다.



<br>

---

자 그럼 이제 그림으로 한 번 퀵 정렬을 이해해 보도록 하겠다.

1. pivot값을 먼저 정해준다.

- pivot값은 리스트의 가장 앞에 위치한 원소도 가능하고 가장 뒤에 위치한 원소도 가능하다. 어느 원소로 잡아도 되지만 나는 가장 중간에 위치한 '5'로 정하였다.

![image](https://user-images.githubusercontent.com/79521972/153881076-391ba485-1f88-41f1-86b9-f41099d05ee8.png)

<br>

2. 그렇게 해서 이 pivot값을 기준으로 원소들을 **재배치**한다. 

- 재배치하는 방식은 아래와 같이 5보다 작은 값들은 5의 왼쪽으로 5보다 큰 값들은 5의 오른쪽으로 이동시키는 방식이다.

![image-20220214232337193](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20220214232337193.png)

이 때 pivot값의 index가 바뀌는 것은 자연스러운 일로 pivot값 자체가 중요한 것이기 때문에 크게 신경 쓰시지 않아도 된다.

- 그럼 pivot값을 중심으로 두 개의 서브 리스트가 생긴 것이다.

<br>

3. 이 서브 리스트들에서 각각 pivot값을 다시 정해 준다.

- 나는 왼쪽 서브 리스트에서는 중간 값인 1, 오른쪽 서브리스트에서는 6으로 잡았다.

![image](https://user-images.githubusercontent.com/79521972/153882595-17470368-e60e-43db-aaca-b98b128eb5ff.png)

4. 그럼 다시 이 pivot값을 중심으로 대소 비교를 통한 재배치 과정을 진행한다.

- 여기서 오른쪽의 서브 리스트는 원소의 갯수가 하나만 남았기 때문에 더 이상 분할 할 수가 없으므로 오른쪽의 퀵 정렬은 종료가 된다.
  왼쪽의 서브 리스트는 아직 정렬 할 원소가 남아있기 때문에 해당 리스트에서 pivot값을 정해 계속해서 정렬해 주도록 하겠다.

<br>

- pivot값을 중간값인 3으로 잡고 진행해 보겠다.

![image](https://user-images.githubusercontent.com/79521972/153884073-7aa8b019-3a32-467e-988f-38c4f01aa986.png)

- 또 다시 pivot값을 기준으로 재배치하여 분할을 진행한다.
- 분할을 통해 나온 양쪽 서브 리스트 원소의 갯수가 1개씩만 남았으므로 퀵 정렬이 완전히 종료가 되고 실제로 정렬된 배열을 보면 오름차순으로 잘 정렬 되었음을 볼 수있을 것이다.

<br>

퀵 정렬을 움직이는 애니메이션을 통해 보면 다음과 같다.

여기서는 pivot값을 서브 리스트의 중간 값이 아니라 가장 맨 오른쪽 값으로 정하여 진행한 모습이다.

- 실제로 퀵 정렬을 할 때 정하는 피벗값을 리스트의 맨 오른쪽 값으로 정하는 경우가 많다.

![Sorting_quicksort_anim](https://user-images.githubusercontent.com/79521972/153884489-eb997f20-34e2-4275-afa8-54b85494dcad.gif)

[출처: 위키백과 - 퀵 정렬](https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC)

<br>

### Pivot 결정의 중요성

아래와 같은 배열을 정렬해야 한다고 하고 피벗값을 가장 좌측에 있는 값으로 잡는다고 해보자.

![image](https://user-images.githubusercontent.com/79521972/155835775-b44ae36f-37ef-4f71-8537-c513e6d30ae1.png)

<br>

이를 오름차순으로 정렬하려 하는데 뒤의 모든 값들이 내림차순으로 정렬되어 있고 41이 이미 가장 큰 숫자이다.

이 상태에서 1차 정렬을 진행하면 다음과 같이 된다.

![image](https://user-images.githubusercontent.com/79521972/155835938-d4582793-f139-4e84-940b-ff85f932c39d.png)

이처럼 피벗의 위치를 찾았더니 모든 값과 비교하였을 때 전부 swap이 되었을 것이고, 또 가장 마지막에 위치해 있을 것이다.

<br>

피벗이 적당히 중간쯤에 위치했다면 양 쪽의 부분 집합이 균등하게 분배돼서 전체 비교 횟수를 월등하게 줄일 수있었음에도 이러한 현상이 반복되었다면 비교해야 하는 횟수는 계속해서 늘어날 수 밖에 없는 것이다.

- 사실상 버블 정렬과 다를게 없는 것이다.

<br>

이런 최악의 경우, 시간 복잡도는 O(n<sup>2</sup>)를 갖게 되며 퀵 정렬이 더 이상 제 기능을 하지 못하게 되는 것이다.(~~slow 정렬?~~)

<br>

<br>

그래서 이처럼 피벗값을 어떤 것으로 잡을 지는 상황에 따라 매우 다르다. 항상 최 우측 혹은 좌측 값을 피벗값을 설정하는 것도 때에 따라서는 최악의 성능을 갖게 될 수 있기 때문이다.

그렇기 때문에 피벗을 정하는 방법을 다음과 같이 생각해 볼 수 있다.

<br>

**1\) 그냥 무작위로 선택하기**

- 랜덤하게 설정하면 항상 최악의 경우가 되진 않을 것이다. 그러나 여전히 최악의 case가 생겨날 수 있다.
  - 평균적으로 O(nlog<sub>2</sub>n)의 시간 복잡도를 유지한다.

<br>

**2\) Median-Of-3 method**

이 방법은 배열의 여러 값들 중에서 가장 좌측, 우측, 중간 이렇게 3개의 값들을 뽑고, 이 3개의 값들에 대해서 정렬을 한 뒤, 그 중 중간 값을 가장 좌측 또는 우측으로 이동시켜서 피벗으로 설정하는 방법이다.

<br>

이 값이 피벗으로 사용되어 전체 배열을 균등하게 분할할 수 있다는 보장은 없지만, 최소한 이 값이 전체 중 최대/최소 값에는 해당 되지 않는 다는 것을 보장하기 때문에 중심 극한 정리에 따라서 평균 O(nlog<sub>2</sub>n)의 시간 복잡도를 유지할 수 있는 것이다.

예를 들어, 다음과 같은 배열을 보자

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 35   | 33   | 42   | 10   | 14   | 19   | 27   | 44   | 26   | 31   |

이 배열에서 left는 35, mid는 14, right는 31인데 정렬을 하면 right값인 31이 median값이 되고 이 값을 pivot 값으로 사용한다는 의미이다. 

<br>

## 퀵 정렬 구현

퀵 정렬 또한 **분할 정복 방법**으로 구현할 것이기 때문에 quickSort라는 메소드를 재귀호출하여 구현해 보도록 하겠다.

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

sort() 메소드에서는 본격적으로 퀵 정렬을 시작할 것이기 때문에 quickSort에 배열과 시작 인덱스는 0 마지막 인덱스는 배열의 마지막 인덱스를 넣어주면서 호출한다.

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

- 가장 먼저 quickSort도 종료 조건으로 low가 high와 같아지는 상황, 즉 분할을 모두 마쳐 서브 리스트의 크기가 1인 상황, 즉 원소가 하나인 상황에 return을 한다.
- 이제 퀵 정렬을 시작해 볼 것이다. 퀵 정렬에서는 가장 먼저 **pivot**값을 정해주어야 한다.
  여태 그래왔듯 중간 값으로 pivot값을 정해주도록 한다. '(low + high) / 2'와 같이 해주지 않은 이유는 overflowException을 방지하기 위함이다.
  - int형 변수의 크기 범위는 2<sup>-31</sup>~2<sup>31</sup> 이기 때문이다.
  
- pivotValue에는 pivot인덱스에 해당 하는 pivot값을 넣어주도록 한다.
  - 우리는 이 값을 아주 중요하게 다룰 것이다.

- left와 right에는 배열의 low(시작), high(끝)값을 각각 넣어준다.
- 첫 번째 while문
  - left의 값과 right의 값이 겹치지 않을 때 까지 다음 내용을 반복한다.
  - 내부 while문_1
    - 배열의 left값이 pivot값보다 작다면 잘 배치 되어있는 것으로 다음 left index를 순회해야 하기 때문에 left 값을 1 증가시켜 다음 index로 이동한다.
  - 내부 while문_2
    - 배열의 right값이 pivot값보다 크다면 잘 배치 되어있는 것으로 다음 right index를 순회해야 하기 때문에 right값을 1 감소시켜 다음 index로 이동한다.
  - 만약 left 와 right이 이동하다가 서로 만나게 되었거나 서로 교차하지 않은 상황이라면 배열의 left의 값과 right의 값을 서로 바꾸어 주는 과정을 진행 후 left와 right의 index를 각자 방향으로 이동시킨다.
- 이를 재귀함수를 통해 왼쪽과 오른쪽 각각에 대하여 퀵 정렬을 진행한다.



<br>

이해를 위해 그림을 통해 설명하도록 하겠다.

![image](https://user-images.githubusercontent.com/79521972/153900024-530417b3-9082-41c2-9e5f-bac31ab2fb44.png)



- pivot값이 5로 잡혔기 때문에 처음 left의 index는 0번에서 pivot이 위치한 곳까지 오게되고 right은 맨 끝에서 2가 위치한 곳까지 오게된다.
  - left 서브 리스트의 원소는 모두 5보다 작기 때문에 pivot까지 온 것이고
  - right 서브 리스트의 원소중 5보다 크지 않은 2가 존재하여 해당 위치에서 멈춘 모습이다.
- 그리고 왼쪽 인덱스와 오른쪽 인덱스가 아직 교차하지 않은 상태이므로 left와 right의 값을 바꿔준다.
- swap이후 다시 while문으로 돌아와 진행한다.
- 이 상태에서 left는 7을 만나 멈추고 right은 이미 pivot이기 때문에 그대로이다.
- 이 때도 역시 left와 right index가 아직 서로 교차하기 전이므로 left값과 right값을 바꾸어 준다.
  - 그러고 나서 left와 right가 교차하게 되어 왼쪽 리스트와 오른쪽 리스트가 pivot에 의해 정렬되었다.
- 이를 서브 리스트의 크기가 1이 될 때까지 반복하는 것이다.

<br>

<br>

또 다른 예로 pivot을 맨 왼쪽으로 잡은 경우를 도식화한 것을 보고 마무리 하도록 하겠다.

![image](https://user-images.githubusercontent.com/79521972/153901057-238955ac-bb29-453b-9ff0-6978bf6a308f.png)



**[장점]**

1. 특정 상태가 아닌 이상 평균 시간 복잡도는 nlog<sub>2</sub>n이며, 다른 nlog<sub>2</sub>n 알고리즘에 비해 대체적으로 속도가 매우 빠르다. 유사하게 nlog<sub>2</sub>n 정렬 알고리즘 중 <span style="color:red">분할정복 방식인 merge sort에 비해 2~3배정도 빠르다.</span> (정리 부분의 표 참고)

2. 추가적인 별도의 메모리를 필요로하지 않으며 재귀 호출 스택프레임에 의한 공간복잡도는 log<sub>2</sub>n으로 메모리를 적게 소비한다.

**[단점]**

1. 특정 조건하에 성능이 급격하게 떨어진다.
   - 내림 차순으로 정렬되어 있는데 피벗 값을 배열의 첫 index로 잡는 경우
   - 피벗값을 Median-Of-3 방법을 통해 잘 정하도록 하자.
2. 재귀 호출을 사용하기 때문에 재귀를 사용하지 못하는 환경일 경우 그 구현이 매우 복잡해진다.

<br>

### 퀵 정렬의 시간복잡도

- 최선의 경우

  - 비교 횟수
  - ![image](https://user-images.githubusercontent.com/79521972/153885858-13e1f947-e1c5-4f82-9187-1c32abbd63d1.png)

  - 재귀 호출의 depth
    - 레코드 개수 n이 2의 거듭제곱이라고 가정(n=2<sup>k</sup>)했을 때, n = 2<sup>3</sup>인 경우,
      2<sup>3</sup> -> 2<sup>2</sup> -> 2<sup>1</sup> -> 2<sup>0</sup> 순으로 줄어들어 재귀 호출의 depth가 3인 것을 확인 할 수 있을 것이다. 이를 일반화하면 n = 2<sup>k</sup>인 경우, k(k = log<sub>2</sub>n) 임을 알 수 있다.
    - k = log<sub>2</sub>n
  - 각 재귀 호출 단계에서의 비교 연산
    - 각 재귀 호출에서는 전체 리스트의 대부분의 데이터 값을 비교해야 하므로 평균 n번 정도의 비교가 이루어진다.
    - 평균 n번
  -  <span style="color:red">재귀 호출의 깊이</span> ×  <span style="color:red">각 재귀 호출 단계의 비교 연산</span> = **nlog<sub>2</sub>n**

- 이동 횟수 

  - 비교 횟수보다 적기때문에 무시할 수 있다.

- 최선의 경우, 시간 복잡도 T(n) = **O(nlog₂n)**

<br>

- 최악의 경우 (Solution : <u>Median of Three</u>)
  - 리스트가 계속 불균형하게 나누어지는 경우 
    - 위에서 설명한 예시에 더불어 이미 정렬되어 있는 리스트에 대하여 퀵 정렬을 실행하는 경우도 있다.

![image](https://user-images.githubusercontent.com/79521972/153885774-c5aac689-ce6e-4008-9f36-d31648096eb7.png)



-  비교 횟수	
   - 재귀 호출의 depth
     - 레코드의 개수 n이 2의 거듭제곱이라고 가정(n=2<sup>k</sup>)했을 때, 재귀 호출의 depth는 n이다.
     - n번
   - 각 순환 호출 단계의 비교 연산
     - 각 순환 호출에서는 전체 리스트의 대부분의 데이터 값을 비교해야 하므로 평균 n번 정도의 비교가 이루어진다.
     - 평균 n번
   - <span style="color:red">재귀 호출의 깊이</span> ×  <span style="color:red">각 재귀 호출 단계의 비교 연산</span> = **n<sup>2</sup>**
-  이동 횟수
   - 비교 횟수보다 적기 때문에 무시할 수 있다.
-  최악의 경우, 시간 복잡도 T(n) = **O(n<sup>2</sup>)**

<br>

- 평균 
  - 평균 T(n) = **O(nlog₂n)**
  - 시간 복잡도가 O(nlog₂n)를 가지는 다른 정렬 알고리즘과 비교했을 때도 퀵 정렬이 월등히 빠르다.
  - 퀵 정렬이 불필요한 데이터의 이동을 줄이고 **먼 거리의 데이터를 교환**할 뿐만 아니라, <span style="color:blue">한 번 결정된 pivot들이 추후 연산에서 제외되는 특성</span> 때문이다.

<br>



### 정렬 간 시간복잡도 비교

| 정렬 방식               | Average                  | Worst                | Memory   | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| ----------------------- | ------------------------ | -------------------- | -------- | ----------- | ------------- | --------------------------------- |
| Bubble 정렬             | O(n<sup>2</sup>)         | O(n<sup>2</sup>)     | O(1)     | O           | O             | 7.438                             |
| Selection 정렬          | O(n<sup>2</sup>)         | O(n<sup>2</sup>)     | O(1)     | X           | O             | 10.842                            |
| Insertion 정렬          | O(n<sup>2</sup>)         | O(n<sup>2</sup>)     | O(1)     | O           | O             | 22.894                            |
| Shell 정렬              | O(nlog<sub>2</sub>n)     | O(n<sup>2</sup>)     | O(1)     | X           | O             | 0.056                             |
| Merge 정렬              | O(nlog<sub>2</sub>n)     | O(nlog<sub>2</sub>n) | O(n)     | O           | X             | 0.014                             |
| <mark>Quick 정렬</mark> | **O(nlog<sub>2</sub>n)** | **O(n<sup>2</sup>)** | **O(1)** | **X**       | **O**         | **0.034**                         |
| Heap 정렬               | O(nlog<sub>2</sub>n)     | O(nlog<sub>2</sub>n) | O(1)     | X           | O             | 0.026                             |

<br>



**정렬(Sorting)**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Sorting(%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Bubble 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Bubble-Sort(%EB%B2%84%EB%B8%94-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Insertion 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Counting 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Radix 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Bucket 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>



<br>

## 참고 자료

.

