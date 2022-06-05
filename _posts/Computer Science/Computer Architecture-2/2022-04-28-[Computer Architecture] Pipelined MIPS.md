---
layout: single
title: "[Computer Architecture] Pipelined MIPS"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Pipeline']
---

<br>

# Exam Review)

Q) multi cycle에서 fastest cylce이 의미하는 것?

A) clock time을 결정할 때는 가장 짧은 clock을 사용하도록 해야 하고 fastest clock cycle time을 결정하는 것은 가장 longest path(critical path)가 결정하기 때문에 시험에서 이 path에 대한 clock cycle period를 구하는 것이다.

<br>

# Pipelined MIPS

Performance

- Latency (Execution Time)
- Throughput (Bandwidth) : 초(unit time)당 수행하는 일(task)
  - \# of task/unit time

<br>

**Example) ideal case**

- instruction 100만개가 있고, 한 instruction 당 1sec가 걸린다고 가정

  - total time = 100만 sec

  - Latency = 1 sec

  - Throughput = 1 inst/sec

<br>

- 0.2로 나눈 것(5개로 나뉨, multi)

  - total time = 1 sec + (0.2 x 999,999) = 0.2 x 1000000 = 20만 sec
  - Latency = 1 sec
    - 그대로 1초임

  - Throughput = 1/0.2 = 5(inst/sec)

<br>
그 동안은 instruction을 하나만 봤기 때문에 performance를 1/exec.Time 으로 생각했는데 실제 프로그램은 수많은 instruction들로 이루어져 있기 때문에 latency 하나만 가지고는 performance를 생각할 수 없다. 

따라서 instruction이 여러개가 있을 때는 throughput을 봐야한다.

- 이는 pipeline의 performance로 고려된다.

5개로 나눴을 때 throughput이 1에서 5가 된 것을 보면 많이 쪼갤 수록 throughput이 좋아질 텐데 이를 어디까지 쪼갤 수 있을까?

<br>

**Example) non-ideal case(practical case)**

0.3 + 0.4 + 0.3 + 0.3 + 0.2

과 같이 not even 하게 나누어 졌다고 생각해 보자. 그러면 가장 긴 시간이 걸리는 0.4에 해당하는 부분을 기준으로 clock cycle time이 결정될 것이다.(즉, 모든 clock이 0.4s임)

register 마다의 overhead(t<sub>pcq</sub>, t<sub>setup</sub>)를 고려해야 할 것임

- latency = 0.4 x 5 = 2.0
- Total Time = 2.0 + 999,999 x 0.4
  - = 1M x 0.4 = 40만 sec
- Throughput = 1/0.4sec = 2.5
  - 따라서 Throughput이 stage 갯수만큼 정비례하여 높아질 것 같지만 (ideal) practical한 상황에서는 linear하게 증가하지는 않는다.

<br>

많이 나누면 나눌 수록 좋은데 자기 마음대로 나눌 수 있는가? -> NO

back path가 존재하는 경우 돌아갔을 때 돌아간 path 그 이전의 clock이 끝날 때까지 기다려야 하므로 제한이 걸린다.

- 이렇듯 pipeline cylce이 진행하는 것을 저해하는 요소를 **hazard**라고 한다.

<br>

# Pipelined MIPS

어떤 한 순간에 봤을 때 여러 개의 instruction을 실행하고 있음.

- single이나 multi는 한 순간에 무조건 한 개의 instruction을 실행해야 함.



---

Q) pipeline의 갯수보다 실행할 명령어가 더 적으면 비효율적

A) Yes, but practical case에서 instruction의 수는 수백만, 수천만개임.

Q) multi cycle과의 차이?

A) multi cylce은 5개로 나눠서 이 전체를 clock으로 사용하여 한 순간에 한 instruction만을 실행했는데, pipeline은 각 순간에 여러개의 다른 명령어가 실행될 수 있고 반드시 한 instruction 당 5 cycle 동안 진행되어야 한다.

---

## Pipelined MIPS Processor

