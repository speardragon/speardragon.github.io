---
layout: single
title: "[Computer Architecture] week 4-1."
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture', 'Intro']
---



# Review)

![image](https://user-images.githubusercontent.com/79521972/160535013-589fec78-5b74-42eb-9ff4-85c18839f01a.png)

스택 사용 위치를 알려주는 stack pointer가 존재, stack pointer는 항상 스택 메모리에 제일 마지막 값을 가리킨다.

<br>



![image](https://user-images.githubusercontent.com/79521972/159404503-407c2aaa-c693-4704-90f7-6826b5397d92.png)

- t: temporary
  - 임시로 쓰기 위한 변수이다.

- s: saved
  - s의 내용은 중요한 변수 함부로 바꾸거나 할 수 없다.

- at
  - 



<br>

---



## Array

- Access large amounts of similar data
- Index: access each element
- Size: number of elements

- 5-element array
- **Base address** = = 0x12348000 (address of first element, array[0])
- First step in accessing an array: load base address into a register

![image](https://user-images.githubusercontent.com/79521972/160526538-f30c97be-8de8-46e8-9198-ed26594a4fc4.png)

## Accessing Arrays

```c
// C Code
int array[5];
array[0] = array[0] * 2;
array[1] = array[1] * 2;
```



```assembly
# MIPS assembly code
# array base address = $s0
lui $s0, 0x1234 		# 0x1234 in upper half of $S0
ori $s0, $s0, 0x8000 	# 0x8000 in lower half of $s0

lw $t1, 0($s0) 			# $t1 = array[0]
sll $t1, $t1, 1 		# $t1 = $t1 * 2
sw $t1, 0($s0) 			# array[0] = $t1

lw $t1, 4($s0) 			# $t1 = array[1]
sll $t1, $t1, 1 		# $t1 = $t1 * 2
sw $t1, 4($s0) 			# array[1] = $t1
```



<br>

## Array using For Loops

```c
// C Code
int array[1000];
int i;

for (i=0; i < 1000; i = i + 1)
	array[i] = array[i] * 8;
```



```assembly
# MIPS assembly code
# $s0 = array base address, $s1 = i
# MIPS assembly code
# $s0 = array base address, $s1 = i
# initialization code
lui $s0, 0x23B8 		# $s0 = 0x23B80000
ori $s0, $s0, 0xF000 	# $s0 = 0x23B8F000
addi $s1, $0, 0			# i = 0
addi $t2, $0, 1000 		# $t2 = 1000

loop:
    slt $t0, $s1, $t2 		# i < 1000?
    beq $t0, $0, done 		# if not then done
    sll $t0, $s1, 2 		# $t0 = i * 4 (byte offset)
    add $t0, $t0, $s0 		# address of array[i]
    lw $t1, 0($t0) 			# $t1 = array[i]
    sll $t1, $t1, 3 		# $t1 = array[i] * 8
    sw $t1, 0($t0) 			# array[i] = array[i] * 8
    addi $s1, $s1, 1 		# i = i + 1
    j loop 					# repeat
done:
```



<br>

## Function Calls

- Caller: calling function (in this case, main)
- Calle: called function (in this case, sum)

```c
//C Code
void main()
{
    int y;
    y = sum(42, 7);
    ...
}

int sum (int a, int b)
{
    return (a + b);
}
```

- $ra를 통해 다시 돌아오고 반환할 때는 $v0에 담아서 반환한다.



<br>

## Function Conventions

- Caller:
  - passes arguments to callee
  - jumps to callee
- Callee:
  - **performs** the function
  - **returns** result to caller
  - **returns** to point of call
  - must not overwrite registers or memory needed by caller

j vs. jal



<br>

## MIPS Function Conventions

- Call Function: jump and link (jal , or call)
- Return from function: jump register (jr)
- Arguments: $a0 - $a3 ($4-$7)
- Return value: $v0 - $v1 ($2-$3)



<br>

## Function Calls

```c
//C Code
int main() {
    simple();
    a = b + c;
}

void simple() {
    return;
}
```



```assembly
# MIPS assembly code
0x00400200 main: jal simple  #PC + 4 -> $ra
0x00400204 add $s0, $s1, $s2
...

0x00401020 simple: jr $ra
```

jal : 다음 address(0x00400204), 즉 돌아올 자리를 $ra에 저장시키고 jump한다.

> void means that simple doesn't return a value

<br>

jal: jumps to simple (call)

- $ra = PC + 4 = 0x00400204

jr $ra: jumps to address in $ra (0x00400204) (ret)



<br>

## Input Arguments & Return Value

MIPS Conventions

- `Argument values`: $a0 - $a3 
- `Return value`: $v0



```c
// C Code
int main() 
{
    int y;
    ...
    y = diffofsums(2, 3, 4, 5); // 4 arguments ,$a0-$a3
    ...
}
int diffofsums(int f, int g, int h, int i)
{
    int result;
    result = (f + g) - (h + i);
    return result; // return value
}
```



```assembly
# MIPS assembly code
# $s0 = y

main:
	...
	addi $a0, $0, 2 # argument 0 = 2
    addi $a1, $0, 3 # argument 1 = 3
    addi $a2, $0, 4 # argument 2 = 4
    addi $a3, $0, 5 # argument 3 = 5
    jal diffofsums # call Function
    add $s0, $v0, $0 # y = returned value
    ...
# $s0 = result
diffofsums:
    add $t0, $a0, $a1 # $t0 = f + g
    add $t1, $a2, $a3 # $t1 = h + i
    sub $s0, $t0, $t1 # result = (f + g) - (h + i)
    add $v0, $s0, $0 # put return value in $v0
    jr $ra # return to caller
```

s0에 담기는 내용은 $ra 이후에 사라지는가?

<br>



만약 main에서 t0, t1, s0를 이미 사용했다면 즉 4개가 넘으면 stack memory을 이용한다.

```assembly
# MIPS assembly code
# $s0 = result
diffofsums:
    add $t0, $a0, $a1 	# $t0 = f + g
    add $t1, $a2, $a3 	# $t1 = h + i
    sub $s0, $t0, $t1 	# result = (f + g) - (h + i)
    add $v0, $s0, $0 	# put return value in $v0
    jr $ra 				# return to caller
```

- diffofsums **overworte** 3 register: $t0, $t1, $s0
- diffofsums can use **stack** to temporarily store registers



<br>



## The Stack

- Memory used to temporarily save variables

- Like stack of dishes, last-in-first-out(LIFO) queue
- Expands: uses more memory when more space needed
- Contracts: uses less memory when the space is no longer needed(pop)

<br>

- 스택은 위에서부터 내려오는 방식, 힙은 아래에서 위로 올라가는 방식

- 스택을 사용하기 전에는 쓸 만큼 stack pointer를 증가 시켜서 다음 빈 스택을 가리켜야 하기 때문에 stack pointer를 이동시켜야 한다.
- 즉, stack pointer 전까지의 공간은 누가 사용하고 있는 것

<br>

- Grows down (from higher to lower memory address)
- Stack pointer: $sp points to top of the stack

![image](https://user-images.githubusercontent.com/79521972/160535683-c7461610-e7b9-45bf-b6f8-ffcbf52e676e.png)

<br>

## How Functions use the Stack

- Called functions must have no unintened side effects
- But diffofsums overwrites 3 registers: $t0, $t1, $s0

```assembly
# MIPS assembly
# $s0 = result
diffofsums:
	add $t0, $a0, $a1 	# $t0 = f + g
	add $t1, $a2, $a4 	# $t1 = h + i
	sub $s0, $t0, $t1 	# result = (f + g) - (h + i)
	add $v0, $s0, $0 	#put return value in $v0
	jr $ra 				#return to caller
```



## Storing Register Values on the Stack

- push(sw), pop(lw)

```assembly
# $s0 = result
diffofsums:
addi $sp, $sp, -12 	# make space on stack
					# to store 3 registers
sw $s0, 8($sp) 		# save $s0 on stack
sw $t0, 4($sp) 		# save $t0 on stack
sw $t1, 0($sp) 		# save $t1 on stack
add $t0, $a0, $a1 	# $t0 = f + g
add $t1, $a2, $a3 	# $t1 = h + i
sub $s0, $t0, $t1 	# result = (f + g) - (h + i)
add $v0, $s0, $0 	# put return value in $v0
lw $t1, 0($sp) 		# restore $t1 from stack
lw $t0, 4($sp) 		# restore $t0 from stack
lw $s0, 8($sp) 		# restore $s0 from stack
addi $sp, $sp, 12 	# deallocate stack space
jr $ra 				# return to caller
```

---

Q)$t1,t0, s0에다가 스택 내용을 저장하고 스택 pointer만 옮기는 것이기 때문에 사실상 스택 안에는 그 내용이 유지되어 있는 것인데 스택 포인터 아래에 있는 내용은 그냥 빈 공간 취급하는 것인가?

A) 맞음 override

Q)subroutine에서 모든 걸 다 저장해야하는가?

- 그래서 t와 s로 구분한 것



---



### The stack during diffofsums Call

![image](https://user-images.githubusercontent.com/79521972/160536148-de64e9b7-f0d9-49f6-9ec8-4249cadc2669.png)

<br>



## Register



![image](https://user-images.githubusercontent.com/79521972/160529523-d2f1af63-f092-47f4-bf08-e89bbf71f03a.png)

s로 시작하는 데이터는 중요한 데이터이기 때문에 다른 function에서 사용후 값을 다시 되돌리는 callee-saved 방식이고 t로 시작하는 데이터는 temporary 데이터이기 때문에 다른 function에서 마음대로 값을 바꿀 수 있기 때문에 main 함수에서 저장해야 하는 caller-saved 방식이다.



```assembly
# $s0 = result
diffofsums:
    addi $sp, $sp, -4 # make space on stack to 
    			      # store one register
    sw $s0, 0($sp)    # save $s0 on stack
    				  # no need to save $t0 or $t1
    add $t0, $a0, $a1 # $t0 = f + g
    add $t1, $a2, $a3 # $t1 = h + i
    sub $s0, $t0, $t1 # result = (f + g) - (h + i)
    add $v0, $s0, $0  # put return value in $v0
    lw $s0, 0($sp)    # restore $s0 from stack
    addi $sp, $sp, 4  # deallocate stack space
    jr $ra 			  # return to caller
```

그래서 t의 내용은 function에서 저장하지 않아도 되기 때문에 `addi $sp, $sp, -12`가 아니라`addi $sp, $sp, -4` 이고 (s내용만 저장) 

Q)어? 근데 t0가 이미 있어서 스택에 저장해야 되는 것 아닌가?

A) 그게 아니라 t0 내용은 main함수가 아닌 다른 함수에서 마음대로 사용해도 되는 메커니즘이기 때문에 굳이 스택을 만들어 줄 필요없이 0부터 사용하고 싶은대로 사용하는 것이다.

<br>



---

f1()으로 jump하기 전에 $ra에 *을 저장하고 jump하고 f1()함수가 종료할 때 `jr $ra`을 통해 caller로 돌아가게 된다.

그런데 만약 f1() 안에 f2()라는 함수를 호출하면 또 f2로 jump하기 전에 $ra에 f2의 return address가 담기게 되는데 f2가 종료시에 `jr $ra`(f1()함수의)로 돌아가고 f1에서 함수 종료시 $ra 명령어로 main으로 돌아가야 하는데 현재 $ra에는 f2의 주소가 담겨있기 때문에 이는 stack을 사용하여 해결한다!



즉, main에서 f1()으로 jal할 때 $ra 값이 stack에 push 되었다가 f2()가 호출될때는 $ra에 overriding되어도 f1()에 다시 돌아와서 return address를 할 때에는 stack의 내용을 pop하면 main의 return address값을 가져올 수 있는 것이다.



<br>

---

## Multiple Function Calls

```assembly
proc1:
    addi $sp, $sp, -4 	# make space on stack
    sw $ra, 0($sp) 		# save $ra on stack(push)
    jal proc2 			# override current return address to $ra
    ...
    lw $ra, 0($sp) 		# restore $s0 from stack
    addi $sp, $sp, 4 	# deallocate stack space(원상복귀)
    jr $ra 				# return to caller
```





<br>

## Recursive Function Call

- Function that calls it self

- When convertint to assembly code:

  - In the first pass, treat recursive calls as if it's calling a different function and ignore overwritten registers.
  - Then save/restore registers on stack as needed.

- ex) Factorial function

  - factorial(n) = n!

    - =n*(n-1)\*(n-2)\*(n-3) ... \*1

  - Example: factorial(6) = 6!

    =6\*5\*4\*3\*2\*1

    =720

