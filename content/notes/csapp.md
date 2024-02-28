### **Computer Systems: A Programmer's Perspective**

#### 1. A tour of computer systems
- Preprocessor -> Comppiler -> Assembler -> Linker

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

*Trhead-Level Concurrency*:
    - each its own L1, L2 caches
    - L1 split in: (data) d-cache, (instructions) i-cache
    - L3 is unified
    - Hyperthread: 2 threads per core (2 copies of some parts of the hardware like PC and register files)

*Instruction-Level Parallelism*: multiple instructions at one time
*Single-Instruction, Multiple-Data (SIMD) Parallelism*: e.g. add 8 floats in parallel.