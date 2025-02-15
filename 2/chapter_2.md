# Chapter 2 - Representing and Manipulating Information

Machines use binary values to store and transmit information while humans use the decimal notation. Two-valued signals (binary) are useful for machines because they are simple and reliable:
- Easier to deal with 2 states
- Simplicity for circuit design, e.g. where a transistor is either ON or OFF := logic gates can be built from them
- Encoding all data in binary enables universal computation and scalability
- Boolean logic aligns naturally with binary representation

A single bit is not useful but we can group them together and apply some `interpretation` to give meaning to the different possible bit patterns.

```
1 bit → Can be either 0 or 1 → 2 values
2 bits → Can be {00, 01, 10, 11} → 4 values
3 bits → Can be {000, 001, 010, 011, 100, 101, 110, 111} → 8 values
4 bits → 16 values
8 bits (1 byte) → 256 values
```

Every time you add one more bit, you double the number of possible values.

To make sense of bit patterns, we **define rules** that map patterns to real-world meanings. Here are a few ways the same bit pattern can be interpreted differently:

### **1. Binary Numbers (Unsigned Integers)**
If we interpret `1101` as an **unsigned binary number**, we compute:

\[
1 \times 2^3 + 1 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = 8 + 4 + 0 + 1 = 13
\]

So, if we assume **unsigned integer interpretation**, `1101` represents the number **13**.

### **2. Signed Integers (Two's Complement)**
If we assume the **two's complement** system (for signed numbers), the leftmost bit determines whether it's positive or negative:
- `1101` in **two's complement (4-bit)** means:
  - `1` (leftmost bit) → Negative number
  - Convert by inverting bits and adding 1: `1101 → 0011 + 1 → 0100` (which is 4)
  - So `1101` represents `-3`.

### **3. Characters (Text Representation - ASCII)**
If we use **ASCII encoding**, 8-bit patterns map to characters:
- `01000001` in ASCII represents the letter `'A'`.
- `01100001` represents `'a'`.

If we had a **sequence** of bit patterns:  
`01001000 01100101 01101100 01101100 01101111`,  
this could be interpreted as the ASCII characters **"Hello"**.

### **4. Colors (RGB Representation)**
If we treat `1101 0000` as an **8-bit grayscale value**, it represents a shade of gray in an image.  
If we use a **24-bit RGB system**, three groups of 8 bits (`R, G, B`) define a color:
- `11111111 00000000 00000000` → Bright **Red** (`255, 0, 0` in RGB).
- `00000000 11111111 00000000` → Bright **Green** (`0, 255, 0` in RGB).

### **5. Machine Instructions (CPU Operations)**
In a computer's **instruction set architecture (ISA)**, certain bit patterns represent machine instructions.
For example, in an imaginary CPU:
- `10110000 01100001` might mean **"Move the value 97 into register B"**.

## 2.1 - Information Storage

Computers use bytes (8-bit blocks) as the smallest addressable memory unit, organizing memory as a large virtual byte array.
Each byte has a unique address, forming the virtual address space, which is an abstraction managed by hardware, OS, and storage.

Compilers and runtime systems partition memory for storing data, instructions, and control info. 
Pointers in C store virtual addresses, and while the compiler tracks type information, machine code treats all data as raw bytes with no inherent type awareness.

## 2.1.1 - Hexadecimal Notation

### **Why Use Hexadecimal?**
- **Binary** (base-2) is too long and verbose for human reading.
- **Decimal** (base-10) requires tedious conversions to and from binary.
- **Hexadecimal** (base-16) is more compact and directly tied to binary, making it ideal for representing bit patterns.

### **Hex Representation**
- Hexadecimal uses digits `0-9` and letters `A-F` to represent values from **0 to 15**.
- A **byte (8 bits)** in hexadecimal ranges from `0x00` to `0xFF` (0 to 255 in decimal).
- In C, **hexadecimal constants** are written with a prefix `0x` (e.g., `0xFA1D37B`).

### **Conversions**
- **Binary ↔ Hex:** Group bits into **4-bit chunks**. Convert each group to a hex digit.
  - Example: `0x173A4C` → Binary: `000101110011101001001100`.
