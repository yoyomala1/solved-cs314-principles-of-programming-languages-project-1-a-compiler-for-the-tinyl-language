Download Link: https://assignmentchef.com/product/solved-cs314-principles-of-programming-languages-project-1-a-compiler-for-the-tinyl-language
<br>
<h1>1             Background</h1>

<h2>1.1           The tinyL language</h2>

tinyL is a simple expression language that allows assignments and basic I/O operations.

<table width="311">

 <tbody>

  <tr>

   <td width="119"><em>&lt;</em>program<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="155"><em>&lt;</em>stmt list<em>&gt; </em>!</td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>stmt list<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="155"><em>&lt;</em>stmt<em>&gt; &lt;</em>morestmts<em>&gt;</em></td>

  </tr>

 </tbody>

</table>

<em>&lt;</em>morestmts<em>&gt;       </em>::=      ; <em>&lt;</em>stmt list

<table width="383">

 <tbody>

  <tr>

   <td width="119"><em>&lt;</em>stmt<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="227"><em>&lt;</em>assign<em>&gt; </em>| <em>&lt;</em>read<em>&gt; </em>| <em>&lt;</em>print<em>&gt;</em></td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>assign<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="227"><em>&lt;</em>var<em>&gt; </em>= <em>&lt;</em>expr<em>&gt;</em></td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>read<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="227">% <em>&lt;</em>var<em>&gt;</em></td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>print<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="227">$ <em>&lt;</em>var<em>&gt;</em></td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>expr<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="227"><em>&lt;</em>arith expr<em>&gt; </em>|<em>&lt;</em>logical expr<em>&gt; </em>|<em>&lt;</em>var<em>&gt; </em>|<em>&lt;</em>digit<em>&gt;</em></td>

  </tr>

 </tbody>

</table>

<em>&lt;</em>arith

<table width="461">

 <tbody>

  <tr>

   <td width="119"><em>&lt;</em>logical expr<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="305">&amp; <em>&lt;</em>expr<em>&gt; &lt;</em>expr<em>&gt; </em>|| <em>&lt;</em>expr<em>&gt; &lt;</em>expr<em>&gt;</em></td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>var<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="305">a | b | c | d | e | f</td>

  </tr>

  <tr>

   <td width="119"><em>&lt;</em>digit<em>&gt;</em></td>

   <td width="37">::=</td>

   <td width="305">0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9</td>

  </tr>

 </tbody>

</table>

Two examples of valid <strong>tinyL </strong>programs:

<ol>

 <li>%a;%b;c=&amp;3*ab;d=+c1;$d!</li>

 <li>%a;b=-*+1+2a58;$b!</li>

</ol>

<h2>1.2           Target Architecture</h2>

The target architecture is a simple RISC machine with virtual registers, i.e., with an unbounded number of registers. All registers can only store integer values. A RISC architecture is a load/store architecture where arithmetic instructions operate on registers rather than memory operands (memory addresses). This means that for each access to a memory location, a load or store instruction has to be generated. Here is the machine instruction set of our RISC target architecture. <em>R<sub>x </sub></em>, <em>R<sub>y </sub></em>, and <em>R<sub>z </sub></em>represent three arbitrary, but distinct registers.

