---
title: 'An Theoretical Approach to Getting Around EMET''s EAF Protection'
date: 2015-01-18T07:48:00.000-08:00
draft: false
url: /2015/01/bypassing-emets-eaf-protection-slightly.html
---

**Update (01/28/15): As pointed out by Gorlob on /r/netsec ([https://www.reddit.com/r/netsec/comments/2tnfac/bypassing\_emets\_eaf\_protection\_a\_slightly/co1t26o](https://www.reddit.com/r/netsec/comments/2tnfac/bypassing_emets_eaf_protection_a_slightly/co1t26o))****, this techique may not work between different build versions of a given DLL. The reason for this is because different patches and updates may end up changing the offsets and locations of various functions within a DLL, such as the msvcrt.dll DLL we use in this tutorialm which would mean that even if I was to target, say a Windows 7 machine, I would have to change the offsets every time that machine was updated or patched. Please keep this in mind when reading the article below, and realize that while this is a bypass, its not really applicable to real world situations as it stands.**


So a lot of people as of late have been talking about EMET an in particular how to bypass many of its protections and features that it offers. I thought this was an interesting challenge given my background in exploit development, so I decided to devote my dissertation paper to finding how effective EMET 5.1 is at preventing people from exploiting programs using three different types of exploits. However a few days ago I hit an interesting problem and today I would like to share the solution I came up with in the hopes that it may be useful to others.

My problem started when I needed to find a way to bypass EAF. For those of you who don't know, EAF protection is a protection offered as part of EMET's protection suite which prevents exploits from reading the Export Address Table (EAT) by blocking access to the EAT based on the origin of the code. Shellcode often needs to read the EAT in order to find out where various functions are in memory that it needs to call. Thus by filtering access to this table, shellcode, including metasploit shellcode, will be prevented from working, and EMET will simply detect an EAF bypass, will alert the user, and the program will be closed down. (Please see [https://download.microsoft.com/download/A/A/8/AA853FAE-7608-462E-B166-45B0F065BA13/EMET%205.1%20User%20Guide.pdf](https://download.microsoft.com/download/A/A/8/AA853FAE-7608-462E-B166-45B0F065BA13/EMET%205.1%20User%20Guide.pdf) for more details, in particular page 8)


There is a way around this however. As discussed in Aaron Portnoy's "Bypassing All Of The Things" presentation ([https://www.exodusintel.com/files/Aaron\_Portnoy-Bypassing\_All\_Of\_The\_Things.pdf](https://www.exodusintel.com/files/Aaron_Portnoy-Bypassing_All_Of_The_Things.pdf)), one can simply obtain the addresses from the Import Address Table (IAT), as discussed in slide 77, rather than the EAT. The IAT contains a list of entires, each which is a pointer to the actual address of the corresponding function in virtual memory.

To take a look at a executable's IAT, we can simply crack open a free version of IDA Pro and wait for IDA to finish examining the entire program. When its done, simply click on the tab labeled `"Imports"`. You should end up seeing something like this:


[![](https://2.bp.blogspot.com/-cEsJ8OwPEPo/VLuyhlLAkmI/AAAAAAAAAeY/ALD4NrLfp4g/s0/2015-01-18%2B13_17_23-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://2.bp.blogspot.com/-cEsJ8OwPEPo/VLuyhlLAkmI/AAAAAAAAAeY/ALD4NrLfp4g/s0/2015-01-18%2B13_17_23-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


On the far left we can see the addresses for each of the IAT entries, followed by an ordinal number if there is one (so that the function can be called via its ordinal number rather than via its normal address. Google this if you don't know what I'm talking about, its a bit beyond this post), the name of the function, and the library from which it came from.

So we have a way to get the actual address of the functions listed in the IAT, but what if the function you want does not exist as an entry within the IAT? Well, first you need to find an entry that exists in the same library as the function that you want to find the address of. Say I wanted to call `ExitProcess`, a function located in `Kernel32.dll`, and I know there is a function `ReadFile` which is also within `Kernel32.dll`.

There is an entry to `ReadFile` within the IAT at `0054E1F4`. Dereferencing this address will get me the address of `ReadFile`. Once I have the address of `ReadFile` I can then take advantage of another trick to get the real address of `ExitProcess`. You see, because `ReadFile` and `ExitProcess` are in the same DLL, they will both have the same base address. These are usually the first 2 bytes of a 32 bit address. Furthremore, all functions are loaded at static offsets from the base address, reguardless of how randomized the DLL might be by ASLR etc. Because of this, with knowledge of where `ReadFile` is in memory, I can simply add or subtract the offset from `ReadFile` to `ExitProcess` from the address of `ReadFile` to get the address of `ExitProcess` in memory. This offset will never change even after the machine reboots and the location of `Kernel32.dll` changes.


However some of you might have noticed a slight problem with this. What if the program doesn't have an IAT entry to a function within the libary your desired function is in? Aka you want to call a function located in `msvcrt.dll` but there are no IAT entries that point to any functions within that library? Well this is exactly the situation I encountered in the field recently. I wanted to call the function `memcpy` using the IAT. `memcpy` is located in `msvcrt.dll`, however as you can see from the photo below, there is no IAT entry for `memcpy`:


[![](https://3.bp.blogspot.com/-sulo7t45yX8/VLu1o3_oq6I/AAAAAAAAAek/yNcHS0wFRT0/s0/2015-01-18%2B13_30_52-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://3.bp.blogspot.com/-sulo7t45yX8/VLu1o3_oq6I/AAAAAAAAAek/yNcHS0wFRT0/s0/2015-01-18%2B13_30_52-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


As a matter of fact, there isn't any entry in the IAT table for function that belongs to a library who's name even starts with "m". We're going to have to get a little creative here.

If we look at the exe within Immunity Debugger, we notice that msvcrt.dll is actually loaded by the application itself. However because of rebasing, the address will change every time the system restarts:


[![](https://1.bp.blogspot.com/-PWlFTJwaxBI/VLu23SbnP6I/AAAAAAAAAew/ynFWzUOQvAU/s0/2015-01-18%2B13_36_01-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://1.bp.blogspot.com/-PWlFTJwaxBI/VLu23SbnP6I/AAAAAAAAAew/ynFWzUOQvAU/s0/2015-01-18%2B13_36_01-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


However there is a way to get this to work. What we can do is abuse some of the properties of `LoadLibraryA` to still dynamically get the location of `memcpy`. Conveniently, the IAT has an entry for this:


[![](https://3.bp.blogspot.com/-rjqgN_fWKjY/VLu4WrguEFI/AAAAAAAAAe8/UWJmvsu9YMg/s0/2015-01-18%2B13_42_09-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://3.bp.blogspot.com/-rjqgN_fWKjY/VLu4WrguEFI/AAAAAAAAAe8/UWJmvsu9YMg/s0/2015-01-18%2B13_42_09-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


Using this entry, we can simply dereference the memory address `0054E268` within our shellcode to get the address of `LoadLibraryA`. Once we have the address of `LoadLibraryA`, we can call it and pass a pointer to the string "msvcrt" on the stack (I have not shown how I dynamically created this on the stack, but its not too hard to do). Here is an example of this in an exploit I am presently working on (to give a more realistic example):


[![](https://3.bp.blogspot.com/-ayes18wWqzM/VLvG1ayzXgI/AAAAAAAAAfM/-oHuvPWJ2Qg/s0/2015-01-18%2B14_44_17-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://3.bp.blogspot.com/-ayes18wWqzM/VLvG1ayzXgI/AAAAAAAAAfM/-oHuvPWJ2Qg/s0/2015-01-18%2B14_44_17-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


As you can see, we are presently calling `LoadLibraryA`, by dereferencing the address in `EDX`. `EDX` contains `0054E268`, or the IAT entry for `LoadLibraryA`. By deferencing and then calling this address I end up calling `LoadLibraryA` itself. On the stack you can also note that I have placed the address where the "msvcrt" string is located, namely `10027090`, as the library that I wish to load. Lets see what happens after this call completes:


[![](https://2.bp.blogspot.com/-sxJyMwlDx-A/VLvHqcKOx7I/AAAAAAAAAfU/sSiX9-Vvh38/s0/2015-01-18%2B14_47_51-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)](https://2.bp.blogspot.com/-sxJyMwlDx-A/VLvHqcKOx7I/AAAAAAAAAfU/sSiX9-Vvh38/s0/2015-01-18%2B14_47_51-Windows%2B8%2B64%2Bbit%2B%5BRunning%5D%2B-%2BOracle%2BVM%2BVirtualBox.png)


As we can see, the base address of `msvcrt.dll` is placed into EAX once the call is done.

This means that based off of one entry within the IAT table, we can figure out the address of any other function in memory. Furthermore, even if the corresponding DLL is not loaded within the current program, `LoadLibraryA` will load the libary for you into the current process and return the base address where that library was loaded into `EAX`. Therefore, reguardless of the load status of the DLL which contains the function you are after, this trick will still work.

At this point, all we need to do is add to `EAX` the offset to the function we want to call within `msvcrt.dll`. These offsets are static, as mentioned before, so even after a reboot they will still remain the same, thus defeating the rebasing.

Anyway, I hope you guys found that interesting and useful. Just a little something I found during my research into EMET.

\-tekwizz123