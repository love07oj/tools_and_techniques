# Intro to x86-64

### Task 1 Description and Objectives 

This room will look at the basic primitives of Intel's x86-64 assembly language, 
and will use these primitives to understand the construction of basic programs using loops, functions and procedures.
The username of the machine attached to the next task is tryhackme and the password is reismyfavl33t. 

Here are a few things to note before beginning the room:

   * this room will use the AT&T syntax. In general, people either use the AT&T syntax or the Intel Syntax.
   * This room aims to be a gentle introduction to radare2. While they are not shown here, radare has a lot of powerful features and tools.
   * As soon as your start r2, remember to enter e asm.syntax=att to ensure that you are using the AT&T syntax.
   * The addresses shown on the images in the tasks below may be different from the addresses you view when you disassemble the files.
   

### Task 2 Introduction 


Computers execute machine code, which is encoded as bytes, to carry out tasks on a computer. Since different computers have different processors,
the machine code executed on these computers is specific to the processor. In this case, we’ll be looking at the Intel x86-64 instruction set architecture which 
is most commonly found today. Machine code is usually represented by a more readable form of the code called assembly code. This machine is code is usually produced by a compiler,
which takes the source code of a file, and after going through some intermediate stages, produces machine code that can be executed by a computer. 
Without going into too much detail, Intel first started out by building 16-bit instruction set, followed by 32 bit, after which they finally created 64 bit.
All these instruction sets have been created for backward compatibility, so code compiled for 32 bit architecture will run on 64 bit machines. As mentioned earlier,
before an executable file is produced, the source code is first compiled into assembly(.s files), after which the assembler converts it into an object program(.o files), 
and operations with a linker finally make it an executable. 


The best way to actually start explaining assembly is by diving in. We’ll be using radare2 to do this - radare2 is a framework for reverse engineering and analysing binaries.
It can be used to disassemble binaries(translate machine code to assembly, which is actually readable) and debug said binaries
(by allowing a user to step through the execution and view the state of the program). 

The first step is to execute the program intro by running

``` ./intro ```

```console
tryhackme@ip-10-10-45-203:~/introduction$ ./intro
value for a is 1 and b is 2
value of a is 2 and b is 1
```

From the execution, it can be seen that the program is creating two variables and switching their values. Time to see what it’s actually doing under the hood!

Go to the introduction folder on the virtual machine and run the command:

```r2 -d intro ```

This will open the binary in debugging mode. Once the binary is open, one of the first things to do is ask r2 to analyze the program, and this can be done by typing in:```aa ```

Which is the most common analysis command. It analyses all symbols and entry points in the executable.
Then run

```e asm.syntax=att ```

to set the disassembly syntax to AT&T.


The analysis in this case involves extracting function names, flow control information and much more! r2 instructions are usually based on a single character, so it is easy to get more information about the commands. For general help, run:

```? ```

For more specific information, for example, about analysis, run

```a? ```


Once the analysis is complete, you would want to know where to start analysing from - most programs have an entry point defined as main. To find a list of the functions run:

```afl ```



```sh
tryhackme@ip-10-10-45-203:~/introduction$ r2 -d intro
Process with PID 1490 started...
= attach 1490 1490
bin.baddr 0x55ecc0669000
Using 0x55ecc0669000
asm.bits 64
 -- Can you you challenge a perfect immortal machine?
[0x7f9697063090]> aa
[x] Analyze all flags starting with sym. and entry0 (aa)
[0x7f9697063090]> e asm.syntax=att
[0x7fd8c08ac090]> afl
0x55b7fdad6560    1 42           entry0
0x55b7fdcd6fe0    1 4124         reloc.__libc_start_main
0x55b7fdad6590    4 50   -> 40   sym.deregister_tm_clones
0x55b7fdad65d0    4 66   -> 57   sym.register_tm_clones
0x55b7fdad6620    5 58   -> 51   entry.fini0
0x55b7fdad6550    1 6            sym..plt.got
0x55b7fdad6660    1 10           entry.init0
0x55b7fdad6730    1 2            sym.__libc_csu_fini
0x55b7fdad6734    1 9            sym._fini
0x55b7fdad66c0    4 101          sym.__libc_csu_init
0x55b7fdad666a    1 78           main
0x55b7fdad6540    1 6            sym.imp.__printf_chk
0x55b7fdad6510    3 23           sym._init
0x55b7fdad6000    3 97   -> 123  map.home_tryhackme_introduction_intro.r_x

```

