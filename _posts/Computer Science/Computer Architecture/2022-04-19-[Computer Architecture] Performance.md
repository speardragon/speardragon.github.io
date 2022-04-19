---
layout: single
title: "[Computer Architecture] Multi-cycle Performance"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Peroformance']
---



<br>

## Multicycle Processor Performance

- Instructions take different number of cycles
  - 3 cycles: beq, j
  - 4 cycles: R-Type, sw, addi
  - 5 cycles: lw
- CPI is weighted average
- SPECINT2000 benchmark:
  - 25% loads 
  - 10% stores 
  - 11% branches 
  - 2% jumps 
  - 52% R-type

**Average CPI** = (0.11 + 0.02)(3) + (0.52 + 0.10)(4) + (0.25)(5) = 4.12 (?)



<br>

Multicycle critical path: **Tc** = t<sub>pcq_ALUout</sub> + t<sub>mux </sub>+ max(t<sub>ALU</sub> + t<sub>mux</sub>, t<sub>mem</sub>) + t<sub>setup</sub>

- write 는 read보다 조금 걸리고 
- register file은 memory보다 조금 걸린다.(빠르다)

![image](https://user-images.githubusercontent.com/79521972/163916051-6231ccba-5a19-4015-8045-957effb91092.png)



<br>

## Multicycle  Performance Example

![image](https://user-images.githubusercontent.com/79521972/163916368-89f5b8ae-f402-4aae-840e-a70ea3fda7c7.png)

**Tc** = t<sub>pcq_ALUout</sub> + t<sub>mux </sub>+ max(t<sub>ALU</sub> + t<sub>mux</sub>, t<sub>mem</sub>) + t<sub>setup</sub>

 = Tc = t<sub>pcq_ALUout</sub> + t<sub>mux </sub>+ t<sub>mem</sub> + t<sub>setup</sub>

 =  [30 + 25 + 250 + 20] ps

 = 325 ps



<br>

## 중요한 의제(시험에 나옴)

For a program with 100 billion instructions executing on a multicycle  MIPS processor

- CPI = 4.12
- Tc = 325 ps



Execution Time 

= (# instructions) × CPI × Tc 

= (100 × 109 )(4.12)(325 × 10-12) 

= 133.9 seconds



This is slower than the single-cycle processor (92.5 seconds). Why?

1. 제일 많이 걸리는 게 clock cycle을 정하는데 clock을 정확히 이분하게 나누지 않았다.
   - Not even valenced
   - Not all steps same length

2. 자르다보면 다음 cycle에 계산하기 위해 저장하기 위한 register(f/f)가 중간중간에 들어가는데 register에서는 t<sub>pcq</sub>, t<sub>setup</sub> 이라는 overhead로 인해 clock이 정확하게 5배 빨라지지 못하는 것이다.

   - Sequencing overhead for each stop(t<sub>pcq</sub>+ t<sub>setup</sub> = 50 ps)

   - 그래서 CPI에서 손해본 것을 clock cylce에서 감당할 수 없었다.



Q) 어떻게 이분하게 쪼개는가

A) mips는 data memory 떄문에 unbalence가 발생

<br>

## Review: Single-Cycle Processor

![image-20220419125734382](C:\Users\c_dragon\AppData\Roaming\Typora\typora-user-images\image-20220419125734382.png)





## Review: Multicycle Processor



![image](https://user-images.githubusercontent.com/79521972/163917275-c1874daa-15e7-4cf3-ba73-958731324b8c.png)

- 장점 resource sharing(memory, ALU)



















