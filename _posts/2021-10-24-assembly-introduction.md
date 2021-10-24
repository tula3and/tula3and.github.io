---
title: "What is assembly language?"
categories:
  - Computer Science
tags:
  - Assembly
  - Language
last_modified_at: 2021-10-24
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

An assembly language is an low-level programming language
to communicate directly with a computer's hardware.

A high-level program turns into an assembly file with a compiler and
the file is converted into a readable form for computers, machine language with an assembler.
(Unlike assembly languages, machine languages are hard to read because they consist of binary and hexadecimal characters.)
For example, there are four steps before executing a C program.

```
                                                               Object (Library)     
                                                                     |
C program --{Compiler}--> Assembly program --{Assembler}--> Object --+--{Linker}--> Executable --{Loader}--> Memory
```

The object file is a combination of machine language instructions, data, and information
needed to place instructions properly in memory.
Every line of the machine language file, called instructions, can be replaced to assembly language,
so it is easy to fix an low-level language file with assembly languages.

## Instructions

Instructions are the basic command describing an operation of a processor and
each of them has an opcode and operands.
Processors have an instruction set architecture (ISA) which is a set of instructions and implementation of the ISA
(microarchitecture) can be various as long as it satisfies the specific design constraints and goals.
In this post, every instruction follow the RISC-V assembly language notation.

*RISC-V is little endian processor: the MSB of data is placed at the highest address.

### 1. `add` and `addi`

There are many arithmetic instructions but I just pick two: `add` and `addi`.

- `add <reg'>, <reg>, <reg>`: `<reg'>` has the sum of the others
- `addi <reg'>, <reg>, <constant>`: same with `add` but there is a immediate value at the end

### 2. `ld` and `sd`

There are two instructions for data transfer between a register and a memory: `ld` and `sd`.

- `ld <reg>, <offset>(<address>)`: load doubleword (I-type)
- `sd <reg>, <offset>(<address>)`: store doubleword (S-type)

### 3. Logical operations

There are instructions for bitwise logical operations.

- `sll` `slli`: shift left
- `srl` `srli`: shift right
- `sra` `srai`: shift right and fill with sign-bit
- `and` `andi`: bit-by-bit AND
- `or` `ori`: bit-by-bit OR
- `xor` `xori`: bit-by-bit XOR

FYI, `xori` can work as NOT operation: `xori <reg'>, <reg>, 111...111(2)`.

### 4. Conditional branches

These instructions are commonly used to build if-else statements or loops.

- `beq <reg'>, <reg>, <label>`: if (`<reg'>` == `<reg>`), move to instruction labeled `<label>`
- `bne <reg'>, <reg>, <label>`: if (`<reg'>` != `<reg>`), move to instruction labeled `<label>`
- `blt` `bltu`: `<`, use `ble` or `bleu` for the equal sign (`u` means unsigned) 
- `bgt` `bgtu`: `<`, use `bge` or `bgeu` for the equal sign (`u` means unsigned)

(1) `if-else` statement: `if (i==j) f=g+h; else f=g-h;` with assembly

```assembly
; f,g,h,i,j --- reg x19,x20,x21,x22,x23
      bne x22, x23, Else
      add x19, x20, x21
      beq x0, x0, Exit
Else: sub x19, x20, x21
Exit:
```

(2) `while` loop: `while (arr[i]==k) i+=1;` with assembly

```assembly
; i,k,base of arr --- reg x19,x20,x21
Loop: slli x22, x19, 3 ; x22 = i*8 (8-byte)
      add x22, x21, x22
      ld x23, 0(x22)
      bne x23, x20, Exit
      addi x19, x19, 1
      beq x0, x0, Loop
Exit:
```

### 5. `jal` and `jalr`

There are two instructions for the procedures which are similar to functions.

- `jal x1, <procedure>`: jump to `<procedure>` and write return address to `x1`
- `jalr x0, <offset>(x1)`: jump to the address stored in `<offset> + x1`

Using `x0` as the destination does not work because `x0` is hardwired to the constant value 0.

## Procedure calls

A caller calls a procedure and the procedure becomes callee.
The caller gives the callee arguments and the callee return results to the caller.
In the register, there is a space to pass parameters and return results from `x10` to `x17`.
Especially, `x1` is only for returning the point of origin, so every procedure call start with a `jal` instruction.

Then, where should the computer save parameters or results if the spaces are full?
It can be possiable when a procedure calls other procedures again and again. (e.g. recursive functions)
Therefore, use `register spilling` to prevent the situation.
It just saves values from the registers to memory with the data structure, `stack`.
The stack pointer is saved in register `x2` and the pointer moves from higher to lower address, so
before saving data, do not forget to move the pointer by `addi sp, sp, -<size>`.

In each procedure call:

- Caller saves from `x10` to `x17` (arguments), from `x5` to `x7`, and from `x28` to `x31`.
- Callee saves `x1` (return address), from `x8` to `x9`, and from `x18` to `x27`.

A recursive procedure is a good example to understand above the all. Check the codes below.

```c
long long int fact(long long int n) {
  if (n < 1)
    return 1;
  else
    return n * fact(n-1);
}
```

The C codes above is a factorial function and it is converted into the assembly below.

```assembly
; n --- reg x10
fact: addi sp, sp, -16 ; save 2 items
      sd x1, 8(sp)
      sd x10, 0(sp)
      
      addi x5, x10, -1
      bge x5, x0, else
      addi x10, x0, 1 ; return 1      
      
      addi sp, sp, 16
      jalr x0, 0(x1)

else: addi x10, x10, -1
      jal x1, fact
      
      addi x6, x10, 0 ; move x10 to x6
      
      ld x10, 0(sp)
      ld x1, 8(sp)
      addi sp, sp, 16
      
      mul x10, x10, x6

      jalr x0, 0(x1) ; last x1 -> main
```

## References

- [Computer Organization and Design RISC-V Edition: The Hardware Software Interface](https://www.amazon.com/Computer-Organization-Design-RISC-V-Architecture/dp/0128122757)
- [Investopedia: Assembly Language](https://www.investopedia.com/terms/a/assembly-language.asp)

---

💬 _Any comments and suggestions will be appreciated._
