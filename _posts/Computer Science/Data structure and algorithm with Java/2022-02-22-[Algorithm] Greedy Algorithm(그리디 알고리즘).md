---
layout: single
title: "[Algorithm] Greedy Algorithm(그리디 알고리즘)"
categories: ['Computer Science', 'Data structures and algorithms with Java', 'Algorithm', 'Greedy']
tag: ['Data structure', 'Algorithm', 'Greedy', '그리디 알고리즘']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

<br>

## Greedy Algorithm(탐욕 알고리즘)

이번 시간에는 그리디 알고리즘에 대해서 배워 볼 것이다.

<br>

#### 그리디(탐욕) 알고리즘이란?

- 단어에서 알 수 있듯이 'Greedy'라는 단어가 '탐욕스러운', '욕심 많은' 이라는 뜻을 내포하고 있다.

- 그래서 말 그래도 `선택의 순간마다 당장 눈 앞에 보이는 최적의 상황만을 쫓아 최종 답에 도달`하는 알고리즘이다.
- 최적해를 구하는 데 사용되는 `근사적인` 방법이다.
- 여러 경우 중 하나를 결정해야 할 때마다 그 순간에 최적이라고 생각이 드는 것을 선택해 나가는 방식이다.
- <span style="color:blue">매 선택에서 현재 당장 최적인 값이다.</span>

그렇지만 <mark>그리디 알고리즘은 전체에서 최적값을 언제나 구할 수는 없다.</mark>

다음과 같은 그래프를 보자.

