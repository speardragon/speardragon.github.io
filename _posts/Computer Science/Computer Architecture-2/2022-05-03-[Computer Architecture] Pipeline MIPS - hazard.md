---
layout: single
title: "[Computer Architecture] Pipelined MIPS - hazard"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Pipeline', 'hazard']
---

<br>

전에 배운 single cylce에 pipeline을 더해서 성능을 획기적으로 상승시킨다. 그러나 이 과정에서 발생하는 문제, 혹은 위험(hazard)이 있는데 이는 다음과 같다.

- Structural hazard
  - HW는 하나인데 같은 cylce에 두 개의 명령어가 쓰려고 함.
- Data hazard
  - 두 개의 명령어가 같은 storage에 접근
  - 명령어가 정순서로 뽑아질 때 나옴
- Control hazard
  - 한 명령어가 다음 명령어에 영향을 끼친다.

<br>

### Structural hazard

![image](https://user-images.githubusercontent.com/79521972/166847210-24d111d2-6958-43fb-8c97-1936d39a13f6.png)

모든 instruction의 수행이 IF-ID-EX-MEM-WB으로 이루어진다고 가정했을 때 4번 째 Cycle에서는 첫 번째 instruction의 Memory Access와 네 번째 instruction의 IF가 동시에 Memory Access를 요구하는 것을 볼 수 있다. 메모리는 당연히 어디서 해당 signal을 받았는지 알 수 없기 때문에 결론적으로 Resource Conflict 현상이 발생하는 것이다.

- 이에 대한 해결책으로는 Stall로, 저렇게 중복해서 쓰는 부분에 대해서는 다른 instruction 처리 시 쉬어주게끔 고려하는 것이다. 즉, 기기적으로 이상이 없음에도 일부러 수행을 멈추게 하여 비용적인 측면에서는 더 효율적이겠지만 아무래도 성능이 좋을 순 없다.

<br>

### Data Hazard

Data에 대한 Read/Write Sequence가 엇갈리면서 발생하는 문제이다. 간단히 두 번째 instruction 결과가 첫 번째 instruction에 dependent하기에 발생하는 문제인 것이다.

이에 따른 경우는 다음 3가지와 같이 정의된다.

- Read After Write(RAW)
- Write After Write(WAW)
- Write After Read(WAR)

위를 보면 Write 과정이 data hazard를 야기하는 주된 원인임을 알 수 있을 것이다.



<br>

## Load-use hazard

![image](https://user-images.githubusercontent.com/79521972/166400646-cb957cc9-dd55-432d-9ae7-987ee29b9e71.png)

lw의 계산 결과는 DM의 뒤에 나오기 때문에 이전의 hazard와 같이 forwading 방식으로 ALU에서 가져와도 그 연산 결과는 주소값일 뿐 실질적으로 원하는 register 값이 아니기 때문에 그 방식으로는 해결할 수 없다.

<br>

## Stalling

![image](https://user-images.githubusercontent.com/79521972/166402010-786cd9e3-52bc-4ca2-84a5-c02ca38dc8f4.png)

앞의 데이터가 준비 되기 전까지 그 데이터를 쓰는 공간을 아무것도 하지 않고 가만히 냅둔다. (stall)

즉, 그 상태를 그대로 유지하는 것이다.

- 그러기 위해선 다음 두 조건을 만족해야 한다.
  - 새로운 명령어가 들어오지 않는다.
  - register file에 data가 update되지 않는다.

<br>

## Stalling Hardware

![image](https://user-images.githubusercontent.com/79521972/166853077-3ba70154-c560-40fe-89aa-ad8fb12f7d66.png)



![image](https://user-images.githubusercontent.com/79521972/166853247-b71c01ef-a750-4d4d-9e3f-469614e31685.png)![image](https://user-images.githubusercontent.com/79521972/166853273-82a87cbf-1cd8-462f-910b-1820eca8608c.png)

StallF와 StallD 신호를 통해 IF와 ID의 register를 disabled 시키는 것이다.

그렇게 되면 E방에는 데이터가 한 clock 동안 비어있게 되는데 이 과정을 flush라고 하고 해당 동작을 FlushE signal이 관할하여 clear 시키는 역할을 한다.

<br>

## Stalling Logic

![image](https://user-images.githubusercontent.com/79521972/166853299-b9bc0ba8-d6e0-40ab-83b6-32aa5deaa556.png)

lw 명령어인 경우를 판별하기 위해서 MemtoReg를 사용하였다.



<br>

## Control Hazards

- beq:  
  - branch not determined until 4th stage of pipeline 
    - pipeline penalty

  - Instructions after branch fetched before branch occurs 
  - These instructions must be **flushed** if branch happens

- Branch misprediction penalty
  - number of instruction flushed when branch is taken
  - May be reduced by determining branch earlier




<br>

## Control Hazards: Origianl Pipleine

![image](https://user-images.githubusercontent.com/79521972/166402777-05c6df3b-b8a1-46bb-987d-ca55529f91e4.png)

비교를 M방에서 하면 앞에서 만들어진 데이터가 모두 버려져야 하기 때문에 비효율적인 동작이 발생하게 된다.

<br>

## Control Hazards

![image](https://user-images.githubusercontent.com/79521972/166402793-c2783595-e9a8-4e88-be13-d4b125344659.png)

beq 명령어로 인해 jump하고자 하는 label과 beq 명령 사이에 있는 instruction 들은 쓸모가 없어지기 때문에 Flush 되어야 한다.



<br>

## Early Branch Resolution

그래서 조금이라도 이 penalty를 줄이기 위해서 M방에서 check하던 AND gate 대신에 D방의 comparator를 추가하여 해결할 것이다. 

또한 BTA를 계산하던 ALU 모듈 역시 D방으로 옮겨서 조금이라도 빨리 비교하고 jump를 할 수 있도록 한 것이다.

![image](https://user-images.githubusercontent.com/79521972/166402814-b3770c05-d10f-4fe4-845a-c9beca9e223c.png)

Introduced another data hazard in **Decode stage**

- BTA 계산 하는 모듈
- 같은 지를 비교하기 위한 comparator 

그런데 이렇게 하자니 새로운 hazard가 다시 발생한다.

<br>

## Early Branch Resolution

![image](https://user-images.githubusercontent.com/79521972/166402838-44d57870-0a73-4436-b912-059bcd50f1fd.png)

- 왼쪽의 code는 두 개 앞에 가고있는 clock이 add이기 때문에 forwarding으로 해결 가능한 문제이다.
  - 다음 다음 clock에서는 ALU에서 이미 계산이 완료 되었기 때문에


- 그런데 만약 오른쪽의 code처럼 두 clock 앞에 가는 명령이 lw 이거나 바로 앞에 가는 clock에서 add와 같은 register write이 일어난다면 그 때는 ALU 계산을 하기도 전이라서 새로운 data hazard를 발생하기 때문에 이 경우에는 stall이 필요하다.

Q) 만약에 lw 명령어에서 rt에 load 하는 명령 바로 다음 beq에서 그 레지스터를 사용하면 stalling을 두 클락동안 해야 하나요?





<span style="color:red">(위와 같은 코드를 주고) 이랬을 때 어떤 문제(hazard)가 발생하느냐? -> 시험문제</span>

<br>

## Handling Data Dependency for Branch inst.

![image](https://user-images.githubusercontent.com/79521972/166402919-d9511429-6fdb-4aeb-9608-c35b0d990d94.png)





<br>

## Control Forwarding &amp; Stalling Logic

- **Forwarding logic:** 

![image](https://user-images.githubusercontent.com/79521972/166403016-d513e2c6-1196-41bb-ae55-f34ec0263efb.png)

두 개 앞에 가는 명령이 reg write



- **Stalling logic:**

![image](https://user-images.githubusercontent.com/79521972/166403100-5d2f49d5-2479-443b-9ba5-f891747f4c67.png)



![image](https://user-images.githubusercontent.com/79521972/166403082-2cfb57ba-bf2d-4576-81e2-7d3cfc90b628.png)

한 개 앞에 가는 명령이 reg write || 두 개 앞에 가는 명령이 lw (lwstall 인 듯 위에는 표시가 안 되어 있음)



MemtoReg: lw명령어만 갖고 있는 control signal

즉, 두 개 앞에 가고 있는 명령어가 lw일 때를 의미한다.



<br>

## Pipielined Processor with Full Hazard Handling



![image](https://user-images.githubusercontent.com/79521972/166854611-6111dfea-4b2b-466e-b061-26844de96f31.png)

beq을 check 하기 위해서 ALU를 사용하지 않고 comparator를 사용하였음



<br>

## Handling jump instruction

![image](https://user-images.githubusercontent.com/79521972/166403389-fff443f5-9e1a-489a-a724-3958667f1ba1.png)



jump는 condition을 따지지 않고 바로 실행하기 때문에 control 신호가 branch와 OR에 들어가서 fetch한 명령어를 clear시킨다.

- fetch stage should be flushed.

- 이미 들어와 있는 것을 clear 시킴
- 그러고 나서 새로운 명령어가 fetch된다.

위 그림에서 control input/output에 대해 생각해 보기(0503 강의 마지막 부분)



---

## Branch Prediction

- Guess whether branch will be taken 
  - Backward branches are usually taken (loops) 
  - Consider history to improve guess 
  - Good prediction reduces fraction of branches  requiring a flush























