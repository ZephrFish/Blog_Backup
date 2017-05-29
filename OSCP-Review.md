As with most people who sit Offensive Security's courses; [Penetration Testing with Kali(PWK)](https://www.offensive-security.com/information-security-training/penetration-testing-training-kali-linux/) & [Wifu](https://www.offensive-security.com/information-security-training/offensive-security-wireless-attacks/) and achieve Offensive Security Certified Professional/Wireless (OSCP/OSWP) , I too have joined the ranks of people who have passed both successfully. 

As a result this is my course and exam review of both, because... why not? I sat both exams in Feb/March so this review is a little late sorry folks! This post will be split into two main sections, a review of PWK/OSCP as a whole followed by OSWP Material + Exam. 

# Pentesting with Kali
![](/content/images/2017/04/offsec-student-certified-emblem-rgb-oscp.png)

## Are you really ready for OSCP? 

I get a lot of people asking me how difficult the course was/is and if it's worth doing? YES!! it is worth it no matter what your level is. It is considered a great entry level certification for Penetration Testing however can serve as a great resource for testers with some experience too. I found it easy to moderately difficult for some machines, however at the time of sitting the exam I have just over 3 years Pen-testing experience. Some folks may call me out as a noob for sitting it, although I have experience in the industry I am by no means an expert. Every day is a school day and by sitting the course and exam I learned a few extra skills to add to my testing utility belt.

If you're unsure as to where your skillset lies try your hand at some of the machines on [vulnhub](https://www.vulnhub.com), [Andrew Hilton](https://twitter.com/andrewhilton83) did a great write-up on some of the machines to try as a pre-requisite, you can check his write-up out [here](https://medium.com/@a.hilton83/oscp-training-vms-hosted-on-vulnhub-com-22fa061bf6a1).

## Training Material

So as with all other reviews, the first in line is the training material. For those who don't know this comes in the format of a PDF & videos. Personally I went through the videos in one sitting up until the Buffer Overflow material then switched to the PDF to get a better understanding (in addition to the PDF I found [Penetration Testing: A hands-on guide to hacking](https://www.amazon.co.uk/Penetration-Testing-Hands-Introduction-Hacking/dp/1593275641) very helpful in explaining buffer overflows & exploits). 

The videos are very well laid out and come in 5-6 minute sittings per section which is manageable with viewing a few in one go then reading the PDF to catch up. Included along the way in the PDF are some course exercises which I recommend you do as you go; this is because they're worth `+5 pts` in your exam should you be on the line between pass and fail. Alongside the `+5 pts` they also serve as an additional learning resource to allow you to learn more about certain aspects of the course which can be useful if you want to learn more about the aspect each is explaining. Of all of the course exercises the one I recommend you do the most is the section on buffer overflows as it will give you good prep for the labs and potentially the exam.

