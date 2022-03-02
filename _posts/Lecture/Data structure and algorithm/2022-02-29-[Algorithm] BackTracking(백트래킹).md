---
layout: single
title: "[Algorithm] BackTracking(백트래킹)"
categories: ['Lecture', 'Data structure and algorithms with Java', 'Algorithm', 'BackTracking']
tag: ['Data structure', 'Algorithm', 'BackTracking', '백트래킹']
toc: false
toc_sticky: true
---

 이번 시간에는 백트래킹 알고리즘에 대해서 알아보도록 하겠다.

<br>

## 1. 백트래킹이란?

`백트래킹(BackTracking)`은 <span style="color:red">해를 찾는 도중 해가 아니라서 막히면, 뒤로 되돌아 가서 다시 해를 찾아 나가는 방법</span>이다.

또한 다음과 같이 풀어서 설명하기도 한다.

> 재귀적으로 문제를 하나씩 풀어가다가 현재 재귀를 통해 확인 중인 노드가 제한 조건에 의해 위배 되고 있지는 않은지 판단하고(이 과정이 유망한지를 판단하는 과정), 그러한 경우에 해당 노드를 제외하고 돌아가서 다음 단계로 나아가는 방식이다.

여기서 중요한 점은 특정 노드는 제외하고 진행을 이어간다는 점이다.

<br>

<span style="color:blue">부르트 포스</span>는 무작정 그냥 모든 방법에 대하여 가능한 해를 나열하는 것이라면 <span style='color:red'>백트래킹</span>은 모든 조합의 수를 살펴보되 단 조건이 만족할 때만 살펴보는 것이다.

<br> 이렇게 조건에 의해 더 이상 탐색할 필요가 없다고 판단된 노드를 제외하는 과정을 가지치기(pruning)이라고 한다.

이러한 방식을 통해 모든 경우의 수를 체크하지 않고 필요한 것만 체크해 가기 때문에 풀이 시간을 단축할 수 있는 것이다.

<br>

- **백트래킹의 유망성 판단**

특정 노드의 유망성을 판단한다는 것은 해당 노드가 해가 될 수 있는지 없는지를 판단하는 것이다. 만약 유망하지 않다고 판단되면 그 노드의 이전(부모)으로 돌아가(back) 다음 자식 노드로 간다.(tracking)

<br>

## 2. 백트래킹 사용 예시

백트래킹을 사용하는 예시 중 가장 유명한 알고리즘인 DFS가 있다.

