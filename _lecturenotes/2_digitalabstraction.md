
50.002 Computation Structures
Information Systems Technology and Design
Singapore University of Technology and Design
**Natalie Agus (Fall 2020)**

# Logic Synthesis

## Overview
The purpose of creating combinational devices is to **synthesise logic**, meaning that we create a device that is able to give a certain combination of output given a certain combination input. In other words, a device that adheres to a truth table, i.e: its *functional specification.* 

Any combinational device has to have a functional specification. Functional specifications are represented with **truth tables**. So for example, the following is the truth table of an NAND gate *(input: A and B, output: Y)*: 

$$\begin{matrix}
A & B & Y\\
\hline
0 & 0 & 1\\
0 & 1 & 1\\
1 & 0 & 1\\
1 & 1 & 0\\
\end{matrix}
$$

The truth table for an AND gate is the opposite *(input: A and B, output: Y)*: 

$$\begin{matrix}
A & B & Y\\
\hline
0 & 0 & 0\\
0 & 1 & 0\\
1 & 0 & 0\\
1 & 1 & 1\\
\end{matrix}
$$
 So as you can see, "NAND" is just a short for "not-an-and". The output of a NAND gate is always `1` unless both A and B are `1`. The output of an AND gate is the opposite, that is it is always `0` unless both A ***and*** B are `1`.  

  
## N-input Gates

  

There are 16-possible 2-input gates, as shown in the image below. The AND gate and the NAND gate are among the few ones that are more common. The OR, NOR, XOR, and XNOR gates are very commonly used too. In short, **there are $2^{2^x}$ possible x-input gates**, but not all are exactly useful in practice.

> *Note:* please do not memorize these things blindly. They're **logic**, encoded in binary. The names of the gates signify the logic, for example logic AND means that we shall *detect* and produce a discernible output if input `i` **and** `j` (or any other inputs) to the device are all `1`. 

<img src="https://dl.dropboxusercontent.com/s/p407rd2m9n943hh/tt.png?raw=1"  alt="Figure1"  width="80%">  
  


## Sum of Products
We can also have functional specifications in terms of **boolean expression**. To convert truth tables into boolean expressions, we take the following steps:

1. Look **only for the rows with output= 1**. In the case of the NAND gate, we look at row 1, 2, and 3. 
1. For each row in with output = 1, if the value of the input is a 0, then express it with a NOT.

	> Taking the NAND gate's truth table as example, 
	> - For row 1 of NAND gate's truth table, we have $\overline{A} \text{ } \overline{B}$. 
	> - For row 2, we have $\overline{A}B$. 
	> - For row 3 we have $A\overline{B}$.
3. Sum all the expressions from the rows with Y=1.
	> Taking the NAND gate's truth table as example, the sum of product is:
$$\begin{aligned}
Y = \overline{A} \text{ } \overline{B} + \overline{A}B + A\overline{B}
\end{aligned}$$
4. The expression above is called the **sum of products**.

*Sometimes in textbooks, it is called as canonical sum of products. They mean the same thing as just "sum of products".*

## Universal Gates

  

NAND and NOR gates are **universal**, meaning that each *alone* can implement **any** boolean function. AND, OR, and INV alone aren't sufficient, but together these three can express any boolean expressions (we will see this in the next section). 

We can use just NANDs or just NORs gates to make AND, OR and INV gates:

<img src="https://dl.dropboxusercontent.com/s/dflzkxdqvuyypjt/univgates.png?raw=1"  alt="Figure3"  width="70%">

Therefore, NANDs and NORs are <ins>universal</ins> gates.

## Straightforward Logic Synthesis

Recall that the goal of combinational devices is that they are created to adhere to a certain functional specification. We can make various logic gates and combine them to synthesize a more complex logic or truth table. However, there are basic logics that can be used to synthesize any kinds of other (more complex) logic. These are: INV (inverter), AND gate, and OR gate. 

The truth table for OR gate is as follows, that is the output is `1` if either A or B input is 1:
$$\begin{matrix}
A & B & Y\\
\hline
0 & 0 & 0\\
0 & 1 & 1\\
1 & 0 & 1\\
1 & 1 & 1\\
\end{matrix}
$$


==Given a sum-of-products boolean expression, we can make a combinational device that has that boolean expression as functional specification using **these three types of logics**: INV, AND, and OR with *arbitrary* number of inputs.==

For example, given the following sum of products expression,

$$ \begin{aligned}
Y = \overline{C} \text{ }\overline{B} A + \overline{C} B A + CB\overline{A} + CBA
\end{aligned}$$

 
We can make a combinational device as such that it adheres to the expression above using these three logic devices only, as shown below: 

<img src="https://dl.dropboxusercontent.com/s/9snlkdv9s4ldy10/gates.png?raw=1"  alt="Figure2"  width="40%" height="40%">

