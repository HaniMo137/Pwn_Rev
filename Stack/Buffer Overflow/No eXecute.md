*The defense against [[Shellcode]]*
**NX** bit, which stands for No eXecute, defines areas of memory as either **instructions** or **data**. This means that the input will be stored as **data**, and any **attempt** to run it as instructions will crash the program, effectively neutralising shellcode.

*Windows version of NX is DEP , stands for Data Execution Prevention*
### Checking for NX: 
U can either use [[PwnTools]] , or [[Rabin2]] .
```bash
$ checksec --file=vuln
```

```bash
$ rabin2 -I vuln
[...]
nx       false
[...]
```

## Bypassing NX :
To get around NX, exploit developers have to leverage a technique called [[ROP]] , Return Oriented Programming.