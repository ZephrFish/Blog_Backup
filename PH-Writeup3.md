# gif it time it'll come to you - Finding More Holes in The Hub
Following suit of stored cross site scripting vulnerabilities this post will talk about another issue I found in Pornhub. Unfortunately someone found before me and as a result this bug was a duplicate. However, this is my write-up of it and how it was possible, how it worked and the madness of it! 

Finding cross-site scripting vulnerabilities ain't easy and this finding was no different, as you'll see below the discovery method is pretty obscure and likely something people don't usually look for/at.

- Difficulty: **Medium** 
- Risk: **High** 
- Affected URLs: **pornhub.com** 
- Report Link: Closed as ***Duplicate***
- Date Closed: 15th June, 2016
- Date Reported:  15th June , 2016 

The issue resided within the gif generation functionality of the site, whereby an attacker could create a malicious GIF that would result in execution of malicious JavaScript via a data URI.

The gif generation functionality is located at `http://www.pornhub.com/gifgenerator` warning this is likely **NSFW**. **[You have been warned!].**

For those of you who do not know, this is what the gif generation tool looks like, simply select a video URL and it will generate a GIF of that video. To find and create the exploit GIF I was able to do the following.
Insert a random video into the create gif tool, then trap the request in burp suite or another proxy for manipulating requests. Enter any title, tags. Turn on intercept within proxy to catch the request before it gets sent to the server.

I was able to modify the title content and replace with a malicious payload:

`0;url=data:text/html;base64,PHNjcmlwdD5hbGVydCgnWmVwaHJGaXNoJyk8L3NjcmlwdD4="http-equiv="refresh"test=""`

By forwarding this back to the server, this should then save the image title as our payload. From here, when the GIF is visited the User's browser is directed to the data URI which is decoded to the output below and rendered in their browser.

Essentially this is a base64 string of `<script>alert("ZephrFish")</script>` An alert box has been used to demonstrate this. However, this attack will accept any javascript in the form of a data URI.  The reason this attack worked was due to the way the application processed the gif title. When the gif is uploaded it would attempt to inject the title into a `twitter:meta` tag which is pre-loaded on every page the gif was displayed meaning that the attack is launched as soon as the page started to render! The offending tag can be seen in the code snippet below:

`<meta name="twitter:title" content="0;url=data:text/html;base64,PHNjcmlwdD5hbGVydCgnWmVwaHJGaXNoJyk8L3NjcmlwdD4="http-equiv="refresh"test=""">`

As can clearly be seen the base64 content was successfully injected into the tag and as a result was rendered when the page loaded:


This issue has since been patched however it was a fun and interesting vulnerability to exploit through the stages and report back to Pornhub. As the original report has not been publicly disclosed on hackerone I had to request permission from Pornhub to write about this:



Thanks for reading, feel free to [Follow me](https://twitter.com/ZephrFish) on twitter or tweet me
