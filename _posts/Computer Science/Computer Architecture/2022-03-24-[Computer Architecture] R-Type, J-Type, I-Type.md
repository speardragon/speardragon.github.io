---
layout: single
title: "[Computer Architecture] R-Type, J-Type, I-Type"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



# Review)

**assembly instruction**

![image](https://user-images.githubusercontent.com/79521972/159843857-8a39415e-5445-4956-9453-403f56029513.png)



<br>

**Machine instruction**

![image](https://user-images.githubusercontent.com/79521972/159824475-918f28c9-0278-4d6c-a16c-db34ccf6f99f.png)

lw, sw, beq, bne는 I type을 써도 무방하다. 공간 할애하는 것이 유사하기 때문에

<br>

**R-Type**

- opcode: 어떤 명령어 인지를 표현하는 bit

- 남는 bit 공간 중 5bit는 shamt 명령어를 사용하기 위해 사용되는 공간이다.
- 그래서 또 최종적으로 남는 6bit는 funct. code로 지정하여 R-type인 경우에 opcode와 더불어 사용되는 공간이다.

<br>



**Decode**

- opcode를 본다.
- R -type의 opcode는 000000으로 설정되어있다.
  - 그래서 구체적인 R-type의 명령어를 알고싶으면 function code를 봐서 연산 종류를 알 수 있게 되는 것이다.(add, sub, xor, and,...)



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
  - Operation is completely determined by opcode(6 bit)

![image](https://user-images.githubusercontent.com/79521972/159845601-d31eb4ef-d48c-4b79-ab1e-ea4b3c78119b.png)



<br>

### I-Type Examples

![image](https://user-images.githubusercontent.com/79521972/159845634-9038ed8a-84ff-4b8b-98b5-9292435a9346.png)

![image](https://user-images.githubusercontent.com/79521972/159845667-ee1015ed-fba4-4ddc-afe5-cfc9608e43ac.png)



code와 field value의 register 위치가 다름에 유의(destination의 위치가 code에서는 맨 처음이다.)

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

- 32-bit instructions & data stored in memory
- Sequence of instructions: only difference between two applications
- To run a new program:
  - No rewiring required
  - Simply store new program in memory
- Program Execution:
  - Processor **fetches** (reads) instructions from memory in sequence
  - Processor **performs** the specified operation



<br>

### The Stored Program

![image](https://user-images.githubusercontent.com/79521972/159845977-d050a693-4fa4-49bc-9f29-dd20012c665d.png)



<br>

### Interpreting Machine Code

- Start with opcode: tells how to parse rest
- If opcode all 0's 
  - <mark>R-Type instruction</mark>
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

- **and, or, xor, nor**
  - and: useful for **masking** bits
    - Masking all but the least significant byte of a value:  
      - 0xF234012F AND 0x000000FF = 0x0000002F
  - or: useful for **combining** bit fields
    - Combine 0xF2340000 with 0x000012BC:  
      - 0xF2340000 OR 0x000012BC = 0xF23412BC
  - nor: useful for **inverting** bits:
    - A NOR $0 = NOT A
- **andi, ori, xori**
  - 16-bit immediate is zero-extended (not sign-extended)
  - nori not needed



<br>

각 example 참고



<br>

### Shift Instructions

5가 들어가는 위치는 shamt이기 때문에 register가 들어가는 자리가 하나가 남는데 그 자리는 그냥 쓰지 않는 자리이다.

- sll: shift left logical
  - Example: `sll $t0, $t1, 5 # $t0 <= $t1 << 5`

- srl: shift right logical
  - Example: `srl $t0, $t1, 5 # $t0 <= $t1 >> 5`
- sra: shift right arithmetic
  - Example: `sra $t0, $t1, 5 # $t0 <= $t1 >>> 5`



shift right 할 때는 sign extension으로 앞 부분의 새로 만들어지는 bit를 채운다.

<br>

### Variable Shift Instructions

- sllv: shift left logical variable
  - Example: `sllv $t0, $t1, $t2 # $t0 <= $t1 << $t2`
- srlv: shift right logical variable
  - Example: srlv $t0, $t1, $t2    # $t0 <= $t1 >> $t2

- srav: shift right arithmetic variable
  - Example: srav $t0, $t1, $t2  # $t0 <= $t1 >>> $t2




<br>

### Shift Instructions

![image](https://user-images.githubusercontent.com/79521972/159847674-e550b219-f365-43e1-98df-9088d867e4de.png)



![image](https://user-images.githubusercontent.com/79521972/159847722-e6b29826-e73d-459b-b461-44cdf0debd67.png)





<br>

### Generating Constants

- 16-bit constant using addi:

  - C Code

    - ```c
      // int is a 32-bit signed word 
      int a - 0x4f3c;
      ```

  - MIPS assembly code

    - ```assembly
      # $s0 = a
      addi $s0, $0, 0x4f3c
      ```

- 32-bit constants using load upper immediate(lui) and ori:

  - 32 bit인 경우 16bit의 제약 조건에서 사용할 수 없기 때문에 두 개로 나눠서 사용한다.

    - lui / ori
  
  - C Code

    - ```c
      int a = 0xFEDC8765; //32bit
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
  - Quotient(몫) in `lo`
  - Remainder(나머지) in `hi`
- **M**oves **f**rom **lo/hi** special registers
  - `mflo $s2`
    - $s2 <- $lo : lo(몫, 32bit)으로부터 데이터를 가져와 $2에 저장한다.
  - `mfhi $s3`



<br>

### Branching

- Execute instructions out of sequence(점프)

- Types of branches:

  - **Conditional** 
    - branch if equal (bea)
    - branch if not equal (bne)

  - **Unconditional**
- Jump (j)
    - jump register (jr) : register로 jump해라
    
- jump and link (jal)



<br>

### Review: The Stored Program

![image](https://user-images.githubusercontent.com/79521972/159848676-bc959799-1288-4f24-9c5e-1629d0a7c959.png)

PC부터 시작

<br>

### Conditional Branching (beq)

**\# MIPS assembly**

![image](https://user-images.githubusercontent.com/79521972/159848789-8fd71fbe-e9f3-4583-8c72-ca8d6d4ae164.png)

> Labels indicate instruction location. They can't be reserved words and must be followed by colon (:)

sll는 x2의 효과가 있기 때문에 2만큼 shift left를 하면 x2<sup>2</sup> 이기 때문에 $s1에는 4가 저장된다.



<br>

### The Branch Not Taken (bne)

![image](https://user-images.githubusercontent.com/79521972/159924349-0201c5de-d535-4876-b725-15fe22546514.png)





### Unconditional Branching(j)

![image](https://user-images.githubusercontent.com/79521972/159924405-1cefcf8b-f22c-437a-8876-f2a694de37b6.png)



### Unconditional Branching(jr)

![image](https://user-images.githubusercontent.com/79521972/159924430-c37afc03-366e-4d27-a74c-ef91be47e412.png)

> jr is an R-type instruction

<br>



### High-level Code Constructs

- if statements
- if/else statements
- while loops
- for loops



<br>

#### If Statement

- C Code

```c
if (i == j)
    f = g + h;

f = f - i;
```

- MIPS assembly code

```assembly
# $s0 = f, $s1 = g, $s2 = h
# $s3 = i, $s4 = j
	bne $s3, $s4, L1
	add $s0, $s1, $s2
	
L1 sub $s0, $s0, $s3
```

> Assembly tests opposite case (i != j) of high-level code(i == j)

high level 언어에서는 ==이면 안의 구문을 실행하는 것이니까 assembly 언어에서는 !=이면 그 다음을 생략 하고 L1으로 점프하게 한 것이다.

<br>

#### If/Else Statement

- C Code

```c
if (i == j)
    f = g + h;
else
    f = f - i;
```

- MIPS assembly code

```assembly
# $s0 = f, $s1 = g, $s2 = h
# $s3 = i, $s4 = j
    bne $s3, $s4, L1
    add $s0, $s1, $s2
    j done
L1: sub $s0, $s0, $s3
done:
```

<br>

#### While Loops

- C Code

```c
// determines the power
// of x such that 2x = 128
int pow = 1;
int x = 0;
while (pow != 128) {
    pow = pow * 2;
    x = x + 1;
}
```

- MIPS assembly code

```assembly
# $s0 = pow, $s1 = x
    addi $s0, $0, 1
    add $s1, $0, $0
    addi $t0, $0, 128
while: beq $s0, $t0, done
    sll $s0, $s0, 1
    addi $s1, $s1, 1
    j while
done:
```

> Assembly tests for the opposite case (pow == 128) of the C code (pow != 128).

<br>

#### For Loops

```c
for (initialization; condition; loop operation)
    statement
```

- initialization: executes before the loop begins
- condition: is tested at the beginning of each iteration
- loop operation: executes at the end of each iteration
- statement: executes each time the condition is met

<br>

- High level code

```c
// add the numbers from 0 to 9
int sum = 0;
int i;
for (i=0; i!=10; i = i+1) {
	sum = sum + i;
}

```

- MIPS assembly code

```assembly
# $s0 = i, $s1 = sum
    addi $s1, $0, 0
    add $s0, $0, $0
    addi $t0, $0, 10
for: beq $s0, $t0, done
    add $s1, $s1, $s0
    addi $s0, $s0, 1
    j for
done:
```

<br>

### Less Then Comparison



- C Code

```c
// add the powers of 2 from 1 
// to 100
int sum = 0;
int i;
for (i=1; i < 101; i = i*2) {
	sum = sum + i;
}
```



- MIPS assembly code

```assembly
# $s0 = i, $s1 = sum
    addi $s1, $0, 0
    addi $s0, $0, 1
    addi $t0, $0, 101
loop: slt $t1, $s0, $t0
      beq $t1, $0, done
      add $s1, $s1, $s0
      sll $s0, $s0, 1
      j loop
done:
```

`$t1=1 if i<101`

`$t1=0 else if i > 101`

그래서 slt와 beq를 합쳐서 bgt를 사용하면 이를 알아서 slt+beq로 컴파일 해 준다.

- beq에서는 101보다 작은지 큰지에 대한 정보인 t1의 정보와 0을 비교하여 loop를 빠져나가는 것이다.



<br>