I tried to do them all whilst I was doing the training material. Unfortunately, like many I fell into the trap of starting on the labs early! As a result, the course exercises fell into the "I'll get to that later" category on my priorities. Which never ended up getting finished :-(, thankfully I gained enough experience in the labs to better prepare me for the exam thus the course exercises weren't required.

## Labs
The labs are worth the whole cost of PWK on their own, they serve as a legal safe play area to hone in your skills and learn new ones. There are many varying degrees of difficult boxes on the labs ranging from novice level through to pretty difficult, however the one main lesson I learned was not to over think things. Think that there's always a way around problem x or y, it is just down to the way you look at it and even sometimes just needs a minute for you to take a step back and think more.

There is no concrete objective for the labs, however it is worth attempting to gain root on a decent number(15+) before looking to book the exam. I managed to gain 'root' access on 33 across the four networks available. That's one thing you might not have heard yet? There's a total of four(4) networks in the PWK labs, each containing different machines. The scenario starts with you being placed in the public DMZ network and the more machines you are able to root/gain access to, the more you end up discovering.

Each box on the networks has a different story, each of them are pwn-able either  manually or some have a point and click exploit option available with metasploit. My advice would be if you can try to find a way to pop the box manually first before taking the easy shell option, mainly because in the exam you only get one fire of the metasploit button - meaning once that life-line is used you're on your own with exploits etc.

## Exam & Documentation
Apart from the course exercises there is little focus on documentation throughout the course apart from when it comes to the exam. From a testing perspective arguably the most important aspect of the test is the report(as that's what your client is paying for in the end!). 

I felt the lack of reporting focus/writing gave the course more of a focus on hacking(which isn't a bad thing by the way) than actual pentesting. However Offsec if you're reading this? maybe you can include some more in an updated version of the course for future delegates.

After sitting the course for a little over 2 months, with 33 `proof.txt` files in my possession I felt I was ready to face the beast that was the exam. So I logged onto the VPN and picked  my exam date for 2 weeks time **`*click*`**, booked...

Now most would think in this situation wewt! Exam booked now for some fun! However not me, given my previous track record with exams I was now nervous for sitting the exam as I often fail these things :-(. I ploughed on through the lab for the next 2 weeks practising bits and pieces in an attempt to better ease my stress(all in the mind!). 

### The Exam Experience

[Flashforward](https://en.wikipedia.org/wiki/FlashForward) 2 weeks and exam day was upon me, I had scheduled start for 10am Monday morning. As my brain doesn't normally wake up till mid morning on a Monday I shot for the hour after 9, however that morning I was WIDE AWAKE at 7am... Nerves I reckon but it felt like an age for the start time to arrive.

**PING** 10.01am by exam email arrives, detailing all the things I need to do where to submit things what to do with x,y & z. Queue mind blank, I forget how terminal works could not work out how to connect the VPN or do anything for the best part of 20 mins. "Oh shit.. I'm failing this" the thought that kept playing on loop in my head for those 20mins until I was back in the zone ready to start. 30 minutes later and I had my first 25 points! another 10 points 20 min later... WOW I thought half way there and it's not even lunchtime.

2 boxes to get and I've passed I kept telling myself for the next few hours then all of a sudden I popped 2 limited shells, great I was 13 points from passing without the lab report however as I'd written up the lab I was hoping I was only 8 points from passing meaning another limited shell and I'd be hopefully golden. Panic stricken the hours drew in and it was late afternoon still 8-13 away from passing the thoughts of "I'm not good enough, I should go back to IT repair" kicked in. 

7pm rolled in and I was starting to doubt myself majorly, however took half an hour out, went for a drive to clear my head came back and **BOOM HEADSHOT**... or almost, I nailed one of the machines to get me another root shell, I was over the mark if I included my lab report 65 for exam + 5 for lab report. I really needed 70 clear in the exam to be safe in my own mind so the minutes rolled on and finally that last box I was picking away at finally fell, I had limited on 2 boxes and 3 root shells so that was 70 points needed to pass 10 hours in!

Naturally however the game was not won yet. Part of the exam is a report that Offsec require you to write-up containing your findings in both technical and executive summary format(just like a pentest irl). As I'd nailed the exam machines in under 24 hours I felt it best to start on the report and what a great choice that was, by about 1am I had it done and had enough time to grab extra screenshots and commands before my time ran out. 

Next day my VPN dropped off just before 10am, I had another run through of the report to check it was in tip-top condition and proceed to send it off to offsec. A little over 24 hours later my email pinged, the results had landed...

![](/content/images/2017/04/oscp.png)

...I'd passed! The feeling of relief and euphoria was amazing afterwards!

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">YEEEEEEAAAAAAHHHHHHH I passed <a href="https://twitter.com/hashtag/OSCP?src=hash">#OSCP</a> thanks for a fun course and exam <a href="https://twitter.com/offsectraining">@offsectraining</a> <a href="https://t.co/X2hFRzJVHT">pic.twitter.com/X2hFRzJVHT</a></p>&mdash; Andy | ゼフラフィッシュ (@ZephrFish) <a href="https://twitter.com/ZephrFish/status/834688154493935616">February 23, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


## Aftermath

The aftermath of having sat the course, well it was great fun but like most I had a feeling of emptiness where I had seen the use of the labs in my free time I no longer had that joy. Instead I turned my focus back to bug bounties and started back at what felt like square one looking for flaws here and there.  After a few weeks though I still had that offsec itch so decided to pay for the WiFu/OSWP course as it was an affordable ~£300. Next comes my review of that course and how I faired in that exam.

# WiFu 3.0
![](/content/images/2017/04/offsec-student-certified-emblem-rgb-oswp.png)

## Are you really ready for OSWP? 
Following on from OSCP, yes you probably are. Haven't done OSCP? Well you're probably capable of doing OSWP on your todd too. As a prerequisite to the course I'd say probs some very basic Linux knowledge will do, the course itself is fairly well explained. If you're feeling left out have a read of some basic wireless hacking too and you'll be set. 

## Training Material & Exercises
As a course it was a different feel than that of PWK, it has the difference in that there are no virtual labs, you need to make your own via the use of your own hardware and office/house. It also feels pretty dated and is long overdue a re-write, mainly focusing on WEP & WPA/WPA2 but there are many more wireless attacks out there now, it doesn't cover off any post-exploitation either which can be fun. 

Anyway the course material starts out by explaining the core basics in wireless which is a bit dull but stick with it, pays off later! Moving swiftly past the theory there's eventually some hands on hacking which we all know and love. This is where having your own hardware comes in as you need to perform different wireless attacks against your access point you've setup(all is explained within the course ware so don't worry). 

As you can probably tell from the price-tag the WiFu course is significantly shorter than PWK and as a result I was able to do all of the exercises and videos in a weekend. Having recently passed OSCP I opted to book the nearest date I could for my OSWP exam, roll on 1 week later exam time was upon me.

## Sitting the Exam
So exam day this time fell on a Sunday, however with all exams something had to go wrong didn't it... This Sunday happened to fall on British Summer Time i.e when the clocks go forward. I was all raring to go at my exam time which was scheduled for 13:00 UK Time, sat down at my desk and kept refreshing my emails for the email to come in. Alas 30 min went by still no email. So I had to email support going erm guys it's 30 min into my exam and there's nothing come in, I only have 4 hours to sit the exam! Little did I know... an hour later support emailed back saying there had been a time difference fault and that this had never happened before :-(. Not a good start, eventually I got my exam material through so all went well to start with...

...until my damn VM decided to crash and I was stuck with building a new VM in the time available, thankfully I was able to nail the exam challenge in the remaining 1 hour and a half. Wrote the report and delivered that on Sunday evening, two days later I got the email I was hoping for, I'd passed OSWP!!!!

![](/content/images/2017/04/oswp.png)

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">At least I passed one out of two exams this week, back to the books next week for round two of <a href="https://twitter.com/hashtag/CCT?src=hash">#CCT</a> in meantime, I passed <a href="https://twitter.com/hashtag/OSWP?src=hash">#OSWP</a> :D <a href="https://t.co/B12ZnLoIux">pic.twitter.com/B12ZnLoIux</a></p>&mdash; Andy | ゼフラフィッシュ (@ZephrFish) <a href="https://twitter.com/ZephrFish/status/846635758761000960">March 28, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Of course having passed OSCP & OSWP in the space of two months was a great achievement for me personally as all previous exams I've sat either infosec or general exams I've failed first time! So relating back to the track record it was set straight again. Until next time folks, OSCE might be in the future one day :-D.
