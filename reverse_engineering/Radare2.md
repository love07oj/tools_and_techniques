# # CC: Radare2

### Task 1  Intro 

It is designed to teach you how some of the more common things in radare2 are used.


### Task 2 Command Line Options 



A quick intro to some of the commonly used command line flags for radare2, some of these flags will be extremely useful for later tasks. Include all parts of the flag including the -. All flags can be found in the help menu


**1. What flag to you set to analyze the binary upon entering the r2 console (equivalent to running aaa once your inside the console)**

-A 

**2. How do you enable the debugger?**

-d

**3. How do you open the file in write mode?**

-w 

**4. How do you enter the console without opening a file**

``` - ```


### Task 3 Analyzation 

Once inside the radare console you have a myriad of options to analyze your binary. Generally all analyzation commands start with the letter ```a ```. If you want to list all possible commands that can be done with your starting letter(s) you add a question mark to the end. For example ``` a? ``` would output ```ab,aa,ac ``` along with a description on what each command does.


**1. What command "Analyzes Everything" (all functions and their arguments: Same as running with radare with -A)**

aaa

**2. What command does basic analysis on functions?**

af

**3. How do you list all functions?**

afl

**4. How many functions are in the example1 binary?**

12

**5. What is the name of the secret function in the example1 binary?**

secret_func


### Task 4 Information 

```i  ``` is a command that shows general information of the binary. Like ``` a ``` it has many sub commands each with varying degrees of specificity.


**1. What command shows all the information about the file that you're in?**

ia

**2. How do you get every string that is present in the binary?**

izz

**3. What if you want the address of the main function?**

iM

**4. What character do you add to the end of every command to get the output in JSON format?**

j

**5. How do you get the entrypoint of the file?**

ie 

**6. What is the secret string hidden in the example2 binary?**


use the command ```izzqq ``` where qq is used to show lesser and important stuff


```goodjob ```


### Task 5 Navigating Through Memory 

```s ``` is the command that is used to navigate through the memory of your binary. With it and its variations you can you can get information about where you are in the binary as well as move to different points in the binary.

Note: For user created functions that aren't main, you will have to add sym. before them for example sym.user_func


**1. How do you print out the the current memory address your located at in the binary?**

s

**2. What command do you use to go to a specific point in memory with the syntax <command> <address>?**

s

**3. What command would you run to go 5 bytes forward?**

s+ 5

**4. What about 12 bytes backward?**

s- 12

**5. How do you undo the previous seek?**

s-

**6. How would go to the memory address of the main function?**

s main

**7. What if you wanted to go to the address of the rax register?**

sr rax


### Task 6  Printing 

```p ``` is a command that shows data in a myriad of formats. The command is useful for when you want to get information about what is happening in memory, and get some of the data that's contained in memory as well. With the``` p ``` command it is also useful to know about the ```@ ``` symbol in radare. The ``` @ ``` symbol is used to specify that something is an address in memory, for example if you wanted to specify you were talking about the memory address of the main function you would use ```<command>@main ```


**1. How would you print the hex output of where you currently are in memory?**

px

**2.How would you print the disassembly of where you're currently at in memory?**

pd

**3. What if you wanted the disassembly of the main function?**

pd a main

**4. What command prints out the emoji hexdump? (this is not useful at all I just find it funny)**

pxe

**5. What if you decided you were too good for rows and you wanted the disassembly in column format?**

pC


```sh 
[0x7f76a7bd3146]> pdf @main
            ; DATA XREF from entry0 @ 0x55d7dfc6654d
┌ 25: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_8h @ rbp-0x8
│           ; var int64_t var_4h @ rbp-0x4
│           0x55d7dfc66660      55             push rbp
│           0x55d7dfc66661      4889e5         mov rbp, rsp
│           0x55d7dfc66664      c745fc010000.  mov dword [var_4h], 1
│           0x55d7dfc6666b      c745f8050000.  mov dword [var_8h], 5
│           0x55d7dfc66672      b800000000     mov eax, 0
│           0x55d7dfc66677      5d             pop rbp
└           0x55d7dfc66678      c3             ret
```

**6. What is the value of the first variable in the main function for the example 3 binary?**

1

**7. What about the second variable?**

5

### Task 7 The Mid-term 


Congrats on getting to this point, you now know enough to pass the mid-term exam. The questions in this task will all be related to commands that were in previous tasks so if you skipped one, I recommend going back and doing it. As you probably guessed from the file name all exercises in this task will be done using the midterm binary file.

**1. How many functions are in the binary?**

```sh 
[0x7f96ccc42090]> afl | wc -l
14
```

**2. What is the value of the hidden string?**

```izzqq ```

you_found_me

**3. What is the return value of secret_func()?**

4

**4. What is the value of the first variable set in the main function(in decimal format)?**

```sh [0x7f96ccc42090]> pdf @main
            ; DATA XREF from entry0 @ 0x55659a94354d
┌ 25: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_8h @ rbp-0x8
│           ; var int64_t var_4h @ rbp-0x4
│           0x55659a943660      55             push rbp
│           0x55659a943661      4889e5         mov rbp, rsp
│           0x55659a943664      c745fc0c0000.  mov dword [var_4h], 0xc ; 12
│           0x55659a94366b      c745f8c00000.  mov dword [var_8h], 0xc0 ; 192
│           0x55659a943672      b800000000     mov eax, 0
│           0x55659a943677      5d             pop rbp
└           0x55659a943678      c3             ret
```

