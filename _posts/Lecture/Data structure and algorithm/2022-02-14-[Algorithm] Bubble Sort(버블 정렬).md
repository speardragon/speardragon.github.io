---
layout: single
title: "[Algorithm] Bubble Sort(버블 정렬)"
categories: ['Lecture', 'Data structures and algorithms with Java', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', 'Bubble Sort', '버블 정렬']
toc: true
toc_sticky: true
---

이번 시간에는 정렬의 종류 중 버블 정렬에 대해서 배워보도록 하겠습니다.



## Bubble Sort란?

![img](https://user-images.githubusercontent.com/79521972/155823069-59a51ce6-b784-4b62-bf65-1b43ab2a4be7.gif)

(bubble sort animation)

그렇다면 첫 번째 정렬 방식인 Bubble Sort(버블 정렬)에 대해서 배워보도록 하겠다.

버블 정렬을 간략히 말하자면 다음과 같다.

> 서로 인접한 두 원소를 순회하며 검사하여 정렬하는 알고리즘

다음과 같은 예시를 통해 이해를 해 보도록 하자.

<br>

아래에 정렬되지 않은 리스트가 있다.

![image](https://user-images.githubusercontent.com/79521972/153805072-7bec4c4c-3038-414f-a627-52f16b7a9b1b.png)

![image](https://user-images.githubusercontent.com/79521972/153805175-5a4780b2-85d1-441b-b66b-be51dc40310f.png)

우선 가장 앞에 위치한 두 개의 값을 비교하게 된다. 그래서 만약 정렬되어 있지 않다면, 즉 다시 말해서 왼쪽의 값이 오른쪽의 값보다 크다면 두 숫자를 바꿔주게 된다.

<br>

위 그림에서는 첫 비교가 5와 4이기 때문에 5와 4를 비교했을 때 왼쪽 값이 더 크기 때문에 두 값의 위치를 바꾸어 준다.

![image](https://user-images.githubusercontent.com/79521972/153805330-1218f63e-0df2-4836-8ec0-996ed7790afe.png)

<br>

그 다음 5와 1을 비교 연산하여 이 역시도 위치가 바뀌게 되고 이런 방식으로 한 칸씩 계속 옮겨 가며 정렬을 진행해 준다.

![image](https://user-images.githubusercontent.com/79521972/153805388-7dcd9a4a-f85e-4b1e-8d57-e1257a9d24ca.png)

<br>

이 연산을 리스트의 가장 끝까지 진행하게 되면 다음과 같은 그림처럼 된다.

![image](https://user-images.githubusercontent.com/79521972/153805563-37d0fd38-e8c0-4525-b348-0257656b73fb.png)

한 cycle을 진행할 때는 리스트 크기보다 1 작은 index까지 진행해야 한다.

- 배열의 마지막 index는 비교할 다음 인덱스가 없기 때문

<br>

이 행위가 리스트의 가장 끝까지 진행이 되었다면 한 cycle 동안 진행 된 것으로 보고 이때 리스트의 가장 마지막 위치에 들어있는 값은 리스트의 모든 데이터 중에서 가장 큰 값이 되는 것을 볼 수 있다.
<br>

하지만 이 값을 제외한 앞의 값들은 여전히 정렬되지 않은 상태이다. 따라서 버블 정렬로cycle 정렬을 하더라도 완전히 정렬되지 않을 수 있는 것이다.
<br>

![image](https://user-images.githubusercontent.com/79521972/153808450-a72a5ff4-2181-4289-8609-1ff83ce929fc.png)

그렇다면 다시 처음부터 버블 정렬을 시작한다(2 cycle). 그러면 또 마지막을 제외한 index까지 정렬을 하면 다음과 같이 남은 데이터들 중 가장 큰 수가 마지막에서 두 번째에 위치하게 될 것이다.

![image](https://user-images.githubusercontent.com/79521972/153809503-a8a12cb1-9489-4271-bb44-882918e3945b.png)

<br>

그렇게 해서 한 cycle을 마무리 할 때마다 남은 리스트 에서 가장 큰 값이 뒤부터 하나씩 채워지게 된다. 다음 그림을 보자.

![image](https://user-images.githubusercontent.com/79521972/153809712-6c87de3a-684f-4705-b51e-ceb08c21b4be.png)

![image](https://user-images.githubusercontent.com/79521972/153809761-c9292a40-0be3-4e90-aba5-ae6a8b820b29.png)

![image](https://user-images.githubusercontent.com/79521972/153809834-cf38052b-7bdb-4ffd-9ef1-8c4a873b6b9e.png)

![image](https://user-images.githubusercontent.com/79521972/153809846-fd1d5edc-9687-4c18-a48f-2242216b71fa.png)

![image](https://user-images.githubusercontent.com/79521972/153811924-e20904be-dbd5-4655-9b6c-fd499deb644a.png)

![image](https://user-images.githubusercontent.com/79521972/153809857-84c0ad39-36b7-4753-84ea-6583cd272b0b.png)

위와 같은 과정으로 진행되어 남은 리스트의 데이터가 하나 남았을 때는 더 이상 정렬할 것이 없는 것으로 보고 모든 값들이 정렬이 된 상태가 된다.



<br>

### **버블 정렬의 진행과정**

- 인접한 두 element 값을 비교한다.
- 두 값이 정렬되어 있지 않다면(즉, 왼쪽 값 > 오른쪽 값이면) 두 element의 위치를 교환한다.
- 정렬이 완료된 elements를 제외하고 위의 과정을 계속 반복한다.

  - <span style="color:red">만약 처음에 배열의 크기가 3 이라면 2번 비교를 하게 되고 4 이면 3번 비교를 하기 때문에 n개의 데이터가 있다면 n-1 번의 비교를 하게 된다. </span>
  그 후로 데이터가 하나씩 줄어들기 때문에 n-2, n-3, ... , 2, 1 번의 순서대로 비교를 하게 되는 것이다. 이는 다음과 같은 식으로 표현 된다.
  - (n - 1) + (n - 2 ) + (n - 3) + ... + 2 + 1 = n*(n - 1) / 2
- 위 식을 통해 리스트의 맨 마지막 자리에 가장 큰 수가 들어가게 된다.



<br>

버블 정렬 애니메이션을 보면서 





![Bubble_sort_animation](https://user-images.githubusercontent.com/79521972/153813457-c7a2f99c-9798-48df-85a6-8d4e14d7ceba.gif)



[출처 : 위키 백과https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC)

위와 같이 정렬되는 모습이 마치 거품과 같다 하여 버블 정렬로 불리게 된 것이다.

또한 시간 복잡도는 O(N<sup>2</sup>) 형태이지만 직관적이고 단순한 알고리즘이기에 꽤 많이 쓰이는 정렬 중에 하나이다.

<br>

그렇다면 버블 정렬을 자바로 구현해 보도록 하자.

<br>

## 버블 정렬 구현

아래 코드는 ISort interface를 선언하여 sort 메소드의 프로토타입을 선언한 모습이다.

sort메소드의 리턴타입은 void인데 이는 정렬을 In-place 방식으로 정렬할 것임을 의미한다.

```java
//ISort.java

package sort;

public interface ISort {
    void sort(int[] arr);
}
```

또한 안정 정렬로 정렬을 진행할 것이다.

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

- 이중 for문에서 바깥 for문은 전체 리스트에 대해서 반복하는 것이고 안의 for문은 정렬된 리스트를 제외하고 반복을 진행하는 것이다. 
- 그렇기 때문에 0부터 arr.length - 1 - i 만큼 반복을 해야지 i값이 늘어남에 따라 고정된 데이터가 늘어나는 것이기 때문에 반대로 순회해야 하는 리스트의 크기는 그만큼 줄어드는 것이다.

그 후 안쪽 for문에서 if문으로 만약 왼쪽 값이 오른쪽 값보다 크게 되면 정렬을 진행해 준다.

사실 이 두 개의 위치를 바꾸는 것이기 때문에 정렬이라기 보다 swap 한다고 생각하면 된다.

<br>



### 버블 정렬의 특징

**[장점]**

- 구현이 정렬 중에서 가장 단순하다.

<br>

**[단점]**

- 순서에 맞지 않는 element를 인접한 element와 교환한다.
- 하나의 element가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환 되어야 한다.
- 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.

<br>
일반적으로 자료의 교환(swap) 작업이 자료의 이동 작업(move)보다 더 복잡하기 때문에 버블정렬은 단순함에도 불구하고 잘 쓰이지는 않는다.



<br>

### 시간 복잡도

- 비교 횟수

  - 최상, 평균, 최악 모두 일정
  - n - 1, n -2, ... , 2, 1 번 = n(n - 1)/2

- 교환(swap) 횟수

  - 만약 최악의 경우, 입력 자료가 역순으로 정렬 되어 있다면 한 번 교환하기 위해서 3번의  교환이 필요하기 때문에 (비교 횟수 * 3)번 이다.
  - 3n(n - 1)/2

  - 최고의 경우, 입력 자료가 이미 전부 정렬 된 상태라면 어떠한 자료의 이동도 발생하지 않는다.

- 시간 복잡도(T(n)) = O(n<sup>2</sup>)



<br>



### 정렬 간 시간복잡도 비교

| 정렬 방식                | Average              | Worst                | Memory   | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| ------------------------ | -------------------- | -------------------- | -------- | ----------- | ------------- | --------------------------------- |
| <mark>Bubble 정렬</mark> | **O(n<sup>2</sup>)** | **O(n<sup>2</sup>)** | **O(1)** | **O**       | **O**         | **7.438**                         |
| Selection 정렬           | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)     | X           | O             | 10.842                            |
| Insertion 정렬           | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)     | O           | O             | 22.894                            |
| Shell 정렬               | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)     | X           | O             | 0.056                             |
| Merge 정렬               | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(n)     | O           | X             | 0.014                             |
| Quick 정렬               | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)     | X           | O             | 0.034                             |
| Heap 정렬                | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(1)     | X           | O             | 0.026                             |

<br>



**정렬(Sorting)**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Sorting(%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Insertion 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Quick 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Quick-Sort(%ED%80%B5-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>