<table width="604">

 <tbody>

  <tr>

   <td colspan="4" width="172"><strong>instr. format</strong></td>

   <td width="326"><strong>description</strong></td>

   <td colspan="3" width="106"><strong>semantics</strong></td>

  </tr>

  <tr>

   <td colspan="4" width="172"></td>

   <td width="326"><strong>memory instructions</strong></td>

   <td colspan="3" width="106"></td>

  </tr>

  <tr>

   <td colspan="4" width="172">LOADI <em>R<sub>x </sub></em>#<em>&lt;</em>const<em>&gt;</em></td>

   <td width="326">load constant value #<em>&lt;</em>const<em>&gt; </em>into register <em>R<sub>x</sub></em></td>

   <td colspan="3" width="106"><em>R<sub>x </sub></em>← <em>&lt;</em>const<em>&gt;</em></td>

  </tr>

  <tr>

   <td colspan="4" width="172">LOAD <em>R<sub>x </sub>&lt;</em>id<em>&gt;</em></td>

   <td width="326">load value of variable <em>&lt;</em>id<em>&gt; </em>into register <em>R<sub>x</sub></em></td>

   <td colspan="3" width="106"><em>R<sub>x </sub></em>← <em>&lt;</em>id<em>&gt;</em></td>

  </tr>

  <tr>

   <td colspan="4" width="172">STORE <em>&lt;</em>id<em>&gt;R<sub>x</sub></em></td>

   <td width="326">store value of register <em>R<sub>x </sub></em>into variable <em>&lt;</em>id<em>&gt;</em></td>

   <td colspan="3" width="106"><em>&lt;</em>id<em>&gt; </em>← <em>R<sub>x</sub></em></td>

  </tr>

  <tr>

   <td colspan="4" width="172"></td>

   <td width="326"><strong>arithmetic instructions</strong></td>

   <td colspan="3" width="106"></td>

  </tr>

  <tr>

   <td colspan="2" width="64">ADD <em>R<sub>x</sub></em></td>

   <td width="25"><em>R<sub>y</sub></em></td>

   <td width="82"><em>R<sub>z</sub></em></td>

   <td width="326">add contents of registers <em>R<sub>y </sub></em>and <em>R<sub>z </sub></em>, and store result into register <em>R<sub>x</sub></em></td>

   <td width="23"><em>R<sub>x</sub></em></td>

   <td width="42">← <em>R<sub>y</sub></em></td>

   <td width="42">+ <em>R<sub>z</sub></em></td>

  </tr>

  <tr>

   <td colspan="2" width="64">SUB <em>R<sub>x</sub></em></td>

   <td width="25"><em>R<sub>y</sub></em></td>

   <td width="82"><em>R<sub>z</sub></em></td>

   <td width="326">subtract contents of register <em>R<sub>z </sub></em>from register<em>R<sub>y </sub></em>, and store result into register <em>R<sub>x</sub></em></td>

   <td width="23"><em>R<sub>x</sub></em></td>

   <td width="42">← <em>R<sub>y</sub></em></td>

   <td width="42">− <em>R<sub>z</sub></em></td>

  </tr>

  <tr>

   <td colspan="4" width="172">MUL <em>R<sub>x </sub>R<sub>y </sub>R<sub>z</sub></em></td>

   <td width="326">multiply contents of registers <em>R<sub>y </sub></em>and <em>R<sub>z </sub></em>, and store result into register <em>R<sub>x</sub></em></td>

   <td colspan="3" width="106"><em>R</em><em>x </em>← <em>R</em><em>y </em>∗ <em>R</em><em>z</em></td>

  </tr>

  <tr>

   <td colspan="4" width="172"></td>

   <td width="326"><strong>logical instructions</strong></td>

   <td colspan="3" width="106"></td>

  </tr>

  <tr>

   <td width="57">AND <em>R<sub>x</sub></em></td>

   <td colspan="3" width="115"><em>R</em><em>y </em><em>R</em><em>z</em></td>

   <td width="326">apply bit wise AND operation to contents of registers <em>R<sub>y </sub></em>and <em>R<sub>z </sub></em>, and store result into register <em>R<sub>x</sub></em></td>

   <td colspan="3" width="106"><em>R</em><em>x </em>← <em>R</em><em>y </em>&amp; <em>R</em><em>z</em></td>

  </tr>

  <tr>

   <td colspan="4" width="172">OR <em>R</em><em>x </em><em>R</em><em>y </em><em>R</em><em>z</em></td>

   <td width="326">apply bit wise OR operation to contents of registers <em>R<sub>y </sub></em>and <em>R<sub>z </sub></em>, and store result into register <em>R<sub>x</sub></em></td>

   <td colspan="3" width="106"><em>R</em><em>x </em>← <em>R</em><em>y </em>| <em>R</em><em>z</em></td>

  </tr>

  <tr>

   <td colspan="4" width="172"></td>

   <td width="326"><strong>I/O instructions</strong></td>

   <td colspan="3" width="106"></td>

  </tr>

  <tr>

   <td colspan="4" width="172">READ <em>&lt;</em>id<em>&gt;</em></td>

   <td width="326">read value of variable <em>&lt;</em>id<em>&gt; </em>from standard input</td>

   <td colspan="3" width="106">read( <em>&lt;</em>id<em>&gt; </em>)</td>

  </tr>

  <tr>

   <td colspan="4" width="172">WRITE <em>&lt;</em>id<em>&gt;</em></td>

   <td width="326">write value of variable <em>&lt;</em>id<em>&gt; </em>to standard output</td>

   <td colspan="3" width="106">print( <em>&lt;</em>id<em>&gt; </em>)</td>

  </tr>

  <tr>

   <td width="57"></td>

   <td width="8"></td>

   <td width="25"></td>

   <td width="82"></td>

   <td width="326"></td>

   <td width="23"></td>

   <td width="42"></td>

   <td width="42"></td>

  </tr>

 </tbody>

