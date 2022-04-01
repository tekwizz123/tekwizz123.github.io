---
layout: post
title: 'PWB Exam Review and Insights'
#subtitle: 
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [PWB]
---

Okay I guess a blog post has been long overdue by this point. Looking back I see the last post I made was a month ago. Let me explain what happened between now and then.

Ok basically after I got stuck in that two week rut, I come to realize that I should ask for help. So basically what happens is that I end up asking people for help, and in exchange I help them out with their problems on #offsec. This actually works quite well, as I end up reinforcing the information that I already learned while learning what I am doing wrong with new ones.

Unfortunately I have a bit of a problem after that. I got into another 4-5 hosts but I realize I need to do better. Too bad then, because right in the middle of all of this, I have another vacation for a week. To be honest, in retrospect I think I needed that vacation because when I came back from learning how to scuba dive on said vacation, my mind was so much more refreshed and focused.

Anyway, that aside I come back from the vacation and I realize I only have like 3 days left. This then turns into a mad rush as I focus on getting as many last hosts as I can and ask people for help, giving what I can in return. I realize that many of these hosts were not that hard, and that I had just been approaching it from the wrong angle (ex performing brute force on the wrong service or overlooking a simple detail that I underestimated the importance of).

So I end up hacking like another 5-6 hosts in those last few days. And to be honest, I feel rather sad. Why? Well several reasons:


1.  I'm out of lab time. Those labs were fun damn it!
2.  I don't know if I'm really ready at all.
3.  I don't know what hosts I should practice attacking in preparation for the exam.
4.  I didn't get into the Dev or Admin network, only the IT one.
5.  People generally f\*\*\*\*d up the host that connected the student network to the dev one.

\* On a side note I will say that the decision to take the exam a bit earlier than I planned was not mine. I have college coming up in case you haven't already noticed, and once I get in I am sure as hell not doing something like this on top of course work. So once I realized this, I had to quickly draw up a plan that would allow me to get a retake if necessary and still give me time to pack up my stuff for England.\*

Back to the story...

So I have about a week off before I start the exam. So I book it for Wednesday, and start studying. I find a site called [boot2root.info](https://boot2root.info/) which is hosted by g0tmi1k (amazing guy) where I found pretty much every boot2root you will ever need. Ever.

No I wasn't kidding.

EVER.

So after i'm done jumping around the room like a crazed ape, I decide to download a few challenges. Some them I did before, but I thought it would be good to do them again. So I went through Kioptrix 1, then Kioptrix 2, Kioptrix 3 and then WackoPiko. WackoPiko was tbh interesting. I got a web shell in a few minutes (easy one to do tbh) but I got stuck escalating to root. I kept getting mmap not allowed errors.

All of a sudden I hit on one that works. For some reason, the mmap from this one was allowed and I get my root shell :) w00tw00t! I kid you not, that gave me the most immense high of the entire course. And it wasn't even one of their machines :P Idk why, but it just did. The feeling of defeating what looks to be a brick wall is one that never gets old.

Anyway Tuesday rolls around and I wake up and start my day normally. At about 12 pm I check my email and this is pretty much what happens:

\*Login in\*
Ooh email from offsec.
Wait....hold on...exam?
\*click exam pack email\*
WHAT THE MOTHER \*\*\*\*\*\*\*\*!?
IT WAS TODAY?
HOLY.....
\*runs to pc and downloads everything\*
\*runs to other pc and prints out guide\*
F\*\*\* F\*\*\* F\*\*\* F\*\* F\*\*\* F\*\*\* F\*\*\*

Yeah it was that bad. I had lost 6 hours of my lab time, not realizing that I had booked it for Tuesday. The exam warning email even said this, but I had mistakenly not checked the date (which is sent to you in a format like M/D/Y, and not like Tuesday 12th June 2012.)

So I start the exam. I start running the scans and then damn it, then internet cuts off. Connect back to internet and continue scans...

I gather all the information I can about all the ports available + run any information scans I can run. I keep one file open for my scans, and another for each of the steps that I took.

I then read over the lab guide while waiting for the scans to run. Looking it over I realize that the exploit-dev one will most likely take the longest and decide to start on that one first.

\*5 hours later\*

I test the exploit. I realize it doesn't quite work yet. Modify the exploit

\*5 1/2 hours later\*

I get a system shell on the first host and gather the information I need to prove I hacked it.

The next one falls rather easily, though I end up using a metasploit lifeline on it....hmm nothing here that I can use on the other hosts. I gather all the information I can and attempt the next one.

The next 3 go rather slowly. One of them I end up giving up too early and find what I need after trying all possibilities. From there I get stuck again. After a suggestion from a friend I get in, and get stuck trying to do my privilege escalation. Nothing seems to work. Once again, bouncing ideas off of a friend helps clear my head and I get root on the system.

The same goes for the other one.

The last one I manage to get a shell on the system, but I am unable to root it. In the last hour I attempt to crack the SSH login for one of the users, but of course the time runs out. And when I mean runs out I mean at 24 hours they literally cut you off and force you out. No oh...you can get 10 more minutes. When they say 24 they mean 24. No more, no less.

In the end I end up getting 4 systems down and 1 partially compromised, scoring more than enough points to potentially pass. I crash into bed, which actually turns out to be a rather sleepless night, as I keep tossing and turning. Not that im scared about anything tbh, but my body is still on the "We are Tron. We do not stop. We do not falter. Continuing the hack....w00tw00t found....shell time mode..." kind of mode.

Anyway next day I wake up, and I just generally fool around with some British comedy for the next hour or so. Eventually I get my mind focused and get down to it. It ends up taking a hell of a lot longer than I thought. I must have gone from about 10 am to about 9 pm. No joke. Then again I was on Skype and IRC some of the time so that's probably why. But still, I feel ashamed. So 9 pm roars its messed up son of a gun face and I find out via IRC that I need to add in the exploit details for each exploit. Ok no problem. So I go and get that done. Wait....what about the URL for each and every exploit? Oh god...

So I go on a very long quest to get that done. And believe me, doing that as the last thing on the report is NOT FUN AT ALL. It annoyed me. It taunted me. But I got it done in the end.

11 PM

God time moves fast. I pm rAWjAW. I ask him how big the file size needs to be. He says 10 MB per attachment.

Ok np. Lets compress this baby.....or not? I find it only goes down 1 MB after I attempt to compress it.

\* One hasty convo with connection later....\*

I find out I really aught to do it in PDF. But PDF is 16 MB....meh I'll try the other method.

So I take my .ODT file and try and split that into multiple files.....

\*Some long minutes later\*

OOPS. Google doesn't allow sending more than 25 MB in attachments. And my .odt report is 31 MB.

I go and fetch the .pdf file and split it up into parts. All good. Upload and send.

And then I wait....

And I wait.....

And eventually I wait an hour.

I get tired of waiting and PM rAWjAW again.

"Hey mate, did you get the email?"

"We will check now, give us a sec"

By this point I'm so tired I'm not thinking straight.

"Yep we got it, should receive an email soon"

And what do I do? Look at soon, think oh....lets wait some more.

2 AM

Eventually I look at that message again and think...oh I aught to check it now.

\*checks\*

Yep its there.

I shut everything down for the night and head to bed.


And now here we are the day after all of this, and I have to wait around 3 days for the results to come back. Should be good :) So thats my entire journey put to an end.