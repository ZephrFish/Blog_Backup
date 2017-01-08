Following my post on [Web Application Testing Methodologies](https://blog.zsec.uk/ltr101-methodologies/), I received a lot of feedback and requests to elaborate more on the methodology. As it is geared towards pentesters, some newbies might not understand what things are or what tools can be used to achieve the goal. 

I have tried my best to outline tools for each stage of methodology below and further reading for each. Additionally breaking down each stage with more information on how to do each check has been requested by several folks; these will make up future posts otherwise this will end up being massive!

***Buckle up, it's going to be a long one ladies and gentlemen...***

#Recon Tooling
 - Utilize port scanning
 -Don't look for just the normal 80,443 - run a port scan against all 65536 ports. You'll be surprised what can be running on random high ports.  Common ones to look for re:Applications: 80,443,8080,8443,27201. There will be other things running on ports, for all of these I suggest [ncat](https://nmap.org/ncat/) or [netcat](https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf) OR you can roll your own tools, always recommend that!
	  - Tools useful for this: [nmap](https://nmap.org/), [masscan](https://github.com/robertdavidgraham/masscan), [unicornscan](https://kalilinuxtutorials.com/unicornscan/)
	  - Read the manual pages for all tools, they serve as gold dust for answering questions.
 - Map visible content 
	 - Click about the application, look at all avenues for where things can be clicked on, entered, or sent.
	 - Tools to help: [Firefox Developer Tools](https://addons.mozilla.org/en-Gb/firefox/addon/web-developer/) - Go to Information>Display links.
 - Discover hidden & default content
    - Utilize [shodan](https://account.shodan.io/register) for finding similar apps and endpoints - Highly recommended that you pay for an account, the benefits are tremendious and it's fairly inexpensive.
    - Utilize the [waybackmachine](https://archive.org/web/) for finding forgotten endpoints
    - Map out the application looking for hidden directories, or forgotten things like /backup/ etc. 
    - Tools: [dirb](https://github.com/seifreed/dirb) - Also downloadable on most linux distrobutions, [dirbuster-ng](https://github.com/digination/dirbuster-ng.git) - command line implementation of dirbuster, [wfuzz](https://github.com/xmendez/wfuzz),[SecLists](https://github.com/danielmiessler/SecLists).
 - Test for debug parameters & Dev parameters
	 - RTFM - Read the manual for the application you are testing, does it have a dev mode? is there a `DEBUG=TRUE` flag that can be flipped to see more?
 - Identify data entry points
	 - Look for where you can put data, is it an API? Is there a paywall or sign up ? Is it purely unauthenticated?
 - Identify the technologies used
	 - Look for what the underlying tech is. useful tool for this is nmap again & for web apps specifically [wappalyzer](https://wappalyzer.com/).
 - Map the attack surface and application
	 - Look at the application from a bad guy perspective, what does it do? what is the most valuable part? 
		 - Some applications will value things more than others, for example a premium website might be more concerned about users being able to bypass the pay wall than they are of say cross-site scripting. 
		 - Look at the application logic too, how is business conducted?


# Access Control Testing
###Authentication
The majority of this section is purely manual testing utilizing your common sense and eyes, does it look off? Should it be better? Point it out, tell your client if their password policy isn't up to scratch!

 - Test password quality rules
	 - Look at how secure the site wants it's passwords to be, is there a minimum/maximum? is there any excluded characters - `'`,`<`, etc - this might suggest passwords aren't being hashed properly. 
 - Test for username enumeration
	 - Do you get a different error if a user exists or not? Worth noting the application behaviour if a user exists does the error change if they don't?
 - Test resilience to password guessing
	 - Does the application lock out an account after `x` number of login attempts?
 - Test password creation strength
	 - Is there a minimum creation length? Is the policy ridiculous e.g "must be between 4 and 8 characters **passwords are not case sensitive**" -- should kick off alarm bells for most people!
 - Test any account recovery function
	 - Look at how an account can be recovered, are there methods in place to prevent an attacker changing the email without asking current user? Can the password be changed without knowing anything about the account? Can you recover to a different email address?
 - Test any "remember me" function
	 - Does the remember me function ever expire? Is there room for exploit-ability in cookies combined with other attacks?
 - Test any impersonation function
	 - Is it possible to pretend to be other users? Can session cookies be stolen and replayed? Does the application utilize anti-[cross site request forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))?
 - Test username uniqueness
	 - An you create a username or is it generated for you? Is it a number that can be incremented? Or is it something the user knows and isn't displayed on the application?
 - Check for unsafe distribution of credentials
	 - How are logins processed, are they sent over http? Are details sent in a POST request or are they included in the URL(this is bad if they are, especially passwords)?
 - Test for fail-open conditions
	 - Fail-open authentication is the situation when the user authentication fails but results in providing open access to authenticated and secure sections of the web application to the end user.
 - Test any multi-stage mechanisms 
	 - Does the application utilize multi-steps, e.g username -> click next -> password -> login, can this be bypassed by visiting complete page after username is entered?(similar to IDOR issues)
	 -  Session Management
	- How well are sessions handled, is there a randomness to the session cookie? Are sessions killed in a reasonable time or do they last forever? Does the app allow multiple logins from the same user(is this significant to the app?).
	- Test tokens for meaning
			- What do the cookies mean?!
 - Test tokens for predictability
	 - Are tokens generated predictable or do they provide a sufficiently random value, tools to help with this are Burp Suite's sequencer tool.
 - Check for insecure transmission of tokens
	 - This lies the same way as insecure transmission of credentials, are they sent over http? are they included in URL? Can they be accessed by JavaScript? Is this an Issue?
 - Check for disclosure of tokens in logs
	 - Are tokens cached in browser logs? Are they cached server side? Can you view this? Can you pollute logs by setting custom tokens?
 - Check mapping of tokens to sessions
	 - Is a token tied to a session, or can it be re-used across sessions?
 - Check session termination
	 -  is there a time-out?
 - Check for session fixation
	 - Can an attacker hijack a user's session using the session token/cookie? 
 - Check for cross-site request forgery
	 - Can authenticated actions be performed within the context of the application from other websites?
 - Check cookie scope
	 - Is the cookie scoped to the current domain or can it be stolen, what are the flags set> is it missing secure or http-only? This can be tested by trapping the request in burp and looking at the cookie. 
	
 - Understand the access control requirements
	 - How do you authenticate to the application, could there be any flaws here?
 - Test effectiveness of controls, using multiple accounts if possible
 - Test for insecure access control methods (request parameters, Referrer header, etc)

# Input Validation
 - Fuzz all request parameters
	 - Look at what you're dealing with, are parameters reflected? Is there a chance of [open redirection](https://zseano.com/tut/1.html)?
 - Test for [SQL injection](https://www.owasp.org/index.php/SQL_Injection)
	 - Look at if a parameter is being handled as SQL, don't automate this off the bat as if you don't know what a statement is doing you could be doing `DROP TABLES`.
 - Identify all reflected data
 - Test for [reflected cross site scripting (XSS)](https://www.owasp.org/index.php/Testing_for_Reflected_Cross_site_scripting_(OTG-INPVAL-001))
 - Test for [HTTP header injection](https://www.gracefulsecurity.com/http-header-injection/)
 - Test for [arbitrary redirection](https://zseano.com/tut/1.html)
 - Test for [stored attacks](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))
 - Test for [OS command injection](https://www.owasp.org/index.php/Testing_for_Command_Injection_(OTG-INPVAL-013))
 - Test for [path traversal](https://www.owasp.org/index.php/Path_traversal)
 - Test for JavaScript/HTML injection - similar to xss
 - Test for file inclusion - both [local](https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion) and [remote](https://www.owasp.org/index.php/Testing_for_Remote_File_Inclusion)
 - Test for [SMTP injection](https://www.owasp.org/index.php/Testing_for_IMAP/SMTP_Injection_(OTG-INPVAL-011))
 - Test for SOAP injection - can you inject SOAP envelopes, or get the application to respond to SOAP, this ties into XXE attacks too.
 - Test for[ LDAP injection](https://www.owasp.org/index.php/LDAP_injection) - not so common anymore but look for failure to sanitise input leading to possible information disclosure
 - Test for [XPath injection](https://www.owasp.org/index.php/XPATH_Injection) - can you inject xml that is reflected back or causes the application to respond in a weird way?
 - Test for template injection - does the application utilize a templating language that can enable you to achieve xss or worse remote code execution?
	 - There is a tool for this, automated template injection with [tplmap](https://github.com/epinna/tplmap)
 - Test for [XXE  injection](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing) - does the application respond to external entity injection?


# Application/Business Logic
 - Identify the logic attack surface
	 - What does the application do, what is the most value, what would an attacker want to access?
 - Test transmission of data via the client
	 - Is there a desktop application or mobile application, does the transferral of information vary between this and the web application 
 - Test for reliance on client-side input validation
	 - Does the application attempt to base it's logic on the client side, for example do forms have a maximum length client side that can be edited with the browser that are simply accepted as true?
 - Test any thick-client components (Java, ActiveX, Flash)
	 - Does the application utilize something like Java, Flash, ActiveX or silverlight? can you download the applet and reverse engineer it?
 - Test multi-stage processes for logic flaws
	 - Can you go from placing an order straight to delivery thus bypassing payment? or a similar process?
 - Test handling of incomplete input
	 - Can you pass the application dodgy input and does it process it as normal, this can point to other issues such as RCE & XSS.
 - Test trust boundaries
 
	 - What is a user trusted to do, can they access admin aspects of the app?
 - Test transaction logic
- Can you pay £0.00 for an item that should be £1,000,000 etc?
- Test for Indirect object references(IDOR)
- Can you increment through items, users. [uuids](https://www.rohk.xyz/uber-uuid/) or other sensitive info?

	 

# Application Infrastructure
 - Test segregation in shared infrastructures/ virtual hosting environments
 - Test segregation between ASP-hosted applications
 - Test for web server vulnerabilities - this can be tied into port scanning and infrastructure assessments
 - Default credentials
 - Default content
 - Dangerous HTTP methods
 - Proxy functionality


# Miscellaneous tests
 - Check for DOM-based attacks - open redirection, cross site scripting, client side validation. 
 - Check for frame injection, frame busting(can still be an issue)
 - Check for local privacy vulnerabilities
 - Persistent cookies
 - Weak cookie options
 - Caching
 - Sensitive data in URL parameters
 - Follow up any information leakage
 - Check for weak SSL ciphers
 - HTTP Header analysis - look for lack of security headers such as:
      - Content Security Policy (CSP)
      - HTTP Strict Transport Security (HSTS)
      - X-XSS-Protection
      - X-Content-Type-Options
      - HTTP Public Key Pinning

Hopefully this post has been an insight into what to look for and how it can be looked for. Your own methodology is up to you, it is your responsibility to test and then act/report on what you've found.

As always if you've got any questions or queries [tweet me](https://twitter.com/ZephrFish)

I will say one thing though, **if you're going to ask questions, at least read the other posts on this blog before asking as they will answer 99% of queries.**

As a final note, I am going to be starting an eBook of Learning the Ropes 101 so stay tuned for more info on this! 