As seen here, there actually is a function at main. Let’s examine the assembly code at main by running the command

```pdf @main ```

Where pdf means print disassembly function. Doing so will give us the following view

```sh
[0x7f1335102090]> pdf @main
/ (fcn) main 78
|   int main (int argc, char **argv, char **envp);
|           ; DATA XREF from entry0 (0x5641ba19e57d)
|           0x5641ba19e66a      4883ec08       subq $8, %rsp
|           0x5641ba19e66e      b902000000     movl $2, %ecx
|           0x5641ba19e673      ba01000000     movl $1, %edx
|           0x5641ba19e678      488d35c90000.  leaq str.value_for_a_is__d_and_b_is__d, %rsi ; 0x5641ba19e748 ; "value for a is %d and b is %d\n"
|           0x5641ba19e67f      bf01000000     movl $1, %edi
|           0x5641ba19e684      b800000000     movl $0, %eax
|           0x5641ba19e689      e8b2feffff     callq sym.imp.__printf_chk
|           0x5641ba19e68e      b901000000     movl $1, %ecx
|           0x5641ba19e693      ba02000000     movl $2, %edx
|           0x5641ba19e698      488d35c90000.  leaq str.value_of_a_is__d_and_b_is__d, %rsi ; 0x5641ba19e768 ; "value of a is %d and b is %d\n"
|           0x5641ba19e69f      bf01000000     movl $1, %edi
|           0x5641ba19e6a4      b800000000     movl $0, %eax
|           0x5641ba19e6a9      e892feffff     callq sym.imp.__printf_chk
|           0x5641ba19e6ae      b800000000     movl $0, %eax
|           0x5641ba19e6b3      4883c408       addq $8, %rsp
\           0x5641ba19e6b7      c3             retq
```


As we can see from above, the values on the complete left column are memory addresses of the instructions, and these are usually stored in a structure called the stack(which we will talk about later). The middle column contains the instructions encoded in bytes(what is usually the machine code), and the last column actually contains the human readable instructions. 

The core of assembly language involves using registers to do the following:

  * Transfer data between memory and register, and vice versa

  * Perform arithmetic operations on registers and data

  * Transfer control to other parts of the program


Since the architecture is x86-64, the registers are 64 bit and Intel has a list of 16 registers:

64 bit | 32 bit
-------|-------
%rax | %eax
%rbx | %ebx
%rcx | %ecx
%rdx | %edx
%rsi | %esi
%rdi | %edi
%rsp | %esp
%rbp | %ebp
%r8 | %r8d
%r9 | %r9d
%r10 | %r10d
%r11 | %r11d
%r12 | %r12d
%r13 | %r13d
%r14 | %r14d
%r15 | %r15d





Even though the registers are 64 bit, meaning they can hold up to 64 bits of data, other parts of the registers can also be referenced. In this case, registers can also be referenced as 32 bit values as shown. What isn’t shown is that registers can be referenced as 16 bit and 8 bit(higher 4 bit and lower 4 bit). 

The first 6 registers are known as general purpose registers. The ```%rsp ``` is the stack pointer and it points to the top of the stack which contains the most recent memory address. The stack is a data structure that manages memory for programs. ```%rbp ``` is a frame pointer and points to the frame of the function currently being executed - every function is executed in a new frame. To move data using registers, the following instruction is used:

``` movq source, destination ```

This involves:

  * Transferring constants(which are prefixed using the $ operator) e.g. ```movq $3 rax ``` would move the constant 3 to the register
  * Transferring values from a register e.g.``` movq %rax %rbx ``` which involves moving value from rax to rbx
  * Transferring values from memory which is shown by putting registers inside brackets e.g. ``` movq %rax (%rbx) ``` which means move value stored in ```%rax ``` to memory location represented by ``` %rbx. ```

