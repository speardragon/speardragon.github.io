---
layout: single
title: "[Computer Architecture] 03.15 performance와 CPU time"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Performance', 'CPU time', 'Latency']
---



### Review)

- performance(speed):clock frequency

- area(cost): 

  - 이는 design에서 줄이는 part가 있고 공정(fab)에서 줄이는 part가 있는데 데이터가 많이 들어간다고 해서 문제가 되지 않는다.

  - 더 중요한 문제는 fab, 즉 어떤 공정을 쓰느냐

- power(energy) = power ∝ V<sub>dd</sub><sup>2</sup> * f * C<sub>L</sub>

위 3가지의 tradeoff를 잘 고려하여야 함

<br>

- **perf**(speed) = 1 / exec_time
- latency(exec_time)라고도 하고
  - throughput라고도 하지만
  - 당분간은 exec_time으로 정의를 할 것이다.
  
- exec_time(Time to execute a program) = sec/prog (프로그램당 몇초가 걸리는가)

<br>

- Exec_Time(sec/prog) = (# of instruction / prog ) * (# of clocks/inst) * (sec/clock)
  - 위 식을 약분하여 sec/prog가 나온다.
  - sec/clock = 1 / clock frequency(얼마나 빠른 clock을 쓸 수 있는가)
  - 1. 프로그램당 얼마나 많은 instruction을 포함하는가?
    2. instruction 당 몇 개의 clocks을 지나는가?(CPI, Clocks Per Instruction)
       	- average 개념?
       	- (avg) CPI
    3. 한 클락당 몇 초인가?
  - 따라서
    - Exec_time = 1번 * 2번 * 3번

<br>

- avg.CPI?(Clock per insturction)
  - instruction이 add를 수행하는데는 1clk이 소모되고 multiply를 수행하는데는 10clk이 사용된다하면 이 CPU의 CPI는 무엇인가? 라고 했을 때 대답하기 애매하기 떄문에 이를 평규내서 말하는 것이다.
  - IPC = 1/CPI
  - 요즘은 한 클락안에  parallel로 여러 instuction을 수행하기 때문에 IPC를 고려하는 경우도 많아지지만 우리 수업에서는 CPI를 고려하도록 할 것이다.



<br>

MIPS(RISC) 같은 것은 instruction수가 많고(단순한 명령어로 수행하기 때문에), 

x86(CISC) 같은 것은 instruction수가 적은데(복잡한 명령어로 수행하는데) 둘 중 instruction수가 많은 RISC가 무조건 더 비효율적인가? -> 아니다.

오히려 간단한 명령어로 구성된 RISC가 더 빠른 성능을 보일 수 있다.

<br>

clock frequency를 결정하는 것은 중간의 combinational logic인데 이것이 CISC는 복잡하게 되어 있기 때문에 짧은 clock을 쓰는 것이 어려울 수도 있는 것이다.



---

# 교재

## Response Time and Troughput

- Response time(당분간은 이것으로 생각한다.)
  - How long it takes to do a task

- Throughput
  - Total work done per unit time
    - e.g., tasks/transactions/... per hour

- How are response time and throughput affected by
  - Replacing the processor with a faster version?
  - Adding more processors?

- We'll **focus on response time** for now...

 

<br>

## Relative Performance

- define Performance = 1/Execution Time

- "X is *n* time faster than Y"

  - Performance~X~/Performance~Y~ = Execution time~Y~/Execution time~X~ = *n*

- Example: time taken to run a program

  - 10s on A, 15s on B
  - Execution Time~B~/Execution Time~A~= 15s / 10s = 1.5

  - So A is 1.5 times faster than B





## Measuring Execution Time

- Elapsed time
  - Total response time, including **all aspects**
    - Processing, I/O, OS overhead, idle time
  - Determines system performance
  - 모든 것을 고려한 시간
- CPU time
  - Time spent processing a given job
    - Discounts I/O time, other job's shares
  - Comprises <span style="color:red">user CPU time</span> and system CPU time
  - Different programs are affected differently by CPU and system system perfromance



## CPU Clocking

- Operation of digital hardware governed by a constant-rate clock

![image](https://user-images.githubusercontent.com/79521972/158715602-0fd0f678-0377-4cd1-a38d-ddcf88239488.png)

- **Clock period(clock cycle time)**: duration of a cycle 
  - e.g., 250ps = 0.25ns = 250x10^-12^s
- **Clock frequency (rate)** : cycles per second
  - e.g, 4.00GHz = 4000MHz = 4.0 x 10^9^Hz



<br>

## CPU Time

CPU를 설계할 것이기 때문에 이 CPU만 사용하는 데 걸리는 시간이 중요함.

![image](https://user-images.githubusercontent.com/79521972/158300858-290102a8-594b-430c-b085-21b77fd908fb.png)

- Performance improved by(성능 향상을 위해선)
  - Reducing number of clock cycles
  - Increasing clock rate (combinational logic으로 조절)
  - Hardware designer must often trade off clock rate against cycle count



<br>

## Instruction Count and CPI

![image](https://user-images.githubusercontent.com/79521972/158301278-236494f8-8c3e-4c6b-852a-c9e39c60114a.png)

- instruction Count for a program
  - Determined by`program(programming language)`, `ISA(CISC or RISC)` and `compiler`
- Clock Rate = 1/Clock Cycle Time
- Average cycles per instruction (CPI)
  - Determined by CPU hardware
  - If different instructions have different CPI
    - **Average CPI** affected by instruction mix

#### CPI in More Detail

- If different instruction classes take different numbers of cycles

![image](https://user-images.githubusercontent.com/79521972/158716885-7be5a851-9b13-476e-b047-7566c2df738f.png)

- Weighted average CPI

![image](https://user-images.githubusercontent.com/79521972/158716918-0bdc614f-be3d-4c0b-8cb5-553b0ec68afb.png)

<br>

<mark>이 부분 작년 시험에 나왔음.</mark>

- CPU Time 의 각 term(3 가지) 에 대해 설명을 하고 각 term이 무엇에 dependant한지

<br>



## Performance Summary

cf) perf = 1/CPU_Time

![image](https://user-images.githubusercontent.com/79521972/158715771-4d21ac98-ef24-447e-90c6-543b6807d1a4.png)

CPU Time = Instructions term x CPI x clock cycle time(T<sub>c</sub>)

- Performance depends on 
  - **Algorithm**: affects IC(Instruction Count), possibly CPI
  - **Programming language**: affects IC, CPI
  - **Compiler**: affects IC, CPI
  - **instruction set architecture**: affects IC, CPI, T<sub>c</sub>, 세가지 전부에 영향을 준다.(그 만큼 중요)
  - **Semiconductor**
    - 반도체 기술도 T<sub>c</sub>에 영향을 주고 CPI에도 영향을 줄 수 있다.

<br>

## Pitfall(함정): MIPS as a Performance Metric

- MIPS: Milion of instructions per Second
- Doesn't accout for 
  - Differences in ISA between computers
  - Differences in complexty between instructions

![image](https://user-images.githubusercontent.com/79521972/158718908-c5f905b4-7977-45ae-abe5-346aee1aca4e.png)

- CPI varies between programs on a given CPU

- 50 MIPS vs. 100MIPS -> 어떤 명령어(복잡도)를 실행하느냐에 따라 뭐가 더 좋은지가 달라짐
  - 그래서 MIPS만 가지고 성능을 논하기는 힘들다.

<br>

## Power Trends

![image](https://user-images.githubusercontent.com/79521972/158719232-92a0191d-fd0d-4ed2-a19f-5a2441b06bb5.png)



- clock rate이 더 이상 오르지 않는 이유는 이를 올릴 수는 있는데 계속 올리다 보면 f는 power에 비례하기 때문에 power가 너무 커져 감당할 수 없느 열이 발생할 수 있기 때문이다.
- How could clock rate grow by a factor of 1000 while power grew by only a factor of 30?
- In CMOS IC technology(Dynamic power)
- ==Power = Capacitive load x Voltage<sup>2</sup> x Frequency==
  - power는 30배 증가하고 frequency는 1000배가 늘었지만 제곱하여 비례하는 voltage의 경우 5V에서 1V로 줄었기 때문에 clock rate에 비해 power가 크게 늘어나지 않은 것이다.

- clock frequency(f)를 늘리고 싶은데 power때문에 못늘리고 있는 상황
  - 그러면 Vdd를 줄이면 되지 않나? -> 이미 5V에서 0.8V까지 줄어든 상황, 거의 한계이다.
  - 그래서 지금 현재 power는 f와 직결되어 있다고 봐도 무방하다.



## Reducing Power

- Suppose a new CPU has
  - 85% of capacitive load of old CPU
  - 15% voltage and 15% frequency reduction

![image](https://user-images.githubusercontent.com/79521972/158719818-86fea2a8-6d7a-47b1-b3fc-38ef54111a52.png)

- **The power wall**
  - We can't reduce voltage further
  - We can't remove more that
- ==How else can we improve performance?==
  - Multicore로 해결할 수 있다!



### Uniprocessor Performance

![image](https://user-images.githubusercontent.com/79521972/158720138-7414657a-1ed6-47da-ada0-6b51130741de.png)

- growth limited by **power**, instruction-level parallelism, long memory latency
  - power 때문에 많이 늘어나지 못함

- 하지만 게임, 프로그램 등으로 성능 향상은 반드시 이루어 져야 한다.
  - 그래서 multicore가 나타나기 시작

<br>

### Multiprocessors

- 프로세서가 여러개

- Multicore microprocessors
  - More than one processor per chip
- multicore라고 해도 한 processor가 실행될 때 나머지 processors는 놀고 있다. 그래서,
- Requires explicitly <span style="color:red">parallel programming</span>
  - Compare with instruction level parallelism
    - Hardware executes multiple instructions at once
    - Hidden from the programmer
  - Hard to do
    - Programming for performance
    - Load balancing
    - Optimizing communication and synchronization

<br>

## Amdahl's Law

어떤 100초가 걸리는 프로그램이 있다. 이 중 80%는 parallel process를 할 수 있고 20%는 못한다. 근데 10초만에 끝내고 싶다고 해도 parallel processing이 되지 않는 20% 때문에 불가능할 것이다. 때문에 이 20%가 중요한 것이다. 이를 수식으로 표현하면 다음과 같다.

-  Improving an aspect of a computer and expecting proportional improvement in overall performance

![image](https://user-images.githubusercontent.com/79521972/158305000-51d5727e-9885-4290-acdc-2b7f123c8e40.png)

T<sub>affected</sub> : parallelize 가능한 것(parallelizable)   -> 1-f

T<sub>unaffected</sub> : parallelize 불가능한 것(unparallelizable)  -> f

n: 프로세서 개수

- T<sub>n</sub> = 1-f/n + f

- Example: multiply accounts for 80s/100s
  - How much improvement in multiply performance to get 5x overall?

![image](https://user-images.githubusercontent.com/79521972/158721884-27213f48-a119-4e9d-b088-ee578cb7ca2c.png)

- Corollary: make the common case fast!

<br>

그래서 T<sub>unaffected</sub>가 굉장히 중요한 factor라는 것을 나타낸 것이 Amdahl's Law이다.

- T<sub>unaffected</sub>가 bottleneck















































