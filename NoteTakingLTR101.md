One of the most important tasks to do alongside hacking & reporting is note taking and tracking your work. Why? you might ask, because you never know when a session is going to die or you might use a cool one-liner and want to go back to it. Keeping concise notes of what you are working on is very useful as it will allow you to keep track of little bugs you find, as well as notes on reproducing big ones. 

### Note Taking
When taking notes, there are many tools available for the task and it depends on personal preference too. Two common tools used for this are [Keepnote](http://keepnote.org) & [Microsoft Onenote](https://www.onenote.com), Keepnote is cross platform and works on Linux, Windows & MacOS whereas One note is only Windows & Mac. Others also find it useful to take notes in a text editor of their choice, my personal choice is to use [Notepad++](https://notepad-plus-plus.org) or [Sublime text](https://www.sublimetext.com).

When taking notes, I find it useful to keep track of what I'm looking at by splitting the tasks up into sections. So if I find an interesting looking application or port I'll put a section down for that. An example sketchpad of my notes for an example host, in this case I have used my base domain of `zephr.fish`, the ports noted are purely for example purposes.
```
== Target ==
https://zephr.fish

== Interesting Ports ==
3306 - MySQL
60893 - Memcache
8080 - Possible Web application
60001 - Possible Application or DB

== Web Applications Running ==
8080 - Apache Tomcat
60001 - Adobe Coldfusion

== Possible Attacks ==
RCE - ColdFusion(zephr.fish:60001)
XXE - ColdFusion(zephr.fish:60001)
XSS - Main Domain(zephr.fish:80)
```

The example above shows the target URL I've set out, any interesting ports I've identified and potential exploits available for the technologies running on the box. These exploits/vulnerabilities are usually gathered from a lot of Google-ing. Note taking is a useful skill for any profession, it can be useful for summarising text you've read. I often find it very useful to comment on books/blogs/tutorials I've read to keep them bookmarked for the days I need them.

Topping it off, it is also very useful in testing, when you find a cool vulnerability and want to write it up before you move on.

### Session Tracking
Going hand in hand with note taking is session tracking. Which is essentially noting all the commands you use, the packets you send and the URLs you might visit.

Now that sounds like a lot of work doesn't it? It does  however it can be easily automated using some great tooling and tweaks to your methods. 

You might also be wondering why would I want to keep track of the packets I send? It can be useful for many reasons however the main one being when pentesting, a client environment may experience downtime or issues then turn to the testers at the time to either pass blame or ask for logs. 

Now, if we've been tracking our packets we can easily sift through all of the traffic that was sent to the target to pinpoint if said issue was a result of testing or not. 

However on the other side packet tracking can be very useful to identify how a service reacts to different types of traffic, it can also help you keep track of what content websites reference over different protocols. To achieve this job there are two tools I'd recommend: [tcpdump](http://www.tcpdump.org/tcpdump_man.html) & [wireshark](https://www.wireshark.org/download.html). 

Tcpdump is a command line tool for tracking different types of traffic, it provides the user with an output of both source, destination IP addresses and ports. It comes into its own when you are running a server with only SSH access and no GUI, whereas wireshark is essentially a graphical wrapper for tcpdump it still has it's benefits as you can load pcap files into it that have previously been captured and use its filters to pinpoint certain traffic and protocols. To give some exposure/stuff to play with on wireshark,try the following:
######To start Wireshark
- Open up Wireshark from the Programs menu/open a terminal and type `wireshark&` **Note: This will not work on a ssh only server, also if you do not have it installed it can be obtained from [here](https://www.wireshark.org).**
- Start monitoring the LAN interface
“Capture -> Interfaces…”
- Select the “Start” button next to the LAN interface on your machine.
######Actions
- Open a terminal
- ping `www.google.com`
- Open a web browser
http://www.google.com
- Identify an ICMP request / response pair in Wireshark. *Tip, you can filter for ICMP traffic in Wireshark by entering “icmp” (without quotes) into the “Filter:” text box.*
Identify a TCP handshake in Wireshark. Tip, filter on “tcp”.
- Identify a UDP request / response in Wireshark. Tip, filter on “dns”.
- Identify some none TCP / UDP traffic. What are these packets used for?


Sometimes network traffic isn't everything you want to track, what about that cool one liner you used to grep, cut and sed all the info from that index.html? Or that nmap line that bagged you all the ports and services you needed to find bug x? For this there are several cool things build into *unix that can be used. The first of which is [script](http://man7.org/linux/man-pages/man1/script.1.html) straight out of the manual page it is described as: `script makes a typescript of everything displayed on your terminal.` 

######How is this useful exactly? 
For both pentesting and hunting it can be used to give a print out of all commands run, similar to the use of tcpdump/wireshark in a pentesting sense as you can use it as evidence in a report or feedback to a client. Similarly in a bug bounty report it can be useful to demonstrate the commands and steps taken to find a bug. 

###### Simple Usage of `script`
1. To start logging a session simply type `script ltr101.sh` (ltr101.sh can be named anything, this is just what I'm using for this example).
2. Type as normal, when done type exit. 

###### Example Script
```
$ script ltr101.sh
Script started, output file is ltr101.sh
$ cat index.html | cut -d ">" -f 2 | cut -d "=" -f 2 | sed 's/\"//g'  > wordlist.txt
$ exit
Script done, output file is ltr101.sh
```
Now that we have the file saved it can be viewed either in your favourite file editor or printed out to the terminal with `cat`.

Another highly useful command(more a shortcut) that can be used in unix based systems is `ctrl+r`. I find it really useful to search back through my bash history to use the same commands again or edit them slightly. You can press the up arrow to go through your history. However, it can take a while if you're like me and type a lot of commands. Instead, try `ctrl+r`. To do this: first press `Ctrl + r`, then start typing the command or any part of the command that you are looking for. You'll see an autocomplete of a past command at your prompt. If you keep typing, you'll get more specific options appear. You can also press `Ctrl + r` again as many times as you want to, this goes back in your history to the previous matching command each time.

Once you see a command you need or want to used, you can either run it by pressing return, or start editing it by pressing arrows or other movement keys(depending on your keybindings setup). I find this a really useful trick for going back to a command I know I used recently, but which I can't remember or don't want to look up again. It is also very useful when using ssh, if you can't be bothered typing in `ssh user@x.x.x.x -p xxxx`.

Finally on the topic of session tracking there is one other key to keep in mind, however this is only related to web application & mobile application testing. This is the fantastic feature of Burp Suite Pro - being able to save your session & being able to store everything in a project file. Why might this be useful? 

Essentially a project file on burp stores all of the traffic that has been passed through it, whether this be in scope or not(your scope is set in the scope tab of `target`). It also allows adds a failsafe should java crash(FUU Java) or windows decides updates need installed NOW. Or in general just to have a running log of things that are happening in the browser/burp session.

Did you enjoy this? Check out the other #ltr101 posts [here](https://blog.zsec.uk/tag/ltr101/) or consider buying my [book](https://leanpub.com/ltr101-breaking-into-infosec).