</table>

<h2>1.3           Dead Code Elimination</h2>

Our tinyL language does not contain any control flow constructs (e.g.: jumps, if-then-else, while). This means that every generated instruction will be executed. However, if the execution of an operation or instruction does not contribute to the input/output behavior of the program, the instruction is considered “dead code” and therefore can be eliminated without changing the semantics of the program. Please note that READ instructions may not be eliminated although the read value may have no impact on the outcome of the program.

All READ instructions are kept since the overall input/output program behavior needs to be preserved.

Your dead-code eliminator will take a list of RISC instructions as input. For example, in the following code

LOADI <em>R<sub>x </sub></em>#<em>c</em><sub>1 </sub>LOADI <em>R<sub>y </sub></em>#<em>c</em><sub>2</sub>

LOADI <em>R<sub>z </sub></em>#<em>c</em><sub>3</sub>

ADD <em>R</em><sub>1 </sub><em>R<sub>x </sub>R<sub>y</sub></em>

MUL <em>R</em><sub>2 </sub><em>R<sub>x </sub>R<sub>y</sub></em>

STORE a <em>R</em><sub>1</sub>

WRITE a

the MUL instruction and the third LOADI instruction can be deleted without changing the

semantics of the program. Therefore, your dead-eliminator should produce the code:

LOADI <em>R<sub>x </sub></em>#<em>c</em><sub>1</sub>

LOADI <em>R<sub>y </sub></em>#<em>c</em><sub>2</sub>

ADD <em>R</em><sub>1 </sub><em>R<sub>x </sub>R<sub>y</sub></em>

STORE a <em>R</em><sub>1</sub>

WRITE a

<h1>2             Project Description</h1>

The project consists of two components:

<ol>

 <li>Complete the partially implemented recursive descent LL(1) parser that generates RISCmachine instructions.</li>

 <li>Write a dead-code eliminator that recognizes and deletes redundant, i.e., dead RISCmachine instructions.</li>

</ol>

The project represents an entire programming environment consisting of a compiler, an optimizer, and a virtual machine (RISC machine interpreter). You are required to implement the compiler and the optimizer (described in Section 2.1 and 2.2). The virtual machine is provided to you (described in Section 2.3).

<h2>2.1           Compiler</h2>

The recursive descent LL(1) parser implements a simple code generator. You should follow the main structure of the code as given to you in file Compiler.c. As given to you, the file contains code for function digit, assign, and print, as well as partial code for function expr. As is, the compiler is able to generate code only for expressions that contain “+” operations and constants. You will need to add code in the provided stubs to generate correct RISC machine code for the entire program. Do not change the signatures of the recursive functions. Note: The left-hand and right-hand occurrences of variables are treated differently.

<h2>2.2           Dead Code Elimination Optimization</h2>

