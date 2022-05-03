---
layout: single
title: "[Computer Architecture] Pipelined MIPS"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Peroformance']
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

- latency = 0.4 x 5 = 20
- Total Time = 2.0 x 999,999 x 0.4
  - = 1M x 0.4 = 40만 sec
- Throughput = 1/0.4sec = 2.5
  - 따라서 Throughput이 stage 갯수만큼 높아질 것 같지만 (ideal) practical한 상황에서 linear하게 증가하지는 않는다.

<br>

많이 나누면 나눌 수록 좋은데 자기 마음대로 나눌 수 있는가? -> NO

back path가 존재하는 경우 돌아갔을 때 그 전의 clock이 끝날 때까지 기다려야 하므로 제한이 걸린다.

- 이렇듯 pipeline cylce이 진행하는 것을 저해하는 요소를 **hazard**라고 한다.

<br>

# Pipelined MIPS

어떤 한 순간에 봤을 때 여러 개의 instruction을 실행하고 있음.

- single이나 multi는 한 순간에 무조건 한 개의 instruction을 실행해야 함.



---

Q) pipeline의 갯수보다 실행할 명령어가 더 적으면 비효율적

A) Yes, but practical case, instruction의 수는 수백만, 수천만개임.

Q) multi cycle과의 차이?

A) multi cylce은 5개로 나눠서 이 전체를 clock으로 사용하여 한 순간에 한 instruction만을 실행했는데, pipeline은 각 순간에 여러개의 다른 명령어가 실행되고 있음.

---

## Pipelined MIPS Processor

- Temporal parallelism
- Divide single-cycle processor into 5 stages:
  - Fetch (IM)
  - Decode (& Reg. read)
  - Execute (ALU)
  - Memory (DM)
  - Writeback (Reg.Write)
- Add pipeline registers between stages



## Single-Cylce vs. Pipelined

![image](https://user-images.githubusercontent.com/79521972/166343558-9b1fbc76-b85d-40cd-8ee6-5616bf17fd59.png)

<br>

## Pipelined Processor Abstraction

![image](https://user-images.githubusercontent.com/79521972/166343710-09d2f105-85cd-4176-b6d9-c5f813381083.png)



위 그림을 보면 알 수 있듯이 한 순간에 모든 방에서 instruction이 돌아가는 경우가 존재하기 때문에 memory의 share가 더 이상 불가능해 진다.

따라서 모든 hardware가 다 있어야 함.

<br>

## Single-Cycle & Pipelined Datapath

![image](https://user-images.githubusercontent.com/79521972/166343820-09862ccb-aee4-4fe4-9d30-1dfe570c25af.png)

clock에 inverter가 달려 있음 -> falling edge 에서 trigger(writing)





<br>

## Corrected Pipelined Datapath

![image](https://user-images.githubusercontent.com/79521972/166344254-9885a9f6-0e2b-4d99-a65b-6dd141277f46.png)

> WrtieReg must arrive at same time as Result





<br>

## Pipelined Processor with Control

![image](https://user-images.githubusercontent.com/79521972/166344372-28eb0d17-6a20-49f4-ad87-e02f127ce5ad.png)

- Same control unit as single-cylce processor
- Control delayed to proper pipeline stage

각 cylce에서 필요한 control을 끌고가서 register(clock)과 함께 사용한다.

위 그림에서 두 개의 hazards가 존재함.(data hazard, control hazard)

<br>

## Pipeline Hazards

- When an instruction depends on result from  instruction that hasn’t completed

- Types:
  - Data hazard: register value not yet written back to  register file
    - Read After Write를 지켜야 함.
    - falling edge에서 write
  - Control hazard: next instruction not decided yet  (caused by branches)
    - beq



<br>

## Data Hazard

![image](https://user-images.githubusercontent.com/79521972/166344936-fe91d292-4fb0-4dc6-b559-31e2bdf3c4df.png)



같은 clock 상에 있으면 데이터를 가져다 쓸 수 있는데 (5번 clock에서 가져다 쓴 것처럼) 위 그림처럼 c3와 c4에서 읽어야 하는데 c5에서 wrtie이 되기 때문에 가져다 쓸 수 없는 문제가 생긴다. 

그래서 명령어의 위치를 합리적인 곳에 넣을 수 있는지 컴파일러가 엄밀히 따져서 괜찮은 경우에 그곳에 끼우는데 (optimize) 이 때 다시 back 하는 path가 생기게 된다.

이를 data hazard라고 한다.

<br>

## Handling Data Hazards

- Insert **nops** in code at compile time 

- **Rearrange code** at compile time 

- Forward data at run time 

- Stall the processor at run time





<br>

## Compile-Time Hazard Elimination

- Insert enough nops for result to be ready
- Or move independent useful instructions forward

![image](https://user-images.githubusercontent.com/79521972/166345365-cf3e5a2a-0fe6-4dcd-8170-cbacc16dbbb0.png)

이처럼 nop(no operation) 을 끼워도 되고 전혀 상관 없는 두 명령어를 갖다가 끼워도 된다.
<br>

## Data Fowarding(Bypassing)

![image](https://user-images.githubusercontent.com/79521972/166345412-54c23cb4-2b7d-4bee-97a6-1991da4b045e.png)



명령어를 실행하다 보면 s0(register)에다가 write하는데 이 계산된 결과가 나오게 되는 시점을 생각해 보면 3번 째 clock에서 이 값이 만들어진 것을 알 수 있다. (by ALU)

그니까 괜히 이 값이 register에 저장될 때까지 기다리지말고 중간에 데이터가 만들어지면 바로 가져다 쓰자라는 아이디어.

![image](https://user-images.githubusercontent.com/79521972/166345993-0c819396-e801-4e63-b512-4982efcd9a44.png)

이러한 과정은 위 경우처럼 write clock 전에 read를 하려고 하는 경우에만 처리를 하는 것이다.

ALU에 들어갈 연산자를 어디서 가져올 것인지를 결정하기 위해 mux control로 결정한다.



어떤 경우에 이런 경우가 생길까?

- ```
  i1: add $4, $1,  $2 
  i2:         $4, $8 
  i3:         $8, $4 
  ```

- 이처럼 write 이전에 read를 하는 경우

- i2에서 $4는 앞에가는 명령어인  i1의 ALU 결과에 있을 것이므로 ALU로 부터 값을 가져오면 된다.

- i3에서는 $4는 i1이



<br>

## Data Forwarding

- Forward to Execute stage from either: 
  - Memory stage or 
  - Writeback stage

- Forwarding logic for ForwardAE:

```
if ((rsE != 0) AND (rsE == WriteRegM) AND RegWriteM) 
	then ForwardAE = 10
else if ((rsE != 0) AND (rsE == WriteRegW) AND RegWriteW) 
	then ForwardAE = 01
else ForwardAE = 00
```

> Forwarding logic for ForwardBE same, but replace rsE with rtE



- (E번방에 있는 읽고자 하는 source register가 앞에 가는 명령어의 write 할 register와 같으면) and (M번방에 있는 명령어가 register에 뭔가를 write할 때)
  - ForwardAE = 10
- (E번방에 있는 읽고자 하는 source register가 앞에 앞에(두 번 앞에) 가는 W방에 명령어의 write 할 register와 같으면) and (W번방에 있는 명령어가 register에 뭔가를 write할 때)





























