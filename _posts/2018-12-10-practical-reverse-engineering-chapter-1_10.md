---
layout: post
title: 'Practical Reverse Engineering - Chapter 1 pg 35 - Exercise #5 - KeInitializeDPC and KeInitializeThreadedDpc'
#subtitle: 
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [Pratical Reverse Engineering, Walkthrough]
---

So for starters lets see if there is any MSDN documentation on this function. Looking things up we see this link: [https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/content/wdm/nf-wdm-keinitializedpc](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/content/wdm/nf-wdm-keinitializedpc)

So from this we can tell the function header should be the following:

```c++
NTKERNELAPI VOID KeInitializeDpc(
    PRKDPC Dpc,
    PKDEFERRED_ROUTINE DeferredRoutine,
    PVOID DeferredContext
);
```

The corresponding disassembly of this function can be seen below:

```c++
void __stdcall KeInitializeDpc(PRKDPC Dpc, PKDEFERRED_ROUTINE DeferredRoutine, PVOID DeferredContext)
public KeInitializeDpc
KeInitializeDpc proc near
xor     eax, eax
mov     dword ptr [rcx], 113h
mov     [rcx+38h], rax
mov     [rcx+10h], rax
mov     [rcx+18h], rdx
mov     [rcx+20h], r8
retn
KeInitializeDpc endp
```

Looking at the definition of this function, we immediately notice that `RCX` appears to be some sort of structure that is getting filled out. According to x64 documentation, the first argument is passed in `RCX`, the second in `RDX`, the third in `R8` and the fourth in `R9`. This would mean that `RCX` corresponds to `PRKDPC Dpc`.

But what is `PRKDPC`? If we refer back again to [https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/content/wdm/nf-wdm-keinitializedpc](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/content/wdm/nf-wdm-keinitializedpc), we can see this is a pointer to a `KDPC` object. Using the magic of WinDBG, we can dump the outline of this structure, even though its supposed to be an _opaque structure_ (aka "we may change this structure so don't rely on the details of it remaining the same"), using WinDBG's `dt` command. Specifically, `dt nt!_KDPC` will give us the details we need:

```
0:010> dt nt!_KDPC
ntdll!_KDPC
   +0x000 TargetInfoAsUlong : Uint4B
   +0x000 Type             : UChar
   +0x001 Importance       : UChar
   +0x002 Number           : Uint2B
   +0x008 DpcListEntry     : _SINGLE_LIST_ENTRY
   +0x010 ProcessorHistory : Uint8B
   +0x018 DeferredRoutine  : Ptr64     void 
   +0x020 DeferredContext  : Ptr64 Void
   +0x028 SystemArgument1  : Ptr64 Void
   +0x030 SystemArgument2  : Ptr64 Void
   +0x038 DpcData          : Ptr64 Void
```

Ok so now that we know what offsets correspond to different parts of the `KDPC` object, we can translate the x64 assembly code into the following C code:

```c++
NTKERNELAPI VOID KeInitializeDpc(PRKDPC Dpc, PKDEFERRED_ROUTINE DeferredRoutine, PVOID DeferredContext){
  PRKDPC->TargetInfoAsUlong = 0x113;
  PRKDPC->DpcData = NULL;
  PRKDPC->ProcessorHistory = 0;
  PRKDPC->DeferredRoutine = DeferredRoutine;
  PRKDPC->DeferredContext = DeferredContext;
}
```

If we check this against the HexRays disassembly we can see this is pretty much the same output:

```c++
void __stdcall KeInitializeDpc(PRKDPC Dpc, PKDEFERRED_ROUTINE DeferredRoutine, PVOID DeferredContext)
{
  Dpc->TargetInfoAsUlong = 275;
  Dpc->DpcData = 0i64;
  Dpc->ProcessorHistory = 0i64;
  Dpc->DeferredRoutine = (void (__fastcall *)(_KDPC *, void *, void *, void *))DeferredRoutine;
  Dpc->DeferredContext = DeferredContext;
}
```

So now that is out of the way, the question remains why `TargetInfoAsUlong` is specifically set to a value of `0x113`. Referring back to the  WinDBG dump, we can see that `TargetInfoAsUlong` is actually a structure that contains a one byte `Type` field, a one byte `Importance` field, and a two byte `Number` field.