The last letter of the mov instruction represents the size of the data:

Intel Data Type | Suffix |Size(bytes)
----------------|--------|-----------
Byte | b | 1
Word | w | 2
Double Word | l | 4
Quad Word | q | 8
Single Precision | s | 4
Double Precision | l | 8

When dealing with memory manipulation using registers, there are other cases to be considered:

   * (Rb, Ri) = MemoryLocation[Rb + Ri]
   * D(Rb, Ri) = MemoryLocation[Rb + Ri + D]
   * (Rb, Ri, S) = MemoryLocation(Rb + S * Ri]
   * D(Rb, Ri, S) = MemoryLocation[Rb + S * Ri + D]

Some other important instructions are:

  * ```leaq source, destination ```: this instruction sets destination to the address denoted by the expression in source

  * ```addq source, destination ```: destination = destination + source

  * ```subq source, destination ```: destination = destination - source

  * ```imulq source, destination ```: destination = destination * source

  * ```salq source, destination ```: destination = destination << source where << is the left bit shifting operator

  * ```sarq source, destination ```: destination = destination >> source where >> is the right bit shifting operator

  * ```xorq source, destination ```: destination = destination XOR source

  * ```andq source, destination ```: destination = destination & source

  * ```orq source, destination ```: destination = destination | source

Before understanding how programs work, it is important to understand registers, memory manipulation and some basic instructions. The next sections will have more hands on use of radare2.


### Task 3  If Statements 



The general format of an if statement is

```c
if(condition){

  do-stuff-here

}else if(condition) //this is an optional condition {

  do-stuff-here

}else {

  do-stuff-here

}
```

If statements use 3 important instructions in assembly:

   * ```cmpq source2, source1 ```: it is like computing a-b without setting destination

   * ```testq source2, source1 ```: it is like computing a&b without setting destination

Jump instructions are used to transfer control to different instructions, and there are different types of jumps:

Jump Type | Description
----------|------------
jmp |Unconditional
je | Equal/Zero
jne | Not Equal/Not Zero
js  | Negative
jns | Nonnegative
jg | Greater
jge | Greater or Equal
jl | Less
jle | Less or Equal
ja | Above(unsigned)
jb | Below(unsigned)

	
The last 2 values of the table refer to unsigned integers. Unsigned integers cannot be negative while signed integers represent both positive and negative values. SInce the computer needs to differentiate between them, it uses different methods to interpret these values. For signed integers, it uses something called the two’s complement representation and for unsigned integers it uses normal binary calculations. 


### Task 4  If Statements Continued 


Go to the if-statement folder and Start r2 with ``` r2 -d if1 ```

And run the following commands:

``` aaa ```
``` afl ```
``` pdf @main ```

This analyses the program, lists the functions and disassembles the main function.

```sh 
[0x7f6753fe2090]> pdf @main
/ (fcn) main 43
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x563d55d6350d)
|           0x563d55d635fa      55             pushq %rbp
|           0x563d55d635fb      4889e5         movq %rsp, %rbp
|           0x563d55d635fe      c745f8030000.  movl $3, var_8h
|           0x563d55d63605      c745fc040000.  movl $4, var_4h
|           0x563d55d6360c      8b45f8         movl var_8h, %eax
|           0x563d55d6360f      3b45fc         cmpl var_4h, %eax
|       ,=< 0x563d55d63612      7d06           jge 0x563d55d6361a
|       |   0x563d55d63614      8345f805       addl $5, var_8h
|      ,==< 0x563d55d63618      eb04           jmp 0x563d55d6361e
|      |`-> 0x563d55d6361a      8345fc03       addl $3, var_4h
|      |    ; CODE XREF from main (0x563d55d63618)
|      `--> 0x563d55d6361e      b800000000     movl $0, %eax
|           0x563d55d63623      5d             popq %rbp
\           0x563d55d63624      c3             retq
```

 We’ll then start by setting a break point on the ``` jge ``` and the ``` jmp ``` instruction by using the command:

``` db 0x55ae52836612 ``` (which is the hex address of the ``` jge ``` instruction) 

