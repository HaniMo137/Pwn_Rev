*The better way to calculate offsets*
De Bruijn sequences of order `n` is simply a sequence where no string of `n` characters is repeated. It makes finding the padding or the offset until EIP much simpler , the idea is just to find the one possible match within the sequence to EIP value and then calculate the offset.

### Generating the pattern : 
[[Radare2]] the goat comes with a command-line tool called **ragg2** that can generate the sequence : 
```bash
$ ragg2 -P 100 -r
AAABAACAADAAEAAFAAGAAHAAIAAJAAKAALAAMAANAAOAAPAAQAARAASAATAAUAAVAAWAAXAAYAAZAAaAAbAAcAAdAAeAAfAAgAAh
```
- -P for length / -r shows ascii bytes rather than hex pairs .
### Using the pattern : 
we just need to input the pattern in [[Radare2]] when prompted for input, make it crash and then calculate the offset from EIP.
[[Radare2]] the goat again has a in-built command **wop0** to calculate the offset.
```bash
[0x41534141]> wopO `dr eip` 
52
```
- **dr eip** is calculated first using the backticks , before the **wop0** is run on the result of it. 
- O hadik  machi 0 :).