Explanation:
* The boolean expression of the output Y contains **4 terms** that are added or summed together.
 * The 4-input OR gate at the output Y represents the **summation** of these four terms.
* The AND gates in the *second "column"* of the figure represents the **combination** of each of the input terms, 
	> For example: $\overline{C} \text{ }\overline{B} A$ means *not C*, *not B*, and *A* combined together as an input to a 3-input AND gate. 
* The INV at the input represents the NOT inputs *(negated inputs).*

Using these steps, we can come up with the *simplest* (not necessarily the most efficient, cheapest, smallest, etc), most straightforward logic synthesis. 

Notice that if the expression contains many terms summed together, we need bigger OR gate at the output. This causes the size of our device to be bigger, and therefore more expensive. In the later sections, we learn how to ***reduce*** the boolean expression such that we have lesser number of terms, and thus are able to synthesize the logic more effectively. 

oolean lgebra to manipulate boolean emay he simpler properties is as follows.

$$\begin{aligned}
\text{OR rules: } & a+1 = 1, \\
& a+0 = a, \\
& a+a=a\\
\text{AND rules: } & a1=a, \\ & a0=0, \\ & aa=a\\
\text{COMPLEMENT rules: } & a + \bar{a}=1, \\ & a\bar{a}=0 \\ 
\end{aligned}$$

Below are more laws that are built based on some of the rules above and each other. We do not have to prove each law in this course, but if you're interested, you can look for other references such as [here](https://www.electronics-tutorials.ws/boolean/bool_6.html).
$$\begin{aligned}
\text{Commutative law: } & a+b = b+a, \\ &ab = ba\\
\text{Associative law: } & (a+b)+c = a+(b+c),\\ & (ab)c = a(bc)\\
\text{Distributive law: } & a(b+c) = ab+ac, \text{ --- OR distributive},\\ & a+bc = (a+b)(a+c) \text{ --- AND distributive}\\
\end{aligned}$$
The two laws below are useful to perform boolean minimisation because we might end up with lesser number of terms while keeping the same logic:
$$\begin{aligned}
\text{Absorption law: } & a+ab=a, \\ & a+\bar{a}b = a+b,\\ &a(a+b) = a, \\ &a(\bar{a}+b) = ab\\
\text{Reduction law: } &ab + \bar{a}b = b, \\& (a+b)(\bar{a}+b) = b
\end{aligned}$$


Note that b## Boolean aAlgebra also applies for the inverted form, e.g: if $\bar{a}$ is swapped to $a$ instead: 
*  $a+\bar{a}b = a+b$ (original); $\bar{a}+ab = \bar{a}+b$ (invert $a$)
* $ab + \bar{a}b = b$ (original); $a\bar{b} + \bar{a}\bar{b} = \bar{b}$ (invert $b$) 
* $a(a+b) = a$ (original); $\bar{a}(\bar{a}+\bar{b}) = \bar{a}$ (invert both)

There's a lot of boolean theorems that are derived from the above, for example:
$$\text{Consensus Theorem: } ab + \bar{a}c + bc =  ab + \bar{a}c$$
Proof:
$$\begin{aligned}
ab + \bar{a}c + bc &=  ab + \bar{a}c + (\bar{a}+a)bc\\
&=ab + abc+ \bar{a}c + \bar{a}bc \\
&= ab(1+c) + \bar{a}c (1+b)\\
&=ab + \bar{a}c\end{aligned}$$

What's important is to pay attention to the relationship between each variables. You can easily let $\bar{a} = x$ and find the formula applies as well for the inverted version. 
 
### DeMorgan's Theorem

DeMorgan's theorem is useful as a tool for us

 

Another tool we need to master before being able to minimise or reduce boolean expression is boolean algebra properties. They are useful to manipulate boolean expressions so that we quations. A summary end up with simpler terms and reduce the terms, while still keeping the logic euivalent. 
  

$$\begin{aligned}
\overline{a+b} &= \overline{a} \text{ } \overline{b}\\
\overline{ab} &= \overline{a} + \overline{b}
\end{aligned}$$


of these properties is as follows:

<img src="https://dl.dropboxusercontent.com/s/1etmcz1rt7lrooc/Q4.png?raw=1"  alt="Figure4"  width="80%">


### Boolean Minimization Example

When given a boolean expression, we need to be creative and utilize all properties of boolean algebra to minimise the expression. For example, we can use the *reduction* rule from the boolen algebra cheat-sheet above to perform boolean minimization:

$$\begin{aligned}
Y &= \overline{C} \text{ }\overline{B} A + \overline{C} B A + CB\overline{A} + CBA\\
&= \overline{C} \text{ } \overline{B} A + \overline{C} BA + CB \text{ (reduce the last two terms)}\\
&= \overline{C} A + CB \text{ (reduce the ist two terms)}
\end{aligned}$$