``` db 0x55ae52836618 ``` (which is the hex address of the ``` jmp ``` instruction)


We’ve added breakpoints to stop the execution of the program at those points so we can see the state of the program. Doing so will show the following:


```sh 
[0x7f6753fe2090]> pdf @main
/ (fcn) main 43
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x563d55d6350d)
|           0x563d55d635fa      55             pushq %rbp
|           0x563d55d635fb      4889e5         movq %rsp, %rbp
|           0x563d55d635fe      c745f8030000.  movl $3, var_8h
|           0x563d55d63605      c745fc040000.  movl $4, var_4h
|           0x563d55d6360c      8b45f8         movl var_8h, %eax
|           0x563d55d6360f      3b45fc         cmpl var_4h, %eax
|       ,=< 0x563d55d63612 b    7d06           jge 0x563d55d6361a
|       |   0x563d55d63614      8345f805       addl $5, var_8h
|      ,==< 0x563d55d63618 b    eb04           jmp 0x563d55d6361e
|      |`-> 0x563d55d6361a      8345fc03       addl $3, var_4h
|      |    ; CODE XREF from main (0x563d55d63618)
|      `--> 0x563d55d6361e      b800000000     movl $0, %eax
|           0x563d55d63623      5d             popq %rbp
\           0x563d55d63624      c3             retq
```



We now run ```dc ``` to start execution of the program and the program will start execution and stop at the break point. Let’s examine what has happened before hitting the breakpoint:

   * The first 2 lines are about pushing the frame pointer onto the stack and saving it(this is about how functions are called, and will be examined later)

   * The next 3 lines are about assigning values 3 and 4 to the local arguments/variables var_8h and var_4h. It then stores the value in var_8h in the %eax register. 

   * The ```cmpl ``` instruction compares the value of eax with that of the var_8h argument

To view the value of the registers, type in:```dr ```

```sh 
[0x563d55d63612]> dr
rax = 0x00000003
rbx = 0x00000000
rcx = 0x563d55d63630
rdx = 0x7ffc1cb396d8
r8 = 0x7f6753fdcd80
r9 = 0x7f6753fdcd80
r10 = 0x00000000
r11 = 0x00000000
r12 = 0x563d55d634f0
r13 = 0x7ffc1cb396c0
r14 = 0x00000000
r15 = 0x00000000
rsi = 0x7ffc1cb396c8
rdi = 0x00000001
rsp = 0x7ffc1cb395e0
rbp = 0x7ffc1cb395e0
rip = 0x563d55d63612
rflags = 0x00000297
orax = 0xffffffffffffffff
```

We can see that the value of rax, which is the 64 bit version of eax contains 3. We saw that the jge instruction is jumping based on whether value of eax is greater than var_4h. To see what’s in var_4h, we can see that at top of the main function, it tells us the position of var_4h. Run the command: ``` px @rbp-0x4 ```

And that shows the value of 4. 

We know that eax contains 3, and 3 is not greater than 4, so the jump will not execute. Instead it will move to the next instruction. To check this, run the ```ds ``` command which seeks/moves onto the next instruction.

```sh 
[0x563d55d63612]> ds
[0x563d55d63612]> pdf @main
/ (fcn) main 43
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x563d55d6350d)
|           0x563d55d635fa      55             pushq %rbp
|           0x563d55d635fb      4889e5         movq %rsp, %rbp
|           0x563d55d635fe      c745f8030000.  movl $3, var_8h
|           0x563d55d63605      c745fc040000.  movl $4, var_4h
|           0x563d55d6360c      8b45f8         movl var_8h, %eax
|           0x563d55d6360f      3b45fc         cmpl var_4h, %eax
|       ,=< 0x563d55d63612 b    7d06           jge 0x563d55d6361a
|       |   ;-- rip:
|       |   0x563d55d63614      8345f805       addl $5, var_8h
|      ,==< 0x563d55d63618 b    eb04           jmp 0x563d55d6361e
|      |`-> 0x563d55d6361a      8345fc03       addl $3, var_4h
|      |    ; CODE XREF from main (0x563d55d63618)
|      `--> 0x563d55d6361e      b800000000     movl $0, %eax
|           0x563d55d63623      5d             popq %rbp
\           0x563d55d63624      c3             retq
```

