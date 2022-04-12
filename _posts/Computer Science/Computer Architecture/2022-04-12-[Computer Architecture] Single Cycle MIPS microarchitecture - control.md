---
layout: single
title: "[Computer Architecture] Microarchitecture (Single Cycle)"
categories: ['Computer Science', 'Computer Architecture']
tag: ['Computer Architecture']
---



## Control

- mux로 control
- write assert 방식



두 개 빼고는 다 mux control



<br>

### Single-Cycle Control

![image](https://user-images.githubusercontent.com/79521972/162873204-0a718faf-d08c-49dd-9250-246f5b76b0a3.png)

- 먼저 opcode를 보고 어떤 명령어 인지를 본다.
- 그 명령어에 필요한 control signal을 쫙 보낸다.
- ALU(sub control)에 들어가서  +,/, & 와 같은 것 중에 어떤 것인지 본다.



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
  - Only updates on clock edge when write control input is 1
  - Used when stored value is required later

![image](https://user-images.githubusercontent.com/79521972/162874335-cf57954d-1cbc-4cea-ab8d-39b1a4b53509.png)



<br>

## Clocking Methodology

- Combinational logic transforms data during clock cycles
  - Between clock edges
  - Input from state elements, output to state element
  - Longest delay determines clcok period

![image](https://user-images.githubusercontent.com/79521972/162874701-cfb5b0ac-0860-4e5c-bf60-5906f9af907b.png)

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
    if (we3) rf[wa3] <= wd3;
    
    assign rd1 = (ra1 != 0) ? rf[ra1] : 0;
    assign rd2 = (ra2 != 0) ? rf[ra2] : 0;
endmodule

```



<br>

### HDL description

![image](https://user-images.githubusercontent.com/79521972/162876097-c37ce601-8e21-4eef-bca0-1b37e905e27f.png)

- imem은 실행 중에 바뀌지 않는 ROM 모델이다.
- clock에 의해 실행 되는 것이 아닌 asycn이다.

<br>



![image](https://user-images.githubusercontent.com/79521972/162876110-1830bfac-3815-467c-a2b4-8168a08ae6c2.png)



- clk에 동기되어 write되는 RAM
- 실행 중에 data가 계속 바뀜



<br>

## The Main Control Unit

- Control signals derived from instruction



<br>

## Control Unit: ALU Decoder

![image](https://user-images.githubusercontent.com/79521972/162876610-a89995fb-4e73-465b-87bd-81a293d92186.png)



![image](https://user-images.githubusercontent.com/79521972/162876634-8699ffd4-139a-4c50-a6ad-1a3261da6a72.png)





<br>

## Control Unit Main Decoder

<span style="color:red"> 시험 문제</span>

![image](https://user-images.githubusercontent.com/79521972/162877284-5154e99a-14d4-4106-a2d6-a3ecd45efa5e.png)

![image](https://user-images.githubusercontent.com/79521972/162878122-6bbb7141-9b08-42d0-adde-90809dfdfcea.png)

![image](https://user-images.githubusercontent.com/79521972/162878135-4c047c7e-a868-460a-b365-03d4de2d6dbf.png)



<br>

## Extended Functionality: j







## A sample HDL Testbench

![image](https://user-images.githubusercontent.com/79521972/162879037-16169d97-36aa-426b-b611-213511f68806.png)



<br>

## A sample Test Program

![image](https://user-images.githubusercontent.com/79521972/162878940-89358d0d-71ca-4e8b-9744-0fcdd7c621dd.png)



![image](https://user-images.githubusercontent.com/79521972/162878965-4d43a20d-10df-4479-9243-bc5827fa019f.png)

<br>

































