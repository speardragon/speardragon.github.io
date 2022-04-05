---
layout: single
title: "[Computer Architecture] week 5-2"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



# Review)

RISC는 시험에 나오지 않음 (MIPS와 유사하기 때문에)

## ARM v8 instruction (from COD)

- In moving to 64-bit, ARM did a complete overhaul.
- ARM v8 resembles MIPS
- Changes from v7:
  - No conditional execution field
  - Immediate field
  - Dropped load/store multiple
  - PC is no longer a GPR
  -  GPR set expanded to 32
  -  Addressing modes work for all word sizes 
  - Divide instruction
  - Branch if equal/branch if not equal instruction

MIPS와 거의 유사함



<br>

## Intel x86 Register

![image](https://user-images.githubusercontent.com/79521972/161671399-d1d81aee-9469-47ad-8ff7-17331745e223.png)

flag register



<br>

## Intel x86 Instructions

![image](https://user-images.githubusercontent.com/79521972/161671594-0a7e9543-7b9a-4fc2-b12f-388e7d4c7d17.png)



RISC에서는 fetch할 때 32bit만 가져오면 됐는데 CISC는 32bit로 고정되어 있지 않다. 

그래서 CISC는 instruction buffer라는 것에 몽땅 다 넣어서 명령어가 몇 bit인지 decode하여 명령어를 가져온다.



<br>

## MIPS and RISC-V

![image](https://user-images.githubusercontent.com/79521972/161671868-027a12ae-aa23-436f-bf87-fd12ea4e1569.png)



MIPS와 비교했을 때, RISC-V는 opcode가 뒤쪽으로 가 있다.



---

<br>

## RISC-V Register Set

![image](https://user-images.githubusercontent.com/79521972/161672037-7235da1a-0aef-4286-a565-0b0573a9e1b0.png)





<br>

## Branching

- Execute instructions out of sequence
- Types of brances:
  - Conditional
    - branch if equal (beq)
    - branch if equal (beq)
    - branch if equal (beq)
    - branch if equal (beq)
  - Unconditional
    - jump(j)
    - jump register(jr)
    - jump register(jr)
    - jump register(jr)





<br>

## RISC-V Function Calling Conventions

- Call Function: jump and link (jal func) 
- Return from function: jump register (jr ra) 
- Arguments: a0 – a7
- Return value: a0



<br>

## Preserved Registers

![image](https://user-images.githubusercontent.com/79521972/161672266-afa76b96-7341-4767-865f-84efc5466812.png)



<br>

## Machine Language

- Binary representation of instructions
-  Computers only understand 1’s and 0’s
-  32-bit instructions 
  - Simplicity favors regularity: 32-bit data &  instructions
-  4 Types of Instruction Formats: 
  - R-Type
  -  I-Type
  -  S/B-Type
  - U/J-Typ



<br>



### U/J-Type

- Upper - immediate-Type
- Jump-Type
- Differ only in immediate encoding
- 





## Review: Instruction Formats

![image](https://user-images.githubusercontent.com/79521972/161672444-c040f3c0-7400-490b-a713-2f4ea43c5446.png)

MIPS의 R,I, J와 거의 유사하다.



<br>

## Compressed instructions

- 16-bit RISC-V instructions
  - register 32-bit을 다 쓰지 않아 더 가볍지만 할 수 있는 명령이 제한되기 때문에 microprocessor나 embedded에 사용된다.
- Replace common integer and floating-point  instructions with 16-bit versions.
-  Most RISC-V compilers/processors can use a  mix of 32-bit and 16-bit instructions (and  use 16-bit instructions whenever possible). 
- Uses prefix: c.



<br>

## Compressed Machine Formats

![image](https://user-images.githubusercontent.com/79521972/161672587-e1e20c8c-357d-4503-a9e8-7454cb175951.png)





<br>

## RISC-V Floating-Point Extensions

- RISC-V offers three floating point extensions: 
- RVF: single-precision (32-bit)
- 8 exponent bits, 23 fraction bits
- RVD: double-precision (64-bit) 
- 11 exponent bits, 52 fraction bits
- RVQ: quad-precision (128-bit) 
- 15 exponent bits, 112 fraction bits





## Exceptions

MIPS와 마찬가지임 전 강의 참조.





## RISC-V Instructions

MIPS와 거의 같다.

![image](https://user-images.githubusercontent.com/79521972/161673006-a87859d5-e4a7-4321-809d-9a9e798700fb.png)

<img src="https://user-images.githubusercontent.com/79521972/161672888-094eff76-2a63-4a82-ad8b-36760e398b15.png" alt="image" style="zoom:80%;" />

![image](https://user-images.githubusercontent.com/79521972/161672918-1d3a6a7b-3363-4d48-b538-b8f173547d3a.png)



























