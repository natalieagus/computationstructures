﻿---
layout: academic
permalink: /problemsets/ps8
title: Problem Set 8
description: Week 11 practice Questions
---

50.002 Computation Structures
Information Systems Technology and Design
Singapore University of Technology and Design

# Problem Set 8

This page contains all practice questions that constitutes the topics learned in <ins>Week 11</ins>:  **Virtual Memory**, **Virtual Machine**.

Each topic's questions are grouped into **three** categories: basic, intermediate, and challenging. You are recommended to do all basic problem set before advancing further. 

## Virtual Memory Trivia (Basic)
Consider a virtual memory system that uses a single-level page map to translate virtual addresses into physical addresses. Each of the questions below asks you to consider what happens when one of the design parameters of the original system is changed.

1. If the physical memory size (in bytes) is **doubled**, how does the number of bits in each entry of the page table change?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Increases by 1 bit. Assuming the page size remains the same, there are now twice as many physical pages, so the physical page number needs to expand by 1 bit.
	</p></div>

2. If the physical memory size (in bytes) is **doubled**, how does the number of entries in the page map change?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	No change. The number of entries in the page table is determined by the size of the virtual address and the size of a page – it’s not affected by the size of physical memory.
	</p></div>

3. If the virtual memory size (in bytes) is doubled, how does the number of bits in each entry of the page table change?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	No change. The number of bits in a page table entry is determined by the number of control bits (usually 2: dirty and resident) and the number of physical pages – the size of each entry is not affected by the size of virtual memory.
	</p></div>

4. If the virtual memory size (in bytes) is doubled, how does the number of entries in the page map change?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	The number of entries doubles. Assuming the page size remains the same, there are now twice as many virtual pages and so there needs to be twice as many entries in the page map.
	</p></div>

5. If the page size (in bytes) is doubled, how does the number of bits in each entry of the page table change?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Each entry is one bit smaller. Doubling the page size while maintaining the size of physical memory means there are half as many physical pages as before. So the size of the physical page number field decreases by one bit.
	</p></div>

6. If the page size (in bytes) is doubled, how does the number of entries in the page map change?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	There are half as many entries. Doubling the page size while maintaining the size of virtual memory means there are half as many virtual pages as before. So the number of page table entries is also cut in half.
	</p></div>

7. The following table shows the first 8 entries in the page map. Recall that the valid bit is 1 if the page is resident in physical memory and 0 if the page is on disk or hasn’t been allocated. If there are 1024 ($2^{10}$) bytes per page, what is the physical address corresponding to the decimal virtual address 3956?
	$$
	\begin{matrix}
	\text{Virtual Page} & \text{Valid bit} & \text{Physical page}\\
	\hline
	0 & 0 & 7\\
	1 & 1 & 9\\
	2 & 0 & 3\\
	3 & 1 & 2\\
	4 & 1 & 5\\
	5 & 0 & 5\\
	6 & 0 & 4\\
	7 & 1 & 1\\
	\hline
	\end{matrix}$$
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	3956 = 0xF74. So the virtual page number is 3 with a page offset of 0x374. Looking up page table entry for virtual page 3, we see that the page is resident in memory (valid bit = 1) and lives in physical page 2. So the corresponding physical address is (2<<10)+0x374 = 0xB74 = 2932.
	</p></div>


## Page Replacement (Basic)

Consider two possible page-replacement strategies: LRU (the least recently used page is replaced) and FIFO (the page that has been in the memory longest is replaced). The merit of a page-replacement strategy is judged by its hit ratio. Assume that, after space has been reserved for the page table, the interrupt service routines, and the operating-system kernel, there is only sufficient room left in the main memory for four user-program pages. Assume also that initially virtual pages 1, 2, 3, and 4 of the user program are brought into physical memory in that order.

1. For each of the two strategies, what pages will be in the memory at the end of the following sequence of virtual page accesses? Read the sequence from left to right: (6, 3, 2, 8, 4).
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	LRU:<br>
	start: 4 3 2 1<br>
	access 6: replace 1 => 6 4 3 2<br>
	access 3: reorder list => 3 6 4 2<br>
	access 2: reorder list => 2 3 6 4<br>
	access 8: replace 4 => 8 2 3 6<br>
	access 4: replace 6 => 4 8 2 3<br>
	<br>
	FIFO:<br>
	start: 4 3 2 1<br>
	access 6: replace 1 => 6 4 3 2<br>
	access 3: no change => 6 4 3 2<br>
	access 2: no change => 6 4 3 2<br>
	access 8: replace 2 => 8 6 4 3<br>
	access 4: no change => 8 6 4 3
	</p></div>

