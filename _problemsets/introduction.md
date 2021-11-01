---
layout: academic
permalink: /problemsets/introduction
title: Introduction to something
description: Thunder is a lightning fast responsive theme based on default Jekyll theme. It is minimal and free from JavaScript. It has a css file of size 5kb. This theme is best suited for minimal blogs.
---

* TOC
{:toc}

# Test Header 1
lorem ipsum something something

# Test Header 2
lorem ipsum something something

## Test Subheader 2
This is math:

$$\begin{matrix}
x & y & z \\ 
\hline 
0 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 0 & 0 \\ 1 & 1 & 1 \\ \hline \end{matrix}$$

$$\begin{aligned}
ab + \bar{a}c + bc &=  ab + \bar{a}c + (\bar{a}+a)bc\\
&=ab + abc+ \bar{a}c + \bar{a}bc \\
&= ab(1+c) + \bar{a}c (1+b)\\
&=ab + \bar{a}c\end{aligned}$$

# Combinational Circuit's Functional Specs (Basic)
Consider the following circuit that implements the 2-input function $H(A,B)$:

<img src="  https://dl.dropboxusercontent.com/s/2vy52yuzs24xfc4/Q2new.png?raw=1"  width="50%" height = "50%">

1. Write down the **truth table** for $H$.

2. Give a **sum-of-products expression** that corresponds to your truth table.
3. Using the information below, what are the $t_{cd}$ and $t_{pd}$ of the circuit?
	- $t_{cd}$ and $t_{pd}$ of NR2:  5, 30
	- $t_{cd}$ and $t_{pd}$ of ND2: 5, 30
	- $t_{cd}$ and $t_{pd}$ of AN2:  6, 50
	- $t_{cd}$ and $t_{pd}$ of OR2: 10, 20
	- $t_{cd}$ and $t_{pd}$ of INV: 1, 3


<div cursor="pointer" class="collapsible">Show Answer</div><div class="content"><p>
<ol type="1">
<li> The truth table is as follows: 
$$\begin{matrix}
A & B & H \\
\hline
0 & 0 & 1\\
0 & 1 & 1 \\
1 & 0 & 0 \\
1 & 1 & 1\\
\hline
\end{matrix}
$$</li><br>
<li> We begin by finding the expression of the topmost two circuits and applying de Morgan's law $$\overline{A + \overline{B}} = \overline{A}B$$ 
<br><br>Then, we find the expression of the next pair, which is $AB$.  We combine this with the above using a NOR gate and reduce the result, $$\overline{\overline{A}B + AB} = \overline{B}$$<br>
<br>Finally, we find the expression for the bottom two pairs, which is simply $A+B$. Combining this with the above expression, we reduce and apply de Morgan's law:
$$\begin{aligned} \overline{(A+B)\overline{B}} &= \overline{A \overline{B} + B \overline{B}} = \overline{A\overline{B}} = \overline{A} + B\\
\end{aligned}$$
</li><br>
<li> <strong>The contamination delay</strong> is the path  (from any input to any output)  that results in the shortest time: NR2 + NR2 + ND2 = $5 + 5 + 5 = 15$. <strong>The propagation delay</strong> is the path (from any input to any output) that results in the longest time: AN2 + NR2 + ND2 = $50 + 30 + 30 = 110$.</li></ol>
</p></div>

<br />