- Temporal parallelism
- Divide single-cycle processor into 5 stages:
  - Fetch (IM)
  - Decode (& RF. read)
  - Execute (ALU)
  - Memory (DM)
  - Writeback (Reg.Write)
- 위 다섯 과정을 나누면서 생기는 칸막이가 pipeline register인 것이다.
- 필요하든 필요하지 않든 위 다섯가지 방은 무조건 거쳐야 한다.
- Add pipeline registers between stages

<br>

## Single-Cylce vs. Pipelined

![image](https://user-images.githubusercontent.com/79521972/166343558-9b1fbc76-b85d-40cd-8ee6-5616bf17fd59.png)

<br>

## Pipelined Processor Abstraction

![image](https://user-images.githubusercontent.com/79521972/166343710-09d2f105-85cd-4176-b6d9-c5f813381083.png)

- hazard가 없는 경우

5 cylces 이후부터는 끝날 때까지 **한 clock 당 한 instruction**이 끝나기 때문에 hazard가 없는 경우의 CPI는 1이다.

위 그림을 보면 알 수 있듯이 어떤 한 순간에 모든 방에서 instruction이 돌아가는 경우가 존재하기 때문에 data memory와 instruction memory가 동시에 사용되는 경우가 있어 multi cycle 처럼 memory(Data memory와 Instruction Memory)의 share가 더 이상 불가능해진다.

따라서 single cycle처럼 각각의 모든 hardware resource가 다 있어야 함.

<br>

## Single-Cycle & Pipelined Datapath

![image](https://user-images.githubusercontent.com/79521972/166343820-09862ccb-aee4-4fe4-9d30-1dfe570c25af.png)

W방에 들어가면서 rising edge에서 write back 하여 register file에 data를 write 하려고 할 때 register file의 clk이 rising edge에서 동작하면 충돌이 일어나기 때문에 rising edge에서 writeback 하면 negative edge에서 register file에 data가 실제로 저장되도록 해야 한다.

<br>

![image](https://user-images.githubusercontent.com/79521972/166847895-43956f61-0711-492b-9e39-a5490de1a7ef.png)

위 명령이 진행되는 것을 보면 어느 특정 clock에서 하나의 Register에 대해서 Write와 Read가 동시에 일어나기도 한다. 

그러나 하나의 hardware에서 두 가지 동작을 동시에 할 수는 없기 때문에 write을 한 후에 read를 하기 위해서 write하는 동작이 synchrounous하게 동작하므로 이를 rising edge에서 동작하는 것이 아니라 falling edge에서 동작하게끔 하여 해결할 수 있다.

- 이에 대한 corrected path는 아래의 그림에 나와있음

- clock에 inverter가 달려 있음 -> falling edge 에서 trigger(writing)

<br>

![image](https://user-images.githubusercontent.com/79521972/166847909-e0985c13-008c-4e04-aae7-3c6735639520.png)

sw 명령의 경우 메모리에서 write과 read가 동시에 일어나는데 실제 memory write 명령이 Memory 단계에서 일어나는 것이 아니라 Writeback 단계에서 일어나는데(즉, next cycle) 해당 clock의 rising edge에서 write을 동작하게 하면 lw명령에서 memory를 read할 때는 asynch로 read하기 때문에 문제가 없다.

<br>

## Corrected Pipelined Datapath

- add $3, $4, $5

![image](https://user-images.githubusercontent.com/79521972/166344254-9885a9f6-0e2b-4d99-a65b-6dd141277f46.png)

> WriteReg must arrive at same time as Result

빨간색으로 표시된 path의 의미는 add와 같은 R-type 명령의 경우 register에 값을 저장하려면 해당 register를 언제 RF에 저장할 지가 관건인데 이를 저 빨간 path 처럼 저장할 register의 주소를 writeback 방까지 같이 끌고 가도록 한 것이다.

- 즉 register에 값을 저장하고자 하면 그 **저장을 할 값**과 **저장을 할 곳**의 register가 **같이** 이동하여 그 다음 명령어에서 들어온 register 값으로 대치되는 문제를 해결할 수 있도록 하는 것이다.

<br>

## Pipelined Processor with Control

![image](https://user-images.githubusercontent.com/79521972/166344372-28eb0d17-6a20-49f4-ad87-e02f127ce5ad.png)

- Same control unit as single-cycle processor
  - single cycle과 동일한 control unit 사용

- Control delayed to proper pipeline stage
- 현재 clock에서 사용 될 control signal은 HW로 들어가고 다음 step에서 사용될 control signal은 그냥 연결된 path를 통해 그대로 다음 방으로 전달되게 한다.

각 cylce에서 필요한 control을 끌고가서 register(clock)과 함께 사용한다.

위 그림에서 두 개의 hazards가 존재함.(data hazard, control hazard)

- writeback (Reg.Write) - data hazard
- beq - control hazard

<br>

## Pipeline Hazards

- When an instruction depends on result from instruction that hasn’t completed

- Types:
  - **Data hazard**: register value not yet written back to register file
    - Read After Write(RAW)를 지켜야 함.
    - falling edge에서 write
  - **Control hazard**: next instruction not decided yet  (caused by branches)
    - beq
  
  - How to solve? or avoid?

<br>

## Data Hazard

그림에 rising edge와 falling edge가 표현되어 있음 - falling edge가 Reg.read

![image](https://user-images.githubusercontent.com/79521972/166344936-fe91d292-4fb0-4dc6-b559-31e2bdf3c4df.png)



같은 clock 상에 있으면 앞에서 배웠던 것처럼 clock의 rising edge와 falling edge의 적절한 선택(falling에서 write  그 직후에 read)으로 데이터를 가져다 쓸 수 있었는데 (위 그림의 5번 clock에서 가져다 쓴 것처럼) 

위 그림처럼  c5에서 write 된 것을 c3와 c4에서 읽어야 하는 경우 만들어지지 않은 값을 가져다 써야 하는 문제가 생긴다. 

이를 data hazard라고 한다.

<br>

## Handling Data Hazards(단계적으로 시도)

- Insert **nops** in code at compile time 
  - 매우 비효율적 - clock 낭비

- **Rearrange code** at compile time 

- **Forward** data at run time 

- **Stall** the processor at run time



<br>

## Compile-Time Hazard Elimination

- Insert enough **nops** for result to be ready
- Or **move** **independent useful instructions forward**

![image](https://user-images.githubusercontent.com/79521972/166345365-cf3e5a2a-0fe6-4dcd-8170-cbacc16dbbb0.png)

이처럼 nop(no operation)을 끼워서 의미 없는 2 cycle을 보낸다. 그렇게 되면 반드시 write이 된 clock 이후에 read가 이루어지기 때문에 문제가 발생하지 않게 해준 것이다.

하지만 두 clock cycle의 낭비가 있다. 

그래서 이를 해결하기 위해서는 기존의 명령어를 끼워 넣는 방법도 있는데,

이는 중복되는 경우 dependency가 없는 명령어를 찾아서 순서를 조금 바꿔서 실행시키는 방법이다. 명령어의 위치를 합리적인 곳에 넣을 수 있는지 컴파일러가 엄밀히 따져서 괜찮은 경우에 그곳에 끼우는데 (optimize) 이 때 다시 back 하는 path가 생기게 되고 이를 hazard라고 한다.

<br>

## Data Fowarding(Bypassing)

![image](https://user-images.githubusercontent.com/79521972/166345412-54c23cb4-2b7d-4bee-97a6-1991da4b045e.png)

i1 명령어를 실행하다 보면 s0(register)에다가 write하는데 이 계산된 결과가 나오게 되는 시점을 생각해 보면 3번째 clock에서 이 값이 만들어진 것을 알 수 있다. (by ALU)

그니까 괜히 이 값이 register에 저장될 때까지 기다리지말고 굳이 register file에서 값을 가져오는 것이 아니라 중간에 데이터값이 만들어지면 바로 가져다 쓰자라는 아이디어이고 이를 통해 nop 없이도 문제를 해결할 수 있게 된다.

- 2 clock을 벌 수 있기 때문 - E방과 W방은 두 clock 차이

그래서 i2를 보면 $s0는 update되기 전의 원래 있던 $s0인데 얘를 register에 저장만 해 두고 실제로 ALU에 들어가는 $s0는 i1의 ALU를 통해 만들어진 값을 넣어주게 되는 것이다.

- 그래서 사실상 $s0 값이 들어가는 게 아니라 i1에서 ALU의 결과 값이 들어가는 것.

그 이후에 $s0는 Writeback 과정을 통해 새로운 값이 그제서야 update 된다.

<br>

## Data Forwarding to solve hazard

![image](https://user-images.githubusercontent.com/79521972/166345993-0c819396-e801-4e63-b512-4982efcd9a44.png)

이러한 과정은 위 경우처럼 write clock 전에 read를 하려고 하는 경우에만 처리를 하는 것이다.

따라서 hazard가 없는 경우는 그냥 데이터를 가져오는데 만약 hazard가 발생할 우려가 있는 것에 대해서는 ALU에 들어갈 연산자를 어디서 가져올 것인지를 결정하기 위해 <mark>mux control</mark>로 결정한다.

- 가져오는 연산자는 해당 clock보다 한 단계 뒤 혹은 두 단계 뒤에 있는 값을 가져오게 된다.

<br>

어떤 경우에 이런 경우가 생길까?

- ```
  i1: add $4, $1, $2 
  i2:         $4, $8 
  i3:         $8, $4 
  ```

- 이처럼 write 을 하기도 전에 read를 하는 경우(write이 되기 위해서는 5 clock이 걸리기 때문에)

- i2에서 $4를 읽는 방은 E방인데 **앞에** 가는 명령어인  i1의 ALU 결과에 있을 것이므로 M방으로 부터 값을 가져오면 된다.

- i3에서 $4는 **앞에 앞에** 가는 명령어인 i1에서 아직 register에 write이 되진 않았지만 그 전에 W방에서 뽑아올 수 있다.

---

Q) 그럼 $4가 write된 이후에는 어떻게 가져오는가?

A) register file에 저장이 된 것이기 때문에 그냥 register file에서 읽어올 수 있는 것이다.

<br>

## Data Forwarding

- Forward to Execute stage from either: 
  - **Memory stage** 
  - or 
  - **Writeback stage**
  
- Forwarding logic for ForwardAE:

```c
if ((rsE != 0) AND (rsE == WriteRegM) AND RegWriteM) 
	then 	ForwardAE = 10
else if ((rsE != 0) AND (rsE == WriteRegW) AND RegWriteW) 
	then 	ForwardAE = 01
else 		ForwardAE = 00  //no hazard
```

> Forwarding logic for ForwardBE same, but replace rsE with rtE

WriteRegX: X 방에서 write하려고 하는 Register (즉, add 명령의 경우 rd)

RegWriteX: X방에서의 RegWrite **control 신호** (<mark>각 방에서 계속 쭉 이어지고 있는 control signal</mark>)

<br>

- (E번방에서 읽고자 하는 source register(rs)가 하나 앞에 가는 명령어(M방)에서 write 할 register와 같으면) and (M번방에 있는 명령어가 register에 뭔가를 write할 때)
  - ForwardAE = 10
- (E번방에서 읽고자 하는 source register(rs)가 앞에 앞에(두 번 앞에) 가는 W방에 명령어에서 write 할 register와 같으면) and (W번방에 있는 명령어가 register에 뭔가를 write할 때)
  - ForwardAE = 01


---

Q) WriteReg는 어디서 알 수 있는가?

A) 그림에서 자세히 보면 rs, rt는 data path를 따라 W방까지 쭉 이어진다. 단지 방에 따라 뒤에 붙은 알파벳만 다를 뿐. 그래서 그 이어져 오던 rs와 현재 클락의 싱싱한 rs가 hazard unit에서 같은 register인지 비교되어, 만약 같다면 ForwardAE 신호가 결정되는 것이다.
