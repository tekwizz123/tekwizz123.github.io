---
layout: post
title: Day Before PWB Exam
#subtitle: 
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [PWB]
---

Okay so its the day before PWB starts and I'm super bored so I thought I would give ya all an update.

Anyway, today I'm officially graduating from school :) Ceremony starts at 2pm and as I write this right now its 10:30 am, though I was up at 8 am after by bro busted into my room to find a calculator for his SAT test (yep today is one of those days)

Anyway enough about me, and onto my thoughts about PWB. Yesterday I asked around to find what would be the best way to prepare on my last day before the exam and some people have recommended that you do some boot2roots. (A great list can be found on g0tmi1k's website. Search google for "vulnerable by design" and you should find it)

So naturally I downloaded some, specifically all of the Kioptrix ones. So far as of this morning I have managed to get through the first three, though I had to ask for help on one of them (the second level) because the exploit on exploit-db is out of date, and thus won't work, and on another one.

Specifically that second one was an interesting lesson. This is something you may want to keep in mind when pentesting:

*   Don't upload php files to a server via wget without renaming them to .php.txt first.

Why? Well, if you don't rename the file, wget will automatically interpret that file for you on the server. Which is not good if all that results in is a shell on your own server rather than on the remote host hehe.

Yeah other than that i'm working on r00ting Kioptrix Level 4 atm, but it seems like one of the files is missing from the archive.....odd, but i'll figure it out soon.

Also have downloaded a few wordlists from g0tmi1k's site in case I need them for bruteforcing and went ahead and printed out his privilege escalation guide that will help me when I get stuck on getting r00t on the linux hosts.

That's pretty much all i have to say for now....lets see what tomorrow brings.