No operation instructions do nothing xD. Which makes it very useful for shellcode exploits . If we pad our exploits on the left with NOPs instructions and then point EIP at the middle , it ll simply keep moving until it reaches the actual shellcode. It s called NOP slide or NOP sled .  \x90
or maybe u can use 
```python
nop = asm(shellcraft.nop())
```