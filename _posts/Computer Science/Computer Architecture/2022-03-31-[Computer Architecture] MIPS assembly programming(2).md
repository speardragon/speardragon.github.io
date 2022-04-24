---
layout: single
title: "[Computer Architecture] MIPS assembly programming(2)"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Addressing Modes

### How do we address the operands?(In MIPS)

- Register Only
- Immediate
- Base Addressing
- PC-Relative
- Pseudo Direct

<br>

#### Register Only

- Operands found in registers
  - Example: add $s0, $t2, $t3
  - Example: sub $t8, $s1, $0
- Immediate
  - 16-bit immediate used as an operand
    - Example:addi $s4, $t5, -73
    - Example: ori $t3, $t7 0xFF

<br>

#### Base Addressing

- Address of operand is:
  base address + sign-extended immediate

- Example: lw $s4, 72($0)
  - $0은 data memory의 address
  - address = $0 + 72
  
- Example: sw $t2, -25($t1)
  - address = $t1 - 25

<br>

#### PC-Relative Addressing

- Program memory

- 우리는 label을 그냥 사용하면 되지만 예를 들어 `else:` 로 가야할 때 PC에서 얼마만큼 떨어진 곳으로 가야 하는 지를 알아야 한다.

- Program memory address

```assembly
0x10 		beq $t0, $0, else
0x14 		addi $v0, $0, 1 
0x18 		addi $sp, $sp, i
0x1C 		jr $ra
0x20 	else: addi $a0, $a0, -1
0x24 		jal factorial
```

위와 같은 경우에 beq에서 $t0와 $0가 같으면 else로 jump하는데 이 때 4칸을 뛰어야 한다.

그런데 PC에서는 미리 다음 주소로 이동하기 위해 값을 다음으로 지정해 놓고 있기 때문에 3칸만 뛰어도 되는 것이다.