- **DFS**의 설명은 [<u><span style="color:blue">이곳</span></u>](https://speardragon.github.io/lecture/data%20structure%20and%20algorithms/algorithm/graph/Algorithm-%EA%B9%8A%EC%9D%B4-%EC%9A%B0%EC%84%A0-%ED%83%90%EC%83%89(DFS)(%EA%B7%B8%EB%9E%98%ED%94%84-%ED%83%90%EC%83%89)/)을 참조<br>

<br>

DFS는 재귀를 통해 가능한 경로 중에서 유망한 경로만 찾아서 탐색을 진행하고 다시 돌아와 그 중에서 가장 최적의 경로를 반환하는 방식이다.

<br>

DFS 말고도 `의사 결정, 최적화 ,열거하기` 등의 문제를 해결할 수있다.

<br>

백트래킹을 사용해 푸는 유명한 문제가 있다.

![image](https://user-images.githubusercontent.com/79521972/155931190-4323c6e4-94d7-43e5-8010-b0a1a550f0a9.png)

<br>

### 2-1. N-Queen

N-Queen 문제를 통해서 백트래킹 사용 예시를 한 번 알아보자.

- [백준 9663번 N-Queen](https://www.acmicpc.net/problem/9663)

> *N \* N 체스판에 최대한 많은 Queen을 놓고 싶다. 다만, 서로를 공격하지 않게 올려놓고 싶다면 총 몇 가지의 경우의 수가 있을까?*

<br>

체스를 잘 모르는 사람을 위해 설명하자면 체스 말중에 Queen(퀸)은 상하좌우 뿐 아니라 대각선 방향으로 이동할 수 있는 말이다.

<br>

이러한 상황에서 퀸을 어떻게 배치해야 서로를 공격하지 않을까?

서로를 공격하지 않아야 하므로 k번째 column에 넣는 Queen을 두기 위해서는 해당 위치에 대하여 `왼쪽, 왼쪽위 대각선, 왼쪽 아래 대각선`에는 다른 Queen들이 존재하지 않아야 한다.

<br>

체스판의 왼쪽 상단 부터 하나씩 둘 수 있는지를 판단해서 놓는다고 하면 한 column에 하나의 Queen밖에 둘 수 없다는 것은 자명한 사실이다. 이를 계산하면 다음과 같다.

- `모든 경우의 수 = 컬럼의 수 ^ 한 컬럼에 Queen을 놓는 경우의 수 = N  ^ N`
  - Power N of N

이는 O(N^N) 시간 복잡도를 갖으며 N=10인 경우만 본다고 하더라도 대략 100억개가 넘는 숫자이다.

<br>

<mark>이제 백트래킹 기법을 사용해 보자.</mark>

<br>

1.  한 column에 Queen을 하나 두었다면 오른쪽 column에 가능한 자리에 Queen을 하나 둔다.

2. 또 다시 오른쪽 column 중에서 가능한 자리에 Queen을 하나 둔다.

3. 이 과정을 반복한다.

   - 이때 만약 N개의 Queen을 두었다면 count를 하나 증가 시킨다.

   - 성공하기 직전으로 상태를 되돌린다.(Back)

   - N번째 column 중 다른 칸을 시도한다.
     - 만약 없다면 N - 1 번째 column에 있는 Queen을 들어서 다음으로 가능한 칸에 옮긴다.
   - 다시 N 번째 column에 있는 Queen을 둘 수있는 곳을 찾는다.
   - N - 1 번째 column의 모든 경우를 시도했다면, N -2 번째로 되돌아가서 다음으로 가능한 위치로 Queen을 옮긴다.

<br>

<br>

 이를 도식화하여 다시 설명해 보겠다.

1. 가장 먼저 가장 좌측 column(열)에서 시작하여 하나씩 비교해서 해당 column의 모든 row(행)에 퀸이 이미 배치 되었는지 체크한다.

![image](https://user-images.githubusercontent.com/79521972/155932935-b4edf4a6-9b12-4778-a4ec-e713a09ca2f7.png)

- 제일 좌측 열의 모든 위치에 Queen이 있는지 확인

<br>

2. 각 행을 체크할 때는 해당하는 행에 퀸이 없다면 우선 유망한 후보로 고르고, 해당 행 전체를 조사하여 퀸이 배치 되었는지를 조사한다.

![image](https://user-images.githubusercontent.com/79521972/155933191-11248acd-7dde-44d9-91bc-37a301e6d99e.png)

- 0 by 0의 위치가 유망하다고 판단되어 위처럼 체크 되었다.

<br>

3. 행 또한 체크를 마쳤다면, 이번엔 대각선으로 체크하여 퀸이 배치된 내역이 존재하는지 확인한다.

![image](https://user-images.githubusercontent.com/79521972/155933420-dc302606-320f-49e5-9667-8be2d8b7f64e.png)

<br>

4. 위의 과정을 통해 모두 비어 있다고 판단되면 해당 위치에 퀸을 놓고, 다음 열에서도 동일하게 체크를 수행하는 과정을 이어나간다.

![image](https://user-images.githubusercontent.com/79521972/155934420-59ed9a5f-268d-4da8-b662-8d2a41325276.png)

<br>

이 과정을 계속 반복하면 다음과 같이 될 것이다.

![image](https://user-images.githubusercontent.com/79521972/155934829-b9cb5652-92dd-4149-8662-3909dc882984.png)

<br>

<mark>그러나 어떤 열에 대해 놓을 수 있는 곳을 보다가 놓지 못한 곳이 있으면 어떻게 될까?

![image](https://user-images.githubusercontent.com/79521972/155935738-142c1f35-74ac-4728-941d-f3dd9520d293.png)

이 경우가 바로 특정 열에 어떠한 곳에도 퀸을 배치할 수 없는 경우이다.

<mark>이럴때 바로 백트래킹을 통해 해결하는 것이다.</mark>

<br>

위의 그림처럼 4 번째 열에는 어떠한 곳도 퀸을 배치할 수 없기 때문에 백트래킹을 통해 바로 전인 3 번째 열로 돌아가 다시 다른 가능한 행에 배치하는 것이다. 

그러면 다음 그림과 같이 배치가 완료 될 것이다.

![image](https://user-images.githubusercontent.com/79521972/155935950-70a87cd7-fd21-47e5-bf13-eac7338096bc.png)

<br>

이러한 방식을 반복을 통해 전체 퀸을 모든 열에 놓을 수 있는 경우를 찾을 때까지 반복한다.

- 재귀 + 반복문 기반 백트래킹 

<br>

### 2-2. N-Queen 구현

```java
import java.util.Scanner;
 
public class Main {
 	
    //멤버 변수: 체스판, 크기, 결과 카운트
	public static int[] arr;
	public static int N;
	public static int count = 0;
 
	public static void main(String[] args) {
 		
        //체스판 크기 입력 및 생성
		Scanner in = new Scanner(System.in);
		N = in.nextInt();
		arr = new int[N];
 
		nQueen(0);
		System.out.println(count);
 
	}
 
	public static void nQueen(int depth) {
		// 모든 원소를 다 채운 상태면 count 증가 및 return 
		if (depth == N) {
			count++;
			return;
		}
 
		for (int i = 0; i < N; i++) {
			arr[depth] = i;
			// 놓을 수 있는 위치일 경우 재귀호출
			if (Possibility(depth)) {
				nQueen(depth + 1);
			}
		}
 
	}
 
	public static boolean Possibility(int col) {
 
		for (int i = 0; i < col; i++) {
			// 해당 열의 행과 i열의 행이 일치할경우 (같은 행에 존재할 경우) 
			if (arr[col] == arr[i]) {
				return false;
			} 
			
			/*
			 * 대각선상에 놓여있는 경우
			 * (열의 차와 행의 차가 같을 경우가 대각선에 놓여있는 경우로 판정)
			 */
			else if (Math.abs(col - i) == Math.abs(arr[col] - arr[i])) {
				return false;
			}
		}
		
		return true;
	}
}
```

<br>



## 2-3. 비선형 구조화(DFS)

비선형으로 구성된 자료 구조를 깊이 우선 탐색을 진행할 때 더 이상 나아갈 수 없는 상황에서 그 이전 단계로 되돌아가(back) 다시 탐색을 진행하는 과정을 말한다.

![image](https://user-images.githubusercontent.com/79521972/155937222-d82b7fb2-2798-4c2a-a44e-4baa2b8a083a.png)

- 일반적으로 최적화 문제를 해결할 때 사용하는 방법.
- 문제의 구조를 비선형을 구조화하여 가능한 모든 상태(노드)를 깊이 우선 탐색 방식으로 해를 구성해 나간다.
- 이 과정에서 더 이상 탐색을 진행할 수 없다고 판단 되면(조건 불만족) 백트래킹 하여 다시 다른 상태(노드)부터 탐색을 진행한다.

<br>

## 2-3-1. 예제 - 행렬

> 다음과 같은 행렬에서 3개 원소를 서로 행과 열이 중복 되지 않도록 선택하는 게임이 있다 여기서 얻을 수 있는 최소 점수는 얼마인가?

```
{{1, 5, 3},
 {2, 4, 7},
 {5, 3, 5}}
```

행과 열이 중복되지 않아야 한다는 말은 각 행에서 1개 이상의 값은 얻을 수 없다는 말과 같다. 

그리고 서로 열끼리도 중복하지 않아야 하기 때문에 다음 문제를 다음 그림과 같이 구조화 하겠다.

![image](https://user-images.githubusercontent.com/79521972/155937665-c0177b7a-1f5b-4e6b-80fb-07c6d5d478ee.png)

<br>

위 그림은 1행 1열의 값 1을 선택했을 때의 탐색 구조의 일부를 나타낸 것이다. 

행과 열이 중복되지 않아야 하기 때문에 {2}는 선택할 수 없다.

<br>

따라서 깊이 우선 탐색으로 이를 진행하면 첫 번째 해는 다음과 같이 정해진다.

![image](https://user-images.githubusercontent.com/79521972/155938502-095f0644-4090-4d40-9e4c-99b9888b5a02.png)



처음으로 구한 해 1-4-5를 탐색하여 구한 수는 10이다.

문제 조건에 따라서 `1행 1열, 2행 2열`을 선택했기 때문에 3행에서 1열과 2열은 선택할 수 없다.

<br>

여기서 10이 최종적인 해인지 아닌지 현재 상태로는 확정할 수 없다.

- 가능한 경우가 아직 남아있기 때문

<br>
따라서 백트래킹으로 가능한 모든 해를 구해야만 최종적인 해를 구할 수 있다.

![image](https://user-images.githubusercontent.com/79521972/155938064-c4c109d5-5de1-453f-a5f1-1e7a67d541b9.png)

1로 백트래킹을 사용하여 7로 진행할 수 있는 해를 구할 수 있다.

이를 반복하여 얻을 수 있는 경우의 수는 다음과 같다.

- {1, 7, 3} = 11
- {5, 2, 5} = 12
- {5, 4 , 5} = 14
- {3, 2, 3} = 8
- {3, 4, 5} =12

따라서 답은 최소값인 8이 된다.



<br>

## 3. DFS vs. 백트래킹(back tracking)

DFS는 그래프 탐색 방법 중 하나로 그래프 구조에서 모든 정점을 탐색할 수 있는 알고리즘 중에 하나이다. 

깊이를 우선순위로 하여 탐색을 진행하기 때문에 재귀 혹은 스택 구조를 활용한다.

<br>
재귀를 이용하여 탐색을 진행한다는 점에서 완전탐색 알고리즘의 재귀 / 백트래킹과 유사하다고 느낄 수 있다.

<br>

하지만, 재귀라는 것은 자기 자신의 함수(메소드)를 호출하는 방식이기 때문에 DFS는 재귀 방식을 이용해 수행하는 많은 알고리즘 중에 하나인 것이다.

또한 백트래킹은 재귀를 통해 알고리즘을 풀어 가는 기법으로 가지지기를 통해서 탐색을 하다가 유망하지 않다고 생각이 들면 추가 탐색을 진행하지 않고 뒤로 돌아(Back) 다시 탐색(Tracking)을 진행하는 방식인 것이다.

<br>

그래서 DFS와 백트래킹 간에는 이러한 차이점이 있는 것이다.