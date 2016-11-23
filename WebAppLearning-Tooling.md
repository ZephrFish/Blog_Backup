# Web Application Testing - Tooling
Having given an [introduction into web app testing](http://blog.zsec.uk/101-web-testing-1/) it is now time to move onto the tooling. Noting that this is for testing and not specifically bug bounty hunting. The tooling and techniques are slightly different.

The art of web testing is made up of many different areas, the traditional thought process would point directly to web applications however this can vary on a very wide scale. The sub-headings below explain some of the tooling that can be used for each stage of testing. 

###Browsers
Generally the most common browsers for pentesting are Firefox & chrome as these tend to be the most widely used by <s>consumers</s> victims. Personally I tend to use Firefox for most applications and chrome for more heavy applications that require the use of Java, Silverlight or Flash(heathen!). 

Both have a great selection of plugins and add-ons. These are a few I'd suggest you check out:

- [FoxyProxy](https://getfoxyproxy.org/downloads/#proxypanel) {for chrome and Firefox}: Good for quickly switching local proxy if channelling traffic through a proxy such as [Burp Suite](https://portswigger.net/burp/) the interface is easy to use and easy to setup.
- [Wappanalyzer](https://wappalyzer.com/) {for chrome and Firefox}: Very useful addon for quickly identifying the technologies used by the application or any frameworks in user. Particularly useful for noticing when applications are using AngularJS or other frameworks at a glance.
- [Firefox Developer Tools](https://addons.mozilla.org/en-Gb/firefox/addon/web-developer/): It's in the name, this is only for firefox however Chrome's dev tools that are inbuilt and can be accessed by pressing F12 (or if you're reading this on one of those new macs, well you made a bad decision). The dev tools on Firefox as an add-on are great as there are many options including show all the JavaScript or other files on a single page including what's being loaded and where. 
- [Hackbar](https://addons.mozilla.org/en-Gb/firefox/addon/hackbar/?src=search): Again another Firefox only tool, personally I don't use this as I feel it crowds too much of the screen and most of it's functionality can be achieved by using a local proxy however felt I should include it anyway as I've seen a fair few folks using it.

There are many other add-ons and extensions out there however the four described above are the most commonly used. Additionally there are specific browsers that have been created for testing specifically [OWASP Mantra](http://www.getmantra.com/) is worth mentioning. It is a fork of Firefox but with tonnes of plugins and addons built in. I have previously used it before for pentesting however have found that with time it's better to use only the plugins you really want/need rather than have a million and one options!
  

###Proxies
Once you have a browser setup how you like it, usually the next natural step is to setup a proxy to intercept traffic, manipulate it and look at specifics within an app. 

The industry standard for this job is generally burp suite, as mentioned above.  It comes in two flavours, free and pro. The free version feeds the basic needs of most as, it acts as a transparent proxy allowing modification of traffic and manipulation of requests/data. 

The pro version however holds its own too, if you're working professionally as a pentester you cannot go wrong with Burp. It is a tool that should be in every web application tester's arsenal. 

The benefit of using a proxy over testing 'blind' is that you can trap any request, play with it then pass it on to the application. By doing this you will find that a lot of issues pop out straight away such as client side filtering that isn't honoured server side or hidden values that are submitted in POST requests that contain juicy information. The list is endless as to why its a great tool and a great piece of kit to have. 

I am going to do a completely separate post dedicated to burp suite, giving tips on how to set it up, some cool tricks that can be done and some day to day usage top tips.

There are other options out there too though, [OWASP ZAP](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) & [Fiddler](http://www.telerik.com/fiddler) are other options if burp isn't for you. Both of these are free!

###My Setup
Given all the tools of the trade I personally have a somewhat common setup to most, I use several tools in my day to day testing.

####Testing Browser
The browser I tend to gravitate towards is Firefox with `foxyproxy`, `Wappanalyzer` and the dev tools configured.  Configured with burp's certificate for pass-through. 

####Proxy Choice
Burp suite pro is my weapon of choice when it comes to pentesting & for bug bounties I'll generally use a combo of Burp & Fiddler.

####What is your Methodology?
For web app testing usually I start off cold with manual discovery to get a feel for the application. Then content semi-automated discovery using tools like dirb & nikto which are both built into kali. My methodology varies per app per application of testing be this pentesting or bounty hunting I have two seperate mindsets. Reason being that bounties tend to have a much greater scope than a pentest where on a test usually one or two URLs/Apps will be in scope. A bug bounty may have `*.domain.com` in scope meaning the methodology can be switched up, I'd ususally go after DNS then look at ports, then use something like [Eyewitness](https://github.com/ChrisTruncer/EyeWitness) to screencap each app running HTTP/HTTPS.
 
At the end of the day I generally tend to test applications based upon my previous experience in pentesting whilst every day is indeed a school day. Some things are genuinely just broken same old.

##But...what about Automated Tooling though?
To cover off all bases in tooling it would be rude of me not to mention automated tools. I've been asked before what's the best `scanner` or what tool is best for this job?
As a blanket term the advice I'd give to anyone is, learn how to do everything manually before you even think of looking at automated scanning and tooling. For the sole reasons of:

- Not helpful for newbies
- Can be counter productive
- Learn to Walk before you can sprint
- Can have Inaccurate Results
- Will produce False Positives

When you are a bit more accustomed to manual testing only then should you venture forth into the world of automation. In terms of recommendations for automated tools there aren't many *great* ones in my opinion. In pentesting there is *Nessus*(it's expensive!) which is good for some things but terrible and inaccurate for others. 
There is also burp pro's scanner which is getting better and better with every update which is nice to see, kudos to [portswigger](https://portswigger.net/) for developing a great tool. 
