---
layout: single
title: "[Computer Architecture] System Veriolog"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



# Chapter 4

## Computer systems Abstraction

![image](https://user-images.githubusercontent.com/79521972/161673342-499c3aac-93a2-4210-8c59-95d18345ed8e.png)



- HDL을 할 때는 RTL level에서 함

- RTL level: register에 담아놨다가 사용

- HDL -> gate level : synthesis



![image](https://user-images.githubusercontent.com/79521972/161673627-d11b3b14-434a-451f-b566-a31f4c79547b.png)

synthesizable한 것에 대해서 simulation.

전체 feature는 simulation을 위한 것.



우리가 관심을 가질 내용은 processor를 설계하는 것.(modeling simulation)



<br>



## HDL to Gates

- Simulation
  - Inputs applied to circuit
  - Outputs checked for correctness
  - Millions of dollars saved by debugging in simulation instead of hardware



- Synthesis
  - Transforms HDL code







## Blocking vs. Nonblocking Assignment

- <= is nonblocking assignment
  - Occurs simultaneously with others

- = is blocking assignment
  - Occurs in order it appears in file













































