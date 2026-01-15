When a function calls another function, it

- pushes a **return pointer** to the stack so the called function knows where to return
    
- when the called function finishes execution, it pops it off the stack again
    

Because this value is saved on the stack, just like our local variables, if we write _more_ characters than the program expects, we can overwrite the value and redirect code execution to wherever we wish. Functions such as `fgets()` can prevent such easy overflow, but you should check how much is actually being read.

### Known exploits :
- [[Ret2win]] 
- [[Shellcode]] 
- [[ROP]]