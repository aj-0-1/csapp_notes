# Chapter 1 - A Tour of Computer Systems

## 1.1 Information Is Bits + Context

*hello.c* is a source file, which is a sequence of bits (each bit can have a value of 0 or 1) that is organised in 8-bit chunks called bytes. Each byte represents some text character in the program. The **ASCII** standard represents each character with a unique byte-sized integer value. The program is stored in a file as a sequence of bytes. Each byte has an integer value that corresponds to some character.

Let's take an example of a simple text file containing the word "Hi".

1. **Text to Encoding:**
   - "H" in ASCII is 72.
   - "i" in ASCII is 105.

2. **Encoding to Binary:**
   - 72 in binary is `01001000`.
   - 105 in binary is `01101001`.

3. **Stored as Bytes:**
   - [ 01001000 ] [ 01101001 ]

All information in a system - disk files, programs in memory, user data, data transferred across a network etc. - is represented as a bunch of bits.


## 1.2 Programs Are Translated by Other Programs into Different Forms

The compiler reads the source file *hello.c* and translates it into an executable object file *hello*. The *compilation system* is a collection of 4 phases: *preprocessor*, *compiler*, *assembler* and *linker*. 

**Preprocessing phase**
The preprocessor (cpp) modifies the original C program according to directives that begin with the *#* character. E.g. *#include<stdio.h>* tells the preprocessor to read the contents of the system header file *stdio.h* and insert it directly in the program text. The resulting C program typically ends with the *.i* suffix.

**Compilation phase**
The compiler (cc1) translates the text file *hello.i* into the text file *hello.s*, which contains an assembly-language program

**Assembly phase**
The assembler (as) translates *hello.s* into machine-language instructions, packages them in a form known as a relocatable object program, and stores the result in the object file *hello.o*. This file is a *binary file* containing bytes to encode the instructions for function *main*

**Linking phase**
E.g. The *hello* program calls the *printf* function which is a part of the standard C library which resides in a seperate precompiled object file called *printf.o* which is merged with the *hello.o* program by the linker (ld). Thr result is the *hello* file, which is an executable object file that can be loaded into memory and executed


## 1.4 Processors Read and Interpret Instructions Stored in Memory

The *hello* file can be executed with the command *./hello*. 
