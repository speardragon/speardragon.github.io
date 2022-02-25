---
layout: single
title: "[Algorithm] Bubble Sort(버블 정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', 'Bubble Sort', '버블 정렬']
toc: true
toc_sticky: true
---

이번 시간에는 정렬의 종류 중 버블 정렬에 대해서 배워보도록 하겠습니다.







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













