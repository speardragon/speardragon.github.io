---
layout: single
title: "[Algorithm] Dynamic Programming(다이나믹 프로그래밍)"
categories: ['Lecture', 'Data structure and algorithms', 'Algorithm', 'DynamicProgramming']
tag: ['Data structure', 'Algorithm', 'DynamicProgramming', '다이나믹 프로그래밍', '동적 계획법']
toc: false
cover: '/assets/images/datalgoritm.png'
toc_sticky: true
---

<br>

## Dynamic Programming - 다이나믹 프로그래밍

이번 시간에는 다이나믹 프로그래밍(동적 계획법 ,DP)에 대해서 배워 보도록 하자.



#### 1. 다이나믹 프로그래밍이란?

`Dynamic Programming 줄여서 DP(또는 동적 계획법)`는 <span style="color:red">특정 범위까지의 값을 구하기 위해서 그것과 다른 범위까지의 값을 이용하여 효율적으로 값을 구하는 알고리즘 설계 기법</span>이다.

위의 말을 쉽게 말해 아래와 같이 말할 수 있다.

- 하나의 큰 문제를 여러 개의 작은 문제로 나누어 그 결과를 저장하여 다시 큰 문제를 해결할 때 사용하는 것
- 큰 문제를 작은 문제로 쪼개서 그 답을 저장해 두었다가 **재활용**(기억하며 풀기)



#### 2. 왜 DP를 쓰는 것일까?

사실 일반적인 재귀(Naive Recursion)방식 또한 DP와 매우 유사하다고 볼 수 있는데, 차이점은 일반적인 재귀를 단순히 사용할 때 작은 문제 하나에서 작은 문제가 있다면 이를 계속 반복하여야 하기 때문에 규모가 큰 계산에서 굉장히 비효율적이게 될 수 있다는 것이다.

DP의 예시인 피보나치 수열을 보면서 왜 DP를 사용하는 지 이해해 보도록하자.

피보나치 수열은 재귀함수로도 풀 수 있는 유명한 문제이다.

피보나치 수열은 다음과 같다.

- 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144 ...

이를 구하고 싶을 때 재귀로 함수를 구성하는 방법은 매우 단순하다.

``` 
return f(n) = f(n-1) + f(n-2)
```

그런데 f(n-1), f(n-2)에서 각 함수를 1번씩 호출하면 동일한 값을 2번씩 구하게 되고 이로 인해 100번째 피보나치 수를 구하기 위해 호출되는 함수의 횟수는 기하급수 적으로 증가한다.(약 **7해번 이상 함수 호출**, 아마 죽을 때까지 계산을 해야할 수도 있다.)

코딩 문제는 그 문제 자체만 풀면 되긴 하지만 만약 입력 조건의 범위가 매우 큰 경우에 위와 같이 굉장히 오랜 시간 걸릴 수 있다는 것이다.

<br>

