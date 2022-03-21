---
title: 'PyCommands Tutorial'
date: 2012-04-01T07:53:00.002-07:00
draft: false
url: /2012/04/pycommands-tutorial.html
tags: 
- PyCommand
---

#### May I know why do I get "Failed to execute sc...
[Unknown](https://www.blogger.com/profile/17692969032060292384 "noreply@blogger.com") - <time datetime="2017-06-18T22:22:53.132-07:00">Jun 1, 2017</time>

May I know why do I get "Failed to execute script!" from PyCommand? Kinda sucks. Btw, I am using Windows XP installed in Virtualbox.
<hr />
#### Hello there, Did you drop the script into "C...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2017-06-19T03:12:09.168-07:00">Jun 1, 2017</time>

Hello there,

Did you drop the script into "C:\\Program Files (x86)\\Immunity Inc\\Immunity Debugger\\PyCommands"? Also are you sure you are using Python 2.7 and not Python 3.x which the script might not be compatible with? Can I ask that you post which script you are trying to run? Happy to help if this still doesn't work out for you.
<hr />
#### Additionally when the "Failed to execute scri...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2017-06-19T03:14:29.524-07:00">Jun 1, 2017</time>

Additionally when the "Failed to execute script!" error occurs, most of the time it displays a traceback. Would you be able to post this at it would help me determine what the error is related to?
<hr />
#### Thanks for the reply. At first, I suspected that t...
[Unknown](https://www.blogger.com/profile/17692969032060292384 "noreply@blogger.com") - <time datetime="2017-06-19T18:43:48.720-07:00">Jun 1, 2017</time>

Thanks for the reply. At first, I suspected that there are some compatibility issues between Python 2.7 and the Immdbg because the stable version of Immdbg preceded Python 2.7 for few years. So, I decided to give Python 2.6 a try, but still the same problem.

Here's the script I extracted from the book, Practical Malware Analysis.

#!/usr/bin/env python

import immlib

def main():
imm = immlib.Debugger()
imm.setBreakpoint(0x00401875)
imm.Run()
cfile = open("C:\\\\temp062da212",'rb')
buffer = cfile.read()
sz = len (buffer)
membuf = imm.remoteVirtualAlloc(sz)
imm.writeMemory(membuf,buffer)
regs = imm.getRegs()
imm.writeLong(regs\['EBP'\]-12, membuf)
imm.writeLong(regs\['EBP'\]-8, sz)
imm.setBreakpoint(0x0040190A)
imm.Run()

Then, I suspected there may be something wrong with the code, so I just tried to run the first few instructions.

import immlib

def main():
imm = immlib.Debugger()
imm.setBreakpoint(0x00401875)
imm.Run()

Even a simple breakpoint and run instruction didn't work too.

So, I tried to remove "#!/usr/bin/env python", it didn't work either.

Prior to this issue, when I was importing immlib to Python, I got some sort of message like "Failed to load DLL for debugger". Then I ran Debugger.pyd with Dependency Walker, I realized that Debugger.pyd couldn't find ImmunityDebugger.exe. Therefore, I decided to move the Libs and Pycommand folders as well as Debugger.pyd and ImmunityDebugger.exe under the same directory. The message disappeared after that, I could import the immlib as usual.

As for the traceback from failed execution, there's no traceback at all.

Thanks and Regards.
<hr />
#### So I fixed your problem. Have no idea why Immunity...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2017-06-20T05:48:51.245-07:00">Jun 2, 2017</time>

So I fixed your problem. Have no idea why Immunity is so borked at the moment but everything under the Debugger class seems to not work for me either at the moment.

Therefore I decided to rip the code from the Run() function as it was defined in immlib.py (which defines the Debugger class) and this seems to work fine.

Here is the final script:



#!/usr/bin/env python
\_\_VERSION\_\_ = '1.0'
import immlib
import getopt
import debugger
imm = immlib.Debugger()

def main(args):
imm.clearState()
debugger.run(0x00401875)
cfile = open("C:\\\\temp062da212",'rb')
buffer = cfile.read()
sz = len(buffer)
membuf = imm.remoteVirtualAlloc(sz)
imm.writeMemory(membuf,buffer)
regs = imm.getRegs()
imm.writeLong(regs\['EBP'\]-12, membuf)
imm.writeLong(regs\['EBP'\]-8, sz)
imm.clearState()
debugger.run(0x0040190A)
return "Done!"

Hope that helps :)
<hr />
#### Thanks for the kind reply. It is really kind of yo...
[Unknown](https://www.blogger.com/profile/17692969032060292384 "noreply@blogger.com") - <time datetime="2017-06-20T18:42:58.336-07:00">Jun 2, 2017</time>

Thanks for the kind reply. It is really kind of you, I will give a try and update you.
<hr />
#### Sir, I want disassembly from the start to end of ....
[Unknown](https://www.blogger.com/profile/12947336080984714418 "noreply@blogger.com") - <time datetime="2019-08-26T04:36:59.736-07:00">Aug 1, 2019</time>

Sir, I want disassembly from the start to end of .exe file. Im getting search for instructions and disassembling those specific instructions but how to apply the same to all instructions. Disasm requires address to disassemble. How can i put a loop on all the addresses sequentially? Please help
<hr />
#### I assume you are using Immunity Debugger here, in ...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2019-08-26T17:57:13.080-07:00">Aug 1, 2019</time>

I assume you are using Immunity Debugger here, in which case I would think that Immunity Debugger should be automatically disassembling the code for you? Is this not the case?

Given Immunity Debugger is pretty much dead by this point I would strongly recommend moving on to more modern programs such as x64dbg or WinDbg.

With that out of the way if you still want to use Immunity Debugger I would recommend that you search the help docs (which have been removed from online access since Immunity Debugger is so old) and find a command which will grab you the base address of the program as well as the size of the program itself.

The main error that I see with your approach however is that the PE file will be a mix of code and data. Your approach assumes that everything in the PE file will be data so you will likely end up with incorrect disassembly as a result if you try process the entire PE file this way.

There may be a way around this by finding out the size of the code segment itself but this isn't really a complete fix to this issue, especially given that programs like IDA Pro use way more heuristics and detection methods to separate code, data, and strings from one another within a PE (aka its a lot of work).
<hr />
#### Thankyou Sir. I want the python code actually of d...
[Unknown](https://www.blogger.com/profile/12947336080984714418 "noreply@blogger.com") - <time datetime="2019-08-27T03:23:49.128-07:00">Aug 2, 2019</time>

Thankyou Sir. I want the python code actually of disassembling start to end addresses. Disasm requires address, instruction or opcode as an argument. I want to ask how i can get disassembly in a text file for every address.
I have tried IDA Pro also but couldnt get IDA Python plugin in its free version.
I have 20000 exe files to disassemble. Please suggest.
<hr />
#### If not Immunity Debugger, how can i get disassembl...
[Unknown](https://www.blogger.com/profile/12947336080984714418 "noreply@blogger.com") - <time datetime="2019-08-27T04:30:47.858-07:00">Aug 2, 2019</time>

If not Immunity Debugger, how can i get disassembly in a file of the complete data section of an .exe file using python? Sorry, I'm new to this therefore understanding as reading blogs.
<hr />
#### In x64dbg\*
[Unknown](https://www.blogger.com/profile/12947336080984714418 "noreply@blogger.com") - <time datetime="2019-08-27T04:31:21.817-07:00">Aug 2, 2019</time>

In x64dbg\*
<hr />
#### Sir, is there an updated link for online documenta...
[Anonymous]( "noreply@blogger.com") - <time datetime="2020-12-08T18:26:44.706-08:00">Dec 2, 2020</time>

Sir, is there an updated link for online documentation? The old one in your article is no longer working.
<hr />
#### No, not that I am aware of, it seems they took the...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-12-08T21:56:36.492-08:00">Dec 2, 2020</time>

No, not that I am aware of, it seems they took the tool offline. Immunity Debugger is very old now and you are probably better off served by tools such as WinDbg or x64Dbg.
<hr />
#### But these two don't support scripts in Python,...
[Anonymous]( "noreply@blogger.com") - <time datetime="2020-12-08T23:15:57.686-08:00">Dec 3, 2020</time>

But these two don't support scripts in Python, do they? I think it pretty handy to be able to write scripts using Python APIs.
<hr />
#### But these two don't support scripts in Python,...
[Anonymous]( "noreply@blogger.com") - <time datetime="2020-12-09T01:26:11.493-08:00">Dec 3, 2020</time>

But these two don't support scripts in Python, do they? I think it pretty handy to be able to write scripts using Python APIs.
<hr />
#### No they don't, although they do support other ...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-12-20T15:52:49.580-08:00">Dec 0, 2020</time>

No they don't, although they do support other types of scripts.
<hr />
