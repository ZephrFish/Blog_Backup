# Learning the Ropes 101: Basic Networking

In regards to the technological skill set required in this sector, dialling it down to complete basics is where to start. It is very important to understand and get your head around basic networking before you start to look at anything else as networking is how the internet works.

####How Does the Internet Work?
This section will explain at a high level how the basics of the internet works and what is required to simply browse to a website, the process involved and the underlying technology.

#####TCP/IP & DNS
TCP/IP stands for Transmission Control Protocol/Internet Protocol. It's the Internet's fundamental "control system" and it's really two systems in one. In the computer world, a "protocol" is simply a standard way of doing things—a tried and trusted method that everybody follows to ensure things get done properly. So what do TCP and IP actually do?

Internet Protocol (IP) is simply the Internet's addressing system. All the machines on the Internet—yours, mine, and everyone else's—are identified by an Internet Protocol (IP) address that takes the form of a series of digits separated by dots or colons. If all the machines have numeric addresses, every machine knows exactly how (and where) to contact every other machine. When it comes to websites, we usually refer to them by easy-to-remember names (like https://blog.zsec.uk) rather than their actual IP addresses—and there's a relatively simple system called DNS (Domain Name System) that enables a computer to look up the IP address for any given website. In the original version of IP, known as IPv4, addresses consisted of four pairs of digits, such as 12.34.56.78 or 123.255.212.55, but the rapid growth in Internet use meant that all possible addresses were used up by January 2011. That has prompted the introduction of a new IP system with more addresses, which is known as IPv6, where each address is much longer and looks something like this: 123a:b716:7291:0da2:912c:0321:0ffe:1da2. However IPv6 has been around for a while it's still not fully integrated as a normality, many businesses have it setup for sites, but it is not as mainstream as IPv4.

The other part of the control system, Transmission Control Protocol (TCP), sorts out how packets of data move back and forth between one computer (in other words, one IP address) and another. It's TCP that figures out how to get the data from the source to the destination, arranging for it to be broken into packets, transmitted, resent if they get lost, and reassembled into the correct order at the other end.

#####TCP 3-Way Handshake
To fully understand the basics, another topic of the way things work is the 3-Way handshake, in this example we'll talk about a PC connecting to a server/website over HTTP.

In order to establish a connection each device must send a ***SYN*** packet and receive an ***ACK***  from the other device,  following this; one of the SYNs and one of the ACKs is sent together by setting both of the relevant bits (a message sometimes called a ***SYN+ACK***). This makes a total of three messages, and for this reason the connection procedure is called a three-way handshake.

#####Subnets
You may have heard the term subnet before when looking into the topic of networking or it might be completely new to you. Either way, this section will explain what they are why they're important to know about.
######What is a subnet?
A subnetwork or more commonly referred to as subnet is a section of a greater network. It can represent all the machines at one geographic location, in one building, or on the same local area network (LAN). 

######...but why?
Having an organization's network divided into subnets allows it to be connected to the Internet with a single shared network address this can also be paired with network address translation(NAT). Without subnets, an organization could get multiple connections to the Internet, one for each of its physically separate subnetworks, but this would require an unnecessary use of the limited number of network numbers the Internet has to assign. It would also require that Internet routing tables on gateways outside the organization would need to know about and have to manage routing that could and should be handled within an organization.

######Further Reading on Networking
I got into networking by following the Cisco CCNA Networking & Routing fundamentals, I'd suggest looking at the material for sure as the core fundamentals are very useful however the actual certification is only really valuable if you plan to pursue networking as a career.

Other Sources for more information can be found in the reading list below:

 1. [Networking Fundamentals](https://www.amazon.co.uk/Network-Fundamentals-Exploration-Companion-Networking/dp/1587132087/ref=sr_1_4?ie=UTF8&qid=1463610967&sr=8-4&keywords=networking+fundamentals)
 2. [How Does the Internet Actually Work?](https://web.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm)
