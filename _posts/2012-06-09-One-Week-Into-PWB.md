---
layout: post
title: One Week in To PWB
subtitle: Reflecting on Lessons Learned After a Week of PWB
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [PWB, Lessons]
---

So its now been 1 week since I first started this course and I must say its starting to kick me in the ass in some places. A prime example of this was today. Whilst I obviously can't release all the details of the challenge, the specific challenge that I was struggling with was the extra mile challenge from chapter 6 of the lab. 

The second challenge I got stuck on was actually very hard to do. First off, the exploit must end in "}" for the exploit to be triggered, making it rather interesting to find out where to place your shellcode and how it would fit into the provided space. Second off, as noted in one of the forums, you had to use a DLL from the application itself and not from the OS to get the `POP POP RET` to work for the SEH. This caused much frustration and confusion for me...though in actually it was pretty obvious that I should have done that as its standard to check application DLLs first, then OS DLLs as this makes the exploit more reliable (the application DLLs load in the same place across OS versions, where as many OS DLLs load at different places across OS versions.)

The third challenge was getting all of the bad characters sorted out. To do this I used `generatecodes.pl` and sent this as the payload, then manually checked the results. This allowed me to find that an extra character was also causing problems in addition to the standard ones I had already filtered out, and thus made the application execute the standard SEH handler normally.

The fourth and hardest challenge that I faced though was the limited space available for the shellcode. While I can't tell you what I did to go about it, I can say that Corelan's tutorials definitely helped here, and I was able to craft some shellcode that ultimately resulted in a Metasploit bind shell on port 4444 that was encoded so as to remove instances of some bad characters.

The main thing I want to point out here is that all of this was possible because when one faces the wall, you really do have to try harder. What I found works best is working at it in chunks. I worked till I felt like I was about to give up, tried a thing or two more, and took a break. Yep. A break really does help, and it lets you calm down and think things through more. This last exploit took me literally a day to create (from 12 am to about 10 pm) on and off so it was a lot of work but the breaks helped to make things clearer.

Well thats all I have for now. :)