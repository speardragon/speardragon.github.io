---
layout: single
title: "[Computer Architecture] Single Cycle MIPS microarchitecture - control"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



## Control

control 신호들이 명령어에 해당하는 값들로 세팅됨.



**control 방식**

- mux에서 하는 control
- write assert 방식
  - memory 혹은 register에서 write을 할 건지 말 건지를 경정하는 control


<br>

두 개(memory, register) 빼고는 다 mux control



<br>

### Single-Cycle Control

![image](https://user-images.githubusercontent.com/79521972/162873204-0a718faf-d08c-49dd-9250-246f5b76b0a3.png)

- 먼저 opcode를 보고 어떤 명령어 인지를 본다.(lw, sw, addi ....)
- 그 명령어에 필요한 control signal을 쫙 보낸다.
- ALU decoder(sub control)에 들어가서  +,/, & 와 같은 명령은 새끼 control에서 하게끔 하여 두 level로 나누었다.



<br>

## ALU

![image](https://user-images.githubusercontent.com/79521972/162873413-18b5c2ad-8776-4809-a874-0cb03546a032.png)





<br>

![image](https://user-images.githubusercontent.com/79521972/162873458-3abb870f-b1c6-4d31-aa3c-252c14dee602.png)

![image](https://user-images.githubusercontent.com/79521972/162873482-527b374b-6c22-4245-98ae-b2a9996fb346.png)





<br>

## Review: Sequential Elements(f/f)

- Register: stores data in a circuit
  - Uses a clock signal to determine when to update the stored value
  - **Edge-triggered**: update when CLK changes from 0 to 1



![image](https://user-images.githubusercontent.com/79521972/162873710-138c898c-b369-47ee-9d76-87ff5d7f52f1.png)

<br>

- Register with write control
  - Only updates on clock edge **when write control input is 1**
  - Used when stored value is required later

![image](https://user-images.githubusercontent.com/79521972/162874335-cf57954d-1cbc-4cea-ab8d-39b1a4b53509.png)



<br>

## Clocking Methodology

- Combinational logic transforms data during clock cycles
  - Between clock edges
  - Input from state elements, output to state element
  - Longest delay determines clcok period

![image](https://user-images.githubusercontent.com/79521972/162874701-cfb5b0ac-0860-4e5c-bf60-5906f9af907b.png)

- clock edge가 rising edge가 되면 state element(register)에 값이 update됨.



명령어 중에 data memory와 register file이 동시에 write enable되는 경우는 없음.

<br>

## Register File



![image](https://user-images.githubusercontent.com/79521972/162875636-e0812050-bec3-476f-a77d-aa119170d3a4.png)



빨간 점선은 같은 부분이다.

왼쪽은 write port(A3, WD3) 오른쪽은 read port(A1, A2, RD1, RD2)를 나타낸다.



<br>

```
module regfile(input logic clk, 
            input logic we3, 
            input logic [4:0] ra1, ra2, wa3, 
            input logic [31:0] wd3, 
            output logic [31:0] rd1, rd2);
            
	logic [31:0] rf[31:0];
	
    // three ported register file : read two ports combinationally
    // write third port on rising edge of clk
    // register 0 hardwired to 0
    // note: for pipelined processor, write third port on falling edge of clk
    
    always_ff @(posedge clk)
    	if (we3) rf[wa3] <= wd3; //synch write
    
    assign rd1 = (ra1 != 0) ? rf[ra1] : 0;
    assign rd2 = (ra2 != 0) ? rf[ra2] : 0;
endmodule
```



<br>

### HDL description

![image](https://user-images.githubusercontent.com/79521972/162876097-c37ce601-8e21-4eef-bca0-1b37e905e27f.png)

- imem은 실행 중에 바뀌지 않는 ROM 모델이다.(load)
- clock에 의해 실행 되는 것이 아닌 asycn이다.

<br>



![image](https://user-images.githubusercontent.com/79521972/162876110-1830bfac-3815-467c-a2b4-8168a08ae6c2.png)

- asych read

- clk에 동기되어 write되는 synch
- 실행 중에 data가 계속 바뀜(RAM)



<br>

## The Main Control Unit

- Control signals derived from instruction



<br>

## Control Unit: ALU Decoder

![image](https://user-images.githubusercontent.com/79521972/162876610-a89995fb-4e73-465b-87bd-81a293d92186.png)

- 00: lw, sw
- 01: beq
- 10: R-Type



![image](https://user-images.githubusercontent.com/79521972/162876634-8699ffd4-139a-4c50-a6ad-1a3261da6a72.png)





<br>

## Control Unit Main Decoder

![image](https://user-images.githubusercontent.com/79521972/163663142-62fff2c2-cd42-417e-a900-65c55c021cb4.png)

<span style="color:red"> 시험 문제</span>

![image](https://user-images.githubusercontent.com/79521972/162877284-5154e99a-14d4-4106-a2d6-a3ecd45efa5e.png)

![image](https://user-images.githubusercontent.com/79521972/162878122-6bbb7141-9b08-42d0-adde-90809dfdfcea.png)

![image](https://user-images.githubusercontent.com/79521972/162878135-4c047c7e-a868-460a-b365-03d4de2d6dbf.png)

RegWrite 0이면 register write port에 무슨 값이 들어와도 어차피 write 할 것이 아니기 때문에 MemtoReg는 don't care이다.



Q) j도 000100???



<br>

## Extended Functionality: j

![image](https://user-images.githubusercontent.com/79521972/163292914-c4d36661-2e78-4779-9e67-e447a72ae397.png)

우리는 명령어의 길이가 32bit라고 알아왔었는데 jump 명령에서는 주어지는 다음 address 값이 26bit이므로 PC 값에 들어가기에 부적절하다. 그래서 다음과 같은 addressing 과정이 더해진다.

**addressing**

한 명령어의 크기는 4btye이다(32bit). 명령어의 시작 주소가 0이라면 다음 주소는 4, 그 다음 주소는 8이 되는 것이다. 이 때 <span style="color:red">잘 보면 명령어의 LSB 0 번째와 1 번째 bit는 항상 0이라는 것을 알 수 있다.</span>

- 4 = 0100 , 8 = 1000

> 즉 명령어는 xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx00 과 같은 구조로 이루어져 있다.

따라서 우리는 저 x의 부분만 안다면 전체 명령어를 PC 값으로 만들어 낼 수 있는 것이다. 

저 x부분이 우리가 

<br>

26비트의 imme 값이 left shift 하면 x4(메모리 한 주소당 4byte) 를 하여 28비트가 되고 그 값이 중간에 PC 값과 concatanate 되어 해당 값이 BTA가 되어 이 주소로 이동하게 된다.

<br>

branch는 offset이라 0~15bit로 * 4 해서 PC에 더해주었지만 j와 jal은 **26bit 전체**가 절대주소가된다. 

따라서 25-0 bit를 따로 빼줘서 32bit로 크기 맞춰주고 그게 PC값이 되어야한다.

 <br>

그렇다면 26bit 짜리놈을 어떻게 32bit로 만들까?

25bit 짜리를 일단 shift left 2를 한다. 어차피 memory에서는 4byte 단위로 잘라먹으니까 하위 2bit는 무조건 00이다. 따라서 명령어로 교환할때는 2bit를 뗀상태로 교환하고 마지막에 붙치는 형식.

그리고 상위 4bit는 현재 PC값의 상위 4bit에서 가져온다. 그렇게하면 2<sup>28</sup> = 256MB 범위의 주소를 이동할 수 있다.

 



![image](https://user-images.githubusercontent.com/79521972/163298649-27d1ac3e-ea5f-4cc2-a6fe-992d36fd41c7.png)

![image](https://user-images.githubusercontent.com/79521972/163296553-cf666f72-eb09-4f91-8183-6c8f59f02ea8.png)



Q) 왜 left shift를 하는데 26에서 28이 됨?

A) 

x4를 했기 때문에 뒤에 00이 붙기 때문에



Q) Target address 상위 4bit을 0000으로 사용하지 않는 이유?

Dynamic library는 프로그램 실행 중에 들어오는 부분으로, address의 시작주소 4bit는 0000이 아니다.

따라서 target address 상위 4bit을 0000으로 제한하게 되면 dynamic library로써는 사용할 수 없다.

따라서 dynamic library를 포함하여 더 넓은 범위에서 사용하기 위해서 상위 4bit을 current PC의 상위 4bit로 사용한다.



<br>

## A sample HDL Testbench

![image](https://user-images.githubusercontent.com/79521972/162879037-16169d97-36aa-426b-b611-213511f68806.png)



<br>

## A sample Test Program

![image](https://user-images.githubusercontent.com/79521972/162878940-89358d0d-71ca-4e8b-9744-0fcdd7c621dd.png)



![image](https://user-images.githubusercontent.com/79521972/162878965-4d43a20d-10df-4479-9243-bc5827fa019f.png)

<br>

































