---
layout: single
title: "[Computer Architecture] MIPS instruction set"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Chapter 6

MIPS에 대한 내용이 주를 이룰 것



### Introduction

- Jumping up a few levels of abstraction
- Architecture:
  - programmer's(software) view of computer
    - Defined by instructions & operand locations
- Microarchitecture: 
  - how to implement an architecture in **hardware**(covered in Chapter 7)




<br>

### Assembly Language

- 기계가 알아먹는 기본적인 명령어(0, 1로 이루어진) 하나하나를 1:1로 symbolic하게 표현

- Instruction: commands in a computer's language

  - Assembly language: human-readable format of instructions

  - Machiune language: computer-readable format(1's and 0's)



### RISC-V

- MIPS와 매우 유사



<br>

### Review) 지난 주 강의 참조

![image](https://user-images.githubusercontent.com/79521972/159168100-42129ca9-017f-41fe-9fe0-ee90c40539ff.png)

<br>

```
int a, b, c
```

```
main
...
a = b + c
```

<br>

```assembly
load Mem(b) -> $2
load Mem(c) -> $3
add $2, $3 -> $4
store Mem(a) <- $4
```

<br>

- 기본적으로 필요한 IS
  - add
  - lode
  - store

- 하지만 b라는 데이터에 3이라는 숫자를 더하려면 어떤 명령어를 써야할까?

  - ```assembly
    addi sr1, cont, dr #source register1, constant, destination register1
    ```

<br>
이와 같이 얼마나 많은 Instruction을 사용할 수 있는가가 processor의 spec을 결정하는데 MIPS에는 instruction set이 약 4~50개가 있음. 

그런데 이 중에서 핵심적인 기능만 수행할 수 있는 것을 어떻게 만들까?

<br>



<br>

[<u>MIPS Instruction Set 링크 참</u>조](https://uweb.engr.arizona.edu/~ece369/Resources/spim/MIPSReference.pdf)

instruction set의 종류(기본,핵심,필수가 되는 세 가지 기능)

1. Arithmetic/Logic comp. (add, addi, andu, sub, subi, and, andi, or, ori, ...)
2. data move(memory-to-reg, reg-to-memory, lw, sw,...)
3. flow control (conditional branch(if), jump, call/return)



<br>

```assembly
add rd, rs, rt # rs + rt -> rd
addi rd, rs, Const # rd:register destination, rs:register source, Const:constanct number

lw, rd, Mem_adr #register 내용을 memory로 옮겨라
sw, rs, Mem_adr 

beq, $1, $2, Label # if $1 == $2, Label로 jump (branch equal)
bne, $1, $2, Label # if $1 != $2, Label로 jump

J Label #just jump to Label
Jal Function # jump and link; function으로 갔다가 다시 돌아와라(call)
```

위와 같은 명령어들은 모두 32-bit로 구성되어 있음.

- R타입
  - 만약 제일 쉬운 add인 경우
    - 맨 앞에 op code정보(add)가 6bit를 차지하고
    - 뒤에 잇따라 오는 세 개의 register의 정보는 5bit씩 표현된다.
    - 그러면 남는 11bit는? -> 거의 사용되지 않는다.

- I타입
  - load, store의 경우
    - 맨 앞에 op code(load or store) - 6bit
    - register 정보 - 5bit
    - 남는 bit는 모두 memory address로 사용될 수 있음
  - beq, bne의 경우
    - 맨 앞 op code - 6bit
    - register 두 개 5bit씩 - 10 bit
    - 나머지는 Label 정보

- J 타입
  - J, Jal의 경우
    - 맨 앞 op code - 6bit
    - 나머지 Label or Function 정보



### 메모리 위치를 지정하는 방법

- Direct addressing
  - mem.adr가 0x10010 이라고 가정했을 때 
    - memory address가 몇 번지인지 딱 지정한 방법
    - 선언한 변수가 중간에 수정이나 삭제가 되면 위 주소가 바뀌어 버린다.
    - 혹은 다른 메모리가 0x10008 번지로 지정되면 위 memory 주소는 전부 옮겨야 하기 때문에 매우 번거로워 이 방식은 잘 사용하지 않는다.



- Indirect addressing(base addressing)

  - ![image](https://user-images.githubusercontent.com/79521972/159170333-f2d23f7a-f0eb-46bb-9563-137edb9d1476.png)

  - 메모리의 위치(0x10010)를 찾아갈 때  바로 찾아가는 것이 아니라

  - 메모리가 현재 점유하고 있는 범위의 시작 주소를 가리키는 포인터(<span style="color:blue">base register</span>)가 존재하여 그 포인터에는 해당 주소로 부터 메모리가 몇 번째 떨어진 곳에 있는 지의 number가 들어있음

  - ```assembly
    lw $5, 10($4) # 10:offset, $4:base register
    ```





https://skagh.tistory.com/8







