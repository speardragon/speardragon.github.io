---
layout: single
title: "[Computer Architecture] I/O"
categories: ['Computer Science', 'Computer Architecture']
tag: ['I/O']
---

<br>

https://com24everyday.tistory.com/173

IO는 0번지가 없음



direct memory access(DMA)

# Chapter 6. Storage and Other I/O Topics

## Introduction

CPU랑 Memory가 아닌 것은 전부 I/O이다.

- I/O devices can be characterized by 
  - Behaviour: input, output, storage 
  - Partner: human or machine 
  - Data rate: bytes/sec, transfers/sec 
- I/O bus connections

![image](https://user-images.githubusercontent.com/79521972/171084746-56a2b456-f13d-4c86-b0e4-6121c4fb581c.png)



<br>

## I/O System Characteristics

- **Dependability** (reliability신뢰성)is important 
  - disk가 날라가면 큰일나기 때문에 speed 도 중요하지만 dependability가 매우 종요한 것
  - Particularly for storage devices 
- **Performance** measures 
  - Latency (response time) 
  - Throughput (bandwidth) 
  - Desktops & embedded systems 
    - Mainly interested in response time & diversity of  devices 
  - Servers 
    - Mainly interested in throughput & expandability of  devices



<br>

## Dependability

system이 얼마나 안전하냐

![image](https://user-images.githubusercontent.com/79521972/171087780-a64dbcf1-426a-41c7-afa7-c8401a8f540d.png)

- Fault: failure of a component
  - May or may not lead to system failure



<br>

## Dependability Measures

- Reliability – measured by the mean time to failure (MTTF). Service  interruption is measured by mean time to repair (MTTR) 

- Availability – a measure of service accomplishment Availability = MTTF/(MTTF + MTTR) 

- To increase MTTF, either improve the quality of the components or  design the system to continue operating in the presence of faulty  components 

- 1. Fault avoidance: preventing fault occurrence by construction 

- 2. Fault tolerance: using redundancy to correct or bypass faulty  components (hardware) 

     - l Fault detection versus fault correction

     - l Permanent faults versus transient faults















