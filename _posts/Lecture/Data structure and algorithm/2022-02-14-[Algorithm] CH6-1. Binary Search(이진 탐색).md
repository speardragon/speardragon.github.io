---
layout: single
title: "[Algorithm] CH6-1. Binary Search(이진 탐색)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'BinarySearch']
tag: ['Data structure', 'Algorithm', 'Binary Search', '이진 탐색']
toc: true
toc_sticky: true
---

<br>

# 정렬

이번 CH6에서는 `정렬`에 대해 배워 보도록 하겠습니다.

정렬을 배우기 앞서 이진 탐색 내용이 공부가 되어있으면 편하기 때문에 우선은 이진 탐색에 대해서 먼저 알아보도록 하겠습니다.

<br>

## Binary Search - 이진 탐색

자료구조 및 알고리즘 강의 초반에 시간복잡도를 설명하면서 이진 탐색에 대한 내용을 간단히 설명을 했었습니다.

다시 설명하자면 이진 탐색은 **오름차순**으로 정렬되어 있는 리스트 내에서 특정 값의 index를 찾는 알고리즘입니다.

다음과 같은 리스트를 생각해 봅시다.

![image](https://user-images.githubusercontent.com/79521972/153797551-d603c486-574c-4fb5-9205-f94c9d886c60.png)

1부터 20까지의 숫자가 들어있는 리스트에서 특정 값의 위치를 찾는 방법 중 가장 효율 적인 방법은 무엇일까요?

숫자 7을 찾는다고 가정해 봅시다.

숫자가 정렬되어 있는 것은 보장 받지만 리스트 안의 숫자가 모두 연속된 숫자임은 보장 받지 못합니다. 그렇기 때문에 리스트의 7번째 값을 가져온다고 해서 7을 가져오지는 못할 것입니다.
<br>

![image](https://user-images.githubusercontent.com/79521972/153797804-749fd68b-61c2-4c7a-8745-228684b407fa.png)

이진 정렬에서는 먼저 가운데 숫자를 확인합니다. 이 리스트에서는 11이라는 숫자가 되겠네요.

<br>

![image](https://user-images.githubusercontent.com/79521972/153798087-06885c4a-0a3d-4421-802e-eac902dc137a.png)

이 리스트는 오름차순으로 정렬되어 있기 때문에 11이라는 숫자를 기준으로 더 작은 숫자는 왼쪽에 11보다 더 큰 숫자는 오른쪽에 있음을 보장 받습니다. 우리가 찾고자 하는 7은 11보다 작기 때문에 11의 왼쪽에 있기 때문에 왼쪽 값을 다시 확인합니다.

![image](https://user-images.githubusercontent.com/79521972/153798306-c87109d3-caf0-4e26-8a52-4fbded86ebc5.png)

1부터 11까지 리스트에서 가장 가운데 숫자는 5입니다. 따라서 마찬가지로 5보다 왼쪽에는 5보다 작은 숫자가 5를 기준으로 오른쪽에는 5보다 큰 숫자가 있을 것 입니다.
<br>

다시 남은 리스트의 가운데 값을 확인해 보니 9가 나왔습니다.

![image](https://user-images.githubusercontent.com/79521972/153798409-9c80c902-c271-4cfc-8288-d7a1ca795507.png)

여기서 다시 7은 9보다 작기 때문에 9의 왼쪽 남은 부분에서 탐색을 진행하여 7을 찾게 됩니다.

이처럼 한 번 탐색을 진행 할 때마다 값의 범위가 1/2씩 줄어들기 때문에 이진 탐색(binary search)라는 이름이 붙여지게 된 것입니다.

<br>

그래서 binary search는 한 번의 시행마다 우리가 봐야하는 데이터의 크기가 1/2 씩 줄어든다는 특징 때문에 빠른 속도를 가질 수 있습니다.

- 시간복잡도 O(logN)
- ( N = 100 ) vs. (logN = 6.64)

이것이 시간 복잡도 O(N)을 갖는 것과 얼마나 차이가 있는 지를 계산 했을 때 자료의 크기가 100이라면 앞에서 하나하나 탐색해서 찾으려면 최대 100 번만큼의 시간이 소요되는 반면, binary search를 사용하면 log100의 값으로 대략 6.64 번 만에 찾을 수 있는 것입니다.

데이터의 크기가 커질 수록 훨씬 더 이 차이가 심해지기 때문에 그만큼 더 효율적이게 되는 것입니다.

하지만 binary search는 정렬된 조건 하에서만 사용 가능 하기 때문에 이러한 제약 조건 또한 존재합니다.



## Binary Search 구현

이제는 그럼 BinarySearch가 어떻게 동작하는 지에 대해 구현을 직접 해 보도록 하겠습니다.

```java
package sort;

public class BinarySearch {
    
    ...
}
```

위의 코드와 같이 BinarySearch 클래스에 구현을 하도록 하겠습니다.

<br>

알고리즘 순서는 다음과 같습니다.

1.  데이터의 중간 인덱스 값을 찾는다.
2.  중간 인덱스 위치를 기준으로 arr를 절반으로 나눈다.
3.  나눠진 절반의 리스트에서 target 값을 찾는다.

<br>



```java
public int search(int[] arr, int target) {
    
    int left = 0;
    int right = arr.length - 1;
    
    int mid;
    while(left <= right) { // 종료조건
        mid = left + ((right - left) / 2);
        
        if(arr[mid] == target) {
            return mid;
        }
        
        if(arr[mid] < target) {
            left = mid + 1;
            
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

먼저 리스트의 양 끝을 나타내는 변수 left와 right을 선언 후 초기화 합니다. 그리고 중간 값을 나타내는 mid 변수도 선언합니다.

반복문을 돌며 이진 탐색을 할 것인데 while문의 종료 조건은 left보다 right가 클 때, 즉 모든 arr의 데이터를 본 경우에 종료하기로 합니다.

중간 값인 mid에 중간 index를 넣기 위한 작업을 합니다. 이에 대한 수식은 아래와 같이

```
mid = (left + right) / 2
```

처럼 나타낼 수 있지만 이렇게 하면 변수 mid는 int형 변수이기 때문에 들어갈 수 있는 숫자의 범위가 존재합니다.(-2^31 ~ 2^31) 그렇기 때문에 이 범위를 넘어가게 되면 overflowExeception이 발생 할 수 있습니다. 그래서 이를 막기 위해서 

```
mid = left + ((right - left) / 2);
```

와 같이 식을 표현해 준 것입니다.

이제 본격적으로 이진 탐색에 들어값니다.

- 만약 arr[mid] 값이 우리가 찾고자 하는 target값과 같다면 현재의 index값을 반환합니다.
- 아닌 경우에 더불어 arr[mid] 값이 target보다 작다면 left의 위치를 mid보다 한 칸 위로 올려주어야 리스트의 범위가 알맞게 좁혀지기 때문에 left = mid + 1을 해 주었습니다.
- arr[mid] 값이 target보다 크다면 right의 위치를 mid보다 한 칸 아래로 내려야 리스트의 범위가 좁혀지므로 right = mid - 1을 해 주었습니다.

마지막으로 이 while문을 다 돌았는데도 arr[mid]값이 target값과 존재하지 않는다면 return 되지 않고 while문을 빠져나와 -1을 반환하게 됩니다.

