![image](https://user-images.githubusercontent.com/79521972/154899733-b7b71e9f-a089-43e2-a63d-fdcd415fc7a8.png)

A에서 출발하여 E에 가야한다고 할 때 경우의 수는 다음과 같다.

1. 먼저 B or C or D 중에서 가야할 길을 찾는다.
   - 가장 걸리는 시간이 짧은 C경로로 간다.
2. 그리디 알고리즘은 이 최적의 선택으로 끝까지 쭉 진행하기 때문에 다른 경로는 고려하지 않는다.
3. 그 후 바로 E에 도착하여 총 **41시간**이 소모 되었다.
   - 하지만 A-C-E로 갈 때 보다 A-B-E로 갈 때 뒤의 소모 되는 시간이 **더 적어** 시간 소모가 더 적은데
   - 최초의 선택에서 최적의 선택은 했지만 **전체 문제**를 봤을 때 최적의 값은 구하지 못하게 된 것이다.

이와 같은 그리디 알고리즘의 단점이 존재할 수 있는데 그럼에도 불구하고 그리디 알고리즘은 **속도가 매우 빨라** 간단한 문제에서 그 힘을 발휘할 수 있다.

<br>



하지만 항상 최적해가 되지 않는다는 제약에 의해 아래와 같은 특수한 조건이 만족되어야 한다.

1. <span style="color: red">탐욕 선택 속성(Greedy Choice Property)</span>
   - 이전의 선택이 이후의 선택에 영향을 주지 않는다.
2. <span style="color: red">최적 부분 구조(Optimal Substructure)</span>
   - 문제에 대한 최종 해결 방법은 부분 문제에 대한 최적 문제 해결 방법으로 구성 된다.
   - 즉, 부분 문제의 최적결과가 전체에도 그대로 적용될 수 있어야 하는 것이다.

<br>

이러한 조건이 성립하지 않는 경우 그리디 알고리즘의 최적해는 구할 수 없고 만약 이 조건을 만족한다면 어떤 특별한 구조가 있는 문제에 대해서 이 그리디 알고리즘이 언제나 최적해를 찾아낼 수 있는데 이 구조를 매트로이드라 한다.

<br>

위의 2번 조건은 뒤에서 배울 Dynamic Programming의 2번 조건과 동일한 조건인데 이에 대해 간단히 설명하자면 다음과 같다.

- DP의 경우, 문제가 Overlapping되기 때문에 다음에 풀 문제가 이전의 작은 문제의 결과에 영향을 받게 된다. 즉, 다시말해서 동일한 형식의 문제만 점점 커지는 것일 뿐, 이전의 경우에 영향을 받는다는 것이다.

- <span style="color:blue">하지만 그리디 알고리즘은 이와 달리 영향을 받아서는 안 된다.</span> 왜냐하면 그래야지 다른 경우의 결과와는 상관없는 최적해를 구할 수 있기 때문이다.



<br>

**정리**

> 따라서 그리디 알고리즘은 항상 최적의 결과를 도출하는 것은 아니지만, 어느 정도 최적의 근사한 값을 빠르게 도출해낼 수 있다는 장점이 있다. 이 장점으로 인해 그리디 알고리즘은 <u>근사 알고리즘</u>으로 사용할 수 있다.

> 그리디 알고리즘을 적용해도 언제나 최적의 해를 구할 수 있는 문제(매트로이드)가 있고, 이러한 문제에 탐욕 알고리즘을 사용해서 빠른 계산 속도로 답을 구할 수 있다.



<br>

### 그리디 알고리즘의 예시

#### Action Selection 문제

그리디 알고리즘의 가장 대표적인 예시 중 하나인 `활동 선택(Action Selection) 문제`에 대해 살펴보자.

활동 선택 문제는 <span style="color: red">N개의 활동에 대해 각 활동에는 시작 시간 및 종료 시간이 있을 때, 한 사람이 최대한 많이 할 수 있는 활동(Acticity)의 수를 구하는 문제</span>이다.

즉, 쉽게 말해서 각각의 활동(Activity)에는 시간이 소모되므로 하나를 선택했다면 그 동안에는 해당 시간에 다른 활동을 할 수 없다. 이 때 가장 많은 활동에 참여하기 위한 방법을 찾아내는 것이다.

아래 표를 보면서 이해해 봅시다.

![image](https://user-images.githubusercontent.com/79521972/154902519-a4d09832-314e-4ddd-9c9d-7c0f0582f8ff.png)

위에서 설명한 각 활동과 그것들의 시작/ 종료 시간이 설정 되어 있음을 알 수 있다. 이 문제는 이때 최대한 많은 활동을 해야 하므로 언제 시작하든지 간에 <span style="color: blue">전체에서 가장 종료 시간이 빠른 것부터 선택해야 한다.</span>

어차피 시작 시간은 항상 종료 시간 이전일 것이기 때문에 고려하지 않아도 된다.

그래서 <span style="color: purple">종료 시간을 기준으로 먼저 정렬</span>한 뒤, <span style="color: red">제일 먼저 끝나는 활동을 무조건 선택</span>하고 끝났을 때, <span style="color: red">바로 다음에 선택할 수 있는 활동</span>을 찾아 수행하는 방식을 **반복**하여 해결할 수 있다.

<br>

![image](https://user-images.githubusercontent.com/79521972/154902909-bb8fbb4d-12ed-4788-ad75-27dbd64b2cb7.png)

1. 먼저 종료 시간 기준으로 정렬 후 제일 먼저 끝나는 D 활동을 선택한다.
2. D 활동이 종료하면 바로 다음 활동 가능한 C 활동을 선택한다.
3. C 활동 종료 후 B 활동을 시작 할 수 없으므로 건너 뛰고 활동이 가능한 시간인 A 활동을 선택한다.
4. A 활동도 종료하면 그 다음으로 활동 가능한 F를 선택하여 마무리 된다.

<br>

### <mark>코드로 구현</mark>

```java
import java.util.ArrayList;
import java.util.Collections;

public class Main{
    public static void main(String[] args){
        // 활동 정보를 List에 저장하고 정렬한다.
        ArrayList<Action> list = new ArrayList<>();
        list.add(new Action("A", 7, 8));
        list.add(new Action("B", 5, 7));
        list.add(new Action("C", 3, 6));
        list.add(new Action("D", 1, 2));
        list.add(new Action("E", 6, 9));
        list.add(new Action("F", 10, 11));
        Collections.sort(list);
        
        // Greedy 알고리즘 수행 후, 결과 출력
        ArrayList<Action> ans = greedy(list);
        for(Action act : ans){
            System.out.print(act.name + ", ");
        }
    }

    // Greedy 알고리즘을 통해 선택된 활동들을 반환한다.
    private static ArrayList<Action> greedy(ArrayList<Action> list){
        int time = 0;
        ArrayList<Action> ans = new ArrayList<>();

        for(Action act : list){
            if(time <= act.startTime){
                time = act.endTime;
                ans.add(act);
            }
        }
        return ans;
    }
}

// 활동 정보를 가진 Class로 Comparable을 구현하여 종료 시간 기준 오름차순으로 정렬함.
class Action implements Comparable<Action>{
    String name;
    int startTime;
    int endTime;
    public Action(String name, int startTime, int endTime){
        this.name = name;
        this.startTime = startTime;
        this.endTime = endTime;
    }

    @Override
    public int compareTo(Action a) {
        return this.endTime - a.endTime;
    }

    @Override
    public String toString() {
        return this.name;
    }
}
```



<mark>결과</mark>

```
D, C, A, F, 
```

<br>



### 예시 2 - 거스름돈 문제

이번엔 그리디 알고리즘의 유명한 예시인 거스름돈 문제에 대해서 보도록 하자.

이번엔 돈을 낸 뒤, 거스름돈을 최소 개수의 동전 지폐의 조합으로 주는 경우, 그 개수를 구하는 문제이다.

예를 들어, 

- 누군가 **<u>2,730원 어치의 물건을 사고 5,000원을 냈다고 가정</u>**하면 **<u>거스름돈은 총 2,270원</u>** 일 것이다.
- 이때, 지폐와 동전 종류가 아래와 같을 때, 최소의 개수로 거스름돈을 주는 경우는 어떤 경우일까?
  - 지폐 및 동전의 종류: 1,000원, 500원, 100원, 50원, 10원
  - 정답: 1,000원 2개 / 100원 2개 / 50원 1개 / 10원 2개
  - 총 7개

이는 MSD(Most Significant Digit)으로 풀어낸 것으로 가장 중요한 단위를 먼저 계산하는 것을 의미한다. 위의 경우에서 자신보다 더 큰 지폐 혹은 동전으로 거스름 돈을 준다면 나머지는 무조건 더 적은 개수로 반환하는 것이 가능하기 때문에 이를 사용할 수 있다.

<br>

따라서, **이전의 선택과 관련없이** <span style="color: red">현재 상태에서 최선의 결과를 선택하여 전체에서 최적의 결과를 낼 수 있게 된다.</span>

하지만 만약 거스름돈이 120원을 줘야하는 상황에서 거스름돈으로 줄 수 있는 동전의 종류가 다음과 같다면 어떨까?

- 동전의 종류: 50원, 40원, 10원

<br>

이를 MSD 원리로 풀게 되면 <span style="color: blue">50원 2개 / 10원 2개</span>로 총 <span style="color: blue">4개</span>가 나오게 된다.

<br>

그런데 사실 이 문제는 40원짜리 3개를 거슬러 주면 더 적은 개수로 거스름돈을 줄 수 있다.

이러한 문제가 발생한 이유는 40원은 자신보다 큰 동전(50원)의 약수가 아니기 때문에 50원은 40원을 대체해 더 적은 숫자로 반환하는 경우라고 보장할 수 없게 된다.

<br>

>  이처럼, 그리디 알고리즘으로 해결할 수 있으려면 조건을 충분히 만족하는 지를 검증할 수 있어야 한다.



## 관련된 문제

[<u>백준 5585번 거스름</u>돈](https://www.acmicpc.net/problem/5585)

[<u>백준 11047번 동전</u>](https://www.acmicpc.net/problem/11047)

[<u>백준 11000번 강의실 배정</u>](https://www.acmicpc.net/problem/11000)







