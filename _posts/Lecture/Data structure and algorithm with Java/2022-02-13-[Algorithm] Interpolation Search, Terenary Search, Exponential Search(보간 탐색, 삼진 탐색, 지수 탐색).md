---
layout: single
title: "[Algorithm] 보간 탐색, 삼진 탐색, 지수 탐색(Interpolation, Terenary, Exponential Search)"
categories: ['Lecture', 'Data structures and algorithms with Java', 'Algorithm', 'BinarySearch']
tag: ['Data structure', 'Algorithm', 'Interpolation Search', '보간탐색']
toc_sticky: true
---

이번 시간에는 보간 탐색(Interpolation Search), 삼진 탐색(Ternary Search), 지수 탐색(Exponential Search)에 대해서 알아 보도록 하겠다.

<br>

## 보간 탐색

보간 탐색(Interpoation search)은 **정렬된 리스트**에서 범위를 좁혀 가면서 탐색해 나가는 알고리즘이다. 

보간 탐색은 전화부에서 이름(책의 항목이 정렬되는 키 값)을 검색하는 방법과 매우 유사하다.

<br>

동작하는 방식이 이진 탐색과 거의 유사하지만 탐색 위치를 정하는 방식에 있어서 차이점이 존재한다.

<br>

이진 탐색과 보간 탐색에서 탐색 위치를 정하는 공식은 다음과 같다.

- arr[]: 데이터가 들어가 있는 배열
- low: arr[] 배열의 시작 index
- high: arr[] 배열의 마지막 index
- target: 검색 값
- pos: 탐색 위치 index

<br>

**이진 탐색은 탐색 위치**가 중간 값을 잡았기 때문에 다음과 같은 공식이 나왔다.

- >  pos = (low + high) /2 

- > or    = low + (high - low) / 2

<br>

**보간 탐색의 탐색 위치**를 정하는 공식은 다음과 같다.

```
pos = low + (target - arr[low]) * (high - low) / (arr[high] - arr[low])

* low : 현재 구간의 시작 index
* high: 현재 구간의 마지막 index
* target: 찾고자 하는 Data(key)
```

 

