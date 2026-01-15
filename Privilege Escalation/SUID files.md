A SUID binary lets you run a program as its owner — often root — even if you're a normal user. 
- Represented by an s in the user permissions .
```bash
-rwsr-xr-x  1 root root  12345 Jun 10 12:34 program

```
- Search using FIND :
```bash
find / -perm -4000 2>/dev/null
```
- Used in privilege escalation .
#### Linux Permission Model :
- Every process has a user ID and GID.
- Every File and Dict is owned by a user and group.
- Childe Processes inherit from parent processes.
```C
  getuid();
  getgid();
```

  - UID = 0 -> root
  ![[Pasted image 20251201211031.png]]
  - Linux permission model bits.
![[Pasted image 20251201211115.png]]