### Karnaugh Map for Boolean Minimisation

 TheKarnaghmap offers an alternative method to perform boolean minimization. This is a method to easily perform boolean minimization, and **ultimately the end goal is to reduce the digital circuit to its minimum number of gates** (save cost and save space).


*The following figure shows a 2-input (by input it just basically means how many input boolean variables), 3-input, and 4-input Karnaugh maps.* **Please do not change the order**, they follow Gray code configuration to preserve adjacency so rules 1-6 below can apply. 
> It is possible to rotate them clockwise or anticlockwise but do so only if you understand the logic behind arrangement of Karnaugh map configuration, which is out of this syllabus. 

<img src="https://dl.dropboxusercontent.com/s/3aaw73p23w2zd4j/k1.png?raw=1"  alt="Figure4"  width="60%" height="60%">

The number of **cells** of Karnaugh maps with $x$ inputs is $2^x$ cells. Then, fill in '1' to all the cells that represent logic '1' on the boolean expression. 

This is illustrated in the truth table and its corresponding Karnaugh map below:

  
<img src="https://dl.dropboxusercontent.com/s/cmx3apt9l48izd5/k2.png?raw=1"  alt="Figure5"  width="60%">

Then you can **simplify** the Karnaugh Map using these 6 ground rules:

1. **Groups should contain as many "1" cells** (i.e. cells containing a logic 1) as possible and no blank cells.
2. Groups can **only** contain 1, 2, 4, 8, 16 or 32... etc. cells (powers of 2).
3. A "1" cell can only be grouped with **adjacent** "1" cells that are immediately *above, below, left or right* of that cell; **no diagonal grouping.**
4. Groups of "1" cells can **overlap**. This helps make *smaller groups as large as possible*, which is an advantage in finding the **simplest** solution.
5. The *top/bottom* and *left/right edges*  (and also the 4 **corners**) of the map are considered to be **continuous**, so larger groups can be made by grouping cells across the top and bottom or left and right edges of the map:
	* Top row and bottom row (or parts of them) can form one group as long as condition 1-4 are satisfied. 
	* Leftmost column and rightmost column  (or parts of them)  can also form one group as long as condition 1-4  are satisfied. 

6.

1. There should be as ***few*** groups as possible.

  
  

Following the rules above, the simplified example Karnaugh map is:

<img src="https://dl.dropboxusercontent.com/s/ul1xkga719faqes/k3.png?raw=1"  alt="Figure3"  width="0%" height="40%">

  

To convert this Map back into boolean expression, we need to look at each group and use a little bit of logic:

1. In the **blue group,** the output is `1`1 regardless of A, and regardless of C. Hence, the boolean expression for the blue group is just M.

2. In the green group, the output is `1`1 regardless of M. Therefore, the boolean expression for the green group is AC.

3. The complete simplified boolean expression is: X = M + AC.
	> X = M + AC is logically equivalent to X = $\bar{A}M\bar{C}$+$\bar{A}MC$+ $AM\bar{C}$+$A\bar{M}C$+$AMC$ (the sum of products of its truth table). 
	> 
	> You can also obtain the minimized expression using boolean algebra:
	>	- Reduction rule: $\bar{A}M\bar{C}$+$\bar{A}MC$ = $\bar{A}M$
	> - Reduction rule: $AM\bar{C}$+$AMC$ = $AM$
	> - So far we have: X = $AM+ \bar{A}M+AM\bar{M}{C}$
	> - We can further reduce the first two terms, resulting in  X = $M+A\bar{M}{C}$
	> - Use absorption rule to absorb $\bar{M}$, we end up with X = $M+AC$

<div class="yellowbox"> Note that minimised boolean forms are not necessarily unique. The number of terms left in the final expression is unique but its possible to have a different form. </div>

## Logic Synthesization with CMOS
We can create a combinational logic device easily given the *minimized* boolean expression, using any of the universal gates:
* NANDs only
* NORs only
* AND, INV, and OR 

Each gate can be created using transistors: PFETs and NFETs arranged in a complementary way. The schematic of each is as follows:
<img src="https://dl.dropboxusercontent.com/s/tnleg2coz9kjpul/andorinv.png?raw=1"  alt=“F1”  width="0%" height = "60%">

> The OR and AND gates are simply the NOR and NAND gates with inverter at the output. 

We can also create the device straight using CMOS recipe given the minimised boolean expression (instead of using the universal gates). For example, given this minimised equation that we did earlier:
$$Y= \overline{C} A + CB$$

It can be made this way with a combination of **universal gates**:
<img src="https://dl.dropboxusercontent.com/s/rqc1v5b9uioneef/dev.png?raw=1"  alt=“F1”  width="60%" height = "0%">

This requires **20 MOSFETs** to build.

