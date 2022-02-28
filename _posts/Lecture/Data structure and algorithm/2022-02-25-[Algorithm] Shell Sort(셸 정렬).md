---
layout: single
title: "[Algorithm] Shell Sort(셸 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬', 'Shell sort', '쉘 정렬']
toc: true
toc_sticky: true
---

 이번 시간에는 쉘 정렬(Shell Sort)에 대해서 배워볼 것이다.

<br>

## 1. 셸 정렬이란?

![vQDGWb](https://user-images.githubusercontent.com/79521972/155940530-287b1003-275b-471e-9f87-09de8766e5ed.gif)

셸 정렬은 삽입 정렬의 성질을 이용하고 이를 보완한 삽입 정렬의 일반화 버전이라고 볼 수 있다.

- **삽입 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조

<br>

삽입 정렬의 특징은 다음과 같은 것이 있었다.

1. 입력되는 초기 리스트가 `거의 정렬`되어 있을 경우에만 효율적으로 사용 가능하다.
2. 한 번에 한 요소의 위치만 결정 되기 때문에 비효율적이다.

<br>

이러한 삽입 정렬의 장점은 뽑아 먹고 단점은 보완한 것이 셸 정렬이다.

그래서 셸 정렬은 기존의 삽입 정렬을 수행하기 전에 전체 데이터를 `거의 정렬된` 상태로 만들면 기존의 삽입 정렬을 그대로 처음부터 사용하는 것보다 더 좋은 성능을 발휘할 수 있다는 점에서 착안된 것이다.

<br>

우선 셸 정렬은 

Memory 상에서 필요 시 상호 위치만 변경될 뿐 추가적인 배열은 생성이 불필요하다는 점에서 <mark>In-place 정렬</mark> 방식이고,

같은 값을 가지는 데이터의 기존 순서 유지를 보장할 수 없다는 점에서 <mark>Unstable 정렬</mark>이다.

<br>

## 2. 동작 방식

셸 정렬의 동작 방식은 삽입 정렬과 동일하다.

그러나 다른 점은 기존 **삽입 정렬**이 삽입 위치를 찾기 위해 인접한 값들끼리만 비교했다면 **셸 정렬**은 <span style="color:red">Gap</span>을 두어 인접하지 않은 값들끼리 비교하는 방식이다.

<br>

그리고 이 Gap을 줄여가야 하는데, Gap이 1이 된다면 삽입 정렬과 동일한 상태로 동작하게 된다. 그래서 Gap이 1이 되기 전까지는 전체 데이터가 `거의 정렬된` 상태를 유지하는 것이다.

<br>

셸 정렬이 Unstable한 이유는 이 Gap을 통해서 인접하지 않은 값들끼리 교환이 일어날 수 있기 때문인데 이제 어떻게 셸 정렬이 동작하는 지 알아보자.

<br>

  초기 배열의 상태는 다음과 같다.

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 20   | 35   | -15  | 7    | 76   | 1    | -3   | 8    | 0    | -50  |

<br>

정렬은 0번 부터 시작하지만 0번과 비교할 데이터는 0 + k 번이다. <span style="color:blue">여기서 k가 바로 우리가 위에서 말했던 Gap이다.</span>

이 k를 초기에 (배열의 크기 / 2)로 정하고 시작하자.

<br>

1\) k = (10/2) = 5인 경우

![image](https://user-images.githubusercontent.com/79521972/155942656-c0412389-889f-4ae3-800d-d35baa1fdf19.png)

같은 높이(행)에 있는 것들끼리 비교하여 정렬한다고 생각하면 된다.

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | -3   | -15  | 0    | -50  | 20   | 35   | 8    | 7    | 76   |

위 배열은 간격 별 정렬 후의 1차로 정렬된 상태이다.

1차적인 정렬을 마쳤기 때문에 Gap을 조금씩 줄여야 한다. 되도록 이 Gap은 홀수가 되는 것이 좋기 때문에 다음 Gap 3으로 해 보겠다.

<br>

2\) k = 3인 경우

![image](https://user-images.githubusercontent.com/79521972/155942903-8f4eeba0-a195-44f4-ab3b-ee1b2ac109ee.png)

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | -50  | -15  | 1    | -3   | 7    | 35   | 8    | 20   | 76   |

<span style="color:red">마지막으로 Gap을 1로 다시 줄여서 삽입 정렬을 진행한다. </span>

이때 Gap이 1일때 삽입 정렬을 시도하는 것은 거의 정렬된 상태에서 삽입 정렬을 시도하는 것이기 때문에 상당히 빠른 퍼포먼스로 정렬을 마무리 할 수 있는 것이다.

<br>

3\) Gap = 1인 경우, 삽입 정렬 시도

| 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| -50  | -15  | -3   | 0    | 1    | 7    | 8    | 20   | 35   | 76   |

위 배열과 같이 모두 정렬이 잘 마무리 된 것을 확인 할 수 있다.



## 3. 셸 정렬 구현

```java
package com.test;

public class ShellSort {

    public static void main(String[] args){
        int[] intArray = {20, 35, -15, 7, 55, 1, -22};

        // Gap에 따라 정렬 하기 위해 Gap을 이용한 반복문을 생성함
        for(int gap = intArray.length / 2; gap > 0; gap /= 2){

            // Gap의 크기에 맞게 최초 정렬을 시작할 기준을 지정하여 반복문 형성
            for(int i=gap; i < intArray.length; i++) {

                // i에 지정된 값에 해당하는 Value를 정렬 시를 대비해 미리 저장해 둠
                int newElement = intArray[i];

                // j를 이용해 Gap 만큼의 반복 정렬을 수행할 것이므로 따로 저장
                int j = i;

                // 해당 반복 정렬을 조건이 참인 경우 수행
                // 해당 조건은, j의 index 값이 gap보다 커야 하며, j-gap의 index에 지정 된 배열 값이 이전에 저장된 내용보다 큰 경우
                while (j >= gap & intArray[j - gap] > newElement) {

                    // 해당 값을 뒤 쪽으로 미루어 저장
                    intArray[j] = intArray[j - gap];

                    // 반복 비교를 위해 gap만큼 차감
                    j -= gap;
                }

                // 기존에 저장한 배열 값을 저장
                intArray[j] = newElement;
            }
        }

        for(int i=0; i < intArray.length; i++){
            if(i == intArray.length-1){
                System.out.print(intArray[i]);
            } else {
                System.out.print(intArray[i] + ", ");
            }
        }
    }
}
```



<br>

## 4. Gap 정하는 방법

- 각 회전마다 Gap을 절반으로 줄여나간다.
  - 즉, 각 회전이 반복될 때마다 하나의 부분 리스트의 속한 값들의 갯수는 증가한다.
- Gap은 홀수로 하는 것이좋다.
- Gap을 절반으로 줄여나갈 때 짝수가 된다면 +1 (소수인 경우 반올림) 하여 홀수로 만들어 준다.
- Gap이 1이 될때까지 반복한다.

<br>

위의 예시에서는 사실 Gap sequence를 단순하게 배열의 크기에서 2로 나누면서 진행해 나갔지만, 실제로 이에 대한 연구 `Best Gap Sequence, Knuth Sequence, Ciura Sequence`등에서 좋은 Gap을 얻는 방법은 계속해서 찾아내는 중이다.

이에 대한 내용은 구글 검색을 통해 알아보도록 하고 셸 정렬에 대한 내용은 여기서 마치겠다.

<br>

### 5. 정렬 간 시간복잡도 비교

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

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>