The rip(which is the current instruction pointer) shows that it moves onto the next instruction - which shows we are correct. The current instruction then adds 5 to var_8h which is a local argument. To see that this actually happens, first check the value of var_8h, run ``` ds ``` and check the value again. This will show it increments by 5.

```sh 
[0x563d55d63612]> px @rbp-0x8
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffc1cb395d8  0300 0000 0400 0000 3036 d655 3d56 0000  ........06.U=V..                                                                                             
0x7ffc1cb395e8  971b c153 677f 0000 0100 0000 0000 0000  ...Sg...........
0x7ffc1cb395f8  c896 b31c fc7f 0000 0080 0000 0100 0000  ................
0x7ffc1cb39608  fa35 d655 3d56 0000 0000 0000 0000 0000  .5.U=V..........
0x7ffc1cb39618  f7be b82b f269 9b83 f034 d655 3d56 0000  ...+.i...4.U=V..
0x7ffc1cb39628  c096 b31c fc7f 0000 0000 0000 0000 0000  ................
0x7ffc1cb39638  0000 0000 0000 0000 f7be 386c 39fb 19d0  ..........8l9...
0x7ffc1cb39648  f7be 4671 dc65 2fd1 0000 0000 fc7f 0000  ..Fq.e/.........
0x7ffc1cb39658  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc1cb39668  3317 ff53 677f 0000 3876 fd53 677f 0000  3..Sg...8v.Sg...
0x7ffc1cb39678  5116 0800 0000 0000 0000 0000 0000 0000  Q...............
0x7ffc1cb39688  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc1cb39698  f034 d655 3d56 0000 c096 b31c fc7f 0000  .4.U=V..........
0x7ffc1cb396a8  1a35 d655 3d56 0000 b896 b31c fc7f 0000  .5.U=V..........
0x7ffc1cb396b8  1c00 0000 0000 0000 0100 0000 0000 0000  ................
0x7ffc1cb396c8  79b7 b31c fc7f 0000 0000 0000 0000 0000  y...............
[0x563d55d63612]> ds
[0x563d55d63612]> px @rbp-0x8
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffc1cb395d8  0800 0000 0400 0000 3036 d655 3d56 0000  ........06.U=V..                                                                                             
0x7ffc1cb395e8  971b c153 677f 0000 0100 0000 0000 0000  ...Sg...........
0x7ffc1cb395f8  c896 b31c fc7f 0000 0080 0000 0100 0000  ................
0x7ffc1cb39608  fa35 d655 3d56 0000 0000 0000 0000 0000  .5.U=V..........
0x7ffc1cb39618  f7be b82b f269 9b83 f034 d655 3d56 0000  ...+.i...4.U=V..
0x7ffc1cb39628  c096 b31c fc7f 0000 0000 0000 0000 0000  ................
0x7ffc1cb39638  0000 0000 0000 0000 f7be 386c 39fb 19d0  ..........8l9...
0x7ffc1cb39648  f7be 4671 dc65 2fd1 0000 0000 fc7f 0000  ..Fq.e/.........
0x7ffc1cb39658  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc1cb39668  3317 ff53 677f 0000 3876 fd53 677f 0000  3..Sg...8v.Sg...
0x7ffc1cb39678  5116 0800 0000 0000 0000 0000 0000 0000  Q...............
0x7ffc1cb39688  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffc1cb39698  f034 d655 3d56 0000 c096 b31c fc7f 0000  .4.U=V..........
0x7ffc1cb396a8  1a35 d655 3d56 0000 b896 b31c fc7f 0000  .5.U=V..........
0x7ffc1cb396b8  1c00 0000 0000 0000 0100 0000 0000 0000  ................
0x7ffc1cb396c8  79b7 b31c fc7f 0000 0000 0000 0000 0000  y...............
```

Note that because we are checking the exact address, we only need to check to 0 offset. The value stored in memory is stored as hex. 


