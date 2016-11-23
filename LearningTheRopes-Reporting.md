# Learning the Ropes 101: Stay Beautiful, Stay Verbose

Having recently just arrived back from Defcon 24 Las Vegas, I've not had a chance to write up much in regards to blogging and for that I am sorry! 

Anyway this post will discuss the importance of reporting and what it means to me to create a beautiful, reproducible and verbose report, this can be applied to both a pentest or a bounty report as they are the same scenario, just slightly different writing styles (bearing in mind this is my opinion on this topic!).

When writing any security report especially from a technical standpoint, whether this be bounties or pentesting it is super important to have reproducible steps(I learned this from studying forensics at Uni). 

As essentially, you are outlining your findings to a target be this a client or a bounty program. Most of us will find it really easy to post a report stating I found *x* wrong with *y* in this location. However what does this look like to the client? Sure you might have found a cool vulnerability or an interesting way of bypassing security control *z*, however the client might see this as *"Interesting, but how the hell did you get there? What do I need to do to fix and reproduce it?" HELP PLEASE!*. 

So an answer here to this is creation of a verbose report, one that has the necessary information contained within it to allow said client to not only reproduce what you found but also with detailed steps and references to correctly fix the issue. The next few sections will explain the structure I follow to create a decent report, note this might not be what you are used to however you will thank me for it later when programs are singing your praises due to the detail of your report.

#####Bug Bounty Reporting
Normally when I find a bug on a program I will take the extra time to craft a unique report for the affected client and the affected area of the application or site. However usually I will follow this vague structure:

 1. Issue Description 
 2. Issue Identified
 3. Affected URL/Area
 4. Risk Breakdown
 5. Steps to Reproduce
 6. Affected Demographic/User base
 7. Recommended Fix or Remediation Steps
 8. References
 9. Screenshots of the issue to reproduce

To quickly explain what each of these headers should contain here's an outline:

- **Issue Description** - A generic overview of the issue, I usually use the default text from OWASP as it explains the issue well.
- **Issue Identified** - A more specific description of the issue identified within the application.
- **Affected URL/Area** - The affected urls or area of the application where the issue exists.
- **Steps to reproduce** - A clear outline of the steps required to execute the payload as an attacker, this can include how to setup the payload and launch it.
- **Affected Demographic/User Base** - Explain who this issue affects? Is it everyone or just a select amount of users? How can this occur?
- **Recommended Fix** - How do you fix the issue? What is the recommended remediation actions required to successfully fix issue x?
- **References** - Include additional reading for the client to further backup the issues explained or elaborate more on other potential issues chained to the one identified.

Now, not every report I deliver to a program is going to have this applicable structure as it obviously depends on the issue identified. Creation of a properly verbose and informative report can be dialled down to a methodology taught to me by my boss when I first started out in testing: *Introduce, Show, Explain*. Essentially you are presenting your evidence, showing how it's possible to do *X* then explaining how it's an issue or what it demonstrates. 

To put this structure into context here is an example bug report that typically I might submit. This particular issue is an example of stored cross site scripting within a file upload feature. All of the information outlined in this example has been created for this blog post and isn't live data :-).

######Issue Description
Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted web sites. XSS attacks occur when an attacker uses a web application to send malicious code, generally in the form of a browser side script, to a different end user. 

######Issue Identified
The consultant identified that the update profile picture is vulnerable to cross site scripting, it is possible to upload an image with a MIME type of `text/html` this is then stored on the user's profile as an XSS payload, the outline below demonstrates the steps taken to exploit and reproduce.

