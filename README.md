# Chapter 1 - A Tour of Computer Systems

## 1.1 Information Is Bits + Context

`hello.c` is a source file, which is a sequence of bits (each bit can have a value of 0 or 1) that is organised in 8-bit chunks called bytes. Each byte represents some text character in the program. The **ASCII** standard represents each character with a unique byte-sized integer value. The program is stored in a file as a sequence of bytes. Each byte has an integer value that corresponds to some character.

Let's take an example of a simple text file containing the word "Hi".

1. **Text to Encoding:**
   - "H" in ASCII is 72
   - "i" in ASCII is 105

2. **Encoding to Binary:**
   - 72 in binary is `01001000`
   - 105 in binary is `01101001`

3. **Stored as Bytes:**
   - [ 01001000 ] [ 01101001 ]

All information in a system - disk files, programs in memory, user data, data transferred across a network etc. - is represented as a bunch of bits.


## 1.2 Programs Are Translated by Other Programs into Different Forms

The compiler reads the source file `hello.c` and translates it into an executable object file `hello`. The *compilation system* is a collection of 4 phases: `preprocessor`, `compiler`, `assembler` and `linker`. 

1. **Preprocessing phase**
   - The preprocessor (cpp) modifies the original C program according to directives that begin with the **#** character. E.g. `#include<stdio.h>` tells the preprocessor to read the contents of the system header file `stdio.h` and insert it directly in the program text. The resulting C program typically ends with the `.i` suffix

2. **Compilation phase**
   - The compiler (cc1) translates the text file `hello.i` into the text file `hello.s`, which contains an assembly-language program

3. **Assembly phase**
   - The assembler (as) translates `hello.s` into machine-language instructions, packages them in a form known as a relocatable object program, and stores the result in the object file `hello.o`. This file is a `binary file` containing bytes to encode the instructions for function `main`

4. **Linking phase**
   - E.g. The `hello` program calls the `printf` function which is a part of the standard C library which resides in a seperate precompiled object file called `printf.o` which is merged with the `hello.o` program by the linker (ld). Thr result is the `hello` file, which is an executable object file that can be loaded into            memory and executed


## 1.4 Processors Read and Interpret Instructions Stored in Memory

The `hello` file can be executed with the command `./hello`. The shell loads the executable by executing a sequence of instructions that copies the code and data in the object file from disk to main memory (using *direct memory access*). Once loaded, the processor begins executing machine-language instructions in `main`. Output is copied from memory to register file to display device.

### 1.4.1 Hardware Organisation of a System

1. **Buses**
   - Buses are a collection of electrical conduits that carry bytes of information back and forth between the components. They are typically desgined to transfer fixed-size chunks of bytes known as *words*. The number of bytes in a word (word size) is a fundamental system parameter. E.g. 4 bytes (32 bits) or 8 bytes (64 bits).
  
2. **I/O Devices**

3. **Main Memory**
   - A temporary storage device that holds both a program and the data it manipulates while the processor executes the program
   - Physically, consists of a collection of `dynamic random access memory` (**DRAM**) chips
   - Logically, it is organised as a linear array of bytes, each with it's own unique address (array index) starting at zero.
  
4. **Processor**
   - **CPU** is the engine that interprets/executes instructions stored in **main memory**.
   - At it's core is a register (word-size storage device) called the *program counter* which at any point of time, points at some machine-language instruction in main memory
   - The processor executes the instruction and updates the PC to point at the next one
   - Instruction = Simple operation , these simple operations revolve around the main memory, *register file* (small storage device with a collection of word-size registers), and *arithmetic/logic unit* (ALU - commputes new data and address values).
   - Example operations: Load, Store, Operate, Jump etc.
  

### 1.5 Caches Matter