![image](https://user-images.githubusercontent.com/79521972/160977970-16c48d2c-21c1-443a-a33e-a0ab07071f3e.png)



BTA(Branch Target Address) = **(PC + 4)** + (Imm<<2)

- 4를 더한 것은 PC가 다음으로 이동하기 위해 미리 한 칸 가 있는 것이고 imm(3)에 left shift를 2번하면 x4의 효과가 있기 때문에 한 것이다.(따라서 (PC+4) + 12)

<br>

#### Pseudo-direct Addressing

```assembly
0x0040005C 			jal sum
...
0x004000A0 		sum: add $v0, $a0, $a1
```



![image](https://user-images.githubusercontent.com/79521972/160978342-d67ec7d4-8e9e-47f4-85c7-73f1a4f2cf4e.png)



- 26bit밖에 없는데 32bit를 만들어야 하는 경우

  - JTA(Jump Target Address)

  - 맨 뒤에 2 비트를 0으로 채움

  - 맨 앞의 4bit(한 바이트)는 PC에서 그대로 가져온다.

  - J타입은 원래 어디로 jump 할 지를 바로 정해주는 direct addressing인데 여기서 32bit를 만들기 위해 앞의 4bit는 PC에서 왔기 때문에 Pseudo-direct addressing이라고 한다.

<br>

## How to Compile & Run a Program

![image](https://user-images.githubusercontent.com/79521972/160979034-7ab2fc1e-6ee8-4ccd-b08a-34796fa17d98.png)





<br>

## What is Stored in Memory?

- Instructions (also called text)

- Data 

  - Global/static: allocated before program begins

  - Dynamic: allocated within program 

- How big is memory? 

  - At most 2<sup>32</sup> = 4 gigabytes (4 GB) 
  - From address 0x00000000 to 0xFFFFFFFF



<br>

## MIPS Memory Map

- Text segment: 256MB 
- Global data segment: 64 KB,  accessed by $gp 
- $gp does not change during  execution (unlike $sp)
- Reserved: I/O 영역(위) 과 OS(아래)가 사용하는 공간

![image](https://user-images.githubusercontent.com/79521972/160979282-13099d79-9ddb-4521-8d72-b11e222e5945.png)

<br>

## Example RISC-V Memory Map

![image](https://user-images.githubusercontent.com/79521972/160979433-a915096d-d612-4091-93f7-e34e9d0af372.png)

address는 다르지만 기본적인 구조는 매우 유사함(MIPS와)



<br>

## Example Program: C Code

```c
int f, g, y; // global variables
int main(void) 
{
    f = 2;
    g = 3;
    y = sum(f, g);
    
    return y;
}
int sum(int a, int b) {
	return (a + b);
}
```



## Example Program: compiling

```assembly
.data
f:
g:
y:
.text
main:
    addi $sp, $sp, -4 	# stack frame
    sw $ra, 0($sp) 		# store $ra
    addi $a0, $0, 2 	# $a0 = 2
    sw $a0, f 			# f = 2 ; label을 그냥 그대로 써도 됨
    addi $a1, $0, 3 	# $a1 = 3
    sw $a1, g 			# g = 3
    jal sum 			# call sum
    sw $v0, y 			# y = sum()
    lw $ra, 0($sp) 		# restore $ra
    addi $sp, $sp, 4 	# restore $sp
    jr $ra 				# return to OS
sum:
    add $v0, $a0, $a1 	# $v0 = a + b
    jr $ra 				# return
```





## Example Program: Symbol Table

![image](https://user-images.githubusercontent.com/79521972/160980280-e573b6b8-50e7-429f-b04c-7eb950f0d6c8.png)

f,g,y : data memory

main, sum: program memory(text memory)

<br>

## Example Program: linking (Executable)

![image](https://user-images.githubusercontent.com/79521972/160980322-7696a025-98a3-43e9-99e4-0c55e10ca64a.png)

`sw $a0, 0x8000($gp)`가 의미하는 것:

global pointer는 memory의 global data 공간을 가리키고 있는데 이 공간에서 0x8000만큼 떨어진 곳으로 store하라는 것인데 0x8000은 맨 앞의 8이 이진수로 '1000'을 나타내고 이는 -8이기 때문에  -8000을 의미하는 것이기 때문에 0x10008000에 있던 $gp 가 0x10000000으로 가서 f를 저장하고 g를 저장할 때는 0x8004만큼 움직이면 -7996을 의미하는 것이기 때문에 f를 저장한 곳보다 4만큼 위의 공간에 저장하는 것이다.

<br>

## Example Program: loading (in memory)

- OS loads the program from  storage
- OS sets $gp=0x10008000,  and $sp= 0x7ffffffc, and  jump to the beginning of  the program  (jal 0x00400000).  
-  Reserved area: memory-mapped I/O, interrupt  vector

![image](https://user-images.githubusercontent.com/79521972/160980658-918b2b67-6204-4b85-a643-4e82a61f1797.png)





---

## Lab 1: MIPS Assembly Programming

MIPS을 해보는 것처럼 실행할 수 있는 runtime simulator인 MARS로 구동시킬 것임



---

## Odds & Ends (Misc.)

- Pseudo-instructions
  - Not a part of ISA, but commonly used
  - assembler가 진짜 instruction으로 바꿔줌
- Exceptions(interrupt)
  - Unscheduled function call(예기치 않은 function call)
  - Exception handler is at 0x80000180
- Signed and unsigned instructions
- Floating-point instructions



<br>

### Pseudoinstruction

![image](https://user-images.githubusercontent.com/79521972/160982585-ed6ec6eb-d2b8-42b3-92b4-cef838e9e948.png)

- li $s0, 0x1234AA77 (load immediate) : 32bit를 immediate로 쓸 때에는 두 과정으로 나누어 준다.
  - lui / ori



<br>

### Exception

- Unscheduled function call to exception handler 
- Caused by: 
  -  Hardware, also called an **interrupt**, e.g., keyboard 
  - Software, also called **traps**, e.g., undefined instruction 
-  When exception occurs, the processor:  
  - Records the cause of the exception (원인을 저장)
  - Jumps to exception handler (at instruction address  0x80000180) 
  - Returns to program

<br>

### Exception Registers

- Not part of register clk

  - Cause: **Records** **cause** of exception

  - EPC (Exception PC): Records PC where exception occurred 
    - **exception handler에서 다 처리한 후 다시 돌아올 자리**

- EPC and Cause: part of Coprocessor 0 

- Move from Coprocessor 0

  - mfc0 $t0, EPC 
  - Moves contents of EPC into $t0



<br>

## Exception Causes

![image](https://user-images.githubusercontent.com/79521972/160984004-e9bbc166-237c-488b-b997-fc29c2e264d6.png)





## Exception Flow

- Processor saves cause and exception PC in **Cause** and **EPC** 
-  Processor jumps to exception handler (0x80000180) 
- Exception handler: 
  -  Saves registers on stack
     -  내가 사용할 register 저장(어떤 register가 문제인지 모르기 때문에)
  -  Reads **Cause** register 
     -  `mfc0 $k0, Cause `
  -  Handles exception 
  -  Restores registers 
  -  **Returns** to program 
    - mfc0 $k0, EPC `//$k0, $k1 reserved by OS jr $k0`
    - jr $k0



## Exception Example

- Example: read one input from a keyborad (assum interrupt is enabled)

![image](https://user-images.githubusercontent.com/79521972/160984184-03d27317-ab2e-4f1f-b7d7-bdb48ddfdfdd.png)



<img src="https://user-images.githubusercontent.com/79521972/161664669-86bff572-4fde-4ddb-8d54-8076b44d0fe5.png" alt="image" style="zoom:50%;" />

$k0-$k1 : OS temporaries

<br>

## Signed & Unsigned Instructions

- Addition and subtraction
- Multiplication and division
- Set less than

<br>

## Addition & Subtraction

- Signed: add, addi, sub
  - Same operation as unsigned versions
  - But processor takes exception on overflow

- Unsigned: addu, addiu, subu
  - Doesn't take exception on overflow

> Note: addiu sign-extends the immediate (two versions are identical except exception is triggered.

<br>

## Mul

- Signed: mult, div
- Unsigned: multu, divu

ex) 0xFFFF_FFFF * 0xFFFF_FFFF

=> 0xFFFFFFFE00000001   (unsigned)

=> 0x0000000000000001 (signed)

<br>

## Set Less Than

- Signed: slt, slti
- Unsigned: sltu, sltiu

> Note: sltiu sign-extends the immediate before comparing it to the register.

<br>

## Loads

- Signed: 
  - Sign-extends to create 32-bit value to load into register
  - Load halfword: lh 
  - Load byte: lb
- Unsigned:
  - Zero-extends to create 32-bit value
  - Load halfword unsigned: lhu
  - Load byte: lbu

<br>

## Floating-Point Coprocessor(or Accelerator)

별로 중요하지 않음, 내부가 중요함

<br>

## Example design

![image](https://user-images.githubusercontent.com/79521972/160984525-2c2b69ae-5eea-413e-9ea5-878d9abf25a3.png)

<br>



## Floating-Point Instructions

- Floating-point coprocessor(Coprocessor 1)
- 32 32-bit floating-pointer registers($f0-$f31)
- Double-precision values held in two floating point registers
  - e.g., $f0 and $f1,$f2 and $f3, etc.
  - Double-precision floating point registers: $f0, $f2, $f4, etc.

![image](https://user-images.githubusercontent.com/79521972/160984582-8e09efe1-7f7a-4061-9545-7074a12b1297.png)



<br>

## F-Type Instruction Format

- Opcode = 17 (010001<sub>2</sub>)
- Single-precision:
  - cop = 16 (010000<sub>2</sub>)
  - add.s , sub.s, div.s, neg.s, abs.s, etc.
- Double-precision:
  - cop = 17 (010001<sub>2</sub>)
  - add.d, sub.d, div.d, neg.d, abs.d, etc.

<br>



## Floating-Point Branches

- Set/clear condition flag: fpcond 
  - Equality: c.seq.s, c.seq.d 
  - Less than: c.lt.s, c.lt.d 
  - Less than or equal: c.le.s, c.le.d
- Conditional branch
  - bclf: branches if fpcond is FALSE 
  - bclt: branches if fpcond is TRUE
- Loads and stores  
  - lwc1: lwc1 $ft1, 42($s1)  ; load coprocessor 1
  - swc1: swc1 $fs2, 17($sp)



<br>

## ARM & MIPS instruction set

![image](https://user-images.githubusercontent.com/79521972/160984760-2ad592ce-a197-49b9-8dfb-01f4abbeed12.png)



난이도: MIPS < RISC-V < ARM

ARM은 복잡, MIPS은 비교적 쉬움, 이 사이에 있는 것이 RISC-V



---

Q)

A)handling하는 방식이 processor마다 조금씩 다를 수 있다.



Q)floating point /single, double

A)

범위가 틀린 것

single precision

- 32 bit
- float a;

double precision

- 64bit
- double b;



Q)unsigned에서는 overflow가 일어나지 않는데 이를 어떻게 처리하는 것인가?

A)있을 수 있으나 무시하여 발생하지 않도록 설계하는 것이다.

---

















