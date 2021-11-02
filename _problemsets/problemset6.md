---
layout: academic
permalink: /problemsets/problemset6
title: Problem Set 6
description: Week 9 practice questions containing topics from Assembler and Compilers, Stack and Procedure.
---

* TOC
{:toc}

**50.002 Computation Structures**
<br>
Information Systems Technology and Design
<br>
Singapore University of Technology and Design

# Problem Set 6

This page contains all practice questions that constitutes the topics learned in <ins>Week 9</ins>:  **Assemblers and Compilers**, **Stack and Procedure**.

Each topic's questions are grouped into **three** categories: basic, intermediate, and challenging. You are recommended to do all basic problem set before advancing further. 


## Byte Memory Array Loading (Basic)
The memory is loaded as a byte array `{14, 00, 3F, 60, 18, 00, 5F, 60, 00, 10, 01, 80, 1C, 00, 1F, 64, 00, 00, 00, 00, 02, 00, 00, 00, 04, 00, 00, 00, 00, 00, 00, 00}` before starting execution. 

For example, this means that `MEM[0] = 0x14` and `MEM[2] = 0x3F` and so on.

What is the word at memory address `0xC`? And what is the word at memory address `0x14`? 

<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
<code>MEM[0xC] = 64 1F 00 1C</code>, and <code>MEM[0x14] = 00 00 00 02</code>.
</p></div><br>



## The Remainder (Basic)
 
 Refer to the C-code below:
```
int f(int a, int b){
	 if (a<b) return [6];
	 else return f(a-b, b);
} 
```

The above C-code compiles to the following Beta assembly code:
```
F:	PUSH(LP)
	PUSH(BP)
	MOVE(SP, BP)
	PUSH(R1)
	PUSH(R2)
	LD(BP, -12, R0)			| #1
	LD(BP, -16, R1)
	CMPLT([4A], [4B], R2)	

LL01: BNE(R2, LL02)			| #2
	  SUB(R0, R1, R0)
	  PUSH(R1)
	  PUSH(R0)
	  BR(F, LP)				| #3
	  DEALLOCATE(2)

LL02: POP([5a])
      POP([5b])
      MOVE(BP, SP)
      POP(BP)
      POP(LP)
      JMP(LP)
```

