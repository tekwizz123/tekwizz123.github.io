---
layout: post
title: OSCE Review and Experience
subtitle: Reviewing The Hardest Certificate I Have Taken So Far
cover-img: /assets/img/OSCE.png
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [OSCE, Review]
---

# Signing Up

If you have done OSCP before, signing up should be fairly familiar. For those of you masochists out there, the sign up process generally consists of you proving your identity in some way, usually by a scan of your driving license or alternatively, your passport. Because Offsec already had my details on file from when I did the OSCP, I did not need to do anything but click on the personalized link they had sent me for future signups, whereby my details were then passed onto them automatically upon signup.

Once Offsec have received your details and checked your identity, they will then give you the option to pay for the course. As this course is more advanced then OSCP, the price is a little bit higher, at $1500 for 60 days of lab time, or $1200 for 30 days of lab time. Retakes of the exam are $100 per retake.

In my personal experience I would say the best method to take would be to set aside 3 months, as like the OSCP and to take the 60 day option. The reason for this is that granted, you won't need the full 60 days to cover the course, but it gives you the option to train for 45-50 days, take the exam, rest for a day or two, and then figure out where you went wrong on the first try in time for a second exam try.

Furthermore, from experience and what I've heard from others, most people tend to fail this exam on their first try. My personal opinion is that unless you have been doing this sort of stuff for a long time, its highly unlikely you will manage to get it on the first time. So don't expect for that to happen.

Do however expect to fail, and to learn a lot from the mistakes that will occur during your first exam attempt. The period between my first and second exam attempt where I did a lot of personal research into the various topics was actually where I learned the most, and is where two future posts's topics were researched in further detail, to give an example of the level of learning that occurs here.

After you have paid the fee for the course, you will be given access to a calendar from which you can book the start of your course. Please note that your lab access time will start from the start of your course. Contrary to some people's belief, it does not start from the moment you access the labs. You could spend the entire time not in the labs and the timer will still count down. So use the labs to your advantage! :)

Once a date has been chosen, you should receive an email stating:


> Your lab start date is **15/06/14 00:00 (June 15th) GMT 0** at which point, you will receive your course materials and lab account details via email at the following address: **\*removed for my sanity\***


There will also be a link to a sample VPN connection pack that you can use to make sure you can VPN into the labs. If you have any problems during this stage, contact Offsec to find out what might be wrong and how to fix it.

Once you have done all of that, congrats, you now have signed up for CTP :)

# Starting the Labs

Two days prior to starting your labs, you will get an email reminding you of your starting date. This is to make sure that everything is okay with you and to remind you to get prepared :) On the day of the access to the labs, at exactly the time specified or a few minutes after it (depends on your connection speed) you will get a lab email.

In the email will be several details that you will need to be aware of. In particular there are 4 links. **Please note that these links will only be valid for 72 hours. Therefore you need to download all of these and keep a backup of them. If you loose them, good luck mate. Offsec will not provide backup copies.**

Therefore, its best to download all of these links and save them to a safe place. There are:

*   Lab videos (this is going to be a fairly large download)
*   CTP manual
*   Lab connectivity guide (if you've done OSCP this should be familiar)
*   Lab connectivity pack

You will also get your username and password for the lab VPN at this point, as well as access to the control panel for the machines.

Now students of OSCP may find this surprising, but these machines are entirely yours. No one else's. No more fights over control. There just yours to play around with without any delays. You'll soon learn why you need this though, as a lot of the challenges aren't very tolerant of faults and changes in the way you do things.

Finally for the course, Offsec recommends you to use a 32 bit VM of Kali Linux, and provides a link for you to download the VM to your local drive. I would highly recommend using this as not only does this provide a layer of abstraction between your host system and the labs, but it also helps later on if you have any problems with the labs.

Finally, a while later you will receive a email that contains your OSID, your login to the Offensive Security forums, and a link to schedule your exam. Save this email somewhere safe as you will need it when you book your exam.


# Getting Started in the Labs

In the labs you will have access to several machines (I am deliberately not telling you how many as that is a secret for those in the labs I believe. Also to avoid possible pitfalls related to giving hints away, and I'd really like to keep my OSCE if you don't mind :P)

On those machines will be various software components that will relate to the exam handbook. Each chapter of the handbook focuses on a different scenario that Offensive Security experienced when they were doing real world pentests and explains the problem as well as how they walked through each of the steps to solve it. Finally, the problem at the end usually asks you to recreate their work in the labs and might suggest trying to do improvements to it in some way to gain further understanding.

# Topics Covered

I will now provide a breakdown of the topics covered and explain in detail what they entail and my opinion and review on each of them:

## Cross Site Scripting Attacks

This was a pretty easy chapter, but went into how to use XSS in more interesting scenarios and how to use it to steal cookies to hijack sessions as well as how to use XSS to get a shell via a client side attack. This was a nice section however I felt a little bit cheated when they went from "Oh, so we now have admin control" to "Lets get a shell on the system". Thus I might have liked to see some more interesting scenarios being presented here, but the ones presented were beautifully explained.

## Web Fu

At this point the course starts to take a turn for the more interesting as we look into a multi stage web application attack example that takes advantage of one of the flaws that existed in an old version of PHPNuke and examines how we can take what appears to initially look like a simplistic vulnerability that has minimal impact into full fledged system access on the computer by abusing 2 vulnerabilities within the PHPNuke software, combined with a couple of handy tricks.

This was one of my favorite chapters of the entire course, as it really changed by perspective on how even simple vulnerabilities can cause problems when combined later down the line. This was a valuable and important lesson to learn at this stage in the game as it would quickly become more apparent later on how we could abuse this.

## The Backdoor Angle

This course was a nice introduction to LordPE and some of the various tools you would be using in the next chapter. I was surprised to learn how easy it is to backdoor a file but to be honest, when I saw the method they presented, it made a lot of sense. Maybe some more interesting alternative methods could be presented here like alternative data streams, as the section does seem to be quite focused around just the one area in particular, but it was a nice introduction none the less.

## Bypassing Antivirus Systems

This was another fun module. Taking on the stuff learned in the previous chapter, one is taught how to evade common antivirus signature detection technology, the most popular detection mechanism in use today, thus dropping detection rates quite significantly.

The method for this is quite similar to the previous example, with the exception of the code stub. A sample code stub is supplied, but it leaves you enough room and explanation to code your own if you ever need to. The logic behind it not complex if you have a decent familiarity with x86 assembly and even if you don't they walk you through the assembly code so that you understand everything that is going on within the stub.

Pretty brilliant module tbh. Didn't really have any complaints with it minus the fact that I would like to see this updated with a section on bypassing heuristics. These techniques do tend to update more frequently, however at the end of the day there will always be some AV out there that relies on old techniques and thus this would serve well for learning how to bypass it. Furthermore, by studying old techniques, one can learn some common pitfalls of past AV's and search for new methods to bypass AV when the old methods get patched.

## Bypassing ASLR on Vista

This module was one of two modules I think Offsec could have done without in their course. The example of bypassing ASLR on Vista with a sample exploit and a 3 byte overwrite was brilliant, and I have actually incorporated a similar idea into one of the exploits I plan to release in another blog.

However the main downfall of this module is that there is no other place this or similar protection mechanisms such as DEP are talked about. Its just the 3 byte overwrite example and then bamn, next chapter. I would have like to see this section expanded a bit or more examples/technology bypassing included. Perhaps that would push it more into AWE space, but I feel just adding one example for ASLR doesn't serve it any justice.

## Cracking The Egghunter

To me this module was a little bit boring as I have done egghunters for a long time before this course (I started using egghunters in exploits back when I did my OSCP to give you an idea).

That being said, I think it was still a very well written chapter. The Offensive Security guys take you though Skape's popular egghunter, how each piece of the code works, and how it all fits together, and then demonstrates it on a sample exploit. They also explain the benefits of using this egghunter vs making several long jumps.

Overall I liked this, but it was a bit repetitive for me having past experience in egghunters.

## Windows TFTP Server

This was a nice chapter, and was really the section where I learned a lot. In this section one takes a look at a buffer overflow in a popular TFTP server and looks at how to write a Spike fuzzing template for it.

Along the way, I learned many mistakes that can be made when one tries to write a Spike fuzzer and learned how to make a Spike template for a target protocol fairly quickly.

From there one was able to write the fuzzer, run it, wait for a few minutes, and get a controllable crash. The rest of the chapter then walked through how to exploit the crash in a semi-difficult environment.

## HP NNM

The monster, the beast, the main challenge, pain made in hell itself. Whatever you want to call it, this one is going to take at least 3-4 times the time of the other modules in order for you to fully understand it all. So don't be afraid of it, just realize its a fairly complex exploit.

Overall this module was probably the second one that really taught me a lot. From custom encoding shellcode to making my own shellcode to figuring out really stupid problems with my code, this one caused me a bit of grief and strife. But this was only a taste of what was to really come. Alas this was and still is to this day what I consider one of the nastiest exploits I have come across.

I really can't say too much about this one besides from that, just make sure you study this one well. A lot of the course's lessons and knowledge derive from this one section, so take some time out of your day to understand this section really well.

# The Exam Try 1

So after 1 month of study and another few weeks off for vacation, I decided to do my OSCE exam. It was a tight squeeze to get it to fit in time, but I managed to make it work.

Boy. Did that thing kick my ass black and blue. For a good solid 8 hours I got stuck on one of the challenges, only to realize my mistake was due to trying to do things too fast. Once I figured that out, it only took me 2 hours to figure out how to get full privileges on that system.

Aside from that, I was only able to pop one other machine, however it was so easy for me that I didn't feel any satisfaction really in popping it minus the points value.

After that I was stuck with X (a number between 1 and 5) other machines. I didn't realize it at the time, but I had actually completed another without my knowledge, due to a misreading of the instructions. Later, on my second try, I realized this mistake and was able to fix it.

# Post Exam Try

The other machines I was unable to compromise. At this point I was a messy pile of half stress, half satisfaction and half confusion. I had trained for this? I should be able to get at least a bit more than this? I mean I know its meant to be hard but it shouldn't be that bad right? It quickly came to me that I was going to have to do more research into the various sections I missed.

So I took some time off and researched whilst I had a few family vacations I couldn't move around (greencards and other important stuff). Along the way I managed to find a vulnerability in Kolibri WebServer 2.0 via an overly long POST request (https://seclists.org/bugtraq/2014/Aug/86), and trained my skills in developing fuzzer templates for various protocols.

I also learned custom shellcode and made some nifty examples for use the second time around. I also made some tools to automate some of the problems that I had in the labs, which came in handy thought-out the exam, especially one of the tools which I estimate cut the time down for one of the sections by about 2 hours in total.

By the time I was done, I had working solutions for each of the targets in the exam lab ready to go. I then went ahead and rebooked my exam. This date was then moved again after I learned that I had to undergo dental surgery for my wisdom teeth which might potentially result in me being very out of it during the exam. As thus I then scheduled my exam for the Monday after the appointment, which also gave me the time to go see a friend (@TheColonial) and some mates (@stevelord, @n0x00, @\_Freakyclown\_) the day before for dinner.

# The Exam Try 2

The second try of the exam was much much easier, as I had a general idea of my previous pitfalls and knew roughly what to expect and how to get around my previous issues that I faced.

The whole process ended up taking quite a long time as I had to do new screenshots for each of the hosts, and there were a lot of screenshots, but eventually I manged to complete the first two machines. By this point I was at pretty much the same point as last time.

I then tried the one I didn't get last time. For some reason the exploit worked locally, but not remotely. I checked with an Offsec admin that this was to be expected, and after confirming that it was, went back to work on the target. I tried many solutions against the target, all working locally but not remotely. Eventually I realized I was getting nowhere.

At this point I then went back to the other targets that I had missed and got them done. Having feeling satisfied that I had managed to nail the other ones, I went back to the challenge one.

From then on, this thing proceeded to kick my ass. Despite my best attempts, I was only able to get partial access to this box. However I didn't even know if I would get partial points on it. The instructions and specifications for the points for this target in particular where a little bit unclear and left me on the edge of passing.

Realizing this, I decided to send an email off to challenges AT offensive-security DOT co(take a guess you spam bots). However they said they would be unable to answer any of my questions until I submitted the exam documentation in. Alright then....guess theres only one way to figure this out.....going to have to do this report.

# Writing the Report

Report writing is never fun, but this one was a real pain. In total it took me just under the 24 hours prescribed to write the report with a 4-5 hour nap during all of that. This was probably due to me including too much detail, and is something that I need to work on in my report writing. I like writing very detailed reports but there is a certain point where you just start adding detail for the sake of it rather than trimming it down to only what is needed to get your point across. Hopefully as I write more reports I'll learn from my mistakes and be able to tweak that but at present it remains one of my main pitfalls.

Anyway, after 24 long hours, I finally managed to get my report in by 11 am. At this point all I had left to do was wait for the results. After fixing some issues with Gmail being a right pain with the documentation format I eventually decided to submit everything as a text file with instructions on how to turn the files back into their respective formats to get around the filtering. At this point I was getting really tired, so I decided to go to sleep and check the results in 2 days.

2 days later went back to check my results. It turns out that within 24 hours of me taking the exam, they had emailed me to confirm that I had passed! I was ecstatic. Finally after all this time I had gotten the OSCE certification! All the hard work and pain had finally paid off in one spectacular email :)

# Review and Reflections

Overall this course was a hell of challenge. But I would definitely recommend it to anyone who wants to improve their security skills to the next level. It is a lot of work and your going to need some time to understand all the concepts thoroughly, but it is well worth your time and will greatly help to advance your skills in the realm of web application security, exploit development and creative thinking.

What will I be doing on from here on out? Well to be honest I am not sure. I have a final year of university I need to finish first before I do anything job related. However at the same time with only about a class a day I might still have time to conduct spare research and activities outside of my classes and outside class research time (which will be needed for my dissertation). My primary focus this year is going to be my dissertation of course as that counts for two of my regular courses.

With that aside, I would quite happily do another Offsec certification. I have my eyes on AWE next, but due to the costs involved with that and a lack of sponsorship, I may have to wait for now whilst I save the money up for a hotel, the course, and food/general living expenses, as well as travel.

Anyway, I hope you enjoyed this review of the OSCE certification. Let me know if you agreed or disagreed with my opinion on anything in this post. I'm interested to hear what you think.