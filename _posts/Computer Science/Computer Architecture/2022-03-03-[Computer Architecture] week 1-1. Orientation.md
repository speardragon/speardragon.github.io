---
layout: single
title: "[Computer Architecture] 03.03 Orientation"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']

---

 miPs vs. ARM vs. RISC-V



**mips**: wokrstation, embede system에 사용

**ARM**: ARM이라는 회사에서 사용,운용

**RISC-V**: open-source

mips가지고 공부를 하고 arm하고 어떤 차이가 있지? risc-v하고 무슨 차이가 있지?



mips와 risc-v는 95%의 의 유사도

mips와 arm은 거의 다르다.



우리 수업에서는 mips를 가지고 공부를 하되 차이점도 알아가도록 하겠다.

<Br>

컴퓨터 구조 파트는 mips, arm, risc-v하고 차이가 있기 때문에 arm edition은 힘들 것이다.

<br>

교재는 그대로 사용(디지털 공학때 사용했던 것, Mips version)

<br>
instruction set: 스펙

<br>

로보트를 가지고 있다고 생각해 보자.

로보트한테 커피를 탈때 설탕, 가루, 시럽 등을 특정 비율로 만들어서 가져오라고 하자.

`주방에 가서 머피를 타서 가지고 와`

![image](https://user-images.githubusercontent.com/79521972/156500057-d950c27b-8890-40fa-b327-5cf4bc06039e.png)

주방에 가서 커피를 만드는데 커피 몇, 설탕 몇, 우유 몇의 비율을 제공한다.

이 명령을 이해하기 위해선 robot는 

<br>

로보트의 기능은 다음과 같다.

- 앞,좌우로 가기
- 뱅글뱅글 도는 기능
- 젓는 기능
- +, -

<br>

robot의 instruction(명령어)은 다음과 같을 것이다.

↑, →, ↑, ←, ↑, water, sugar, mix ..... (단순한 명령어들)

이를 robot의 스펙이라고도 하며 instruction이 탑재되었다고 한다.

스펙의 기능은 로봇마다 다를 것이며 위의 명령어가 모두 포함된 기능을 한 번에 할 수있는 명령어도 만들 수 있는 것이다.

- 이는 복잡한 명령어들이다.

<br>

즉, 컴퓨터가 알아먹을 수 있는 명령어(instruction)들의 모음을 스펙(spec)이라고 하는 것이다.

이것의 종류에 RISC, CISC 등이 있는 것이다.
RISC:

- MIPS
- ARM
- RISC-V

CISC:

- x86

<br>
복잡한 것들은 모두 빼고 최대한 단순한 것으로 구성하자

<br>

intel 프로세서는 instruction의 구성을 보니까 특정 몇 개 기능은 가뭄에 콩나듯 사용되는데 모든 기능을 다 넣으려다 보니까 규모가 너무 크다.(CISC)

그래서 주된 기능만 사용할 수 있는 컴퓨터를 만들면 간단해 지지 않을까 해서 만들어 본 것이 **RISC**

<br>

Instruction Set Architecture (ISA)

- 프로그래머가 보는

<br>

High-level language (Human friendly): int a; / a = b+c ....

assembly language: add, load, store ,...

machine language: 01011101 

<br>

```
int a,b,c <- data
main()
	a = b + c <- program
```

![image](https://user-images.githubusercontent.com/79521972/156502993-f13938a2-d381-4bc7-a8d4-c5f8ef714710.png)

```
load b
load c
add x,b,c
store x
```

와 같이 변환되어 넘어감

<br>

cpu는 굉장히 빠르지만 memory는 cpu보다는 느림



register(reg)파일에 저장하여 alu로 넘겨서 이를 다시 reg파일에 넘긴



load b가 의미하는 것은 b를 register 파일로 가져와라







<br>

### 이번학기에는?

- instruction에는 어떤 명령어가 있을떄

- 위의 것들을 수행할 수 있는 프로세서는 어떻게 생겼을까?

- architecture, micro-architecture

- cache - memory
- IO
- parallel processing(프로세싱이 여러개 있는 경우)



























































