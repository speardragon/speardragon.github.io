---
layout: single
title: "[Algorithm] CH6-3. Bubble Sort(버블 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', 'Bubble Sort', '버블 정렬']
toc: true
toc_sticky: true
---

<br>

# 버블 정렬

이번 시간에는 정렬의 종류 중 버블 정렬에 대해서 배워보도록 하겠습니다.





## 정렬

버블 정렬에 대해 설명 드리기 전에 정렬의 특징에 대해 먼저 설명해 드리겠습니다.

<br>

### 안정(stable) 정렬 vs. 불안정(unstable) 정렬

먼저 정렬은 `안정 정렬`과 `불안정 정렬`로 구분되어 있습니다.

아래와 같은 리스트를 생각해 봅시다.

![image](https://user-images.githubusercontent.com/79521972/153802812-b8ab7009-9867-4189-ab4f-4adac17c12da.png)

이 리스트에는 1이라는 중복된 데이터가 들어있어 a와 b로 표시하여 구분하였습니다. 이 리스트를 정렬해 보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79521972/153803153-54e2db15-3807-4cf1-b33c-d400c1b4390c.png)

첫 번째 리스트는 중복된 숫자의 순서까지도 보장이 되어 정렬된 모습이고 두 번째 리스트는 그렇지 못한 모습입니다. 이 때, 첫 번째 리스트를 안정 정렬 되었다 하고 두 번째 리스트를 불안정 정렬 되었다 합니다.

이번 예는 중복된 숫자로 예시를 들어 크게 와닿지 않으실 수 도 있습니다. 그래서 더 이해하기 쉬운 예시를 들어보겠습니다.

<br>

아래와 같이 이름 순으로 정렬 된 파일이 있다고 가정합시다.



| 이름 순 정렬  |            |
| ------------- | ---------- |
| 국어과제1.pdf | 2021-06-01 |
| 국어과제2.pdf | 2021-06-01 |
| 영어과제1.pdf | 2021-04-01 |
| 영어과제2.pdf | 2021-04-01 |
| 한문과제1.pdf | 2021-07-01 |
| 한문과제2.pdf | 2021-07-01 |

이 파일들을 날짜 순으로 재 정렬 해 보겠습니다.

<br>

중복된 날짜가 있을 때 안정 정렬을 하게 되면 아래와 같이 이름 순으로 정렬 된 파일을 보장 받습니다. 

| 날짜 순 정렬(안정) |            |
| ------------------ | ---------- |
| 영어과제1.pdf      | 2021-04-01 |
| 영어과제2.pdf      | 2021-04-01 |
| 국어과제1.pdf      | 2021-06-01 |
| 국어과제2.pdf      | 2021-06-01 |
| 한문과제1.pdf      | 2021-07-01 |
| 한문과제2.pdf      | 2021-07-01 |

<br>

반면 불안정 정렬로 하게 되면 아래와 같이 날짜 순으로는 정렬이 되지만 파일의 이름 순으로 정렬되어 있는 것은 보장 받지 못합니다.

| 날짜 순 정렬(안정) |            |
| ------------------ | ---------- |
| 영어과제2.pdf      | 2021-04-01 |
| 영어과제1.pdf      | 2021-04-01 |
| 국어과제2.pdf      | 2021-06-01 |
| 국어과제1.pdf      | 2021-06-01 |
| 한문과제2.pdf      | 2021-07-01 |
| 한문과제1.pdf      | 2021-07-01 |



### In-place 정렬 vs. Out-of-place 정렬

두 번째는 `구현 방식`에 따라 in-place sort와 out-of-place sort로 나뉘게 됩니다.

In-place 정렬은 원본 데이터 내에서 정렬이 이루어 지게 되는 정렬을 말하는 것이고 원본 데이터가 아닌 새로운 배열로 정리된 output 결과를 만드는 것을 Out-of-place 정렬이라고 합니다.

