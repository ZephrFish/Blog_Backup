Currently at time of writing I'm ranked #1 finder of Bugs on https://hackerone.com/pornhub which is a nice position to hold. This post is to explain the techniques I've used to get to where I am and how I found my most recent $2500 bug on pornhub. To update this post also this finding has now been included within [Web Hacking 101](https://leanpub.com/web-hacking-101) by [Peter Yaworski](https://twitter.com/yaworsk).

Difficulty: **Low**

URL: **stage.pornhub.com**

Report Link: https://hackerone.com/reports/119871

Date Reported: **March 1st, 2016**

Bounty Paid: **$2500**

Twitter: https://twitter.com/ZephrFish

####Probing for Holes
Starting out on most bounty hunts & pentests I always carry out reconnaissance, this varies across scopes however for the original pornhub bounty their scope was:

    *.pornhub.com

To which I thought great! ALL the  targets. Subdomain enumeration using various DNS tools my weapons of choice are shown in the list below. However as good as the default lists are, from previous DNS research I have built a custom list of over 1 million unique sub domains pulled from: [All Your Hostnames are Belong to Us](https://haxpo.nl/haxpo2015ams/wp-content/uploads/sites/4/2015/04/D1-P.-Mason-K.-Flemming-A.-Gill-All-Your-Hostnames-Are-Belong-to-Us.pdf).

 - [Sublist3r](https://github.com/aboul3la/Sublist3r)
 - [Fierce](https://github.com/mschwager/fierce)
 - [Enumall](https://github.com/jhaddix/domain)

Long story short, I identified close to 90 unique hostnames, next stage in enumeration for me personally is visually mapping the estate. This was achieved using [Eyewitness](https://github.com/ChrisTruncer/EyeWitness), fed it the list of urls and it dropped a html report of the urls that have valid http/https pages.

From this the fun begins, mapping out the domains for potentially interesting ports open begins. 

####  Odd ports?
Using nmap, I was able to discover some random ports open on a selected subdomain:

    stage.pornhub.com

Firstly using basic nmapping, I decided to have a pop at this subdomain, however an IP is easier to scan than a domain. 

    nslookup stage.pornhub.com
    Server:         8.8.8.8
    Address:        8.8.8.8#53
    
    Non-authoritative answer:
    Name:   stage.pornhub.com
    Address: 31.192.117.70

Now that I have an IP, let the games begin:

**nmap -sSV -p- 31.192.117.70 -oA stage_ph -T4 &**

     Starting Nmap 6.47 ( http://nmap.org ) 
     Nmap scan report for 31.192.117.70
     Host is up (0.017s latency).
     Not shown: 65532 closed ports
     PORT    STATE    SERVICE      VERSION
     80/tcp  open     http         nginx
     443/tcp open     http         nginx
     60893/tcp open   memcache
        
     Service detection performed. Please report any incorrect results at  http://nmap.org/submit/ .
     Nmap done: 1 IP address (1 host up) scanned in 22.73 seconds
    

Looks fairly box standard, but wait, WTF is **memcache**? WTF is port **60893**?!?! 
**lmgtfy**: After some quick research I found it is a service for object caching found here: https://memcached.org/. Further reading landed me at the WiKi for it, https://github.com/memcached/memcached/wiki all the commands required, all the  information! 

#### TL;DR: Memcache
Straight from their website: Memcached is an in-memory key-value store for small chunks of arbitrary data (strings, objects) from results of **database calls**, **API calls**, or page rendering.
What did this mean for me? Well not a lot but surely this shouldn't be exposed to the internet? What happens if I connect to it with netcat? 

##### Entering Deeper
DB & API you say? Now that this new found port was exposed time to poke at it. so simply using netcat to attack it:

    nc 31.192.117.70 60893 -v
    version
	VERSION 2.1.1r-49-gbe05158
	stats
	STAT pid 8141
	--SNIP--
	STAT evictions 0
	END

Nothing malicious, just a quick version check and some statistics. From this I decided this should probably be reported  to Pornhub as the service was clearly responding to commands. Further investigation uncovered that there are more malicious commands that can be run, including clearing the system or causing an overflow. 

As a result I reported this to pornhub who followed this timeline:

 - Reported on Hackerone to Pornhub: March 1st 2016
 - Pornhub Verify it's an issue:  	 March 1st 2016  	
 - Pornhub Triage Bug:  March 1st 2016	
 - Pornhub Award $75 Bounty: March 8th 2016	
 - Report Disclosed Publicly: May 26th 2016 
 - Pornhub re-award vulnerability with a **$2,425** bounty: May 29th 2016 

Pornhub Network issued the following statement:

> We have modified the payout table to better align with the other
> public bounty programs. We want to be fair with every researchers, and
> as such we are retroactively crediting all past submissions according
> to the new payout table.

Overall it was a decent outcome, $2500 is a decent bounty as it was originally, 75 so a 2425 increase is always appreciated.


**This issue has now been fixed, pornhub have closed the port and removed the service**
