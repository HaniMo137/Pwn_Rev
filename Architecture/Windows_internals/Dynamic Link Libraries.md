*Dll, loved by programs and actors* 
Bunch of libraries that contains code and data that can be use by more than one program at the same time !!
#### Why? 
They help modularization of code , code reuse , efficient memory usage , reduced disk space (Imagine allocating the same function multiple times because some high ego coder don t use DLL ).
This way the OS and programs load faster , run faster and taking less space in memory (So u can aimtrain while listening to spotify in parallel ) , because most of the reused and necessary code is already allocated :D .

#### Threats:
When DLL is loaded as a function , the DLL is assigned as a dependency. Attackers then can target DLLs rather than apps to control program's execution .
-  DLL Hijacking ([T1574.001](https://attack.mitre.org/techniques/T1574/001/))
-  DLL Side-Loading ([T1574.002](https://attack.mitre.org/techniques/T1574/002/))
-  DLL Injection ([T1055.001](https://attack.mitre.org/techniques/T1055/001/))

#### Syntax :
- DLL are created the same way as any other app , the only require slight modification.
- Header file define what functions are imported and exported.
sampleDLL.h : (header file)
```c 
#pragma once
#include <windows.h>

#ifdef EXPORTING_DLL
  #define DLL_API __declspec(dllexport)
#else
  #define DLL_API __declspec(dllimport)
#endif

#ifdef __cplusplus
extern "C" {
#endif

DLL_API void HelloWorld(void);

#ifdef __cplusplus
}
#endif

```
sampleDLL.cpp :
```c
#include "stdafx.h"          // optional (only if you use precompiled headers)
#define EXPORTING_DLL
#include "sampleDLL.h"

BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID reserved)
{
    (void)hModule; (void)reason; (void)reserved;
    return TRUE;
}

void HelloWorld(void)
{
    MessageBoxW(NULL, L"Hello World", L"In a DLL : D", MB_OK);
}

```
#### How :
*load-time dynamic linking* or *run-time dynamic linking* are the methods used to load DLLs in a program.
- Using load time dl , explicit calls to the DLL functions are made from the app. (u need a header .h and import library .lib)
- Using run time dl, a function called **LoadLibrary** is used to load the DLL at run time, after loading it , u need to use **GetProcAddress** to identify the exported DLL function to call. 
```cpp
typedef VOID (*DLLPROC) (LPTSTR);
...
HINSTANCE hinstDLL;
DLLPROC HelloWorld;
BOOL fFreeDLL;

hinstDLL = LoadLibrary("sampleDLL.dll");
if (hinstDLL != NULL)
{
    HelloWorld = (DLLPROC) GetProcAddress(hinstDLL, "HelloWorld");
    if (HelloWorld != NULL)
        (HelloWorld);
    fFreeDLL = FreeLibrary(hinstDLL);
}
```
### Malicious : 
Threat actors tend to use run-time dynamic linking more , because a malicious program may need to transfer files between memory regions, and transferring a single DLL is more manageable than importing using other file requirements . 
(This needs a lot of practice to understand :( )