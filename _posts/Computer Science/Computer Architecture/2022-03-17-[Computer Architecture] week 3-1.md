---
layout: single
title: "[Computer Architecture] week 1-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Chapter 6





### Introduction

- Jumping up a few levels of abstraction
- Architecture:
  - pro



### Assembly Language

- 기계가 알아먹는 명령어를 1:1로 symbolic하게 표현
- instruction



### RISC-V





(핸드폰 사진)

MIPS에는 instruction set이 약 4~50개가 있음. 이 중에서 핵심적인 기능만 수행할 수 있는 것을 어떻게 만들까?



<br>

[참고](https://uweb.engr.arizona.edu/~ece369/Resources/spim/MIPSReference.pdf)

instruction set의 종류(기본,핵심,필수 세 기능)

1. Arithmetic/Logic comp. (add, addi,  sub, subi, and, or, ...)

2. data move(memory-to-reg, reg-to-memory, lw, sw,...)

3. flow control (conditional branch(if), jump, call/return)

4. extra.....



<br>

```assembly
add rd, rs, rt # rs + rt ->rd
addi rd, rs, Const # rd:register destination, rs:register source

lw, rd, Mem_adr
sw, rs, Mem_adr 

beq, $1, $2, Label # if $1 == $2, Label로 옮겨라 (branch equal)
bne, $1, $2, Label

J Label
Jal Function # function으로 갔다가 다시 돌아와라(call)
```

모든 명령어들은 32-bit로 구성되어 있음.





### 메모리 위치를 지정하는 방법

















