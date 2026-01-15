_pwntools is a CTF framework and exploit development framework library written in python. Which is designed to be simple, fast and too effective_ 

### Shellcode generation :
The "pwnlib.shellcraft" functions allow us to generate shellcode. Given we are exploiting a Linux system, we choose "linux" as the platform. 
- /bin/sh drops SUID privileges
- Most shells drop privileges unless invoked with -p to preserve effective UID. Therefore, normal /bin/sh shellcodes will run as user → not as SUID target.
- "/bin/bash", "-p"
- -p executes the shell with the current permission sets4


0xF00DF00D / 0xCAFEF00D / 0x08049296 

116 / 
