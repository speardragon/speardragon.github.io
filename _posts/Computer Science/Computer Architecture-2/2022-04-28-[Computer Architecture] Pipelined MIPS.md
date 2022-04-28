---
layout: single
title: "[Computer Architecture] Pipelined MIPS"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Peroformance']
---

<br>

# Exam Review)

Q) multi cycle에서 fastest cylce이 의미하는 것?

A) fastest clock cycle을 결정하는 것은 가장 longest path가 결정하기 때문에 이 path에 대한 clock cycle period를 구하는 것이다.

<br>

# Pipelined MIPS

Performance

- Latency (Execution Time)
- Throughput (Bandwidth)
  - \# of task/unit time



Example) ideal case

- total time = 100만 sec
- Latency = 1 sec
- Throughput = 1 inst/sec





total time = 1 sec + (0.2 x 999,999)

-  = 0.2 x 1M = 20만 sec

Latency = 1 sec

Throughput = 1/0.2 = 5(inst/sec)



여러개가 있을 때는 throughput을 봐야한다?



Example) non-ideal case

- latency = 0.4 x 5 = 20
- Total Time = 2.0 x 999,999 x 0.4
  - = 1M x 0.4 = 40만 sec
- Throughput = 1/0.4sec = 2.5





실행할 것이 여러개가 있으면 pipeline



<br>





























