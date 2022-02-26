---
layout: single
title: "[Algorithm] Insert Sort(삽입 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Insert Sort', '삽입 정렬', '정렬', 'Sorting']
toc: true
toc_sticky: true
---

이번 시간에는 삽입 정렬(Insertion Sorting)에 대해 배워 보도록 하겠다.

![img (1)](https://user-images.githubusercontent.com/79521972/155829980-1232f05e-23f1-49c8-9edf-d2c04bb33e14.gif)

<br>

### 삽입 정렬이란?

삽입 정렬은 특정 데이터를 리스트의 앞에서부터 <span style="color:red">이미 정렬된 서브 리스트</span>의 값들과 비교하여 자신의 위치에 삽입하는 방식이다. 

이때 서브 리스트는 이미 정렬이 되어있기 때문에 서브 리스트 안에서도 자신이 삽입이 되어야 할 위치가 정해져 있을 것이다. 

그 위치에 데이터를 삽입하는 것이 바로 삽입 정렬이다.

<br>

예를 들어, 

![image](https://user-images.githubusercontent.com/79521972/155829619-0fcc7cb2-0f5a-4341-a3b7-851077024beb.png)

- 손안의 카드를 정렬하는 방법과 유사하다고 생각할 수 있다.
  - 새로운 카드를 기존의 정렬된 카드 더미(Deque) 속에서 올바른 자리를 찾아 삽입하고 새로 삽입될 카드의 수만큼 계속 반복해 가면서 전체 카드가 정렬되는 것이다.

그렇다면 이런 의문을 가질 수 있다.

> 정렬되지 않는 리스트를 정렬하고 싶은 것인데 이미 정렬된 서브 리스트는 어디서부터 나오는 것인가?

만약 사이즈가 1인 배열이 있다고 생각해 보자. 그렇다면 그 배열은 어떤 값이 들어있더라고 정렬된 상태라고 할 수 있을 것이다. 아래와 같이 말이다.

각각 크기가 1인 배열 3개가 있는 모습이다.

![image](https://user-images.githubusercontent.com/79521972/153825575-8091f7cd-0caf-4206-9d3c-92241929eb45.png)

<br>

삽입 정렬은 바로 이 아이디어에서 출발하는 것이다.

<br>

### 삽입 정렬 과정

가장 맨 앞의 데이터를 정렬된 서브 리스트로 보고 시작한다. 

![image](https://user-images.githubusercontent.com/79521972/153825928-353f05ad-6a9f-4d57-be3d-5a567e3294cb.png)

<br>

그렇기 때문에 사실 실질적으로는 두 번째 값 index인 1부터 정렬을 본격적으로 시작하는 것이다.

위의 예에서는 '4'라는 값을 가진 데이터는 **숫자 '5' 하나만 가진 서브 리스트**에 순서에 맞게 삽입해야 한다. 그렇다면 4는 5보다 작기 때문에 서브 리스트의 0번 index에 4를 추가한다.

![image](https://user-images.githubusercontent.com/79521972/153826222-323ead0f-c09a-4de0-a2e7-7071c6b0acb3.png)

<br>

그렇게 사이즈가 2인 서브 리스트가 생성이 된다.

이를 반복하면 서브 리스트의 크기는 점점 정렬된 상태로 커질 것이고 마지막에는 original 배열의 크기가 되어 원래의 배열을 정렬된 상태가 되는 것이다.

<br>

그 이후에는 뻔하다.  그 다음 값인 1을 가지고 와서 4와 5가 존재하는 서브 리스트의 어느 부분에 삽입할 지를 결정한다. 마찬가지로 0번 index이므로 다음 그림과 같이 된다.

![image](https://user-images.githubusercontent.com/79521972/153832179-1e16fa2f-71d9-4de0-a467-c7e88b29f562.png)

<br>

위 그림과 같이 또 정렬된 서브 리스트가 만들어지게 된다.

<br>

위의 과정을 반복하여 서브 리스트에 점점 값이 정렬되어 들어오게 되면서 완전히 오름차순으로 정렬된 리스트가 될 것이다. 아래에 그 과정을 그림으로 나타냈다.

![image](https://user-images.githubusercontent.com/79521972/153827043-5e9d12e9-eb70-48e6-a405-ba93c9492f80.png)

<br>

### 삽입 정렬 (Insertion Sorting) 구현

삽입 정렬을 코드로 구현해 보자.

```java
package sort;

public class InsertionSort implements ISort {
    @Override
    public void sort(int[] arr) {
        for (int i = 1; i < arr.length ;i++) {
            int key = arr[i]; // 삽입 위치를 찾아줄 데이터
            int j = i - 1; // 0 - j 정렬된 서브 리스트
            while(j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            arr[j + 1] = key;
        }
    }
}
```

위에서 설명했듯이 가장 맨 앞의 데이터를 서브 리스트로 생각하고 그 다음 데이터 부터 정렬을 진행하기 때문에 **for문의 초기 시작은 1부터 시작**한다. 

**for 반복문**

- key라는 변수에 삽입을 할 데이터를 받는다.
  - 삽입 할 데이터는 서브 리스트의 바로 다음 값이며 오른쪽으로 하나씩 이동하며 지정된다.
- j라는 변수는 **서브 리스트의 크기를 의미하는 인덱스**로 서브 리스트는 0부터 존재하고 순회를 할 때는 서브 리스트의 끝부터 시작하기 때문에 j는 서브 리스트의 크기를 의미하는 것이다.
- **while 반복문**
  - 이곳에서 서브 리스트의 정렬이 이루어진다.
  - 삽입 정렬이 된 서브 리스트의 마지막 인덱스(j)에는 가장 큰 값이 들어가 있다.
  - 그렇기 때문에 arr[j] (서브 리스트 마지막 값)가 key(삽입 데이터)값보다 작으면 key값은 서브 리스트의 가장 마지막에 들어가야 한다.
    - 그렇기 때문에 while문 종료후 (j + 1) index에 key 삽입

  - 반복 구간: j가 음수가 아니면서 서브 리스트의 마지막 값이 key 값보다 크면정렬 진행
    - 서브 리스트의 마지막 값부터 처음 값까지 key값과 비교하면서 진행 하기 때문에 j는 1만큼 씩 줄어든다. 
  - 만약 key값이 서브 리스트 마지막 인덱스의 값보다 작으면 서브 리스트는 key가 들어갈 공간을 하나 씩 뒤로 밀어가며 비교하여 알맞은 j값을 찾는다.

- j 값이 서브 리스트 순회를 마쳐 0이 되면 반복문 종료 및 정렬 종료.

<br>

### 삽입 정렬의 특징

**[장점]**

- 안정 정렬이다.
- 데이터 수가 적을 수록 알고리즘이 간단해 지므로 다른 복잡한 정렬 알고리즘에 비해 조금 더 좋을 경우가 있다.
- 대부분의 데이터들이 이미 정렬이 되어 있는 경우에 사용할 시 다른 불필요한 과정을 거치지 않기 때문에 효율적일 수 있다.

<br>

**[단점]**

- 다른 정렬 알고리즘에 비해서 데이터의 이동이 많은 편이다.
  - 리스트 내의 데이터가 어느 정도 정렬이 되었다면 데이터의 이동이 적어진다.
- 데이터 수가 많은 경우 사용하지 않기를 권장한다.

<br>

### 삽입 정렬의 시간 복잡도

**최선의 경우**

-  비교 횟수 : 이동없이 1번의 비교만 이루어진다.
  - 루프 횟수 : (n - 1) 번
- O(n)

**최악의 경우**

- 비교 횟수 : 외부 루프 안의 각 반복마다 i번의 비교 수행
  - 루프 횟수 : (n - 1) + (n - 2) + (n - 3) + ... + 2 + 1 = n(n - 1) / 2
  - O(n<sup>2</sup>)
- 교환 횟수
  - 외부 루프의 단계에서 (i + 2)번의 이동 발생
  - O(n<sup>2</sup>)
- O(n<sup>2</sup>)

<br>

### 정렬 간 시간복잡도 비교

| 정렬 방식                   | Average              | Worst                | Memory   | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| --------------------------- | -------------------- | -------------------- | -------- | ----------- | ------------- | --------------------------------- |
| Bubble 정렬                 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)     | O           | O             | 7.438                             |
| Selection 정렬              | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)     | X           | O             | 10.842                            |
| <mark>Insertion 정렬</mark> | **O(n<sup>2</sup>)** | **O(n<sup>2</sup>)** | **O(1)** | **O**       | **O**         | **22.894**                        |
| Shell 정렬                  | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)     | X           | O             | 0.056                             |
| Merge 정렬                  | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(n)     | O           | X             | 0.014                             |
| Quick 정렬                  | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)     | X           | O             | 0.034                             |
| Heap 정렬                   | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(1)     | X           | O             | 0.026                             |

<br>



**정렬(Sorting)**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Sorting(%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Bubble 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Bubble-Sort(%EB%B2%84%EB%B8%94-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Quick 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Quick-Sort(%ED%80%B5-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Counting 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Radix 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Bucket 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>



<br>

## 참고 자료

[삽입 정렬 애니메이션](https://www.quora.com/What-is-the-best-way-to-sort-an-unordered-list)