Or the **primitive** way:
<div class="redbox"> Note that constructing a CMOS circuit is freestyle, and <strong>theres more than one way to construct the circuit that produces the same logic</strong>. You can choose to construct the pull-down first or the pull-up first. You can also choose to construct the negation of the circuit and invert the overall output. <mark>Please do not memorise this blindly</mark>. At the end of the day, whichever method you choose, it is fine as long as the CMOS circuit produces the <strong>correct logic</strong>. If you want to <i>minimise</i> them though them some careful design is required. It is an art to design a CMOS circuit and it is beyond the scope of this course. 
</div>

* **Step 1:** Construct a **pull-down circuitry**: 
	* for each '+' (OR) we build a parallel NFET circuit
	* for each $\cdot$ (AND) we build a series NFET circuit 

	Therefore we have two sets of two NFETs in series: 
	<img src="https://dl.dropboxusercontent.com/s/vsfyv7iefuv19cx/DEVMOS1.png?raw=1"  alt=“F1”  width="60%" height = "60%">

* **Step 2:** Add inverter at the output.
	* In **Step 1** we created a pull-down circuitry that is *activated* when each of the terms in the boolean expression produces an overall '1'. 
		> E.g: when $B, C$ are both 1, the pull-down is activated as current can flow from $Y$ to the GND. The effective output at $Y$ will be then `0`0 when $B,C$ are both `1`1. 
	* Since what we want is the *opposite*, we need to put an inverter at the output. 
		>that is   $Y=1$ when $B,C$ are both 1, we need to put an inverter at the output, as shown: 
		<img src="https://dl.dropboxusercontent.com/s/x2ktjw4gqxx979r/devmos2.png?raw=1"  alt=“F1”  width="60%" height = "60%"> 
		<div class="yellowbox"> If what you want is for $Y$ to be <code>0</code> when $B,C$ are both <code>1</code> then there's no need to put an inverter in the end. Just draw the complementary pullup and call it a day </div>

* **Step 3:** Construct the **complementary** pull-up circuitry and assemble. 
	>  Refer to the CMOS recipe in the previous chapter. 

	<img src="https://dl.dropboxusercontent.com/s/ft9xplwm26ksgks/devmos3.png?raw=1"  alt=“F1”  width="60%" height = "60%">

This requires **14 MOSFETs** to build, lesser than the previous design. It is definitely easier (for us) to create a combinational logic device using a bunch of universal gates, but it comes at the cost of money and size. 

Note that the CMOS recipes that we learn in this course also **does not guarantee** that you can build a device with **minimised number of transistors,** given its functional specification. It is an *art* to create the most efficient circuit in terms of money, size, and usage. 




## Special Combinational Logic Devices

### The Multiplexer
  

The Multiplexer (shorted as "mux") is a special combinational logic device that is very commonly used in practice. It is implemented using basic logic gates (INV, AND, and OR, or NANDs). The mux is expensive to manufacture, but *universal*, meaning that it can **implement any boolean function because essentially it "hardcodes" the truth table**. 

The symbol for a mux is as shown in the image below. The truth table is written at the side. A mux **always** has **three** types of terminals: 
* $2^k$ bits data inputs, 
* `k` bits selector signal(s) --*this is also an input, but we have a special name for them them: selector*-- , and 
* 1-bit output. 

It's functioncomponents: the inputs, the selector signal(s), and the output. It basically "*allows*" oneeither of the input signals to be reflected at  `OUT`pass through when selected. 

For example in the case of 2-input mux below, when S=0, it will reflect whatever value the signal  $A$ carries (`1` or `0`) as its output:

> Take some time to make sense of the truth table. That is if S=0, OUT = A. Else, if S=1, OUT = B. produce the signal  $D_0$ as its output:

<img src="https://dl.dropboxusercontent.com/s/nbatvm3m7xvq279/muxtt.png?raw=1"  alt="Figure3"  width="50%" height="50%">

You can build a 2-input multiplexer using basic gates:
<img src="https://dl.dropboxusercontent.com/s/kl35pytim23xlm4/muxin.png?raw=1"  alt=“F1”  width="60%" height = "60%">

Some properties about multiplexers:
1. Muxes are **universal**, meaning that it can implement any boolean functions
1. A Mux can have $2^k$ data inputs, and $k$ bits select inputs, and **only can have 1 output** terminal. 

We can also generalise the multiplexer to take more inputs: 4, or 8, or 16, etc. We can either build a bigger multiplexer or cascade many 2-input multiplexers. The following figure shows an example of a 4-input multiplexer, implemented as a big mux (left) or using a series of 2-input mux (right):  

<img src="https://dl.dropboxusercontent.com/s/g5sqzvvn5pqwoha/4mux.png?raw=1"  alt=“F1”  width="80%" height = "80%"> 	
  
Similarly, you can build a 4-input mux using basic logic gates: 
<img src="https://dl.dropboxusercontent.com/s/pl9902hnvpeg9mp/4muxin.png?raw=1"  alt=“F1”  width="50%" height = "50%">

