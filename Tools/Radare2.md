``` bash
r2 -d -A vuln
```
- -d : to run
- -A : performs analysis
```bash
afl 
```
- afl : Analyse functions list
```bash 
s main; pdf 
```
- s : moves to main
- pdf : Print Disassembly Function 
```bash
db 0x080491bb 
```
- db : debug breakpoint
```
dc (debug continue)
pxw @ esp (analyse the top of the stack)
```
- pxw : tells r2 to analyse the hex as words
```
ds (debug step)
dr reg (display the content of the reg)
```
