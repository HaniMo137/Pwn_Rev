# One Parameter :

### 32-bit : 

![[Pasted image 20260105101446.png]]
- First is the return pointer : it s the first value that gets pushed.
- Second is the arguments passed into the function.
```
return address        param_1
```

### 64-bit :

![[Pasted image 20260105115209.png]]
Lol it s different , the parameter gets moved to `rdi` (`edi` is lower 32 bits of `rdi`)

![[Pasted image 20260105115426.png]]

- Registers are used for parameters, but the return address is still pushed onto the stack and in ROP is placed right after the function address


# Multiple Parameters :
## 32-bit :
![[Pasted image 20260105163606.png]]

- Push the arguments in reverse order of how they're passed in.  
```
return pointer        param1        param2        param3        [...]        paramN
```
## 64-bit:
- The arguments are pushed in `rdi` , `rdx` and  `rsi` (or , their lower 32 bits)
  ![[Pasted image 20260106140143.png]]

## Bigger values in 64 bit :
- It's stored ultimately in `rdi` not `edi` .
```
movabs rdi, 0xdeadbeefc0ded00d
call sym.vuln
```
- `movabs` used to encode the `mov` instruction for-bit instructions .