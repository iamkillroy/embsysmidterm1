# Embedded Systems

* Embedded systems are systems designed for <i>specific purposes</i>
* They have resource constraints and aren't as fast, but they are built to do one thing well
* Cyberphysical = Embedded System where physical systems is the emphasis

# C Programming
* C is a middle/low level programming language that allows for both abstraction and also low level interfacing
* C is the most significant programming language used in Embedded Systems
* C is compiled, faster and directly in ASM

| char | int    | double | float |
|------|--------|--------|-------|
| 1    | 2 or 4 | 8      | 4     |
* Constants can't be changed. Volatile is fetched from memory every time because the state of it cannot be cached.
# Bit Shifting
| <<         | >>          | &             | ^             | \|           | ~                              |
|------------|-------------|---------------|---------------|--------------|--------------------------------|
| left shift | right shift | AND every bit | XOR every bit | OR every bit | One Bit's Compliment every bit |
# Pointers
* A pointer is the address of the variable memory
```
//makes myLetter
char myLetter = 'a';
//* signifies address
//& gets the current address
char *ptr = &myLetter;
```
* IMPORTANT! If you allocate a variable and then try to reference it OUTSIDE of its stack call then you get undefined behavior (the value has been pushed off of the stack)
```
int * makeANum(){
  int newNum = 67;
  return &newNum;
}
int main(){
  int *result = makeANum();
  //the * gets the (now dereferenced) variable inside of the stack
  printf("my number is %d", *result);
  //will be garbage data ^^^
  return 0;
}
```
# Compilers
* Compilers have four steps
1. Preprocessing .c -> .i
2. Compiling .i -> .a
3. Assembler .a -> .o
4. Linker .o -> .exe/.bin
## Preprocessing
* Preprocessing is when you include headers/macros
  * Headers are libraries, Macros are variables
  * ```#include <stdio.h>```
  * ```#define MYLIFE "sad"```
  * Removes comments too
## Compiling
* Compiling is when you take a program and turn it into Assembly code, dependent on your system.
## Assembler
* Converts assembly code to machine code (unreadable machine code instructions)
* Leaves the addresses undefined to be filed by the linker
## Linker
* replaces references to memory/standard libraries with the system specific library
* EX: macOS has a different stdlib than Windows (win32)
* VCS repository is a repo of tracked file
# GPIO
* Reprogrammable I/O
* Represents digital signals (5V on 0V low)
* Use volatile!!! reading the output of these things needs to change each time
# Electrical Engineering BS
* Ohms' Law: I * R = V
# Endianness 
* Little Endian = LSB first, MSB last
  * Ex: 8 = 0001
* Big Endian = MSB first, LSB last
  * Ex: 8 = 1000
# EECS 140 IEEE Standard
* Two bit's == king
* IEEE defined how to do fractional numbers pretty well
## Float
* 1 bit sign, 8 bits exponent, 23 bits fraction
* 6 significant decimal digits on decimal precision
## Double
* 1 bit sign, 11 bits exponent, 52 bits fraction
* 11-17 siginficant decimal digits of precision
## How to do the math for mantissa
* First, turn the integer to binary:
  * 6.134 = 110 (or 6)
* Next, convert the fraction to binary
  * Multiply each by two 
  * if the value > 1 then the bit is zero
  * else it's 0
  * then you keep going

## Example table!!!
| 0.134 * 2 | 0.268 | 0 (1 > 0.268) |
|-----------|-------|---------------|
| 0.268 * 2 | 0.572 | 0 (1 > 0.572) |
| 0.572 * 2 | 1.144 | 1 (1 < 1.144) |
| 0.144 * 2 | 0.288 | 0 (1 > 0.288) |
| 0.288 * 2 | 0.567 | 0 (1 > 0.567) |
| 0.567 * 2 | 1.152 | 0 (1 < 1.152) |
# MIPS ISA
* MIPS is a Embedded Systems instructions set
* Every location is 1 byte addressable
* You can store one byte per register
* There is 2^16 unique memory locations
## Types of instructions
* There's 3
  * R-type, which makes up arithmetic instructions (RRR-ithmetic)
  * I-type, which accesses memory, load/store (put IN)
  * J-type control flow, JUMPS the program, branches the program, and handles call/return


| R Type  | OPCODE ( 6 bits) | RS (6 bits register #1) | RT (6 bits register #2)   | RD (destination register) | 32 |
|---------|------------------|-------------------------|---------------------------|---------------------------|----|
| I Type  | OPCODE ( 6 bits) | RS (register #1)        | RT (destination register) | IMM (immediate offset_    | 32 |
| J Type  | OPCODE ( 6 bits) | ADDR (16 bit address)   | ---->                     | ----->                    | 32 |

* opcodes are the unique 6 bit values that tell what instruction type it is
* NOTE that while the instruction type makes sense, MIPS asm in text is different
* Ex: add $t0, $t1, $t2 adds $t1+$t2 and stores it in $t0, not $t0 + $t1 = $t2