Therefore setting the value of `TargetInfoAsUlong` to `0x113` actually sets `Type` to `0x13`, `Importance` to `0x1` and `Number` to `0x0`.  To get a better idea of the `KDPC` structure I tried referring to ReactOS, specifically [https://doxygen.reactos.org/d9/d82/struct__KDPC.html](https://doxygen.reactos.org/d9/d82/struct__KDPC.html), however there is no documentation there on any of the fields, just the KDPC structure itself.

So with ReactOS out of the way I ended up turning to Geoff Chappel's website, which has a TON of really cool blog posts on research he has done into the Windows kernel. Specifically he has a brilliant article on KDPC available at [https://www.geoffchappell.com/studies/windows/km/ntoskrnl/api/ke/dpcobj/kdpc.htm](https://www.geoffchappell.com/studies/windows/km/ntoskrnl/api/ke/dpcobj/kdpc.htm). From here we can learn that the value of the `Importance` field is actually `MediumImportance`, which is the default value for a `KDPC` object unless it is changed via `KeSetImportanceDpc`.

This is kind of ironic as if we look at the code for `KeSetImportanceDpc` we will see what has to be the world's simplest kernel function:

```
public KeSetImportanceDpc
KeSetImportanceDpc proc near
mov     [rcx+1], dl
retn
KeSetImportanceDpc endp
```

Which makes sense as this is essentially just taking in a a `KDPC` object along with a byte value as arguments, and is then setting the `Importance` flag of the KDPC object to the byte value specified.

If we now turn our attention to the `Type` field we can see that Geoff Chappel notes that this field is `DpcObject` for a normal DPC or `ThreadedDpcObject` for a threaded DPC. Looking up `DpcObject` brings us to https://www.geoffchappell.com/studies/windows/km/ntoskrnl/structs/kobjects.htm which shows that on Windows kernel versions later than 3.51 (aka any modern OS), the value for `DpcObject` is `0x13`.

For those interested, according to Geoff Chappel `ThreadedDpcObject` is apparently `0x1A` on NT 6.3 and later (aka Windows 8.1 and later), however its value from NT 5.2 to 6.2 (Windows XP to Windows 8) was `0x18`. This can be confirmed if I decompile the function `KeInitializeThreadedDpc` on a Windows 10 build:

```
KeInitializeThreadedDpc proc near
xor     eax, eax
mov     dword ptr [rcx], 11Ah
mov     [rcx+38h], rax
mov     [rcx+10h], rax
mov     [rcx+18h], rdx
mov     [rcx+20h], r8
retn
KeInitializeThreadedDpc endp
```

Again this is pretty much the same code as before, the only difference is that this time the `Type` value is being set `0x1A`, or 
`ThreadedDpcObject`. Otherwise the code is exactly the same as `KeInitializeDpc`.

Finally Geoff Chappel notes that the `Number` field denotes the importance or the processor on which this DPC will be run. I assume given that this is set to `0` that the idea is its either being either set to its initial priority or its being set to run on processor 0. With this knowledge we can create a better decompiled version of both `KeInitializeDpc` and `KeInitializeThreadedDpc`:

```c++
void __stdcall KeInitializeDpc(PRKDPC Dpc, PKDEFERRED_ROUTINE DeferredRoutine, PVOID DeferredContext)
{
  PRKDPC->Type = DpcObject;
  PRKDPC->Importance = MediumImportance;
  PRKDPC->Number = 0;
  PRKDPC->DpcData = NULL;
  PRKDPC->ProcessorHistory = 0;
  PRKDPC->DeferredRoutine = DeferredRoutine;
  PRKDPC->DeferredContext = DeferredContext;
}

void __stdcall KeInitializeThreadedDpc(PRKDPC Dpc, PKDEFERRED_ROUTINE DeferredRoutine, PVOID DeferredContext)
{
  PRKDPC->Type = ThreadedDpcObject;
  PRKDPC->Importance = MediumImportance;
  PRKDPC->Number = 0;
  PRKDPC->DpcData = NULL;
  PRKDPC->ProcessorHistory = 0;
  PRKDPC->DeferredRoutine = DeferredRoutine;
  PRKDPC->DeferredContext = DeferredContext;
}
```

Hope you enjoyed this walkthrough. If you have any questions, comments, or otherwise feel free to drop them down below. I'm still very new to Windows kernel reversing so its quite possible I've stuffed something up, though I'm open to suggestions and improvements so please do send them my way.

More kernel function reversing entries will be coming soon, although they may be separate entries as some of these are a bit longer than I originally expected with the extra background research I'm doing for them.