2. Which (if either) replacement strategy will work best when the machine accesses pages in the following (stack) order: (3, 4, 5, 6, 7, 6, 5, 4, 3, 4, 5, 6, 7, 6, ...)?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	LRU misses on pages 3 & 7, hence having 2/8 miss rate. FIFO doesn’t work well on stack accesses, as it will have 5/8 miss rate.
	</p></div>

3. Which (if either) replacement strategy will work best when the machine accesses pages in the following (repeated sequence) order: (3, 4, 5, 6, 7, 3, 4, 5, 6, 7, ...)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Both strategies have a 100% miss rate in the steady state.
	</p></div>

4. Which (if either) replacement strategy will work best when the machine accesses pages in a randomly selected order, such as (3, 4, 2, 8, 7, 2, 5, 6, 3, 4, 8, ...)?
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	Neither FIFO nor LRU is guaranteed to be the better strategy in dealing with random accesses since there is no locality to the reference stream.
	</p></div>


## VA-PA Address Translation (Basic)

Consider a virtual memory system that uses a single-level page map to translate virtual addresses into physical addresses. The following table shows the first 8 entries in the page map. If there are 1024 ($2^{10}$) bytes per page, what is the physical address corresponding to the virtual address `0x1C92`?

<img src="https://dl.dropboxusercontent.com/s/93f6hoa3nk9iyah/vapa.png?raw=1" width="50%" height="50%">

<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
The PA is <code>0x0492</code>. You can obtain it by figuring out how many bits is the VPN. Given that there's 1024 bytes per page, this gives us the clue that PO is 10 bits, which means that VPN is consisted of 6 bits: <code> 000111</code>. From the table, we find that the PPN is <code>1</code> for VPN 7. Appending PPN and PO and making it 16 bits hex representation, we have PO: <code> 0010010010 </code> and PPN: <code> 000001 </code>. Appending them together and converting them to results in <code>0x0492</code>.
</p></div>



## VM Party (Intermediate)

Refer to the state of RAM, pagetable, and TLB above. The addresses in the RAM, D, and R bits are written in binary, while the VPN, PPN, and LRU are written in Hex.

**The biggest LRU number refers to the MOST recently used item and vice versa,** and that BYTE addressing is used. The total size of the RAM is 4 PAGES and the total number of entries in the pagetable is 8. The letter A, B, C, and D written above refers to the entire page, meaning that address `000000` to `001100` belong to page A, address `010000` to `011100` belong to page B, and so on.

<img src="https://dl.dropboxusercontent.com/s/rddyib7t32636a2/VMqns.png?raw=1" width="70%" height="70%">


1. How many bits is PO, PPN, and VPN?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	#bits of PO is 4, #bits of PPN is 2, #bits of VPN is 3
	</p></div>

2. Which of the following instructions **does not** require access to the pagetable?
	*	`ST(R0, 0b1111100, R31)`
	*	`ST(R0, 0b0110100, R31)`
	*	`ADDC(R31, 0x3, R0), ST(R0, 0b0010100, R31)`
	*	`ADDC(R31, 0b1000000, R2), LD(R2, 0b0011000, R3), SHLC(R3, 0x4, R3)`
	
	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	You don't require access to the pagetable if the entry is already cached at the TLB. The second instruction and the third instruction requires translation of VPN 3 and VPN 1 which are both present in the TLB -- this means we don't need to access the pagetable anymore. 
	</p></div>

3. We want to call the following instruction: `ST(R31, 0b1100100, R31)`. Where is this data with VA of`0b1100100` located?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	VPN 6 is not resident. Therefore it is on disk. 
	</p></div>

4. Where will the content for VPN 6 be located in the RAM after the instruction `ST(R31, 0b1100100, R31)` is executed, i.e: which data -- A, B, C, or D will be replaced by the content of VPN 6?

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	It will replace data B since its PPN is not in the pagetable (means that its not a relevant content and can be overwritten). 
	</p></div>

5. Draw the state of the TLB after `ST(R31, 0b1100100, R31)` is executed.

	<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
	<img src="https://dl.dropboxusercontent.com/s/3q2ydp6117bq1dm/TLBANS.png?raw=1" width="70%" height="70%">
	</p></div>