![image](https://user-images.githubusercontent.com/79521972/154906849-34a2419d-f80d-4235-970f-f7d4a33f064b.png)

그러나 한 번 구한 작은 문제의 결과 값을 저장해두고 재사용 한다면 어떨까? 앞에서 계산된 값을 다시 반복할 필요가 없이 약 200회 내에 계산이 가능해진다.

즉, 매우 효율적으로 문제를 해결할 수 있게 된다. 시간복잡도를 기준으로 아래와 같이 개선이 가능하다.
시간 복잡도 측면에서**O(n^2) → O(f(n)) 로 개선**이 가능해 진다. (다항식 수준으로, 문제에 따라 다름.)



<br>

##### 분할 정복(Divide and Conquer)과의 차이점?

분할 정복과 다이나믹 프로그래밍 모두 주어진 문제를 작은 문제로 작게 쪼개서 서브 문제들을 해결하고 이를 토대로 큰 문제를 해결한다는 점에서 비슷하게 보일 수 있다.

둘이 다른 점은

- 분할 정복의 경우, 분할된 하위 문제가 동일하게 **중복이 일어나지 않는 경우**에 쓰이고
- 다이나믹 프로그래밍의 경우, 분할된 하위 문제가 동일하게 **중복이 일어나는 경우**에 쓰이는 것이다.



 

#### 3. DP의 사용 조건

앞서 배운 그리디 알고리즘과 마찬가지로 DP도 사용 조건이 있다.

1. <span style="color: red">Overlapping Subproblems(겹치는 부분 문제)</span>
2. <span style="color: red">Optimal Substructure(최적 부분 구조)</span>

<br>

①Overlapping subproblems

DP는 기본적으로 문제를 나누고 그 문제의 결과 값을 재활용해서 전체 답을 구한다. 그래서 **동일한 작은 문제들이 반복해서 나타나는 경우**에 사용이 가능하다.

즉, DP는 부분 문제의 결과를 저장해서 다시 계산하지 않을 수 있어야 하는데, 해당 부분 문제(subproblems)가 반복적으로 나타나지 않는다면 재사용이 불가능하니 부분 문제가 중복되지 않는 경우에는 사용할 수 없다.

 

②Optimal Substructure(최적 부분 구조)

부분 문제(subproblems)의 최적 결과 값을 사용해 전체 문제의 최적 결과를 낼 수 있는 경우를 의미한다. 그래서 특정 문제의 정답은 문제의 크기와는 상관없이 항상 동일하다.



#### DP를 사용하는 경우

DP는 특정한 경우에 사용하는 알고리즘이 아니라 하나의 방법론이기 때문에 다양한 문제해결에 쓰일 수 있다. 그렇기 때문에 DP를 적용할 수 있는 문제인지 아닌지를 알아내는 것부터 가 시작이다.

과정은 다음과 같이 진행된다.

1. DP로 풀 수 있는 문제인지 확인
2. 문제의 변수 파악
3. 변수 간 **관계식(점화식)** 만들기
4. <mark>메모하기(Memoization or tabulation)</mark>
5. 기저 상태 파악하기
6. 구현하기



1. **DP로 풀 수 있는 문제인지 확인**

이 부분부터 상당히 어려운데 DP의 조건에 의해 현재 직면한 문제가 작은 문제들로 쪼개질 수 있는 지, 즉 하나의 함수로 표현될 수 있는지를 판단해야 한다.

즉, 위의 조건들을 면밀히 하나씩 만족되는 지 체크를 해 본다. 

일반적으로 특정 데이터 내 최대화나 최소화 계산을 한다거나 특정 조건 내에서 데이터를 세야하는 경우, 확률 등의 계산을 할 때 DP를 통해 푸는 경우가 많이 있다.

<br>

2. **문제의 변수 파악**

DP는 현재 변수에 따라서 나오는 결과 값을 찾고 그것을 전달함으로써 재사용하는 과정을 거친다. 즉, 문제 내에서 변수의 개수를 알아내야 한다는 것, 이것을 영어로 `"state"`를 결정한다고 한다.

예를 들어, **피보나치 수열**에서는 n번째 숫자를 구하는 것이므로 'n'이 변수가 된다. 그 변수가 얼마이냐에 따라 결과값이 다르지만 그 결과를 재사용하는 것이다.

또한, 문자열 간의 차이를 구할 때는 문자열의 길이, Edit 거리 등 2가지 변수를 사용한다.

<br>

3. **변수 간 관계식 만들기**

변수들에 의해 결과 값이 달라지지만 동일한 변수값인 경우 결과는 동일하다. 또한 우리는 그 결과값을 그대로 이용할 것이므로 그 관계식을 만들어낼 수 있어야 한다.

그러한 식을 **점화식**이라고 부르며 그를 통해 우리면 짧은 코드 내에서 반복/재귀를 통해 문제가 자동으로 해결되도록 구축할 수 있게 된다.

예를 들어 피보나치 수열에서는 f(n) = f(n-1) + f(n-2) 였다. 이는 변수의 개수, 문제의 상황마다 모두 다를 수 있다.

<br>

4. **메모하기**

영어로 Memoization. 사실 이 DP의 핵심은 Memoization이라고 봐도 무방하다. 

변수 간 관계식까지 정상적으로 생성되었다면 **변수의 값에 따른 결과를 저장**해야 한다. 이것을 메모한다고 하여 **Memoization**이라고 부르는 것이다.

변수 값에 따른 결과를 저장할 배열 등을 미리 만들고 그 결과를 나올 때마다 배열 내에 저장하고 그 저장된 값을 재사용하는 방식으로 문제를 해결해 나간다.

이 결과 값을 저장할 때는 보통 배열을 쓰며 변수의 개수에 따라 배열의 차원이 1~3차원 등 다양할 수 있다.

<br>

5. **기저 상태 파악하기**

여기까지 진행했으면, **가장 작은 문제**의 상태를 알아야 한다. 보통 몇 가지 예시를 직접 손으로 테스트하여 구성하는 경우가 많다. 

**피보나치 수열**을 예시로 들면, f(0) = 0, f(1) = 1과 같은 방식이다. 이후 두 가지 숫자를 더해가며 값을 구하지만 가장 작은 문제는 저 2개로 볼 수 있다.

우리가 초등학교, 중학교 시절 점화식을 배울 때 0이나 1의 값을 넣어보고 이를 토대로 식을 이끌어 내는 것과 마찬가지라고 볼 수 있다.

해당 기저 문제에 대해 파악 후 미리 **배열** 등에 저장해두면 된다. 이 경우, 피보나치 수열은 매우 간단했지만 문제에 따라 좀 복잡할 수 있다.

<br>

6. **구현하기**

위와 같이 DP를 사용할 수 있는 판이 깔렸다면 DP를 사용해야 한다. 이 때 사용하는 방법에는 두 가지 방식으로 나뉘는데 다음과 같다.

1) Bottom-Up (Tabulation 방식) - 반복문 사용
2) Top-Down (Memoization 방식) - 재귀(Naive Recursion) 사용

 

