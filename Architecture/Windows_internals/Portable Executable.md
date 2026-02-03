*Yup this is where the fun begins*
![[Pasted image 20260203152328.png]]

PE format defines all the info about the executable, stored data, and how the components are stored , it s a structure for executable and object files :).
PE data can be seen in the hex dump of the executable file.
PE data is structured into 7 components (7 sins of exec?)
#### Dos Header :
The `MZ` Dos header defines the file format as `.exe` 
![[Pasted image 20260203152824.png]]
#### Dos stub :
Just a program that runs by default at the beginning of a file that prints a compatibility message (no effect for most users). 
Message : `This program cannot be run in DOS mode`

#### PE File Header :
It provides the PE header information of the binary (file format, signature , image file header ...) It has the least human readable output.
In the hex dump it starts from `PE` stub.

#### Image Optional Header :
Most important part of the NT Headers, it contains info allowing the PE file to be executed. "Optional" name is confusing , but it's necessary for PE image to be executed (it s just some file types don t contain it ._ .  ).

#### Data Dictionaries :
Part of the image optional header , they point to the image data directory structure.

#### Section Table :
Available sections and information in the image. Sections store the contents of the file, such as code, imports, and data. 
![[Pasted image 20260203154204.png]]

# Sections : 
I guess this is the most important part , it s where the secrets are hidden.
![[Pasted image 20260203154313.png]]
