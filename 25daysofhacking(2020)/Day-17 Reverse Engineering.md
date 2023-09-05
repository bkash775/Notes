
2. Introduction to x86-64 Assembly  

Computers execute machine code, which is encoded as bytes, to carry out tasks on a computer. Since different computers have different processors, the machine code executed on these computers is specific to the processor. In this case, we’ll be looking at the Intel x86-64 instruction set architecture which is most commonly found today. Machine code is usually represented by a more readable form of the code called assembly code. This machine is code is usually produced by a compiler, which takes the source code of a file, and after going through some intermediate stages, produces machine code that can be executed by a computer.

Without going into too much detail, Intel first started out by building a 16-bit instruction set, followed by 32 bit, after which they finally created 64 bit. All these instruction sets have been created for backward compatibility, so code compiled for 32-bit architecture will run on 64-bit machines. As mentioned earlier, before an executable file is produced, the source code is first compiled into assembly(.s files), after which the assembler converts it into an object program(.o files), and operations with a linker finally make it an executable. 

The best way to actually start explaining assembly is by diving in. We’ll be using `radare2` to do this - [radare2](https://github.com/radareorg/radare2) is a framework for reverse engineering and analysing binaries. It can be used to disassemble binaries(translate machine code to assembly, which is actually readable) and debug said binaries(by allowing a user to step through the execution and view the state of the program).

---

Luckily for us, everything we need has been provided to you via an Instance that you can deploy and log into:

1. Press the "Deploy" button on the top-right of this task
2. Wait for the IP address of the target Instance to display
3. Log into your Instance using the following information:

**IP Address:** `10.10.189.148`

**Username:** elfmceager

**Password:** adventofcyber

---

Let's proceed to run through how Radare2 works exactly. Although you shouldn't do this if the program is unknown, it is safe for us to execute to see what _should_ be happening like so:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/1.png)

_The above program shows that there are 3 variables(**a**, **b**, **c**) where **c** is the sum of **a** **and b**._ 

Time to see what’s happening under the hood! **Run** the command `r2 -d ./file1`  

This will open the binary in debugging mode. Once the binary is open, one of the first things to do is ask r2 to analyze the program, and this can be done by typing in: `aa`

**Note, when using the** `aa` **command in radare2, this may take between 5-10 minutes depending on your system.**

Which is the most common analysis command. It analyses all symbols and entry points in the executable. The analysis, in this case, involves extracting function names, flow control information, and much more! r2 instructions are usually based on a single character, so it is easy to get more information about the commands.

I.e. For general help, we can run: `?` or if we wish to understand more about a specific feature, we could provide `a?`

3. Computer says...Done?!

Once the analysis is complete, you would want to know where to start analysing from - most programs have an entry point defined as main. To find a list of the functions run: `afl`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/2.png)  

**Note that memory addresses may be different on your computer.** 

  

As seen here, there actually is a function at main. Let’s examine the assembly code at main by running the command `pdf @main` Where **pdf** means **p**rint **d**isassembly **f**unction. Doing so will give us the following view:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/3.png)  

  

  

3. Register me this, register me that...

The core of assembly language involves using registers to do the following: 

- Transfer data between memory and register, and vice versa
- Perform arithmetic operations on registers and data
- Transfer control to other parts of the program Since the architecture is x86-64, the registers are 64 bit and Intel has a list of 16 registers:

|   |   |   |
|---|---|---|
|Initial Data Type|Suffix|Size (bytes)|
|Byte|b|1|
|Word|w|2|
|Double Word|l|4|
|Quad|q|8|
|Single Precision|s|4|
|Double Precision|l|8|

When dealing with memory manipulation using registers, there are other cases to be considered:

- (Rb, Ri) = MemoryLocation[Rb + Ri]
- D(Rb, Ri) = MemoryLocation[Rb + Ri + D]
- (Rb, Ri, S) = MemoryLocation(Rb + S * Ri]
- D(Rb, Ri, S) = MemoryLocation[Rb + S * Ri + D]

  

4. Read the instructions!

Some other important instructions are:

