I get loads of messages on various mediums each week asking about how to get into information security & bug hunting. Queries range from how to do things through to how to get into the industry and where to start. 

I started this series for those people, learning and wanting to learn more about this industry, what is involved and along the way give some practical tips and tricks. This post serves as one of the more practical ones, giving a rough check-list on items to cover off in penetration testing, the list is geared more towards pentesters in mind however it can be applied to bug bounty hunting too.

 As I've learned over the past few years each person is different and will develop their own methodology of testing applications, however most will follow a rough structure on finding and enumerating apps. I stumbled across several lists and methodologies in my time testing however this is my take.

When testing applications it is important to have some form of methodology or check-list in place. It's not essential however it can be VERY useful, it allows the tester to go through all avenues looking for vulnerabilities and enumerate more and more.

The list below is by no means a complete methodology it has been adapted from the Web application hacker's handbook & other publications.  It  does however serve as a rough guideline on things to analyse and look for.

#Reconnaissance
 - Utilize port scanning
 - Map visible content 
 - Discover hidden & default content
    - Utilize shodan for finding similar apps and endpoints
    - Utilize the waybackmachine for finding forgotten endpoints
 - Test for debug parameters & Dev parameters
 - Identify data entry points
 - Identify the technologies used
 - Map the attack surface and application

# Access Control Testing
- Authentication
 - Test password quality rules
 - Test for username enumeration
 - Test resilience to password guessing
 - Test password creation strength
 - Test any account recovery function
 - Test any "remember me" function
 - Test any impersonation function
 - Test username uniqueness
 - Check for unsafe distribution of credentials
 - Test for fail-open conditions
 - Test any multi-stage mechanisms 
- Session Management
	- Test tokens for meaning
 - Test tokens for predictability
 - Check for insecure transmission of tokens
 - Check for disclosure of tokens in logs
 - Check mapping of tokens to sessions
 - Check session termination
 - Check for session fixation
 - Check for cross-site request forgery
 - Check cookie scope
- Access Controls
 - Understand the access control requirements
 - Test effectiveness of controls, using multiple accounts if possible
 - Test for insecure access control methods (request parameters, Referrer header, etc)

# Input Validation and Handling
 - Fuzz all request parameters
 - Test for SQL injection
 - Identify all reflected data
 - Test for reflected XSS
 - Test for HTTP header injection
 - Test for arbitrary redirection
 - Test for stored attacks
 - Test for OS command injection
 - Test for path traversal
 - Test for JavaScript/HTML injection
 - Test for file inclusion - both local and remote
 - Test for SMTP injection
 - Test for SOAP injection
 - Test for LDAP injection
 - Test for XPath injection
 - Test for template injection
 - Test for XXE  injection


# Application/Business Logic
 - Identify the logic attack surface
 - Test transmission of data via the client
 - Test for reliance on client-side input validation
 - Test any thick-client components (Java, ActiveX, Flash)
 - Test multi-stage processes for logic flaws
 - Test handling of incomplete input
 - Test trust boundaries
 - Test transaction logic
 - Test for Indirect object references(IDOR)

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
      

Now as stated in the intro this list is by no means a conclusive one, however hopefully it will allow you to build a better picture of what needs to be tested, what can be tested and when to include it. Your own methodology is up to you, it is your responsibility to test and then act/report on what you've found. If you've got any questions or queries [tweet me](https://twitter.com/ZephrFish).