![image](https://user-images.githubusercontent.com/79521972/155676400-c3452525-92d9-4b0c-8cdf-5ccc34d733ac.png)

위 그림은 검색 값 **3**을 찾을 때 첫 번째 탐색 위치를 보여준다.

이진 탐색은 항상 중간 위치로 탐색 위치를 결정하는 반면, 보간 탐색은 검색 key값에 따라 다른 위치로 이동하게 된다.

예를 들어, 검색 key 값이 상대적으로 앞쪽에 있다고 판단되면 비교적 앞쪽에서 탐색을 진행하게 되고 뒷쪽에 있다고 판단되면 뒷쪽에서 탐색을 진행하는 것이다.

<br>

데이터가 선형으로 분포되어 있다면 위의 그림처럼 보간 탐색은 한 번에 데이터를 찾을 수 있기도 하다. 이처럼 보간 탐색은 <mark>데이터가 선형으로 분포되어 있을 때 가장 검색 효율이 좋다.</mark>

> 데이터가 선형으로 분포되어 있다는 것은 데이터가 인덱스 값에 비례하여 분포해 있다는 의미이다.

<br>

### 보간 탐색 방식

보간 탐색의 방식을 살펴보자.

보간 탐색은 다른 탐색과 마찬가지로 정렬된 배열에서만 사용할 수 있고 이진 탐색과 동작 방식은 동일하다.

그 과정은 다음과 같다.

1. 탐색 위치(pos)를 구한다.
2. 탐색 위치(pos) 값과 구하고자 하는 검색 key값을 비교한다.
   - 값이 같다면 종료
   - arr[pos] < key(검색 값이 더 큰) 인 경우, 탐색 위치 기준 배열의 오른쪽 구간을 대상으로 탐색한다.
     - low = pos + 1
   - arr[pos] > key(검색 값이 더 작은) 인 경우, 탐색 위치 기준 배열의 왼쪽 구간을 대상으로 탐색한다.
     - high = pos - 1



<br>

### 종료 조건

보간 탐색의 종료 조건에도 두 가지 방법이 존재하며 이 중 한 가지라도 만족하면 탐색이 종료된다.

<br>

1. 데이터 탐색에 성공한 경우
   - 검색 key 값을 발견한 경우
   - arr[pos] == key

<br>

2. 검색에 실패한 경우

- 검색 범위를 벗어나게 되는 경우(반복문 종료)
- target < arr[low] && target > arr[high]

<br>

### Search 방식

아래의 그림을 보면서 보간 탐색이 어떻게 이루어 지는지 보도록 하겠다.

보간 탐색을 통해 다음 배열에서 32라는 key값을 찾는 과정을 보도록 하자.

1. 탐색 위치 pos를 구한다.

```
pos = low + (target - arr[low])*(high - low)/(arr[high] - arr[low])
    = 0 + (32 - 5)*(9 - 0)/(60 - 5) = 4.42 = 4
```

![image](https://user-images.githubusercontent.com/79521972/155677713-b0450c59-082f-463f-9d09-55127c6337cf.png)

계산 결과 나온 pos값은 4이고 배열의 4번 index가 pos가 된다.

<br>

2. arr[pos] (27)와 검색 key값(32)을 비교한다.
   - arr[pos] < target 이므로 배열의 오른쪽 구간을 탐색 범위로 하여 탐색을 진행한다.

```
low = pos + 1 = 4 + 1 =5
```

![image](https://user-images.githubusercontent.com/79521972/155678105-1a9724f5-9e89-4743-80d6-6b822439be44.png)

<br>

3. 바뀐 값으로 다시 탐색 위치 pos를 구한다.

```
pos = low + (target - arr[low])*(high - low)/(arr[high] - arr[low])
    = 5 + (32 - 32)*(9 - 5)/(60 - 32) = 5
```

![image](https://user-images.githubusercontent.com/79521972/155678303-42d1f576-5a41-4a25-a979-b84476ea9ec9.png)

<br>

4. arr[pos] 값과 검색 key값을 비교한다.
   - 두 값이 같기 때문에 원하는 key값을 찾은 것이므로 탐색을 종료한다.



<br>

## 보간 탐색 구현

```java
int interpolationSearch(int arr[], int n, int target)
{
   
  int low = 0, high = (n - 1);
  int pos = 0;

  while (arr[low] != arr[high] && target >= arr[low] && target <= arr[high]) {
       pos = low + (((double)(high - low) / (arr[high] - arr[low])) * (target - arr[low]));

        if (arr[pos] == target)
          return pos;
        else if (arr[pos] > target)
          high = pos - 1;
        else
          low = pos + 1;
  }      

  if (target == arr[low])
    return low;
  else
    return -1;
}
```

- 위에서 진행한 것을 통해 코드로 구현한다.

몇몇 특징 적인 부분만 설명을 하겠다.

1. arr[low] == arr[high] 일 때 pos값은 low이다.
2. pos값을 구할 때 드는 시간 소모를 줄이기 위해 arr[low] != arr[high] 조건을 추가하여 반복문을 벗어나면 target값과 arr[low] 값을 비교하는 logic을 추가하였다.

<br>

### 시간 복잡도

보간 탐색은 데이터가 선형적으로 균등하게 분포가 되어 있다면 평균 O(log(logn))의 시간 복잡도가 요구 되지만 최악의 경우 데이터가 기하 급수적으로 늘어난다면 O(n)의 시간 복잡도를 소모할 수도 있다.

| 연산   | Best | Average      | Worst |
| ------ | ---- | ------------ | ----- |
| Search | O(1) | O(log(logn)) | O(n)  |

<br>





## 삼진 탐색(Ternary Search)

삼진 탐색은 이진 탐색과 진짜 거의 동일하다고 볼 수 있는데, 이진 탐색은 중앙을 기준으로 좌/우로 구간을 나누었다면 <span style="color:red">삼진은 왼쪽, 중간, 오른쪽 3개의 구간으로 나누는 방식이다.</span>

<br>

mid1 값을 좌측 중간값, mid2 값을 우측 중간값이라고 하자. 그러면 다음과 같은 식을 통해 탐색 범위가 정해진다.

```
mid1 = low + (high - low) / 3
mid2 = right - (high - low) / 3
```

이 식을 보고 드는 의문이 있을 것이다.

> 동일한 O(logn)의 시간 복잡도가 요구외어도 밑의 같이 삼진은 3이고 이진 2이기 때문에 더 효과 적인 것인가?

<br>

하지만 예상과 다르기 실제로는 <mark>이진 탐색이 훨씬 더 선호된다</mark>. 그 이유는 최악의 경우, 삼진 탐색 방식은 비교를 더 많이 해야하기 때문이다.



<br>

#### 삼진 탐색 구현

```java
// 재귀 호출 사용
public static int recursiveSearch(int[] arr, int x, int start, int end){
    if(start > end) 
        return -1;
    
    int mid1 = start + (end - start) / 3;
    int mid2 = end - (end - start) / 3;

    if(arr[mid1] == x){
        return mid1;
    }

    if(arr[mid2] == x){
        return mid2;
    }

    if(x < arr[mid1]){
        return recursiveSearch(arr, x, 0, mid1-1);
    } else if(x > arr[mid2]){
        return recursiveSearch(arr, x, mid2+1, end);
    } else {
        return recursiveSearch(arr, x, mid1+1, mid2-1);
    }
}
```

<br>

## 지수 탐색(Exponential Search)

이것도 크게 다른 점은 없지만 찾는 방식이 조금 다르다.

1, 2, 4, 8, ... 이런 식으로 탐색 index 값을 지수 형태로 높여가면서 구간을 탐색하는 방식이다. 다른 탐색과 마찬가지로 이런 과정을 통해 탐색을 진행하다가 해당되는 범위를 찾으면 그 안에서 다시 선형 혹은 이진 탐색을 통해 최종 값을 찾는 것이다.

시간 복잡도는 O(logn)이다.

<br>

#### 지수 탐색 구현

```java
public static int search(int[] arr, int x){

        // 가장 앞의 위치에 있다면 바로 반환, 1부터 시작할 것이기 때문
        if(arr[0] == x){
            return 0;
        }

        int i=1;
        // index 값이 배열 크기보다 작아야 그 안에서 구간을 찾는다.
        // 또한 배열의 값이 더 작은 동안에만 수행하여 구간을 찾는다.
        while(i < arr.length && arr[i] <= x){
            i = i * 2;
        }

        // 찾아낸 구간에서 이진 탐색으로 수행
        // i값이 전체 배열의 범위를 넘어갈 수 있으니 Math.min으로 설정
        return Arrays.binarySearch(arr, i/2, Math.min(i, arr.length-1), x);
    }
```

<br>





