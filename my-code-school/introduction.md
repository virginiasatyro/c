# My Code School

## Introduction to Programming through 'C'

### Introduction to programming and programming languages: C Programming Tutorial 01

**Computer** - general purpose machine.

**Program** - set of instrucions to perform a task.

**Binary** - the language of computers. It's simple to simulate in hardware, in an electrical circuit.

**Bit** - 0 (unset bit) or 1 (set).

**CPU** - Central procesing unit - computer understand and execute intructions. The processor. Intructions - binary.

**Opcode** - operation code. Exemple : 20 bits for arithmetical:

opecode: 0 0 0 1 (add)

operand 1: 0 0 0 0 0 1 0 0

operand 2: 0 0 0 0 0 1 0 1

**Assembly language** - instructions can be more readable.

Assembly language instructions -> ASSEMBLER -> machine language instructions

**High level languages** - abstraction from machine architecture. C, C++, Java, FORTRAN, ...

1. **Compiled languages** - source code -> COMPILER -> machine code. Ex.: C.
2. **Interpreted languages** - do not generate executable codes that can be executed separately. source code -> INTERPRETER -> no executable file is created. Ex.: Python.

**C** - high level language, needs compilation

### Basics of computer's memory and Getting started: C Programming Tutorial 02

**Hard-disk drive** - stores for exemple: compiler.exe, app.c, app.exe, ... . Secondary storage.

**RAM** - volatile memory, everything in RAM is cleared as soon as power is removed. Primary storage. Main memory. Access is a lot more faster than access to hard-disk.

**Operating System** - helps CPU to switch time between programs and sharing of memory. Manage executions of all programs.

### Writing and executing your first program: C Programming Tutorial 03

```C
#include<stdio.h>

int main()
{
    printf("Hello Wolrd!\n");

    return 0;
}
```