The next instruction is an unconditional jump and it just jumps to clearing the eax register. The ``` popq ``` instruction involves popping a value of the stack and reading it, and the return instruction sets this popped value to the current instruction pointer. In this case, it shows the execution of the program has been completed. To understand better about how an if statement work, you can check the corresponding C file in the same folder.


The following questions involve analysing the if2 binary.

**1 What is the value of var_8h before the popq and ret instructions?**

96

**2 what is the value of var_ch before the popq and ret instructions?**

0

**3 What is the value of var_4h before the popq and ret instructions?**

1

**4 What operator is used to change the value of var_8h, input the symbol as your answer(symbols include +, -, * , /, &, |)?**

&


## Task 5  Loops 

Usually two types of loops are used: for loops and while loops. The general format of a while loops is:

```c
while(condition){

  Do-stuff-here

  Change value used in condition

}
```

The general format of a for loop is

```c 
for(initialise value: condition; change value used in condition){

  do-stuff-here

}
```

Let’s start looking up loops by entering the loops folder, running r2 with the loops 1 file. After this, analyse everything, list the functions and disassemble the main function. 

```sh 
[0x7f76c8c36090]> pdf @main
/ (fcn) main 44
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_ch @ rbp-0xc
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x55d6dc04750d)
|           0x55d6dc0475fa      55             pushq %rbp
|           0x55d6dc0475fb      4889e5         movq %rsp, %rbp
|           0x55d6dc0475fe      c745f4040000.  movl $4, var_ch
|           0x55d6dc047605      c745f8090000.  movl $9, var_8h
|           0x55d6dc04760c      c745fc0a0000.  movl $0xa, var_4h
|       ,=< 0x55d6dc047613      eb04           jmp 0x55d6dc047619
|      .--> 0x55d6dc047615      8345f402       addl $2, var_ch
|      :|   ; CODE XREF from main (0x55d6dc047613)
|      :`-> 0x55d6dc047619      837df408       cmpl $8, var_ch
|      `==< 0x55d6dc04761d      7ef6           jle 0x55d6dc047615
|           0x55d6dc04761f      b800000000     movl $0, %eax
|           0x55d6dc047624      5d             popq %rbp
\           0x55d6dc047625      c3             retq
```

Let start of by setting a break point at the jmp instruction using the command: ```db address-of-instruction ```

Doing this allows use to skip the first few lines of instructions, which as we saw using if statements, it just passing in values to local arguments(note that the constant showed by $0xa represents that value of 10 in hex). Once execution reaches the breakpoint at the jmp instruction, run ```ds ``` to move to the next instruction. Since this is an unconditional jump, it will move to the cmpl instruction.

```sh 
[0x55d6dc047613]> pdf @main
            ;-- rax:
/ (fcn) main 44
|   int main (int argc, char **argv, char **envp);
|           ; var int32_t var_ch @ rbp-0xc
|           ; var int32_t var_8h @ rbp-0x8
|           ; var int32_t var_4h @ rbp-0x4
|           ; DATA XREF from entry0 (0x55d6dc04750d)
|           0x55d6dc0475fa      55             pushq %rbp
|           0x55d6dc0475fb      4889e5         movq %rsp, %rbp
|           0x55d6dc0475fe      c745f4040000.  movl $4, var_ch
|           0x55d6dc047605      c745f8090000.  movl $9, var_8h
|           0x55d6dc04760c      c745fc0a0000.  movl $0xa, var_4h
|       ,=< 0x55d6dc047613 b    eb04           jmp 0x55d6dc047619
|      .--> 0x55d6dc047615      8345f402       addl $2, var_ch
|      :|   ;-- rip:
|      :|   ; CODE XREF from main (0x55d6dc047613)
|      :`-> 0x55d6dc047619      837df408       cmpl $8, var_ch
|      `==< 0x55d6dc04761d      7ef6           jle 0x55d6dc047615
|           0x55d6dc04761f      b800000000     movl $0, %eax
|           0x55d6dc047624      5d             popq %rbp
\           0x55d6dc047625      c3             retq

```