######Risk Breakdown
Risk: **High**
Difficulty to Exploit: **Medium**
CVSS2 Score: **7.9** [(AV:N/AC:M/Au:S/C:C/I:C/A:N)](https://nvd.nist.gov/cvss.cfm?calculator&version=2&vector=(AV:N/AC:M/Au:S/C:C/I:C/A:N))

######Affected URLs
 - https://example.com/update-profile
 - https://example.com/file/upload

######Steps to Reproduce
The following steps indicate a proof of concept outlined in three(3) steps to reproduce and execute the issue.

**Step 1:**
Navigate to https://example.com/update-profile and select edit as shown in screenshot attached labelled step1.jpg.

**Step 2:**
Modify the profile image request with a local proxy, in this case the consultant is using Burp Suite. Change the Content type from image to text/html as shown in the post request:

    POST /file/upload/ HTTP/1.1
    Host: example.com
    ---snip----
    
    -----------------------------900627130554
    Content-Disposition: form-data; name="stored_XSS.jpg"; filename="stored_XSS.jpg"
    Content-Type: text/html
    
    <script>alert('ZephrFish')</script>
    -----------------------------900627130554

When this is sent, the following response is shown:

    HTTP/1.1 200 OK
    Date: Sat, 13 Aug 2016 14:31:44 GMT
    ---snip---
    
    {"url": "https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg?Expires=1465669904&Signature=dNtl1PzWV&Key-Pair-Id=APKAJQWLJPIV25LBZGAQ", "pk": "56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg", "success": true}

**Step 3:**
The file has been uploaded to Application X and is hyperlinked to from the profile page as shown in step 3.jpg. By simply following the link to the image which in this case is:

    https://example.com/56fc3b92159006271305543ef45a04452e8e45ce4/stored_XSS.jpg

The payload is executed as shown in attached screenshot labelled step3.jpg, thus this demonstrates the issue is stored cross site scripting.

######Affected Demographic
This issue will affect all users on the site who view the profile of the attacker, when the image is rendered the payload is executed instead of a profile image. Additionally when the malicious user posts anything on the forums the payload will execute.

######Remediation Instructions
Insure that file upload checks the MIME type of content being uploaded, for additional security implement server side content checking to ensure file headers match that of the file extension. Additionally make sure that all user input is treated as dangerous do not render any HTML tags.

######References
For more information on remediation steps check out reference [[2]](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)).

 - [OWASP XSS Explained](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))
 - [OWASP XSS Prevention Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet)

#####Reporting in Pentesting
The above report is an example of what can be submitted to a bug bounty program, however this type of reporting isn't always applicable to pentesting. This is mainly due to the target audience for pentest reports tends to be several types of people, in it's core a pentest report will usually be read by management and the technical teams. What this means for the tester/reporter is that there will be sections within the report that will need to be adapted and tailored for each audience. In a nutshell, usually you don't want to be talking about XSS, CSRF, and other buzzwords to C-level management as it will go over their head and disadvantage you in putting your point across. Instead consider writing like you are explaining the issue to your granny "*#teachgranny*" - [cornerpirate](https://twitter.com/cornerpirate). Consider phrases such as "This would enable a malicious attacker to gain access to user information which could result in loss of sensitive data" as an example. However when pitching your point to the technical teams you can outline in more detail more like the bug report above. 

The sections in a pentest report will usually contain(but are not limited to) headings such as:

 1. Management/Executive Summary
 2. Scope/Targets
 3. Table of issues/recommendations
 4. Vulnerabilities Discovered
 5. Additional Information

Usually a pentest report will span over 50-60 pages at a minimum(for this reason and several others I will not be providing an example report), however this can be any number realistically depending on an unlimited amount of variables.
 

#####Making Things Beautiful
Making things beautiful in reporting is really easy, it depends on how you're representing your information though, whether this be within Microsoft Word or Markdown or another format of your choice. The key point to understand is you want to portray your information in the clearest way possible, whether this be with report details in markdown using formatting, tables and other essentials or within [Word](https://cornerpirate.com/word-tips/)? 

Essentially when creating reports, the main thing is understanding your format, so if it's bug bounty reports you're after then you'll want to get a greater understanding of the formatting within markdown, how to outline your data in a pretty and readable form.


 **Stay Beautiful, Stay Verbose. Enjoy your work.**

Enjoy reporting your issues and putting your point across in a clear manner.

To close, this approach may not be for everyone however it is my opinion that if you find a bug worth reporting why not make it clear and understandable.