따라서 정렬을 할 때에는 

- 시간 복잡도
- 안정/ 불안정 정렬
- 구현 방식

와 같은 것들을 생각하여 정렬이 진행됩니다.



### 정렬 간 시간복잡도 비교

![image](https://user-images.githubusercontent.com/79521972/153818083-078c6760-47d3-42cc-aaac-03c07f6b67f2.png)

- 단순하지만 비효율적인 방법에는
  - 삽입 정렬, 선택 정렬, 버블 정렬
- 복잡하지만 효율적인 방법에는
  - 퀵 정렬, 힙 정렬 ,합병 정렬, 기수 정렬

이 있습니다.

<br>



## Bubble Sort

그렇다면 첫 번째 정렬 방식인 Bubble Sort(버블 정렬)에 대해서 배워보도록 하겠습니다.

버블 정렬의 간략히 말하자면 다음과 같습니다.

> 서로 인접한 두 원소를 순회하며 검사하여 정렬하는 알고리즘

다음과 같은 예시를 통해 이해를 해 보도록 하겠습니다.

아래에 정렬되지 않은 리스트가 있습니다.

![image](https://user-images.githubusercontent.com/79521972/153805072-7bec4c4c-3038-414f-a627-52f16b7a9b1b.png)

우선 가장 앞에 위치한 두 개의 값을 비교하게 됩니다. 그래서 만약 정렬되어 있지 않다면, 즉 다시말해서 왼쪽의 값이 오른쪽의 값보다 크다면 두 숫자를 바꿔주게 됩니다.

![image](https://user-images.githubusercontent.com/79521972/153805175-5a4780b2-85d1-441b-b66b-be51dc40310f.png)

위 그림에서는 5와 4를 비교하여 왼쪽 값이 더 크기 때문에 두 값의 위치를 바꾸어 줍니다.

![image](https://user-images.githubusercontent.com/79521972/153805330-1218f63e-0df2-4836-8ec0-996ed7790afe.png)

<br>

그 다음 5와 1을 비교 연산하여 이 역시도 위치가 바뀌게 되고 이런 방식으로 한 칸씩 계속 옮겨 가며 정렬을 진행해 줍니다.

![image](https://user-images.githubusercontent.com/79521972/153805388-7dcd9a4a-f85e-4b1e-8d57-e1257a9d24ca.png)

<br>

이 연산을 리스트의 가장 끝까지 진행하게 되면 다음과 같은 그림처럼 됩니다.

![image](https://user-images.githubusercontent.com/79521972/153805563-37d0fd38-e8c0-4525-b348-0257656b73fb.png)

이 행위가 리스트의 가장 끝까지 진행이 되었다면 리스트의 가장 마지막 위치에 들어있는 값은 리스트의 모든 데이터 중에서 가장 큰 값이 될 것입니다.

하지만 이 값을 제외한 앞의 값들은 정렬되지 않은 상태입니다. 따라서 버블 정렬로 한 번 정렬을 하더라도 완전히 정렬되지 않을 수 있습니다.
<br>

![image](https://user-images.githubusercontent.com/79521972/153808450-a72a5ff4-2181-4289-8609-1ff83ce929fc.png)

그렇다면 다시 처음부터 버블 정렬을 시작합니다. 그렇게 또 마지막을 제외한 index까지 정렬을 하면 다음과 같이 남은 데이터들 중 가장 큰 수가 마지막에서 두 번째에 위치하게 될 것입니다.

![image](https://user-images.githubusercontent.com/79521972/153809503-a8a12cb1-9489-4271-bb44-882918e3945b.png)

그렇게 해서 한 cycle을 마무리 할 때마다 남은 리스트 에서 가장 큰 값이 뒤부터 하나씩 채워지게 됩니다.

![image](https://user-images.githubusercontent.com/79521972/153809712-6c87de3a-684f-4705-b51e-ceb08c21b4be.png)

![image](https://user-images.githubusercontent.com/79521972/153809761-c9292a40-0be3-4e90-aba5-ae6a8b820b29.png)

![image](https://user-images.githubusercontent.com/79521972/153809834-cf38052b-7bdb-4ffd-9ef1-8c4a873b6b9e.png)

![image](https://user-images.githubusercontent.com/79521972/153809846-fd1d5edc-9687-4c18-a48f-2242216b71fa.png)

![image](https://user-images.githubusercontent.com/79521972/153811924-e20904be-dbd5-4655-9b6c-fd499deb644a.png)

![image](https://user-images.githubusercontent.com/79521972/153809857-84c0ad39-36b7-4753-84ea-6583cd272b0b.png)

위와 같은 과정으로 진행되어 남은 리스트의 데이터가 하나 남았을 때는 모든 값들이 정렬이 된 상태입니다.



<br>

**버블 정렬의 진행과정**

- 인접한 두 element 값을 비교합니다.

- 두 값이 정렬되어 있지 않다면(왼쪽 값 > 오른쪽 값) 위치를 교환합니다.

- 정렬이 완료된 elements를 제외하고 위의 과정을 반복합니다.

  - 만약 처음에 데이터가 3개이면 2번 비교를 4번이면 3번 비교를 하기 때문에 n개의 데이터가 있다면 n-1 번의 비교를 하게 됩니다. 그 후로 데이터가 하나씩 줄어들기 때문에 n-2, n-3, ... ,2,1 번 비교를 하게 되는 것입니다. 다음과 같은 식으로 표현 됩니다.

  - (n - 1) + (n - 2 ) + (n - 3) + ... + 2 + 1 = n(n - 1) / 2

  - 위 식을 통해 리스트의 맨 마지막 자리에 가장 큰 수가 들어가게 됩니다.
    <br>

    

    

    ![Bubble_sort_animation](https://user-images.githubusercontent.com/79521972/153813457-c7a2f99c-9798-48df-85a6-8d4e14d7ceba.gif)



[출처 : 위키 백과https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

위와 같이 정렬되는 모습이 마치 거품과 같다 하여 버블 정렬로 불리게 된 것입니다.

또한 시간 복잡도는 O(N^2) 형태이지만 직관적이고 단순한 알고리즘이기에 꽤 많이 쓰이는 정렬 중에 하나입니다.

<br>

## 버블 정렬 구현

아래 코드는 ISort interface에 sort 메소드의 바디를 선언한 모습입니다. sort메소드의 리턴타입은 void인데 이것의 의미는 정렬을 인플레이스로 정렬할 것이라는 의미입니다.

```java
//ISort.java

pacakge sort;

public interface ISort {
    void sort(int[] arr);
}
```

또한 안정 정렬로 정렬을 진행할 것입니다.

<br>

```java
//BubbleSort.java

package sort;

public class BubbleSort implements ISort {
    @Override
    public void sort(int[] arr) {
        
        for (int i = 0; i < arr.length - 1; i++) { //전체 리스트
            for (int j = 0; j < arr.length - 1 - i; j++) { //정렬된 리스트 제외
                if(arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
    }
}
```

이중 for문에서 바깥 for문은 전체 리스트에 대해서 반복하는 것이고 안의 for문은 정렬된 리스트를 제외하고 반복을 진행하는 것입니다. 그렇기 때문에 0 부터 arr.length - 1 - i 만큼 반복을 해야지 i값이 늘어남에 따라 리스트의 크기는 그만큼 줄어들게 되는 것입니다.

그 후 안쪽 for문에서 if문으로 만약 왼쪽 값이 오른쪽 값보다 크게 되면 정렬을 진행해 줍니다.

사실 이 두 개의 위치를 바꾸는 것이기 때문에 정렬이라기 보다 switch를 하신다고 생각하시면 됩니다. 그렇기 때문에 코드도 되게 간결하게 마무리 됩니다. 













