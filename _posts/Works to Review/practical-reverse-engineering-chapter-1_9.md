---
title: 'Practical Reverse Engineering - Chapter 1 pg 17 Exercise '
date: 2017-11-09T09:04:00.003-08:00
draft: false
url: /2017/11/practical-reverse-engineering-chapter-1_9.html
---

### Question 1


**Question: Given what you learned about CALL and RET, explain how you would read the value of EIP? Why can't you just do MOV EAX, EIP?**



Answer: The easiest way I could think of to read the current value of EIP was to do the following:



CALL \*instruction after this call instruction\* <- 5 Bytes

POP EAX <- 1 Byte (the instruction that we will point to)

ADD EAX, 4 <- 3 bytes



Essentially we will do a CALL to the POP EAX instruction, at which point ESP will point to the address where "POP EAX" is located in memory (aka the return address for the CALL instruction). POP EAX will ensure EAX gets set to this value. Adding 4 to EAX will ensure we add 1 to this value to compensate for the "POP EAX" instruction, then we add another 3 for the bytes that formulate the "ADD EAX, 4" instruction.



And there we go, we have EIP :)



To answer the other question we can refer to page 82 of [https://software.intel.com/sites/default/files/managed/39/c5/325462-sdm-vol-1-2abcd-3abcd.pdf](https://software.intel.com/sites/default/files/managed/39/c5/325462-sdm-vol-1-2abcd-3abcd.pdf) or section 3.5 of volume 1 of the Intel Software Developer's Manual. On this page they mention that EIP can only be controlled by control transfer instructions like CALL, JMP, etc. It cannot be directly accessed by software in x86.



### Question 2


**Question: Come up with at least two code sequences to set EIP to 0xAABBCCDD.**



Answer:



Option 1

PUSH 0xAABBCCDD ; PUSH value onto the stack

RETN ; POP the value at the top of the stack into EIP and resume execution from this location.



Option 2

JMP 0xAABBCCDD ; Just straight up redirect execution. Why not make it simple after all?

Option 3
CALL 0xAABBCCDD ; Another option :)

Option 4 - Aka the fancier conditional option
XOR EAX, EAX ; Set EAX to zero
TEST EAX, EAX ; Check if EAX is 0
JZ 0xAABBCCDD ; Jump to 0xAABBCCDD if so.



### Question 3


**Question: In the example function, _addme_, what would happen if the stack pointer were not properly restored before executing RET?**



Answer: Well considering the stack pointer would be the part where we do the _PUSH EBP_ instruction, we would return execution to EBP, which means we would return execution to the base pointer of the function that called the _addme()_ function. Since this base pointer will be an address on the stack, this will result in the program trying to execute data off of the stack itself as though it was code. What happens from here depends on the data that is on the stack, but most likely some invalid instructions would be executed and the program would crash. This is assuming the stack is executable as on modern systems the more likely scenario is that DEP would kick in and you would get an ACCESS VIOLATION as the stack would be marked as non-executable.



### Question 4


**Question: In all of the calling conventions explained the return value is stored in a 32-bit register (EAX). What happens when the return value does not fit in a 32-bit register? Write a program to experiment and evaluate your answer. Does the mechanism change from compiler to compiler?**



Answer: So the answer to this, according to my testing it depends how the program is written. For a test I wrote the following program:```
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char *argv[]) {
 long long num = 0x8888888822223344;
 return num;
}

```The output of this when compiled with Dev C++ 5.11 with the TDM-GCC 4.9.2 32 Bit Release option and then decompiled with IDA Pro was as follows:
```

`; int __cdecl main(int argc, const char **argv, const char **envp)
public _main
_main proc near

argc= dword ptr 8
argv= dword ptr 0Ch
envp= dword ptr 10h

push ebp
mov ebp, esp
and esp, 0FFFFFFF0h
sub esp, 10h
call ___main
mov dword ptr [esp+8], 22223344h
mov dword ptr [esp+0Ch], 88888888h
mov eax, [esp+8]
leave
retn
_main endp
`
```As can be seen in this solution, GCC has attempted to try and save a 64 bit number by using the stack to store the upper 32 bits of the number at \[ESP+0xC\] and the lower 32 bits of the number at \[ESP+0x8\]. However it only returns the lower 32 bits of the number as EAX is set to the value of \[ESP+0x8\], which will just contain the lower 32 bits of 0x8888888822223344, aka 0x22223344.

Now for the interesting bit. So what if one sets the return value directly then? Well we get the following compilation warning:```
[Warning] overflow in implicit constant conversion [-Woverflow]
```Here is the program:```

`#include <stdio.h>
#include <stdlib.h>
int main(int argc, char *argv[]) {
    return 0x8888888822223344;
}
`
```And here is the corresponding assembly:
```

`; int __cdecl main(int argc, const char **argv, const char **envp)
public _main
_main proc near

argc= dword ptr  8
argv= dword ptr  0Ch
envp= dword ptr  10h

push    ebp
mov     ebp, esp
and     esp, 0FFFFFFF0h
call    ___main
mov     eax, 22223344h
leave
retn
_main endp
`
```Looks like our number got truncated again, but unlike last time no attempt was made to actually try to save the upper 32 bits of the number specified as the return address anywhere. The compiler instead decided to optimize things and just drop the upper 32 bits entirely, thereby only utilizing the lower 32 bits, aka 0x22223344, in its operations.

Finally, lets try Visual Studio 2017 Community. Upon compilation of the first example we get the following warning:```
c:\users\*redacted*\consoleapplication2\consoleapplication2.cpp(10): warning C4244: 'return': conversion from '__int64' to 'int', possible loss of data
```Looks to be a fairly accurate description of what might happen here :) But lets just check the disassembly to be sure:```

`; int __cdecl main_0(int argc, const char **argv, const char **envp)
_main_0 proc near

var_D0= byte ptr -0D0h
var_C= dword ptr -0Ch
var_8= dword ptr -8
argc= dword ptr  8
argv= dword ptr  0Ch
envp= dword ptr  10h

push    ebp
mov     ebp, esp
sub     esp, 0D0h
push    ebx
push    esi
push    edi
lea     edi, [ebp+var_D0]
mov     ecx, 34h
mov     eax, 0CCCCCCCCh
rep stosd
mov     [ebp+var_C], 22223344h
mov     [ebp+var_8], 88888888h
mov     eax, [ebp+var_C]
pop     edi
pop     esi
pop     ebx
mov     esp, ebp
pop     ebp
retn
_main_0 endp
`
```Yep looks looks like the error message was correct, the return value is being truncated to just its first 32 bits before being returned to the caller.