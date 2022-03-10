---
layout: single
title: "[Computer Architecture] week 1-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Review)

![image](https://user-images.githubusercontent.com/79521972/157591408-f57299f3-b57e-459f-b18c-eb17239509b9.png)



data cache

b,c,a: data cache에 저장됨

program들(명령어들): instuction cache에 저장되어 있음

control decoder



교려해야할 중요한 세 가지

- power
- area(cost)
- opofb



perf. = 1 / exec_Time



**Execution Time**

1) Latency (response time)
2) Throughput
   - throughput(bandwidth) = # of task / time unit(hour, sec)





### case 1: 10초에 사람 한 명(clock 하나 당 실행)

latency = 10 sec

total exec_time = 10 x 100 = 1000 (sec)

throughput = 0.1 (task/sec)



### case 2: 10초에 사람 5 명(clock의 주기가 위 보다 짧음)

어느 순간(10초 이후)에는 동시에 작업이 일어남

latency = 10 sec

total exec time = 10 + (99 x 2) ≒ 208 (sec) ≒ 200 (sec)

throughput = 0.5 (task / sec)



칸막이는 그냥 나오지 않고 이를 만드는 시간이 조금 걸리긴 하나 명령어가 길때는 훨씬 더 효율적일 것이다.

case2보다 case1이 5배 만큼 높은 throughput(성능, performance)을 가짐 



<br>







<br>

## Levels of Program Code (from COD)

Compiler에 의해 language를 machine이 알아듣도록 계속 변환 시킴

<br>

- High-level language

  - Level of abstraction closer to problem domain

  - Provides for productivity and psrtabiltiy

  - ```c
    swap(int v[], int k)
    {
        int temp;
        temp = v[k];
        v[k] = v[k+1];
        v[k+1] = temp;
    }
    ```

- Assembly lanuage

  - Textual representation of instrcutions

  - ```assembly
    swap:
    	muli $2, $5.4
    	add $2, $4.$2
    	lw $15, 0($2)
    	lw $16, 4($2)
    	sw $16, 0($2)
    	sw $15, 4($2)
    	jr $31
    ```

- Hardware representation

  - Binary digits(bits)

  - Encoded instructions and data

  - ```
    00000000000000000000011010100000100000000000100110
    101000000000000001111111000000000001111111000001
    010100000000000111111101010010
    ```

    

## Inside the Processor(CPU) (from COD)

- Datapath: performs operations on data
- Control: sequences data path, memory
- Cache memory
  - small fast SRAM memory, for immediate access to data



- Appl A12 Bionic Processor



## A safe Place for Data (from COD)

- Volatile main **memory**
  - Loses instructions and data when power off
- Non-volatile **secondary memory(storage)**
  - Magnetic disk
  - Flash memory
  - Optical disk(CDROM, DVD)



## Semiconductor Technology

- Silicon: semiconductor
- Add materials to transform properties:
  - Conductors 
  - Instulators
  - Switch



<br>

## Manufacturing ICs

![image](https://user-images.githubusercontent.com/79521972/157595430-1c2f2c1a-03b2-488d-b029-2f55f1f6efc5.png)

반도체 공정

- fab
- design
- test