<br>

```c
// High-level code
int factorial (int n) {
    if (n <= 1)
        return 1;
    else
        return (n * ractorial(n-1));
}
```

![image](https://user-images.githubusercontent.com/79521972/160531382-2afbfc0d-32ba-411f-beb9-1604e0e46cb7.png)

<br>

```assembly
factorial:
    addi $sp, $sp, -8 	# save regs
    sw $a0, 4($sp)
    sw $ra, 0($sp)
    addi $t0, $0, 2
    slt $t0, $a0, $t0 	# a <= 1 ?
    beq $t0, $0, else 	# no: go to else
    addi $v0, $0, 1 	# yes: return 1
    addi $sp, $sp, 8 	# restore $sp
    jr $ra 				# return
else:
    addi $a0, $a0, -1 	# n = n - 1
    jal factorial 		# recursive call
    lw $ra, 0($sp) 		# restore $ra
    lw $a0, 4($sp) 		# restore $a0
    addi $sp, $sp, 8 	# restore $sp
    mul $v0, $a0, $v0 	# n*factorial(n-1)
    jr $ra 				# return
```

<img src="https://user-images.githubusercontent.com/79521972/160536462-7c002ba7-92f5-42c5-861e-406f608ed500.png" alt="image" style="zoom:67%;" />

<br>

## Stack During Recursive Call

![image](https://user-images.githubusercontent.com/79521972/160536690-6a247284-83d2-412f-950c-53e5d81ee3fc.png)



<br>

## Function Call Summary

- Caller
  - Put arguments in $a-$a3
  - Save any needed registers ($ra, maybe $t0-t9)
  - jal callee
  - Restore registers
  - Look for result in $v0

- Callee
  - **s register만 저장**
  - Save registers that might be distrubed ($s0-$s7)
  - Perform function
  - Put result in $v0
  - Restore registers
  - jr $ra



<br>

## Addressing Modes

How do we address the operands?

- Register Only
  - add $5, $6, $7

- Immediate
  - addi $5, $6, -3
- Base Addressing
  - lw $1, 4($2) `<- data memory`
- PC-Relative
  - beq $1, $2, Label   `<- prog. mem.`
- Pseudo Direct
  - J Label   `<- prog. mem.`



