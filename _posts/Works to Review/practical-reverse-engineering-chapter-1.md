---
title: 'Practical Reverse Engineering - Chapter 1 pg 11 Exercise 1'
date: 2017-11-09T06:39:00.003-08:00
draft: false
url: /2017/11/practical-reverse-engineering-chapter-1.html
---

Problem
-------

"This function uses a combination of SCAS and STOS to do its work. First, explain what is the type of the \[EBP+8\] and \[EBP+C\] in line 1 and 8 respectively. Next explain what this snippet does."



### Disassembly:

```

`1   mov edi, [ebp+8]
2   mov edx, edi
3   xor eax, eax
4   or ecx, 0FFFFFFFFh
5   repne scasb
6   add ecx, 2
7   neg ecx
8   mov al, [ebp+0xC]
9   mov edi, edx
10  rep stosb
11  mov eax, edx
`
```

### Explanation:

```

`Line 1: We set EDI to be the value at EBP+8

Line 2: We save the value of EBP+8 to EDX.

Line 3: Clear out EAX, aka set EAX to 0.

Line 4: Set ECX to 0xFFFFFFFF or -1 by OR'ing all of 
        its bytes with 0xFFFFFFFF. As 0x1 OR 0x1 = 1, 
        and 0x1 OR 0x1 = 1, all bits will be set, 
        making it -1.

Line 5: Read value held in EDI, aka the value pointed
        to by EBP+8, and check if it is the same as EAX, 
        or 0. When we look at the REPNE instruction, we
        can see this is essentially a strlen operation 
        as we will keep repeating this operation and 
        decrementing ECX by 1 each time until we hit 
        a NULL byte terminator aka 0.

Line 6: Add 2 to ECX. If we performed the STOSB 
        instruction 8 times, the value in ECX would 
        be -9. Adding 2 to this value will make it -7, 
        aka the negative equivalent of the number of 
        characters in the string before the null byte.

Line 7: Use the NEG ECX instruction to do a 2's compliment
        operation on ECX and in effect flip the sign bit 
        of ECX, transforming it from a negative number to a
        positive one. In our example, ECX would go from -7 to 7.

Line 8: Move byte held at EBP+0xC into AL.

Line 9: Set EDI back to the address pointed to by EBP+8 
        using EDX, where we had saved the original EBP+8 value.

Line 10: For ECX times, aka the number of characters in the 
         string as determined earlier, write the byte 
         contained at EBP+0xC, aka AL, into the string 
         array pointed to by EBP+8, aka EDI.

Line 11: Move EDX, aka the start of the string buffer
         or EBP+8, into EAX so we can return it to 
         the calling function.
`
```So in essence this could be simplified down to the following in C:```

`int len = strlen(*(EBP+8));
memset(*(EBP+8), (BYTE *)(EBP+0xC), len);
`
```We also can answer the other question as we now know that \[EBP+0xC\] is a pointer to a byte value to use for the memset operation, and \[EBP+8\] is a pointer to a NULL terminated string which we want to memset to the value pointed to by \[EBP+0xC\].

Hope that helps!

\-tekwizz123