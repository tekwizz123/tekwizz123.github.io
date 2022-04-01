---
layout: post
title: OSCE Lessons
#subtitle: 
#cover-img: /assets/img/bsides-london.jpg
#thumbnail-img: /assets/img/thumb.png
#share-img: /assets/img/path.jpg
tags: [OSCE, Lessons]
---
# Intro
This is just a few notes that I had whilst doing the OSCE. Since they have proved to be helpful to me both for the OSCE and for other areas of life I thought I'd share them here both as a help for those taking the OSCE as well as a reminder to myself for the future when I get stuck on something :)

# Lessons
## Lesson 1: Things are Never What They Initially Appear to Be
Ouch. This one was a very tough lesson to learn, but it taught me a lot. A lot of things in the CTP exam appear to be easy or very hard or contain steps that make sense at first, but when you actually get down to them, they are either easier than you thought they were, or, more often, they take more jiggling than what was initially covered on the course.

A prime example of this was the NNM exploit. This one was a pain in the ass for lack of better wording. For a good solid 3 weeks this thing proceeded to get me close to where I wanted (I'm being deliberately vague here to avoid tripping on to spoilers where Offsec might get offended), but it just wasn't quite there. Turns out in the end the exploit wasn't what I had initially envisioned it to be and I had to adjust my expectations and assumptions to get it correctly working again.

A simple change, but it drives a point home. Don't assume that something is easy or hard just cause someone else tells you it is. Take their advice into consideration for sure, but you won't know until YOU do it.

## Lesson 2: Never Give Up
Woot, I've hit the first cheesy line of this entire thing. But its actually one of the most important on this list. There were many times in the exam (which as of writing, I still haven't passed) and the CTP coursework itself where I just felt like giving up. A few times I even decided to take the a few hours off cause of the rage that was induced from continuously banging my head against the problem.

However one has to learn to continue on if you want to succeed. I still haven't managed to perfect this skill but OSCE has taught me that I need to really get a grip on this so as to prevent myself from wasting additional time. There were a few times in the exam where I took too long of a break cause I thought I wanted to just stop working.

I never wanted to plainly give up though, which is what kept me going in the end, but there were definitely a few moments where I just wanted to hit the sack and call it a day. Finding a motive that you want to achieve or a next small step will help you get over the ruts that occur when you just can't seem to get anywhere and you just want to throw the towel in.

## Lesson 3: Time Management
Yeah. I suck at this more than others. Particularly with the OSCE exam. There is a lot of things going on there and you have to keep yourself aware of what your doing and how long you are spending on it. I ended up spending way too long on a particular task, which, while I did manage to crack at the end of the day, ended up taking a significant proportion of the exam time and left me too tired to end up getting the final target I needed.

Ironically enough, when I wasn't pressured by time, I managed to figure out how to get into the third target in matter of probably about 2 hours max. Its that exam pressure that can lead to you making stupid mistakes and lack of cohesive judgement. As @TheColonial helpfully pointed out to me throughout the exam (big thanks to him), its best to remain calm and focused. Panicking will only start to set you back and will result in you overlooking things that could be the key to your success.

## Lesson 4: Your Never Truly Good At Something Until You Do It Hands On
Honestly I think a lot of people don't realize this. You can't simply read a book and expect to know how to do something. I'm not saying don't read books at all or just skip the theory. If you do that, well....good luck ;)

However what I am saying is that I do know of several people in my life or past life who seem to think that they can just read the book and then magically do the material when asked to. Sure, some people can do that by stroke of luck or very quick learning skills, but for the majority of us, your going to have to practice those skills hands on.

The reason for this is that I find there are a lot of things that are different between how someone describes something as happening, and what actually happens in real life. Especially on the CTP course, there a LOT of things that can go pear shaped really fast if you don't try things out and learn from your mistakes. This also applies to other concepts in general.

If you can I would suggest trying to build a lab and get some practice under your belt with real world concepts; it will help more than you think. I'm hoping to do this myself soon, and if I do, I'll try post a guide to help others out. There are already quite a few guides out there already for those who want to get set up now though ;) I'll leave you to go Google for them, but Rapid7 has a fairly nice one if you look that up :)

## Lesson #5: Think O.T.B
In other words, think outside the box. Some of the best learning is done when you really stretch your mind to think of different possibilities that are outside of what you would consider normal. This kind of thinking also allows you to bypass assumptions that might have been put in place by developers or manufacturers and is a good skill to have in general as it allows you to flexibly adapt to different situations and craft solutions to them on the fly.

To give an example, say you have a file that your trying to hide from an antivirus. You could pack the file, but then the packer's signature might get detected. Whats another way you could get around it? Well you could try encrypt the packer's signature to get around the antivirus detection, but what obfuscating the file? Or how about encrypting only the sections that get detected? What if you don't include the payload in the file at all and instead download it from a remote source? What if you add in delays to the file's execution to hinder heuristics? What if you add delays and then split the program up into child processes that then report to one child process that kills the other children once it is done?

Theres a lot of different ways to get to the answer. Depending on your situation you may need to be more or less creative and put in more or less effort, but there is almost always a way to get to the solution. You just have to be patient and willing to get the job done ;)

## Lesson #6: The Last One, Friends
Ok, kinda cheesy, but a good, close group of friends helps much more than you think it will. Especially in the OSCE exam where your emotions will be tested as you ride the emotional roller coaster that is that challenge.

You ideally should have a group of friends that come from different backgrounds. This will allow you to get detailed insight from different friends about various problems that your having from a variety of perspectives and skillsets, which will help you form a more accurate picture about the problem.

You might also find them helpful to have around when you simply want to bounce some ideas off of someone and explain your logic out loud to them, which can help you realize your errors when you have to explain them to someone else.

# Conclusion

Hope that helped some people learn from some of the mistakes that I've had in the CTP course, even if they seem obvious or silly. There all my honest mistakes that I've made in the recent past so none of these are ones I've made up for demonstrations sake.

Let me know what you think of these, if you think there is anything else to add, or if I should start a list of tips and tricks from various mistakes people have made (could be a fun community list :) )