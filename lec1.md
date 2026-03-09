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
# R-Type
* NOTE that while the instruction type makes sense, MIPS asm in text is different
* Ex: add $t0, $t1, $t2 adds $t1+$t2 and stores it in $t0, not $t0 + $t1 = $t2
* Also, there's two special things with R-type instructions
1. SHAMT -> stands for shift ammount and is the amount bit shifted in shift instructions
2. FUNC -> for whatever reason? probably space, all r-type instructions have OPCODE = 0b000000, and when using R-Type you specify what instruction in the funct spot



| Number | Name   | Usage / Purpose |
|--------|--------|----------------|
| 0      | `$zero` | Constant 0 (read-only) |
| 1      | `$at`   | Assembler temporary (used by pseudo-instructions) |
| 2      | `$v0`   | Function return value / expression evaluation |
| 3      | `$v1`   | Function return value / expression evaluation |
| 4      | `$a0`   | Function argument 1 |
| 5      | `$a1`   | Function argument 2 |
| 6      | `$a2`   | Function argument 3 |
| 7      | `$a3`   | Function argument 4 |
| 8      | `$t0`   | Temporary register (caller-saved) |
| 9      | `$t1`   | Temporary register (caller-saved) |
| 10     | `$t2`   | Temporary register (caller-saved) |
| 11     | `$t3`   | Temporary register (caller-saved) |
| 12     | `$t4`   | Temporary register (caller-saved) |
| 13     | `$t5`   | Temporary register (caller-saved) |
| 14     | `$t6`   | Temporary register (caller-saved) |
| 15     | `$t7`   | Temporary register (caller-saved) |
| 16     | `$s0`   | Saved register (callee-saved) |
| 17     | `$s1`   | Saved register (callee-saved) |
| 18     | `$s2`   | Saved register (callee-saved) |
| 19     | `$s3`   | Saved register (callee-saved) |
| 20     | `$s4`   | Saved register (callee-saved) |
| 21     | `$s5`   | Saved register (callee-saved) |
| 22     | `$s6`   | Saved register (callee-saved) |
| 23     | `$s7`   | Saved register (callee-saved) |
| 24     | `$t8`   | Temporary register (caller-saved) |
| 25     | `$t9`   | Temporary register (caller-saved) |
| 26     | `$k0`   | Reserved for OS kernel |
| 27     | `$k1`   | Reserved for OS kernel |
| 28     | `$gp`   | Global pointer (points to global data segment) |
| 29     | `$sp`   | Stack pointer |
| 30     | `$fp` / `$s8` | Frame pointer / saved register 8 |
| 31     | `$ra`   | Return address (used by `jal`) |

# Serial Communication (RS-232 UART)
* Very crusty old communcation standard
* Two lines, data and signal
* To show there's gonna be communication you do 1 to 0 to show valid data is coming
## Transmission Standard
* Generally serial has
  * Start bit (0)
    * It's actually held high until transmission starts (0)
    * Then a MSB bit size of N
    * Then an (optional) parity bit
      * Even parity bit will be zero if the added up data = 0 
      * Odd is the samething. They're sit to make the statement evaluate to be even or to be odd if you added the parity to the data and treated it as an unsigned int
      * If there's an even number of bits, the even parity = 0 else 1
      * If there's an odd number of bits, the odd parity = 0 else 1
    * Stop bit, always 1 and just holds high 
# Speed and Baudrate
* Because it's asynchronus the sender and reciever have to decide on one speed to transmit at.
* Baud = bits per second
```
              CLK FREQ
BAUD RATE = --------------
            16 * UBBR + 1
```
# I Type Instructon
* Memory is cool. I Type allows us to access this.
* There are shockingly data sizes that are larger than 8 bits (each address holds a bit)
* Using I type we can load these into registers and then do computational stuff to them
* Let's say we wanna do A = B + C
```
//let's say we have two 
//register M0 and M1 (maybe an int)
lw $t0, M[1]
lw $t1, M[0]
//load both
add $t2, $t1, $t0
```
* Storing larger data types are really confusingly called "words"
* Words in MIPS are 4 bytes (32 bits)
* I type uses justttt twoooo registerrrss!! Plus an offset one
## Immediate I type instructions
* You can also use that offset immediate register to do some quick math
* EX: let's say you wanna dd 5 to $t1
```
addi $t0, $t1, 5
```
* Adds $t1 + 5 and stores at $t0
* Think I = I type
# von Nuemann architecture
* IR - instruction register
* PC - program counter
* PC is where we are in memory and what we're executing
* IR is the value at the PC and the instruction
# J Type Instructions
* Controls program flows
* Branch statements branch if a condition is true
* Unconditional jump = go somewhere with no expectation of returning "j somewhere"
# Memory Layout of C Program (Stack/Heap)
* Three parts: 
  * Static 
    * Stores text/code
    * Initialised data
    * Uninitailised data
  * Stack
    * Stores function calls
  * Heap
    * Deep cross stack data
```
DATA STRUCTURE IN C

------------------ high mem (0xFFFFFFF)
    STACK
    | grows down
    ∨
    
    ^
    |
    HEAP (grows up)
------------------
UNINITIALIZED DATA
------------------
INITIALIZED DATA 
-----------------
TEXT/PROG DATA
---------------- low mem (0x0000000)
```
## Text section
* Text contains executable code
* below the heap and stack to protect it
* Also usually read only
## Initialized data
* Data initialized before, predefined.
* CAN BE CHANGED
* Contains global/static variables
## Unitialized data
* empty space for variables that get used later, when the value is not immediately known at runtime
* also includes 0 initialized variables
* STATIC VARIABLES ARE COOL AF AND THEY KEEP THEIR VALUE PERSISTENTLY
## Heap
* Used for DYNAMIC memory allocation
* Grows from low to high
* Used for dynamic stuff
* Can be accessed in different frames
## Stack
* Function call stack that stores local variables
* After exiting a frame, bye bye!!
### Stack stuff in MIPS
* TLDR there's just a SP that you can adjust to change where you're at. You need to adjust it manually and set the data but you don't need to erase it if not in use
# Caller/Callee Jump MIPS Stuff Convention
