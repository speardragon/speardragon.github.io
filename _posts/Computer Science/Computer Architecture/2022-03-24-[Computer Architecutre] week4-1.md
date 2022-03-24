---
layout: single
title: "[Computer Architecture] week 4-1."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



# Review)

**assembly instruction**

![image](https://user-images.githubusercontent.com/79521972/159843857-8a39415e-5445-4956-9453-403f56029513.png)



**Machine instruction**

![image](https://user-images.githubusercontent.com/79521972/159824475-918f28c9-0278-4d6c-a16c-db34ccf6f99f.png)



**R-Type**

- opcode: 어떤 명령어 인지를 표현하는 bit

- 남는 bit 공간 중 5bit는 shamt 명령어를 사용하기 위해 사용되는 공간이다.
- 그래서 또 최종적으로 남는 6bit는 funct. code로 지정하여 R-type인 경우에 opcode와 더불어 사용되는 공간이다.





**Decode**

- opcode를 본다.
- R -type의 opcode는 000000으로 설정되어있다.
  - 그래서 R-type의 명령어를 알고싶으면 function code를 봐서 연산 종류를 알 수 있게 되는 것이다.(add, sub, xor, and,...)



<br>

---

### I-Type

- Immediate type

- 3 operands:
  - rs, rt: register operands
  - imm: 16-bit two's complement immediate
- Other fields:
  - op: the opcode
  - Simplicity favors regularity: all instructions have opcode
  - Operation is completely determined by opcode

![image](https://user-images.githubusercontent.com/79521972/159845601-d31eb4ef-d48c-4b79-ab1e-ea4b3c78119b.png)



<br>

### I-Type Examples

![image](https://user-images.githubusercontent.com/79521972/159845634-9038ed8a-84ff-4b8b-98b5-9292435a9346.png)

![image](https://user-images.githubusercontent.com/79521972/159845667-ee1015ed-fba4-4ddc-afe5-cfc9608e43ac.png)





<br>

### J-Type

- Jump-type
- 26-bit address operand(addr)
- Used for jump instructions(j)

![image](https://user-images.githubusercontent.com/79521972/159845745-fe37029c-69ed-403c-867e-bb6dd0a7d224.png)



<br>

### Review: Instruction Formats

![image](https://user-images.githubusercontent.com/79521972/159845788-7dd1148b-5a8f-4c38-b193-b2dd2d9058c0.png)

<br>

### Power of the Stored Program

- 32-bit insturctions & data stored in memory
- Sequence of instrucntions: only difference between two applications
- To run a new program:
  - No wewiring required
  - Simply store new program in memory
- Program Execution:
  - Processor fetches (reads) instructions from memory in sequence
  - Processor performs the specified operation



<br>

### The Stored Program

![image](https://user-images.githubusercontent.com/79521972/159845977-d050a693-4fa4-49bc-9f29-dd20012c665d.png)



<br>

### Interpreting Machine Code

- Start with opcode: tells how to parse rest
- If opcode all 0's 
  - R-Type instruction
  - Function bits tell operation
- Otherwise
  - opcode tells operation (see the table appendix B.1 ~ B.3)

![image](https://user-images.githubusercontent.com/79521972/159846065-a4e98595-4ea7-4721-a9f4-7d2b53ea36c0.png)



<br>

### MIPS opcodes

![image](https://user-images.githubusercontent.com/79521972/159846219-334e5d68-1683-4e87-962d-d352a525f737.png)![image](https://user-images.githubusercontent.com/79521972/159846230-042da64b-3ed1-4790-a1ef-a7b1485bda79.png)





<br>

### Programming

- High-level languages:
  - ex) C, Java, Python
  - Written at higher level of abstraction
- Common high-level software constructs:
  - if/else statements
  - for loops
  - while loops
  - arrays
  - function calls

<br>

### Logical Instructions

- and, or, xor, nor
  - and: useful for **masking** bits
    - Masking all but the least significant byte of a value:  0xF234012F AND 0x000000FF = 0x0000002F
  - or: useful for **combining** bit fields
    - Combine 0xF2340000 with 0x000012BC:  0xF2340000 OR 0x000012BC = 0xF23412BC
  - nor: useful for **inverting** bits:
    - A NOR $0 = NOT A
- andi, ori, xori
  - 16-bit immediate is zero-extended (not sign-extended)
  - nori not needed



<br>

각 example 참고



<br>

### Shift Instructions

- sll: shift left logical
  - Example: `sll $t0, $t1, 5 # $t0 <= $t1 << 5`

- srl: shift right logical
  - Example: `srl $t0, $t1, 6 # $t0 <= $t1 >> 5`
- sra: shift right arithmetic
  - Example: `sra $t0, $t1, 5 # $t0 <= $t1 >>> 5`



shift right 할 때는 sign extension으로 앞 부분의 새로운 bit를 채운다.



### Variable Shift Instructions

- sllv: shift left logical variable
  - Example: `sllv $t0, $t1, $t2 # $t0 <= $t1 << $t2`



<br>

### Shift Instructions

![image](https://user-images.githubusercontent.com/79521972/159847674-e550b219-f365-43e1-98df-9088d867e4de.png)



![image](https://user-images.githubusercontent.com/79521972/159847722-e6b29826-e73d-459b-b461-44cdf0debd67.png)





<br>

### Generating Constants

- 16-bit constant using addi:

  - C Code

    - ```c
      // int is a 32-bit sogned word int a - 0x 4d3c;
      ```

  - MIPS assembly code

    - ```assembly
      # $s0 = a
      addi $s0, $0, 0x4f3c
      ```

- 32-bit constants using load upper immediate(lui) and ori:

  - C Code

    - ```c
      int a = 0xFEDC8765;
      ```

  - MIPS assembly code

    - ```assembly
      # $s0 = a
      lui $s0, 0xFEDC
      ori $s0, $s0, 0x8765
      ```



<br>

### Multiplication, Division

- Special register: lo, hi
- 32 x 32 multiplication, 64 bit result
  - `mult $s0, $s1`
  - Result in `{hi,lo}`
- 32-bit division, 32-bit quotient, remainder
  - `div $s0, $s1`
  - Quotient in `lo`
  - Remainder in `hi`
- **M**oves **f**rom **lo/hi** sepcial registers
  - `mflo $s2`
    - $s1 <- $lo
  - `mfhi $s3`



<br>

### Branching

- Execite insturcitons out of sequence

- Types of branches:

  - Conditional 
    - branch if equal (bea)
    - branch if not equal (bne)

  - Unconditional

    - Jump (j)
    - jump register (jr)

    - jump and link (jal)



<br>

### Review: The Stored Program

![image](https://user-images.githubusercontent.com/79521972/159848676-bc959799-1288-4f24-9c5e-1629d0a7c959.png)



<br>

### Conditional Branching (beq)

**\# MIPS assembly**

![image](https://user-images.githubusercontent.com/79521972/159848789-8fd71fbe-e9f3-4583-8c72-ca8d6d4ae164.png)

> Labels indicate instruction location. They can't be reserved words and must be followed by colon (:)



<br>

### The Branch Not Taken (bne)



### Unconditional Branching(j)



### Unconditional Branching(jr)



<br>

### High-level Code Constructs

- if statements
- if/else statements
- while loops
- for loops



<br>

#### If Statement

C Code

```c
if (i == j)
    f = g + h;

f = f - i;
```



MIPS assembly code

```assembly
# $s0 = f, $s1 = g, $s2 = h
# $s3 = i, $s4 = j
```



<br>

#### While Loops







#### For Loops





<br>

### Less Then Comparison

s0가 t0보다 less than이면 1





<br>

Q)assembly code에서 

A)











