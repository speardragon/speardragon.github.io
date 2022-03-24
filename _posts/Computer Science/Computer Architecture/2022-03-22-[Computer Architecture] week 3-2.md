---
layout: single
title: "[Computer Architecture] week 3-2."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



# Review)

CPU가 CPU로써 기능을 가지려면 3 종류의 기능은 반드시 갖춰야 한다.

- data processing(ALU)
- data move(load, store)
- flowcontrol(branch, jump)

- extra...

3 가지 instructions을 정의해 볼 것이다.



<br>

Q)memory에서 가져오는데 memory를 적는 부분에 왜 register 주소($)를 사용하는가?

A)$2에 7000번지를 넣는다고 하고 아래와 같은 코드를 적으면 $2에 10을 더하는 것은 7000에 10을 더하여 7010번지 memory에 load하는 것과 같은 의미가 된다. 이 때의 $2를 base address라고 한다.

**base addressing**

```assembly
lw $5, 10($2)
```

$2에 7000이 담겨있다고 하면 해당 값에 10만큼의 offset을 적용하였으므로 7010번지에 찾아가는 것이다.

base register를 찾은 다음에 offset을 적용하여 indirect(간접적으로) load한다.

direct로 하는 방법은 아래와 같다.

```assembly
lw $5, 7010
```

<br>

이렇게 indirect방법으로 메모리 주소를 참조하는 이유는 direct 방법으로 하다보면 변수를 하나 넣거나 뺄 때 다른 메모리 주소들의 값들이 밀려서 매 번 달라질 것이기 때문이다. 

즉, indirect로 하게 되면 $2라는 주소(base address라고 한다.)를 간접적으로 참조하여 해당 값이 바뀌어도 정확히 내가 원하는 값에 접근할 수 있기 때문이다.

<br>

<br>

$0에는 항상 0이라는 값이 들어있다. 이를 이용하여 위의 표현을 다음과 같이 표현할 수 있다.

0은 특정 연산을 할 때, 비교를 할 때 굉장히 중요한 지표(기준) 이기 때문에 따로 지정해 둔 것이다.

```assembly
lw $5, 7010($0) #zero register가 base register
```

그래서 이런식으로 사용할 수도 있겠지만 이렇게는 잘 사용하지 않는다.



<br>

- 모든 instruction은 32bit으로 정해져 있다.
- datapath(register file + ALU)
- Control(PC, IR, Control unit)
- Fetch - (decode &) Execute
- 명령어(instruction)를 IR에 가져와서 Decode해야 하는데 무슨 명령어 인지 구분하기 위해서, 즉 , 알아먹기 위해서 모든 명령어는 다음과 같이 표현된다.
  - 총 32bit로 이루어져 있다.
  - 32bit의 초반 6bit는 op-code 부분으로 표현된다.
  - 만약 add 의 경우 인자가 3 개가 필요하기 때문에 각각의 정보를 담는 5bit를 할당하여 이에 대한 15bit가 데이터 부분으로 표현된다.(R-type)
  - 만약 addi의 경우 한 개의 opcode 6bit, data 두 개와 Constant(immediate) 정보를 포함하기 때문에 앞의 두 데이터에 대해서는 5bit씩 할당하고 Const에 대한 부분은 나머지 16bit가 들어간다.(R-type)
    - 그래서 Const는 16bit로 표현될 수 있기 때문에 -2<sup>15</sup> ~ 2<sup>15</sup>(+-32k) 범위 내의 constant 값만 사용할 수 있다.
  
  - lw 와 sw 의 경우 register 정보 2 개와 constant 정보 1 개가 필요하기 때문에 위의 addi와 똑같은 구조의 bit를 사용하면 된다.(I-type)
  - J(Jump)의 경우 constant 1 개의 정보 만을 필요로 하기 때문에 6bit의 opcode와 나머지 26bit의 const. 정보가 차지하는 구조를 사용한다.(J-type)
  