Here the ```cmpl ``` instruction is trying to compare what’s in the local argument var_ch with the value 8. To see what’s in var_ch, check the start of the disassembled function and check the memory. In this case, it is ``` rbp-0xc ```

```sh 
[0x55d6dc047613]> px @ rbp-0xc
- offset -       0 1  2 3  4 5  6 7  8 9  A B  C D  E F  0123456789ABCDEF
0x7ffe5498d8e4  0400 0000 0900 0000 0a00 0000 3076 04dc  ............0v..                                                                                             
0x7ffe5498d8f4  d655 0000 975b 86c8 767f 0000 0100 0000  .U...[..v.......
0x7ffe5498d904  0000 0000 d8d9 9854 fe7f 0000 0080 0000  .......T........
0x7ffe5498d914  0100 0000 fa75 04dc d655 0000 0000 0000  .....u...U......
0x7ffe5498d924  0000 0000 46ea b5f4 2749 9b74 f074 04dc  ....F...'I.t.t..
0x7ffe5498d934  d655 0000 d0d9 9854 fe7f 0000 0000 0000  .U.....T........
0x7ffe5498d944  0000 0000 0000 0000 0000 0000 46ea d5aa  ............F...
0x7ffe5498d954  1e58 ca20 46ea 4bae 2360 db21 0000 0000  .X. F.K.#`.!....
0x7ffe5498d964  fe7f 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffe5498d974  0000 0000 3357 c4c8 767f 0000 38b6 c2c8  ....3W..v...8...
0x7ffe5498d984  767f 0000 fb5a 0700 0000 0000 0000 0000  v....Z..........
0x7ffe5498d994  0000 0000 0000 0000 0000 0000 0000 0000  ................
0x7ffe5498d9a4  0000 0000 f074 04dc d655 0000 d0d9 9854  .....t...U.....T
0x7ffe5498d9b4  fe7f 0000 1a75 04dc d655 0000 c8d9 9854  .....u...U.....T
0x7ffe5498d9c4  fe7f 0000 1c00 0000 0000 0000 0100 0000  ................
0x7ffe5498d9d4  0000 0000 7cf7 9854 fe7f 0000 0000 0000  ....|..T........
```

And shows that it contains 4. The next instruction is a ```jle ``` which is going to check is the value is var-ch is less than or equal to 8. Since 4 is less than 8, it will jump to the ``` addl ``` instruction. 



The ```addl ``` instruction will add 2 to the value of var-ch and continue to go to the ```cmpl ``` instruction. Since 2 was added to var_ch, var_ch will now contain 6 which is still less than 8, and it will jump back to the ```addl ``` instruction. This can be seeing by continuing execution using the ```ds ``` statement. We know this is a loop because the ```addl ``` instruction is being executed more than once, and this is in combination with comparing the value of var_ch to 8. So we can infer the structure of the loop to be

```c
while(var_ch < 8){

 var_ch = var_ch + 2

}
```

A quicker way to examine the loop would be to add a break point to ```cmpl ``` instruction and running dc. Since this is a loop, the program will always break at the ```cmpl ``` instruction(because this instruction checks the condition before executing what is inside the loop). You can check the loop1.c file to see the structure of the loop! 

Use the loop2 binary to answer the following questions.

**1 What is the value of var_8h on the second iteration of the loop?**

5

**2 What is the value of var_ch on the second iteration of the loop?**

0

**3 What is the value of var_8h at the end of the program?**

2

**4 What is the value of var_ch at the end of the program?**

0


## Task 6  crackme1 


 analyse the crackme1 binary. This binary checks if the user has a correct password, and this can be done by running the binary and entering the password.


**1 What is the password?**

127.0.0.1


## Task 7 crackme2

Analyse the crackme2 binary and try find the correct password, as with the previous question.

**1 What is the password?**


```console
tryhackme@ip-10-10-147-112:~/crackme$ strings crackme2
```

```/home/tryhackme/install-files/secret.txt ```

go to the directory 

```console
tryhackme@ip-10-10-147-112:~/crackme$ cat /home/tryhackme/install-files/secret.txt
vs3curepwd
```

```
echo vs3curepwd | rev
dwperuc3sv
```
