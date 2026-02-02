The Virtual Memory allows processes to use the memory as if it s a physical memory while avoiding collisions and troubles between different apps.
#### The idea :
The concept is that each process has its own private virtual address space, and have no clue or access to other processes private spaces .
The memory Manager (An Os component) , is responsible of translating virtual addresses to physical addresses. 
#### Transfers : 
Pages and transfers are used by the mem manager to handle the memory, so that it ll be possible for apps that needs a virtual memory bigger than allocated physical memory to work too. Clever way to run good games on ur old slow laptop  .
(That 's why when u run a game on a weak RAM , it will have more lags  and fps drops , because a lot of transfers of pages are happening , and they cost a lot :0 )

![[Pasted image 20260202150102.png]]

#### Layout :
The address space is split in half , lower one is allocated for processes . The upper half is allocated to OS . Don t ask why,  it's Windows :( .
![[Pasted image 20260202152644.png]]