Below is an example of how a mux can be used to implement a more complex combinational device, the full adder that we encounter in the lab. The truth table of a full adder is as shown, it is basically an addition (of three inputs) in base 2:

  

<img src="https://dl.dropboxusercontent.com/s/lryt8p85jrowz40/addr.png?raw=1"  alt=“F1”  width="30%" height = "30%">  

The multiplexer can simply implement the truth table by mapping each type of output bit $C_{out}$, and $S$ in each of the input terminals of the mux as illustrated below (for the carry out): 


<img src="https://dl.dropboxusercontent.com/s/0vpdyz1lch62jd1/muxc.png?raw=1"  alt="F7"  width="60%" height="60%">

We can do the same thing for $S$, and both of them combined will function as a full adder. 

### Decoder

The Decoder (also known as "demux") is a special combinational logic device that is also very commonly used in practice. It can have $k$ select inputs, and $2^k$ possible output combinations. The schematic of a 1-select input decoder is:
<img src="https://dl.dropboxusercontent.com/s/btt8jleh2quwnjq/decoder2hzdkw3c4rzxsd6p/demuxin.png?raw=1"  alt=“F1”  width="50%" height = "50%">

> Practice: Draw out the truth table of the decoder above.


The schematic of a 2-select inputs decoder: $S_0$ and $S_1$ is (we omit the "IN") because it is usually just VDD:

<img src="https://dl.dropboxusercontent.com/s/8uagnvsipvppgby/decoderinside.png?raw=1"  alt="Figure3"  width="50%" height = "570%">

> Take some time to trace out the selector values to the output and draw out a truth table for the decoder. 

*Note: do not worry about the logic gate schematics of a decoder. It is only there to show you that a decoder is made up of the normal logic gates like inverters and AND gates.* 

Some properties about decoders:

1. A Decoder is basically the *opposite* of a multiplexer. It has $k$ select inputs, and $2^k$  **possible data outputs**, and only 1 bit of input (typically VDD). The symbol is shown below:
<img src="https://dl.dropboxusercontent.com/s/ig6s46lb2s992c23ye21rfq2xi8e0u/demux.png?raw=1"  alt=“F1”  width="430%" height = "430%"> 

2. This figure omits the 1 bit input to the decoder because **it is always set to 1** in practice.
3. Therefore, for a 4 bit decoder as shown in the figure above, the input signals are only the two **SELECTOR** signals, denoted as $IA_0$ and $IA_1$ in the figure.
4. **At any given time** only 1 bit of the $2^k$ output bits can be `1`1 (high). This is apparent when we try to draw the truth table for a $k$ input decoder. For example, the truth table for a 1-selector bit decoder is:
$$
\begin{matrix}
S & O_1 & O_2\\
\hline
0 & 1 & 0 \\
1 & 0 & 1\\
\hline
\end{matrix}
$$
The truth table for a 2-selector bits decoder is:
$$
\begin{matrix}
S_0 & S_1 & O_0 & O_1 & O_2 & O_3\\
\hline
0 & 0 & 1 & 0 & 0 & 0 \\
0 & 1 & 0 & 1 & 0 & 0 \\
1 & 0 & 0 & 0 & 1 & 0 \\
1 & 1 & 0 & 0 & 0 & 1 \\
\hline
\end{matrix}
$$
	>In other words, only the selected output $i$ is HIGH (`1`1), and the rest of the $2^k-1$ data output is LOW (`0`0).

### Read-Only-Memories (ROM)

  

