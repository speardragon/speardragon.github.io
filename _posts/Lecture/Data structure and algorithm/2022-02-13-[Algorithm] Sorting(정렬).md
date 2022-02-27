---
layout: single
title: "[Algorithm] Sorting(정렬)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'Sorting']
tag: ['Data structure', 'Algorithm', 'Sorting', '정렬']
toc: true
toc_sticky: true
---

그 동안 배운 자료구조를 통해 데이터를 다루는 방법에 대해 알아 보도록 하겠다.

이번 시간에는 정렬에 대해서 먼저 설명하겠다.

<br>

## 정렬 알고리즘이란?

가장 기본적으로 자료 구조에 정의된 데이터들을 어떻게 정렬할 수 있는지에 대해서 알아보자. 정렬 알고리즘은 다양하게 존재하는데 우선 기본적인 종류는 다음과 같다.

<br>

**1\) 간단하지만(simple) 느린(slow) 방식**

- Bubble Sort
- Selection Sort
- Insertion Sort

<br>

**2\) 약간 복잡하지만 빠른 방식**

- Shell Sort
- Merge Sort
- Quick Sort
- Heap Sort

<br>

**3\) 아주 빠르지만 제한적으로만 사용 가능한 방식** 

- Counting Sort
- Radix Sort
- Bucket Sort

<br>

**4\) 기타 필요에 따라 사용되는 방식**

- Topological Sort

<br>

정렬 알고리즘은 특정 방식에 따라 여러 종류로 나뉠 수 있다.

<br>

**실행 방법에 따라**

- 비교식(Comparative) 정렬 
  - 비교식 정렬은 정렬을 수행하면서 두 개의 데이터를 서로 비교하여 필요하면 교환하는 방식으로 실행한다.
- 분산식(Distribute) 정렬
  - 정렬할 데이터를 부분적으로 나누어 분해한 뒤 다시 그 집합을 정렬하고 합치면서 전체를 정렬하는 방식이다.

<br>

**정렬 장소에 따라**

- 내부(Internal) 정렬
  - 내부 정렬은 정렬 자료를 메인 메모리에 저장하며 정렬하여 속도가 빠르지만 용량에 따라 수행가능한 수준이 제한적이다.
- 외부(External) 정렬
  - 외부 정렬은 정렬 자료를 보조 기억장치에서 정렬하며 대용량이므로 큰 데이터를 정렬할 수 있지만 속도는 제한적이다.

<br>

**저장되는 방식에 따라**

- 안정(Stable) 정렬
  - 안정 정렬은 정렬이 진행됨에 따라 같은 수준의 순서를 갖는 객체를 정렬할 때는 원래의 순서를 바꾸지 않는 정렬 방식이다.
    예를 들어, 객체 A와 B는 순서상으로 완전히 동일하다면 정렬이 끝난 뒤와 정렬 전의 A, B의 순서는 유지되는 경우를 의미한다. 
- 불안정(Unstable) 정렬
  - 불안정 정렬은 해당 내용이 반드시 유지되지 않을 수 있는 모든 정렬을 의미한다.

<br>

**메모리 사용 방식에 따라**

- In-Place 정렬
  - In-place 정렬은 데이터를 정렬 시 입력된 n 사이즈의 크기 배열 공간이 추가로 필요하지 않은 경우이다.
- Out-of-place 정렬
  - Not-in-place라고도 하며, 정렬 시에 기존 데이터가 저장된 배열 외에 추가적인 공간을 더 필요로 하는 경우를 의미한다.

<br>



안정 정렬 / 불안정 정렬, 그리고 In-place 정렬/ Out-of-place 정렬에 대해서 더 자세히 알아보자.

### 안정(stable) 정렬 vs. 불안정(unstable) 정렬

앞서 말했듯 저장되는 방식에 따라  `안정 정렬`과 `불안정 정렬`로 구분되어 있다.

아래와 같은 리스트를 생각해 보자.