- **Decimal → Hex:** Divide by 16, track remainders, and map to hex digits.
  - Example: Decimal `314156` → Hex `0x4CB2C`.
- **Hex → Decimal:** Multiply each hex digit by **powers of 16** and sum them.
  - Example: `0x7AF` → Decimal `1967`.

## 2.1.2 - Data Sizes

### **Word Size and Virtual Address Space**
- **Word Size:** Refers to the number of bits used for pointers and addresses. It impacts the **maximum virtual address space**.
  - A **32-bit word** allows access to 4 GB of memory.
  - A **64-bit word** allows access to **16 exabytes** of memory.
  
### **Shift from 32-bit to 64-bit**
- **64-bit systems** became common, starting in high-end machines and moving to desktops, laptops, and smartphones.
- **Backward compatibility**: 64-bit machines can run **32-bit programs**, but **32-bit programs** can't run on 64-bit-only machines.

### **Data Types and Sizes**
- Common data types include:
  - **char** (1 byte), **short** (2 bytes), **int** (4 bytes), **long** (4 or 8 bytes depending on system).
  - Floating-point numbers: **float** (4 bytes), **double** (8 bytes).
  - Fixed-size integers (e.g., **int32_t** and **int64_t**) ensure consistency across systems.
  
### **Pointer Size**
- A **pointer** always uses the word size of the system. For example, on a 64-bit machine, a pointer is 8 bytes.

### **Portability and Data Type Sizes**
- Portability requires understanding and managing the **sizes of data types** across different systems.
- **C99 standard** introduced fixed-size types to avoid relying on the system's default sizes.
- Transition from 32-bit to 64-bit machines has led to bugs due to assumptions about data sizes, such as using an **int** to store a pointer.

## 2.1.3 Addressing and Byte Ordering

For objects that span multiple bytes, two key conventions are needed:

1. **Addressing**: The address of the object is the smallest address of its bytes.
   - Example: If an `int` has an address `0x100`, the four bytes of the `int` are stored at `0x100`, `0x101`, `0x102`, and `0x103`.

2. **Byte Ordering**: The ordering of the bytes within an object:
   - **Big-endian**: The most significant byte is stored first.
   - **Little-endian**: The least significant byte is stored first.

### Example with `int` value `0x01234567`:
- **Big-endian**: Stored as `01 23 45 67`
- **Little-endian**: Stored as `67 45 23 01`

### Endianness in Practice
- **Intel-compatible machines** typically use **little-endian**.
- **IBM/Oracle machines** (e.g., SPARC) typically use **big-endian**.
- Many modern processors are **bi-endian** (can switch between little-endian and big-endian).

### Importance of Byte Ordering
- **Network communication**: Machines with different endianness may interpret byte order differently. The network standard, **network byte order**, is big-endian.
- **Machine-level code**: Machine code instructions depend on the byte order.
- **System-level programming**: Manipulating bytes or casting between types (e.g., `int` to `char`) can reveal byte order.

The difference between integer and floating-point representations lies in their encoding schemes, even when they represent the same value.
- **Integer Representation**: The number `12345` is stored in **binary** directly, with the hexadecimal representation `0x3039`. This is a straightforward encoding with no special format beyond binary.
- **Floating-Point Representation**: The same number is stored using the IEEE 754 standard for **floating-point encoding**, which involves a **sign bit**, **exponent**, and **mantissa**. The hexadecimal representation for `12345` in floating-point is `0x4640E400`.

Despite the differences in encoding, both formats aim to store the value `12345`. When comparing the binary forms, we find 13 bits in common between the two representations:
- Integer in binary: `00000000000000000011000000111001`
- Floating-point in binary: `01000110010000001110010000000000`

This overlap occurs because the number `12345` happens to fall within a range where both integer and floating-point formats represent it in a similar manner. However, the two formats are still distinct, and this alignment is specific to certain values.

### Memory Decoding Based on Type in C

In C, the type of a variable or pointer guides the compiler in interpreting the data stored in memory. The compiler uses the type to apply the correct decoding scheme when accessing the value.