12

**5. What about the second one(also in decimal format)?**

192

**6. What is the next function in memory after the main function?**

We simply move to the last instruction of the main function and add 1 more byte. Then print our current function.

```sh

[0x55659a943661]> s 0x55659a943678
[0x55659a943678]> s+ 1
[0x55659a943679]> pdf
┌ 7: sym.midterm_func ();
```

**7. How do you get a hexdump of four bytes of the memory address your currently at?**

px 4


### Task 8  Debugging 

Recall that in the task "Command Line Options" you learned that the -d flag has radare enter debug mode. Debug mode allows you to set breakpoints and offers a lot of options to not only navigate through your binary, but to analyze the data that goes in and out of the registers as well.


**1. How do you set a breakpoint?**

db

**2. What command is used to print out the values of all the registers?**

dr

**3. How do you run through the program until the program either ends or you hit the next breakpoint?**

dc

**4. What if you want to step through the binary one line at a time?**

ds

**5. How do you go forth 2 lines in the binary?**

ds 2 

**6. How do you list out the indexes and memory addresses of all breakpoints?**

dbi


### Task 9 Visual Mode 

While visual mode is by no means necessary and won't inherently teach you anything new about the binary you're currently running. It allows the assembly to more human readable and provides a lot of options to enhance the visual appeal of radare and can definitely improve efficiency. Therefore I would state it's a valuable tool that you should know how to use. All commands involving visual mode start with ```v ```

**1. How do you enter "graph mode" which allows everything to be organized in nice readable boxes?**

vV

**2. What character do you press to run normal radare commands inside visual mode?**

:

**3. How do you go back to the regular radare shell(leaving visual mode)?**

q

**4. What if you want to step through the binary inside Visual mode?**

s

**5. How do you add a comment?**

;

**6. Look through any of the binaries in Visual Mode and see just how much more beautiful everything looks.**

set a breakpoint if you want to enter visual mode 


### Task 10 Write Mode 


Occasionally you might end up in a situation where a task is impossible to solve with the current instructions. For example take this code 

```c
int val = 4;

if(val == 5){

printf("%s","You win!");

}
```

You will never be able to get it to print out You win! because under normal circumstances val will never be set equal to 5. This is where write mode comes in, it allows you to change instructions so you can get certain conditions to execute. All commands involving write mode start with ```w ```

**1. How do you write a string to the current memory address.**

w

**2. What command lists all write changes?**

wc

**3. What command modifies an instruction at the current memory address?**

wA

**4. Get the example4 binary to show the You win! message**

```sh 
[0x7fa0b9375090]> pdf @main
            ; DATA XREF from entry0 @ 0x556e1fdf659d
┌ 52: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_4h @ rbp-0x4
│           0x556e1fdf66b0      55             push rbp
│           0x556e1fdf66b1      4889e5         mov rbp, rsp
│           0x556e1fdf66b4      4883ec10       sub rsp, 0x10
│           0x556e1fdf66b8      c745fc040000.  mov dword [var_4h], 4
│           0x556e1fdf66bf      837dfc05       cmp dword [var_4h], 5
│       ┌─< 0x556e1fdf66c3      7518           jne 0x556e1fdf66dd
│       │   0x556e1fdf66c5      488d35980000.  lea rsi, qword str.You_win ; 0x556e1fdf6764 ; "You win!"
│       │   0x556e1fdf66cc      488d3d9a0000.  lea rdi, qword [0x556e1fdf676d] ; "%s"
│       │   0x556e1fdf66d3      b800000000     mov eax, 0
│       │   0x556e1fdf66d8      e883feffff     call sym.imp.printf     ; int printf(const char *format)
│       └─> 0x556e1fdf66dd      b800000000     mov eax, 0
│           0x556e1fdf66e2      c9             leave
└           0x556e1fdf66e3      c3             ret
[0x7fa0b9375090]> s 0x556e1fdf66bf
[0x556e1fdf66bf]> wx 837dfc04
[0x556e1fdf66bf]> exit
Do you want to quit? (Y/n) y
Do you want to kill the process? (Y/n) n
root@beetle:~/Desktop/thm/z# ./example4
You win!

```


### Task 11 The Final Exam 


**What is the password that outputs the you win! message?**

