---
layout: single
title: "[Computer Architecture] Pipelined MIPS"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Pipeline']
---

<br>

## Load-use hazard

![image](https://user-images.githubusercontent.com/79521972/166400646-cb957cc9-dd55-432d-9ae7-987ee29b9e71.png)



<br>

## Stalling

![image](https://user-images.githubusercontent.com/79521972/166402010-786cd9e3-52bc-4ca2-84a5-c02ca38dc8f4.png)

앞의 데이터가 준비 되기 전



<br>

## Stalling Hardware

![image](https://user-images.githubusercontent.com/79521972/166402144-8a4230c9-47fd-4e07-800d-22097ab6533a.png)





<br>

## Staing Logic

lwstall = 

- ((rsD == rtE) oR (rtE==r))



<br>

## Control Hazards

- beq:  
- branch not determined until 4th stage of pipeline 
  - Instructions after branch fetched before branch occurs 
  - These instructions must be flushed if branch happens
  -  Branch misprediction penalty
- number of instruction flushed when branch is taken
  - May be reduced by determining branch earlier



<br>

## Control Hazards: Origianl Pipleine

![image](https://user-images.githubusercontent.com/79521972/166402777-05c6df3b-b8a1-46bb-987d-ca55529f91e4.png)



<br>

## Control Hazards

![image](https://user-images.githubusercontent.com/79521972/166402793-c2783595-e9a8-4e88-be13-d4b125344659.png)





<br>

## Early Branch Resolution

![image](https://user-images.githubusercontent.com/79521972/166402814-b3770c05-d10f-4fe4-845a-c9beca9e223c.png)

Introduced another data hazard in Decode stage

<br>

## Early Branch Resolution

![image](https://user-images.githubusercontent.com/79521972/166402838-44d57870-0a73-4436-b912-059bcd50f1fd.png)



<span style="color:red">(위와 같은 코드를 주고) 이랬을 때 어떤 문제(hazard)가 발생하느냐? -> 시험문제</span>

<br>

## Handling Data Dependency for Branch inst.

![image](https://user-images.githubusercontent.com/79521972/166402919-d9511429-6fdb-4aeb-9608-c35b0d990d94.png)





<br>

## Control Forwarding & Stalling Logic

- Forwarding logic: 

![image](https://user-images.githubusercontent.com/79521972/166403016-d513e2c6-1196-41bb-ae55-f34ec0263efb.png)





- Stalling logic:

![image](https://user-images.githubusercontent.com/79521972/166403100-5d2f49d5-2479-443b-9ba5-f891747f4c67.png)



![image](https://user-images.githubusercontent.com/79521972/166403082-2cfb57ba-bf2d-4576-81e2-7d3cfc90b628.png)

MemtoReg: lw명령어만 갖고 있는 control signal

즉, 두 개 앞에 가고 있는 명령어가 lw일 때를 의미한다.



<br>

## Pipielined Processor with Full Hazard Handling

![image](https://user-images.githubusercontent.com/79521972/166403290-ecabb329-3c24-4320-a42e-a99cd1ff9aea.png)

beq을 check 하기 위해서 ALU를 사용하지 않고 comparator를 사용하였음



<br>

## Handling jump instruction

![image](https://user-images.githubusercontent.com/79521972/166403389-fff443f5-9e1a-489a-a724-3958667f1ba1.png)



fetch stage should be flushed.

- 이미 들어와 있는 것을 clear 시킴

control input/output에 대해 생각해 보기