### Integers
- Integers in C (e.g., `int`, `long`, `short`) are typically stored in memory as binary values.
- The exact size of an integer depends on the system and the specific integer type, but:
  - `int` is typically 4 bytes (32 bits).
  - `short` is typically 2 bytes (16 bits).
  - `long` is typically 4 or 8 bytes depending on the architecture.

**Example:**
- For `int x = 12345;`, the integer `12345` will be stored as a 32-bit binary number in memory.

### Floating-Point Numbers
- Floating-point numbers (e.g., `float`, `double`) are stored in memory using the IEEE 754 standard.
  - This format represents numbers in three parts: **sign**, **exponent**, and **fraction (mantissa)**.
- The binary representation is different for each format:
  - `float` (32 bits): 1 bit for the sign, 8 bits for the exponent, and 23 bits for the mantissa.
  - `double` (64 bits): 1 bit for the sign, 11 bits for the exponent, and 52 bits for the mantissa.
  
**Example:**
- The integer `12345` encoded as a floating-point number would have a different binary pattern than the same number stored as an integer.
  - Integer: `0x00003039` (32-bit representation).
  - Float: `0x4640E400` (32-bit IEEE 754 representation).

### Pointers
- A **pointer** is a variable that stores the memory address of another variable.
- The memory used by a pointer is typically 4 bytes (on a 32-bit system) or 8 bytes (on a 64-bit system), as it needs to store the memory address.
- The type of the pointer determines how the memory it points to is interpreted.

**Example:**
- `int *p;` stores the memory address of an integer, and when dereferenced (`*p`), it accesses the integer stored at that memory location.
- For a pointer to an integer, the pointer's memory address is treated as a **memory address** (i.e., a value representing a location).

### Arrays
- Arrays are collections of elements, typically of the same type, stored in contiguous memory locations.
- The size of an array is determined by the type of its elements and the number of elements in the array.
  - Example: `int arr[5];` declares an array of 5 integers.
  - The elements of the array are stored in memory one after the other.

### Strings
- In C, strings are arrays of characters terminated by a special character: the **null character** (`'\0'`).
- Each character in the string is represented by a corresponding ASCII value (or another encoding, such as UTF-8).
  - Example: `"hello"` is stored as: `['h', 'e', 'l', 'l', 'o', '\0']`.

### Booleans
- C does not have a built-in `bool` type in its original standard. Instead, integers are used to represent boolean values.
  - `0` is considered `false`.
  - Any non-zero value is considered `true`.
- From C99 onwards, `stdbool.h` introduces the `bool` type, which is still an alias for integers.

**Memory Representation:**
- `bool` is typically stored as a 1-byte value, though it is logically just a single bit (0 for false, non-zero for true).
  
**Example:**
- `bool flag = true;` is equivalent to `int flag = 1;`.

### Summary of Memory Representation

| Data Type      | Memory Representation |
|----------------|-----------------------|
| **Integer**    | Stored as a binary number, usually 4 bytes (`int`), 2 bytes (`short`), or 8 bytes (`long`). |
| **Floating-Point** | Stored using IEEE 754 format; `float` is 4 bytes, `double` is 8 bytes. |
| **Pointer**    | Stores a memory address, typically 4 bytes (32-bit) or 8 bytes (64-bit). |
| **Array**      | A contiguous block of memory for storing multiple elements of the same type. |
| **String**     | Array of characters terminated by `'\0'`, each character is typically 1 byte. |
| **Boolean**    | Stored as an integer (0 for false, non-zero for true), typically 1 byte. |

- The memory organization and decoding are dependent on the **type** of the variable (e.g., integer, float, pointer).
- **Pointers** are used to point to a memory location, and the type of pointer tells the system how to interpret the memory it points to.
- **Strings** are treated as arrays of characters, ending with a null character `'\0'`.
- **Booleans** are typically represented as integers (0 for `false`, non-zero for `true`), but are often stored as 1 byte.

Each data type's **memory layout** and **decoding** are managed by the C compiler according to the **data type** and **pointer type**, ensuring correct interpretation and access to the data.