- _leaq source, destinatio_n: this instruction sets destination to the address denoted by the expression in source
- _addq source, destination_: destination = destination **+** source
- _subq source, destination_: destination = destination **-** source
- _imulq source, destination_: destination = destination ***** source
- _salq source, destination_: destination = destination **<<** source where **<<** is the left bit shifting operator
- _sarq source, destination_: destination = destination **>>** source where **>>** is the right bit shifting operator
- _xorq source, destination_: destination = destination **XOR** source
- andq source, destination: destination = destination **&** source
- _orq source, destination_: destination = destination **|** source

Now let’s actually walk through the assembly code to see what the instructions mean when combined.

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/4.png)

The line starting with sym.main indicates we’re looking at the main function. The next 3 lines are used to represent the variables stored in the function. The second column indicates that they are integers(_int_), the 3rd column specifies the name that `r2` uses to reference them and the 4th column shows the actual memory location.

The first 3 instructions are used to allocate space on that stack (ensures that there’s enough room for variables to be allocated and more). We’ll start looking at the program from the 4th instruction (`movl $4`). We want to analyse the program while it runs and the best way to do this is by using breakpoints.

A breakpoint specifies where the program should stop executing. This is useful as it allows us to look at the state of the program at that particular point. So let’s set a breakpoint using the command `db`  in this case, it would be `db 0x00400b55` To ensure the breakpoint is set, we run the `pdf @main` command again and see a little b next to the instruction we want to stop at.

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/5.png)

Now that we’ve set a breakpoint, let’s run the program using `dc`  

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/6.png)

  

Running `dc` will execute the program until we hit the breakpoint. Once we hit the breakpoint and print out the main function, the rip which is the current instruction shows where execution has stopped. From the notes above, we know that the mov instruction is used to transfer values.  This statement is transferring the value 4 into the`local_ch` variable. To view the contents of the `local_ch` variable, we use the following instruction `px @memory-address` In this case, the corresponding memory address for `local_ch` will be `rbp-0xc` ( from the first few lines of @pdf main) This instruction prints the values of memory in hex:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/7.png)  

  

This shows that the variable currently doesn’t have anything stored in it (it’s just **0000**). Let’s execute this instruction and go to the next one using the following command (which only goes to the next instruction) `ds` If we view the memory location after running this command, we get the following:  

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/8.png)

We can see that the first 2 bytes have the value **4**! If we do the same process for the next instruction, we’ll see that the variable `local_8h` has the value **5**.

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/9.png)  

If we go to the instruction `movl local_8h, %eax`, we know from the notes that this moves the value from `local_8h` to the `%eax` register. To see the value of the `%eax` register, we can use the command:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/10.png)  

  

If we execute the instruction and run the `dr` command again, we get:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/11.png)  

This technically skips the previous instruction `movl local_ch, %edx` but the same process can be applied to it. Showing the value of _**rax**_ (the 64 bit version) to be **5**. We can do the same for similar instructions and view the values of the registers changing. When we come to the `addl %edx, %eax`, we know that this will add the values in **edx** and **eax** and store them in **eax**. Running `dr` shows us the **rax** contains **5** and **rdx** contains **4**, so we’d expect **rax** to contain **9** after the instruction is executed:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/12.png)  

Executing `ds` to move to the next instruction then executing `dr` to view register variable shows us we are correct:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/13.png)  

The next few instructions involve moving the values in registers to the variables and vice versa:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/14.png)  

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-17/2/15.png)  

After that, a string (which is the output is loaded into a register and the `printf` function is called in the _3rd_ line. The second line clears the value of **eax** as **eax** is sometimes used to store results from functions. The _4th_ line clears the value of **eax**. The _5t_h and _6th_ lines are used to exit the main function.   

5. To finalise our workflow...

The general formula for working through something like this is:

1. set appropriate breakpoints
2. use ds to move through instructions and check the values of register and memory
3. if you make a mistake, you can always reload the program using the `ood` command

[You may find this radare2 cheatsheet](https://scoding.de/uploads/r2_cs.pdf) useful in your adventures...

  

6. Challenge

Use your new-found knowledge of Radare2 to **analyse the "challenge1" file** in the Instance `10.10.189.148` that is attached to this task to answer the questions below.