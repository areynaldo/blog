#### Computer Systems: A Programmer's Perspective

#### 1. A tour of computer systems

- Preprocessor -> Compiler -> Assembler -> Linker

- *OS*:
    1. protects the hardware from misuse
    2. provide simple and uniform interface to the hardware

    via *processes*, *virtual memory* and *files*

- *Processes*:
    - *context*: current values of the PC, register file, contents from main memory...
    - *context switch*: saves the context of the current process and restores the one of another process.

- *Kernel*:
    - always resident in memory
    - takes control on *syscalls*

- *Files*:
    - bytestring
    - IO devices are modeled as files

- *Amdahl's Law*: to speed up a system, speed up a very large fraction of it
    ```python
    # T: time
    # S: x speedup
    # alpha: fraction
    # k: x improvement
    T_new = (1 - alpha)*T_old + (alpha*T_old)/k
          = T_old((1 - alpha) = alpha/k)

    S = 1/((1 - alpha) + a/k)
    ```

- *Trhead-Level Concurrency*:
    - each its own L1, L2 caches
    - L1 split in: (data) d-cache, (instructions) i-cache
    - L3 is unified
    - Hyperthread: 2 threads per core (2 copies of some parts of the hardware like PC and register files)

- *Instruction-Level Parallelism*: multiple instructions at one time
- *Single-Instruction, Multiple-Data (SIMD) Parallelism*: e.g. add 8 floats in parallel.

#### 2. Representing and Manipulating Information

- *Big Endian*:
    Adress:   | 0x100 |  0x101 | 0x102 | 0x103
    ----------|-------|--------|-------|-------
    Value:    | 01    |  23    | 45    | 67    

- *Little Endian*:
    Adress:   | 0x100 |  0x101 | 0x102 | 0x103
    ----------|-------|--------|-------|-------
    Value:    | 67    |  45    | 23    | 01   

#### 3. Machine-Level Representation of Programs

- *Instruction Set Architecture*: defines format and behavior of a machine-level program
- Memory adressses used by a a machine-level program are virtual adresses
- *Program Counter*: adress of the next instruction to be executed
- *Integer file*: 16 named locations storing 64-bit values
- *Condition code registers*: status information about recent instructions. Usefull for control and data flow.
- *Vector registers*: one or more integer or floating point.

- *x86*:
    - Instruction length 1..15 (bytes)
    - suffixes:

        type   | Intel             |  asm suffix | size (bytes)
        -------|-------------------|-------------|-------
        u8     | byte              |  b          | 1
        u16    | word              |  w          | 2
        u32    | double word       |  l          | 4
        u64    | quad word         |  q          | 8
        *u8    | quad word         |  q          | 8
        f32    | single precision  |  s          | 4
        f64    | double precision  |  q          | 8

    - *general-purpose registers*:
    ```c
        ( %al  ... %bpl ), %spl, ( %r8b ... %r15b ) (8 bits )
        ( %ax  ... %bp  ), %sp , ( %r8w ... %r15w ) (16 bits)
        ( %eax ... %ebp ), %esp, ( %r8d ... %r15d ) (32 bits)
        ( %rax ... %rbp ), %rsp, ( %r8  ... %r15  ) (64 bits)
    ```
    - *operands*:
    ```asm
        nop %rax                # register value
        nop 0x104               # memory adress
        nop $0x104              # literal
        nop 4(%rax)             # adress pointed by the register + 4 offset
        nop 9(%rax, %rdx)       # 9 + (adress in register_a) + (value in register_d)
        nop (%rax, %rdx, 4)     # (adress in register_a) + (value in register_d) * 4
    ```
    - *mov*:
    ```asm
        movl    $0x4050, %eax       # immediate        to register,        4 bytes
        movw    %bp, %sp            # register         to register,        2 bytes
        movb    (%rdi, %rcx), %eax  # memory           to register,        1 bytes
        movb    $-17, (%eax)        # immediate        to memory,          1 bytes
        movq    $0x4050, -12(%rbp)  # register         to memory,          8 bytes
        movabsq $0x4050, %rbp       # 64bit immediate  to register (only), 8 bytes
    ```
    - *movz*
        - same as move but remaining bytes will be filled to zero.
        - Register or memory as source and register as destination.
    ```asm
        movzbw # byte to word
        movzbl # byte to long-word
        movzwl # word to long-word
        movzbq # byte to quartet
        movzwq # word to quartet
        # long-word to quartet with movl
    ```
    - *movs*
        - same as move but with sign extension.
        - Register or memory as source and register as destination.
    ```asm
        movsbw # byte to word
        movsbl # byte to long-word
        movswl # word to long-word
        movsbq # byte to quartet
        movswq # word to quartet
        movslq # long-word to quartet
    ```
    - *stack*
        - %rsp (stack pointer)
    ```asm
        pushq %rax # push value to the stack
        popq  %rdx # pop value from the stack to a register
    ```
    ```asm
        # Push
        subq $8, %rsp
        movq %rbp, (%rsp)

        # Pop
        movq (%rsp), %rax
        addq $8, %rsp
    ```
