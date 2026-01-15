*Controlling execution with snippets of code*
**Gadgets** are small snippets of code followed by a `ret` instruction .
```asm
pop rdi; ret
```
The idea is to string together a large chain of gadgets to manipulate the program flow using their `ret` instructions .
##### Example :



