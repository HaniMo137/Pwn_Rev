*The diff between the sizes*
Everything in pwn section is applicable to 64-bit as well as 32-bit; the only diff is in [[PwnTools]] commands, switch ```p32()``` to ```p64()``` as the memory addresses are longer.

The real difference between the two, however, is the way you pass parameters to functions ; in 32-bit, all parameters are pushed to the stack before the function is called. In 64-bit, however, the first 6 are stored in the registers RDI, RSI, RDX, RCX, R8 and R9 respectively as per the [calling convention](https://en.wikipedia.org/wiki/X86_calling_conventions) 