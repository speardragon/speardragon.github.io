---
layout: single
title: "[System Programming] week1. Orientation"
categories: ['Lecture', 'System Programming']
tag: ['System Programming', 'Unix']
toc: true
toc_sticky: true
---







 오늘 얘기는 시험에 안 나옴

Algorithm: 컴퓨터가 이해하는 logic

<br>

1. What to do: 소프트웨어 프로그래머가 찾아야 하는 것으로 무엇에 대해 해결할지를 결정하는 것
   - 문제를 찾는 과정

2. How to do: 컴퓨터가 어떻게 what to do를 해결해야 할 지를 작성하는 것



Coding: Algorithms와 data structure 를 기반으로 program을 만드는 행위

- 사실 코딩은 그렇게 중요하지 않음

<br>

**Error**

- syntax error: 컴파일러가 잡아주기 때문에 해결이 어렵지 않음
- semantic error: 설계 과정에 문제로 해결하기가 까다로움

<br>

machine language를 instruction이라고 함: computer에게 내리는 지시니까

<br>

이 과목이 중요한 이유 

- 취업을 했을때 우리가 하는 것은 application 프로그래밍이 아닌 system 프로그래밍이다.
- cover하는 영역이 매우 넓음.

<br>

이번 학기에는 주로 **OS**에 대해 배울 것임

**OS가 제공하는 api**를 사용(프로그램 작성)해봄으로써 os 커널 구조를 이해하는 수업이 될 것임

- 주로 Linux 사용 (우분투,Ubuntu)

<br>

**System software**

- software designed to provide <mark>a platform</mark> **to other software**

- 다른 프로그램을 실행시키기 위해서 도움을 주는 프로그램
- system sorftware의 사용자는 다른 software 
- 이를 삭제했을 떄 다른 소프트웨어에 영향을 끼친다.
  - 게임 프로그램은 삭제해도 다른 소프트웨어에 영향을 끼치지 않기 때문에 시스템 소프트웨어가 아님
- 다른 소프트웨어에게 서비스를 제공하기 위한 소프트웨어



<BR>
**시스템 소프트웨어의 예시**

- OS kernals
- Drivers
- Bare Metal Hypervisors(가상화 tool)
- Compilers (that produce native binaries) and Debuggers



**시스템 소프트웨어가 아닌**

- GUI chat application(Slack, Discord, etc)
- Web-based JavaSript Application
- Web Service API



<br>
**시스템 프로그래밍이 어려운 이유**

하드웨어 computer architecture를 잘 알아야 함

OS도 잘 알아야 함.

시스템 소프트웨어는 OS의 기능을 잘 활용해야 하기 때문에 (하드웨어를 제어하는 것이 포함되어 있음)

다른 소프트웨어에게 서비스를 제공하는 platform이기 때문에 performance가 상당히 중요하여 programming하기가 굉장히 어렵다.

<br>

