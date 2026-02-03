ASLR stands for **Address Space Layout Randomization**, a security technique designed to prevent predictable memory layout in running processes. 
- **Goal** : To randomize the memory addresses used by the system components such as stack, heap, shared libraries, and executables.
- **Why** : Attackers often exploit software vulnerabilities by jumping to predictable memory locations (like shellcodes). By randomizing these locations, ASLR makes it difficult to predict where to target. 

### How it works :
*When a process starts* :
- 1 : ASLR shifts memory segments to random addresses.
- 2 : These segments include all the possible components.
- 3 : Each run has its own memory layout , memory addresses are unpredictable and non deterministic.
#### Why it is useful :
- Defense against exploits like [[Buffer Overflow]] and [[Smashing The Stack For Fun And Profit]] , since the attackers cannot predict where their payload will land
- Attackers may need to brute-force memory addresses, increasing the likelihood of crashes and detection.

## Low-level implementation of ASLR :
#### Program Loader :
- Part of the OS that prepares the program for execution.
- The Loader allocates memory for the program's various segments (text/code/data/stack/heap/libraries).
- ASLR adds random offsets to these allocations before the process starts.
#### Kernel :
- The kernel generates random values for memory base addresses using RNG (Random number generator ).
- The seed of RNG is init during boot, ensuring randomness across reboots.
#### Position-Independent Code (PIC):
- For ASLR to randomize the code segment , the executable must be compiled as [[PIE]].
- [[PIE]] ensures that the program can execute correctly regardless of its load address. 
#### Virtual memory Layout :
- Each [[Process]] has its own virtual address space, managed by the [[OS]].
- The virtual addresses are mapped to the physical memory allowing ASLR to shuffle virtual addresses without affecting physical memory usage using **Page tables**.
#### Randomization Granularity:
- Memory is allocated in pages .
- ASLR shifts memory locations by random multiples of the page size.

## How 
- [[Stack]] : the stack pointer is init to a random offset within a predefined range.
- [[Heap]] : randomizing the starting address for dynamic memory allocation (brk or mmap)
- [[Shared Libraries]] : The loader uses *mmap* to load libraries at random addresses.
- [[Executables]] : if compiled with PIE , the code's base address is randomized.