One of the application of a decoder is to create a read-only-memories (ROM). 
> You can buy them online, like [this product](https://learn.adafruit.com/digital-circuits-5-memories/read-only-memory). 

For example, if we "hard-code" the Full-Adder using a decoder, we end up with the following schematic:
<img src="https://dl.dropboxusercontent.com/s/t90f9n3ypg9aj9c4cifjdy1ccen651/decoder.png?raw=1"  alt="Figure3"  width="70%">

Explanation for the schematic above:

- At the output of the decoder, the little circuit with inverted triangle symbol signifies a **pulldown circuit** (an NFET connected to ground), which will "drain" a signal into LOW (0).

- Recall that at  each **combination** of select signal $A, B$, and $C_i$, only one of the 8 outputs of the decoder will be `1`1. 
	> - For example, when $A=0, B=0, C_{in}=1$, the second output line of the decoder from the top is `1`1. 
	<img src="https://dl.dropboxusercontent.com/s/o5meriyxc47k0bn/sel1.png?raw=1" width="40%" height="40%">
	> - There's a pulldown at the S line, which drains the `VDD` and results in `0` at S line towards the inverter. There's no pulldown for the $C_{out}$ line, so the value fed in towards the inverter in the $C_{out}$ line is `1`> - There's a pulldown for S (it is connected to the ground), which makes it 0, and no pulldown for the $C_{out}$, which makes it 1.
	> - Therefore, when $A=0, B=0, C_i=1$, $S=0$ and $C_{out}$ is 1. 

- Note the **presence of inverters by invention** at the end of the two vertical output lines for $S$ and $C_{out}$, so the overall output is inverted to be `1`1 for $S$S and `0`0 for $C_{out}$.

- By **invention**, the location of the "pulldown" circuits **correspond to a '1' in the truth table** for that particular output ($S$ or $C_{out}$).

- For $K$ inputs, decoder produces $2^K$ signals, only `1`1 which is asserted (valid "High", or simply "selected") at a time. 

The properties of ROM are as follows:

1. ROMs ignore the structure of combinational functions (our truth table is "hardcoded". 

2. The selectors are like **addresses** of an entry.
3. For an $N$-input boolean function, the size of ROM is roughly $2^N \times \text{ \#outputs}$. 
	> For example, the Full Adder has 3 inputs (A, B, $C_{in}$), and 2 outputs (S and $C_{out}$). Hence the size of the ROM is $2^3 * 2 = 16$.


## Conclusion
Synthesizing combinational logic is not a simple task. There are many ways to realise a functionality, i.e: the **logic** *(that the device should implement)* represented by the truth table or boolean expression. We can use universal gates (only NANDS, or only NORS), a combination of gates (INV, AND, and OR), or many other ways (multiplexers, ROMs, etc). 

Of course "*hardcoding*" a truth table using ROM and Multiplexers are convenient, because we do not need to think about simplifyfing the boolean expression of our truth table (which can get really difficult and complicated when the truth table is large, i.e: complicated functionality). However it comes at a cost: the **cost of the materials** to build the ROM / Multiplexers, and at the **cost of space** (we need use a lot of logic gates to build these). 


# The Digital Abstraction

## Overview

One of the cheapest ways to encode information is in terms of 0s and 1s, using <span style="background-color:yellow"> voltages</span>. This method of determining *discrete* values out of voltages (which value is originally made up of real number, and therefore *continuous* and *infinite*) is called the **digital abstraction**.

What are the benefits of the digital abstraction?
1.  The digital abstraction is where one interprets voltage values as **binary** values, thus allowing us to encode information using voltages.
2.  Using voltages to encode bits of 0's and 1's provides a **cheap and stable way** for us to exchange information through digital devices.
3.  We can also **manipulate** or **change** information encoded using voltages very easily.

The voltages that represent digital bits are generated by **semiconductor devices (MOSFET)** -- something that we will learn in the next chapter. The benefit of using semiconductors  the ease of generation, and that they require zero power in steady-state. The drawbacks however, is that the voltages generated by these semiconductors are easily affected by external disturbances,  and hence they may be unstable.

To preserve the integrity of information encoded in digital devices made of semiconductor materials, we need to **set some contracts between these interconnected digital devices**. In this notes, we are going to learn *how* we can use voltages to encode information in a stable way that follows a particular contract called the <span style="background-color:yellow"> **static discipline** </span> to guarantee the behavior of each processing block in the system.

  
## A Digital Processing Element: Combinational Device and Combinational Digital System

A digital device is any device that uses voltages to encode information in terms of “low voltage” **(bit 0)** and “high voltage” **(bit 1)**. Its output is a pure function of the present input only *(there's no memory of past inputs)*, and it has the following criteria.

A combinational device is a specific type of digital device that has the following criteria:
1.  One or more digital inputs
2.  One or more digital outputs
3.  A **functional specification** that details the value of each output for each possible **combination** of inputs (can be illustrated in terms of truth table / boolean expression)
	> Its circuit performs an operation assigned logically by a boolean expression or truth table.
4. A **timing specification** consisting of an upper bound required  
propagation time for the device to compute the specified output  
values given a set of valid and stable input value(s)

Later on you will learn another type of digital logic devices called the **sequential** logic device, whose output depends not only on the present input but also on the history of the inputs, hence having a *memory*. 

A **set** of interconnected circuit elements is  **combinational**, and can be called **combinational digital system** if and only if:
* **Each circuit element is also combinational with no directed cycles** (no *feedback* loop), and 
* That very device's input is connected to **exactly one output of another device** or to some vast supply of '0's and '1's.

  
## Voltage to encode information

The most naive way to use voltage to encode information is to use ‘low’ voltage to encode valid ‘0’ and ‘high’ voltage to encode valid ‘1’, and define the low and high threshold for each valid ‘0’ and ‘1’.

Anything that is between the low and high threshold value is called the invalid zone, as shown in the figure below:
  
<img src="https://dl.dropboxusercontent.com/s/6uo61vk9yze1aot/Volt.png?raw=1"     width="80%" height = "80%">
  

The static discipline is one of the **contracts** bound for all logical elements making up a digital system. The static discipline is stated as follows:

  

> **A digital system must be able to produce a valid output (for the next device connected at its output terminal) according to its specification if it is given a valid input.**

This contract **guarantees** the behavior for each processing block in a system, so that a set of such interconnected devices may work properly (are able to pass and compute valid information at the end of the chain of connections). This is necessary so that the system has a **predictable behavior.**

> **However this doesn't mean that the opposite is true.** a device that receives invalid input *does not always have to* produce invalid output. We don't care much and cannot define or guarantee the behaviour of the combinational device if it receives invalid input -- it may or may not produce a valid output.


Therefore, one can say that **a combinational logic device always obeys the static discipline**. 


  

## Voltage Specifications and Noise Margin


Consider two digital devices connected in series as shown in the figure below. These devices are called a **buffer**, meaning that they pass the same bit over (if it receives a low voltage, it will produce a low voltage and vice versa). If we were to *naively decide* that any voltage below $V_{low}$=`0.5V` as digital bit `0`, and any voltage above $V_{high}$=`2.5V` as digital bit `1`, then our device **may violate the static discipline.** 

>*Why is this so?*

<img src="https://dl.dropboxusercontent.com/s/9lejkhiqx50ga8y/p4.png?raw=1"  width="50%" height = "50%">


This explanation can be made clear with the following example. Suppose we supply 0.5V and Device 1 is able to produce also 0.5V, which means digital bit . 
* However, the problem is that a *wire*, that connects two or more combinational devices together is susceptible to **noise**. 
* The voltage value that is received at Device 2 may be *slightly higher* than 0.5V, for example: 0.55V instead, and therefore according to our specification, it is *_no longer a valid bit '0'_*.

> *Note that a noise can knock the voltage down as well, this is just an example that's detrimental to the function of the devices in this example.*

  
Device 1 in the figure above **violates** static discipline because *given a **valid** input, it *may* be **unable** to produce a valid output (to **reach** the next device 2)*, because the `0.5V` produced at the output of Device 1 may meet some disturbances that caused it to be slightly off, e.g: `0.55V`.

Hence, we need to account for the presence of some light **noise**.

Instead of naively setting some voltage $V_{high}$ and $V_{low}$ as we did above, we need to set a *range* of Voltages as valid bit `1` and `0` respectively, and ensuring that we  


Hence, we need to have something called the **noise margin** -- the yellow region illustrated in the Figure below. The noise margin is formed by setting *four* Voltage specifications:  $V_{ol}$, $V_{oh}$, $V_{il}$, $V_{ih}$, where  $V_{ol}$< $V_{il}$< $V_{ih}$ < $V_{oh}$ which **defines** what range of voltage values signifies a **valid** digital bit  and a **valid** digital bit `0` *for any combinational logic component in the system*: 
  
<img src="https://dl.dropboxusercontent.com/s/pt0n36pmy9ncyc6/Volt_2.png?raw=1" width="100%" height = "100%">
<br>

> <span style="background-color:yellow"> The *_noise margin_* adds as a **precaution** against external disturbances (noise). </span>

Below are the explanations necessary to understand the fFigure 3 above:
1.  $V_{ol}$ (voltage output low) and $V_{oh}$ (voltage output high) is the voltage that **your system** outputs, depending on whether your system is outputting bit `0` or '1'. The output of this system is going to be received by another system after traversing through some wire.

 
2.  $V_{il}$ (voltage input low) or $V_{ih}$ (voltage input high) is the voltage that **your system** receives as **input** from another system.

3.  The **absolute difference** between $V_{ol}$ and $V_{il}$ is called the **low bit noise margin**, and the **absolute difference** between $V_{oh}$ and $V_{ih}$ is called the **high bit noise margin**.
	> Noise margin is formally defined as the **maximum** voltage amplitude of *extraneous* (erronous) signal that can be added to the noise-free input level *without* causing a drastic change in the output voltage and that it is still within the valid logic level. 

4.  The **noise immunity** (like an "overall" or "effective" noise margin) is the ***minimum*** between the high bit noise margin and the low bit noise margin.

5.  $V_{ol}$ is **lesser** than $V_{il}$, because we would want to have some *buffer* against noise. A device always outputs a lower voltage value to signify digital bit `0`'0' and accepts a slightly higher low-voltage value as digital bit `0`'0'. The same logic applies for the higher region as well, as $V_{oh}$ is greater than $V_{ih}$

  

6.  In our previous case in Figure earlier, if $V_{ol}$ is set to be `0.5V`, and $V_{il}$ is set to be `0.6V`, then *Device 2*  will b are able to **tolerate** up to `0.1V` of noise (if any). Therefore, `0.55V` in our example above is still '*seen*' as a valid bit `0` when it arrives at the input terminal of Device 2, thus making Device 1 ***obeys the static discipline.***


Once **set and chosen,** these four voltage specifications: $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ are to be obeyed by every digital **device in an entire combinational logic circuit**.

  

## Voltage Transfer Characteristic Function (VTC)


The VTC is a **plot** between the input voltage ($V_{in}$) to a digital system/device vs the output voltage ($V_{out}$) of this digital system.

> <span style="background-color:yellow"> VTC **does not** tell us how fast the device is. It just captures the static behavior of the device and tells us what *kind* of device it is.
> </span>

The image below shows the VTC of a **buffer**: a *low* $V_{in}$ gives a *low* $V_{out}$ and vice versa. 

<img src="https://dl.dropboxusercontent.com/s/vod5ltqh4kq9119/vtcbuffer.png?raw=1"  width="60%"  height="60%" alt ="Figure 4"/>

> What will the VTC of an inverter look like? 


The purpose of plotting a VTC (*typically obtained from device measurements, i.e: we supply input voltages at intervals and measure the output*) helps us to **determine** whether or not a digital device **can be used** as a combinational logic device. 

> In other words, to find out if we can find a set of four voltage specifications: $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ for the device so that **the device obeys the static discipline**.

Explanation of the VTC figure above:
- The red zone is called the **forbidden zone**. It is formed by the four voltage specifications: $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ that we set for the entire system. 
.
- The name *'forbidden zone'* comes e the fact that <span style="background-color:yellow">any value within this zone means that the device receives **valid** input but is unable to produce a valid output</span>, hence **violating the static disciplin**e and cannot be used as a combinational logic device.

You can quickly tell if a digital device can be *potentially* be used as a combinational logic device **iff**: you can **find** a set of these four voltage specifications: $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ **whereby its VTC curve  does not cross the forbidden zone** and that $V_{ol}$< $V_{il}$ < $V_{ih}$ < $V_{oh}$.
* We typically begin by *guessing* each value of $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ and check if the curve crosses the forbidden zone (check if static discpline obeyed) formed by these four values. 

* If static discipline is violated, we either adjust our guess or find another device. 
* Also, we want to choose  $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ that **maximises noise immunity**. -

If you can satisfy the condition highlighted above, then it means that the device is a combinational logic device. It's VTC curve has to possesses both **characteristics** below:

1.  There exist *some region in the VTC* whereby  <span style="background-color:yellow">its **absolute** `Gain` is $>1$</span>. `Gain` is actually a function of $V_{in}$ and is **formally** defined as: 

	$$
	\begin{aligned}
		\text{Gain}(V_{in}) = \frac{d V_{out}}{d V_{in}} 
	\end{aligned}
	$$

	In laymen terms you can approximate `Gain` during some transition $V_{in_i}$ to $V_{in_j}$ that results in some $V_{out_k}$ to $V_{out_l}$ respectively by the simply computing the slope between these two points on the VTC: 

	$$ \text{Gain} \approx \frac{V_{out_l}-V_{out_k}}{V_{in_j}-V_{in_i}}  $$

	If you have found four voltage specifications $V_{ol}$, $V_{oh}$, $V_{il}$, and $V_{ih}$ for which the device still obeys the static discipline, you can **approximate** device's maximum `Gain` by computing: 

	$$\max\text{Gain}\approx \frac{V_{oh} - V_{ol}}{ V_{ih} - V_{il}}$$


1.  If  absolute `Gain` $>1$, then there is a **finite, positive** noise margin. If absolute `Gain`$=1$, then there's **zero** noise margin. It is *impossible* to have absolute `Gain` $<1$ and still have the four Voltage specifications $V_{ol}$ < $V_{il}$ < $V_{ih}$ < $V_{oh}$.
	  
	  > You might want to ponder a little and convince yourself why the statement above is true. 

	   Also, having absolute `Gain > 1` **maintains** the signal passed through the system as signal loss is inevitable through the system.

2. The device has a  **Non-linear `Gain`**, meaning that `Gain`  is a **function** of `Vin` and therefore the *_gradient_* along the *entire* curve varies. 
	* The VTC curve for a combinational logic device should not be **entirely** made of a single, constant gradient like the shape of a plot from a basic line equation, but rather more towards an "S" (or mirrored S) shape.

	> In other words, if both characteristics above aren't satisfied in the VTC curve, then it is not the VTC of a combinational logic device.

## Summary
In this chapter, we have learned about the digital abstraction, that is how can we set some **contracts** (via setting the four voltage specifications) such that we can establish digital values out of real-valued voltages. 

In the next chapter, we will learn about the **MOSFET** (transistor), that is one of the smallest component (building block) that makes up a digital device, and how we can use them to form a proper combinational logic elements we call **gates**. These **gates** can be  used to form an even larger **combinational circuits** such as the **adder**, **shifter**, etc, and an even larger one such as the **Arithmetic Logic Unit** (you will build them in Lab 2 and 3). 

> Each larger device will provide greater level of *abstraction*.  

Therefore, it is imperative that each  combinational logic device / component, no matter how small, **must** conform to the static discipline and the established four voltage specifications (that must be chosen such that it fits with their VTC) so that the larger system can work as intended. 