![image](https://user-images.githubusercontent.com/79521972/156678994-c8c1e807-25db-4dc8-abdd-c812a161a1a4.png)

가운데 두 개가 CPU

CPU에서 main memory와 정보를 주고 받기 위한 bus(데이터 전송 채널)가 두 개 있음

- Data Bus
- Address Bus

CPU의 핵심 장치는 Program Control Unit(능동적인 결정), ALU는 slave(시키는 일만 함)

instruction에 의해서 프로그램이 실행되는데 그거는 우리가 짜는 알고리즘에 의해서 만들어 지는 것이다.





![image](https://user-images.githubusercontent.com/79521972/156679532-567e5009-37cf-438d-907c-1cf3d1620fed.png)

word단위로 읽어올 수도 있음.

r/w : read or write 

읽어오는 경우 data bus가 양방향이기 떄문에 main memory에서 데이터 값을 읽어온다.

<br>

Cache : cpu와 main memory 사이에 설치해 놓은 memery 

- 사용 목적은 c와 m의 speed gap을 보완 하기 위한 용도 
- cpu의 속도를 m.m이 따라가지를 못함(m.m은 용량은 굉장히 늘어나긴 했지만)
- 이 격차가 계속 벌어지기 때문에 심각한 문제 발생(성능 측면)
- 폰노이만 구조에서 격차가 크면 성능 저하가 발생한다.





<br>

![image](https://user-images.githubusercontent.com/79521972/156679975-c496397d-ab56-474b-b133-93b2702ccba4.png)

1. main memory에 프로그램과 데이터가 깔려 있어야 함

2. 메모리의 주소는 바이트 단위로 되어있다.(address bus)

   - 메모리의 내용은 addressable 되어야함.

   - address는 16라인이 공급됨(CPU에서 만들어짐)
     - CPU가 만드는 방법: 
     - 어셈블리어로 만든 코드가 있다고 가정, 이를 기계어로 만들면 맨 첫 줄이 0번지이다.
     - 모든 프로그램이 0번지 부터 시작되게 되면 충돌이 발생(멀티 불가능)
   - A15,A14, ... , A1, A0
   - 이 address bus의 데이터가 모두 0이면 0번지를 의미, A0만 1이면 1번지를 의미

3. 어떤 프로그램의 실행은 sequential fashion으로 발생한다. (loop나 selection이 없는 경우, 즉 unless explicitly modified)
   - if-else문의 경우 false일때 특정 블록을 건너뛰는 것이 selection



<br>

하나의 instruction이 실행되기 위해서 다음 과정이 실행된다.

![image](https://user-images.githubusercontent.com/79521972/156681714-e2f0f8ce-8a61-4b8d-a792-7598bd9319d8.png)

읽어온다음에 실행(ALU에 의해)다시 돌아오고 이 loop가 반복

interrupt cycle: 방해가 되었다는 뜻인데 무엇이 방해가 된 걸까?

- CPU에 INT라는 핀이 있는데 
- aschronous 처리
- 외부에서 아무떄나 interrupt를 걸 수 있지만 CPU는 INT핀이 active high로 떠도 첫 번째 instruction때 interrput는 무시하다가 두 번째 instruction를 실행하기 직전에 잠깐 본다. interrupt가 없다면 fetch에 의해 다시 실행.

즉, 하나의 instruction이 실행시에는 어떠한 방해도 받지않는다.



<br>

![image](https://user-images.githubusercontent.com/79521972/156682232-bc179e49-9537-40df-8d79-e5a1c444e568.png)

- CPU register
- cache
- m.m



CPU register는 CPU 칩안에 존재(고속 메모리)

- 용도:?(1:31)

<br>

m.m는 메모리 칩에 존재, cache는 mm과 cpu사이 

<br>



Instruction format:2개의 s, 1개의 D, 4비트 Op code

![image](https://user-images.githubusercontent.com/79521972/156682466-92c9d5ab-813a-4639-8d3a-1aad300d793d.png)



data operation은 ALU 가

그 앞단계들은 control unit

<br>

<br>

### Run Time Environment(RTE)

![image](https://user-images.githubusercontent.com/79521972/156683532-f1245564-6660-4c6c-943f-cbb8b982e09f.png)

RTL의 example은 OS임. 

모든 응용프로그램은 OS에서 실행됨

### OS

![image](https://user-images.githubusercontent.com/79521972/156683812-6b0da35d-739d-4129-904d-e2eda5627e74.png)





![image](https://user-images.githubusercontent.com/79521972/156683873-216a1f63-99f5-4df8-b9e1-14a46f53f0d4.png)

보라색: OS kernal (핵심 부분)

<br>

**OS가 추구하는 목표**

1. Efficiency: 하드웨어 자원을 효율적으로 사용(대표적으로 CPU)
   - CPU는 IO를 직접 실행시키지 않고 IO가 실행될 때는 놀고 있기 때문에 이 시간을 이용하여 다른 것 실행
2. Convenience: 
   - OS가 제공하는 API: System calls
   - 시스템 콜을 사용하기 쉽지 않아서 나온 것이 User interface
   - 



**Process VMs**



**Process Life Cycle**

<br>

![image](https://user-images.githubusercontent.com/79521972/156684990-28659c18-7f1d-4f36-b340-5d251380b587.png)

user interface에 해당하는 것이 libarary function이고 이것이 system calls을 대신 호출해 준다.

<br>

user mode / kernal mode

CPU가 프로그램을 실행할 때 mode가 두 개가 있다. 

user mode: 사용자 프로그램에서 실행되는, 자기한테 할당된 영역만 r/w

kernal mode: 모든 메모리의 영역을 r/w 할 수 있음

두 모드는 권한의 차이인 것임

<br>

바로 kernal mode로 들어가기엔 부담이 있기 때문에 user library가 system call을 대신 호출해 준다.

system call을 호출할 때는 변수가 stack에 생기지 못하기 때문에 부담이 있는 것. 



Ms word 아이콘을 두 번 누르면 창이 두 개가뜸 -> 프로세스가 두개가 실행 된 것

instance두 개가 만들어진 것

<br>

CPU register에서 값을 copy하고 mode를 change 해줘야 하는데 이를 trap을 이용











































