1. You are given a C function code and its equivalent compiled beta assembly code above. What is the 32-bit binary instruction code of “LD(BP,-12,R0)” (instruction #1)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>0110 0000 0001 1011 1111 1111 1111 0100</code>
	</p></div><br>

2. What is the 32-bit binary instruction code of “BNE(R2,LL02)” (instruction #2)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>0111 1011 1110 0010 0000 0000 0000 0111</code>
	</p></div><br>

3. What is the 32-bit binary instruction code of “BR(F,LP)” (instruction #3)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>0111 0111 1001 1111 1111 1111 1110 1101</code>
	</p></div><br>
4. Complete the assembly code line “CMPLT([4a],[4b],R2)”.
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>CMPLT(R0,R1,R2).</code>
	</p></div><br>
5. Complete the assembly code lines “POP([5a])” and “POP([5b])”.	
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>POP(R2), POP(R1)</code>
	</p></div><br>
	
6. What is the return value marked by the placeholder [6] in the C code?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The content of variable <code>a</code>.
	</p></div><br>
	
7. What is the result of the (potentially recursive) function call “f(7,4)”?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The result is 3. 
	</p></div><br>
	
8. If the stack pointer register SP contains the address 0x00001004 before the function f is called (e.g. here, by the instruction “BR(F,LP)”, then what is value of SP before the subsequent instruction in the code is executed (e.g. here, the instruction “DEALLOCATE(2)”).
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<code>Reg[SP] = 0x00001004</code>
	</p></div><br>




## The GCD (Intermediate)
Consider the following implementation of an algorithm for finding the greatest common divisor of two integers **recursively**:

``` 
int gcd(int a,int b) { 
	if (a == b) return a; 
	if (a > b) return gcd(a-b,b); 
	return gcd(a,b-a); 
} 
```
The C compiler has compiled this procedure into the following code for an unpipelined Beta processor:

``` 
gcd: PUSH (LP) 
PUSH (BP) 
MOVE (SP, BP) 

PUSH (R1) 
PUSH (R2) 
LD (BP, -12, R0) 
LD (BP, -16, R1) 

CMPEQ (R0, R1, R2) 
BT (R2, L1) 
CMPLE (R0, R1, R2) 
BT (R2, L2) 
PUSH (R1) 
SUB (R0, R1, R2)
PUSH (R2) 
BR (gcd, LP) 
DEALLOCATE (2) 
BR (L1) 

L2: SUB (R1, R0, R2) 
PUSH (R2) 
PUSH (R0) 
BR (gcd, LP) 
DEALLOCATE (2)

L1: POP (R2) 
POP (R1) 
MOVE (BP, SP) 
POP (BP) 
POP (LP) 
JMP (LP)
```

Answer the following questions. **For convenience sake, although all data is in 32-bit format, for this answer, we omit the leading zeroes**.

1. The program above contains the instruction `LD(BP,-16,R1)`. Explain what the compiler was trying to do when it generated this instruction.
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Recall that BP - 12 is always the first argument of the function. Hence one
	word / one line above (BP - 12), i.e: BP - 16 is the second argument of the
	function. The compiler is loading the value of the second argument (”b”) so it could compute "a == b" later on.</p></div><br>


2. What are the contents of the memory location holding the instruction BR(L1)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	’L1’ is a label. Recall that to quickly compute literal you can count the number of instruction lines between BR (the instruction that uses "label") and the "label" instruction, and subtract it by 1, then convert it to 16 bits. Remember that <strong>PUSH is actually a macro that expands into two Beta instructions</strong>, so there are 7 instructions between L1 and L2:
	<ul>
	<li> The location of L1, from the assembly code above, is located 1 (from SUB(R1, R0,R2) + 2 (from PUSH(R2)) + 2 (from PUSH(R0)) + 1 (from BR(gcd, LP)) + 1 (from DEALLOCATE(2)) + 1 (from POP(R2), which is the location of L1)= 8 lines (8 words) away from the instruction BR(L1).</li><br>
	<li> Therefore the 16-bit ’literal’ value for the 32-bit instruction of BR(L1) is 8 - 1 = 7, which is <code>0000 0000 0000 0111</code>. </li><br>
	<li>The OPCODE for BR : BEQ(R31, L1, R31) is 011101. Ra is R31: 111111, and Rc is also R31: 111111.</li><br>
	<li> Hence the 32-bit instruction of BR(L1) is : <code>011101 11111 11111 0000 0000 0000 0111 = 0x77FF 0007</code></li>
	</ul></p></div><br>

3. When the instruction labeled ”L1” is executed, what is the best characterization of the contents of R0?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	R0 is a <strong>reserved</strong> register for return value in Beta function linkage procedure. Since L1 is just at the beginning of the procedure return sequence, so at that point R0 should contain the value to be returned to the caller. </p></div><br>

4. Looking at the code, a student suggests that both `DEALLOCATE` instructions could be eliminated since deallocation is performed implicitly by the `MOVE(BP,SP)` instruction in the exit sequence. After calling `gcd`, would it be possible to tell if the `DEALLOCATE` instructions had been removed?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<strong>Yes, it would be possible to tell</strong>. Even though the stack is reset correctly by the MOVE(BP,SP) instruction, that happens after the POP(R1) and POP(R2) instructions, so they will not restore the values of R1 and R2 from the values pushed onto the stack during the entry sequence.
	</p></div><br>

5. How many words of stack are needed to execute `gcd(24,16)`? Don't forget to **include** the stack space occupied by the **arguments** in the **initial call.**

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The recursive calls are: <code>gcd(24,16) ---> gcd(8,16) ---> gcd(8,8)</code>, at which point no more recursive calls are made. So three nested procedure calls are executed, each with a 6-word stack frame (2 args, LP, BP, saved R1, saved R2), for a total of 18 words of stack space.
	</p></div><br>
	
During execution of `gcd(28,70)`, the Beta processor is halted and the contents of portions of the stack are found to contain the following: 
<img src="https://dl.dropboxusercontent.com/s/z954a19n78di4od/1.png?raw=1" width="50%" height="50%">




1. What is the value of the second argument (`b`) to the **current** call to `gcd`? 
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The second argument is located at <code>Mem[Reg[BP] - 16] = 0xE</code>.
	</p></div><br>

3. What is the value in the `BP` register at the time the stack snapshot was taken?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p><code>0x1264</code>. This is computed from the value of old BP stored in <code>Mem[Reg[BP] -4]</code> + the size of one stack frame.  <code>Mem[Reg[BP] -4] = 0x124C</code>. The size of 1 stack frame is 6 words = 24 bytes = <code>0x18</code> bytes. Therefore, <code>0x1264</code> = <code>0x124C + 0x18</code>.</p></div><br>

2. Also, what is the correct value for "???" above in the snapshot of the stack?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The recursive call is: <code>gcd(28,70) ---> gcd(28,42) ---> gcd(28,14)</code>, at which point the snapshot was taken. So ”???” is the first argument to the latest preceding call, i.e., 28.
	</p></div><br>


4. What is the address of the `POP(R2)` instruction?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	All calls in the execution of gcd(28,70) have a < b, so all the recursive calls are from the second BR(gcd,LP). Using the value of LP stored at Mem[Reg[BP] - 8] which points at the DEALLOCATE instruction, the POP(R2) is stored at location <code>0x594 + 4 = 0x598</code>. </p></div><br>

5. At the time the stack snapshot was taken, what is the significance of the value `0x1254` in the location at SP?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Since SP points to the first unused location on the stack, the value in the location SP points to has <strong>no significance</strong> to the process, it’s just ununsed stack space.
	</p></div><br>

6.  The stack snapshot was taken just after the execution of a particular instruction. Could the snapshot have been taken just after the execution of the `PUSH(R1)` instruction near the beginning of the gcd procedure?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>No. Given the values of BP and SP, the PUSH(R2) instruction must have already been executed. </p></div><br>


## Beta Assembly Exercise (Intermediate)

**Part 1:**

As a warm-up, hand assemble the following instructions:
```
ADDC(R1,128,R7)
LD(R11,-4,R0)
ST(R0,-4,R11)
BEQ(R9,0x1C,R27) | this code is written at memory address 0x10
```
<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p><pre>
<code>ADDC(R1,128,R7): 0xC0E10080
LD(R11,-4,R0): 0x600BFFFC
ST(R0,-4,R11): 0x640BFFFC
BEQ(R9,0x1C,R27): 0x77690002</code></pre> </p></div><br>

**Part 2:**

The following C code is compiled into byte code:
```
int f(int x, int *y){
	static int a=0;
	int b=0;
	while(b<x){
		 a=a+y[b];
		 b=b+1;
	}
	return a;
}
```
```
int main(int argc, char **argv){
	int z[5]={1,2,3,4,5};
	int r1, r2;
	r1 = f(3,z);
	r2 = f(1, z+1); // z+1 is the address of the second element of z
}  
```  

Refering to the C code in Part 2, state whether each of the following is stored in the: stack, data, or code part of the memory. 
> Note: *Data* is a space in the memory dedicated for global variables, static variables (initialized and uninitialized).  Dynamically allocated variables are stored in another segment called the "heap". Memory management is also language specific, but the general idea is pretty much the same. We will learn more about this in the next term.

* Where is **z** stored?
* Where is **x** stored?
* Where is **a** stored?
* Where is **b** stored?
* Where is **f** stored?
* Where is **r1** stored?

Finally, what is the value of `r1` and `r2` after the program finishes?
<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
z: stack<br>
x: stack<br>
a: data<br>
b: stack<br>
f: code<br>
r1: stack<br><br>
Value of r1 = 6 and r2= 8 after the program finishes. 
</p></div><br>

**Part 3:**

When the above C code is compiled into byte code, it is equivalent to the following assembly code:  
```
f: 
 PUSH(LP)  
 PUSH(BP)  
 MOVE(SP,BP)  
 PUSH(R1)  
 PUSH(R2)  
 PUSH( <I> ) | you need to fill in what <I> is  
 CMOVE(0,R0)  
 PUSH(R0)  
 
while001:  
 LD(SP,-4,R1)  
 LD(BP,-12,R2)  
 CMPLT(R1,R2,R5) 
 <CD1>(R5,endwhile001) | you need to fill in what <CD1> is  
 LD(BP,-16,R1)  
 LD(SP,-4,R2)  
 MULC(R2,4,R2)  
 ADD(R1,R2,R1)  
 
label1:  
 LD(R1,0,R5)  
 LD(R31,stcvar_001,R1)  
 ADD(R1,R5,R2)  
 ST(R2,stcvar_001,R31)  
 LD(SP,-4,R2)  
 
label2:  
 <CD2>(R2,1,R2)  
 ST(R2,-4,SP)  
 BR(while001)  
 
endwhile001:  
 LDR(stcvar_001,R0)  
 DEALLOCATE(1)  
 POP(<I>) | you need to fill in what <I> is
 POP(<II>) | you need to fill in what <II> is
 POP(<III>) | you need to fill in what <III> is

 POP(BP)  
 POP(LP)  
 JMP(LP)
 ```

	
1. After executing the assembly code in Part 3 at `while001`, i.e. `LD(SP,-4,R1)`, what does `R1`'s content represent in the C code's variables in Part 2?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Reg[R1] represents b. 
	</p></div><br>
	
2. After executing the assembly code in Part 3 at line **AFTER** `while001`, i.e. `LD(BP,-12,R2)`, what does `R2`'s content represent in the C code's variables in Part 2?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Reg[R2] represents x. 
	</p></div><br>
	
3. What should `<I>` be in that line of assembly code? 
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	R5.
	</p></div><br>
	
4. What does `R5`'s content represent at memory location `label1`? Look at Part 2 to find the variable name.
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	y[b].
	</p></div><br>
	
5. Which variable in Part 2 correspond to `stcvar_001`?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	a.
	</p></div><br>
	
6. What should `<CD1>` be in the assembly code?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	BEQ.
	</p></div><br>
	
7. What should `<CD2>` be in the assembly code?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	ADDC.
	</p></div><br>
	
8. What should `<II>` be in the assembly code?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	R2.
	</p></div><br>
	
9. What should `<III>` be in the assembly code?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	R1.
	</p></div><br>