![image](https://user-images.githubusercontent.com/79521972/153802812-b8ab7009-9867-4189-ab4f-4adac17c12da.png)

이 리스트에는 1이라는 중복된 데이터가 들어있어 a와 b로 표시하여 구분하였다. 이 리스트를 정렬해 보도록 하겠다.

<br>

![image](https://user-images.githubusercontent.com/79521972/153803153-54e2db15-3807-4cf1-b33c-d400c1b4390c.png)

첫 번째 리스트는 <span style="color:red">중복된 숫자의 순서까지도 보장이 되어 정렬</span>된 모습이고 두 번째 리스트는 그렇지 못한 모습입니다. 이 때, 첫 번째 리스트를 <mark>안정 정렬</mark> 되었다 하고 두 번째 리스트를 <mark>불안정 정렬</mark> 되었다 합니다.

이번 예시는 중복된 숫자로 예시를 들어 크게 와닿지 않을 수 있다.(~~이게 왜 필요한 거지?라는 의문~~) 그래서 더 이해하기 쉬운 예시를 들어보겠다.

<br>

아래와 같이 **이름 순**으로 정렬 된 파일이 있다고 가정하자.

| 이름 순 정렬  |            |
| ------------- | ---------- |
| 국어과제1.pdf | 2021-06-01 |
| 국어과제2.pdf | 2021-06-01 |
| 영어과제1.pdf | 2021-04-01 |
| 영어과제2.pdf | 2021-04-01 |
| 한문과제1.pdf | 2021-07-01 |
| 한문과제2.pdf | 2021-07-01 |

이 파일들을 날짜 순으로 재 정렬 해 보자.

<br>

중복된 날짜가 있을 때 **안정 정렬**을 하게 되면 아래와 같이 **이름 순으로 정렬 된 파일을 보장** 받는다. 

| 날짜 순 정렬(안정) |            |
| ------------------ | ---------- |
| 영어과제1.pdf      | 2021-04-01 |
| 영어과제2.pdf      | 2021-04-01 |
| 국어과제1.pdf      | 2021-06-01 |
| 국어과제2.pdf      | 2021-06-01 |
| 한문과제1.pdf      | 2021-07-01 |
| 한문과제2.pdf      | 2021-07-01 |

<br>

반면 **불안정 정렬**로 하게 되면 아래와 같이 날짜 순으로는 정렬이 되지만 **파일의 이름 순으로 정렬되어 있는 것은 보장 받지 못한다.**

| 날짜 순 정렬(안정) |            |
| ------------------ | ---------- |
| 영어과제2.pdf      | 2021-04-01 |
| 영어과제1.pdf      | 2021-04-01 |
| 국어과제2.pdf      | 2021-06-01 |
| 국어과제1.pdf      | 2021-06-01 |
| 한문과제2.pdf      | 2021-07-01 |
| 한문과제1.pdf      | 2021-07-01 |

<br>

### In-place 정렬 vs. Out-of-place 정렬

`구현 방식`에 따라 In-place 정렬과 Out-of-place 정렬로 나뉘게 된다.

**In-place 정렬**은 <span style="color:red">원본 데이터 내에서 정렬이 이루어 지게 되는 정렬</span>을 말하는 것이고 <span style="color:red">원본 데이터가 아닌 새로운 배열로 정리된 output 결과를 만드는 것</span>을 **Out-of-place 정렬**이라고 한다.

<br>

따라서 정렬을 할 때에는 

- 시간 복잡도
- 안정/ 불안정 정렬
- 구현 방식
- 저장되는 방식

위와 같은 것들을 생각하여 정렬이 진행된다.

<br>

### 정렬 간 시간복잡도 비교

| 정렬 방식      | Average              | Worst                | Memory | Stable 여부 | In-Place 여부 | Run-time(정수 60,000개) 단위: sec |
| -------------- | -------------------- | -------------------- | ------ | ----------- | ------------- | --------------------------------- |
| Bubble 정렬    | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)   | O           | O             | 7.438                             |
| Selection 정렬 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)   | X           | O             | 10.842                            |
| Insertion 정렬 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)   | O           | O             | 22.894                            |
| Shell 정렬     | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)   | X           | O             | 0.056                             |
| Merge 정렬     | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(n)   | O           | X             | 0.014                             |
| Quick 정렬     | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(1)   | X           | O             | 0.034                             |
| Heap 정렬      | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(1)   | X           | O             | 0.026                             |

<br>



**Bubble 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Bubble-Sort(%EB%B2%84%EB%B8%94-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Insertion 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Insert-Sort(%EC%82%BD%EC%9E%85-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Shell 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Merge 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Merge-Sort(%ED%95%A9%EB%B3%91-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Quick 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/sorting/Algorithm-Quick-Sort(%ED%80%B5-%EC%A0%95%EB%A0%AC)/)을 참조<br>

**Heap 정렬**은 우선순위 큐에서 사용하는 정렬이므로 해당 포스팅 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/heap/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Heap-&-Priority-Queue(%ED%9E%99%EA%B3%BC-%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90)/)을 참조<br>

**Counting 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Radix 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Bucket 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>]()을 참조<br>

**Topological 정렬**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-Topological-Sorting(%EC%9C%84%EC%83%81-%EC%A0%95%EB%A0%AC)/)을 참조<br>

