![image](https://user-images.githubusercontent.com/79521972/159824475-918f28c9-0278-4d6c-a16c-db34ccf6f99f.png)



<br>

---

## Architecture Design Principles

Underlying design principles, as articulated by Hennessy and Patterson:

1. Simplicity favors regularity
   - simple한 것은 regularity를 선호한다.
2. Make the common cast that
3. Smaller is a faster
4. Good design demands good compromises



![image](https://user-images.githubusercontent.com/79521972/159403302-78ce4001-ad4b-4917-8ee8-fbdc755a7f12.png)



## Instructions



### Addition

- Addition

- C code

  - ```c
    a = b + c;
    ```

- MIPS assembly code

  - ```assembly
    add a, b, c
    ```



### sub



### Design Principle 1

Simplicity favors regularity







#### Multiple Instructions

- More complex code is handled by multiple MIPS instructions.



ALU의 input은 두 개로만 받는 형태로 취하겠다.(for simplicity)



### Design Principle 2

Make the common case fast



CISC는 명령어가 매우 많은데 어떤 명령어는 가뭄에 콩나듯 사용하여 비효율 적인데

자주 사용하는 것만 사용하게끔 만든 것이 RISC



## Operands

- Operand location : physical location in computer
  - Registers - `add $1, $2, $3`
  - Memory - `lw $1, 4($3)`
  - Constants(also called immediates) - `lw`



### Registers

- MIPS has 32 32-bit registers
- Registers are faster than memory(CPU안에 있기 때문에)
- MIPS called "32-bit architecture" because it operates on 32-bit data



**Why 32 bits?**

Data memory에 있는 data가 register file에 들어가야 하는 상황에 register file의 bit수가 커지면 커질 수록 안 좋기 때문에 적당한 수인 32로 정한 것이다.





### Design Principle 3





### MIPS Register Set

![image](https://user-images.githubusercontent.com/79521972/159404503-407c2aaa-c693-4704-90f7-6826b5397d92.png)

- \$0 : $zero 라고 쓰기도 함

- $at: reservation



- $t0 :아무나 쓸 수 있는?
- $s : 아주 중요한 데이터( 함부로  버리면 안되는)

- $t8-\$t9
- $k0-\$k1 : OS만 사용가능하도록 reserve 해 놓은 곳



- $ra: return address
  - 함수가 호출된 후에 할 일을 다 마치고 다시 돌아와야 하는데 함수를 호출한 곳의 address를 반환해 주는데 이때 사용되는 공간이다.



그래서 우리가 많이 쓰는 곳은 0, 8-25 이다. 나머지는 함부로 쓰기 위험한 것들이 많다.



- Registers:
  - $ before name 
  - Example: $0, "register zero", "dollar zero"







## Memory Operands

### Memory

- Too much data to fit in only 32 registers(word describes 32)
- Store more data in memory
- Memory is large, but slow(than CPU)
- Commonly used variables kept in registers



<br>

- First, we'll discuss word-addressable memory
- Then we'll discuss byte-addressable memory



>  MIPS and RISC_V are byte-addresable

little or big endian (강의 참조)

MIPS는 big endian을 사용한다.



### Word-Addressable Memory

다루지 않을 것임; 우리가 사용하는 것은 모두byte-addressable 이기 때문





### Byte-Addressable Memory

- Each data byte has unique address
- Load/store words or single bytes: load byte(1b) and store byte(sb)
- 32-bit word = 4 bytes, so word address increments by 4



![image](https://user-images.githubusercontent.com/79521972/159405678-b758e658-20b7-4c51-82a4-673c334fc658.png)





### Reading Byte-Addressable Memory

- The address of a memory word must now be multiplied by 4. 
- For example,
  - the 



#### Example: Load a word of data at memory address 8 into $s3.

- $s3 holds the value 0x01EE2842 after load
- ![image](https://user-images.githubusercontent.com/79521972/159405835-822b48fd-e59f-43d4-8f79-4dbaf0d34c66.png)

- 01EE2842는 $s3로 간다.



#### Example: store the value held in $t7 into memory address 0x10(16)

- if t7





### Big-Endian & Little-Endian Memory

- How to number bytes within a word?
- Little-endian: byte numbers start at the little (least significant) end
- Big-endian: byte numbers start at the big (most  significant) end
- Word address is the same for big- or little-endian

![image](https://user-images.githubusercontent.com/79521972/159406143-d69ac8c2-7439-4bd2-b70b-b2a8b886d438.png)

- Jonathan Swift’s Gulliver’s Travels: the Little-Endians broke their eggs on the little end of the egg and the BigEndians broke their eggs on the big end

- It doesn’t really matter which addressing type used – except when the two systems need to share data!

- Suppose $t0 initially contains 0x23456789
- After following code runs on big-endian system, what  value is $s0?

- In a little-endian system?
  - `sw $t0, 0($0)`
  - `lb $s0, 1($0)`





#### Design Principle 4

Good desgin demands good compromises







### Constants/Immediates

subi 필요없음





### R-Type

- Register-type
- 3 register operands:
  - rs, rt: source registers
  - rd: destination register

```assembly
add $rd, $rs, $rt
```

- Other fields:
  -  op: the operation code or opcode(0 for R-type instructions)
  - funct:

![image](https://user-images.githubusercontent.com/79521972/159406787-6bc35546-f937-408d-8670-d8df352dc093.png)

opcode와 funct를 모두 확인하여 무슨 연산인지 알게끔 만들어 놓은 구조이다.







#### R-type Examples

ALU연산을 할 때는 opcode를 0으로 두어서 funct을 보게끔 하였다.





Q) address가 8bit인데 word address는 4비트씩 커지나요?

A) address가 8bit라는 게 아니라 한 주소가 8bit라는 것이다. w



Q) asdf

A)어떤 프로그램은 잘 돌아가다가 MIPS에 가서 돌리면 안되는 경우가 있는데 이를 고려를 잘 해 주어야 한다. 거의 그런일 없을 것이다. memory가 충분하여. 대부분 little endian을 사용한다.