```sh
[0x7f408649f090]> pdf @main
            ; DATA XREF from entry0 @ 0x564fcfcd96bd
┌ 102: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_11h @ rbp-0x11
│           ; var int64_t var_8h @ rbp-0x8
│           0x564fcfcd9835      55             push rbp
│           0x564fcfcd9836      4889e5         mov rbp, rsp
│           0x564fcfcd9839      4883ec20       sub rsp, 0x20
│           0x564fcfcd983d      488b150c0820.  mov rdx, qword [reloc.stdin] ; [0x564fcfeda050:8]=0
│           0x564fcfcd9844      488d45ef       lea rax, qword [var_11h]
│           0x564fcfcd9848      be09000000     mov esi, 9
│           0x564fcfcd984d      4889c7         mov rdi, rax
│           0x564fcfcd9850      e80bfeffff     call sym.imp.fgets      ; char *fgets(char *s, int size, FILE *stream)
│           0x564fcfcd9855      488d45ef       lea rax, qword [var_11h]
│           0x564fcfcd9859      4889c7         mov rdi, rax
│           0x564fcfcd985c      e86fffffff     call sym.get_password
│           0x564fcfcd9861      488945f8       mov qword [var_8h], rax
│           0x564fcfcd9865      488b45f8       mov rax, qword [var_8h]
│           0x564fcfcd9869      488d35a40000.  lea rsi, qword str.youdidit ; 0x564fcfcd9914 ; "youdidit"
│           0x564fcfcd9870      4889c7         mov rdi, rax
│           0x564fcfcd9873      e8f8fdffff     call sym.imp.strcmp     ; int strcmp(const char *s1, const char *s2)
│           0x564fcfcd9878      85c0           test eax, eax
│       ┌─< 0x564fcfcd987a      7518           jne 0x564fcfcd9894
│       │   0x564fcfcd987c      488d359a0000.  lea rsi, qword str.You_win ; 0x564fcfcd991d ; "You win!"
│       │   0x564fcfcd9883      488d3d9c0000.  lea rdi, qword [0x564fcfcd9926] ; "%s"
│       │   0x564fcfcd988a      b800000000     mov eax, 0
│       │   0x564fcfcd988f      e8bcfdffff     call sym.imp.printf     ; int printf(const char *format)
│       └─> 0x564fcfcd9894      b800000000     mov eax, 0
│           0x564fcfcd9899      c9             leave
└           0x564fcfcd989a      c3             rest
```


```sh 
[0x7f408649f090]> pdf @sym.get_password
            ; CALL XREF from main @ 0x564fcfcd985c
┌ 101: sym.get_password (int64_t arg1);
│           ; var int64_t var_28h @ rbp-0x28
│           ; var int64_t var_15h @ rbp-0x15
│           ; var int64_t var_14h @ rbp-0x14
│           ; var int64_t var_10h @ rbp-0x10
│           ; var int64_t var_4h @ rbp-0x4
│           ; arg int64_t arg1 @ rdi
│           0x564fcfcd97d0      55             push rbp
│           0x564fcfcd97d1      4889e5         mov rbp, rsp
│           0x564fcfcd97d4      4883ec30       sub rsp, 0x30
│           0x564fcfcd97d8      48897dd8       mov qword [var_28h], rdi ; arg1
│           0x564fcfcd97dc      bf09000000     mov edi, 9
│           0x564fcfcd97e1      e89afeffff     call sym.imp.malloc     ;  void *malloc(size_t size)
│           0x564fcfcd97e6      488945f0       mov qword [var_10h], rax
│           0x564fcfcd97ea      c745fc000000.  mov dword [var_4h], 0
│       ┌─< 0x564fcfcd97f1      eb36           jmp 0x564fcfcd9829
│      ┌──> 0x564fcfcd97f3      8b45fc         mov eax, dword [var_4h]
│      ╎│   0x564fcfcd97f6      4863d0         movsxd rdx, eax
│      ╎│   0x564fcfcd97f9      488b45d8       mov rax, qword [var_28h]
│      ╎│   0x564fcfcd97fd      4801d0         add rax, rdx
│      ╎│   0x564fcfcd9800      0fb600         movzx eax, byte [rax]
│      ╎│   0x564fcfcd9803      0fbec0         movsx eax, al
│      ╎│   0x564fcfcd9806      83c00a         add eax, 0xa
│      ╎│   0x564fcfcd9809      8945ec         mov dword [var_14h], eax
│      ╎│   0x564fcfcd980c      8b45ec         mov eax, dword [var_14h]
│      ╎│   0x564fcfcd980f      8845eb         mov byte [var_15h], al
│      ╎│   0x564fcfcd9812      8b45fc         mov eax, dword [var_4h]
│      ╎│   0x564fcfcd9815      4863d0         movsxd rdx, eax
│      ╎│   0x564fcfcd9818      488b45f0       mov rax, qword [var_10h]
│      ╎│   0x564fcfcd981c      4801c2         add rdx, rax
│      ╎│   0x564fcfcd981f      0fb645eb       movzx eax, byte [var_15h]
│      ╎│   0x564fcfcd9823      8802           mov byte [rdx], al
│      ╎│   0x564fcfcd9825      8345fc01       add dword [var_4h], 1
│      ╎│   ; CODE XREF from sym.get_password @ 0x564fcfcd97f1
│      ╎└─> 0x564fcfcd9829      837dfc07       cmp dword [var_4h], 7
│      └──< 0x564fcfcd982d      7ec4           jle 0x564fcfcd97f3
│           0x564fcfcd982f      488b45f0       mov rax, qword [var_10h]
│           0x564fcfcd9833      c9             leave
└           0x564fcfcd9834      c3             ret
```



```oekZ_Z_j ```
