---
layout: single
title: "[Computer Architecture] week 1-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



### Review)

- performance(speed):clock frequency

- area(cost): 

  - 이는 design에서 줄이는 part가 있고 공정에서 줄이는 part가 있는데 데이터가 많이 들어간다고 해서 문제가 되지 않는다.

  - 더 중요한 문제는 fab, 즉 어떤 공정을 쓰느냐

- power(energy) = vdd^2에 비례, f와 비례, C(cost)와도 비례



<br>

- **perf**(speed) = 1 / exec_time

  - latency(exec_time)라고도 하고
  - throughput라고도 하지만
  - 당분간은 exec_time으로 정의를 할 것이다.

  - exec_time(Time to execute a program) = sec/prog (프로그램당 몇초가 걸리는가)



- Exec_TIme(sec/prog) = (# of instruction / prog ) * (# of clocks/inst) * (sec/clock)
  - 위 식을 약분하여 sec/prog가 나온다.
  - = 1 / clock frequency(얼마나 빠른 clock을 쓸 수 있는가)
  - 1. 프로그램당 얼마나 많은 instruction을 포함하는가?
    2. instruction 당 몇 개의 clocks을 지나는가?(CPI, Clocks Per Instruction)
       	- average 개념?
       	- (avg) CPI
    3. 한 클락당 몇 초인가?
  - 따라서
    - Exec_time = 1번 * 2번 * 3번



- avg.CPI?
  - instruction이 add를 수행하는데는 1clk이 소모되고 multiply를 수행하는데는 10clk이 사용된다하면 이 CPU의 CPI는 무엇인가? 라고 했을 때 대답하기 애매하기 떄문에 이를 평규내서 말하는 것이다.
  - IPC = 1/CPI
  - 요즘은 한 클락안에  parallel로 여러 instuction을 수행하기 때문에 IPC를 고려하는 경우도 많아지지만 우리 수업에서는 CPI를 고려하도록 할 것이다.







MIPS(RISC) 같은 것은 instruction수가 많고(단순한 명령어로 수행하기 때문에), 

x86(CISC) 같은 것은 instruction수가 적은데(복잡한 명령어로 수행하는데) 둘 중 instruction수가 많은 RISC가 무조건 더 비효율적인가? -> 아니다.

오히려 간단한 명령어로 구성된 RISC가 더 빠른 성능을 보일 수 있다.



# 교재

## Response Time and Troughput

- Response time(당분간은 이것으로 생각한다.)





## Relative Performance

- define Performance = 1/Execution Time





## Measuring Execution Time

- Elapsed time
  - Total response time, including all aspects
    - Processing, I/O, OS overhead, idle time
  - Determines system performance
- CPU time
  - Time spent processing a given job
    - Discounts I/O time, other job's shares
  - Comprises <span style="color:red">user CPU time</span> and system CPU time
  - Different programs are affected differently by CPU and system system perfromance



## CPU Clocking

- Operation of digital hardware governed by a constant-rate clock



- Clock period(clock cycle time): duration of a cycle 
  - e.g., 250ps = 0.25ns = 250x10^-12^s
- Clock frequency (rate) : cycles per second
  - e.g, 4.00GHz





## CPU Time

![image](https://user-images.githubusercontent.com/79521972/158300858-290102a8-594b-430c-b085-21b77fd908fb.png)

- Performance improved by
  - Reducing number of clock cycles
  - Increasing clock rate
  - Hardware designer must often trade off clock rate against cycle count
  - 







## Instruction Count and CPI

![image](https://user-images.githubusercontent.com/79521972/158301278-236494f8-8c3e-4c6b-852a-c9e39c60114a.png)

- instruction Count for a program
  - Determined by program(programming language), ISA(CISC or RISC) and compiler
- Average cycles per instruction (CPI)
  - Determined by CPU hardware
  - If different instructions have different CPI
    - Average CPI affected by instruction mix

Why average?











## Performance Summary(최종)

perf = 1/CPU_Time

![image-20220315124126288](C:\Users\c_dragon\AppData\Roaming\Typora\typora-user-images\image-20220315124126288.png)

CPU Time = Instructions term x CPI x clock cycle time(T~C~)

- Performance depends on 
  - Algorithm: affects IC, possibly CPI
  - Programming language: affects IC, CPI
  - Compiler: affects IC, CPI
  - instruction set architecture: affects IC, CPI, T~c~, 세가지 전부에 영향을 준다.(그 만큼 중요)
  - Semiconductor
    - 반도체 기술도 T~c~와 CPI에 영향을 줄 수 있다.



## Pitfall: MIPS as a Performance Metric

- MIPS: Milion of instructions per Second
- Doesn't accout for 
  - Differences in ISA between computers
  - Differences in complexto





## Power Trends

그림



- clock rate이 더 이상 오르지 않는 이유는 이를 계속 올리다 보면 f는 power에 비례하기 때문에 power가 너무 커져 열이 발생할 수 있기 때문이다.

- How could clock rate grow by a factor of 1000 while power grew by only a factor of 30?
- In CMOS IC technology(Dynamic power)

- ==Power = Capacitive load x Voltage^2^ x Frequency==
  - power는 30배 증가하고 frequency는 1000배가 늘었지만 제곱하여 비례하는 voltage의 경우 5V에서 1V로 줄었기 때문에 clock rate에 비해 power가 크게 늘어나지 못했다.





## Reducing Power

- Suppose a new CPU has
  - 85% of capacitive load of old CPU
  - 15% voltage and 15% frequency reduction



- The power wall
  - We can't reduce voltage further
  - We can't remove more that
- How else can we improve performance?
  - Multi core로 해결할 수 있다!



### Uniprocessor Performance

그림

power 때문에 많이 늘어나지 못함

하지만 게임, 프로그램 등으로 성능 향상은 반드시 이루어 져야 한다.



### Multiprocessors

- Multicore microprocessors
  - More than one processor per chip
- Requires explicitly parallel programming
  - Compare with instruction level parallelism
    - Hardware executes multiple instructions at once
    - Hidden from the programmer
  - Hard to do
    - Programming for performance
    - Load balancing
    - Optimizing communication and synchronization



## Amdahl's Law

- Improving an aspect of a computer and expecting proportional improvement in overall performance

![image](https://user-images.githubusercontent.com/79521972/158305000-51d5727e-9885-4290-acdc-2b7f123c8e40.png)

T~affected~ : parallelize 가능한 것

T~unaffected~ : parallelize 불가능한 것

- Example: multiply accounts for 80s/100s
  - How much improvement in multiply performance to get 5x overall?





- Corollary: make the common case fast!

















































