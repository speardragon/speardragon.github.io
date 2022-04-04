---
layout: single
title: "[Computer Architecture] week 5-1"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



## Addressing Modes

#### Register Only

- Operands found in registers
  - Example: add $s0, $t2, $t3
  - Example: sub $t8, $s1, $0
- Immediate
  - 16-bit immediate used as an operand
    - Example:addi $s4, $t5, -73
    - Example: ori $t3, $t7 0xFF



#### Base Addressing

- Address of operand is:
  base address + sign-extended immediate

- Example: lw $s4, 72($0)
  - address = $0 + 72
- Example: sw $t2, -25($t1)
  - address = $t1 - 25



#### PC-Relative Addressing

- 우리는 label을 그냥 사용하면 되지만 예를 들어 `else:` 로 가야할 때 PC에서 얼마만큼 떨어진 곳으로 가야 하는 지를 알아야 한다.

- Programm memory address

```assembly
0x10 beq $t0, $0, else
0x14 addi $v0, $0, 1 
0x18 addi $sp, $sp, i
0x1C jr $ra
0x20 else: addi $a0, $a0, -1
0x24 jal factorial
```

![image](https://user-images.githubusercontent.com/79521972/160977970-16c48d2c-21c1-443a-a33e-a0ab07071f3e.png)



BTA(Branch Target Address) = (PC + 4) + (Imm<<2)



<br>

#### Pseudo-direct Addressing

```assembly
0x0040005C jal sum
...
0x004000A0 sum: add $v0, $a0, $a1
```



![image](https://user-images.githubusercontent.com/79521972/160978342-d67ec7d4-8e9e-47f4-85c7-73f1a4f2cf4e.png)



맨 앞의 4bit는 PC에서 가져온다.

몇 번지로 가겠다를 바로 지정 but 앞의 4bit는 PC에서 왔기 때문에 Pseudo-direct addressing이다.

<br>

## How to Compile & Run a Program

![image](https://user-images.githubusercontent.com/79521972/160979034-7ab2fc1e-6ee8-4ccd-b08a-34796fa17d98.png)





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

![image](https://user-images.githubusercontent.com/79521972/160979282-13099d79-9ddb-4521-8d72-b11e222e5945.png)



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
    sw $a0, f 			# f = 2
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



<br>

## Example Program: linking (Executable)

![image](https://user-images.githubusercontent.com/79521972/160980322-7696a025-98a3-43e9-99e4-0c55e10ca64a.png)



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

- Pseudoinstructions
  - Not a part of ISA, but commonly used
- Exceptions
  - Unscheduled function call(예기치 않은 function call)
  - Exception handler is at 0x80000180
- Signed and unsigned instructions
- Floating-point instructions



<br>

### Pseudoinstruction

![image](https://user-images.githubusercontent.com/79521972/160982585-ed6ec6eb-d2b8-42b3-92b4-cef838e9e948.png)





<br>

### Exception

- Unscheduled function call to exception  handler 
- Caused by: 
  -  Hardware, also called an interrupt, e.g., keyboard 
  - Software, also called traps, e.g., undefined instruction 
-  When exception occurs, the processor:  
  - Records the cause of the exception 
  - Jumps to exception handler (at instruction address  0x80000180) 
  - Returns to program



### Excemption Registers

- Not part of register flk

  - CauseL Records cuase of exception

  - EPC (Exception PC): Records PC where exception  occurred 

- EPC and Cause: part of Coprocessor 0 

- Move from Coprocessor 0

  - mfc0 $t0, EPC 
  - Moves contents of EPC into $t0





## Exception Causes

![image](https://user-images.githubusercontent.com/79521972/160984004-e9bbc166-237c-488b-b997-fc29c2e264d6.png)





## Exception Flow

- Processor saves cause and exception PC in Cause and EPC 
-  Processor jumps to exception handler (0x80000180) 
- Exception handler: 
  -  Saves registers on stack 
  -  Reads Cause register mfc0 $t0, Cause 
  - Handles exception 
  - Restores registers 
  -  Returns to program 
    - mfc0 $k0, EPC `//$k0, $k1 reserved by OS jr $k0`



## Exception Example

- Example: read one input from a keyborad (assum interrupt is enabled)

![image](https://user-images.githubusercontent.com/79521972/160984184-03d27317-ab2e-4f1f-b7d7-bdb48ddfdfdd.png)





<br>

## Signed & Unsigned Instructions

- Addition and subtraction
- Multiplication and division
- Set less than



## Addition & Subtraction

- Signed: add, addi, sub
  - Same operation as unsigned versions
  - But processor takes exception on overflow

- Unsigned: addu, addiu, subu
  - Doesn't take exception on overflow

> Note: addiu sign-extends the immediate (two versions are identical except exception is triggered.



## Mul



## Set Less



## Load



## Floating-Point Coprocessor(or Accelerator)

별로 중요하지 않음, 내부가 중요함



## Example design

![image](https://user-images.githubusercontent.com/79521972/160984525-2c2b69ae-5eea-413e-9ea5-878d9abf25a3.png)





## Floating-Point Instructions

![image](https://user-images.githubusercontent.com/79521972/160984582-8e09efe1-7f7a-4061-9545-7074a12b1297.png)



<br>

## F-Type Instruction Format



## Floating-Point Branches

- Set/clear condition flag: fpcond 
  - Equality: c.seq.s, c.seq.d 
  - Less than: c.lt.s, c.lt.d 
  - Less than or equal: c.le.s, c.le.d
- Conditional branch
  - bclf: branches if fpcond is FALSE 
  - bclt: branches if fpcond is TRUE
- Loads and stores  
  - lwc1: lwc1 $ft1, 42($s1) 
  - swc1: swc1 $fs2, 17($sp)





## ARM & MIPS instruction set

![image](https://user-images.githubusercontent.com/79521972/160984760-2ad592ce-a197-49b9-8dfb-01f4abbeed12.png)





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

















