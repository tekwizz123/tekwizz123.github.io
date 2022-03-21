---
title: 'Practical Reverse Engineering - Chapter 1 pg 35 - Exercise #2'
date: 2018-11-20T10:29:00.003-08:00
draft: false
url: /2018/11/practical-reverse-engineering-chapter-1_20.html
---

**Question: In the example walkthrough, we did a nearly one-to-one translation of the assembly code to C. As an exercise, re-decompile this whole function so that it looks more natural. What can you say about the developer's skill level/experience? Explain your reasons. Can you do a better job?**

My version of the original code after decompilation by hand into Visual Studio:
```
#include "stdafx.h"
#include <intrin .h="">
#include <tlhelp32 .h="">

typedef struct _IDTR {
    DWORD base;
    SHORT limit;
} IDTR, *PIDTR;

BOOL _stdcall DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {
    IDTR idtr;
    tagPROCESSENTRY32 processEntry;
    HANDLE snapshot;
    __sidt(&idtr);
    if ((idtr.base > 0x8003F400) && (idtr.base < 0x80047400)) {
        return FALSE;
    }
    memset(&processEntry, 0, 0x128);
    snapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (snapshot == INVALID_HANDLE_VALUE) {
        return FALSE;
    }
    processEntry.dwSize = 0x128;
    if (Process32First(snapshot, &processEntry) == 0) {
        return FALSE;
    }
    while (strcmp(processEntry.szExeFile, "explorer.exe") == 0) {
        Process32Next(snapshot, &processEntry);
    }
    if (processEntry.th32ParentProcessID == processEntry.th32ProcessID) {
        return FALSE;
    }
    if (fdwReason == DLL_PROCESS_ATTACH) {
        CreateThread(NULL, 0, (LPTHREAD_START_ROUTINE)0x100032D0, NULL, 0, NULL);
    }
    return TRUE;
}

```Based off of the number of checks that are occurring here I would say the developer seems to be aware of the limitations of some Windows functions and what needs to be checked before continuing. Therefore I would say that they have a good level of experience with the Win32 API. My guess based on [https://www.slideshare.net/MattiaSalvi2/virtual-machines-security-internals-detection-and-exploitation](https://www.slideshare.net/MattiaSalvi2/virtual-machines-security-internals-detection-and-exploitation) is that the attackers were trying to identify if the machine is potentially being virtualized so that they could prevent exploitation if such scenarios where encountered, however as described in the book this is a really bad way of doing things as it assumes that the code is running on core 0.

Since each core/processor has its own individual IDT table, if this code was run on a secondary core or processor (aka not processor 0/core 0) then the IDT register value would not be the same as the values the attacker is expecting in this code. Similarly later versions of Windows also change the IDT base address across boots so this adds even less reliability to the technique utilized by the malware. These two points are noted on page 31 where the authors state _"Clearly 0x8003F400 is only valid for core 0 on Windows XP. If the instruction were to be scheduled to run on another core, the IDTR would be 0xBAB3C590. On later versions of Windows, the IDT base address changes between reboots; hence the practice of hardcoding base addresses will not work"_.

As for if one could improve this, I did some research into things and found this paper: [https://www.slideshare.net/MattiaSalvi2/virtual-machines-security-internals-detection-and-exploitation](https://www.slideshare.net/MattiaSalvi2/virtual-machines-security-internals-detection-and-exploitation) by Matta Salvi which mentions something quite similar to what the code in this example was trying to achieve (ironically though VM detection code actually performs the _opposite_ of what the code in the malware is doing. My guess is some old VM software versions _may_ have been acting as virtual Windows XP machines back in the day, hence why malware may try check the IDT base address) as well as a project called Scoopy.

Looking this up links to ScoopyNG located at [https://www.trapkit.de/tools/scoopyng/](https://www.trapkit.de/tools/scoopyng/) which is a project containing a number of Anti-Virtualization tricks that can be used to detect virtualization. Testing these out most of the techniques didn't work anymore (VMWare and VirtualBox have both gotten smarter about detecting Anti-VM techniques, or at least the well known ones), however there were a few VMWare specific techniques that still worked on a recent version of VMWare Workstation when I tested it out. Keep in mind ScoopyNG project is from 2008 though so I'm sure there are a lot better techniques and mitigations out there today (10 years does change a lot of things technology wise).

On the results of testing ScoopyNG on recent versions of VMWare and VirtualBox, I'd imagine this code could be improved to not only use more specific techniques to determine if they are in a virtual machine (and hence stop exploitation if so), but also something like [GetVersionEx](https://docs.microsoft.com/en-us/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getversionexa) to help determine what version of the operating system they are running on if they wanted to target a specific range of operating systems (provided they were targeting OS versions prior to Windows 8.1). This would also get around the limitation of the IDT technique used as regardless of which core the GetVersionEx function is run on, valid OS version information should still be returned.

On a side note though I'd be interested to know if anyone has a better method for detecting VMs that still works these days. The RedPill method that Joanna Rutkowska ([https://www.securiteam.com/securityreviews/6Z00H20BQS.html](https://www.securiteam.com/securityreviews/6Z00H20BQS.html)) published has long since been mitigated and most of the other techniques seem to be specific to the VM software being utilized. Therefore I'm curious if VM detection strategies nowadays have migrated to performing platform specific attacks (like the checks that SnoopyNG did to successfully detect it was running in VMWare Workstation during my tests) or if there are still ways to do things generically (all the generic test in VirtualBox failed and SnoopyNG didn't have any VirtualBox specific tests so it thought it was running in a native machine).