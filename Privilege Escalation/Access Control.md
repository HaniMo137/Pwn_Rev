- Authentication : who are u? 
-  Authorization : what u can do. (The policy)
- Access Control : the mechanism
#### UNIX access Model :
![[Pasted image 20251222190017.png]]
### POSIX ACL : (access control list)

- 12 bits for each file : (4 sets of three bits each)
- First 3 bits : SUID SGID Sticky-bit
- RWE for : Owner Group All
```bash
$ls -lan # shows the IDs of users
/etc/passwd # infos about users
/etc/shadow # passwords
```

 - SUID : for execs / it gets the file owner's effective UID while running.  (x -> s)
 - SGID : for execs it gets the GID , for dicts the new files subdirs inherit the dict' group.
 - Sticky bit : even if a user has write permission to the directory, they cannot delete or rename files they don’t own. 
 ```bash 
 $ /tmp : drwxrwxrwt #t is sticky bit
 ```
 - r = 4 , w = 2 , x = 1
```bash
 newgrp grpname # to join a grp
 su username # to access as another user
```