The dead code elimination optimizer expect the input file to be provided at the standard input (stdin), and will write the generated code back to standard output (stdout).

The basic algorithm identifies “crucial” instructions. The initial crucial instructions are all READ and WRITE instructions. For all WRITE instruction, the algorithm has to detect all instructions that contribute to the value of the variable that is written out. First, you will need to find the store instruction that stores a value into the variable that is written out. This STORE instruction is marked as critical and will reference a register, let’s say <em>Rm</em>. Then you will find the instructions on which the computation of the register <em>Rm </em>depends on and mark them as critical. This marking process terminates when no more instructions can be marked as critical. If this basic algorithm is performed for all WRITE instructions, the instructions that were not marked critical can be deleted.

Instructions that are deleted as part of the optimization process have to be explicitly deallocated using the C free command in order to avoid memory leaks. You will implement your <em>dead code elimination </em>pass in the file Optimizer.c. All of your “helper” functions should be implemented in this file.

<h2>2.3           Virtual Machine</h2>

The virtual machine executes a RISC machine program. If a READ <em>&lt;</em>id<em>&gt; </em>instruction is executed, the user is asked for the value of <em>&lt;</em>id<em>&gt; </em>from standard input (stdin). If a WRITE <em>&lt;</em>id<em>&gt; </em>instruction is executed, the current value of <em>&lt;</em>id<em>&gt; </em>is written to standard output (stdout). The virtual machine is implemented in file Interpreter.c. DO NOT MODIFY this file. It is there only for your convenience so that you may be able to copy the source code of the virtual machine, for instance, to your laptop and compile it there. Be aware that you are welcome to work on your project using your own computer, however, you need to make sure your code will eventually compile and run on the ilab cluster. All grading will be done on ilab.

The virtual machine assumes that an arbitrary number of registers are available (called virtual registers), and that there are only six memory locations that can be accessed using variable names (‘a’ … ‘e’ ‘f’). In a real compiler, an additional optimization pass maps virtual registers to the limited number of physical registers of a machine. This step is typically called <em>register allocation</em>. You do not have to implement register allocation in this project. The virtual machine (RISC machine language interpreter) will report the overall number of executed instructions for a given input program. This allows you to assess the effectiveness of your optimization component.




<h1>4             How To Get Started</h1>

Create your own directory on the ilab cluster, and copy the provided project code package to your directory. Make sure that the read, write, and execute permissions for groups and others are disabled (chmod go-rwx <em>&lt;</em>directory name<em>&gt;</em>).

Type make compile to generate the compiler. To run the compiler on a test case “sample2.tinyL” in the folder tests/, type ./compile tests/sample2.tinyL. This will generate a RISC machine program in the file tinyL.out. To create your optimizer, type make optimize. The initial (provided) version of the optimizer does not work at all.

To call your optimizer on a file that contains RISC machine code, for instance file tinyL.out, type ./optimize &lt; tinyL.out &gt; optimized.out. This will generate a new file optimized.out containing the output of your optimizer. The operators “&lt;” and “&gt;” are Linux redirection operators for standard input (stdin) and standard output (stdout), respectively. Without those, the optimizer will expect instructions to be entered on the Linux command line, and will write the output to your screen.

You may want to use valgrind for memory leak detection. We recommend to use the following flags, in this case to test the optimizer for memory leaks:

valgrind –leak-check=full –show-reachable=yes –track-origins=yes ./optimize &lt; tinyL.out

The RISC virtual machine (RISC machine program interpreter) can be generated by typing make run. The distributed version of the VM in Interpreter.c is complete and should not be changed. To run a program on the virtual machine, for instance tinyL.out, type ./run tinyL.out. If the program contains READ instructions, you will be prompted at the Linux command line to enter a value. Finally, you can define a <strong>tinyL language interpreter </strong>on a single Linux command line as follows:

./compile test1; ./optimize &lt; tinyL.out &gt; opt.out; ./run opt.out.

The “;” operator allows you to specify a sequence of Linux commands on a single command line.


