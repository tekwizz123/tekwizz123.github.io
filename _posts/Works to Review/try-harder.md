---
title: 'Try Harder :)'
date: 2012-06-09T10:06:00.000-07:00
draft: false
url: /2012/06/try-harder.html
tags: 
- PWB
---

So its now been 1 week since I first started this course and I must say its starting to kick me in the ass in some places. A prime example of this was today.While I obviously can't release all the details of the challenge, the specific challenge that I was struggling with was the extra mile challenge from chapter 6 of the lab. 



The second one was actually very hard to do. First off, the exploit must end in "}" for the exploit to be triggered, making it rather interesting to find out where to located your shellcode and how it would fit into the provided space. Second off, as noted in one of the fourms, you had to use a DLL from the application itself and not from the OS to get the POP POP RET to work for the SEH. This caused much frustration and confusion for me...though in actually it was pretty obvious that I should have done that as its standard to check application DLLs first, then OS DLLs as this makes the exploit more reliable (the application DLLs load in the same place across OS versions, where as many OS DLLs do.)



The third challenge was getting all of the bad characters sorted out. To do this I used generatecodes.pl and sent this as the payload, then manually checked the results. This allowed me to find that an extra character was also causing problems in addition to the standard ones I had already filtered out, and thus made the application execute the standard SEH handler as per normal.



The fourth and hardest challenged that faced me though was the limited space available for the shellcode. While I can't tell you what I did to go about it, I can say that Corelan's tutorials definitely helped here, and I was able to craft some shellcode that ultimately resulted in a bind shell on port 4444, created via a fairly standard msfpayload encoded to remove all the bad characters.



The main thing I want to point out here is that all of this was possible because when one faces the wall, you really do have to try harder. What I found works best is working at it in chunks. I worked till I felt like I was about to give up, tried a thing or two more, and, wait for the magic...., took a break. Yep. A break really does help, and it lets you calm down and think things through more. This last exploit took me literally a day to create (from 12 am to about 10 pm) on and off so it was a lot of work but the breaks helped to make things clearer.



Well thats all I have for now. :)



\-tekwizz123