<mark>구현 방법</mark>

<u>**Bottom-Up**</u>

아래에서 부터 계산을 수행하면서 이를 통해 누적된 것으로 전체 큰 문제를 해결해 나가는 방법이다.

Memoization을 위해 dp 배열을 만들고 이를 1차원이라고 가정하자. dp[0]이 기저 상태이고 dp[n]을 구하여 한다고 했을 때 Bottom-Up은 **dp[0]**부터 시작해서 반복문을 통해 점화식으로 결과를 도출하여 dp[n]까지 그 값을 이동시켜 재활용하는 방식이다.

이는 **Tabulation**이라고도 하는데, 그 이유는 반복을 통해 dp[0]부터 하나씩 채우는 과정을 "table-filling"이라고 하며, 이 table에 저장된 값에 직접 접근하여 재활용하기 때문에 Tabulation이라고 한다.

<br>

**<u>Top-Down</u>**

위와 다르게 dp[0]에서 시작하는 대신 dp[n]을 찾아내기 위해서 위에서부터 바로 호출을 진행한다.

dp[0] 상태까지 내려간 다음 해당 결과 값을 재귀를 통해 이동시켜 재활용하는 방식이다.

이것은 또한 이미이전에 계산을 완료한 경우에는 단순히 메모리에 저장되어 있던 내역을 꺼내서 활용하면 되기 때문에 **가장 최근 상태 값을 메모**해 두었다고 해서 **Memoization**이라고 부르는 것이다.



### 코드로 구현(Java)

```java
packge com.test;

public class Fibonacci{
    // DP 를 사용 시 작은 문제의 결과값을 저장하는 배열
    // Top-down, Bottom-up 별개로 생성하였음(큰 의미는 없음)
    static int[] topDown_memo; 
    static int[] bottomup_table;
    public static void main(String[] args){
        int n = 30;
        topDown_memo = new int[n+1];
        bottomup_table = new int[n+1];
        
        long startTime = System.currentTimeMillis();
        System.out.println(naiveRecursion(n));
        long endTime = System.currentTimeMillis();
        System.out.println("일반 재귀 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(topDown(n));
        endTime = System.currentTimeMillis();
        System.out.println("Top-Down DP 소요 시간 : " + (endTime - startTime));
        
        System.out.println();
        
        startTime = System.currentTimeMillis();
        System.out.println(bottomUp(n));
        endTime = System.currentTimeMillis();
        System.out.println("Bottom-Up DP 소요 시간 : " + (endTime - startTime));
    }
    
    // 단순 재귀를 통해 Fibonacci를 구하는 경우
    // 동일한 계산을 반복하여 비효율적으로 처리가 수행됨
    public static int naiveRecursion(int n){
        if(n <= 1){
            return n;
        }
        return naiveRecursion(n-1) + naiveRecursion(n-2);
    }
    
    // DP Top-Down을 사용해 Fibonacci를 구하는 경우
    public static int topDown(int n){
        // 기저 상태 도달 시, 0, 1로 초기화
        if(n < 2) return topDown_memo[n] = n;
        
        // 메모에 계산된 값이 있으면 바로 반환!
        if(topDown_memo[n] > 0) return topDown_memo[n];
        
        // 재귀를 사용하고 있음!
        topDown_memo[n] = topDown(n-1) + topDown(n-2);
        
        return topDown_memo[n];
    }
    
    // DP Bottom-Up을 사용해 Fibonacci를 구하는 경우
    public static int bottomUp(int n){
        // 기저 상태의 경우 사전에 미리 저장
        bottomup_table[0] = 0; bottomup_table[1] = 1;
        
        // 반복문을 사용하고 있음!
        for(int i=2; i<=n; i++){
            // Table을 채워나감!
            bottomup_table[i] = bottomup_table[i-1] + bottomup_table[i-2];
        }
        return bottomup_table[n];
    }
}
```



**결과**

```
832040
일반 재귀 소요 시간 : 9

832040
Top-Down DP 소요 시간 : 0

832040
Bottom-Up DP 소요 시간 : 0
```





