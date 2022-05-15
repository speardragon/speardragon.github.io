---
layout: single
title: "[System Programming] 10-1장. 메모리 관리"
categories: ['System', 'System Programming']
tag: ['memory', 'variable']
---

<br>

# 10.1 변수와 메모리

## 프로세스

- 프로세스는 실행중인 프로그램이다. 
- 프로그램 실행을 위해서는 
  - 프로그램의 코드, 데이터, 스택, 힙, U-영역 등이 필요하다. 
- 프로세스 이미지(구조)는 메모리 내의 프로세스 레이아웃 
- 프로그램 자체가 프로세스는 아니다 !

<br>

## The Process Address Space

- Linux virtualizes its physical resource of memory 
- Processes do not directly address physical memory. 
- Instead, the kernel associates each process with a unique **virtual** **address space**. 
- This address space is **linear**, with addresses starting at zero, and increasing to some maximum value 
- Pages and Paging 
  - The virtual address space is composed of **pages**. 
  - The system architecture and machine type determine the size of a page, which is fixed; typical sizes include 4 KB (for 32-bit systems), and 8 KB (for 64-bit systems)

<br>

## Paging Model

![image](https://user-images.githubusercontent.com/79521972/168198658-5f67ba47-8f05-46c7-b542-197692732d02.png)

<br>

## 프로세스 이미지 구조 (주소공간)

- 프로세스 구조

![image](https://user-images.githubusercontent.com/79521972/168198905-d71739aa-fccf-4f5d-8986-aae32c569db3.png)

- 코드 세그먼트(code segment) 
  - 기계어 명령어 
- 데이터 세그먼트(data segment) 
  - e.g. int maxcount = 99; (initialized) 
  - e.g. long sum[1000]; (uninitialized) 
- 스택(stack) 
  - 지역 변수, 매개 변수, 반환주소, 반환값, 등 
- 힙(heap) 
  - 동적 메모리 핛당 
  - malloc() in C, 
  - new class() in Java

<br>

## 프로세스 이미지 세그먼트

- Text segment contains 

  - The program code, string literals, constant variables, and other read-only data. 

  - In Linux, this segment is marked read-only 

- Data segment contains 
  - initialized global variables. (함수 밖 선얶) 
  - Static variables (함수 안, 밖 선얶) 
- bss segment contains 
  - uninitialized global variables. 
  - These variables contain special values (essentially, all zeros)

- Stack contains 
  - the process’ execution stack (stack frame), which grows and shrinks dynamically as the stack depth increases and decreases. 
  - The execution stack contains local variables and function return data. 
- Heap contains 
  - a process’ dynamic memory. 
  - This segment is writable and can grow or shrink in size. 
  - This is the memory returned by malloc( )

<br>



























